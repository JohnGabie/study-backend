# Gabarito — Prova 2 (Médio)
**Data:** 2026-04-01

---

## Q1 — List Comprehension

**Resposta correta:**
List comprehension é uma sintaxe Python para criar listas de forma concisa em uma linha.

```python
result = [n * 2 for n in range(10) if n % 2 == 0]
```

Estrutura: `[expressão for item in iterável if condição]`

📖 Effective Python — Brett Slatkin
Item 27: "Use Comprehensions Instead of map and filter"
"List comprehensions are clearer than the map and filter built-in functions because they don't require extra lambda expressions."

---

## Q2 — Append vs Extend

**Resposta correta:**
```python
a = [1, 2, 3]
a.append([4, 5])
# a = [1, 2, 3, [4, 5]] ← adiciona o objeto inteiro como um elemento

b = [1, 2, 3]
b.extend([4, 5])
# b = [1, 2, 3, 4, 5] ← itera e adiciona cada elemento individualmente
```

`append` sempre adiciona um único elemento. `extend` itera o argumento e adiciona cada item.

---

## Q3 — *args e **kwargs

**Resposta correta:**
```python
def func(*args, **kwargs):
    # args é uma tupla com argumentos posicionais extras
    # kwargs é um dict com argumentos nomeados extras
    print(args)   # (1, 2, 3)
    print(kwargs) # {'nome': 'João', 'ativo': True}

func(1, 2, 3, nome='João', ativo=True)
```

`*args` — quando você não sabe quantos argumentos posicionais serão passados.
`**kwargs` — quando você não sabe quantos argumentos nomeados serão passados.

Uso comum: wrappers, decorators, funções que delegam para outras.

---

## Q4 — Default Argument (Problema de Unidade)

**Resposta correta:**
```python
def get_discount(price, percentage=10):
    return price - (price * percentage / 100)

# Chamada correta seria:
get_discount(100, 10)    # → 90.0  (10% de desconto)

# O que foi feito:
get_discount(100, 0.1)   # → 99.9  (0.1% de desconto — quase nenhum)
```

O problema é de **unidade**: a função espera `percentage` como número inteiro (10 = 10%),
mas o chamador passou `0.1` — que seria 0.1%, não 10%. Resultado: 99.9 em vez de 90.

A fix pode ser documentar claramente ou mudar para `percentage=0.10` (decimal 0–1)
e ajustar a fórmula para `price * (1 - percentage)`.

---

## Q5 — try/except/else/finally

**Resposta correta:**
```python
try:
    result = int("abc")  # ← lança ValueError
except ValueError:
    print("erro")        # ← RODA: "abc" não é int
else:
    print("sucesso")     # ← NÃO roda: só roda se try não lançar exceção
finally:
    print("sempre")      # ← SEMPRE roda, com ou sem exceção
```

Output: `"erro"` → `"sempre"`

- `try`: tenta executar
- `except`: trata exceção específica
- `else`: só roda se try completou SEM exceção
- `finally`: roda sempre — útil para cleanup (fechar arquivo, conexão, etc.)

---

## Q6 — == vs is

**Resposta correta:**
- `==` compara **valor** (conteúdo)
- `is` compara **identidade** (mesmo objeto na memória)

```python
a = [1, 2, 3]
b = [1, 2, 3]

a == b  # True  — mesmo valor
a is b  # False — objetos diferentes na memória

c = a
a is c  # True  — mesma referência
```

📖 Fluent Python — Luciano Ramalho
Cap. 8: "Object References, Mutability, and Recycling"
"Every Python object has an identity, a type, and a value. An object's identity never changes once it has been created."

---

## Q7 — Status Codes HTTP

**Resposta correta (mínimo 4):**
- `200 OK` — requisição bem-sucedida
- `201 Created` — recurso criado com sucesso (POST)
- `400 Bad Request` — erro do cliente (dados inválidos)
- `401 Unauthorized` — não autenticado (sem credenciais)
- `403 Forbidden` — autenticado mas sem permissão
- `404 Not Found` — recurso não encontrado
- `422 Unprocessable Entity` — validação falhou (FastAPI usa para Pydantic)
- `500 Internal Server Error` — erro inesperado no servidor

**Nota:** 402 = Payment Required (não é autenticação).

