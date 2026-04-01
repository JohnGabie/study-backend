# ZionHub Backend — Visão Geral
**Explorado em:** 2026-04-01
**Localização:** `projetos/zionhub-backend/`

---

## Stack

| Camada | Tecnologia |
|--------|-----------|
| Framework | FastAPI 0.128.0 |
| ORM | SQLAlchemy 2.0 (async) |
| Banco | PostgreSQL 15 |
| Auth | PyJWT + bcrypt |
| Rate Limiting | slowapi |
| Scheduler | APScheduler |
| WebSocket | websockets |
| Migrations | Alembic (20+ migrations) |
| Testes | pytest |
| IoT | Tuya API + Bridge genérico |
| Email | aiosmtplib |
| Infra | Docker + nginx |

---

## Estrutura de Diretórios

```
app/
├── api/v1/          # 10 routers: auth, users, devices, routines, members, analytics...
├── core/            # config, database, security, middleware, rate_limit
├── crud/            # 13 classes CRUD async (base genérico + especializados)
├── models/          # 14 modelos SQLAlchemy
├── schemas/         # Pydantic schemas (request/response)
├── services/        # device, tuya, bridge, routine, scheduler, monitoring, email, youtube
├── websocket/       # ConnectionManager + routes
├── utils/           # logger
└── main.py          # entry point (lifespan, middlewares, routers)
```

---

## Arquitetura

**Padrão de camadas:**
```
endpoint → service → crud → model
```

**Multi-tenant:** todas as entidades têm `organization_id`. TenantMiddleware extrai org do JWT e injeta em `request.state.org_id`.

**Auth flow:**
1. Register → cria org + user + envia email de verificação → retorna JWT + refresh token
2. Login → verifica bcrypt → JWT (30min) + refresh token
3. Logout → salva `last_logout_at` → tokens anteriores ficam inválidos
4. Refresh → novo access token

**JWT payload:** `{ sub: user_id, org_id, org_slug, exp, iat }`

**Dependency chain:**
```python
Depends(get_db) → AsyncSession
Depends(get_current_user) → User
Depends(get_current_org) → Organization
Depends(require_module("rotinas")) → verifica plano da org
```

---

## Módulos do Produto

- **rotinas** — automação de dispositivos IoT (Tuya + Bridge genérico)
- **mix_page** — controle de áudio / song requests via YouTube
- **secretaria** — módulo customizado

Acesso por módulo é controlado pelo plano da organização (free/starter/pro/custom).

---

## Modelos principais

- `User` — roles: SUPER_ADMIN, ORG_ADMIN, MEMBER, VISITOR
- `Organization` — multi-tenant SaaS
- `Device` — Tuya ou Bridge (IoT)
- `Routine` — automação com triggers TIME/MANUAL/ROUTINE_COMPLETE/DEVICE_STATE
- `RoutineAction` — ação dentro de uma rotina
- `ActivityLog` — trilha de auditoria
- `DeviceSession` — histórico de toggles
- Tokens efêmeros: RefreshToken, PasswordResetToken, EmailVerificationToken, InviteToken
- `BackgroundMusic`, `SongRequest` — módulo de áudio

---

## Segurança — Estado Atual

Existe um `SECURITY_AUDIT.md` no repositório com **15 vulnerabilidades documentadas**.

### Críticas (3)
| ID | Problema |
|----|----------|
| ZIO-42 | `toggle_all_devices` sem filtro de `organization_id` — qualquer usuário autenticado pode controlar dispositivos de outras organizações |
| — | `SECRET_KEY` tem valor padrão fraco no código — se env var não for setada, JWT pode ser forjado |
| — | `DATABASE_URL` tem credenciais hardcoded como fallback |

### Altas (4)
| ID | Problema |
|----|----------|
| ZIO-43 | Endpoints de monitoring (`/start`, `/stop`) sem verificação de role admin |
| ZIO-44 | Políticas RLS criadas no banco mas **não ativadas** |
| — | WebSocket não autentica conexões completamente |
| — | Sem Content-Security-Policy header |

### Médias/Baixas (8)
- Sem paginação validada em alguns endpoints
- Monitoring service em thread separada (possíveis race conditions)
- Logs não estruturados (sem JSON format)
- Type hints incompletos nos services
- Sem CI/CD pipeline

---

## O que está bom

- Arquitetura em camadas correta (endpoint → service → crud → model)
- Async-first com SQLAlchemy 2.0
- CRUD base genérico reduz boilerplate
- Rate limiting com suporte a proxy (X-Forwarded-For)
- Security headers middleware (HSTS, X-Content-Type-Options, X-Frame-Options)
- Body size limit (1MB)
- Token invalidation no logout
- Activity logging para auditoria
- Testes existem (~50-60% cobertura estimada)
- Docker + nginx configurados
- Documentação: README, ARCHITECTURE.md, CONTEXT.md, SECURITY_AUDIT.md

---

## Números

| Métrica | Valor |
|---------|-------|
| Endpoints | 60+ |
| Modelos SQLAlchemy | 14 |
| CRUD repositories | 13 |
| Migrations Alembic | 20+ |
| Arquivos de teste | 4 (~40k LOC) |
| Pacotes Python | 74 |
| Vulnerabilidades documentadas | 15 (3 críticas) |

---

## Para a Fase 4 (Code Review Real)

Quando o aluno tiver o vocabulário das Fases 1-3, o code review vai seguir:

1. `SECURITY_AUDIT.md` — ler, entender cada item, saber corrigir
2. `toggle_all_devices` — primeira correção: adicionar filtro de org_id
3. Endpoints de monitoring — adicionar `require_admin` dependency
4. SECRET_KEY / DATABASE_URL — remover defaults hardcoded
5. RLS policies — entender e ativar
6. Revisar separação de camadas endpoint por endpoint
7. Criar issues no Linear para cada refatoração

**O projeto tem boas bases. Os problemas são corrigíveis e didáticos.**
