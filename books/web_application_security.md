# Web Application Security: Exploitation and Countermeasures for Modern Web Applications — Andrew Hoffman (2020)

## Tese Central
Para defender uma aplicação, você precisa pensar como um atacante. Entender *como* vulnerabilidades são exploradas é o único caminho para escrever código que as previne — não como uma checklist, mas como intuição.

## Por que Este Livro Importa
O ZionHub é uma aplicação web com dados reais de usuários. XSS, CSRF, SQL Injection, IDOR — estas não são abstrações acadêmicas, são ataques que acontecem todo dia. Este livro torna o desenvolvedor capaz de identificar e corrigir essas vulnerabilidades antes do atacante encontrá-las.

## Onde o Livro Pode Ser Questionado
- Foco em aplicações JavaScript/frontend — a perspectiva de backend Python exige complementação
- Alguns exploits dependem de configurações antigas (ex: vulnerabilidades IE-specific)
- O livro cobre recon e exploração — para o ZionHub, o foco deve ser em defesa
- Não cobre autenticação moderna (JWT, OAuth2) com a profundidade que um backend exige

## Frases-Chave do Autor
> "To build secure software, you must think like an attacker." — Prefácio

> "The best security comes from understanding what you're defending against." — Cap. 1

## Mapa por Capítulo

### Cap. 1 — The History of Software Security (p. 1–18)
**Conceito central:** Evolução das ameaças — de phreaking telefônico a exploits modernos.
**Por que importa:** Entender o histórico explica por que certas vulnerabilidades persistem — são padrões repetidos.

### Part I — Recon

#### Cap. 2 — Web Application Reconnaissance (p. 21–25)
**Conceito central:** Como um atacante mapeia uma aplicação antes de atacar.
**Para o ZionHub:** Informações que sua API expõe (stack traces, versões, error messages detalhadas) são dados de recon para um atacante.
**Regras práticas (defesa):**
- Nunca exponha stack traces em respostas de erro em produção
- `X-Powered-By` header revela tecnologia — remova-o
- Mensagens de erro de autenticação não devem indicar se o usuário existe ("user not found" vs. "invalid credentials")

#### Cap. 3 — Structure of a Modern Web Application (p. 27–51)
**Conceito central:** REST APIs, JavaScript, autenticação, bancos de dados — como as peças se conectam.
**Para o ZionHub:** FastAPI com PostgreSQL. Os vetores de ataque são os mesmos independente do stack.

### Part II — Offense

#### Cap. 6 — Cross-Site Scripting (XSS) (p. ≈90)
**Conceito central:** XSS — injeção de JavaScript malicioso que executa no browser da vítima.
**Tipos:**
- **Stored XSS:** payload armazenado no banco e servido a outros usuários
- **Reflected XSS:** payload retornado imediatamente pela resposta
- **DOM-based XSS:** payload manipula o DOM client-side
**Defesa em FastAPI:**
- APIs JSON raramente são vetores diretos de XSS (o browser não interpreta JSON como HTML)
- Se a API retorna HTML, use template engines que escapam automaticamente (Jinja2)
- Headers de segurança: `Content-Security-Policy`, `X-Content-Type-Options: nosniff`
**Conexão:** HTTP: The Definitive Guide (Gourley & Totty) — headers de segurança.

#### Cap. 7 — Cross-Site Request Forgery (CSRF) (p. ≈110)
**Conceito central:** CSRF — força um usuário autenticado a executar ações sem saber.
**Como funciona:** Usuário autenticado em `bank.com` visita `evil.com` que contém um form que submete para `bank.com/transfer`. O browser inclui os cookies automaticamente.
**Defesa:**
- APIs que usam JWT no header `Authorization` não são vulneráveis a CSRF (atacante não tem acesso ao token)
- Se usar cookies de sessão: `SameSite=Strict` ou `SameSite=Lax` no cookie
- CSRF token para forms HTML tradicionais
**Para o ZionHub:** Se usar JWT no Authorization header (padrão FastAPI), CSRF não é um vetor principal.

