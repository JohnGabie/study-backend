# Biblioteca de Referência — Mapa Geral e Conexões

## Status dos Arquivos

| # | Livro | Arquivo | Status |
|---|-------|---------|--------|
| 01 | Python Crash Course — Matthes | `python_crash_course.md` | ✅ |
| 02 | Effective Python — Slatkin | `effective_python.md` | ✅ |
| 03 | Fluent Python — Ramalho | `fluent_python.md` | ✅ |
| 04 | Python Concurrency with asyncio — Fowler | `python_concurrency_asyncio.md` | ✅ |
| 05 | HTTP: The Definitive Guide — Gourley & Totty | `http_definitive_guide.md` | ✅ |
| 06 | Building Data Science Apps with FastAPI — Voron | `building_fastapi.md` | ⚠️ PDF ausente |
| 07 | REST API Design Rulebook — Massé | `rest_api_design_rulebook.md` | ✅ |
| 08 | Learning SQL — Beaulieu | `learning_sql.md` | ✅ |
| 09 | PostgreSQL: Up and Running — Obe & Hsu | `postgresql_up_running.md` | ✅ |
| 10 | Designing Data-Intensive Applications — Kleppmann | `designing_data_intensive_apps.md` | ✅ |
| 11 | Clean Code Fundamentals — Hock* | `clean_code.md` | ✅ |
| 12 | Refactoring — Fowler | `refactoring.md` | ✅ |
| 13 | A Philosophy of Software Design — Ousterhout | `philosophy_software_design.md` | ⚠️ PDF ausente |
| 14 | Architecture Patterns with Python — Percival & Gregory | `architecture_patterns_python.md` | ⚠️ PDF ausente |
| 15 | Domain-Driven Design Distilled — Vernon | `domain_driven_design_distilled.md` | ⚠️ PDF ausente |
| 16 | The Pragmatic Programmer — Hunt & Thomas | `pragmatic_programmer.md` | ✅ |
| 17 | TDD with Python — Percival | `tdd_with_python.md` | ✅ |
| 18 | Python Testing with pytest — Okken | `python_testing_pytest.md` | ✅ |
| 19 | Web Application Security — Hoffman | `web_application_security.md` | ✅ |
| 20 | Site Reliability Engineering — Google | `site_reliability_engineering.md` | ✅ |
| 21 | The Phoenix Project — Kim | `phoenix_project.md` | ⚠️ PDF ausente |

*⚠️ O livro disponível é "Clean Code Fundamentals" de Martin Hock — não o "Clean Code" original de Robert C. Martin.

---

## Mapa de Conexões Cruzadas

### Eixo 1: Qualidade de Código
**Livros:** Clean Code Fundamentals → Refactoring → Pragmatic Programmer → Philosophy of Software Design

```
Clean Code Fundamentals (Hock)
├── Define os princípios: naming, SRP, funções pequenas
├── Tensão com Philosophy of Software Design (Ousterhout) sobre comentários
│   └── Hock: comentários são falha | Ousterhout: comentários que explicam o *porquê* são obrigatórios
└── Precede Refactoring (Fowler)
    └── Fowler: como chegar ao código limpo a partir do código sujo
        └── ambos convergem no Pragmatic Programmer
            └── PP: postura e mentalidade que sustenta tudo acima
```

**Ponto de tensão mais produtivo:** Comentários
- Hock/Martin: "Prefer self-explanatory code instead of comments"
- Ousterhout: "Comments should describe things that aren't obvious from the code"
- **Na prática com FastAPI:** comente o *porquê* de uma decisão não óbvia (ex: um status code incomum, um workaround de ORM), não o *o quê* (o código já diz isso)

---

### Eixo 2: Python da Superfície à Profundidade
**Livros:** Python Crash Course → Effective Python → Fluent Python → Python Concurrency with asyncio

```
Python Crash Course (Matthes) — o *o quê*: sintaxe, estruturas básicas
    ↓
Effective Python (Slatkin) — o *como*: 125 formas de fazer melhor
    ↓
Fluent Python (Ramalho) — o *por quê*: o Python Data Model subjacente
    ↓
Python Concurrency with asyncio (Fowler) — o *quando e como* do asyncio
```

**Ponto de tensão:** Type hints vs. Duck typing
- Ramalho (Fluent Python): Python é duck typing, Protocols são suficientes
- Slatkin (Effective Python): type hints + mypy são boas práticas modernas
- **Na prática:** Pydantic + FastAPI resolvem o conflito — duck typing interno, contratos explícitos na fronteira da API

---

### Eixo 3: APIs e Web
**Livros:** HTTP: The Definitive Guide → REST API Design Rulebook → Building FastAPI (ausente)

```
HTTP (Gourley & Totty) — o protocolo
    ↓
REST API Design Rulebook (Massé) — estilo arquitetural sobre HTTP
    ↓
Building FastAPI (Voron) — implementação em Python [PDF ausente]
```

**Ponto de tensão:** Versionamento de API
- Massé: via header `Accept: application/vnd.api.v1+json`
- Prática comum: via URL path `/v1/users`
- **Na prática com FastAPI:** URL path é mais simples de implementar e de usar; use se não tiver razão forte para headers

---

### Eixo 4: Dados
**Livros:** Learning SQL → PostgreSQL Up and Running → Designing Data-Intensive Applications

```
Learning SQL (Beaulieu) — SQL declarativo, fundamentos
    ↓
PostgreSQL Up and Running (Obe & Hsu) — PostgreSQL específico: tipos, índices, extensões
    ↓
Designing Data-Intensive Applications (Kleppmann) — os trade-offs por trás de tudo
```

