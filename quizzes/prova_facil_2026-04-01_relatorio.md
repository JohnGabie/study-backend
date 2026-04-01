# Relatório de Correção — Prova 3 (Fácil)
**Data:** 2026-04-01
**Score:** 15.4 / 25 (62%)
**Prova 2 (médio):** 8.5/25 (34%)
**Prova 1 (avançado):** 4.7/25 (19%)
**Evolução total no dia:** 4.7 → 8.5 → 15.4

---

## Pontuação por Questão

| Q | Tema | Score | Observação |
|---|------|-------|------------|
| 1 | Tipos de dados Python | 0.5 | `int` e `str` corretos. `double` e `char` são Java/C — Python não tem esses tipos. Corretos: `float`, `bool`, `list`, `dict` |
| 2 | Lista vs Tupla | 0.6 | Captou o essencial. Faltou a palavra-chave: tupla é **imutável** |
| 3 | `//` e `%` | 0.4 | Identificou as operações mas não calculou. 10//3=3, 10%3=1 |
| 4 | Função | 0.9 | Correto com exemplo. Quase perfeito |
| 5 | TypeError: str + int | 0.2 | Não identificou o erro. Escreveu uma f-string com sintaxe inválida |
| 6 | Dicionário | 0.5 | "Como se fosse múltiplas listas" — analogia imprecisa. Dicionário é chave→valor |
| 7 | KeyError | 0.5 | "Vai dar erro?" — correto. "ou cria o valor" — errado |
| 8 | Loop com condicional | 1.0 | Perfeito |
| 9 | return vs print | 0.5 | Print correto. Return confuso — "guarda na memória" não é a distinção central |
| 10 | None | 0.6 | "null do JS" — analogia correta. Quando retorna None? Não soube dizer |
| 11 | O que é API | 0.9 | Excelente. Deu exemplos além de web — Windows, carros |
| 12 | DNS + HTTP | 0.7 | DNS correto. Faltou: depois do IP, o browser faz o request HTTP |
| 13 | Partes de uma URL | 0.4 | Não identificou as partes: protocolo, domínio, path, query string |
| 14 | Cliente vs Servidor | 0.8 | Correto e claro |
| 15 | Status codes 200/404 | 0.6 | Correto mas sem situações de uso |
| 16 | Endpoint FastAPI | 0.6 | Correto mas vago — não disse JSON nem status 200 |
| 17 | JSON | 0.4 | "Arquivo estruturado com regras de syntax" — correto. Não respondeu por que APIs usam |
| 18 | Requisição vs Resposta | 0.6 | Correto e simples |
| 19 | Banco de dados | 0.8 | Correto. "Seguro e constante no disco" — boa intuição |
| 20 | SELECT * FROM users | 0.6 | "Seleciona tudo do usuário" — impreciso. Seleciona **todos os registros** da tabela |
| 21 | SELECT com WHERE | 0.9 | Quase perfeito |
| 22 | INSERT | 0 | Não sabe escrever INSERT |
| 23 | WHERE | 0.8 | "É um filtro" — correto |
| 24 | Tabela/Coluna/Linha | 1.0 | **Melhor resposta da prova.** Claro, correto, com exemplo |
| 25 | COUNT com WHERE | 0.6 | "Seleciona todas as compras" — não viu o COUNT. Retorna o número, não as compras |

**Total: 15.4/25 (62%)**

---

## O que os dados revelam

### Evolução no dia: 19% → 34% → 62%

Cada prova subiu ~15 pontos percentuais. Isso é calibragem de nível funcionando —
o aluno tem uma base que existia antes de qualquer estudo formal.

O gap entre 62% (fácil) e 34% (médio) é grande. Isso é normal:
fundamentos Python são muito mais sólidos do que conceitos avançados.

---

### Pontos fortes revelados nesta prova

**Q8 e Q24 — Notas máximas**

Q8 (loop com condicional): "Checa 1 a 5 se é par. Os pares são impressos."
Lógica de programação clara e direta.

Q24 (estrutura do banco): A melhor resposta da prova.
> "Banco é tudo, tabela é uma 'página' desse banco, uma entidade.
> Linhas = cada usuário. Colunas = atributos que esse usuário teria."

