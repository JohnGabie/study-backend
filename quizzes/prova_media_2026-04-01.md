# Prova 2 — Nível Médio
**Data:** 2026-04-01
**Total:** 25 questões | Foco: conceitos práticos de Python, FastAPI, SQL, HTTP e qualidade de código
**Instruções:** Responda com suas palavras. Não consulte nada. Anote abaixo de cada pergunta.

---

## Bloco 1 — Python (questões 1–6)

**Q1.** O que é uma list comprehension? Reescreva esse código usando uma:
```python
result = []
for n in range(10):
    if n % 2 == 0:
        result.append(n * 2)
```

*Sua resposta:*

---

**Q2.** Qual a diferença entre `append` e `extend` em uma lista? O que cada um faz com esse código:
```python
a = [1, 2, 3]
a.append([4, 5])

b = [1, 2, 3]
b.extend([4, 5])
```

*Sua resposta:*

---

**Q3.** O que são `*args` e `**kwargs`? Quando você usaria cada um?

*Sua resposta:*

---

**Q4.** Qual o problema com esse código? Como corrigir?
```python
def get_discount(price, percentage=10):
    return price - (price * percentage / 100)

result = get_discount(100, 0.1)
```

*Sua resposta:*

---

**Q5.** O que faz o `try/except/else/finally`? O que roda em cada bloco?
```python
try:
    result = int("abc")
except ValueError:
    print("erro")
else:
    print("sucesso")
finally:
    print("sempre")
```

*Sua resposta:*

---

**Q6.** Qual a diferença entre `==` e `is` em Python? Dê um exemplo onde um retorna `True` e o outro `False` para os mesmos objetos.

*Sua resposta:*

---

## Bloco 2 — FastAPI e HTTP (questões 7–13)

**Q7.** O que é um status code HTTP? Sem consultar nada, liste pelo menos 4 que você conhece e o que cada um significa.

*Sua resposta:*

---

**Q8.** Qual a diferença entre `GET` e `POST`? Quando usar cada um?

*Sua resposta:*

---

**Q9.** O que faz esse código FastAPI? Descreva o que acontece quando alguém faz uma requisição para `/users/42`:
```python
@router.get("/users/{user_id}")
async def get_user(user_id: int):
    return {"user_id": user_id}
```

*Sua resposta:*

---

**Q10.** O que é Pydantic e por que ele é usado no FastAPI? O que acontece se o cliente enviar `age: "vinte"` para um campo declarado como `age: int`?

*Sua resposta:*

---

**Q11.** Esse endpoint tem um problema de semântica HTTP. Qual é?
```python
@router.get("/users/delete/{user_id}")
async def delete_user(user_id: int):
    db.delete(user_id)
    return {"deleted": True}
```

*Sua resposta:*

---

**Q12.** O que é um `Depends` no FastAPI? Para que serve esse padrão?
```python
async def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@router.get("/orders")
async def list_orders(db: Session = Depends(get_db)):
    ...
```

*Sua resposta:*

---

**Q13.** Qual a diferença entre `raise HTTPException(status_code=404)` e `return {"error": "not found"}`? Por que um é melhor que o outro?

*Sua resposta:*

---

## Bloco 3 — SQL e Banco (questões 14–18)

**Q14.** O que é um `JOIN`? Quando você precisaria usar um? Escreva um exemplo simples de `SELECT` com `JOIN` entre uma tabela `users` e uma tabela `orders`.

*Sua resposta:*

---

**Q15.** O que é uma `PRIMARY KEY`? O que é uma `FOREIGN KEY`? Qual a diferença?

*Sua resposta:*

---

**Q16.** O que esse SQL retorna?
```sql
SELECT user_id, COUNT(*) as total
FROM orders
WHERE status = 'completed'
GROUP BY user_id
HAVING COUNT(*) > 3
ORDER BY total DESC;
```

*Sua resposta:*

---

**Q17.** O que é um índice em banco de dados? Por que você criaria um na coluna `user_id` da tabela `orders`?

*Sua resposta:*

---

**Q18.** Qual o problema com esse tipo de dado para armazenar preço?
```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name TEXT,
    price FLOAT
);
```

*Sua resposta:*

---

## Bloco 4 — Qualidade e Raciocínio (questões 19–25)

**Q19.** Olhe esse código. O que ele faz? Tem algum problema?
```python
def p(u, a=0, d=0):
    u.a = a
    u.d = d
    return u
```

*Sua resposta:*

---

**Q20.** Por que isso é um problema?
```python
@router.post("/login")
async def login(email: str, password: str):
    user = db.query(User).filter(User.email == email).first()
    if not user:
        raise HTTPException(401, "Usuário não encontrado")
    if not verify_password(password, user.password):
        raise HTTPException(401, "Senha incorreta")
```

*Sua resposta:*

---

**Q21.** O que é um `schema` no contexto de FastAPI? Qual a diferença entre um schema de `request` e um de `response`?

*Sua resposta:*

---

**Q22.** Você tem um endpoint que está lento. A primeira coisa que você faria para investigar seria o quê? Liste pelo menos 2 passos concretos.

*Sua resposta:*

---

**Q23.** O que significa dizer que uma operação é **idempotente**? `GET` é idempotente? `POST` é? `DELETE` é?

*Sua resposta:*

---

**Q24.** Qual o problema com essa estrutura de rota?
```
POST /users/create
GET  /users/getAll
PUT  /users/updateUser/42
DEL  /users/removeUser/42
```

Como ficaria correto?

*Sua resposta:*

---

**Q25.** Por que separar a lógica de negócio do endpoint é uma boa prática? Dê um exemplo de como você faria essa separação em FastAPI.

*Sua resposta:*

---

**Quando terminar:** me manda as respostas e eu corrijo com relatório.
