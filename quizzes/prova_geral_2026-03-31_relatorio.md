# Relatório de Correção — Prova Geral
**Data:** 2026-04-01
**Score:** 4.7 / 25 (~19%)

---

## Pontuação por Questão

| Q | Tema | Score | Observação |
|---|------|-------|------------|
| 1 | SRP | 0.7 | Conceito correto. Exemplo fraco (login vs register é separação de rotas, não SRP) |
| 2 | async/await | 0.2 | **Conceito errado crítico** — `time.sleep` trava o event loop inteiro, não só aquela função |
| 3 | DRY | 0 | Não sabe |
| 4 | 4 Golden Signals | 0 | Não sabe |
| 5 | TDD | 0 | Não sabe |
| 6 | Python Data Model | 0.4 | Intuição certa (métodos especiais), mas chamou de "construtor" |
| 7 | SQL Injection vs IDOR | 0.2 | SQLAlchemy protege via abstração — correto. Não sabe o que é IDOR |
| 8 | HTTP stateless | 0 | Não sabe |
| 9 | Code review endpoint | 0.5 | Identificou acoplamento. Não viu: async bloqueante, `dict` sem Pydantic, return em vez de raise |
| 10 | Default mutável | 0.5 | Identificou o sintoma ("cresce a cada chamada"). Não sabe a causa raiz |
| 11 | IDOR no endpoint | 0.3 | Sentiu que havia problema de manipulação de ID — intuição certa sem vocabulário |
| 12 | Teste mal estruturado | 0.6 | Identificou corretamente: testa muitas coisas de uma vez |
| 13 | N+1 problem | 0.3 | Descreveu o comportamento corretamente. Não sabe que é N+1 nem como corrigir |
| 14 | Return None | 0 | Não conhece o livro |
| 15 | Schema com problemas | 0.1 | Não identificou nenhum dos problemas reais (FLOAT, timezone, FK, índice) |
| 16 | Fixture com scope errado | 0 | Não sabe |
| 17 | URIs com CRUD no nome | 0 | Não sabe |
| 18 | Stack trace exposto | 0 | Não sabe |
| 19 | Refatorar sem testes | 0.2 | "IA como ferramenta, não bengala" — insight genuíno. Não sabe o processo |
| 20 | Tensão de comentários | 0 | Não conhece os livros |
| 21 | Endpoint completo | 0.3 | Direção certa (user_id, comparar IDs). Muito vago |
| 22 | Headlights vs Tracer Bullets | 0.3 | Não conhece os termos mas a intuição foi na direção certa |
| 23 | gather + semaphore | 0 | Não sabe |
| 24 | API lenta — investigação | 0.1 | "Olharia logs" é pragmático mas sem método |
| 25 | Auth security review | 0 | Não sabe — quer chegar lá |

---

## O que os dados revelam

### Pontos fortes (o que existe hoje)

**1. Intuição de acoplamento**
Q9, Q11, Q12, Q13 — sem saber o nome, você sentiu que havia algo errado.
Isso não é trivial. Muita gente que sabe o vocabulário não consegue sentir o problema no código.

**2. Honestidade sobre lacunas**
Você não inventou respostas. "Não sei" 12 vezes é mais valioso do que 12 respostas erradas com confiança. Revela onde o mapa está em branco — e mapa em branco você pode preencher.

**3. Raciocínio pragmático**
Q24: "olharia logs ou pediria ajuda de IA" — é uma resposta de alguém que resolve problemas, não que decora teoria.
Q19: "usar IA como ferramenta, não bengala" — isso é uma posição de engenheiro, não de estudante passivo.

---

### Lacunas críticas (ordenadas por urgência)

**🔴 URGENTE — Conceito errado que vai causar bugs reais**

**async/await (Q2):** Você acredita que `time.sleep` dentro de `async def` só pausa aquela função. Está errado. Ele trava o event loop inteiro — nenhum outro request é atendido enquanto dorme. No FastAPI, isso derruba a performance da API para todos os usuários. Esse é o bug mais perigoso de quem começa com async.

---

**🟠 IMPORTANTE — Vocabulário que você não tem mas intuição que você tem**

Você consegue *sentir* esses problemas mas não consegue *nomeá-los* nem *corrigi-los*:

- **N+1 problem** (Q13): você descreveu o comportamento corretamente. Não sabe que tem nome, nem que a solução é um JOIN ou eager loading.
- **IDOR** (Q11): você sentiu que manipular `order_id` era um problema. IDOR é exatamente isso — acessar recursos de outros usuários por manipulação de ID.
- **Default mutável** (Q10): identificou o sintoma mas não a causa raiz (objeto compartilhado entre chamadas).

Esses três são treináveis rapidamente porque a intuição já existe.

---

**🟡 GAPS DE FUNDAMENTO — Não sabe e não tem intuição ainda**

- **HTTP stateless** (Q8): fundação de toda API REST
- **Status codes corretos** (Q17, Q18): você não sabe que `return {"error": ...}` retorna 200 com corpo de erro
- **SQL: tipos de dados** (Q15): FLOAT para dinheiro é um bug silencioso grave
- **TDD / estrutura de testes** (Q5, Q12, Q16): sabe que o teste estava confuso mas não sabe como estruturar certo
- **DRY, Golden Signals, Design by Contract** (Q3, Q4, Q14): vocabulário técnico ausente

---

## Prioridade de estudo baseada nos dados

| Ordem | Tema | Por quê primeiro |
|-------|------|-----------------|
| 1 | **async/await correto** | Conceito errado ativo que causa bugs reais hoje |
| 2 | **HTTP: stateless, status codes, métodos** | Fundação de tudo no FastAPI |
| 3 | **N+1, IDOR, default mutável** | Intuição já existe, só precisa de vocabulário e solução |
| 4 | **Estrutura de testes com pytest** | Qualquer código que escrever vai precisar disso |
| 5 | **SQL: tipos de dados, índices** | PostgreSQL é o banco do ZionHub |

---

## Uma pergunta antes de continuar

Q2 revelou algo importante: você acredita que `time.sleep(3)` dentro de `async def` só pausa aquela função.

Antes de eu te explicar — o que você acha que acontece com o servidor nesse tempo? Se 100 usuários fazem requests simultâneos e o primeiro trava com `time.sleep(3)`, o que acontece com os outros 99?
