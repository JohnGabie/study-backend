# Gabarito — Prova Geral 2026-03-31
> ⚠️ **Abra só depois de terminar a prova.**

---

## Nível 1 — Conceito

**Q1 — SRP**
Uma função/classe tem uma única razão para mudar. Um endpoint que valida + persiste + notifica tem 3 razões para mudar (regra de negócio, schema do banco, template de email) — viola SRP.
📖 Clean Code Fundamentals — Hock, Cap. 2 (Software Design) + Cap. 3 (funções com `and`/`or` no nome)
📖 Refactoring — Fowler, Cap. 3 — smell "Divergent Change"

---

**Q2 — async def vs def**
`async def` define uma coroutine que roda no event loop. `def` comum roda em um threadpool. `time.sleep(3)` dentro de `async def` é bloqueante — trava o event loop inteiro, impedindo que todos os outros requests sejam atendidos enquanto dorme. O correto é `await asyncio.sleep(3)`.
📖 Python Concurrency with asyncio — Fowler, Cap. 1 e 9

---

**Q3 — DRY**
DRY é sobre não duplicar *conhecimento* (lógica, intenção, regra de negócio) — não sobre texto. Dois trechos idênticos que representam coisas independentes (ex: validação de CEP em dois domínios diferentes do negócio) não são DRY violation — se um mudar, o outro não deve mudar junto.
📖 Pragmatic Programmer — Hunt & Thomas, Cap. 2, Topic 9

---

**Q4 — 4 Golden Signals**
1. **Latência:** tempo de resposta por request — `p50`, `p95`, `p99` via prometheus
2. **Traffic:** requests/segundo — prometheus_fastapi_instrumentator
3. **Errors:** taxa de 5xx — prometheus ou logs estruturados
4. **Saturation:** uso de CPU, memória, conexões de banco, thread pool
📖 Site Reliability Engineering — Google, Cap. 6

---

**Q5 — TDD Red → Green → Refactor**
Red: escreva um teste que falha (o comportamento não existe ainda). Green: escreva o mínimo de código para passar. Refactor: melhore sem quebrar. Escrever o teste antes força você a pensar na interface antes da implementação — o código fica testável por design. Se o teste é difícil de escrever, o design está errado.
📖 TDD with Python — Percival, Cap. 1

---

**Q6 — Python Data Model**
O Python Data Model é o conjunto de protocolos (dunder methods) que permite que objetos customizados se comportem como objetos nativos. Python é um framework: ele chama `__len__` quando você usa `len()`, `__getitem__` quando você usa `[]` ou `for`. Implementar esses métodos dá à sua classe compatibilidade com todo o ecossistema Python gratuitamente.
📖 Fluent Python — Ramalho, Cap. 1

---

**Q7 — SQL Injection vs IDOR**
SQL Injection: payload malicioso que modifica a query SQL. O SQLAlchemy protege usando queries parametrizadas — o ORM nunca interpola strings diretamente no SQL.
IDOR (Insecure Direct Object Reference): acesso a recursos de outros usuários via manipulação de ID. O SQLAlchemy **não protege** — é responsabilidade do desenvolvedor verificar `order.user_id == current_user.id` antes de retornar.
📖 Web Application Security — Hoffman, Cap. 9 (SQLi) e Cap. 10 (IDOR)

---

**Q8 — HTTP Stateless**
Cada request é independente — o servidor não mantém memória de requests anteriores. Consequências: sessões precisam ser gerenciadas pelo cliente (cookies, JWT), autenticação deve ser enviada em todo request, escalabilidade horizontal é mais simples (qualquer servidor pode atender qualquer request).
📖 HTTP: The Definitive Guide — Gourley & Totty, Cap. 1

---

## Nível 2 — Aplicação