#### Cap. 9 — SQL Injection (p. ≈145)
**Conceito central:** Injeção de SQL — payload que modifica a query SQL intencionada.
**Como funciona:**
```sql
-- Query vulnerável:
SELECT * FROM users WHERE email = '{input}' AND password = '{password}'

-- Input malicioso: ' OR '1'='1
-- Query resultante: WHERE email = '' OR '1'='1' AND password = '...'
-- Resultado: retorna todos os usuários!
```
**Defesa com SQLAlchemy (parameterized queries):**
```python
# ✅ SQLAlchemy usa parâmetros automaticamente — nunca é vulnerável
stmt = select(User).where(User.email == email)  # email é parâmetro, não string interpolada

# ❌ raw SQL com interpolação é vulnerável
stmt = text(f"SELECT * FROM users WHERE email = '{email}'")  # NUNCA faça isso!

# ✅ raw SQL com parâmetros é seguro
stmt = text("SELECT * FROM users WHERE email = :email")
result = await session.execute(stmt, {"email": email})
```
**Frase marcante:** O ORM protege você automaticamente — mas SQL raw sem parâmetros desfaz essa proteção.
**Conexão:** Learning SQL (Beaulieu) — entender SQL é entender o que o atacante está injetando.

#### Cap. 10 — Broken Access Control / IDOR (p. ≈165)
**Conceito central:** IDOR (Insecure Direct Object Reference) — acesso a recursos de outros usuários por manipulação de IDs.
**Como funciona:**
```
GET /orders/123  → retorna pedido do usuário A
GET /orders/124  → também retorna pedido do usuário A? Ou do usuário B?
```
**Defesa em FastAPI:**
```python
@router.get("/orders/{order_id}")
async def get_order(order_id: int, current_user: User = Depends(get_current_user)):
    order = await order_service.get(order_id)
    if not order:
        raise HTTPException(404)
    if order.user_id != current_user.id:  # SEMPRE verifique autorização!
        raise HTTPException(403, "Forbidden")
    return order
```
**Frase marcante:** Authentication é "quem você é"; Authorization é "o que você pode fazer". São problemas separados.

#### Cap. 11 — XXE e SSRF (p. ≈185)
**SSRF (Server-Side Request Forgery):** O servidor faz requests para URLs controladas pelo atacante — pode vazar metadados cloud (AWS metadata endpoint).
**Defesa:** Não faça requests a URLs fornecidas pelo usuário sem validação. Whitelist de domínios permitidos.

#### Cap. 13 — Authentication Bypass (p. ≈210)
**Conceito central:** Fraquezas em autenticação — senhas fracas, tokens previsíveis, JWT mal configurado.
**Defesa em FastAPI com JWT:**
- Use uma secret key longa e aleatória: `openssl rand -hex 32`
- Defina expiração curta: `exp` claim com 15-60 minutos para access tokens
- Nunca coloque informações sensíveis no payload JWT (é base64, não criptografado)
- Use `HS256` ou `RS256` — nunca `alg: none`

### Part III — Defense

#### Cap. 20 — Secure Coding Practices (p. ≈310)
**Conceito central:** Práticas de código seguro por design.
**Regras práticas para FastAPI:**
1. Input validation via Pydantic (obrigatório em todos os endpoints)
2. Parametrized queries via SQLAlchemy ORM
3. Authentication via JWT com expiração
4. Authorization checks em todos os endpoints que retornam dados de usuários
5. Rate limiting para prevenir brute force
6. Security headers via middleware
7. HTTPS em produção
8. Logs sem dados sensíveis (passwords, tokens, PII)

## Tensões com Outros Livros
- **vs. HTTP: The Definitive Guide:** HTTP explica o protocolo; Hoffman explica como cada aspecto do protocolo pode ser explorado.
- **vs. Site Reliability Engineering:** SRE foca em reliability; Segurança foca em integridade e confidencialidade. Ambos são requisitos de produção.
- **vs. REST API Design Rulebook:** Massé design APIs para usabilidade; Hoffman design APIs para segurança. Às vezes conflitam — mensagens de erro informativas vs. não vazar informação.

## Aplicação em Python Moderno (FastAPI)
```python
# Middleware de segurança — headers obrigatórios
from starlette.middleware.base import BaseHTTPMiddleware

class SecurityHeadersMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request, call_next):
        response = await call_next(request)
        response.headers["X-Content-Type-Options"] = "nosniff"
        response.headers["X-Frame-Options"] = "DENY"
        response.headers["Strict-Transport-Security"] = "max-age=31536000"
        response.headers["Content-Security-Policy"] = "default-src 'self'"
        return response

app.add_middleware(SecurityHeadersMiddleware)

# Rate limiting com slowapi
from slowapi import Limiter
from slowapi.util import get_remote_address

limiter = Limiter(key_func=get_remote_address)

@router.post("/auth/login")
@limiter.limit("5/minute")  # previne brute force
async def login(credentials: LoginRequest, request: Request):
    ...

# Não vazar informação em erros de auth
async def authenticate_user(email: str, password: str) -> User:
    user = await get_user_by_email(email)
    if not user or not verify_password(password, user.hashed_password):
        raise HTTPException(401, "Invalid credentials")  # não "user not found"!
    return user
```
