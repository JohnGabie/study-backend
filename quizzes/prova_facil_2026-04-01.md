# Prova 3 — Nível Fácil
**Data:** 2026-04-01
**Total:** 25 questões | Foco: fundamentos de Python, HTTP e SQL básico
**Instruções:** Responda com suas palavras. Não consulte nada. Anote abaixo de cada pergunta.

---

## Bloco 1 — Python Básico (questões 1–10)

**Q1.** Quais são os tipos de dados básicos do Python? Liste pelo menos 4 com um exemplo de cada.

*Sua resposta:*

---

**Q2.** Qual a diferença entre uma lista e uma tupla em Python?
```python
lista = [1, 2, 3]
tupla = (1, 2, 3)
```

*Sua resposta:*

---

**Q3.** O que esse código retorna? Por quê?
```python
x = 10
y = 3
print(x // y)
print(x % y)
```

*Sua resposta:*

---

**Q4.** O que é uma função em Python? O que essa função faz?
```python
def somar(a, b):
    return a + b
```

*Sua resposta:*

---

**Q5.** Qual o erro nesse código?
```python
nome = "João"
idade = 25
print("Meu nome é " + nome + " e tenho " + idade + " anos")
```

*Sua resposta:*

---

**Q6.** O que é um dicionário em Python? O que esse código faz?
```python
usuario = {"nome": "João", "idade": 25}
print(usuario["nome"])
usuario["email"] = "joao@email.com"
```

*Sua resposta:*

---

**Q7.** O que acontece se você tentar acessar uma chave que não existe no dicionário?
```python
usuario = {"nome": "João"}
print(usuario["email"])
```

*Sua resposta:*

---

**Q8.** O que esse loop faz?
```python
numeros = [1, 2, 3, 4, 5]
for n in numeros:
    if n % 2 == 0:
        print(n)
```

*Sua resposta:*

---

**Q9.** Qual a diferença entre `return` e `print`? Por que isso importa?
```python
def dobrar(x):
    return x * 2

def mostrar(x):
    print(x * 2)
```

*Sua resposta:*

---

**Q10.** O que é `None` em Python? Quando uma função retorna `None`?

*Sua resposta:*

---

## Bloco 2 — HTTP e API (questões 11–18)

**Q11.** O que é uma API? Explique com suas palavras como se fosse explicar para alguém não técnico.

*Sua resposta:*

---

**Q12.** O que acontece quando você digita um endereço no navegador e aperta Enter? Descreva o que ocorre em alto nível.

*Sua resposta:*

---

**Q13.** O que é uma URL? Identifique as partes dessa URL:
```
https://api.zionhub.com/users/42?active=true
```

*Sua resposta:*

---

**Q14.** Qual a diferença entre o cliente e o servidor em uma aplicação web?

*Sua resposta:*

---

**Q15.** O que significa o status code `200`? E o `404`? Em que situação cada um aparece?

*Sua resposta:*

---

**Q16.** O que esse endpoint FastAPI faz? O que ele retorna?
```python
@app.get("/hello")
def hello():
    return {"message": "Hello World"}
```

*Sua resposta:*

---

**Q17.** O que é JSON? Por que APIs usam JSON?

*Sua resposta:*

---

**Q18.** Qual a diferença entre uma requisição e uma resposta HTTP?

*Sua resposta:*

---

## Bloco 3 — SQL Básico (questões 19–25)

**Q19.** O que é um banco de dados? Para que serve?

*Sua resposta:*

---

**Q20.** O que esse SQL faz?
```sql
SELECT * FROM users;
```

*Sua resposta:*

---

**Q21.** O que essa query retorna?
```sql
SELECT name, email FROM users WHERE active = true;
```

*Sua resposta:*

---

**Q22.** Como você escreveria o SQL para inserir um novo usuário na tabela `users` com `name = 'João'` e `email = 'joao@email.com'`?

*Sua resposta:*

---

**Q23.** O que faz o `WHERE` em uma query SQL? Por que ele é importante?

*Sua resposta:*

---

**Q24.** O que é uma tabela em banco de dados? Qual a relação entre tabela, coluna e linha?

*Sua resposta:*

---

**Q25.** O que esse SQL faz? O que você receberia como resultado?
```sql
SELECT COUNT(*) FROM orders WHERE user_id = 42;
```

*Sua resposta:*

---

**Quando terminar:** me manda as respostas e eu corrijo com relatório.
