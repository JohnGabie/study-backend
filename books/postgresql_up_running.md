# PostgreSQL: Up and Running — Regina O. Obe & Leo S. Hsu (2018, 3ª ed.)

## Tese Central
PostgreSQL não é apenas "outro banco SQL" — é o banco open-source mais avançado disponível. Suas extensões, tipos de dados nativos (jsonb, arrays, uuid, geometria), e capacidades de indexação resolvem problemas que outros bancos exigem ferramentas externas para tratar.

## Por que Este Livro Importa
O ZionHub usa PostgreSQL. Entender os recursos específicos do PostgreSQL (e não apenas SQL genérico) é o que permite escrever queries eficientes, modelar dados corretamente, e aproveitar features que eliminam a necessidade de infraestrutura adicional.

## Onde o Livro Pode Ser Questionado
- Foco nas versões 9.5, 9.6 e 10 — PostgreSQL está na versão 16+ em 2024, com features novas significativas
- Não cobre ORMs nem integração com Python/SQLAlchemy — é sobre o banco, não sobre a aplicação
- Alguns conselhos de performance precisam de validação com `EXPLAIN ANALYZE` no contexto específico
- PostGIS (extensão espacial) tem um capítulo — irrelevante para a maioria dos projetos

## Frases-Chave do Autor
> "PostgreSQL bills itself as the world's most advanced open source database. We couldn't agree more." — Preface

> "SQL is much like chess—a few hours to learn, a lifetime to master." — Preface

## Mapa por Capítulo

### Cap. 1 — The Basics (estrutura inicial)
**Conceito central:** Arquitetura do PostgreSQL. Clusters, databases, schemas, tables.
**Regras práticas:**
- Um cluster PostgreSQL pode ter múltiplos databases
- Schemas organizam objetos dentro de um database (`public` é o default)
- Em projetos multi-tenant, considere schemas separados por tenant

### Cap. 2 — Database Administration
**Conceito central:** Roles (usuários), permissões, configuração do servidor.
**Regras práticas:**
- Princípio do menor privilégio: a aplicação não deve conectar como `postgres` (superuser)
- `GRANT SELECT, INSERT, UPDATE, DELETE ON ALL TABLES IN SCHEMA public TO app_user`
- `pg_hba.conf` controla autenticação — `md5` ou `scram-sha-256` para produção

### Cap. 3 — psql
**Conceito central:** O cliente de linha de comando `psql` — ferramenta indispensável.
**Comandos essenciais:**
- `\l` — lista databases
- `\c dbname` — conecta ao database
- `\dt` — lista tabelas
- `\d tablename` — descreve estrutura de tabela
- `\timing` — mostra tempo de execução
- `\e` — abre editor para escrever queries longas
- `\copy` — importa/exporta dados CSV

### Cap. 4 — pgAdmin
**Conceito central:** Interface gráfica para PostgreSQL.
**Relevante para o ZionHub:** pgAdmin ou DBeaver para explorar o banco em desenvolvimento.

### Cap. 5 — Data Types
**Conceito central:** Os tipos de dados do PostgreSQL — muito além do SQL padrão.
**Tipos mais relevantes para o ZionHub:**
- `TEXT` — string de tamanho variável, sem limite (prefira a `VARCHAR(n)` em PostgreSQL)
- `INTEGER` / `BIGINT` — inteiros (use BIGINT para IDs se o sistema pode crescer)
- `NUMERIC(precision, scale)` — para valores monetários (não use FLOAT para dinheiro!)
- `BOOLEAN` — true/false/null
- `TIMESTAMP WITH TIME ZONE` (timestamptz) — sempre use com timezone em sistemas distribuídos
- `UUID` — para IDs não-sequenciais (mais seguro que serial para exposição em APIs)
- `JSONB` — JSON binário indexável — para dados semi-estruturados (melhor que `JSON` puro)
- `ARRAY` — arrays nativos: `INTEGER[]`, `TEXT[]`
- `ENUM` — tipos enumerados com constraint de banco
**Frase marcante:** "Use TEXT instead of VARCHAR in PostgreSQL — they are identical in performance, but TEXT is simpler."
**Conexão:** Learning SQL (Beaulieu) — tipos padrão SQL. Designing Data-Intensive Applications (Kleppmann) — schema evolution.

### Cap. 6 — Tables, Constraints, and Indexes
**Conceito central:** DDL PostgreSQL-específico.
**Regras práticas:**
- `PRIMARY KEY` cria automaticamente um índice B-tree UNIQUE
- `FOREIGN KEY ... ON DELETE CASCADE` vs. `SET NULL` vs. `RESTRICT` — escolha com intenção
- `CHECK` constraints para regras de negócio simples no banco
- `UNIQUE` constraint para garantir unicidade (ex: email de usuário)

