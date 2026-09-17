# Design

## Contexto

Esta mudança estende o mesmo repositório de especificações greenfield (`nutri-specs`), adicionando a capacidade `anamnesis` sobre o modelo de domínio já proposto em `add-nutritionist-core-features` (Nutricionista, Paciente). Veja proposal.md - Por Que para a motivação.

A change `add-nutritionist-core-features` (que introduz `nutritionist-auth` e `patient-management`) ainda não foi aplicada/arquivada, então `openspec/specs/` ainda está vazio. Por isso, o requisito de isolamento de dados por nutricionista para perguntas e respostas de anamnese é declarado dentro da própria capacidade `anamnesis` nesta mudança, em vez de um delta MODIFIED sobre `nutritionist-auth`. Quando `add-nutritionist-core-features` for arquivada, o requisito equivalente de `nutritionist-auth` (isolamento de paciente/plano alimentar/avaliação física) e o desta capacidade devem ser revisados em conjunto para evitar duplicação de regra de negócio.

## Objetivos / Não Objetivos

**Objetivos:**
- Permitir um catálogo de perguntas de anamnese reutilizável por todos os pacientes de um nutricionista, evitando recriação por paciente.
- Garantir que cada paciente tenha, no máximo, uma resposta vigente por pergunta, atualizável em atendimentos futuros.
- Preservar respostas já registradas mesmo quando a pergunta correspondente é desativada.

**Não Objetivos:**
- Não especificar tipos de pergunta além de texto livre (numérico, sim/não, múltipla escolha) — decisão explícita do usuário para esta versão.
- Não especificar histórico de respostas anteriores — cada resposta é sobrescrita, sem trilha de auditoria, também por decisão explícita do usuário.
- Não especificar agrupamento/seções de perguntas (ex.: "hábitos alimentares", "histórico clínico") — as perguntas formam uma lista simples nesta versão.
- Não especificar exportação das respostas de anamnese em PDF (diferente de `diet-prescription`, que já tem esse requisito) — pode ser avaliado em uma mudança futura.

## Decisões

- **Perguntas pertencem ao nutricionista, não ao sistema como um todo**: cada nutricionista mantém seu próprio catálogo de perguntas, consistente com o modelo multi-tenant já estabelecido (nutricionista → paciente). Alternativa considerada: perguntas globais compartilhadas entre todos os nutricionistas do sistema — rejeitada porque quebraria o isolamento de dados por nutricionista já definido para as demais capacidades e reduziria a flexibilidade de cada nutricionista personalizar seu próprio roteiro de anamnese.
- **Resposta é um valor único e mutável por paciente/pergunta, não uma lista histórica**: modelada como um registro "paciente + pergunta → resposta vigente", atualizado in-place. Isso é deliberadamente diferente do histórico de `physical-assessment`, que mantém todas as avaliações passadas para comparação — a anamnese, conforme decisão do usuário, reflete apenas o estado mais atual do paciente.
- **Desativação em vez de exclusão para perguntas**: espelha a decisão já tomada para pacientes em `patient-management`, preservando respostas já registradas quando uma pergunta deixa de ser usada.

## Riscos / Trade-offs

- [Sem histórico de respostas, perde-se a capacidade de auditar como uma resposta mudou ao longo do tempo] → Aceito para esta versão por decisão explícita do usuário; uma mudança futura pode introduzir histórico caso a necessidade surja.
- [Perguntas por nutricionista podem gerar duplicação de catálogos semelhantes entre nutricionistas diferentes] → Aceitável nesta versão; um catálogo de perguntas sugeridas/compartilhadas pode ser proposto depois, se necessário.
- [Requisito de isolamento de dados duplicado entre esta mudança e `nutritionist-auth` até que `add-nutritionist-core-features` seja arquivada] → Mitigado ao sinalizar explicitamente essa sobreposição aqui, para revisão conjunta no momento do arquivamento.
