# Python Concurrency with asyncio — Matthew Fowler (2022)

## Tese Central
asyncio resolve o problema de I/O concorrente em Python sem threads — usando um único thread com um event loop que alterna entre tarefas enquanto cada uma espera por I/O. Entender o modelo é essencial para escrever aplicações FastAPI corretas e eficientes.

## Por que Este Livro Importa
FastAPI é construído sobre asyncio. Qualquer endpoint `async def` usa o event loop. Sem entender asyncio, você escreve endpoints async que bloqueiam o event loop e derrubam a performance da aplicação inteira — sem perceber.

## Onde o Livro Pode Ser Questionado
- O livro cobre multithreading e multiprocessing também — para o ZionHub, asyncio é o foco principal
- Alguns padrões avançados (circuit breaker, backpressure) são para sistemas de alta escala
- A sintaxe `async/await` esconde a complexidade — o livro a expõe, o que pode ser assustador antes de ser útil
- asyncio não resolve problemas de CPU — para isso, use `ProcessPoolExecutor` ou Celery

## Frases-Chave do Autor
> "asyncio uses a single-threaded event loop to execute coroutines concurrently when they are waiting for I/O." — Cap. 1

> "A coroutine is a function that can pause its execution and yield control back to the event loop." — Cap. 2

## Mapa por Capítulo

### Cap. 1 — Getting to Know asyncio (Caps. 1 e 3 do guia de "I want to...")
**Conceito central:** Single-threaded concurrency. O event loop. Diferença entre concorrência e paralelismo.
**Regras práticas:**
- `async def` define uma coroutine — ela não executa até ser aguardada com `await`
- `await` cede controle ao event loop — o event loop pode rodar outras coroutines enquanto espera
- Nunca chame código bloqueante (ex: `requests.get()`, `time.sleep()`) dentro de `async def` sem wrapping
**Frase marcante:** "asyncio is not about running code faster — it's about running more code concurrently."
**Conexão:** Fluent Python (Ramalho) — Cap. 17 explica generators, que são a base das coroutines.

### Cap. 2 — asyncio Basics: Tasks, Events, and Timeouts
**Conceito central:** `asyncio.Task` como unidade de concorrência. Tasks rodam no event loop concorrentemente.
**Regras práticas:**
- `asyncio.create_task(coro())` agenda a coroutine para execução imediata (não bloqueia)
- `await task` espera a task completar
- `asyncio.wait_for(coro(), timeout=5.0)` para timeouts
- `task.cancel()` para cancelar — gera `CancelledError` na coroutine
**Exemplo do autor:** Fetch de múltiplas URLs concorrentemente — sem tasks, seria sequencial.
**Conexão:** Effective Python (Slatkin) — Item 75: Achieve Highly Concurrent I/O with Coroutines.

### Cap. 3 — Non-blocking Sockets (o modelo por baixo)
**Conceito central:** Sockets não-bloqueantes como fundamento do event loop. `selectors` como a API de baixo nível.
**Regras práticas:** Para o ZionHub, isso é conhecimento de contexto — não é necessário usar seletores diretamente. Entender o modelo ajuda a diagnosticar problemas de performance.
**Conexão:** HTTP: The Definitive Guide (Gourley & Totty) — como as conexões HTTP funcionam no nível do socket.

### Cap. 4 — Concurrent Web Requests (asyncio.gather, wait, as_completed)
**Conceito central:** APIs para rodar múltiplas coroutines concorrentemente.
**Regras práticas:**
- `asyncio.gather(*coros)` — roda todas as coroutines concorrentemente, retorna resultados em ordem
- `asyncio.gather(*coros, return_exceptions=True)` — não cancela tudo se uma falha
- `asyncio.wait(tasks, return_when=FIRST_COMPLETED)` — para padrões mais complexos
- `asyncio.as_completed(coros)` — processa resultados na ordem que terminam
**Exemplo do autor:** Fetch de N URLs concorrentes com aiohttp.
**Conexão:** Designing Data-Intensive Applications (Kleppmann) — o modelo de concorrência afeta consistência dos dados.