#### Tipos de Índice no PostgreSQL
- **B-tree** (padrão): para `=`, `<`, `>`, `BETWEEN`, `LIKE 'foo%'`
- **GIN** (Generalized Inverted Index): para `JSONB`, `arrays`, full-text search
- **GiST**: para geometria, ranges, full-text
- **Hash**: apenas para `=` — raramente vantajoso sobre B-tree
- **Partial index**: `CREATE INDEX ON orders (user_id) WHERE status = 'pending'` — índice em subconjunto

**Frase marcante:** "Indexes speed up reads but slow down writes. Don't over-index."
**Conexão:** Learning SQL (Beaulieu) — Cap. 13 sobre índices. Designing Data-Intensive Applications (Kleppmann) — Cap. 3 sobre storage engines.

### Cap. 7 — PSQL Features (queries avançadas)
**Conceito central:** CTEs, window functions, RETURNING.
**Regras práticas:**
- **CTE (Common Table Expressions):**
```sql
WITH active_users AS (
    SELECT * FROM users WHERE is_active = true
)
SELECT * FROM active_users WHERE created_at > NOW() - INTERVAL '30 days';
```
- **Window Functions:** para rankings, running totals, sem GROUP BY
```sql
SELECT user_id, amount,
    RANK() OVER (PARTITION BY user_id ORDER BY amount DESC) as rank
FROM orders;
```
- **RETURNING:** obtenha o registro após INSERT/UPDATE/DELETE
```sql
INSERT INTO orders (user_id, amount) VALUES (1, 100)
RETURNING id, created_at;
```
**Conexão:** Learning SQL (Beaulieu) — subqueries como precursor de CTEs.

### Cap. 8 — Writing Functions
**Conceito central:** PL/pgSQL para stored procedures e functions no banco.
**Onde questionar:** Para o ZionHub, prefira lógica de negócio na aplicação Python — stored procedures são mais difíceis de versionar, testar e debugar.

### Cap. 9 — Query Performance Tuning
**Conceito central:** `EXPLAIN` e `EXPLAIN ANALYZE` para entender o plano de execução de queries.
**Regras práticas:**
- `EXPLAIN ANALYZE SELECT ...` mostra o plano real de execução com custos reais
- Procure por "Seq Scan" em tabelas grandes — pode precisar de índice
- `VACUUM ANALYZE` para atualizar estatísticas do query planner
- `pg_stat_statements` para identificar queries lentas em produção

### Cap. 10 — Replication and External Data
**Conceito central:** Replicação PostgreSQL para alta disponibilidade.
**Para o ZionHub:** Entendimento de contexto — a infraestrutura de produção decide quando usar.

### Cap. 11 — Useful Extensions
**Extensões mais relevantes:**
- `uuid-ossp` ou `pgcrypto` — para geração de UUIDs (`gen_random_uuid()`)
- `pg_trgm` — para buscas de texto com trigrams (LIKE eficiente)
- `pgcrypto` — para hashing de passwords no banco (mas prefira bcrypt na aplicação)
- `tablefunc` — para pivot tables

## Tensões com Outros Livros
- **vs. Learning SQL:** Beaulieu ensina SQL padrão; Obe & Hsu ensinam PostgreSQL avançado. Leia Beaulieu primeiro.
- **vs. Designing Data-Intensive Applications:** Kleppmann explica por que B-trees existem; Obe & Hsu dizem como usá-los no PostgreSQL.

## Aplicação em Python Moderno (SQLAlchemy + asyncpg)
```python
# Tipos PostgreSQL em SQLAlchemy
from sqlalchemy.dialects.postgresql import UUID, JSONB, ARRAY
from sqlalchemy import Column, Text, TIMESTAMP
import uuid

class User(Base):
    __tablename__ = "users"
    id: Mapped[uuid.UUID] = mapped_column(
        UUID(as_uuid=True),
        primary_key=True,
        default=uuid.uuid4
    )
    name: Mapped[str] = mapped_column(Text)
    metadata: Mapped[dict] = mapped_column(JSONB, default={})
    tags: Mapped[list] = mapped_column(ARRAY(Text), default=[])
    created_at: Mapped[datetime] = mapped_column(
        TIMESTAMP(timezone=True),
        default=func.now()
    )

# RETURNING em SQLAlchemy
stmt = insert(Order).values(user_id=1, amount=100).returning(Order.id)
result = await session.execute(stmt)
order_id = result.scalar_one()

# EXPLAIN ANALYZE via SQLAlchemy (para debugging)
result = await session.execute(text("EXPLAIN ANALYZE SELECT * FROM orders WHERE user_id = :uid"), {"uid": 1})
for row in result:
    print(row[0])
```
