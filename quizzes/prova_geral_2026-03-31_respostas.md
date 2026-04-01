# Prova Geral — Respostas do João
**Data de entrega:** 2026-04-01

---

## Nível 1 — Conceito

**Q1.**
SRP não é um conceito exclusivamente de uma arquitetura. Nós podemos ver em várias, inclusive em clean architecture e POO. Basicamente diz que uma função, classe ou rota deve ter apenas uma responsabilidade e não agrupar funcionalidades.
Exemplo: uma rota de login não deve ser a mesma rota de registro.

**Q2.**
`async def` permite que funções sejam executadas de forma concorrente, sem precisar esperar outras terminarem, diferente do `def` que segue uma fila de execução.
Se usar `time.sleep(3)` dentro de um `async def`, apenas aquela função fica parada, mas outras ainda podem ser executadas.

**Q3.** Não sei.

**Q4.** Não conheço.

**Q5.** Não conheço.

**Q6.**
Acredito que seja algo como um construtor no Python. Provavelmente existem métodos como `length` e `get items` para permitir usar `len()` e `for` em uma classe.

**Q7.**
Não sei a diferença entre SQL Injection e IDOR.
Acredito que o SQLAlchemy protege contra SQL Injection abstraindo queries e evitando erros manuais.

**Q8.** Não sei.

---

## Nível 2 — Aplicação

**Q9.**
- `db.commit()` não é fechado corretamente
- Responsabilidade acoplada (salva usuário e envia e-mail na mesma função)

**Q10.**
A lista `tags` cresce a cada chamada da função, acumulando valores.

**Q11.**
Talvez a validação deveria estar antes. Pode haver problema com manipulação do `order_id`.

**Q12.**
Não sei exatamente, mas parece confuso pois testa muitas coisas ao mesmo tempo (criação, update e delete).

**Q13.**
Ele busca usuários e depois busca pedidos para cada usuário usando `for`.
Não sei explicar o problema exatamente.

**Q14.** Não conheço o Pragmatic Programmer.

**Q15.**
A tabela não tem a transação em si, apenas id, usuário, valor e data.

**Q16.** Não sei.

**Q17.** Não sei.

**Q18.** Não sei.

---

## Nível 3 — Síntese

**Q19.**
Sugestão: estudar mais, melhorar o código e usar IA como ferramenta, não como bengala.

**Q20.** Não conheço os livros, então não sei responder.

**Q21.**
- Schema: pedidos ligados ao user_id
- Query: buscar pelo ID
- Endpoint: usar ID do usuário
- Teste: verificar se IDs são iguais

**Q22.**
Features devem ser feitas rápido, mas a base (segurança, banco, backend) deve ser feita com mais cuidado e maturidade.

**Q23.** Não sei.

**Q24.**
Não sei. Olharia logs ou pediria ajuda de IA.

**Q25.**
Não sei exatamente, mas quero chegar nesse nível de avaliação.
