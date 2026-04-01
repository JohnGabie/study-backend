# Site Reliability Engineering: How Google Runs Production Systems — Beyer, Jones, Petoff & Murphy / Google (2016)

## Tese Central
Reliability não acontece por acidente — é uma propriedade de engenharia que deve ser projetada, medida e gerenciada. O SRE framework do Google transforma "manter sistemas funcionando" de uma arte reativa em uma disciplina de engenharia proativa.

## Por que Este Livro Importa
O ZionHub vai para produção. Produção significa usuários reais, falhas reais, e consequências reais quando algo quebra. Este livro responde: como você sabe que seu sistema é confiável? Como você decide quando é seguro deployar? Como você responde quando algo quebra às 3 da manhã?

## Onde o Livro Pode Ser Questionado
- Escala do Google é irrelevante para o ZionHub no momento — muitos conceitos fazem sentido apenas em escala massiva
- A cultura SRE pode ser difícil de implementar em times pequenos sem processo formal
- O livro é de 2016 — o ecossistema de observabilidade evoluiu (OpenTelemetry, Grafana, etc.)
- Alguns capítulos são muito específicos para a infraestrutura do Google

## Frases-Chave do Autor
> "Hope is not a strategy." — Cap. 1

> "The goal is to make the system more reliable, not less risky to change." — sobre error budgets

> "Every action a human operator takes is a potential source of failure." — Cap. 8 (on-call)

## Mapa por Capítulo

### Part I — Introduction

#### Cap. 1 — Introduction (p. 3–12)
**Conceito central:** O que é SRE. A distinção entre Ops tradicional e SRE.
**Regras práticas:**
- **Tenets of SRE:** Engenheiros de software responsáveis por reliability — não sysadmins
- **50% rule:** SREs não devem gastar mais de 50% do tempo em trabalho operacional reativo — o resto deve ser em engenharia proativa
- **Error budgets:** Se o sistema tem SLO de 99.9% de uptime, o budget de erro é 43.8 minutos/mês — gaste-o conscientemente
**Frase marcante:** "Hope is not a strategy."
**Conexão:** Designing Data-Intensive Applications (Kleppmann) — Cap. 1 sobre reliability como propriedade de design.

### Part II — Principles

#### Cap. 3 — Embracing Risk (p. 25–40)
**Conceito central:** 100% de confiabilidade não é o objetivo — é caro demais e ninguém percebe a diferença entre 99.99% e 100%.
**Regras práticas:**
- **Error Budget:** Defina um SLO (Service Level Objective) — ex: 99.9% de requests bem-sucedidos
- O error budget é a diferença entre 100% e o SLO — é o "quanto pode quebrar"
- Se o error budget está esgotado, pare de lançar features e foque em reliability
- Se o error budget está sobrando, lance mais rápido
**Frase marcante:** "The goal is to align the risk tolerance of the service with the risk tolerance of the users."
**Onde questionar:** Para o ZionHub, SLOs formais podem ser prematuros — mas a mentalidade de "quanto pode quebrar" é valiosa desde o início.

#### Cap. 4 — Service Level Objectives (p. 41–55)
**Conceito central:** SLI, SLO, SLA — a hierarquia de compromissos de confiabilidade.
- **SLI (Service Level Indicator):** Métrica que mede o comportamento do serviço (ex: latência, error rate, throughput)
- **SLO (Service Level Objective):** Target para o SLI (ex: "99.9% de requests com latência < 200ms")
- **SLA (Service Level Agreement):** Contrato com consequências (ex: "se SLO não for atingido, cliente recebe crédito")
**Regras práticas para o ZionHub:**
- Defina SLIs simples: error rate (% de 5xx), latência (p50, p95, p99)
- Comece com SLOs conservadores e ajuste com dados reais
- `p99 latency` é mais relevante que `average` — a média esconde outliers
**Conexão:** Designing Data-Intensive Applications (Kleppmann) — Cap. 1: "describing performance" com percentiles.

#### Cap. 5 — Eliminating Toil (p. 57–70)
**Conceito central:** Toil é trabalho manual, repetitivo e sem valor de engenharia — deve ser automatizado.
**Para o ZionHub:**
- Deploy manual = toil — automatize com CI/CD
- Rotação de secrets manual = toil — use secrets manager
- Reiniciar serviço manualmente = toil — use health checks + auto-restart
**Frase marcante:** "If a human operator needs to touch your system during normal operations, you have a bug."

#### Cap. 6 — Monitoring Distributed Systems (p. 71–88)
**Conceito central:** Os 4 Golden Signals de monitoramento.
**The Four Golden Signals:**
1. **Latency:** Tempo para atender um request — separe sucesso de erros
2. **Traffic:** Demanda no sistema — requests/segundo, queries/segundo
3. **Errors:** Taxa de requests que falham — 5xx, timeouts, exceções
4. **Saturation:** Quão "cheio" está o sistema — CPU, memória, I/O, thread pool
**Regras práticas para o ZionHub:**
- Instrumente estes 4 signals desde o início
- Alertas devem ser acionáveis — não alerte sobre o que você não pode ou não vai corrigir às 3 da manhã
- `prometheus` + `grafana` ou equivalente para métricas
**Conexão:** Designing Data-Intensive Applications (Kleppmann) — observabilidade como aspecto de maintainability.

