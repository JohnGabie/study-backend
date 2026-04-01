# Python Crash Course, 3ª Edição — Eric Matthes (2023)

## Tese Central
Qualquer pessoa pode aprender a programar em Python com projetos práticos reais. O aprendizado acontece fazendo — não lendo sobre fazer.

## Por que Este Livro Importa
É o ponto de entrada mais eficiente para Python. Matthes não ensina sintaxe abstrata — ensina Python através de três projetos completos (jogo, visualização de dados, aplicação web). Para quem já tem experiência, funciona como referência rápida dos fundamentos.

## Onde o Livro Pode Ser Questionado
- Projetos usam Pygame e Django — não Python puro ou FastAPI
- A profundidade é suficiente para iniciantes mas insuficiente para engenharia de software séria — é o ponto de partida, não o destino
- Não cobre asyncio, type hints avançados, Pydantic, ORMs modernos
- Cap. 11 (Testing) usa `unittest` — pytest é a escolha moderna e mais idiomática

## Frases-Chave do Autor
> "When you write tests, you can make any improvements you want to your code and know that you haven't broken existing behavior." — Cap. 11

## Mapa por Capítulo

### Part I — Basics (Caps. 1–11)

#### Cap. 1 — Getting Started (p. 3)
Setup do ambiente Python. Primeira linha de código.

#### Cap. 2 — Variables and Simple Data Types (p. 15)
**Conceito central:** Variáveis como rótulos que apontam para objetos. Strings, inteiros, floats.
**Regras práticas:** f-strings para formatação. `str.lower()`, `str.upper()`, `str.title()` para manipulação.

#### Cap. 3–4 — Lists e Working with Lists (p. 33, 49)
**Conceito central:** Listas como sequências mutáveis. List comprehensions como idioma Python.
**Regras práticas:** `append()`, `insert()`, `pop()`, `remove()`. `sorted()` vs `list.sort()`. List comprehensions > loops para transformações simples.
**Conexão:** Fluent Python (Ramalho) — Cap. 2 aprofunda o modelo de sequências.

#### Cap. 5 — if Statements (p. 71)
Condicionais. Comparações. Booleanos.

#### Cap. 6 — Dictionaries (p. 91)
**Conceito central:** Dicionários como pares chave-valor. Iteração sobre dicts.
**Conexão:** Effective Python (Slatkin) — Items 25–28 sobre uso avançado de dicts.

#### Cap. 7 — User Input and while Loops (p. 113)
Input do usuário. Loops while.

#### Cap. 8 — Functions (p. 129)
**Conceito central:** Funções como unidades de reuso. Parâmetros posicionais, keyword, default values, `*args`, `**kwargs`.
**Regras práticas:** Funções pequenas e focadas. Nomes descritivos.
**Conexão:** Clean Code Fundamentals (Hock) — Cap. 3 sobre funções.

#### Cap. 9 — Classes (p. 157)
**Conceito central:** OOP em Python. `__init__`, herança, `super()`.
**Conexão:** Fluent Python (Ramalho) — Cap. 11 sobre objetos Pythônicos.

#### Cap. 10 — Files and Exceptions (p. 183)
Leitura/escrita de arquivos. `try/except/else/finally`. `json` module.

#### Cap. 11 — Testing Your Code (p. 209)
**Conceito central:** Testes automatizados como verificação de comportamento.
**Regras práticas:** `unittest.TestCase`. Assertions. Fixtures.
**Onde questionar:** O livro usa `unittest` — o ecossistema Python moderno prefere `pytest`. O conceito é o mesmo; a ferramenta é diferente.
**Conexão:** Python Testing with pytest (Okken) — a versão moderna do que Matthes ensina aqui.

### Part II — Projects (Caps. 12–20)

#### Caps. 12–14 — Alien Invasion (jogo com Pygame) (p. 227–299)
Aplicação de OOP, eventos, game loop. Não relevante para o ZionHub diretamente, mas consolida OOP.

#### Caps. 15–17 — Data Visualization (p. 301–381)
matplotlib, Plotly, APIs públicas. Relevante para visualizar dados do ZionHub.

#### Caps. 18–20 — Web Application (Django) (p. 373–461)
**Conceito central:** Aplicação web completa com Django — autenticação, banco de dados, deployment.
**Onde questionar:** O livro usa Django; o ZionHub usa FastAPI. Os conceitos de request/response, models, views são análogos mas com sintaxe diferente.
**Conexão:** HTTP: The Definitive Guide (Gourley & Totty) — o protocolo por baixo do Django e do FastAPI.

## Tensões com Outros Livros
- **vs. Effective Python (Slatkin):** Matthes é o ponto de entrada; Slatkin é o próximo passo — 125 formas de fazer melhor o que Matthes ensina.
- **vs. Fluent Python (Ramalho):** Matthes ensina o *que* Python faz; Ramalho explica o *por que* Python funciona assim.

## Aplicação em Python Moderno
Este livro é o fundamento. Em Python moderno (3.10+):
- f-strings (Cap. 2) → ainda a forma preferida de formatação
- List comprehensions (Cap. 3-4) → idiomático e Pythônico
- Funções (Cap. 8) → base para closures e decorators (Fluent Python)
- Classes (Cap. 9) → base para Pydantic models no FastAPI
- Exceptions (Cap. 10) → base para tratamento de erros em APIs
- Testing (Cap. 11) → base para pytest e TDD
