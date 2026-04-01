# Relatório de Correção — Prova 2 (Médio)
**Data:** 2026-04-01
**Score:** 8.5 / 25 (34%)
**Anterior (Prova 1):** 4.7/25 (19%)
**Evolução:** +3.8 pontos (+15 p.p.)

---

## Pontuação por Questão

| Q | Tema | Score | Observação |
|---|------|-------|------------|
| 1 | List Comprehension | 0.3 | Entendeu o que o código faz. Não sabe o conceito nem como reescrever |
| 2 | Append vs Extend | 0.2 | Append correto. Extend: "remove coisas" — completamente errado |
| 3 | *args / **kwargs | 0 | Não sabe |
| 4 | Default Argument | 0.4 | Identificou o problema de tipo, mas diagnosis incompleto — a questão é unidade (10 vs 0.1) |
| 5 | try/except/else/finally | 0.5 | Acertou o except e intuiu o else. Não mencionou que finally roda sempre |
| 6 | == vs is | 0 | "Deve ser a mesma coisa" — errado. Nunca viu o operador `is` |
| 7 | Status Codes | 0.7 | 4 de 5 corretos. Erro: 402 ≠ "não autenticado" (401 é 401, 402 é Payment Required) |
| 8 | GET vs POST | 0.6 | Distinção correta. Não elaborou quando usar cada um |
| 9 | Path Parameters | 0.4 | Entendeu que retorna o ID. Não viu que FastAPI converte string→int automaticamente nem o formato do JSON |
| 10 | Pydantic | 0.2 | "Tipos de dados" — vago. Não viu que "vinte" causaria erro 422, não 200 |
| 11 | Semântica HTTP | 1.0 | **Perfeito.** "deveria ser @router.delete" — exatamente o problema |
| 12 | Depends | 0.1 | "É alguma configuração" — demasiado vago |
| 13 | HTTPException vs return | 0.5 | Direção certa ("trata o erro com clareza"). Não viu que return retorna HTTP 200 com corpo de erro |
| 14 | JOIN | 0.3 | Sabe que existe, tem noção de sintaxe. Não escreveu a query completa |
| 15 | PRIMARY KEY / FOREIGN KEY | 0.8 | Distinção essencialmente correta e clara |
| 16 | GROUP BY / HAVING | 0.6 | Entendeu o agrupamento e a ordenação. Não mencionou o HAVING COUNT(*) > 3 |
| 17 | Índice | 0.3 | Confundiu índice com ordenação. Índice é sobre velocidade de busca, não sobre ordenação |
| 18 | FLOAT para dinheiro | 0.1 | **Diagnóstico errado.** "Poderia ser double" — double ainda tem imprecisão de ponto flutuante. Correto: NUMERIC/DECIMAL |
| 19 | Naming | 0.2 | "Não faz absolutamente nada" — errado. Faz algo (seta atributos). Perdeu completamente o problema de nomenclatura |
| 20 | Auth Security | 0.1 | "Poderia fazer injeção SQL" — diagnóstico errado. Problema real: mensagens diferentes revelam se email existe (user enumeration) |
| 21 | Schema Request/Response | 0.3 | "Esqueleto de banco de dados" — errado no contexto FastAPI. Mas direcionou GET/POST certo |
| 22 | API Lenta | 0.2 | "Logs + IA" — pragmático mas sem método. Não mencionou profiling, queries lentas, N+1 |
| 23 | Idempotência | 0 | Não sabe |
| 24 | Padrão REST de URIs | 0 | **Reproduziu o mesmo antipadrão.** Sugeriu /users/create em vez de POST /users |
| 25 | Separação de Lógica | 0.7 | "API é apenas access point, lógica não deve estar na API" — correto e bem articulado |

**Total: 8.5/25 (34%)**

---

## O que os dados revelam

### Evolução real desde a Prova 1

De 19% para 34% — quase o dobro. E isso antes de qualquer sessão de estudo.
Isso é calibragem, não decoreba. O aluno já tinha a intuição; a prova anterior
estava nivelada para outro patamar.

**O que cresceu:**
- HTTP semântico (Q7, Q8, Q11, Q13) — onde a intuição já existia, a verbalização melhorou
- SQL relacional (Q15, Q16) — estrutura de dados tem lógica que o aluno segue bem
- Separação de responsabilidades (Q25) — posição própria clara e correta

---

