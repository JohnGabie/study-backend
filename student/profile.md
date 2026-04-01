# Perfil do Aluno

## Avaliação Inicial
**Data:** 2026-04-01 — Prova geral baseline (25 questões, nível avançado)
**Score:** 4.7/25 (19%)

## Avaliação 2
**Data:** 2026-04-01 — Prova nível médio (25 questões)
**Score:** 8.5/25 (34%) — +15 p.p. vs baseline

## Avaliação 3
**Data:** 2026-04-01 — Prova nível fácil (25 questões)
**Score:** 15.4/25 (62%) — +28 p.p. vs baseline | Diagnóstico completo

Prova feita antes de qualquer sessão de estudo — representa o estado zero.
Relatório completo em `quizzes/prova_geral_2026-03-31_relatorio.md`.

## Pontos Fortes
- **Intuição de acoplamento:** consegue sentir que código está errado sem saber nomear (Q9, Q11, Q12, Q13 — Prova 1)
- **Honestidade sobre lacunas:** não inventa respostas — "não sei" é mais valioso que resposta errada com confiança
- **Raciocínio pragmático:** "olharia logs ou pediria ajuda de IA" (Q24 P1), "IA como ferramenta, não bengala" (Q19 P1)
- **Entende SRP conceitualmente** (Q1 P1) — mesmo que o exemplo seja fraco
- **HTTP semântico: nota máxima em Q11 (P2)** — identificou imediatamente que @router.get deveria ser @router.delete
- **Separação de lógica (Q25 P2):** "API é apenas access point. A lógica não deve estar na API" — chegou lá por raciocínio próprio, sem ter lido o livro
- **Estrutura relacional SQL (Q15 P2):** distinção PRIMARY KEY / FOREIGN KEY clara e correta

## Lacunas Identificadas

### 🟢 Base confirmada (Prova 3)
- **Estrutura relacional:** banco/tabela/coluna/linha — modelo mental correto, com exemplos espontâneos
- **Lógica de programação:** loops, condicionais — sólido
- **Conceito de API:** correto e expandido além de web (Windows, carros)
- **DNS:** sabe que existe e o que faz

### 🟡 Contaminação de outras linguagens
- **Tipos Python:** disse `double` e `char` — são tipos de Java/C. Python tem `float` e `str`. Precisa de correção ativa.

### 🔴 Críticas — conceito errado ativo
- **async/await (2026-04-01):** Acredita que `time.sleep` dentro de `async def` pausa só aquela função. Errado — trava o event loop inteiro. Causa bugs reais no FastAPI.
- **FLOAT para dinheiro (2026-04-01):** Acredita que o problema é "casas demais" e sugere DOUBLE como fix. Errado — o problema é imprecisão de ponto flutuante. DOUBLE também tem esse problema. Correto: NUMERIC/DECIMAL.
- **User enumeration (2026-04-01):** Ao ver código de auth com mensagens diferentes, diagnosticou "SQL injection". O ORM protege isso. O problema real é user enumeration — mensagens distintas ensinam ao atacante quais emails existem no sistema.
- **REST URIs (2026-04-01):** Reproduziu o antipadrão que a Q24 perguntou sobre. Ainda usa verbos CRUD na URI (create, getAll, updateUser). Precisa fixar: URI nomeia recurso, método HTTP nomeia ação.

### 🟠 Vocabulário ausente, intuição presente
- **N+1 problem:** descreveu o comportamento mas não sabe o nome nem a solução
- **IDOR:** sentiu que manipular `order_id` era um problema, mas não sabe o termo
- **Default mutável:** identificou sintoma mas não a causa raiz

### 🟡 Gaps de fundamento
- HTTP stateless, status codes, métodos HTTP
- DRY, TDD, 4 Golden Signals, Design by Contract
- SQL: tipos de dados (FLOAT para dinheiro), índices, timezone
- Estrutura de testes com pytest
- Nomes de URIs REST

## Padrões de Raciocínio
- Intuitivo: sente problemas antes de nomear — ponto de partida forte para code review
- Pragmático: resolve problemas, não decora teoria
- Honesto: não blefa quando não sabe

## Discordâncias e Posições Próprias
- (nenhuma registrada ainda)

## Histórico de Sessões

### 2026-04-01 — Setup do sistema + Baseline + Prova Média
- Configurado CLAUDE.md, books/ (16 MDs), memory/, quizzes/, student/
- Prova 1 (avançado): 4.7/25 — mapa de lacunas gerado
- Prova 2 (médio): 8.5/25 — evolução confirmada, novos gaps documentados
- Pendente: Prova 3 (fácil) para completar o mapa de diagnóstico
- Commit inicial feito, push para JohnGabie/study-backend
- Questão aberta não respondida: "O que acontece com os outros 99 requests se o primeiro bloqueia com time.sleep(3)?"

## Exemplos Salvos
- (nenhum ainda)
