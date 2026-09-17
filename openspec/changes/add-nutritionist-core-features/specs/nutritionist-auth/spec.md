# Delta da Especificação

## Purpose

Fornecer cadastro de conta e autenticação para nutricionistas, para que cada nutricionista possa acessar o sistema com segurança e ver e gerenciar apenas seus próprios pacientes e dados.

## ADDED Requirements

### Requirement: Cadastro de Nutricionista
O sistema DEVE permitir que um novo nutricionista crie uma conta informando, no mínimo, nome completo, um endereço de e-mail e uma senha que atenda a uma política mínima de segurança (pelo menos 8 caracteres, combinando letra e número), e, opcionalmente, o nome da empresa/clínica em que atua. O sistema DEVE tratar o e-mail de forma única e case-insensitive, considerando e-mails que diferem apenas em maiúsculas/minúsculas como o mesmo e-mail para fins de duplicidade.

#### Scenario: Cadastro bem-sucedido sem empresa
- **WHEN** um visitante envia um formulário de cadastro com nome completo, um e-mail ainda não utilizado e uma senha que atende à política mínima, sem informar empresa
- **THEN** o sistema cria a conta do nutricionista sem empresa associada e confirma que a conta foi criada

#### Scenario: Cadastro bem-sucedido com empresa
- **WHEN** um visitante envia um formulário de cadastro com nome completo, um e-mail ainda não utilizado, uma senha que atende à política mínima, e o nome de uma empresa
- **THEN** o sistema cria a conta do nutricionista com a empresa associada e confirma que a conta foi criada

#### Scenario: E-mail duplicado rejeitado
- **WHEN** um visitante envia um cadastro com um endereço de e-mail que já pertence a uma conta de nutricionista existente, mesmo que a diferença esteja apenas em maiúsculas/minúsculas
- **THEN** o sistema rejeita o cadastro e retorna um erro indicando que o e-mail já está em uso

#### Scenario: Senha abaixo da política mínima rejeitada
- **WHEN** um visitante envia um cadastro com uma senha menor que 8 caracteres ou que não combine letra e número
- **THEN** o sistema rejeita o cadastro e indica que a senha não atende à política mínima

### Requirement: Login do Nutricionista
O sistema DEVE permitir que um nutricionista cadastrado se autentique usando seu e-mail e senha e receba uma sessão autenticada.

#### Scenario: Login bem-sucedido
- **WHEN** um nutricionista envia seu e-mail e senha corretos
- **THEN** o sistema concede uma sessão autenticada e permite acesso aos próprios dados do nutricionista

#### Scenario: Credenciais inválidas rejeitadas
- **WHEN** um nutricionista envia uma combinação de e-mail/senha que não corresponde a nenhuma conta cadastrada
- **THEN** o sistema rejeita a tentativa de login e não cria uma sessão

### Requirement: Login e Cadastro via Conta Google
O sistema DEVE permitir que um nutricionista se autentique usando uma conta Google, como alternativa ao login por e-mail e senha. Quando não existir conta de nutricionista com o e-mail da conta Google, o sistema DEVE criar uma automaticamente a partir do nome e e-mail informados pela conta Google, sem exigir senha local. Quando já existir uma conta de nutricionista com esse e-mail (criada via cadastro tradicional ou via Google anteriormente), o sistema DEVE autenticar essa conta existente em vez de criar uma duplicata, preservando os dados de perfil (nome, empresa) já cadastrados nela.

#### Scenario: Login via Google cria conta automaticamente
- **WHEN** um nutricionista usa sua conta Google para entrar no sistema e nenhuma conta com esse e-mail existe
- **THEN** o sistema cria uma conta de nutricionista com o nome e e-mail da conta Google, sem senha local, e concede acesso

#### Scenario: Login via Google vinculado a conta existente
- **WHEN** um nutricionista usa sua conta Google para entrar e já existe uma conta de nutricionista com esse e-mail
- **THEN** o sistema concede acesso a essa conta existente, sem criar uma conta duplicada

#### Scenario: Perfil existente preservado ao entrar com Google
- **WHEN** uma conta de nutricionista já existente, com nome e/ou empresa diferentes dos da conta Google, é usada para entrar via Google
- **THEN** o sistema mantém o nome e a empresa já cadastrados, sem sobrescrevê-los com os dados do Google

#### Scenario: Login tradicional indisponível para conta criada só via Google
- **WHEN** um nutricionista cuja conta foi criada exclusivamente via Google tenta entrar informando e-mail e senha
- **THEN** o sistema rejeita a tentativa da mesma forma que credenciais inválidas, sem indicar que a conta existe apenas via Google

### Requirement: Logout do Nutricionista
O sistema DEVE permitir que um nutricionista autenticado encerre sua sessão.

#### Scenario: Logout bem-sucedido
- **WHEN** um nutricionista autenticado solicita encerrar a sessão
- **THEN** o sistema invalida a sessão atual, de forma que ela não possa mais ser usada para acessar dados

### Requirement: Consulta do Próprio Perfil
O sistema DEVE permitir que um nutricionista autenticado consulte os próprios dados de perfil (nome, empresa e e-mail).

#### Scenario: Perfil retornado com sucesso
- **WHEN** um nutricionista autenticado solicita os próprios dados de perfil
- **THEN** o sistema retorna nome, empresa (quando cadastrada) e e-mail desse nutricionista

#### Scenario: Consulta de perfil sem autenticação negada
- **WHEN** a consulta ao próprio perfil é feita sem uma sessão autenticada válida
- **THEN** o sistema nega a solicitação

### Requirement: Isolamento de Dados por Nutricionista
O sistema DEVE vincular todo paciente, plano alimentar e avaliação física ao nutricionista que o criou, e DEVE permitir que um nutricionista autenticado leia ou modifique apenas registros que ele possui.

#### Scenario: Nutricionista não pode acessar paciente de outro nutricionista
- **WHEN** um nutricionista autenticado solicita um paciente, plano alimentar ou avaliação física pertencente a outro nutricionista
- **THEN** o sistema nega o acesso e não retorna os dados desse registro

#### Scenario: Acesso não autenticado negado
- **WHEN** uma solicitação para visualizar ou modificar um paciente, plano alimentar, avaliação física ou o próprio perfil do nutricionista é feita sem uma sessão autenticada válida
- **THEN** o sistema nega a solicitação