**Q9 — Endpoint com múltiplos problemas**
1. **SRP:** valida + persiste + notifica — 3 responsabilidades (Clean Code, Cap. 2; Refactoring — Divergent Change)
2. **`data: dict` em vez de Pydantic schema:** sem validação automática, sem type safety (Effective Python — Item 51)
3. **`return {"error": ...}` em vez de `raise HTTPException`:** retorna 200 com corpo de erro (REST API Rulebook, Cap. 3)
4. **`requests.post(...)` bloqueante dentro de `async def`:** trava o event loop (Python Concurrency, Cap. 1)
5. **`db.commit()` sem transaction handling:** sem rollback em caso de erro na notificação
📖 Múltiplos

---

**Q10 — Default mutável**
Bug: `tags=[]` cria uma única lista compartilhada entre todas as chamadas da função. Toda chamada que não passa `tags` usa e modifica a mesma lista.
Correção:
```python
def create_order(user_id: int, tags: list | None = None):
    if tags is None:
        tags = []
    tags = tags + ["new"]  # não muta a lista original
    return {"user_id": user_id, "tags": tags}
```
📖 Effective Python — Slatkin, Item 36

---

**Q11 — IDOR**
O endpoint retorna o pedido para qualquer usuário autenticado que souber o `order_id`. Falta verificação de autorização: o pedido pertence ao usuário que está requisitando?
Correção: `if order.user_id != current_user.id: raise HTTPException(403)`
📖 Web Application Security — Hoffman, Cap. 10

---

**Q12 — Teste mal escrito**
Problemas:
1. Testa múltiplas operações em um teste — se `delete_user` falhar, você não sabe se `create_user` funciona
2. Um teste deve verificar uma coisa
3. O nome `test_user` não descreve o comportamento esperado
Reescrever: `test_create_user_returns_user_with_id`, `test_update_user_name`, `test_delete_user_removes_from_db` — cada um separado.
📖 Python Testing with pytest — Okken, Cap. 2 (estrutura AAA, um assert por comportamento)
📖 TDD with Python — Percival, Cap. 2

---

**Q13 — N+1 Problem**
Para N usuários, executa 1 query para listar usuários + N queries para buscar os pedidos de cada um = N+1 queries total. Cresce linearmente com o número de usuários.
Correção:
```python
stmt = select(User).options(selectinload(User.orders))
result = await db.execute(stmt)
users = result.scalars().unique().all()
```
📖 Designing Data-Intensive Applications — Kleppmann, Cap. 2 (object-relational mismatch)
📖 Learning SQL — Beaulieu, Cap. 5 (JOIN como solução)

---

**Q14 — Return None**
A função retorna `None` em dois casos de erro diferentes — o chamador precisa checar `if result is None` sem saber *qual* foi o problema. Preferir exceptions específicas.
Também: a função mistura "buscar usuário" com "buscar pedidos" — possível violação de SRP.
📖 Effective Python — Slatkin, Item 32 ("Prefer Raising Exceptions to Returning None")
📖 Pragmatic Programmer — Hunt & Thomas, Cap. 4, Topic 24 ("Dead Programs Tell No Lies")

---

**Q15 — Schema com problemas**
1. **`FLOAT` para valores monetários:** ponto flutuante tem erro de representação — `0.1 + 0.2 ≠ 0.3`. Use `NUMERIC(precision, scale)` para dinheiro.
2. **`TIMESTAMP` sem timezone:** ambíguo em sistemas com usuários em múltiplos fusos. Use `TIMESTAMP WITH TIME ZONE`.
3. **`user_id INTEGER` sem FOREIGN KEY constraint:** sem integridade referencial garantida pelo banco.
4. **Sem índice em `user_id`:** queries por usuário farão Seq Scan.
📖 PostgreSQL Up and Running — Obe & Hsu, Cap. 5 (tipos de dados)
📖 Learning SQL — Beaulieu, Cap. 2 (constraints)

---

