# REST API Design Rulebook — Mark Massé (2012)

## Tese Central
REST não é só usar HTTP — é um conjunto de restrições arquiteturais que, quando aplicadas consistentemente, produzem APIs previsíveis, evolutivas e simples de usar. O livro formaliza essas restrições como regras concretas.

## Por que Este Livro Importa
A maioria das APIs chamadas de "REST" é apenas HTTP com JSON — não REST de verdade. APIs mal projetadas mudam de forma incompatível, têm URLs inconsistentes e forçam os clientes a ler documentação para adivinhar comportamento. Este livro evita esse problema com regras claras.

## Onde o Livro Pode Ser Questionado
- O livro é de 2012 — precede o crescimento de GraphQL e gRPC como alternativas
- Algumas regras são opinionadas e discutidas pela comunidade (versionamento por URL vs. header)
- Para o ZionHub: as regras são valiosas como default, mas podem ser adaptadas com intenção
- WRML (a proposta de formato do autor) não ganhou tração — pode ser ignorada

## Frases-Chave do Autor
> "A REST API must not define fixed resource names or hierarchies." — Cap. 2

> "REST APIs use a uniform interface, which constrains and decouples the architecture." — Cap. 1

## Mapa por Capítulo

### Cap. 1 — Introduction (p. 1–9)
**Conceito central:** REST como estilo arquitetural, não protocolo. As 6 restrições de REST: Client-Server, Stateless, Cache, Uniform Interface, Layered System, Code-On-Demand.
**Regras práticas:**
- **Stateless:** cada request deve conter toda a informação necessária — o servidor não mantém sessão do cliente
- **Uniform Interface:** todas as APIs REST devem ter o mesmo vocabulário (métodos HTTP, status codes, URIs)
**Conexão:** HTTP: The Definitive Guide (Gourley & Totty) — os mecanismos HTTP que REST usa.

### Cap. 2 — Identifier Design with URIs (p. 11–20)
**Conceito central:** URIs identificam recursos — devem ser previsíveis e expressivos.
**Regras (as mais importantes):**
- Use `/` para hierarquia: `/users/{user_id}/orders`
- Não inclua `/` no final: `/users/` ❌, `/users` ✅
- Use `-` para legibilidade: `/user-accounts` ✅, `/user_accounts` ❌
- Sem extensões de arquivo: `/users.json` ❌, `/users` + `Accept: application/json` ✅
- Letras minúsculas em paths: `/Users` ❌, `/users` ✅
- Sem nomes de funções CRUD: `/getUser` ❌, `/users/{id}` ✅
**Frase marcante:** "CRUD function names should not be used in URIs." — p. 18
**Onde questionar:** Algumas empresas usam `/api/v1/` no path para versionamento — Massé prefere headers, mas a convenção de URL é mais simples e mais comum na prática.
**Conexão:** Clean Code Fundamentals (Hock) — naming como comunicação.

### Cap. 3 — Interaction Design with HTTP (p. 21–43)
**Conceito central:** Métodos HTTP têm semântica específica — usá-los corretamente é comunicação com o cliente.
**Regras:**
- **GET:** recupera estado, idempotente, seguro (não muda estado)
- **POST:** cria recurso ou executa ação não-idempotente
- **PUT:** substitui recurso completo (idempotente)
- **PATCH:** atualiza parcialmente (semantica definida pelo body)
- **DELETE:** remove recurso (idempotente)
**Status codes por operação:**
| Operação | Sucesso | Erro cliente | Erro servidor |
|----------|---------|-------------|---------------|
| GET | 200 | 404, 400 | 500 |
| POST (create) | 201 + Location header | 400, 409 | 500 |
| PUT | 200 ou 204 | 404, 400 | 500 |
| DELETE | 204 | 404 | 500 |
**Frase marcante:** "GET must not change the state of a resource." — p. 22
**Conexão:** HTTP: The Definitive Guide (Gourley & Totty) — semântica detalhada dos métodos.

### Cap. 4 — Metadata Design (p. 45–59)
**Conceito central:** HTTP headers como canal de metadados — separados do body.
**Regras:**
- `Content-Type` no request com body
- `Location` no response de 201 Created (URL do recurso criado)
- `ETag` para cache validado
- Use media types customizados com parcimônia
**Conexão:** HTTP: The Definitive Guide (Gourley & Totty) — headers em detalhe.

### Cap. 5 — Representation Design (p. 61–79)
**Conceito central:** O body de uma resposta é uma *representação* do recurso — o mesmo recurso pode ter múltiplas representações (JSON, XML, HTML).
**Regras:**
- `application/json` como media type padrão para APIs modernas
- Consistência na estrutura de respostas — erros sempre têm o mesmo formato
- Não "leaze" state desnecessário — retorne apenas o que o cliente precisa

### Cap. 6 — Client Concerns (p. 81–98)
**Conceito central:** A experiência do cliente da API — versionamento, segurança, paginação.
**Regras:**
- Versionamento via URL path (`/v1/users`) ou via header `Accept: application/vnd.myapi.v1+json`
- Paginação com `?page=1&per_page=20` ou cursor-based
- CORS para APIs acessadas pelo browser

## Tensões com Outros Livros
- **vs. HTTP: The Definitive Guide:** HTTP é o protocolo; REST é o estilo. Massé constrói sobre Gourley.
- **vs. Clean Code Fundamentals:** Naming de recursos segue os mesmos princípios de naming de variáveis — expressivo, sem abreviações, sem funções no nome.
- **vs. Designing Data-Intensive Applications:** Kleppmann fala sobre o backend dos dados; Massé fala sobre a interface pública. Tensão: paginação cursor-based de Kleppmann é mais escalável que page-based de Massé.

## Aplicação em Python Moderno (FastAPI)
```python
# Cap. 2: URIs corretos em FastAPI
# ❌
@router.get("/getUser/{id}")         # CRUD no nome
@router.get("/users/")               # trailing slash
@router.get("/Users/{id}")           # maiúscula

# ✅
@router.get("/users/{user_id}")
@router.get("/users/{user_id}/orders")
@router.get("/user-accounts/{account_id}")

# Cap. 3: Status codes corretos
@router.post("/orders", status_code=201)
async def create_order(data: OrderCreate, response: Response):
    order = await order_service.create(data)
    response.headers["Location"] = f"/orders/{order.id}"
    return order

@router.delete("/orders/{order_id}", status_code=204)
async def delete_order(order_id: int):
    await order_service.delete(order_id)
    # 204 = sem body

# Cap. 5: Formato consistente de erros
class ErrorResponse(BaseModel):
    code: str
    message: str
    details: dict = {}

@app.exception_handler(HTTPException)
async def http_exception_handler(request, exc):
    return JSONResponse(
        status_code=exc.status_code,
        content=ErrorResponse(
            code=str(exc.status_code),
            message=exc.detail
        ).dict()
    )

# Cap. 6: Paginação
@router.get("/orders")
async def list_orders(
    page: int = Query(1, ge=1),
    per_page: int = Query(20, ge=1, le=100)
):
    ...
```
