# Delta da Especificação

## Purpose

Permitir que um nutricionista cadastre um conjunto próprio de perguntas de anamnese, comuns a todos os seus pacientes, e registre e atualize a resposta vigente de cada paciente para essas perguntas a partir da tela de detalhe do paciente.

## ADDED Requirements

### Requirement: Cadastro de Pergunta de Anamnese
O sistema DEVE permitir que um nutricionista autenticado cadastre uma nova pergunta de anamnese informando, no mínimo, o texto da pergunta, associando a pergunta a esse nutricionista. A pergunta cadastrada DEVE estar disponível para todos os pacientes desse nutricionista, e não apenas para um paciente específico.

#### Scenario: Cadastro de pergunta bem-sucedido
- **WHEN** um nutricionista autenticado envia o texto de uma nova pergunta de anamnese
- **THEN** o sistema cria a pergunta associada a esse nutricionista, confirma a criação, e a pergunta passa a estar disponível para resposta em todos os pacientes desse nutricionista

#### Scenario: Texto da pergunta ausente rejeitado
- **WHEN** um nutricionista autenticado envia uma nova pergunta de anamnese sem o texto da pergunta
- **THEN** o sistema rejeita a solicitação e indica que o texto da pergunta é obrigatório

### Requirement: Listagem de Perguntas de Anamnese
O sistema DEVE permitir que um nutricionista autenticado liste as perguntas de anamnese ativas que cadastrou.

#### Scenario: Listar perguntas ativas próprias
- **WHEN** um nutricionista autenticado solicita sua lista de perguntas de anamnese
- **THEN** o sistema retorna apenas as perguntas ativas cadastradas por esse nutricionista

### Requirement: Atualização de Pergunta de Anamnese
O sistema DEVE permitir que um nutricionista autenticado atualize o texto de uma pergunta de anamnese que cadastrou.

#### Scenario: Atualização de pergunta bem-sucedida
- **WHEN** um nutricionista autenticado envia um novo texto para uma pergunta de anamnese que cadastrou
- **THEN** o sistema salva o texto atualizado e passa a exibi-lo para todos os pacientes desse nutricionista

### Requirement: Desativação de Pergunta de Anamnese
O sistema DEVE permitir que um nutricionista autenticado desative uma pergunta de anamnese que cadastrou sem excluir as respostas de pacientes já registradas para ela, e DEVE excluir perguntas desativadas da listagem de perguntas ativas e da tela de detalhe do paciente para novas respostas.

#### Scenario: Desativar uma pergunta
- **WHEN** um nutricionista autenticado desativa uma pergunta de anamnese que cadastrou
- **THEN** o sistema marca a pergunta como inativa, deixa de exibi-la na tela de detalhe do paciente para novas respostas, e preserva as respostas de pacientes já registradas para essa pergunta

### Requirement: Registro da Resposta de Anamnese do Paciente
O sistema DEVE permitir que, na tela de detalhe do paciente, um nutricionista autenticado registre a resposta de um paciente que possui para uma pergunta de anamnese ativa que cadastrou. Cada paciente DEVE ter no máximo uma resposta vigente por pergunta.

#### Scenario: Registro de resposta bem-sucedido
- **WHEN** um nutricionista autenticado envia, na tela de detalhe de um paciente que possui, uma resposta para uma pergunta de anamnese ativa que cadastrou e que ainda não possui resposta registrada para esse paciente
- **THEN** o sistema cria a resposta vigente daquele paciente para aquela pergunta

#### Scenario: Registro para pergunta inativa rejeitado
- **WHEN** um nutricionista autenticado tenta registrar uma resposta para uma pergunta de anamnese que está desativada
- **THEN** o sistema rejeita a solicitação

### Requirement: Atualização da Resposta de Anamnese do Paciente
O sistema DEVE permitir que um nutricionista autenticado atualize, em um atendimento posterior, a resposta vigente de um paciente que possui para uma pergunta de anamnese ativa, substituindo o valor anterior pelo novo valor informado.

#### Scenario: Atualização de resposta bem-sucedida
- **WHEN** um nutricionista autenticado envia um novo valor para a resposta já existente de um paciente que possui em uma pergunta de anamnese
- **THEN** o sistema substitui o valor anterior pelo novo valor como a resposta vigente daquele paciente para aquela pergunta, sem manter o valor anterior

### Requirement: Visualização das Respostas de Anamnese do Paciente
O sistema DEVE permitir que um nutricionista autenticado visualize, na tela de detalhe de um paciente que possui, todas as perguntas de anamnese ativas cadastradas por ele junto com a resposta vigente de cada uma, quando existir.

#### Scenario: Visualizar respostas de anamnese do paciente
- **WHEN** um nutricionista autenticado abre a tela de detalhe de um paciente que possui
- **THEN** o sistema exibe todas as perguntas de anamnese ativas cadastradas por esse nutricionista, mostrando a resposta vigente registrada para esse paciente em cada uma, ou indicando que ainda não há resposta quando não houver

### Requirement: Isolamento de Dados de Anamnese por Nutricionista
O sistema DEVE vincular toda pergunta de anamnese ao nutricionista que a cadastrou e toda resposta de anamnese ao paciente a que pertence, e DEVE permitir que um nutricionista autenticado leia ou modifique apenas perguntas e respostas de anamnese que pertençam a ele ou a pacientes que possui.

#### Scenario: Nutricionista não pode acessar pergunta de outro nutricionista
- **WHEN** um nutricionista autenticado solicita listar, atualizar, desativar ou responder uma pergunta de anamnese cadastrada por outro nutricionista
- **THEN** o sistema nega o acesso
