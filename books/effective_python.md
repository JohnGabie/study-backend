# Effective Python: 125 Specific Ways to Write Better Python — Brett Slatkin (2024, 3ª ed.)

## Tese Central
Python tem idiomas específicos para cada situação — e usar o idioma errado não só é feio, é mais lento, mais frágil e mais difícil de entender. 125 itens concretos sobre como fazer Python do jeito Python.

## Por que Este Livro Importa
O gap entre "escreve Python" e "escreve Python bem" é imenso. Este livro preenche esse gap com regras práticas e concretas — sem teoria, sem filosofia, direto ao ponto. É o livro que transforma Python funcional em Python idiomático.

## Onde o Livro Pode Ser Questionado
- Cada item é apresentado isoladamente — as conexões entre eles ficam a cargo do leitor
- Alguns itens são defensivos demais para código de aplicação (mais relevantes para autores de bibliotecas)
- A 3ª edição tem 125 itens — sem um plano de leitura, vira catálogo sem contexto
- Alguns itens sobre concorrência (Items 67–79) foram parcialmente suplantados pelo livro de Fowler em profundidade

## Frases-Chave do Autor
> "Prefer Raising Exceptions to Returning None." — Item 32

> "Use Comprehensions Instead of map and filter." — Item 40

> "Profile Before Optimizing." — Item 92

## Mapa por Capítulo

### Cap. 1 — Pythonic Thinking (Items 1–9)
**Conceito central:** Python tem uma forma de fazer cada coisa — aprenda a reconhecê-la e preferi-la.
**Itens mais importantes:**
- **Item 2 (PEP 8):** Guia de estilo como contrato de equipe — não é opcional, é comunicação
- **Item 4 (Helper Functions):** Extraia expressões complexas em funções com nome — legibilidade > concisão forçada
- **Item 5 (Multiple Unpacking):** `first, *rest = items` em vez de indexação manual
- **Item 8 (Assignment Expressions / walrus operator):** `if value := get_value():` — reduz repetição sem sacrificar clareza
**Conexão:** Clean Code Fundamentals (Hock) — mesmo argumento, perspectiva mais Python-específica.

### Cap. 2 — Strings and Slicing (Items 10–16)
**Itens mais importantes:**
- **Item 10 (bytes vs str):** Em Python 3, `bytes` e `str` são tipos distintos e não intercambiáveis — fonte comum de bugs em APIs
- **Item 11 (f-strings):** f-strings > `%` formatting > `.format()` — mais legíveis e mais rápidos
- **Item 14 (Slicing):** `items[2:5]` — nunca use índices negativos e positivos juntos sem cautela

### Cap. 3 — Loops and Iterators (Items 17–24)
**Itens mais importantes:**
- **Item 17 (enumerate):** `for i, item in enumerate(items)` em vez de `range(len(items))`
- **Item 18 (zip):** Para iterar dois iterables em paralelo
- **Item 19 (Avoid else after loops):** O `else` de um `for` loop tem semântica contraintuitiva — evite
- **Item 24 (itertools):** `itertools.chain`, `itertools.islice`, `itertools.groupby` — conheça o módulo

### Cap. 4 — Dictionaries (Items 25–29)
**Itens mais importantes:**
- **Item 25 (dict ordering):** Desde Python 3.7, dicts são ordered por insertion — dependa disso com cuidado
- **Item 26 (dict.get):** `dict.get(key, default)` > `if key in dict: ... else: ...`
- **Item 27 (defaultdict):** Para dicts com valores que precisam de inicialização automática
- **Item 29 (Compose Classes):** Quando dicts aninhados se tornam complexos, crie dataclasses