---

## Q8 — GET vs POST

**Resposta correta:**
- `GET` — recupera dados. Idempotente. Sem corpo. Pode ser cacheado. URL pública.
- `POST` — cria ou envia dados. Não idempotente. Tem corpo. Não cacheado.

Usar GET para: buscar usuário, listar pedidos, filtrar produtos.
Usar POST para: criar usuário, fazer login, enviar formulário, iniciar processo.

📖 HTTP: The Definitive Guide — Gourley & Totty
Cap. 3: "HTTP Messages"
"GET is the most common method. It asks the server to send the resource."

---

## Q9 — Path Parameters no FastAPI

**Resposta correta:**
Quando alguém faz `GET /users/42`:
1. FastAPI captura `42` da URL e converte automaticamente para `int` (porque o tipo declarado é `int`)
2. Se a URL fosse `/users/abc`, FastAPI retorna 422 automaticamente (tipo inválido)
3. A função é chamada com `user_id=42`
4. Retorna `{"user_id": 42}` como JSON com status 200

A conversão automática de tipo é feita pelo FastAPI via Pydantic internamente.

---

## Q10 — Pydantic

**Resposta correta:**
Pydantic é uma biblioteca de validação de dados usada pelo FastAPI para validar
automaticamente os dados que entram e saem dos endpoints.

Se o cliente enviar `age: "vinte"` para um campo declarado como `age: int`:
- FastAPI retorna **HTTP 422 Unprocessable Entity** automaticamente
- O endpoint nem chega a ser executado
- O corpo da resposta descreve exatamente qual campo falhou e por quê

📖 Building Data Science Applications with FastAPI — François Voron
Cap. 3: "Developing a RESTful API with FastAPI"
"Thanks to Pydantic, FastAPI automatically validates the data you receive in a request and returns a 422 status code if the data doesn't match the expected types."

---

## Q11 — Semântica HTTP

**Resposta correta:**
O problema é usar `@router.get` para uma operação de deleção.
`GET` por convenção HTTP é seguro e idempotente — não deve causar efeitos colaterais.
Uma operação destrutiva deve usar `DELETE`.

Correto:
```python
@router.delete("/users/{user_id}")
async def delete_user(user_id: int):
    db.delete(user_id)
    return {"deleted": True}
```

📖 REST API Design Rulebook — Mark Massé
Cap. 4: "HTTP"
"GET must not change the state of the resource. DELETE requests a deletion."

---

## Q12 — Depends no FastAPI

**Resposta correta:**
`Depends` é o sistema de **injeção de dependência** do FastAPI.

No exemplo:
1. `get_db()` é um gerador — cria a sessão do banco, faz `yield`, depois fecha no `finally`
2. FastAPI chama `get_db()` automaticamente antes de chamar `list_orders`
3. Injeta a sessão `db` como parâmetro da função
4. Quando o endpoint termina, o `finally` de `get_db()` fecha a sessão

Vantagens: reutilização, testabilidade (pode substituir por mock no teste), separação de concerns.

---

## Q13 — HTTPException vs return de erro

**Resposta correta:**
```python
# Ruim:
return {"error": "not found"}
# → Retorna HTTP 200 com corpo de erro. O cliente precisa parsear o JSON para saber que falhou.

# Bom:
raise HTTPException(status_code=404, detail="not found")
# → Retorna HTTP 404. O cliente sabe imediatamente (pelo status code) que algo deu errado.
```

O protocolo HTTP tem status codes exatamente para isso. Usar `return` com mensagem de erro
quebra a semântica HTTP e obriga todos os clientes a fazer parsing de JSON para detectar erros.

---

## Q14 — JOIN

**Resposta correta:**
JOIN combina linhas de duas tabelas com base em uma coluna relacionada.

```sql
SELECT users.id, users.name, orders.id AS order_id, orders.total
FROM users
JOIN orders ON orders.user_id = users.id;
```

Usar quando: dados estão normalizados em tabelas diferentes e você precisa deles juntos.

📖 Learning SQL — Alan Beaulieu
Cap. 5: "Querying Multiple Tables"
"The join condition is the mechanism by which rows in different tables are associated with each other."

---

## Q15 — PRIMARY KEY / FOREIGN KEY

