# Delta da Especificação

## Purpose

Fornecer cadastro de conta e autenticação para nutricionistas, para que cada nutricionista possa acessar o sistema com segurança e ver e gerenciar apenas seus próprios pacientes e dados.

## ADDED Requirements

### Requirement: Cadastro de Nutricionista
O sistema DEVE permitir que um novo nutricionista crie uma conta informando, no mínimo, nome completo, um endereço de e-mail único e uma senha.

#### Scenario: Cadastro bem-sucedido
- **WHEN** um visitante envia um formulário de cadastro com nome completo, um e-mail ainda não utilizado e uma senha válida
- **THEN** o sistema cria a conta do nutricionista e confirma que a conta foi criada

#### Scenario: E-mail duplicado rejeitado
- **WHEN** um visitante envia um cadastro com um endereço de e-mail que já pertence a uma conta de nutricionista existente
- **THEN** o sistema rejeita o cadastro e retorna um erro indicando que o e-mail já está em uso

### Requirement: Login do Nutricionista
O sistema DEVE permitir que um nutricionista cadastrado se autentique usando seu e-mail e senha e receba uma sessão autenticada.

#### Scenario: Login bem-sucedido
- **WHEN** um nutricionista envia seu e-mail e senha corretos
- **THEN** o sistema concede uma sessão autenticada e permite acesso aos próprios dados do nutricionista

#### Scenario: Credenciais inválidas rejeitadas
- **WHEN** um nutricionista envia uma combinação de e-mail/senha que não corresponde a nenhuma conta cadastrada
- **THEN** o sistema rejeita a tentativa de login e não cria uma sessão

### Requirement: Logout do Nutricionista
O sistema DEVE permitir que um nutricionista autenticado encerre sua sessão.

#### Scenario: Logout bem-sucedido
- **WHEN** um nutricionista autenticado solicita encerrar a sessão
- **THEN** o sistema invalida a sessão atual, de forma que ela não possa mais ser usada para acessar dados

### Requirement: Isolamento de Dados por Nutricionista
O sistema DEVE vincular todo paciente, plano alimentar e avaliação física ao nutricionista que o criou, e DEVE permitir que um nutricionista autenticado leia ou modifique apenas registros que ele possui.

#### Scenario: Nutricionista não pode acessar paciente de outro nutricionista
- **WHEN** um nutricionista autenticado solicita um paciente, plano alimentar ou avaliação física pertencente a outro nutricionista
- **THEN** o sistema nega o acesso e não retorna os dados desse registro

#### Scenario: Acesso não autenticado negado
- **WHEN** uma solicitação para visualizar ou modificar um paciente, plano alimentar ou avaliação física é feita sem uma sessão autenticada válida
- **THEN** o sistema nega a solicitação
