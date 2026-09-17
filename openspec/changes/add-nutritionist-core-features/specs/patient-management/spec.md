# Delta da Especificação

## Purpose

Permitir que um nutricionista cadastre e gerencie os pacientes sob seus próprios cuidados, mantendo as informações básicas e de contato de cada paciente atualizadas e pesquisáveis.

## ADDED Requirements

### Requirement: Cadastro de Paciente
O sistema DEVE permitir que um nutricionista autenticado cadastre um novo paciente informando, no mínimo, nome completo, data de nascimento e sexo, associando o paciente a esse nutricionista.

#### Scenario: Cadastro de paciente bem-sucedido
- **WHEN** um nutricionista autenticado envia o nome completo, data de nascimento e sexo de um novo paciente
- **THEN** o sistema cria o registro do paciente vinculado a esse nutricionista e confirma a criação

#### Scenario: Campo obrigatório ausente rejeitado
- **WHEN** um nutricionista autenticado envia um novo paciente sem um campo obrigatório (nome completo, data de nascimento ou sexo)
- **THEN** o sistema rejeita a solicitação e indica qual campo está ausente

### Requirement: Listagem e Visualização de Pacientes
O sistema DEVE permitir que um nutricionista autenticado liste e visualize os detalhes dos pacientes que possui.

#### Scenario: Listar pacientes ativos próprios
- **WHEN** um nutricionista autenticado solicita sua lista de pacientes
- **THEN** o sistema retorna apenas os pacientes ativos pertencentes a esse nutricionista

#### Scenario: Visualizar detalhe do paciente
- **WHEN** um nutricionista autenticado solicita o detalhe de um paciente que possui
- **THEN** o sistema retorna as informações cadastradas desse paciente

### Requirement: Atualização de Paciente
O sistema DEVE permitir que um nutricionista autenticado atualize as informações cadastradas de um paciente que possui.

#### Scenario: Atualização bem-sucedida
- **WHEN** um nutricionista autenticado envia informações atualizadas para um paciente que possui
- **THEN** o sistema salva as informações atualizadas e retorna o registro do paciente atualizado

### Requirement: Desativação de Paciente
O sistema DEVE permitir que um nutricionista autenticado desative um paciente que possui sem excluir os dados históricos do paciente, e DEVE excluir pacientes desativados da lista padrão de pacientes ativos.

#### Scenario: Desativar um paciente
- **WHEN** um nutricionista autenticado desativa um paciente que possui
- **THEN** o sistema marca o paciente como inativo, remove-o da lista padrão de pacientes ativos e preserva os planos alimentares e o histórico de avaliação física do paciente

#### Scenario: Reativar um paciente
- **WHEN** um nutricionista autenticado reativa um paciente previamente desativado que possui
- **THEN** o sistema marca o paciente como ativo novamente e o inclui na lista padrão de pacientes ativos