**Resposta correta:**
- `PRIMARY KEY` — identificador único de uma linha dentro da própria tabela. Não pode ser NULL, não pode repetir.
- `FOREIGN KEY` — referência ao PRIMARY KEY de outra tabela. Garante integridade referencial (não permite inserir um user_id que não existe na tabela users).

```sql
CREATE TABLE users (id SERIAL PRIMARY KEY, name TEXT);
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(id),  -- FOREIGN KEY
    total NUMERIC(10,2)
);
```

---

## Q16 — Análise de Query SQL

**Resposta correta:**
```sql
SELECT user_id, COUNT(*) as total
FROM orders
WHERE status = 'completed'
GROUP BY user_id
HAVING COUNT(*) > 3
ORDER BY total DESC;
```

Retorna: lista de usuários que fizeram **mais de 3 pedidos com status 'completed'**,
exibindo o user_id e a contagem de pedidos, ordenados do maior para o menor.

- `WHERE`: filtra antes de agrupar (só pedidos completed)
- `GROUP BY`: agrupa por usuário
- `HAVING`: filtra depois de agrupar (só quem tem mais de 3)
- `ORDER BY total DESC`: ordena pelo total decrescente

---

## Q17 — Índices em Banco

**Resposta correta:**
Um índice é uma estrutura de dados (geralmente B-tree) que o banco mantém para acelerar
buscas em uma coluna específica — sem precisar varrer a tabela inteira.

Criar índice em `orders.user_id` porque:
- Toda query que busca pedidos de um usuário específico passa por `WHERE user_id = ?`
- Sem índice: PostgreSQL varre todas as linhas da tabela (full table scan)
- Com índice: PostgreSQL vai direto às linhas daquele user_id

Índice não é sobre ordenação — é sobre velocidade de busca.

📖 PostgreSQL: Up and Running — Obe & Hsu
Cap. 7: "PostgreSQL Performance"
"Indexes speed up data retrieval but slow down writes. Add them on columns you frequently filter, join, or sort by."

---

## Q18 — FLOAT para dinheiro

**Resposta correta:**
O problema é **imprecisão de ponto flutuante**, não "casas demais".

```python
>>> 0.1 + 0.2
0.30000000000000004
```

FLOAT e DOUBLE usam representação binária — alguns números decimais não têm representação
exata em binário. Isso acumula erro invisível em cálculos financeiros.

**Correto:**
```sql
price NUMERIC(10, 2)  -- ou DECIMAL(10, 2)
```

NUMERIC/DECIMAL são tipos de **ponto fixo** — armazenam o número exatamente como decimal.
10.000 transações/dia com erro de R$0.01 cada = R$36.500/ano de imprecisão acumulada.

📖 PostgreSQL: Up and Running — Obe & Hsu
Cap. 4: "pgAdmin and Other Administration Tools"
"For monetary values, never use float. Use numeric, which stores values as exact decimal numbers."

---

## Q19 — Naming (Código com Nomes Ruins)

**Resposta correta:**
```python
def p(u, a=0, d=0):
    u.a = a
    u.d = d
    return u
```

A função **faz algo**: seta atributos no objeto `u` e o retorna.
O problema é que nomes como `p`, `u`, `a`, `d` não comunicam nada.

Provavelmente é algo como:
```python
def patch_user(user, age=0, discount=0):
    user.age = age
    user.discount = discount
    return user
```

📖 Clean Code — Robert C. Martin
Cap. 2: "Meaningful Names"
"The name of a variable, function, or class, should answer all the big questions. It should tell you why it exists, what it does, and how it is used."

---

## Q20 — Segurança no Login

**Resposta correta:**
O problema **não é SQL injection** — o ORM com Pydantic protege isso.

O problema é **user enumeration**: mensagens de erro diferentes revelam se o email existe:
- "Usuário não encontrado" → o atacante sabe que aquele email NÃO está cadastrado
- "Senha incorreta" → o atacante sabe que aquele email **ESTÁ** cadastrado

Isso permite ataques direcionados: testar listas de emails até encontrar os que existem, depois fazer brute force de senha.

**Correto:**
```python
raise HTTPException(401, "Credenciais inválidas")  # sempre a mesma mensagem
```

