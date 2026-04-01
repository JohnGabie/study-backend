# Learning SQL: Generate, Manipulate, and Retrieve Data — Alan Beaulieu (2020, 3ª ed.)

## Tese Central
SQL é uma linguagem declarativa — você descreve *o quê* quer, não *como* obter. Dominar SQL é dominar a forma mais eficiente de trabalhar com dados relacionais, e nenhum ORM substitui esse domínio.

## Por que Este Livro Importa
ORMs como SQLAlchemy abstraem SQL — mas essa abstração tem custo: queries ineficientes, N+1 problems, e perda de controle sobre o que acontece no banco. Entender SQL subjacente é o que separa quem usa ORM de quem usa ORM bem.

## Onde o Livro Pode Ser Questionado
- Exemplos usam MySQL — PostgreSQL tem diferenças em funções e comportamento (ex: `ILIKE`, arrays, jsonb)
- O livro não cobre ORMs, migrations, ou integração com Python
- Window functions e CTEs são introduzidas mas poderiam ter mais profundidade para uso moderno
- Não cobre indexação estratégica em profundidade — o foco é na linguagem, não em performance

## Frases-Chave do Autor
> "SQL is a nonprocedural language: you define what you want, not how to get it." — Cap. 1, p. 9

> "The query optimizer determines the most efficient means of executing your SQL statement." — Cap. 3

## Mapa por Capítulo

### Cap. 1 — A Little Background (p. 1–15)
**Conceito central:** O modelo relacional — tabelas, linhas, colunas, chaves. SQL como linguagem não-procedural.
**Regras práticas:** Entenda a diferença entre SQL procedural (stored procedures) e SQL declarativo (queries) — o poder está no declarativo.
**Frase marcante:** "SQL is a nonprocedural language." — p. 10
**Conexão:** Designing Data-Intensive Applications (Kleppmann) — Cap. 2 sobre modelos de dados e por que o relacional ganhou.

### Cap. 2 — Creating and Populating a Database (p. 17–41)
**Conceito central:** DDL (Data Definition Language) — `CREATE TABLE`, tipos de dados, constraints.
**Regras práticas:**
- Escolha tipos de dados corretos — `VARCHAR(255)` vs `TEXT`, `INTEGER` vs `BIGINT`
- Primary keys e foreign keys como contratos de integridade
- `NOT NULL`, `UNIQUE`, `CHECK` como proteções de qualidade dos dados
**Conexão:** PostgreSQL Up and Running (Obe & Hsu) — tipos de dados específicos do PostgreSQL (uuid, jsonb, arrays).

### Cap. 3 — Query Primer (p. 45–70)
**Conceito central:** A anatomia de um SELECT.
**Regras práticas:**
- Ordem de execução (não de escrita): FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
- `SELECT *` é um smell — seja explícito sobre as colunas que precisa
- Column aliases com `AS` para legibilidade
**Frase marcante:** O query optimizer pode executar sua query em qualquer ordem — o que você escreve é o *o quê*, não o *como*.

### Cap. 4 — Filtering (p. 71–94) — WHERE
**Conceito central:** Condições no WHERE — operadores de comparação, `BETWEEN`, `LIKE`, `IN`, `IS NULL`.
**Regras práticas:**
- `IS NULL` não `= NULL` — comparação com NULL sempre retorna NULL, não true/false
- `LIKE 'Jo%'` para prefixo, `LIKE '%ão'` para sufixo — `%` é wildcard de N chars
- `IN (1, 2, 3)` > múltiplos `OR` — mais legível e geralmente mais eficiente

### Cap. 5 — Querying Multiple Tables (p. 95–116) — JOINs
**Conceito central:** JOINs como operação fundamental do modelo relacional.
**Tipos de JOIN:**
- `INNER JOIN`: retorna apenas linhas com match em ambas as tabelas
- `LEFT OUTER JOIN`: retorna todas as linhas da tabela esquerda, NULL onde não há match
- `RIGHT OUTER JOIN`: inverso do LEFT
- `CROSS JOIN`: produto cartesiano — use com cuidado
**Frase marcante:** "The join condition ties the two tables together." — Cap. 5
**Onde questionar:** JOINs múltiplos em queries complexas podem ser substituídos por subqueries ou CTEs para legibilidade — depende do contexto.
**Conexão:** Designing Data-Intensive Applications (Kleppmann) — object-relational mismatch e o custo dos joins em ORMs.

