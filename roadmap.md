# Roadmap de Estudo — João
**Criado:** 2026-04-01
**Objetivo final:** Entender o ZionHub titim por titim — código, decisões, falhas e melhorias — sem depender de IA para saber o que está acontecendo.

> Este documento é vivo. Atualizado a cada sessão: fases concluídas são marcadas, novas fases são adicionadas quando fizer sentido, ordem muda se o projeto mudar.

---

## Estado atual
**Fase:** 0 — Diagnóstico concluído
**Último commit:** 2026-04-01 — trilogy de provas + prova de código

### O que o diagnóstico revelou
- Modelo mental presente: reconhece problemas, entende estrutura, não blefa
- Gap principal: vocabulário Python/SQL/FastAPI — sabe pensar, ainda não sabe escrever
- FastAPI: 2-3 meses, curso básico + vibecoding. Reconhece padrões, não implementa do zero
- SQL: uso superficial. SELECT/WHERE funciona. Agregações e JOINs complexos: zero
- Conceito errado ativo: FLOAT para dinheiro, async/event loop, user enumeration

---

## Fase 1 — Python com Intenção
**Status:** 🔜 Próxima
**Meta:** Escrever Python sem travar na sintaxe para o que a cabeça já entende

### Módulos
- [ ] Tipos de dados reais do Python (`float`, não `double`; `str`, não `char`; `bool`, `list`, `dict`)
- [ ] Funções: `return` vs `print`, `*args`, `**kwargs`, default arguments
- [ ] Estruturas de dados: dict como acumulador, list comprehension, operações em lista
- [ ] `==` vs `is`, mutabilidade, `None`
- [ ] `try/except/else/finally` na prática
- [ ] `async/await` corretamente — o event loop não é o que você pensa

### Critério de conclusão
Consegue escrever a Q1 da Prova 4 do zero, sem dica, em menos de 5 minutos.

---

## Fase 2 — SQL com Fundamento
**Status:** 🔜 Aguardando Fase 1
**Meta:** Entender e escrever queries reais — não só SELECT *

### Módulos
- [ ] INSERT, UPDATE, DELETE — CRUD completo
- [ ] JOIN: INNER, LEFT — quando usar cada um
- [ ] GROUP BY + HAVING + funções de agregação (COUNT, SUM, AVG)
- [ ] Índices: o que são, quando criar, por que importam
- [ ] Tipos de dados: NUMERIC vs FLOAT, TIMESTAMPTZ vs TIMESTAMP
- [ ] EXPLAIN ANALYZE — ler o plano de execução de uma query

### Critério de conclusão
Consegue escrever a Q3 da Prova 4 do zero, sem consultar nada.

---

## Fase 3 — FastAPI com Intenção
**Status:** 🔜 Aguardando Fase 1
**Meta:** Sair do vibecoding — escrever endpoints entendendo cada linha

### Módulos
- [ ] Pydantic: modelos, validação, 422, request vs response schema
- [ ] `Depends`: injeção de dependência, ciclo de vida da sessão do banco
- [ ] Autenticação: JWT, `get_current_user`, IDOR — como proteger endpoints
- [ ] `HTTPException` vs `return` — semântica HTTP correta
- [ ] Separação de camadas: endpoint → service → repositório
- [ ] REST URIs: recurso, não ação

### Critério de conclusão
Consegue escrever a Q2 da Prova 4 do zero, com proteção IDOR e tratamento de 404.

---

## Fase 4 — ZionHub: Code Review Real
**Status:** 🔒 Bloqueada até Fases 1-3
**Meta:** Ler o código do ZionHub e saber exatamente o que está acontecendo — o que está certo, o que está errado e por quê

### O que vai acontecer nessa fase
- Clonar o repositório do ZionHub para dentro desse projeto de estudo
- Fazer code review real: endpoint por endpoint, modelo por modelo, query por query
- Identificar falhas de segurança documentadas e entender por que são falhas
- Criar issues no Linear para cada problema identificado
- Refatorar junto — você escrevendo, não a IA

### Por que está bloqueada
Sem o vocabulário das Fases 1-3, o code review vira dependência de IA de novo.
Com o vocabulário, vira aprendizado real.

---

## Sessões

### 2026-04-01 — Sessão 1: Diagnóstico completo
- Setup do sistema (CLAUDE.md, books/, memory/, quizzes/, student/)
- 16 livros processados em MD
- Prova 1 (avançado): 4.7/25 — baseline
- Prova 2 (médio): 8.5/25
- Prova 3 (fácil): 15.4/25
- Prova 4 (código): revelou gap entre reconhecimento e escrita
- Diagnóstico fechado. Roadmap criado.
- **Próxima sessão:** iniciar Fase 1 — Python com Intenção

---

## Métricas de Evolução

| Data | Evento | Score / Resultado |
|------|--------|-------------------|
| 2026-04-01 | Prova 1 — Avançado | 4.7/25 (19%) |
| 2026-04-01 | Prova 2 — Médio | 8.5/25 (34%) |
| 2026-04-01 | Prova 3 — Fácil | 15.4/25 (62%) |
| 2026-04-01 | Prova 4 — Código | Q1: travou na sintaxe \| Q2/Q3: não tentou |