### Pontos fortes revelados

**Q11 — Nota máxima.**
"@router.get tá errado, deveria ser @router.delete."
Isso é exatamente o problema. Sem hesitação, sem enrolação.
Na Prova 1 (Q17), o aluno não sabia que URIs não deveriam ter CRUD no nome.
Agora, ao ver o método errado, corrigiu de imediato.
Isso é progresso real — não de vocabulário, de julgamento.

**Q25 — Posição de engenheiro.**
"A API é apenas o access point. A lógica não deve estar na API."
Essa é a tese do Architecture Patterns with Python inteira, dita com as próprias palavras.
O aluno não leu o livro. Chegou lá por raciocínio.

**Q15 — Clareza estrutural.**
"PRIMARY KEY é chave local da tabela, FOREIGN KEY é chave de outra tabela."
Simples, direto, correto. Quem entende relação entre tabelas não vai errar JOIN por muito tempo.

---

### Lacunas que persistem

**🔴 Críticas — conceito errado ativo**

**Q18 — FLOAT para dinheiro:**
"Poderia ser double."
Não. `DOUBLE` é `FLOAT` de precisão dupla — ainda usa aritmética de ponto flutuante.
O problema não é "casas demais". É que `0.1 + 0.2` em float não é `0.3`.
Em banco de dados financeiro, isso acumula erro invisível por anos.
A resposta correta é `NUMERIC(10,2)` ou `DECIMAL` — tipos de ponto fixo.

**Q20 — Diagnóstico de segurança:**
O reflexo foi "injeção SQL". Mas não há concatenação de string aqui — é ORM.
O problema real é outro: mensagens de erro diferentes para email não encontrado
vs senha errada ensinam ao atacante que aquele email existe no sistema.
Isso se chama **user enumeration** — um vetor de ataque real usado em ataques direcionados.
O correto é sempre retornar a mesma mensagem genérica (ex: "Credenciais inválidas").

**Q24 — REST URIs:**
O aluno reproduziu o antipadrão que a questão perguntou sobre.
Sugeriu `CREATE /users/create`, `GET /users/getAll`.
A convenção REST é: o método HTTP já diz a ação. A URI nomeia o recurso.
```
POST   /users          ← criar
GET    /users          ← listar todos
GET    /users/42       ← buscar um
PUT    /users/42       ← atualizar
DELETE /users/42       ← deletar
```

---

**🟠 Vocabulário ausente, intuição presente**

- **List comprehension:** sabe o que o código faz, não sabe o construto
- **Pydantic validation:** sabe que "faz algo com tipos", não sabe que rejeita com 422
- **Depends:** sente que "é configuração de algo", não vê o padrão de injeção de dependência
- **Idempotência:** conceito completamente novo — zero intuição ainda

---

**🟡 Gaps de fundamento confirmados**

- `*args` / `**kwargs` — nunca viu isso com clareza
- `==` vs `is` — nunca viu `is` como operador distinto
- Índice em banco — confunde com ordenação (ORDER BY)
- Schema no contexto FastAPI — ainda pensa como "esqueleto de banco de dados"

---

## Prioridade de Estudo Atualizada

| Ordem | Tema | Por quê agora |
|-------|------|---------------|
| 1 | **NUMERIC vs FLOAT para dinheiro** | Conceito errado ativo — vai criar bugs silenciosos |
| 2 | **List comprehension + *args/kwargs** | Fundamentos Python ausentes que aparecem em todo código |
| 3 | **Pydantic: validação e 422** | FastAPI depende disso — o aluno tem 50% da intuição |
| 4 | **Depends: injeção de dependência** | Padrão central do FastAPI, zero clareza ainda |
| 5 | **User enumeration em auth** | Erro de segurança que o aluno não vê — diagnóstico trocado |
| 6 | **Idempotência** | Zero — conceito necessário para entender GET/PUT/DELETE |
| 7 | **REST URIs sem verbo** | Reproduziu o antipadrão — precisa de fixação |

---

## Uma pergunta antes de fechar

Q18 revelou algo importante sobre como você raciocina sobre tipos de dados.

Você disse "poderia ser double — teria mais casas decimais."

Aqui vai uma situação real: `0.1 + 0.2` em Python retorna `0.30000000000000004`.

Por que você acha que isso acontece? E se esse erro aparecer em 10.000 transações financeiras por dia, o que acontece ao longo de um ano?