#### Cap. 7 — The Evolution of Automation at Google
**Conceito central:** Automação como multiplicador de força — mas com cuidado.
**Regras práticas:** Automatize gradualmente. Automação ruim pode causar mais dano que intervenção humana.

#### Cap. 8 — Release Engineering (p. 115–128)
**Conceito central:** Release engineering como disciplina — como mudar sistemas em produção com segurança.
**Regras práticas:**
- **Continuous Delivery:** Cada commit que passa nos testes pode ser deployado
- **Canary deployments:** Deploy para subset de usuários antes de 100%
- **Feature flags:** Separe deploy de feature release — deploy o código, ative a feature gradualmente
- **Rollback strategy:** Sempre tenha um plano de rollback antes de deployar
**Conexão:** TDD with Python (Percival) — CI como pré-requisito para Continuous Delivery.

### Part III — Practices

#### Cap. 9 — Simplicity (p. 153–162)
**Conceito central:** Simplicidade como virtude de reliability — sistemas simples falham de formas previsíveis.
**Frase marcante:** "Unlike the complex system, the simple system does not hide surprises."
**Regras práticas:** Resista à feature creep. "Dead code is a liability, not an asset." — delete código não usado.
**Conexão:** Pragmatic Programmer (Hunt & Thomas) — Topic 27: "Don't Outrun Your Headlights."

#### Cap. 11 — Being On-Call (p. 177–196)
**Conceito central:** On-call como prática de engenharia, não punição.
**Regras práticas:**
- Runbooks: documentação de como responder a cada tipo de alerta
- Post-mortems sem culpa após incidentes — o objetivo é aprender, não punir
- **Blameless post-mortem:** Foca em o que falhou no sistema, não em quem falhou

#### Cap. 13 — Emergency Response (p. 215–226)
**Conceito central:** Como responder a incidentes.
**Regras práticas:**
- Primeiro: restore service (rollback, disable feature, increase capacity)
- Depois: investigate root cause
- Mantenha um log do incidente em tempo real (Google Doc, Slack thread)
**Frase marcante:** "The most important thing in an emergency is to stabilize the situation."

#### Cap. 14 — Managing Incidents (p. 227–240)
**Conceito central:** Incident Command System — roles claros durante um incidente.
**Roles:** Incident Commander, Operations Lead, Communications Lead.
**Para o ZionHub:** Mesmo em um time pequeno, ter um "incident commander" designado previne conflito de decisões.

#### Cap. 15 — Postmortem Culture (p. 241–252)
**Conceito central:** Post-mortems sem culpa como mecanismo de aprendizado organizacional.
**Estrutura de post-mortem:**
- Summary do incidente
- Timeline dos eventos
- Root cause analysis
- Impact (usuários afetados, tempo de indisponibilidade)
- Action items (o que vai mudar para prevenir reocorrência)
**Frase marcante:** "The blameless postmortem is a cornerstone of SRE culture."

## Tensões com Outros Livros
- **vs. Designing Data-Intensive Applications:** Kleppmann foca em reliability do sistema de dados; SRE foca em reliability do serviço como um todo. São complementares.
- **vs. Web Application Security:** Security é sobre confidencialidade e integridade; SRE é sobre availability. São três lados da tríade CIA.
- **vs. The Phoenix Project (Kim):** Phoenix Project conta a história (fictícia) de uma transformação cultural DevOps; SRE é o manual técnico dessa transformação.

## Aplicação em Python Moderno (FastAPI)
```python
# Os 4 Golden Signals instrumentados com prometheus_fastapi_instrumentator
from prometheus_fastapi_instrumentator import Instrumentator

app = FastAPI()
Instrumentator().instrument(app).expose(app)
# Automaticamente instrumenta: latência, throughput, error rate

# Health check endpoint (essencial para load balancers)
@app.get("/health")
async def health_check():
    return {"status": "healthy", "timestamp": datetime.utcnow()}

# Liveness vs. Readiness (Kubernetes pattern)
@app.get("/ready")
async def readiness_check(db: AsyncSession = Depends(get_db)):
    try:
        await db.execute(text("SELECT 1"))
        return {"status": "ready"}
    except Exception:
        raise HTTPException(503, "Database unavailable")

# Structured logging para observabilidade
import structlog
logger = structlog.get_logger()

@router.post("/orders")
async def create_order(data: OrderCreate, request: Request):
    logger.info(
        "order.create.started",
        user_id=data.user_id,
        request_id=request.headers.get("X-Request-Id")
    )
    order = await order_service.create(data)
    logger.info("order.create.completed", order_id=order.id)
    return order
```
