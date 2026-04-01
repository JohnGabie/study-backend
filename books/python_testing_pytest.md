# Python Testing with pytest — Brian Okken (2022, 2ª ed.)

## Tese Central
pytest é o melhor framework de testes para Python — mais simples, mais poderoso e mais idiomático que unittest. Escrever bons testes com pytest não é sobre quantidade de testes, é sobre clareza de intenção e velocidade de feedback.

## Por que Este Livro Importa
Saber que deve escrever testes é diferente de saber *como* escrevê-los bem. Este livro é o guia prático completo do pytest — fixtures, parametrize, markers, plugins — tudo que você precisa para construir uma suíte de testes que realmente ajuda em vez de atrapalhar.

## Onde o Livro Pode Ser Questionado
- Foco em pytest puro — integração com FastAPI, asyncio e SQLAlchemy exige plugins adicionais não cobertos em profundidade
- A abordagem de "testing the right things" é mencionada mas poderia ser mais desenvolvida
- `unittest` ainda é usado em muitos projetos — o livro não cobre coexistência nem migração
- Exemplos não usam type hints — código moderno Python usa Pydantic e type annotations extensivamente

## Frases-Chave do Autor
> "pytest allows you to write tests that are both simple and expressive." — Prefácio

> "Fixtures are the primary mechanism for test setup and teardown in pytest." — Cap. 3

## Mapa por Capítulo

### Part I — Primary Power

#### Cap. 1 — Getting Started with pytest (p. 3–10)
**Conceito central:** Como pytest descobre e executa testes.
**Regras práticas:**
- pytest descobre arquivos `test_*.py` ou `*_test.py`
- Funções que começam com `test_` são testes
- `pytest` na linha de comando roda todos os testes no diretório atual
- `pytest -v` para verbose, `pytest -x` para parar no primeiro erro, `pytest -k "keyword"` para filtrar

#### Cap. 2 — Writing Test Functions (p. 11–)
**Conceito central:** Testes como documentação executável — o nome do teste descreve o comportamento esperado.
**Regras práticas:**
- Estrutura AAA: **Arrange** (setup), **Act** (executa a operação), **Assert** (verifica o resultado)
- Um teste verifica uma coisa — se você precisa de múltiplos asserts, considere múltiplos testes
- Nomes de teste descritivos: `test_create_order_returns_201_when_valid_data()`
- `pytest.raises(ExceptionType)` para testar exceções esperadas:
```python
def test_get_user_raises_404_when_not_found():
    with pytest.raises(HTTPException) as exc_info:
        get_user_or_404(user_id=99999)
    assert exc_info.value.status_code == 404
```
**Frase marcante:** "Tests as specification." — testes descrevem o contrato do código.
**Conexão:** TDD with Python (Percival) — o mesmo princípio de testes como design.

#### Cap. 3 — pytest Fixtures (p. 31–)
**Conceito central:** Fixtures como mecanismo de injeção de dependência para testes.
**Regras práticas:**
- `@pytest.fixture` define uma fixture
- Fixtures são injetadas por nome nos parâmetros da função de teste
- `yield` em uma fixture executa teardown após o teste
- Scopes: `function` (default), `class`, `module`, `session` — controla quando a fixture é recriada
```python
@pytest.fixture(scope="function")
def db_session():
    session = create_test_session()
    yield session
    session.rollback()  # teardown
    session.close()
```
**Frase marcante:** "Fixtures are the primary mechanism for test setup and teardown." — Cap. 3
**Conexão:** TDD with Python (Percival) — `conftest.py` para fixtures compartilhadas.

#### Cap. 4 — Builtin Fixtures (p. 69–)
**Conceito central:** Fixtures que pytest oferece gratuitamente.
**As mais úteis:**
- `tmp_path` — diretório temporário para arquivos de teste (path como `pathlib.Path`)
- `capsys` — captura stdout/stderr
- `monkeypatch` — substitui valores, funções, variáveis de ambiente temporariamente
- `caplog` — captura log messages

```python
def test_uses_env_variable(monkeypatch):
    monkeypatch.setenv("DATABASE_URL", "sqlite:///:memory:")
    # agora a função que lê DATABASE_URL verá o valor fake
```

#### Cap. 5 — Parametrize (p. 87–)
**Conceito central:** Rodar o mesmo teste com múltiplos inputs.
**Regras práticas:**
```python
@pytest.mark.parametrize("input,expected", [
    (1, "one"),
    (2, "two"),
    (3, "three"),
])
def test_number_to_word(input, expected):
    assert number_to_word(input) == expected
```
Cada par gera um teste separado — mais eficiente que múltiplas funções de teste similares.
**Conexão:** Effective Python (Slatkin) — testes parametrizados são melhores que múltipla escolha.