**Ponto de tensão:** SQL vs. NoSQL
- Beaulieu: ensina o modelo relacional como verdade
- Kleppmann: apresenta ambos como trade-offs conscientes
- **Na prática com ZionHub:** PostgreSQL + JSONB dá o melhor dos dois mundos para dados semi-estruturados

---

### Eixo 5: Testes
**Livros:** TDD with Python → Python Testing with pytest

```
TDD with Python (Percival) — filosofia: por que e quando testar
    ↓
Python Testing with pytest (Okken) — técnica: como testar com pytest
```

**Ponto de tensão:** Unit tests vs. Integration tests
- Percival: usa ambos estrategicamente (functional tests para guiar, unit tests para design)
- Slatkin (Item 109): prefere integration tests sobre unit tests com mocks
- **Na prática com FastAPI:** integration tests que testam endpoint → serviço → banco real > unit tests com mocks de banco. Mocks para APIs externas (email, pagamento, notificação).

---

### Eixo 6: Qualidade e Arquitetura (livros ausentes)
**Livros:** Architecture Patterns with Python → Domain-Driven Design Distilled → Philosophy of Software Design

Estes livros estão sem PDF. Quando disponíveis, o mapa de conexões:
```
Philosophy of Software Design (Ousterhout) — simplicidade como estratégia
    ↓ tensão com Clean Code sobre comentários
DDD Distilled (Vernon) — modelagem de domínio
    ↓ implementação em Python
Architecture Patterns with Python (Percival & Gregory) — padrões: Repository, Unit of Work, Event-driven
    ↓ conecta com
Designing Data-Intensive Applications (Kleppmann) — event sourcing, CQRS
```

---

### Eixo 7: Produção
**Livros:** Web Application Security → Site Reliability Engineering → The Phoenix Project (ausente)

```
Web Application Security (Hoffman) — segurança: o que pode dar errado
    +
Site Reliability Engineering (Google) — reliability: como manter funcionando
    +
The Phoenix Project (Kim) — cultura: como uma organização adota DevOps [ausente]
```

**Conexão crítica:**
- SRE + Security compartilham o princípio de "fail fast and loud" — sistemas que falham silenciosamente são os mais perigosos (DDIA Cap. 1 também concorda)
- A tríade CIA (Confidentiality, Integrity, Availability) distribui: Security cobre C+I, SRE cobre A

---

## Tensões Mais Produtivas da Biblioteca

| Tensão | Livro A | Livro B | Resolução prática |
|--------|---------|---------|-------------------|
| Comentários | Clean Code (evite) | Philosophy of SW Design (escreva o porquê) | Comente decisões de design não-óbvias; não comente o óbvio |
| Unit vs. Integration tests | TDD with Python (ambos) | Effective Python Item 109 (prefira integration) | Integration tests para lógica crítica; unit tests para funções puras |
| DRY vs. premature abstraction | Pragmatic Programmer (DRY é sobre conhecimento) | Pragmatic Programmer Topic 27 (não outrun headlights) | 3 ocorrências similares → considere abstração; 2 → aguarde |
| REST vs. GraphQL/gRPC | REST API Design Rulebook | — | REST para ZionHub hoje; conhecer alternativas para decisões futuras |
| SQL vs. ORM | Learning SQL (escreva SQL bem) | DDIA (ORMs escondem trade-offs) | Use ORM mas entenda o SQL gerado; raw SQL para queries críticas |
| Async vs. Threads | Python Concurrency (async para I/O) | Effective Python (threads para I/O bloqueante) | Async por padrão no FastAPI; `run_in_executor` para libs síncronas |

---

## Mapa de Aplicação ao ZionHub

### Quando receber código para review, consulte:
1. **Naming e funções:** Clean Code Fundamentals (Hock), Cap. 3
2. **Estrutura de endpoint:** REST API Design Rulebook (Massé), Cap. 2–3
3. **Lógica de negócio no endpoint:** Architecture Patterns with Python (ausente), Cap. 1–2
4. **Queries N+1:** Learning SQL (Beaulieu), Cap. 5 + DDIA, Cap. 2
5. **Falta de testes:** TDD with Python (Percival), Cap. 1
6. **Funções que fazem 3 coisas:** Refactoring (Fowler), Cap. 3 — Divergent Change
7. **Async incorreto:** Python Concurrency with asyncio (Fowler), Cap. 1 + 9
8. **Exposição de dados sensíveis:** Web Application Security (Hoffman), Cap. 20
9. **Sem health check / observabilidade:** SRE (Google), Cap. 6 — Four Golden Signals

---

## Ordem de Leitura Recomendada para o Aluno

### Se ainda não leu nada com profundidade:
1. Python Crash Course (fundamento)
2. Effective Python (idiomas Python)
3. Clean Code Fundamentals (postura de qualidade)
4. Refactoring (como melhorar o que existe)
5. Learning SQL (dados fundamentais)

### Para aprofundar o ZionHub imediatamente:
1. Python Concurrency with asyncio (FastAPI usa asyncio)
2. REST API Design Rulebook (design de endpoints)
3. PostgreSQL Up and Running (o banco do ZionHub)
4. TDD with Python → Python Testing with pytest (testes para código sem testes)
5. Web Application Security (produção com usuários reais)

### Para pensar em arquitetura futura:
1. Designing Data-Intensive Applications
2. Architecture Patterns with Python (quando PDF disponível)
3. Domain-Driven Design Distilled (quando PDF disponível)
4. Site Reliability Engineering
