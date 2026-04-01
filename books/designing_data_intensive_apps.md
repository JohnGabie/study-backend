# Designing Data-Intensive Applications — Martin Kleppmann (2017)

## Tese Central
Os problemas difíceis de sistemas modernos não são de CPU — são de dados: como armazená-los de forma confiável, como fazê-los escalar, como mantê-los corretos quando múltiplos processos os acessam simultaneamente. Kleppmann desmonta a "magia" de bancos de dados e sistemas distribuídos para revelar os trade-offs reais.

## Por que Este Livro Importa
Todo desenvolvedor backend toma decisões sobre dados todo dia — escolher entre SQL e NoSQL, usar uma transação ou não, confiar em eventual consistency. Sem entender os fundamentos, essas decisões são apostas. Este livro transforma apostas em escolhas informadas.

## Onde o Livro Pode Ser Questionado
- Foco em sistemas de grande escala (Google, Amazon, LinkedIn) — muitos trade-offs não fazem sentido para o tamanho do ZionHub hoje
- O livro é de 2017 — alguns detalhes sobre tecnologias específicas estão desatualizados (ex: ecossistema de bancos distribuídos evoluiu)
- A profundidade pode paralisar: nem todo projeto precisa se preocupar com CAP theorem na prática
- O livro não fala de ORMs, FastAPI, nem Python — a tradução para o contexto prático é responsabilidade do leitor

## Frases-Chave do Autor
> "Reliability, scalability, and maintainability are not just buzzwords — they are the three main concerns that influence the design of a data system." — Cap. 1, p. 3

> "A fault is not the same as a failure. A fault is when one component deviates from its spec, whereas a failure is when the system as a whole stops providing the required service." — Cap. 1, p. 6

> "The truth is the log. The database is a cache of a subset of the log." — Cap. 11 (sobre event sourcing)

## Mapa por Capítulo

### Cap. 1 — Reliable, Scalable, and Maintainable Applications (p. 3–22)
**Conceito central:** Os três pilares de um sistema de dados bem projetado.
**Regras práticas:**
- **Reliability:** O sistema deve funcionar corretamente mesmo diante de falhas de hardware, software ou humanas. "Crash-only software" — sistemas que falham limpo são mais confiáveis que sistemas que tentam se recuperar de formas imprevisíveis.
- **Scalability:** Capacidade de lidar com crescimento de carga. Descreva a carga (parâmetros de carga) antes de medir performance.
- **Maintainability:** Operabilidade (fácil de operar), Simplicidade (fácil de entender), Evolvabilidade (fácil de mudar).
**Frase marcante:** "Hardware faults, software errors, and human errors are the three main sources of faults." — p. 7
**Onde questionar:** Para o ZionHub, "maintainability" é o pilar mais urgente agora — não escalabilidade.
**Conexão:** Site Reliability Engineering (Google) — SRE opera na camada de reliability em escala.

### Cap. 2 — Data Models and Query Languages (p. 27–63)
**Conceito central:** O modelo de dados é a abstração mais fundamental — ele molda como você pensa sobre o problema.
**Regras práticas:**
- Relacional: bom para dados interconectados com relacionamentos muitos-para-muitos
- Documento (NoSQL): bom para dados com estrutura variável e pouca necessidade de joins
- Grafo: bom para dados altamente interconectados (redes sociais, sistemas de recomendação)
**Frase marcante:** "The object-relational mismatch" — a impedância entre OOP e modelo relacional gera ORMs como solução imperfeita.
**Onde questionar:** Em Python com SQLAlchemy/Alembic, o mismatch é gerenciado mas não eliminado — entender o modelo relacional subjacente é crítico para escrever queries eficientes.
**Conexão:** Learning SQL (Beaulieu) — o modelo relacional em detalhe. PostgreSQL Up and Running (Obe & Hsu) — implementação prática.

