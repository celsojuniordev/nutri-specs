# Delta da Especificação

## Purpose

Permitir que um nutricionista registre as medidas de avaliação física de um paciente ao longo do tempo e compare avaliações entre si para evidenciar a evolução do paciente.

## ADDED Requirements

### Requirement: Registro de Avaliação Física
O sistema DEVE permitir que um nutricionista autenticado registre uma avaliação física datada para um paciente que possui, capturando o peso do paciente, as sete medidas de dobras cutâneas do protocolo de Pollock (tríceps, subescapular, axilar média, peitoral/tórax, suprailíaca, abdominal e coxa), e as circunferências de segmentos corporais (braço, antebraço, tórax, cintura, abdômen, quadril, coxa e panturrilha).

#### Scenario: Registro de avaliação bem-sucedido
- **WHEN** um nutricionista autenticado envia uma nova avaliação física para um paciente que possui, incluindo a data da avaliação e o peso
- **THEN** o sistema cria o registro de avaliação com todas as medidas enviadas associado a esse paciente

#### Scenario: Peso ausente rejeitado
- **WHEN** um nutricionista autenticado envia uma nova avaliação física sem um valor de peso
- **THEN** o sistema rejeita a solicitação e indica que o peso é obrigatório

#### Scenario: Medida negativa ou zero rejeitada
- **WHEN** um nutricionista autenticado envia uma avaliação física contendo um valor negativo ou zero para peso, uma dobra cutânea ou uma circunferência
- **THEN** o sistema rejeita a solicitação e indica qual medida é inválida

### Requirement: Histórico de Avaliações por Paciente
O sistema DEVE permitir que um nutricionista autenticado visualize o histórico cronológico completo de avaliações físicas registradas para um paciente que possui.

#### Scenario: Visualizar histórico de avaliações
- **WHEN** um nutricionista autenticado solicita o histórico de avaliação física de um paciente que possui
- **THEN** o sistema retorna todas as avaliações registradas para esse paciente, ordenadas da mais antiga para a mais recente

### Requirement: Comparação de Duas Avaliações
O sistema DEVE permitir que um nutricionista autenticado selecione duas avaliações físicas do mesmo paciente e visualize a diferença entre elas para o peso, cada dobra cutânea e cada circunferência.

#### Scenario: Comparação bem-sucedida
- **WHEN** um nutricionista autenticado seleciona duas avaliações físicas pertencentes ao mesmo paciente que possui
- **THEN** o sistema retorna, para peso, cada dobra cutânea e cada circunferência, o valor de cada avaliação selecionada e a diferença entre elas

#### Scenario: Comparação entre pacientes diferentes negada
- **WHEN** um nutricionista autenticado tenta comparar duas avaliações físicas que não pertencem ao mesmo paciente
- **THEN** o sistema rejeita a solicitação