Modelo mental correto sobre dados relacionais — e ainda deu o exemplo de `produto`.
Isso vai facilitar muito quando chegar em JOINs e FKs.

**Q11 (API):** Expandiu para além do contexto web espontaneamente — Windows, carros, controles remotos.
Esse é o conceito correto: API é qualquer contrato entre sistemas, não só web.

**Q12 (DNS):** Sabe que existe DNS e que ele resolve IP. Isso não é trivial.

---

### Lacunas desta prova

**Q1 — Python não tem `double` nem `char`**
Esses são tipos de Java, C, C++. Em Python:
- `double` → `float`
- `char` → não existe. Uma string de um caractere é só `str`

Python é dinamicamente tipado — o tipo é inferido pelo valor, não declarado.

**Q5 — Não identificou o TypeError**
O erro do código original é:
```python
print("..." + idade + "...")
# TypeError: can only concatenate str (not "int") to str
```
Você não pode concatenar `str` com `int` usando `+` em Python.
As soluções são:
```python
print(f"Meu nome é {nome} e tenho {idade} anos")   # f-string
print("Meu nome é " + nome + " e tenho " + str(idade) + " anos")  # conversão explícita
```

**Q9 — return vs print — distinção central não nomeada**
"Return guarda na memória" está impreciso. O ponto central é:
- `print` só exibe no terminal — o valor some depois
- `return` devolve o valor para quem chamou a função — pode ser usado, armazenado, passado adiante

```python
resultado = dobrar(5)   # return: posso usar resultado depois
mostrar(5)              # print: só aparece no terminal, não posso usar o valor
```

**Q22 — INSERT SQL**
Zero em algo que é sintaxe básica — e o aluno reconheceu isso honestamente.
```sql
INSERT INTO users (name, email) VALUES ('João', 'joao@email.com');
```

**Q25 — COUNT(*)**
"Seleciona todas as compras do usuário 42" — não viu o `COUNT`.
`COUNT(*)` não retorna as linhas — retorna **um número**: quantas linhas existem.
A query retorna algo como `{"count": 7}`, não as compras.

---

## Mapa de diagnóstico completo — fim das 3 provas

| Nível | Score | % |
|-------|-------|---|
| Prova 1 — Avançado | 4.7/25 | 19% |
| Prova 2 — Médio | 8.5/25 | 34% |
| Prova 3 — Fácil | 15.4/25 | 62% |

### Onde o aluno está hoje (estado zero):

**Sabe bem:**
- Estrutura relacional de banco (tabela/coluna/linha)
- Lógica de programação básica (loops, condicionais)
- Conceito de API além de web
- Semântica HTTP: GET vs DELETE (Q11 da Prova 2 — nota máxima)
- Separação de responsabilidades ("lógica não deve estar na API")
- DNS em alto nível

**Sabe parcialmente (intuição presente, vocabulário ausente):**
- Tipos Python: acertou int/str, errou double/char (contaminação de outras linguagens)
- Tupla vs lista: tem a noção, falta o termo "imutável"
- return vs print: sabe o efeito, não sabe articular a distinção
- None: analogia com JS correta, não sabe quando função retorna None
- JSON: sabe o que é, não sabe por que APIs usam

**Não sabe ainda:**
- INSERT SQL
- COUNT(*) vs SELECT *
- Partes de uma URL (protocolo, domínio, path, query string)
- *args / **kwargs
- == vs is
- Idempotência
- NUMERIC vs FLOAT para dinheiro
- User enumeration em auth
- REST URIs sem verbo

---

## Próximos passos

Com o mapa de diagnóstico completo, a ordem de estudo recomendada:

| Prioridade | Tema | Por quê |
|------------|------|---------|
| 1 | **Tipos Python corretamente** | `float` não é `double`, não existe `char` — base para tudo |
| 2 | **return vs print** | Distinção que aparece em 100% do código Python |
| 3 | **SQL: INSERT, UPDATE, DELETE** | Só sabe SELECT — metade do CRUD está em branco |
| 4 | **Pydantic + 422** | FastAPI depende disso em todo endpoint |
| 5 | **Depends: injeção de dependência** | Padrão central do FastAPI |
| 6 | **NUMERIC vs FLOAT** | Conceito errado ativo para dinheiro |
| 7 | **REST URIs** | Reproduziu o antipadrão — precisa fixar |
