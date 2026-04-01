# Prova Geral — Biblioteca de Engenharia de Software
**Data:** 2026-03-31
**Total:** 25 questões | 3 níveis de dificuldade
**Instruções:** Responda com suas próprias palavras. Não consulte os livros durante a prova. Anote suas respostas abaixo de cada pergunta.

---

## Nível 1 — Conceito (questões 1–8)
> Explique com suas palavras, sem consultar nada.

---

**Q1.** O que é o Single Responsibility Principle (SRP)? Dê um exemplo de violação dele em um endpoint FastAPI.

*Sua resposta:*

---

**Q2.** Qual é a diferença entre `async def` e `def` em FastAPI? O que acontece com o event loop quando você usa `time.sleep(3)` dentro de um `async def`?

*Sua resposta:*

---

**Q3.** O que é o DRY principle? Por que o Pragmatic Programmer diz que DRY é sobre *conhecimento*, não sobre *código*? Dê um exemplo onde dois trechos de código idênticos **não** são uma violação de DRY.

*Sua resposta:*

---

**Q4.** Quais são os 4 Golden Signals do SRE? Para cada um, diga como você mediria isso em um backend FastAPI.

*Sua resposta:*

---

**Q5.** Explique o ciclo Red → Green → Refactor do TDD. Por que escrever o teste *antes* do código muda o design do código?

*Sua resposta:*

---

**Q6.** O que é o Python Data Model? Por que implementar `__len__` e `__getitem__` em uma classe faz ela ganhar suporte a `len()`, `for`, e slicing automaticamente?

*Sua resposta:*

---

**Q7.** Qual é a diferença entre SQL Injection e IDOR? Como o SQLAlchemy protege contra SQL Injection? O que protege contra IDOR?

*Sua resposta:*

---

**Q8.** O que significa dizer que HTTP é *stateless*? Quais são as consequências disso para o design de uma API REST?

*Sua resposta:*

---

## Nível 2 — Aplicação (questões 9–18)
> Analise o código ou cenário e identifique o problema.

---

**Q9.** Analise esse endpoint. Quantos problemas você consegue identificar? Cite o livro e capítulo para cada um.

```python
@router.post("/user")
async def handle_user(data: dict):
    # validate
    if not data.get("email"):
        return {"error": "email required"}
    if not data.get("name"):
        return {"error": "name required"}
    # save
    user = User(email=data["email"], name=data["name"])
    db.add(user)
    db.commit()
    # notify
    requests.post("https://email-service/send", json={
        "to": data["email"],
        "subject": "Welcome!"
    })
    return user
```

*Sua resposta:*

---

**Q10.** Este código tem um bug clássico de Python. Identifique, explique por que acontece, e corrija.

```python
def create_order(user_id: int, tags=[]):
    tags.append("new")
    return {"user_id": user_id, "tags": tags}
```

*Sua resposta:*

---

**Q11.** O que esse código faz de errado no contexto de segurança? Como corrigir?

```python
@router.get("/orders/{order_id}")
async def get_order(order_id: int, db: Session = Depends(get_db)):
    order = db.query(Order).filter(Order.id == order_id).first()
    if not order:
        raise HTTPException(404)
    return order
```

*Sua resposta:*

---

**Q12.** Esse teste está bem escrito? O que está errado? Como você reescreveria?

```python
def test_user():
    user = create_user("joao@email.com", "João")
    assert user is not None
    assert user.email == "joao@email.com"
    assert user.name == "João"
    assert user.id > 0
    assert user.created_at is not None
    update_user(user.id, name="João Silva")
    updated = get_user(user.id)
    assert updated.name == "João Silva"
    delete_user(user.id)
    assert get_user(user.id) is None
```

*Sua resposta:*

---

**Q13.** Esse código tem um N+1 problem. Explique o que é, por que acontece aqui, e como corrigir.

```python
@router.get("/users")
async def list_users(db: AsyncSession = Depends(get_db)):
    users = await db.execute(select(User))
    users = users.scalars().all()
    result = []
    for user in users:
        orders = await db.execute(select(Order).where(Order.user_id == user.id))
        result.append({
            "user": user,
            "orders": orders.scalars().all()
        })
    return result
```

*Sua resposta:*

---

**Q14.** Qual é o problema com essa função do ponto de vista do Pragmatic Programmer?

