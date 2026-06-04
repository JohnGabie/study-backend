# Perfil do Aluno

## Diagnóstico Inicial (2026-04-01)
- Prova avançada: 4.7/25 (19%) — estado zero, antes de qualquer estudo
- Prova média: 8.5/25 (34%)
- Prova fácil: 15.4/25 (62%)
- Relatório completo: `quizzes/prova_geral_2026-03-31_relatorio.md`

## Contexto de Experiência
- **FastAPI:** ~2-3 meses, curso básico + projeto vibecoded com IA
- **Python:** uso prático sem estudo formal. Modelo mental presente, vocabulário lacunar
- **SQL:** uso superficial. SELECT/WHERE funciona, agregações e JOINs complexos não

## Pontos Fortes
- Intuição de acoplamento: sente que código está errado antes de saber nomear
- Honestidade sobre lacunas: não inventa respostas
- Raciocínio pragmático e por dedução — chega em conceitos corretos sem ter lido
- HTTP semântico: URI nomeia recurso, método nomeia ação — chegou por raciocínio próprio
- Separação de lógica: "API é apenas access point" — sem ter lido o livro
- Raciocínio de segurança: propôs token binding sem saber o nome (2026-04-10)

## Lacunas Ativas

### 🔴 Conceito errado — corrigir ativamente
- **async/await:** acredita que `time.sleep` em `async def` pausa só aquela função. Errado — trava o event loop inteiro.
- **FLOAT para dinheiro:** sugere DOUBLE como fix. Errado — problema é imprecisão de ponto flutuante. Correto: NUMERIC/DECIMAL.
- **User enumeration:** diagnosticou como SQL injection. O problema real é user enumeration.
- **REST URIs:** ainda usa verbos CRUD na URI. Fixar: URI nomeia recurso, método nomeia ação.
- **Tipos Python:** disse `double` e `char` — são tipos de Java/C. Python tem `float` e `str`.

### 🟠 Vocabulário ausente, intuição presente
- N+1 problem, IDOR, Default mutável

### 🟡 Gaps de fundamento (não estudados ainda)
- HTTP: estrutura de mensagem, status codes, evolução HTTP/1→3
- DRY, TDD, 4 Golden Signals, Design by Contract
- SQL: índices, timezone
- Estrutura de testes com pytest

## Padrões de Raciocínio
- Intuitivo: sente problemas antes de nomear
- Pragmático: resolve problemas, não decora teoria
- Honesto: não blefa quando não sabe
- Curiosidade lateral: vai fundo em tangentes — redirecionar com propósito, não cortar

## Discordâncias e Posições Próprias
- (nenhuma registrada ainda)

## Sessões
- [2026-04-01](sessions/2026-04-01.md) — Setup + Provas diagnósticas
- [2026-04-10](sessions/2026-04-10.md) — HTTP: fundamentos e segurança de cookies
- [2026-04-13](sessions/2026-04-13.md) — HTTP continuação + ZionHub v2 + SuperTokens

## Trilha Alura — Desenvolvimento Back-end Python

Iniciada em 2026-04-12. Curadoria feita em 2026-04-19: 15 cursos selecionados de 44.

### Legenda
- ✅ Concluído
- 🔄 Em andamento
- ⭐ Fazer — relevante pro ZionHub e gaps atuais
- ⏭️ Pulado (já sabe, Django-específico, ou prematuro)

---

### BASE — Fundamentos de Programação com Python
| # | Curso | Status |
|---|-------|--------|
| 01 | Pensamento computacional | ✅ |
| 02 | Python: primeira aplicação | ⏭️ já sabe |
| 03 | Condicionais if/elif/else | ⏭️ já sabe |
| 04 | Laços for e while | ✅ |
| 05 | Funções | ✅ |

### NÍVEL 1 — Estruturando Aplicações Web
| # | Curso | Status |
|---|-------|--------|
| 01 | Git e GitHub | ⏭️ já sabe |
| 02 | Praticando Python: projetos | ⏭️ genérico |
| 03 | Python: OOP | ✅ |
| 04 | Listas e tuplas | ⏭️ já sabe |
| 05 | OOP avançado + consumir API | ⭐ herança, polimorfismo, abstração — gaps reais |
| 06 | Conjuntos e dicionários | ⏭️ já sabe |
| 07 | Redes e Protocolos | ⭐ complementa estudo de HTTP |
| 08 | Persistência de dados | ⭐ SQL é gap — fundamento |
| 09 | Strings e Regex | ⏭️ útil mas não urgente |
| 10 | Flask + MongoDB | ⏭️ usa FastAPI |
| 11 | Django: templates | ⏭️ Django-específico |
| 12 | Django: persistência e Admin | ⏭️ Django-específico |
| 13 | Django: autenticação e formulários | ⏭️ Django-específico |
| 14 | Django: OAuth2 | ⏭️ Django-específico |
| 15 | FastAPI: auth + DB + deploy | ⭐ prioridade máxima — é o ZionHub |
| 16 | Metodologias Ágeis | ⏭️ |
| 17 | Arquitetura de Software | ⭐ MVC, camadas, separação |
| 18 | SOLID com Python | ⭐ gap real |

### NÍVEL 2 — Qualidade, Segurança e Escalabilidade
| # | Curso | Status |
|---|-------|--------|
| 01 | DRF: APIs RESTful do zero | ✅ |
| 02 | DRF: permissões, CORS, deploy AWS | ⏭️ Django-específico |
| 03 | DRF: validações, paginação, filtros | ⏭️ Django-específico |
| 04 | DRF: testes unitários e de integração | ⏭️ Django-específico |
| 05 | Swagger | ⏭️ FastAPI entrega nativamente |
| 06 | Testes automatizados | ⭐ prioridade máxima — ZionHub tem zero testes |
| 07 | Design Patterns | ⭐ complementa Arquitetura e SOLID |
| 08 | Microsserviços | ⭐ conceito antes de precisar |
| 09 | CI/CD com Docker + GitHub Actions | ⭐ ZionHub precisa |
| 10 | CI/CD na EC2 | ⏭️ depois do #09 se precisar |
| 11 | CI/CD no ECS | ⏭️ específico demais agora |
| 12 | CI/CD: rollback e teste de carga | ⏭️ específico demais agora |
| 13 | Python e OWASP | ⭐ prioridade máxima — 15 vulnerabilidades no ZionHub |

### NÍVEL 3 — Arquitetura, Desempenho e Deploy
| # | Curso | Status |
|---|-------|--------|
| 01 | DDD com Python | ⭐ após Nível 2 |
| 02 | Padrões de Integração Distribuídos | ⭐ após Nível 2 |
| 03 | Mensageria com RabbitMQ | ⭐ após Nível 2 |
| 04 | Mensageria com Kafka | ⭐ após Nível 2 |
| 05 | Performance e Profiling com C | ⏭️ nicho |
| 06–12 | Kubernetes (7 cursos) | ⏭️ excessivo agora |

### Ordem sugerida (blocos)
1. **Agora:** FastAPI auth+DB+deploy → Testes automatizados → OWASP
2. **Fundamentos:** OOP avançado → Redes e Protocolos → Persistência de dados
3. **Arquitetura:** Arquitetura de Software → SOLID → Design Patterns
4. **Produção:** CI/CD Docker+Actions → Microsserviços
5. **Avançado:** DDD → Integração Distribuída → RabbitMQ → Kafka

## Exemplos Salvos
- (nenhum ainda)
