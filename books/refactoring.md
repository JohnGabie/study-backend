# Refactoring: Improving the Design of Existing Code — Martin Fowler (2019, 2ª ed.)

## Tese Central
Refatoração não é reescrever — é melhorar a estrutura do código sem alterar seu comportamento externo, em pequenos passos seguros, guiados por testes. O código mal estruturado não quebra hoje; ele quebra quando você precisa mudá-lo.

## Por que Este Livro Importa
A maioria dos projetos não falha por algoritmos ruins — falha porque o código acumulou dívida técnica que torna cada mudança arriscada e lenta. Fowler oferece um catálogo de movimentos cirúrgicos que transformam código problemático sem quebrar nada.

## Onde o Livro Pode Ser Questionado
- Exemplos são em JavaScript (2ª ed.) — a lógica se aplica a Python, mas a sintaxe difere
- O processo rigoroso (teste → refatora → teste) assume que você tem testes — em código vibecoded sem testes, refatorar é mais perigoso
- Alguns smells são culturalmente relativos: o que é "função longa" em Java pode ser normal em Python com list comprehensions
- O catálogo é vasto (60+ técnicas) — sem contexto, pode se tornar uma lista de hammer em busca de pregos

## Frases-Chave do Autor
> "When you have to add a feature to a program but the code is not structured in a convenient way, first refactor the program to make it easy to add the feature, then add the feature." — Cap. 1, p. 4

> "A poorly designed system is hard to change—because it is difficult to figure out what to change and how these changes will interact with the existing code." — Cap. 1, p. 2

> "The first step in refactoring is always the same. I need to ensure I have a solid set of tests for that section of code." — Cap. 1, p. 8

## Mapa por Capítulo

### Cap. 1 — Refactoring: A First Example (p. 1–45)
**Conceito central:** Fowler começa com um exemplo concreto (sistema de peças teatrais) para mostrar o processo completo antes de definir qualquer princípio.
**Regras práticas:** Antes de qualquer refatoração, garanta testes. Faça um passo de cada vez. Nunca refatore e adicione feature ao mesmo tempo.
**Exemplo do autor:** Função `statement()` com switch case — decomposta em múltiplas funções com responsabilidades claras via Extract Function.
**Frase marcante:** "When feature requests come, they come not as single spies but in battalions." — p. 5 (justificativa para refatorar antes que o código precise mudar)
**Onde questionar:** O exemplo assume JavaScript e OOP. Em Python funcional/asyncio, a decomposição pode ser via funções puras em vez de classes.
**Conexão:** Clean Code Fundamentals (Hock) — ambos concordam que código legível > código esperto. Fowler é mais incremental.

### Cap. 2 — Principles in Refactoring
**Conceito central:** Definição formal: refatoração é uma sequência de transformações que preservam comportamento. O objetivo não é código bonito — é código fácil de mudar.
**Regras práticas:** Refatore continuamente, não em "sprints de limpeza". Separe refatoração de adição de feature nos commits.
**Frase marcante:** "If it ain't broke, don't fix it" — Fowler refuta isso: se vai precisar mudar, e está mal estruturado, o custo de não refatorar é real.
**Onde questionar:** Em contexto com deadline extremo, a disciplina de separar commits pode ser impraticável — mas o princípio permanece válido como ideal.

### Cap. 3 — Bad Smells in Code
**Conceito central:** Code smells são heurísticas — indicadores de que algo pode precisar de refatoração. Não são regras absolutas.
**Smells mais relevantes para Python/FastAPI:**
- **Long Function:** função que faz demais — comum em endpoints vibecoded
- **Divergent Change:** uma classe/função muda por razões não relacionadas (viola SRP)
- **Shotgun Surgery:** uma mudança exige mexer em muitos lugares
- **Feature Envy:** função usa mais dados de outra classe que da sua própria
- **Data Clumps:** grupos de dados que aparecem juntos — candidatos a se tornarem um objeto/dataclass
- **Long Parameter List:** mais de 3-4 parâmetros — considere agrupar em objeto
- **Comments:** comentário que explica o *o quê* (não o *porquê*) — sinal de que o código precisa de rename/extract
**Conexão:** Clean Code Fundamentals lista os mesmos smells com nomes ligeiramente diferentes.

### Cap. 4 — Building Tests
**Conceito central:** Sem testes, refatorar é adivinhar. Testes são a rede de segurança que torna o movimento rápido possível.
**Frase marcante:** Testes devem ser "self-checking" — não é suficiente executar, é necessário verificar o resultado automaticamente.
**Conexão:** TDD with Python (Percival) e Python Testing with pytest (Okken) — como construir essa rede em Python.

### Caps. 6–12 — Catálogo de Refatorações
**Técnicas mais relevantes para o ZionHub:**
- **Extract Function:** a mais usada — quando você vê um comentário antes de um bloco de código, extraia para uma função com nome descritivo
- **Inline Function:** inverso — quando o nome da função não adiciona informação além do corpo
- **Rename Variable/Function:** a mais simples e mais impactante
- **Extract Variable:** quebra expressões complexas em variáveis nomeadas
- **Split Phase:** separa código que mistura duas fases distintas (ex: validação + persistência em um endpoint)
- **Move Function:** quando uma função usa mais dados de outro módulo que do seu
- **Replace Temp with Query:** substitui variável temporária por função — melhora legibilidade em troca de possível custo de performance

## Tensões com Outros Livros
- **vs. Clean Code Fundamentals:** Hock/Martin prescrevem princípios a priori; Fowler refatora a partir do código existente. São complementares: Fowler para código legado, Martin para código novo.
- **vs. Pragmatic Programmer:** PP chama de "broken windows" o que Fowler chama de smells. PP é mais filosófico sobre a atitude; Fowler é cirúrgico sobre o como.
- **vs. TDD with Python:** Percival usa TDD para escrever código correto de início; Fowler usa testes para refatorar código existente. Dois lados da mesma moeda.

## Aplicação em Python Moderno
```python
# Code smell: Long Function com Divergent Change
# Um endpoint FastAPI que valida + persiste + notifica = 3 razões para mudar
@router.post("/orders")
async def create_order(data: dict, db: Session = Depends(get_db)):
    # validation
    if not data.get("user_id"):
        raise HTTPException(400, "user_id required")
    # persistence
    order = Order(**data)
    db.add(order)
    db.commit()
    # notification
    await send_email(data["user_id"], "Order created")
    return order

# Após Extract Function (Split Phase):
@router.post("/orders")
async def create_order(data: OrderCreate, db: Session = Depends(get_db)):
    order = await order_service.create(data, db)
    await notification_service.order_created(order)
    return order
```

O smell "Long Parameter List" em Python se resolve com Pydantic models. O smell "Data Clumps" vira um `dataclass` ou Pydantic schema.
