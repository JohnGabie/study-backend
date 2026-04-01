# Prova 4 — Código
**Data:** 2026-04-01
**Total:** 3 questões | Foco: escrever código, não explicar
**Instruções:** Escreva o código. Sem consultar nada. Se travar, escreva em pseudocódigo ou descreva o que tentaria fazer — isso também vale.

---

## Q1 — Fácil

Você tem uma lista de pedidos. Cada pedido é um dicionário com `user_id` e `total`.

```python
pedidos = [
    {"user_id": 1, "total": 150.0},
    {"user_id": 2, "total": 80.0},
    {"user_id": 1, "total": 200.0},
    {"user_id": 3, "total": 50.0},
    {"user_id": 2, "total": 120.0},
]
```

Escreva uma função `total_por_usuario(pedidos)` que retorna um dicionário com o total gasto por cada usuário.

Resultado esperado:
```python
{1: 350.0, 2: 200.0, 3: 50.0}
```

*Seu código:*

---

## Q2 — Médio

Você tem esse endpoint FastAPI com um problema sério de semântica e segurança.

```python
@router.get("/users/{user_id}/orders")
async def get_orders(user_id: int, db: Session = Depends(get_db)):
    orders = db.query(Order).filter(Order.user_id == user_id).all()
    return orders
```

**Reescreva esse endpoint** corrigindo os dois problemas:
1. Qualquer usuário autenticado pode ver os pedidos de qualquer outro usuário passando um `user_id` diferente
2. Se não houver pedidos, o comportamento atual é silencioso

Assuma que você tem acesso a `current_user` via `Depends(get_current_user)` e que ele tem um atributo `id`.

*Seu código:*

---

## Q3 — Difícil

Escreva a query SQL que responde à seguinte pergunta:

> "Quais usuários fizeram mais de 2 pedidos com total acima de R$ 100, e qual foi o valor médio dos pedidos desses usuários?"

Você tem duas tabelas:
```sql
users (id, name, email)
orders (id, user_id, total, status)
```

O resultado deve conter: `user_id`, `name`, `quantidade_pedidos`, `media_total` — ordenado pelo maior valor médio primeiro.

*Sua query:*

---

**Quando terminar:** manda o código e eu corrijo.