**Q16 — Fixture com scope="session" problemática**
Scope `session` significa que a mesma sessão de banco é compartilhada por todos os testes. Dados criados em um teste persistem para o próximo — os testes se tornam dependentes da ordem de execução. Testes devem ser isolados.
Correção: `scope="function"` + `session.rollback()` no teardown para desfazer cada teste.
📖 Python Testing with pytest — Okken, Cap. 3 (fixture scopes)

---

**Q17 — URIs com CRUD no nome**
`getUserById`, `createOrder`, `deleteUser` — nomes de função CRUD nos URIs violam as regras do REST API Design Rulebook.
Correto:
```
GET    /users/{id}
POST   /orders
DELETE /users/{id}
```
O método HTTP já comunica a operação — o URI identifica o recurso.
📖 REST API Design Rulebook — Massé, Cap. 2, regra "CRUD function names should not be used in URIs"

---

**Q18 — Stack trace exposto em produção**
`str(exc)` e `traceback.format_exc()` expõem detalhes internos do sistema: stack frames, nomes de arquivos, linha de código, possível estrutura do banco. São dados de recon para um atacante (Web App Security, Cap. 2). Em produção, log internamente e retorne apenas uma mensagem genérica.
```python
logger.exception("Unhandled error", exc_info=exc)
return JSONResponse(500, {"error": "Internal server error"})
```
📖 Web Application Security — Hoffman, Cap. 2 (recon — não vaze informação)
📖 Site Reliability Engineering — Google, Cap. 6 (logs para observabilidade interna)

---

## Nível 3 — Síntese

**Q19 — Refatorar código sem testes**
1. Escreva testes para o comportamento *atual* (mesmo que ruim) — Fowler: "The first step is always the same: ensure you have a solid set of tests" (Refactoring, Cap. 1)
2. Verifique que os testes passam — agora você tem uma rede de segurança
3. Refatore em pequenos passos, rodando testes a cada mudança (Refactoring, Cap. 2)
4. Para o código vibecoded: comece pelos testes de contrato da API (functional tests — TDD with Python, Cap. 1) antes dos unit tests
📖 Refactoring — Fowler, Cap. 1 e 4
📖 TDD with Python — Percival, Cap. 22

---

**Q20 — Comentários: Hock vs. Ousterhout**
Hock (seguindo Martin): comentários são falha de expressão — o código deveria se explicar.
Ousterhout: comentários que explicam o *porquê* são obrigatórios; comentários que explicam o *o quê* são redundantes.
Exemplo:
```python
# ❌ Ruim (Hock tem razão): explica o óbvio
user.is_active = True  # sets is_active to True

# ✅ Bom (Ousterhout tem razão): explica decisão não óbvia
# Retornamos 409 em vez de 422 aqui porque o cliente já existe —
# 422 indicaria dados inválidos, mas os dados são válidos; o problema é conflito de estado
raise HTTPException(409, "Email already registered")
```
No ZionHub: comente decisões de design e workarounds; não comente o que o código já diz.

---

**Q21 — Endpoint completo: pedidos com produtos**
```sql
-- Schema
CREATE TABLE orders (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL REFERENCES users(id),
    created_at TIMESTAMPTZ DEFAULT NOW()
);
CREATE TABLE order_items (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_id UUID NOT NULL REFERENCES orders(id),
    product_id UUID NOT NULL REFERENCES products(id),
    quantity INTEGER NOT NULL CHECK (quantity > 0)
);
CREATE INDEX ON orders(user_id);  -- índice na FK!
```
Query eficiente: JOIN em vez de N+1. Endpoint com auth + authz. Teste que verifica isolamento de dados entre usuários.
📖 Learning SQL (Cap. 5 — JOINs), PostgreSQL Up and Running (Cap. 5–6 — tipos e índices), Web App Security (Cap. 10 — IDOR), TDD with Python (Cap. 1 — escreva o teste primeiro)

---