```python
def get_user_data(user_id: int) -> dict | None:
    user = db.query(User).get(user_id)
    if not user:
        return None
    orders = db.query(Order).filter(Order.user_id == user_id).all()
    if not orders:
        return None
    return {"user": user, "orders": orders}
```

*Sua resposta:*

---

**Q15.** Esse schema de banco tem um problema sutil. Identifique e explique as consequências em produção.

```sql
CREATE TABLE transactions (
    id SERIAL PRIMARY KEY,
    user_id INTEGER,
    amount FLOAT,
    created_at TIMESTAMP
);
```

*Sua resposta:*

---

**Q16.** Por que essa fixture de pytest é problemática? Como corrigir?

```python
@pytest.fixture(scope="session")
def db():
    Base.metadata.create_all(engine)
    session = SessionLocal()
    yield session
    session.close()
```

*Sua resposta:*

---

**Q17.** Esse endpoint tem um problema de design REST. Qual é? Como corrigir?

```python
@router.get("/getUserById/{id}")
async def get_user_by_id(id: int):
    ...

@router.post("/createOrder")
async def create_order(data: dict):
    ...

@router.delete("/deleteUser/{id}")
async def delete_user(id: int):
    ...
```

*Sua resposta:*

---

**Q18.** Qual é o problema com esse tratamento de erro em produção?

```python
@app.exception_handler(Exception)
async def global_exception_handler(request, exc):
    return JSONResponse(
        status_code=500,
        content={
            "error": str(exc),
            "traceback": traceback.format_exc()
        }
    )
```

*Sua resposta:*

---

## Nível 3 — Síntese (questões 19–25)
> Conecte dois ou mais conceitos/livros para responder.

---

**Q19.** Martin Fowler em *Refactoring* diz que o primeiro passo de qualquer refatoração é garantir que há testes. Mas o ZionHub tem código vibecoded sem testes. Descreva o processo completo que você seguiria para refatorar um endpoint sem testes com segurança. Cite pelo menos 2 livros.

*Sua resposta:*

---

**Q20.** Há uma tensão entre Clean Code Fundamentals e A Philosophy of Software Design sobre comentários. Qual é a posição de cada um? Com qual você concorda no contexto do ZionHub, e por quê? Dê um exemplo de comentário que seria ruim segundo Hock mas bom segundo Ousterhout.

*Sua resposta:*

---

**Q21.** Um endpoint FastAPI recebe um `user_id` e retorna todos os pedidos desse usuário com os produtos de cada pedido. Descreva: (a) o schema SQL, (b) a query eficiente, (c) o endpoint correto com autenticação e autorização, (d) o teste que verificaria que um usuário não pode ver pedidos de outro. Use os livros que forem relevantes.

*Sua resposta:*

---

**Q22.** O Pragmatic Programmer diz "don't outrun your headlights" e também recomenda "tracer bullets". Parecem contraditórios — um diz não avance demais, o outro diz construa o sistema completo logo. Como você resolve essa tensão na prática do ZionHub?

*Sua resposta:*

---

**Q23.** Explique como `asyncio.gather()`, `asyncio.Semaphore()`, e um driver assíncrono de banco de dados se relacionam para construir um endpoint que faz 3 queries ao banco em paralelo sem sobrecarregar a conexão. Cite Fowler (asyncio) e explique qual o risco se você esquecer o semáforo.

*Sua resposta:*

---

**Q24.** Um usuário reporta que a API "está lenta às vezes". Com base no SRE (4 Golden Signals) e em DDIA (como descrever performance), descreva: (a) qual métrica você olharia primeiro e por quê p99 é mais revelador que a média, (b) como você encontraria a query lenta, (c) como você saberia se é problema do banco ou da aplicação.

*Sua resposta:*

---

**Q25.** Você está fazendo code review de um PR vibecoded que implementa autenticação. Liste os 5 problemas de segurança que você verificaria primeiro (Web Application Security + HTTP Guide), e para cada um: o que seria o sinal de alerta no código e qual seria a correção.

*Sua resposta:*

---

## Espaço para anotações

*Use este espaço para dúvidas ou pensamentos durante a prova:*

---

**Quando terminar:** salve este arquivo com suas respostas e peça ao mentor para corrigir com `corrige prova quizzes/prova_geral_2026-03-31.md`.