### Cap. 6 — Working with Sets (p. 117–130) — UNION, INTERSECT, EXCEPT
**Conceito central:** Operações de conjunto em SQL — combinar resultados de múltiplas queries.
**Regras práticas:** `UNION ALL` > `UNION` quando duplicatas são aceitáveis (mais eficiente — não faz dedup).

### Cap. 7 — Data Generation, Manipulation, and Conversion (p. 131–162)
**Conceito central:** Funções de string, numéricas e de data em SQL.
**Regras práticas em PostgreSQL:** `LOWER()`, `UPPER()`, `TRIM()`, `SUBSTRING()`, `DATE_TRUNC()`, `NOW()`, `EXTRACT()`.

### Cap. 8 — Grouping and Aggregates (p. 163–186) — GROUP BY
**Conceito central:** Aggregation — transformar conjuntos de linhas em valores sumarizados.
**Regras práticas:**
- `COUNT(*)` conta linhas; `COUNT(column)` exclui NULLs
- `GROUP BY` agrupa; `HAVING` filtra grupos (como WHERE mas para grupos)
- Ordem: `GROUP BY` antes de `ORDER BY`

### Cap. 9 — Subqueries (p. 187–212)
**Conceito central:** Queries dentro de queries — para lógica que requer múltiplos passos.
**Regras práticas:** Subqueries correlacionadas (referencia tabela externa) podem ser lentas — candidate a CTE ou JOIN.

### Cap. 10 — Joins Revisited (p. 213–230)
Self-joins, non-equi joins. Casos especiais para hierarquias e comparações entre linhas da mesma tabela.

### Cap. 11 — Conditional Logic (p. 231–244) — CASE
**Conceito central:** `CASE WHEN ... THEN ... ELSE ... END` para lógica condicional dentro de queries.

### Cap. 12 — Transactions (p. 245–258)
**Conceito central:** ACID. `BEGIN`, `COMMIT`, `ROLLBACK`. Savepoints.
**Regras práticas:** Transações garantem que operações múltiplas acontecem atomicamente ou não acontecem.
**Conexão:** Designing Data-Intensive Applications (Kleppmann) — Cap. 7 sobre transações em profundidade.

### Cap. 13 — Indexes and Constraints (p. 259–278)
**Conceito central:** Indexes como estruturas auxiliares para acelerar leituras.
**Regras práticas:**
- Índices em foreign keys (automaticamente no PostgreSQL para PKs, mas não para FKs!)
- Índices em colunas usadas frequentemente em WHERE, JOIN ON, ORDER BY
- Índices têm custo em escrita — não indexe tudo
**Conexão:** PostgreSQL Up and Running (Obe & Hsu) — tipos de índices no PostgreSQL (B-tree, GiST, GIN).

### Cap. 15 — Metadata (p. 299–312)
`INFORMATION_SCHEMA` — como inspecionar a estrutura do banco programaticamente.

## Tensões com Outros Livros
- **vs. PostgreSQL Up and Running:** Beaulieu ensina SQL padrão; Obe & Hsu ensinam PostgreSQL específico. São complementares.
- **vs. Designing Data-Intensive Applications:** Beaulieu ensina o *como* do SQL; Kleppmann ensina o *porquê* do modelo relacional e seus trade-offs.

## Aplicação em Python Moderno (SQLAlchemy + PostgreSQL)
```python
# Cap. 5: JOIN em SQLAlchemy (entender o SQL gerado é crítico)
from sqlalchemy.orm import joinedload

# SQLAlchemy ORM gera LEFT OUTER JOIN
stmt = select(User).options(joinedload(User.orders))
users = await session.execute(stmt)

# SQL equivalente:
# SELECT users.*, orders.* FROM users
# LEFT OUTER JOIN orders ON users.id = orders.user_id

# Cap. 8: Aggregates em SQLAlchemy
from sqlalchemy import func

stmt = (
    select(Order.user_id, func.count(Order.id).label("total_orders"))
    .group_by(Order.user_id)
    .having(func.count(Order.id) > 5)
)

# Cap. 13: Índices em SQLAlchemy models
class Order(Base):
    __tablename__ = "orders"
    id: Mapped[int] = mapped_column(primary_key=True)
    user_id: Mapped[int] = mapped_column(ForeignKey("users.id"), index=True)  # índice na FK!
    created_at: Mapped[datetime] = mapped_column(index=True)  # índice para queries por data
```
