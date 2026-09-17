# Delta da Especificação

## Purpose

Permitir que um nutricionista prescreva um plano alimentar ("receita") para um paciente, composto por refeições e itens alimentares, e entregue esse plano ao paciente como um documento PDF exportado.

## ADDED Requirements

### Requirement: Criação de Plano Alimentar
O sistema DEVE permitir que um nutricionista autenticado crie um plano alimentar para um paciente que possui, composto por uma ou mais refeições, cada uma contendo um ou mais itens alimentares com quantidade e unidade de medida.

#### Scenario: Criação de plano alimentar bem-sucedida
- **WHEN** um nutricionista autenticado envia um novo plano alimentar para um paciente que possui, incluindo pelo menos uma refeição com pelo menos um item alimentar
- **THEN** o sistema cria o plano alimentar associado a esse paciente e confirma a criação

#### Scenario: Plano alimentar sem refeições rejeitado
- **WHEN** um nutricionista autenticado envia um novo plano alimentar sem nenhuma refeição
- **THEN** o sistema rejeita a solicitação e indica que pelo menos uma refeição é obrigatória

### Requirement: Atualização de Plano Alimentar
O sistema DEVE permitir que um nutricionista autenticado atualize um plano alimentar que criou, incluindo adicionar, editar ou remover refeições e itens alimentares.

#### Scenario: Atualização de plano alimentar bem-sucedida
- **WHEN** um nutricionista autenticado envia alterações para um plano alimentar que possui
- **THEN** o sistema salva as refeições e itens alimentares atualizados e retorna o plano alimentar atualizado

### Requirement: Histórico de Planos Alimentares por Paciente
O sistema DEVE permitir que um nutricionista autenticado visualize a lista de planos alimentares criados anteriormente para um paciente que possui, ordenados por data de criação.

#### Scenario: Visualizar histórico de planos alimentares
- **WHEN** um nutricionista autenticado solicita os planos alimentares de um paciente que possui
- **THEN** o sistema retorna todos os planos alimentares criados para esse paciente, ordenados do mais recente para o mais antigo

### Requirement: Exportação do Plano Alimentar em PDF
O sistema DEVE permitir que um nutricionista autenticado exporte um plano alimentar que possui como um documento PDF contendo o nome do paciente, o nome do nutricionista, e todas as refeições com seus itens alimentares e quantidades.

#### Scenario: Exportação em PDF bem-sucedida
- **WHEN** um nutricionista autenticado solicita a exportação em PDF de um plano alimentar que possui
- **THEN** o sistema gera um documento PDF listando o nome do paciente, o nome do nutricionista, e todas as refeições com seus itens alimentares e quantidades, e o disponibiliza para download

#### Scenario: Exportação de plano alimentar de outro nutricionista negada
- **WHEN** um nutricionista autenticado solicita a exportação em PDF de um plano alimentar pertencente a outro nutricionista
- **THEN** o sistema nega a solicitação