### Cap. 5 — Functions (Items 30–39)
**Itens mais importantes:**
- **Item 32 (Raise não Return None):** Uma função que retorna `None` para indicar erro força o chamador a checar — prefira raise
- **Item 36 (None e Docstrings para defaults dinâmicos):** Nunca use objetos mutáveis como default — `def f(items=[])` é um bug clássico
- **Item 37 (keyword-only e position-only):** `def create_user(*, name: str)` força uso de keyword argument
- **Item 38 (functools.wraps):** Preserva metadados ao escrever decorators
**Frase marcante:** "Prefer Raising Exceptions to Returning None." — Item 32
**Conexão:** Clean Code Fundamentals (Hock) — tratamento de erros como responsabilidade explícita.

### Cap. 6 — Comprehensions and Generators (Items 40–47)
**Itens mais importantes:**
- **Item 40 (Comprehensions):** `[x*2 for x in items]` > `list(map(lambda x: x*2, items))`
- **Item 41 (Max 2 subexpressions):** List comprehensions com mais de 2 condições viram ilegíveis — use loop
- **Item 43 (Generators):** Para sequências grandes — gera sob demanda, não armazena tudo em memória
- **Item 45 (yield from):** Para compor generators sem boilerplate
**Conexão:** Fluent Python (Ramalho) — Cap. 17 aprofunda o modelo de generators.

### Cap. 7 — Classes and Interfaces (Items 48–57)
**Itens mais importantes:**
- **Item 51 (dataclasses):** Para classes que são principalmente containers de dados — mais conciso que `__init__` manual
- **Item 55 (Public vs Private):** Em Python, `_name` é convenção, não enforcement — prefira público com documentação clara
- **Item 57 (collections.abc):** Para criar tipos customizados compatíveis com o protocolo Python

### Cap. 9 — Concurrency and Parallelism (Items 67–79)
**Itens mais importantes:**
- **Item 68 (Threads para I/O):** Threads em Python são úteis para I/O bloqueante, mas não para CPU (GIL)
- **Item 75 (Coroutines):** `async/await` para I/O altamente concorrente
- **Item 76 (asyncio migration):** Como migrar código threaded para asyncio
**Conexão:** Python Concurrency with asyncio (Fowler) — deep dive em asyncio.

### Cap. 10 — Robustness (Items 80–91)
**Itens mais importantes:**
- **Item 80 (try/except/else/finally):** `else` executa quando não houve exceção — use para lógica que depende do sucesso do try
- **Item 82 (contextlib):** Context managers com `@contextmanager` para cleanup garantido
- **Item 85 (Never catch Exception):** Pegue exceções específicas — `except Exception` esconde bugs

### Cap. 11 — Performance (Items 92–99)
**Item 92 (Profile First):** Nunca otimize sem medir. Use `cProfile` ou `line_profiler`.

## Tensões com Outros Livros
- **vs. Fluent Python (Ramalho):** Slatkin é prescritivo ("faça assim"); Ramalho é explicativo ("entenda por quê"). Leia Slatkin para regras, Ramalho para fundamentos.
- **vs. Clean Code Fundamentals:** Ambos falam sobre naming e funções, mas Slatkin é Python-específico e Hock é agnóstico de linguagem.
- **vs. Python Concurrency with asyncio:** Items 67–79 de Slatkin são uma introdução; Fowler é o livro completo sobre o tema.

## Aplicação em Python Moderno
```python
# Item 32: Raise, não Return None
# ❌
def get_user(user_id: int) -> User | None:
    user = db.query(User).get(user_id)
    if not user:
        return None  # força o chamador a checar
    return user

# ✅ Em FastAPI, raise HTTPException é o padrão correto
def get_user_or_404(user_id: int) -> User:
    user = db.query(User).get(user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user

# Item 36: Default mutável é bug
# ❌
def add_tag(tags=[], new_tag=""):
    tags.append(new_tag)
    return tags  # mesmo objeto em todas as chamadas!

# ✅
def add_tag(tags: list | None = None, new_tag: str = "") -> list:
    if tags is None:
        tags = []
    return tags + [new_tag]

# Item 37: Keyword-only arguments em FastAPI services
def create_order(*, user_id: int, product_id: int, quantity: int) -> Order:
    ...  # impossível chamar com args posicionais — mais seguro
```
