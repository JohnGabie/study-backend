# HTTP: The Definitive Guide — David Gourley & Brian Totty (2002)

## Tese Central
HTTP é o protocolo universal da web — e a maioria dos desenvolvedores o usa sem entendê-lo. Entender HTTP em profundidade é entender como toda aplicação web funciona no nível fundamental.

## Por que Este Livro Importa
Toda API REST, todo endpoint FastAPI, todo cache, todo problema de performance de rede — tudo passa pelo HTTP. Este livro é a referência completa do protocolo. Sem ele, você debugga problemas de rede no escuro.

## Onde o Livro Pode Ser Questionado
- O livro é de 2002 — não cobre HTTP/2 nem HTTP/3
- Exemplos de código são antigos e em linguagens diversas
- Para o ZionHub: os conceitos são eternos, mas detalhes de implementação podem estar desatualizados
- A profundidade (caching, autenticação, proxies) pode ser over-engineering para APIs simples

## Frases-Chave do Autor
> "HTTP is a stateless request/response protocol." — Cap. 1

> "Every HTTP transaction has a request message and a response message." — Cap. 3

## Mapa por Capítulo

### Part I — HTTP: The Web's Foundation

#### Cap. 1 — Overview of HTTP (p. 3–21)
**Conceito central:** HTTP como protocolo de transporte de hipermídia. Clientes, servidores, recursos, URIs.
**Regras práticas:**
- HTTP é **stateless** — cada request é independente. State deve ser gerenciado pela aplicação (sessions, tokens).
- Request/Response: cliente envia request → servidor envia response. Simples e poderoso.
- **Métodos HTTP:** GET (fetch), POST (create), PUT/PATCH (update), DELETE (remove), HEAD (metadata only)
**Frase marcante:** "HTTP is a stateless, application-level protocol for distributed, collaborative, hypermedia information systems."
**Conexão:** REST API Design Rulebook (Massé) — aplica HTTP semântico para design de APIs.

#### Cap. 2 — URLs and Resources (p. 23–41)
**Conceito central:** URL como endereço único de um recurso.
**Regras práticas:** Estrutura de URL: `scheme://host:port/path?query#fragment`. Em APIs REST: path identifica o recurso; query parametriza a busca; fragment é client-side.
**Conexão:** REST API Design Rulebook (Massé) — regras de design de URIs.

#### Cap. 3 — HTTP Messages (p. 43–73)
**Conceito central:** A anatomia de um request e de um response.
**Request:** `Method URI Version\r\n Headers\r\n\r\n Body`
**Response:** `Version Status-Code Reason\r\n Headers\r\n\r\n Body`
**Headers mais importantes:**
- `Content-Type`: tipo do body (ex: `application/json`)
- `Authorization`: credenciais do cliente
- `Accept`: tipos que o cliente aceita
- `Cache-Control`: diretivas de cache
- `X-Request-Id`: para tracing
**Status codes essenciais:**
- 2xx: sucesso (200 OK, 201 Created, 204 No Content)
- 3xx: redirecionamento (301 Moved Permanently, 302 Found)
- 4xx: erro do cliente (400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 422 Unprocessable Entity, 429 Too Many Requests)
- 5xx: erro do servidor (500 Internal Server Error, 503 Service Unavailable)
**Conexão:** REST API Design Rulebook (Massé) — status codes corretos por tipo de operação.

#### Cap. 4 — Connection Management (p. 74–104)
**Conceito central:** Como TCP e HTTP se relacionam. Conexões persistentes.
**Regras práticas:**
- HTTP/1.0: uma conexão TCP por request (lento)
- HTTP/1.1: conexões persistentes por padrão (`Connection: keep-alive`)
- HTTP/2: multiplexing — múltiplos requests em uma única conexão TCP
**Onde questionar:** FastAPI com uvicorn usa HTTP/1.1 por padrão — HTTP/2 requer configuração adicional.

### Part II — HTTP Architecture

#### Cap. 6 — Proxies (p. 135–166)
**Conceito central:** Proxies como intermediários entre cliente e servidor.
**Regras práticas:** Headers `X-Forwarded-For` e `X-Real-IP` para identificar o IP original do cliente por trás de proxies (Nginx, load balancers).
**Conexão:** Site Reliability Engineering (Google) — proxies e load balancers em produção.

#### Cap. 7 — Caching (p. 167–222)
**Conceito central:** Cache como a otimização de performance mais impactante em HTTP.
**Regras práticas:**
- `Cache-Control: max-age=3600` — recurso pode ser cacheado por 1 hora
- `Cache-Control: no-store` — nunca cachear (dados sensíveis)
- `ETag` + `If-None-Match` para cache validado (304 Not Modified)
- `Last-Modified` + `If-Modified-Since` — alternativa ao ETag
**Onde questionar:** Para APIs autenticadas, cache é mais complexo — o mesmo URL pode retornar dados diferentes para usuários diferentes.

#### Cap. 11 — Client Identification and Cookies (p. 289–316)
**Conceito central:** Como o servidor identifica o cliente em um protocolo stateless.
**Regras práticas:** Cookies para sessions. `HttpOnly` para prevenir XSS. `Secure` para HTTPS only. `SameSite=Strict` para prevenir CSRF.
**Conexão:** Web Application Security (Hoffman) — XSS e CSRF exploram exatamente estas configurações.

#### Cap. 12 — Basic Authentication (p. 317–340)
**Conceito central:** O mecanismo de autenticação HTTP mais simples — e seus problemas.
**Regras práticas:** Basic Auth envia credentials em base64 (não é criptografia!). Sempre use HTTPS com Basic Auth. Para APIs modernas, prefira JWT ou OAuth2.
**Conexão:** Web Application Security (Hoffman) — autenticação em APIs modernas.

#### Cap. 14 — Secure HTTP (HTTPS) (p. 371–416)
**Conceito central:** SSL/TLS como camada de segurança sobre HTTP.
**Regras práticas:** Em produção, HTTPS é obrigatório. Certificates, handshake, cipher suites.
**Conexão:** Web Application Security (Hoffman) — HTTPS é a base de qualquer postura de segurança.

## Tensões com Outros Livros
- **vs. REST API Design Rulebook (Massé):** HTTP é o protocolo; REST é o estilo arquitetural construído sobre HTTP. Massé pressupõe que você entende HTTP.
- **vs. Web Application Security (Hoffman):** Segurança de aplicação web começa com entender HTTP — cookies, headers, HTTPS.
- **vs. Site Reliability Engineering:** SRE monitora saúde de serviços HTTP — entender status codes, latência, e erros é fundamental.

## Aplicação em Python Moderno (FastAPI)
```python
# Status codes corretos em FastAPI
from fastapi import HTTPException
from fastapi.responses import JSONResponse

@router.post("/orders", status_code=201)  # 201 Created
async def create_order(data: OrderCreate):
    order = await order_service.create(data)
    return order

@router.get("/orders/{order_id}")
async def get_order(order_id: int):
    order = await order_service.get(order_id)
    if not order:
        raise HTTPException(status_code=404, detail="Order not found")
    return order

# Headers de resposta para cache
@router.get("/public/config")
async def get_public_config():
    return JSONResponse(
        content={"theme": "dark"},
        headers={"Cache-Control": "max-age=3600, public"}
    )

# Headers para segurança
@app.middleware("http")
async def add_security_headers(request, call_next):
    response = await call_next(request)
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["X-Frame-Options"] = "DENY"
    return response
```