📖 Web Application Security — Andrew Hoffman
Cap. 5: "Authentication and Authorization"
"Returning different error messages for invalid username vs invalid password leaks information about your user database."

---

## Q21 — Schema no FastAPI

**Resposta correta:**
No contexto FastAPI, schema = modelo Pydantic que define a estrutura dos dados.

- **Request schema:** define o que o cliente deve enviar no corpo da requisição
- **Response schema:** define o que o servidor retorna, e pode ocultar campos (como `password`)

```python
class UserCreate(BaseModel):   # request schema
    name: str
    email: str
    password: str

class UserResponse(BaseModel): # response schema
    id: int
    name: str
    email: str
    # password NÃO está aqui — não vaza para o cliente
```

Não é "esqueleto de banco de dados" — é contrato de API.

---

## Q22 — Investigar API Lenta

**Resposta correta (mínimo 2 passos concretos):**

1. **Medir onde está o tempo:** adicionar logging com timestamps antes e depois de operações principais (query, chamada externa, serialização)
2. **Checar as queries SQL:** ligar o log de queries lentas no PostgreSQL (`log_min_duration_statement`) ou usar SQLAlchemy com echo=True para ver o que está rodando
3. **Verificar se há N+1:** contar quantas queries são feitas por request — uma query que chama 100 sub-queries é N+1
4. **Checar índices ausentes:** EXPLAIN ANALYZE no PostgreSQL mostra se há full table scans

"Passar logs para IA" pode ser um passo depois, mas não substitui o entendimento de onde está o gargalo.

---

## Q23 — Idempotência

**Resposta correta:**
Uma operação é **idempotente** se executá-la N vezes tem o mesmo resultado que executá-la 1 vez.

- `GET` → **idempotente** — buscar o mesmo recurso múltiplas vezes não muda nada
- `DELETE` → **idempotente** — deletar um recurso 3 vezes tem o mesmo efeito que deletar 1 vez (após a primeira, o recurso não existe mais)
- `PUT` → **idempotente** — atualizar um recurso com os mesmos dados 3 vezes tem o mesmo efeito
- `POST` → **NÃO idempotente** — criar um recurso 3 vezes cria 3 recursos

Idempotência importa para retry logic: se um request falha e o cliente tenta de novo,
operações idempotentes são seguras de repetir.

📖 REST API Design Rulebook — Mark Massé
Cap. 4: "HTTP"
"Idempotent methods can be called multiple times with the same effect as calling them once."

---

## Q24 — Padrão REST de URIs

**Resposta correta:**
URIs nomeiam **recursos**, não ações. O verbo HTTP já diz a ação.

```
❌ Problema:
POST /users/create
GET  /users/getAll
PUT  /users/updateUser/42
DEL  /users/removeUser/42

✅ Correto:
POST   /users       ← criar usuário
GET    /users       ← listar todos
GET    /users/42    ← buscar um
PUT    /users/42    ← atualizar
DELETE /users/42    ← deletar
```

📖 REST API Design Rulebook — Mark Massé
Cap. 2: "Identifier Design with URIs"
"A URI should identify a resource, not an action. The HTTP method conveys the action."

---

## Q25 — Separação de Lógica de Negócio

**Resposta correta:**
Lógica de negócio no endpoint mistura responsabilidades: validação HTTP, regra de negócio,
e resposta HTTP ficam todas no mesmo lugar — difícil de testar, difícil de reutilizar.

```python
# Lógica isolada em um service
def create_user(data: UserCreate, db: Session) -> User:
    if db.query(User).filter(User.email == data.email).first():
        raise ValueError("Email já cadastrado")
    user = User(**data.dict())
    db.add(user)
    db.commit()
    return user

# Endpoint: só traduz HTTP → service → HTTP
@router.post("/users", response_model=UserResponse)
async def register_user(data: UserCreate, db: Session = Depends(get_db)):
    try:
        user = create_user(data, db)
        return user
    except ValueError as e:
        raise HTTPException(400, detail=str(e))
```

Benefício: `create_user` pode ser testado sem HTTP, reutilizado em outras rotas, ou chamado por tasks assíncronas.

📖 Architecture Patterns with Python — Percival & Gregory
Cap. 1: "Domain Modeling"
"The service layer separates the orchestration of use cases from the HTTP layer."
