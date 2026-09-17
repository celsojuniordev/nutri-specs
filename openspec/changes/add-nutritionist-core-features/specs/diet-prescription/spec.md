# Delta da Especificação

## Purpose

Permitir que um nutricionista prescreva um plano alimentar ("receita") para um paciente, composto por refeições e itens alimentares, e entregue esse plano ao paciente como um documento PDF exportado.

## ADDED Requirements

### Requirement: Criação de Plano Alimentar por Refeições
O sistema DEVE permitir que um nutricionista autenticado crie um plano alimentar para um paciente que possui, organizado em uma ou mais refeições, cada refeição contendo um ou mais itens alimentares. Ao abrir a tela de criação sem nenhuma refeição cadastrada, o sistema DEVE exibir um botão para cadastrar uma nova refeição. Cada nova refeição DEVE receber, por padrão, um nome sequencial no formato "Refeição N" (ex.: "Refeição 1", "Refeição 2"), que o nutricionista pode renomear livremente. O nutricionista DEVE poder cadastrar múltiplas refeições antes de concluir, e ao final DEVE haver a opção de criar o plano alimentar ("Criar") ou cancelar a operação sem salvar nada ("Cancelar").

#### Scenario: Tela de criação sem refeições cadastradas
- **WHEN** um nutricionista autenticado abre a tela de criação de um novo plano alimentar para um paciente que possui e ainda não cadastrou nenhuma refeição
- **THEN** o sistema exibe um botão para cadastrar uma nova refeição e nenhuma refeição na lista

#### Scenario: Cadastrar refeição com nome padrão sequencial
- **WHEN** um nutricionista autenticado clica no botão de cadastrar refeição pela primeira vez, e depois uma segunda vez, dentro da mesma tela de criação
- **THEN** o sistema cria a primeira refeição com o nome padrão "Refeição 1" e a segunda com o nome padrão "Refeição 2"

#### Scenario: Renomear refeição
- **WHEN** um nutricionista autenticado altera o nome de uma refeição cadastrada na tela de criação
- **THEN** o sistema passa a usar o novo nome informado para essa refeição

#### Scenario: Criação de plano alimentar bem-sucedida
- **WHEN** um nutricionista autenticado confirma a criação (botão "Criar") de um novo plano alimentar para um paciente que possui, incluindo pelo menos uma refeição com pelo menos um item alimentar
- **THEN** o sistema cria o plano alimentar associado a esse paciente, com as refeições e itens alimentares informados, e confirma a criação

#### Scenario: Cancelar criação sem salvar
- **WHEN** um nutricionista autenticado clica em "Cancelar" durante a criação de um plano alimentar, com ou sem refeições e itens já preenchidos na tela
- **THEN** o sistema descarta as informações preenchidas e não cria nenhum plano alimentar

#### Scenario: Plano alimentar sem refeições rejeitado
- **WHEN** um nutricionista autenticado confirma a criação de um novo plano alimentar sem nenhuma refeição cadastrada
- **THEN** o sistema rejeita a solicitação e indica que pelo menos uma refeição é obrigatória

### Requirement: Adição de Item Alimentar a uma Refeição
O sistema DEVE permitir que, ao montar uma refeição, o nutricionista autenticado adicione itens alimentares buscando pelo nome do alimento em um campo de busca que consulta os alimentos cadastrados no catálogo de alimentos do nutricionista (capacidade `food-catalog`). Quando a busca não encontrar nenhum alimento correspondente, o sistema DEVE oferecer uma opção para cadastrar rapidamente um novo alimento sem sair da tela de criação do plano alimentar. Para cada item alimentar adicionado, o nutricionista DEVE escolher um tipo de quantidade dentre exatamente duas opções — "unidade" ou "porção em gramas" — e, ao escolher um tipo, o sistema DEVE habilitar um campo numérico para informar a quantidade correspondente. O sistema DEVE permitir, opcionalmente, registrar uma observação em texto livre para cada item alimentar.

#### Scenario: Buscar e selecionar alimento já cadastrado
- **WHEN** um nutricionista autenticado digita o nome de um alimento no campo de busca de uma refeição e esse alimento já está cadastrado em seu catálogo de alimentos
- **THEN** o sistema exibe o alimento correspondente como sugestão e permite selecioná-lo como item da refeição

#### Scenario: Cadastro rápido de alimento não encontrado
- **WHEN** um nutricionista autenticado busca por um alimento no campo de uma refeição e nenhum alimento correspondente é encontrado em seu catálogo
- **THEN** o sistema exibe uma opção para cadastrar rapidamente esse alimento, e ao usá-la cria o alimento no catálogo do nutricionista e o adiciona como item da refeição

#### Scenario: Selecionar tipo de quantidade "unidade"
- **WHEN** um nutricionista autenticado seleciona o tipo de quantidade "unidade" para um item alimentar
- **THEN** o sistema habilita um campo numérico para informar a quantidade de unidades desse item

#### Scenario: Selecionar tipo de quantidade "porção em gramas"
- **WHEN** um nutricionista autenticado seleciona o tipo de quantidade "porção em gramas" para um item alimentar
- **THEN** o sistema habilita um campo numérico para informar a quantidade em gramas desse item

#### Scenario: Item sem tipo de quantidade ou valor rejeitado
- **WHEN** um nutricionista autenticado tenta adicionar um item alimentar a uma refeição sem selecionar um tipo de quantidade ou sem informar o valor numérico correspondente
- **THEN** o sistema rejeita a inclusão do item e indica que o tipo de quantidade e o valor são obrigatórios

#### Scenario: Registrar observação do item
- **WHEN** um nutricionista autenticado informa um texto de observação para um item alimentar de uma refeição
- **THEN** o sistema salva essa observação junto com o item, sem exigi-la para os demais itens

### Requirement: Atualização de Plano Alimentar
O sistema DEVE permitir que um nutricionista autenticado atualize um plano alimentar que criou, incluindo renomear, adicionar ou remover refeições, e adicionar, editar ou remover itens alimentares de uma refeição — incluindo o alimento selecionado, o tipo de quantidade, o valor numérico e a observação de cada item.

#### Scenario: Atualização de plano alimentar bem-sucedida
- **WHEN** um nutricionista autenticado envia alterações para um plano alimentar que possui
- **THEN** o sistema salva as refeições e itens alimentares atualizados e retorna o plano alimentar atualizado

#### Scenario: Editar tipo de quantidade e valor de um item existente
- **WHEN** um nutricionista autenticado altera o tipo de quantidade ou o valor numérico de um item alimentar já existente em um plano alimentar que possui
- **THEN** o sistema salva o novo tipo de quantidade e/ou valor para esse item

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
