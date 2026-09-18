# Proposta

## Por Que

Hoje a especificação de `nutritionist-auth` define que um login bem-sucedido concede uma sessão autenticada, mas não define o que o nutricionista deve encontrar em seguida. Sem essa regra, cada especificação de implementação (frontend) poderia escolher um destino diferente após o login. Como `patient-management` já define a listagem dos próprios pacientes como a visão principal de trabalho do nutricionista, esta mudança torna explícito que é essa listagem que deve ser apresentada assim que o login é concluído.

## O Que Muda

- Adiciona a `nutritionist-auth` a regra de que, após um login bem-sucedido (por e-mail/senha ou por conta Google), o sistema DEVE apresentar ao nutricionista a listagem de seus próprios pacientes ativos (a mesma listagem definida em `patient-management` - "Listagem e Visualização de Pacientes") como conteúdo inicial, em vez de deixar esse destino indefinido.
- Não altera as regras de autenticação em si (política de senha, Google, logout) nem as regras de listagem de pacientes já definidas em `patient-management` - apenas conecta as duas, dizendo qual delas é o ponto de entrada padrão pós-login.

## Capacidades

### Novas Capacidades
Nenhuma.

### Capacidades Modificadas
- `nutritionist-auth`: adiciona um novo requisito descrevendo o destino padrão apresentado ao nutricionista imediatamente após um login bem-sucedido. Nota: esta capacidade ainda está pendente de arquivamento (definida na change `add-nutritionist-core-features`, ainda não arquivada); esta mudança soma um requisito a ela e depende da capacidade `patient-management` (também ainda pendente na mesma change) para a definição de "listagem de pacientes ativos".

## Impacto

- Depende do requisito "Listagem e Visualização de Pacientes" de `patient-management` já existir (mesmo que ainda não arquivado) para ter o que apresentar como destino pós-login.
- Nenhuma alteração de código, API ou sistema existente - este é um repositório de especificações; a mudança orienta como as especificações de implementação de frontend/backend subsequentes devem tratar o pós-login.
