# Fluent Python: Clear, Concise, and Effective Programming — Luciano Ramalho (2022, 2ª ed.)

## Tese Central
Python tem um modelo de dados coerente e poderoso — e a maioria dos desenvolvedores usa apenas a superfície. Programar fluentemente em Python significa entender o protocolo de dados subjacente (dunder methods, descriptors, metaclasses) para escrever código que se comporta como parte do ecossistema, não contra ele.

## Por que Este Livro Importa
É possível escrever Python que funciona sem entender o Python Data Model. Mas código escrito sem esse entendimento é verboso onde deveria ser conciso, reinventa o que já existe, e surpreende negativamente quando o comportamento padrão não é o esperado. Este é o livro que transforma um programador em Python num programador *de* Python.

## Onde o Livro Pode Ser Questionado
- O livro é denso e pressupõe familiaridade com Python — não é para iniciantes
- Alguns padrões (metaclasses, descriptors avançados) raramente são necessários em código de aplicação — são mais úteis para autores de frameworks
- A ênfase em protocols e duck typing pode conflitar com a preferência moderna por type hints explícitos (mypy, Pydantic)
- Exemplos são acadêmicos — a tradução para FastAPI/asyncio exige trabalho do leitor

## Frases-Chave do Autor
> "Python is a multi-paradigm language. Rather than forcing programmers to adopt a single style, it embraces a variety of paradigms." — Preface

> "The Python Data Model is the key to writing Pythonic code." — Cap. 1

## Mapa por Capítulo

### Part I — Data Structures

#### Cap. 1 — The Python Data Model (p. 3–19)
**Conceito central:** Os dunder methods (`__len__`, `__getitem__`, `__repr__`, etc.) são o contrato que permite que objetos customizados se comportem como objetos nativos do Python.
**Regras práticas:** Implemente `__repr__` para debugging. Implemente `__len__` e `__getitem__` para que seu objeto funcione com `len()`, `for`, e slicing. O Python framework chama os dunder methods — você não chama diretamente.
**Exemplo do autor:** `FrenchDeck` — uma classe de baralho que implementa `__len__` e `__getitem__`, automaticamente ganhando suporte para `len()`, iteração, `random.choice()` e `in`.
**Frase marcante:** "By implementing special methods, your objects can behave like built-ins." — p. 5
**Conexão:** Effective Python (Slatkin) — Item 58: Use Plain Attributes, Item 60: Descriptors.

#### Cap. 2 — An Array of Sequences (p. 21–71)
**Conceito central:** Sequências em Python compartilham um protocolo — qualquer objeto que implemente `__len__` e `__getitem__` é uma sequência.
**Regras práticas:** List comprehensions > `map`/`filter` para legibilidade. Prefira `tuple` para dados imutáveis. Use `array.array` para sequências numéricas grandes (mais eficiente que list). `deque` para queues (O(1) em ambos os extremos).
**Frase marcante:** Unpacking e pattern matching como idiomas Python — não como workarounds.
**Conexão:** Effective Python — Item 5: Multiple Assignment Unpacking, Item 16: Catch-All Unpacking.

#### Cap. 3 — Dictionaries and Sets (p. 77–110)
**Conceito central:** Dicts em Python 3.7+ são ordered. Sets são dicts sem valores — mesma implementação de hash table.
**Regras práticas:** `dict.get(key, default)` > `key in dict`. `defaultdict` para defaults automáticos. `Counter` para contagem.
**Conexão:** Effective Python — Items 25–28 sobre dicionários.

### Part II — Functions as Objects

#### Cap. 7 — Functions as First-Class Objects (p. 185–213)
**Conceito central:** Funções em Python são objetos — podem ser atribuídas a variáveis, passadas como argumentos, retornadas de outras funções.
**Regras práticas:** Higher-order functions (`map`, `filter`, `functools.reduce`). `operator` module para funções como objetos. `functools.partial` para aplicação parcial.
**Conexão:** Effective Python — Item 39: `functools.partial` Over lambda.