#### Cap. 6 — Markers (p. 103–)
**Conceito central:** Markers para categorizar e filtrar testes.
**Regras práticas:**
- `@pytest.mark.slow` — marque testes lentos
- `@pytest.mark.skip(reason="...")` — pule um teste
- `@pytest.mark.skipif(condition, reason="...")` — pule condicionalmente
- `@pytest.mark.xfail` — teste esperado falhar (útil para bugs conhecidos)
- Custom markers em `pytest.ini` ou `pyproject.toml`:
```ini
[pytest]
markers =
    integration: marks integration tests
    unit: marks unit tests
```
- `pytest -m "not integration"` para rodar apenas unit tests em CI rápido

### Part II — Working with Projects

#### Cap. 7 — Strategy (p. 121–)
**Conceito central:** Como organizar testes em projetos reais.
**Regras práticas:**
- Estrutura de diretório:
```
project/
├── src/
│   └── myapp/
├── tests/
│   ├── conftest.py         # fixtures compartilhadas
│   ├── unit/
│   │   └── test_services.py
│   ├── integration/
│   │   └── test_endpoints.py
│   └── functional/
│       └── test_scenarios.py
```
- `conftest.py` em cada diretório para fixtures locais

#### Cap. 8 — Configuration (p. 141–)
**Conceito central:** `pyproject.toml` ou `pytest.ini` para configurar pytest.
**Configuração útil:**
```toml
[tool.pytest.ini_options]
testpaths = ["tests"]
asyncio_mode = "auto"  # para pytest-asyncio
markers = ["slow", "integration", "unit"]
```

### Part III — Booster Rockets

#### Cap. 9 — Coverage (p. 159–)
**Conceito central:** `pytest-cov` para medir cobertura de código.
**Regras práticas:**
- `pytest --cov=src --cov-report=html` — gera relatório HTML
- Coverage não é métrica de qualidade — 100% coverage com testes fracos é pior que 70% com testes fortes
- Use coverage para identificar *caminhos não testados*, não como objetivo em si
**Onde questionar:** Muitas equipes têm metas arbitrárias de cobertura (ex: "80%") — o que importa é que os comportamentos críticos estão cobertos.

#### Cap. 10 — Mock e Patch (p. 175–)
**Conceito central:** `pytest-mock` e `unittest.mock` para substituir dependências.
**Regras práticas:**
- `mocker.patch("module.ClassName")` para substituir uma classe
- `mocker.patch.object(instance, "method_name")` para substituir um método
- Verifique que o mock foi chamado com `mock.assert_called_once_with(...)`
**Onde questionar:** Slatkin (Item 109) alerta para excesso de mocks — prefira integration tests com banco real quando possível.

#### Cap. 11 — tox e CI (p. 195–)
**Conceito central:** Rodar testes em múltiplos ambientes.
**Para o ZionHub:** GitHub Actions com pytest.

### Plugins Essenciais
- **pytest-asyncio:** testes de funções `async def`
- **pytest-cov:** coverage
- **pytest-mock:** mocking simplificado
- **httpx + pytest-httpx:** para testar APIs FastAPI
- **factory-boy:** para criar objetos de teste (alternativa a fixtures manuais)
- **faker:** para gerar dados fake realistas

## Tensões com Outros Livros
- **vs. TDD with Python (Percival):** Percival ensina o *porquê* de testar; Okken ensina o *como* usar pytest. Leia Percival para mentalidade, Okken para técnica.
- **vs. Effective Python (Slatkin):** Item 109 (Integration > Unit) e Item 111 (Mocks) são posições que Okken aborda pragmaticamente — sem dogma.

## Aplicação em Python Moderno (FastAPI + pytest-asyncio)
```python
# pyproject.toml
[tool.pytest.ini_options]
asyncio_mode = "auto"
testpaths = ["tests"]

# conftest.py
import pytest
from httpx import AsyncClient
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from app.main import app
from app.models import Base

DATABASE_URL = "postgresql+asyncpg://test:test@localhost/test_db"

@pytest.fixture(scope="session")
async def engine():
    engine = create_async_engine(DATABASE_URL)
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.create_all)
    yield engine
    async with engine.begin() as conn:
        await conn.run_sync(Base.metadata.drop_all)
    await engine.dispose()

@pytest.fixture
async def db(engine):
    async with AsyncSession(engine) as session:
        yield session
        await session.rollback()

@pytest.fixture
async def client():
    async with AsyncClient(app=app, base_url="http://test") as c:
        yield c

# test_orders.py
class TestCreateOrder:
    @pytest.mark.parametrize("quantity,expected_status", [
        (1, 201),
        (0, 422),    # quantity 0 é inválido
        (-1, 422),   # negativo é inválido
    ])
    async def test_create_order_quantity_validation(
        self, client, quantity, expected_status
    ):
        response = await client.post(
            "/orders",
            json={"user_id": 1, "product_id": 1, "quantity": quantity}
        )
        assert response.status_code == expected_status
```
