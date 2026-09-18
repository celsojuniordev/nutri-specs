# Delta da Especificação

## ADDED Requirements

### Requirement: Destino Padrão Após Login
O sistema DEVE apresentar ao nutricionista, imediatamente após um login bem-sucedido (por e-mail/senha ou por conta Google), a listagem de seus próprios pacientes ativos, conforme definida em `patient-management` - "Listagem e Visualização de Pacientes", como conteúdo inicial da sessão autenticada.

#### Scenario: Listagem de pacientes exibida após login por e-mail/senha
- **WHEN** um nutricionista cadastrado efetua login com e-mail e senha corretos
- **THEN** o sistema concede a sessão autenticada e apresenta a listagem dos pacientes ativos desse nutricionista como conteúdo inicial

#### Scenario: Listagem de pacientes exibida após login via Google
- **WHEN** um nutricionista efetua login via conta Google com sucesso (criando uma conta nova ou entrando em uma conta existente)
- **THEN** o sistema concede a sessão autenticada e apresenta a listagem dos pacientes ativos desse nutricionista como conteúdo inicial

#### Scenario: Nutricionista sem pacientes cadastrados
- **WHEN** um nutricionista sem nenhum paciente cadastrado efetua login com sucesso
- **THEN** o sistema apresenta a listagem de pacientes vazia como conteúdo inicial, em vez de qualquer outro destino