**Q22 — "Don't outrun headlights" vs. "Tracer bullets"**
Não são contraditórios — operam em escalas diferentes.
"Tracer bullets": construa o esqueleto completo de ponta a ponta com funcionalidade mínima *real* (não mocks) — valida que todas as camadas se conectam. É sobre velocidade de feedback, não sobre especulação.
"Don't outrun headlights": não adicione features para requisitos futuros imaginados. Construa para o presente.
No ZionHub: Tracer bullet = endpoint → serviço → banco funcionando com o caso mais simples. Não outrun = não adicione cache, event sourcing, ou filas antes de provar que são necessários.
📖 Pragmatic Programmer — Hunt & Thomas, Topic 12 (Tracer Bullets) e Topic 27 (Headlights)

---

**Q23 — gather + Semaphore + driver async**
Sem semáforo: `asyncio.gather()` com 100 tasks pode abrir 100 conexões simultâneas ao banco, esgotando o connection pool e causando timeouts ou erros. O semáforo limita o número de operações simultâneas ao banco (ou API externa):
```python
sem = asyncio.Semaphore(10)
async def query_with_limit():
    async with sem:
        return await db.execute(...)
results = await asyncio.gather(query_with_limit(), query_with_limit(), query_with_limit())
```
O driver assíncrono (asyncpg) permite que o event loop continue atendendo outros requests enquanto espera o banco — sem driver async, mesmo com `await`, o acesso ao banco seria bloqueante.
📖 Python Concurrency with asyncio — Fowler, Cap. 4 (gather) e Cap. 11 (Semaphore) e Cap. 5 (asyncpg)

---

**Q24 — API lenta: investigação**
(a) p99 > média: a média de 50ms pode esconder que 1% dos usuários espera 5 segundos — esses são os mais frustrados. p99 revela os piores casos reais. (DDIA Cap. 1 — "describing performance")
(b) Query lenta: `pg_stat_statements` para identificar queries por tempo total; `EXPLAIN ANALYZE` na query suspeita para ver se há Seq Scan que deveria ser Index Scan.
(c) Banco vs. aplicação: instrumentar o tempo *antes* e *depois* da chamada ao banco. Se o tempo total é 2s e o banco responde em 100ms, o problema é na aplicação (processamento, serialização, N+1 em memória). Se o banco demora 1.8s, o problema é de query/índice.
📖 DDIA — Kleppmann, Cap. 1 + Cap. 3 (storage e indexação)
📖 SRE — Google, Cap. 6 (4 Golden Signals)
📖 PostgreSQL Up and Running — Obe & Hsu, Cap. 9 (EXPLAIN ANALYZE)

---

**Q25 — Code review de autenticação: 5 checks**
1. **Secret key fraca:** `SECRET_KEY = "secret"` ou hardcoded — deve ser variável de ambiente com `openssl rand -hex 32`
2. **JWT sem expiração:** payload sem `exp` claim — token válido para sempre se vazar
3. **`alg: none` aceito:** verificar se o código valida o algoritmo do token — alguns parsers aceitam `none` como algoritmo válido sem verificar assinatura
4. **Payload JWT com dados sensíveis:** JWT é base64, não criptografado — password hash, PII não devem estar no payload
5. **Mensagens de erro informativas:** `"User not found"` vs. `"Invalid credentials"` — a primeira confirma que o email existe, dado útil para brute force
📖 Web Application Security — Hoffman, Cap. 13 (Authentication Bypass)
📖 HTTP: The Definitive Guide — Gourley & Totty, Cap. 12 (Basic Auth) e Cap. 14 (HTTPS)

---

## Escala de Pontuação

| Pontuação | Nível |
|-----------|-------|
| 0–5 | Fundamentos precisam de atenção |
| 6–10 | Base sólida em conceitos isolados |
| 11–15 | Bom domínio, lacunas em síntese |
| 16–20 | Pensamento de engenheiro em formação |
| 21–25 | Pronto para discutir arquitetura |

**Como pontuar:** 1 ponto por questão. Para Nível 3, considere 0.5 se o raciocínio estava certo mas a referência aos livros estava errada.