#### Cap. 9 — Decorators and Closures (p. 255–295)
**Conceito central:** Decorators são funções que recebem uma função e retornam outra — usado extensivamente em FastAPI (`@router.get`, `@router.post`).
**Regras práticas:** Use `functools.wraps` para preservar metadados da função decorada. Closures capturam variáveis do escopo externo — cuidado com mutação.
**Exemplo do autor:** Um decorator de clock para medir tempo de execução.
**Frase marcante:** "A decorator is a callable that takes another callable as argument." — p. 256
**Conexão:** Effective Python — Item 38: `functools.wraps`.

### Part III — Classes and Protocols

#### Cap. 11 — A Pythonic Object (p. 339–381)
**Conceito central:** Como implementar objetos que se comportam como objetos Python nativos.
**Regras práticas:** `__repr__` deve produzir uma string que pode recriar o objeto. `__str__` é para o usuário final. `__eq__` e `__hash__` devem ser implementados juntos.

#### Cap. 13 — Interfaces, Protocols, and ABCs (p. 413–452)
**Conceito central:** Duck typing vs. goose typing vs. static duck typing (Protocol).
**Regras práticas:** Em Python moderno, prefira `Protocol` (typing) a ABCs para definir interfaces — mais flexível e compatível com type checkers.
**Frase marcante:** "Python is strongly typed but dynamically typed." — a distinção importa.
**Conexão:** Effective Python — Item 57: `collections.abc`.

### Part IV — Control Flow

#### Cap. 17 — Iterators, Generators, and Classic Coroutines (p. 569–625)
**Conceito central:** Generators são iterators que produzem valores preguiçosamente — fundamentais para processamento de streams e para asyncio.
**Regras práticas:** `yield from` para compor generators. Generators são a base dos coroutines em asyncio.
**Conexão:** Python Concurrency with asyncio (Fowler) — asyncio é construído sobre o modelo de coroutines de Python.

#### Cap. 21 — Asynchronous Programming (p. 735–800)
**Conceito central:** `async`/`await` em Python — o modelo de concorrência cooperativa.
**Regras práticas:** `async for` e `async with` para async iterators e context managers. `asyncio.gather()` para concorrência de múltiplas coroutines.
**Conexão:** Python Concurrency with asyncio (Fowler) — deep dive no modelo asyncio.

## Tensões com Outros Livros
- **vs. Effective Python (Slatkin):** Slatkin é mais tático e orientado a regras concretas; Ramalho é mais conceitual e profundo no modelo da linguagem. Ramalho explica o *porquê* do que Slatkin recomenda.
- **vs. Python Crash Course (Matthes):** Matthes é o ponto de entrada; Ramalho é onde você vai quando já sabe Python mas quer entendê-lo de verdade.
- **vs. Python Concurrency with asyncio (Fowler):** Ramalho explica os fundamentos de generators/coroutines; Fowler aplica asyncio em sistemas reais.

## Aplicação em Python Moderno
```python
# Cap. 1: Python Data Model — objeto que se comporta como nativo
class OrderCollection:
    def __init__(self, orders: list):
        self._orders = orders

    def __len__(self) -> int:
        return len(self._orders)

    def __getitem__(self, index):
        return self._orders[index]

    def __repr__(self) -> str:
        return f"OrderCollection({self._orders!r})"

# Agora funciona com: len(), for, random.choice(), in, slicing
orders = OrderCollection([order1, order2])
print(len(orders))          # funciona
for order in orders:        # funciona
    process(order)

# Cap. 9: Decorator em FastAPI (o que @router.post faz internamente)
from functools import wraps

def require_auth(func):
    @wraps(func)
    async def wrapper(*args, **kwargs):
        # verifica auth
        return await func(*args, **kwargs)
    return wrapper

# Cap. 13: Protocol para type safety sem herança
from typing import Protocol

class Persistable(Protocol):
    def save(self) -> None: ...
    def delete(self) -> None: ...
```
