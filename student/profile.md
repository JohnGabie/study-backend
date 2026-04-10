# Perfil do Aluno

## Diagnóstico Inicial (2026-04-01)
- Prova avançada: 4.7/25 (19%) — estado zero, antes de qualquer estudo
- Prova média: 8.5/25 (34%)
- Prova fácil: 15.4/25 (62%)
- Relatório completo: `quizzes/prova_geral_2026-03-31_relatorio.md`

## Contexto de Experiência
- **FastAPI:** ~2-3 meses, curso básico + projeto vibecoded com IA
- **Python:** uso prático sem estudo formal. Modelo mental presente, vocabulário lacunar
- **SQL:** uso superficial. SELECT/WHERE funciona, agregações e JOINs complexos não

## Pontos Fortes
- Intuição de acoplamento: sente que código está errado antes de saber nomear
- Honestidade sobre lacunas: não inventa respostas
- Raciocínio pragmático e por dedução — chega em conceitos corretos sem ter lido
- HTTP semântico: URI nomeia recurso, método nomeia ação — chegou por raciocínio próprio
- Separação de lógica: "API é apenas access point" — sem ter lido o livro
- Raciocínio de segurança: propôs token binding sem saber o nome (2026-04-10)

## Lacunas Ativas

### 🔴 Conceito errado — corrigir ativamente
- **async/await:** acredita que `time.sleep` em `async def` pausa só aquela função. Errado — trava o event loop inteiro.
- **FLOAT para dinheiro:** sugere DOUBLE como fix. Errado — problema é imprecisão de ponto flutuante. Correto: NUMERIC/DECIMAL.
- **User enumeration:** diagnosticou como SQL injection. O problema real é user enumeration.
- **REST URIs:** ainda usa verbos CRUD na URI. Fixar: URI nomeia recurso, método nomeia ação.
- **Tipos Python:** disse `double` e `char` — são tipos de Java/C. Python tem `float` e `str`.

### 🟠 Vocabulário ausente, intuição presente
- N+1 problem, IDOR, Default mutável

### 🟡 Gaps de fundamento (não estudados ainda)
- HTTP: estrutura de mensagem, status codes, evolução HTTP/1→3
- DRY, TDD, 4 Golden Signals, Design by Contract
- SQL: índices, timezone
- Estrutura de testes com pytest

## Padrões de Raciocínio
- Intuitivo: sente problemas antes de nomear
- Pragmático: resolve problemas, não decora teoria
- Honesto: não blefa quando não sabe
- Curiosidade lateral: vai fundo em tangentes — redirecionar com propósito, não cortar

## Discordâncias e Posições Próprias
- (nenhuma registrada ainda)

## Sessões
- [2026-04-01](sessions/2026-04-01.md) — Setup + Provas diagnósticas
- [2026-04-10](sessions/2026-04-10.md) — HTTP: fundamentos e segurança de cookies

## Exemplos Salvos
- (nenhum ainda)