### Cap. 3 — Storage and Retrieval (p. 69–101)
**Conceito central:** Como bancos de dados funcionam internamente — LSM-trees vs B-trees, índices, column-oriented storage.
**Regras práticas:** Índices aceleram leituras mas custam em escritas. OLTP vs OLAP são casos de uso fundamentalmente diferentes.
**Onde questionar:** Para o ZionHub, entender índices em PostgreSQL é a aplicação prática imediata deste capítulo.
**Conexão:** PostgreSQL Up and Running (Obe & Hsu) — como criar e usar índices no PostgreSQL.

### Cap. 4 — Encoding and Evolution (p. 111–144)
**Conceito central:** Dados têm vida mais longa que código. Como fazer schemas evoluírem sem quebrar compatibilidade.
**Regras práticas:** Forward compatibility e backward compatibility. Cuidado com renomear campos em schemas (breaking change).
**Onde questionar:** Alembic migrations no ZionHub — cada migration deve ser testada para compatibilidade com versões anteriores da API.
**Conexão:** Designing Data-Intensive Applications é o único livro da biblioteca que cobre isso em profundidade.

### Caps. 5–9 — Replicação, Particionamento, Transações, Sistemas Distribuídos
**Conceito central:** Consistência, disponibilidade e tolerância a partições (CAP) — você só pode ter dois dos três.
**Conceitos-chave:**
- **Replication lag:** em replicação assíncrona, o réplica pode estar atrás do líder — reads podem retornar dados obsoletos
- **ACID:** Atomicity, Consistency, Isolation, Durability — PostgreSQL oferece ACID completo
- **Isolation levels:** Read uncommitted, read committed, repeatable read, serializable — cada um com trade-offs de performance vs. corretude
**Frase marcante:** "The constraints of distributed systems are fundamentally different from single-node systems." — a premissa de todo Part II.
**Conexão:** PostgreSQL Up and Running — transações e isolation levels em PostgreSQL.

### Cap. 10–12 — Batch Processing, Stream Processing, Future of Data Systems
**Conceito central:** Dados como fluxo (stream) versus dados como estado (batch). Event sourcing como alternativa ao modelo CRUD.
**Frase marcante:** "The truth is the log." — event sourcing como padrão arquitetural.
**Onde questionar:** Event sourcing adiciona complexidade significativa — só vale para domínios onde o histórico completo é valioso.
**Conexão:** Architecture Patterns with Python (Percival & Gregory) — event sourcing em Python.

## Tensões com Outros Livros
- **vs. Learning SQL:** Beaulieu ensina o *como* do SQL; Kleppmann ensina o *porquê* dos trade-offs por trás das abstrações SQL.
- **vs. PostgreSQL Up and Running:** Obe & Hsu são práticos e orientados ao PostgreSQL; Kleppmann é teórico e agnóstico de banco.
- **vs. Architecture Patterns with Python:** Ambos falam sobre separação de camadas e event sourcing, mas em escalas diferentes.

## Aplicação em Python Moderno
```python
# Cap. 2: Object-Relational Mismatch em SQLAlchemy
# O ORM esconde o SQL, mas o modelo relacional ainda existe — entendê-lo é crítico

# ❌ N+1 query problem (invisível no ORM sem entender o modelo)
users = session.query(User).all()
for user in users:
    print(user.orders)  # cada acesso dispara um SELECT — N+1 queries

# ✅ Eager loading explícito
users = session.query(User).options(joinedload(User.orders)).all()

# Cap. 4: Schema evolution com Pydantic
class UserV1(BaseModel):
    name: str

class UserV2(BaseModel):
    name: str
    email: str = ""  # campo novo com default = backward compatible

# Cap. 7: Transações no SQLAlchemy
async with session.begin():  # ACID transaction
    user = User(name="João")
    session.add(user)
    # commit automático no final do bloco, rollback em exceção
```

Para o ZionHub: o capítulo mais imediatamente relevante é o 1 (Reliability/Maintainability como design goals) e o 3 (índices no PostgreSQL). O Capítulo 7 (Transactions) se aplica quando houver operações que precisam ser atômicas.
