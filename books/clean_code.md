# Clean Code Fundamentals — Martin Hock (2024)

> ⚠️ **Nota:** O livro disponível nesta biblioteca é "Clean Code Fundamentals" de Martin Hock (Leanpub, 2024) — uma síntese prática baseada no "Clean Code" original de Robert C. Martin e em outros autores. O livro referencia diretamente as obras de Martin. Citações e capítulos refletem a obra de Hock, não a de Martin.

## Tese Central
Código limpo não é estético — é profissional. Um desenvolvedor que escreve código sujo está transferindo custo para outros. O livro defende que artesanato de software (software craftsmanship) é uma postura ética, não uma preferência estilística.

## Por que Este Livro Importa
A maioria dos bugs não vem de algoritmos errados — vem de código que ninguém consegue entender. Este livro formaliza as práticas que separam código que funciona de código que pode ser mantido, evoluído e entendido por humanos.

## Onde o Livro Pode Ser Questionado
- Exemplos são em Java — a transposição para Python nem sempre é direta (Python favorece menos verbosidade e menos encapsulamento formal)
- A regra "funções devem fazer uma coisa" pode entrar em conflito com a legibilidade quando o "uma coisa" é muito granular
- Comentários: Hock (seguindo Martin) diz que comentários são falha de expressão — Ousterhout em *A Philosophy of Software Design* discorda, defendendo comentários que explicam o *porquê*
- O foco em OOP pode não mapear diretamente para código funcional/assíncrono como asyncio

## Frases-Chave do Autor
> "Leave the codebase in a better state than you found it." — Cap. 1: "Boy Scout Rule", p. 7

> "Choose expressive names and avoid mental mapping." — Cap. 3: "Clean Code Best Practices", p. 44

## Mapa por Capítulo

### Cap. 1 — Introduction to Software Craftsmanship and Clean Code (p. 1–10)
**Conceito central:** Ser um desenvolvedor profissional significa se importar com a qualidade do código, não apenas com a entrega.
**Regras práticas:** Aplique o Boy Scout Rule — sempre deixe o código um pouco melhor do que encontrou. Evite Cargo Cult Programming (copiar padrões sem entender o porquê).
**Exemplo do autor:** A Broken Windows Theory aplicada ao código: um arquivo bagunçado "autoriza" outros a bagunçarem mais.
**Frase marcante:** "A Passion for Software Development" — a diferença entre um desenvolvedor que entrega e um que se importa.
**Onde questionar:** O Boy Scout Rule pode gerar escopo infinito se não houver disciplina para delimitar o que "melhorar" significa em cada PR.
**Conexão:** Pragmatic Programmer (Hunt & Thomas) — o conceito de "broken windows" aparece em ambos como metáfora para entropia de software.

### Cap. 2 — Basics of Software Design (p. 11–41)
**Conceito central:** Design não é sobre UML — é sobre coesão, acoplamento e separação de responsabilidades.
**Regras práticas:** Alta coesão + baixo acoplamento. Layered architecture. Evite o "Big Ball of Mud".
**Exemplo do autor:** Layered Architecture com suas variações: horizontal, feature-based, hexagonal.
**Frase marcante:** Cohesion e Coupling como os dois eixos principais para avaliar qualidade de design.
**Onde questionar:** Arquitetura hexagonal pode ser over-engineering para serviços simples do ZionHub.
**Conexão:** Architecture Patterns with Python (Percival & Gregory) — aplica esses princípios concretamente em Python.

### Cap. 3 — Clean Code Best Practices (p. 42–66)
**Conceito central:** Código se comunica. Nomes ruins são mentiras; funções longas são segredos; comentários são confissões de fracasso.
**Regras práticas:**
- Nomes expressivos, sem mapeamento mental
- Nomes positivos para booleanos (`is_active`, não `not_disabled`)
- Funções sem `and`/`or` no nome (violam SRP)
- Prefira código autoexplicativo a comentários
- Agrupe por quebras de linha (parágrafo visual)
**Exemplo do autor:** Evite `and` em nomes de método: `validateAndSave()` está fazendo duas coisas.
**Frase marcante:** "Prefer self-explanatory code instead of comments." — p. 54
**Onde questionar:** Em Python com FastAPI, type hints e Pydantic já comunicam muito — às vezes um comentário curto sobre decisão de negócio vale mais que refatorar o nome da função.
**Conexão:** Refactoring (Fowler) — renomear é uma das refatorações mais simples e mais impactantes.

### Cap. 4 — Software Quality Assurance (p. 67+)
**Conceito central:** Testes não são opcionais — são parte do design. TDD inverte a ordem: primeiro o teste, depois o código.
**Regras práticas:** Test Pyramid (unit → integration → e2e). TDD: Red → Green → Refactor.
**Exemplo do autor:** JUnit 5 para unit tests em Java — translate para pytest em Python.
**Frase marcante:** "Test-driven Development" como prática de design, não só de verificação.
**Onde questionar:** TDD rigoroso pode ser difícil em código vibecoded ou em refatorações de código legado sem testes.
**Conexão:** Python Testing with pytest (Okken) e TDD with Python (Percival) — implementação direta em Python.

## Tensões com Outros Livros
- **vs. A Philosophy of Software Design (Ousterhout):** Hock/Martin dizem "evite comentários". Ousterhout diz "comentários que explicam o porquê são obrigatórios". Essa tensão é real e produtiva — a resposta correta depende do contexto.
- **vs. Refactoring (Fowler):** Concordam na direção (código legível > código esperto), mas Fowler é mais incremental — ele refatora com testes em mãos, Hock/Martin prescrevem princípios a priori.
- **vs. Pragmatic Programmer:** PP é mais filosófico e amplo; Clean Code Fundamentals é mais tático e focado em código.

## Aplicação em Python Moderno
```python
# ❌ Violações comuns em código vibecoded
def process_data_and_save(d, flag=True):
    # processes data
    result = {}
    for k, v in d.items():
        result[k] = v * 2
    if flag:
        db.save(result)
    return result

# ✅ Aplicando princípios do livro
def double_values(measurements: dict[str, float]) -> dict[str, float]:
    return {key: value * 2 for key, value in measurements.items()}

def persist_measurements(measurements: dict[str, float]) -> None:
    db.save(measurements)
```

Em FastAPI: nomes de endpoints, parâmetros Pydantic e nomes de funções de serviço devem ser autoexplicativos. `create_user_account()` > `handle_user()`.
