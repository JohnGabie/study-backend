# Relatório de Correção — Prova 4 (Código)
**Data:** 2026-04-01
**Formato:** 3 questões práticas — escrever código, não explicar

---

## Resultado

| Q | Tema | Resultado |
|---|------|-----------|
| 1 | Python: dicts + loop | Travou na sintaxe. Modelo mental presente, não chegou à solução |
| 2 | FastAPI: IDOR + 404 | Não tentou — pouca vivência com FastAPI real |
| 3 | SQL: JOIN + GROUP BY + AVG | Não tentou — SQL nunca estudado a fundo |

---

## O que essa prova revelou

### Q1 — O gap é de sintaxe, não de lógica

O aluno descreveu uma solução válida em pseudocódigo (duas passagens: listar IDs, depois somar).
A intuição estava certa. O que faltou foi conhecer o padrão `if key not in dict` para acumular valores.

Isso é diferente de não entender o problema — é não ter vocabulário Python suficiente para executar o que a cabeça já viu.

**Dado:** modelo mental de estruturas de dados presente. Precisa de prática de escrita, não de conceito.

### Q2 — FastAPI via curso básico + vibecoding

O aluno tem 2–3 meses de FastAPI, fez um curso básico e depois codou um projeto com IA.
Nunca escreveu endpoint com verificação de segurança (IDOR) do zero.

Sabe reconhecer o problema quando vê (Q11 da Prova 2 — nota máxima).
Não sabe implementar a solução.

**Dado:** gap entre reconhecimento e implementação — clássico de quem aprendeu via vibecoding.

### Q3 — SQL superficial

SQL foi sempre uso superficial — SELECT, WHERE, nunca joins complexos com agregação.
Nunca estudou banco a fundo.

**Dado:** precisa de fundamento SQL do zero, não de revisão.

---

## Contexto do aluno — consolidado após 4 provas

- ~2-3 meses de FastAPI (curso básico + projeto vibecoded)
- Python: sabe o básico, vocabulário incompleto, nunca estudou formalmente
- SQL: uso superficial, sem estudo estruturado
- Aprendizado via fazer + IA, não via leitura de livros ou cursos formais
