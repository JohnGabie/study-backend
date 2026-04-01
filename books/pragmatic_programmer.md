# The Pragmatic Programmer: Your Journey to Mastery — Dave Thomas & Andy Hunt (2020, 20ª ed.)

## Tese Central
Programação é um ofício, não apenas um emprego técnico. O programador pragmático é responsável pelo seu próprio crescimento, pelas suas ferramentas, pelo código que produz — e pensa além da tarefa imediata para o sistema como um todo.

## Por que Este Livro Importa
Diferente de livros táticos (como Clean Code ou Refactoring), este livro forma a *postura* do desenvolvedor. Ele responde à pergunta: "Como pensar sobre software?", não "Como escrever esta função?". É o livro que muda trajetórias, não apenas habilidades pontuais.

## Onde o Livro Pode Ser Questionado
- Alguns tópicos são amplamente conhecidos hoje (controle de versão, debugger) — o que era radical em 1999 é básico em 2024
- O livro é agnóstico de linguagem, o que é uma força e uma fraqueza: falta ancoragem em Python/FastAPI moderno
- A metáfora do "Broken Window" (compartilhada com Clean Code) pode justificar perfeccionismo paralisante se mal interpretada
- "DRY" (Don't Repeat Yourself) é frequentemente mal aplicado — evitar duplicação de *conhecimento* é diferente de evitar duplicação de *código*

## Frases-Chave do Autor
> "Don't Repeat Yourself: Every piece of knowledge must have a single, unambiguous, authoritative representation within a system." — Cap. 2: "A Pragmatic Approach", Topic 9

> "Good design is easier to change than bad design." — Cap. 2: "The Essence of Good Design", Topic 8

> "Don't outrun your headlights." — Cap. 4: "Pragmatic Paranoia", Topic 27

> "Invest regularly in your knowledge portfolio." — Cap. 1: "A Pragmatic Philosophy", Topic 6

## Mapa por Capítulo

### Cap. 1 — A Pragmatic Philosophy (Topics 1–7)
**Conceito central:** Responsabilidade pessoal. Você é o dono do seu código, da sua carreira, dos seus erros.
**Regras práticas:**
- **Topic 1 (It's Your Life):** Você tem agência sobre onde trabalha e o que aprende. Se está insatisfeito com sua habilidade, é responsabilidade sua mudar.
- **Topic 3 (Software Entropy):** "Broken Windows" — uma janela quebrada (código ruim tolerado) sinaliza abandono e convida mais degradação.
- **Topic 4 (Stone Soup):** Seja o catalisador da mudança — comece algo bom, envolva outros.
- **Topic 5 (Good-Enough Software):** Software "bom o suficiente" para o usuário e para a manutenção — perfeição inimiga do bom.
- **Topic 6 (Knowledge Portfolio):** Invista no seu aprendizado como se fosse um portfólio financeiro: diversifique, invista regularmente, reveja periodicamente.
**Frase marcante:** "Care about your craft." — a abertura do livro.
**Conexão:** Clean Code Fundamentals (Hock) — Manifesto for Software Craftsmanship é a mesma postura.

### Cap. 2 — A Pragmatic Approach (Topics 8–15)
**Conceito central:** DRY, Ortogonalidade, Reversibilidade — princípios que tornam o sistema fácil de mudar.
**Regras práticas:**
- **Topic 8 (Good Design):** ETC — Easier to Change. Todo princípio de design (SRP, DRY, desacoplamento) é uma instância disso.
- **Topic 9 (DRY):** Não repita *conhecimento* — não apenas código. Duplicar uma lógica de validação em dois lugares é um DRY violation mesmo que o código seja diferente.
- **Topic 10 (Orthogonality):** Mudanças em um componente não devem afetar outros. Em FastAPI: endpoint ortogonal ao serviço ortogonal ao repositório.
- **Topic 12 (Tracer Bullets):** Construa o esqueleto completo primeiro (endpoint → serviço → banco), depois preencha com funcionalidade real.
**Frase marcante:** "DRY is about the duplication of *knowledge*, of *intent*." — não é sobre duplicação de texto.
**Onde questionar:** DRY aplicado com excesso gera abstrações prematuras — três linhas similares não exigem uma função.
**Conexão:** Refactoring (Fowler) — DRY violation é frequentemente o que gera o smell "Duplicate Code".

### Cap. 4 — Pragmatic Paranoia (Topics 23–27)
**Conceito central:** Não confie em si mesmo. Defenda-se de seus próprios erros com contratos, asserções e limites claros.
**Regras práticas:**
- **Topic 23 (Design by Contract):** Defina precondições, pós-condições e invariantes — Pydantic em FastAPI é design by contract aplicado.
- **Topic 24 (Dead Programs Tell No Lies):** Crash early — é melhor falhar alto e imediatamente do que continuar com estado corrompido.
- **Topic 25 (Assertive Programming):** Use asserts para verificar suposições internas — não para validação de input do usuário.
- **Topic 27 (Don't Outrun Your Headlights):** Não construa para o futuro imaginado — construa para o presente com design que permita mudança.
**Conexão:** Designing Data-Intensive Applications (Kleppmann) — falhas silenciosas são mais perigosas que crashes.

### Cap. 5 — Bend, or Break (Topics 28–32)
**Conceito central:** Desacoplamento como estratégia de sobrevivência.
**Regras práticas:**
- **Topic 28 (Decoupling):** Tell, don't ask — não pergunte ao objeto seu estado para decidir o que fazer; diga ao objeto o que fazer.
- **Topic 31 (Inheritance Tax):** Prefira composição e interfaces a herança profunda — em Python, protocolos e mixins são mais pragmáticos que herança múltipla.
**Conexão:** Architecture Patterns with Python — a mesma batalha contra acoplamento.

### Cap. 7 — While You Are Coding (Topics 37–44)
**Conceito central:** Programação deliberada, não por coincidência.
**Regras práticas:**
- **Topic 38 (Programming by Coincidence):** Se funciona mas você não sabe por quê — é uma bomba relógio. Entenda o que você faz.
- **Topic 40 (Refactoring):** Refatore cedo e frequentemente, como arrancar ervas daninhas — não como demolir e reconstruir.
- **Topic 41 (Test to Code):** Testes não são só verificação — são a especificação executável do comportamento esperado.
- **Topic 44 (Naming Things):** Nomes são o contrato mais básico entre você e o próximo leitor do código.
**Frase marcante:** "Don't program by coincidence." — p. (ver edição)
**Conexão:** Clean Code Fundamentals (Hock) — Naming Things (Topic 44) é o Cap. 3.1 de Hock.

## Tensões com Outros Livros
- **vs. Clean Code Fundamentals:** PP é filosófico e estratégico; Clean Code é tático e específico. PP forma a mentalidade; Clean Code forma os hábitos.
- **vs. Refactoring (Fowler):** PP/Topic 40 menciona refatoração como prática contínua. Fowler dá o catálogo cirúrgico.
- **vs. Designing Data-Intensive Applications:** PP fala sobre sistemas confiáveis no nível do desenvolvedor individual; Kleppmann fala sobre sistemas confiáveis no nível da arquitetura distribuída. Escalas diferentes do mesmo problema.

## Aplicação em Python Moderno
```python
# DRY: conhecimento duplicado (não apenas código)
# ❌ Regra de negócio duplicada em dois lugares
def create_user(email: str):
    if len(email) < 3 or "@" not in email:  # validação aqui
        raise ValueError("invalid email")
    ...

def update_user_email(user_id: int, email: str):
    if len(email) < 3 or "@" not in email:  # mesma lógica aqui = DRY violation
        raise ValueError("invalid email")
    ...

# ✅ DRY: conhecimento de validação em um lugar
class EmailAddress(str):
    @classmethod
    def validate(cls, value: str) -> "EmailAddress":
        if len(value) < 3 or "@" not in value:
            raise ValueError("invalid email")
        return cls(value)

# Design by Contract com Pydantic (Topic 23):
class UserCreate(BaseModel):
    email: EmailStr  # contrato: este campo é um email válido
    age: int = Field(gt=0, lt=150)  # contrato: 0 < age < 150
```

"Tracer Bullets" (Topic 12) é o que diferencia prototipagem de scaffolding: o tracer bullet é o sistema real, sem mocks, funcionando de ponta a ponta — mesmo que com funcionalidade mínima.
