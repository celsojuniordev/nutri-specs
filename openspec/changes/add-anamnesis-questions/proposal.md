# Proposta

## Por Que

O nutricionista precisa coletar informações de anamnese de cada paciente (histórico de saúde, hábitos, preferências etc.) de forma padronizada, reaproveitando o mesmo conjunto de perguntas para todos os pacientes, em vez de recriar um questionário a cada atendimento. Também precisa registrar e atualizar as respostas de cada paciente ao longo dos atendimentos, sempre a partir da tela de detalhe do paciente.

## O Que Muda

- Adiciona uma tela/capacidade própria para o nutricionista cadastrar, listar, atualizar e desativar perguntas de anamnese (texto livre). As perguntas cadastradas são as mesmas para todos os pacientes daquele nutricionista — não são perguntas específicas de um paciente.
- Adiciona, na tela de detalhe do paciente, o registro da resposta de cada pergunta de anamnese ativa para aquele paciente.
- Adiciona a atualização da resposta de uma pergunta de anamnese em atendimentos seguintes: cada pergunta possui uma única resposta vigente por paciente, que é sobrescrita ao ser atualizada (o valor anterior não é mantido como histórico).

## Capacidades

### Novas Capacidades
- `anamnesis`: Cadastro, listagem, atualização e desativação de perguntas de anamnese (comuns a todos os pacientes de um nutricionista) e registro/atualização da resposta única e vigente de cada paciente para essas perguntas.

### Capacidades Modificadas
Nenhuma — a capacidade `nutritionist-auth` ainda não existe em `openspec/specs/` (a change `add-nutritionist-core-features` que a introduz ainda não foi aplicada/arquivada), então o isolamento de dados por nutricionista para perguntas e respostas de anamnese é declarado como requisito próprio dentro da nova capacidade `anamnesis` nesta mudança.

## Impacto

- Estende o modelo de domínio com Pergunta de Anamnese e Resposta de Anamnese do Paciente, vinculadas ao Nutricionista (dono das perguntas) e ao Paciente (dono das respostas), respectivamente.
- Não afeta código, API ou sistema existente — este repositório ainda é apenas de especificações (greenfield).