### Cap. 5 — Non-blocking Database Drivers (asyncpg)
**Conceito central:** Drivers assíncronos para PostgreSQL — `asyncpg` como alternativa ao psycopg2 bloqueante.
**Regras práticas:**
- SQLAlchemy com `asyncpg` para ORM assíncrono — `async_session`
- Connection pools são críticos para performance — `create_async_engine(pool_size=...)`
- `async with session.begin():` para transações assíncronas
**Conexão:** PostgreSQL Up and Running (Obe & Hsu) — o banco por baixo. Designing Data-Intensive Applications (Kleppmann) — modelos de transação.

### Cap. 6 — Multiprocessing (CPU-bound work)
**Conceito central:** Quando asyncio não é suficiente — tasks CPU-bound bloqueiam o event loop.
**Regras práticas:**
- `loop.run_in_executor(ProcessPoolExecutor(), cpu_bound_func)` para offload de CPU
- Para o ZionHub: tasks pesadas de processamento devem usar Celery ou `ProcessPoolExecutor`, não asyncio
**Onde questionar:** Multiprocessing adiciona complexidade (IPC, serialização, debugging) — vale apenas quando necessário.

### Cap. 7 — Multithreading (I/O bloqueante com bibliotecas síncronas)
**Conceito central:** Quando você precisa usar uma biblioteca bloqueante (ex: `requests`, `boto3`) dentro de código assíncrono.
**Regras práticas:**
- `asyncio.run_in_executor(ThreadPoolExecutor(), blocking_func)` para offload de I/O bloqueante
- `asyncio.run_in_executor(None, ...)` usa o default ThreadPoolExecutor do event loop
**Frase marcante:** Threading em Python é para I/O; multiprocessing é para CPU — a GIL limita o paralelismo de threads.
**Conexão:** Effective Python (Slatkin) — Item 68: Threads para I/O.

### Cap. 9 — Web Applications with asyncio (FastAPI/ASGI)
**Conceito central:** ASGI (Async Server Gateway Interface) — o protocolo que FastAPI usa em vez de WSGI.
**Regras práticas:**
- Qualquer endpoint `async def` no FastAPI pode fazer `await` de operações de I/O sem bloquear outros requests
- Endpoints síncronos (`def`, não `async def`) são executados em um threadpool — não bloqueiam o event loop
- Nunca use operações bloqueantes em endpoints `async def` — use `asyncio.run_in_executor` se necessário

### Cap. 11 — asyncio Locks, Semaphores, Events (sincronização)
**Conceito central:** Primitivas de sincronização para coordenar coroutines que compartilham estado.
**Regras práticas:**
- `asyncio.Lock()` para mutual exclusion (não use threading.Lock em asyncio)
- `asyncio.Semaphore(n)` para limitar concorrência (ex: max N requests simultâneos a uma API externa)
- `asyncio.Event()` para sincronização de sinalização entre coroutines

## Tensões com Outros Livros
- **vs. Effective Python (Slatkin):** Slatkin cobre asyncio em itens isolados; Fowler dá a visão completa e integrada.
- **vs. Fluent Python (Ramalho):** Ramalho explica o modelo teórico de coroutines (Cap. 21); Fowler aplica em sistemas reais.
- **vs. Site Reliability Engineering:** SRE se preocupa com confiabilidade em escala; asyncio resolve concorrência. São complementares na produção.

## Aplicação em Python Moderno (FastAPI)
```python
# ❌ Endpoint async que bloqueia o event loop
@router.get("/users/{user_id}")
async def get_user(user_id: int):
    import time
    time.sleep(1)  # BLOQUEANTE — trava o event loop inteiro!
    return {"user_id": user_id}

# ✅ Usando await corretamente
@router.get("/users/{user_id}")
async def get_user(user_id: int, db: AsyncSession = Depends(get_async_db)):
    user = await db.get(User, user_id)  # não-bloqueante
    if not user:
        raise HTTPException(404)
    return user

# ✅ asyncio.gather para múltiplas queries concorrentes
@router.get("/dashboard")
async def get_dashboard(db: AsyncSession = Depends(get_async_db)):
    users_task = asyncio.create_task(get_all_users(db))
    orders_task = asyncio.create_task(get_recent_orders(db))
    users, orders = await asyncio.gather(users_task, orders_task)
    return {"users": users, "orders": orders}

# ✅ Semáforo para limitar calls a API externa
semaphore = asyncio.Semaphore(10)  # máximo 10 calls simultâneos

async def call_external_api(item_id: int):
    async with semaphore:
        async with aiohttp.ClientSession() as session:
            async with session.get(f"/api/{item_id}") as resp:
                return await resp.json()
```
