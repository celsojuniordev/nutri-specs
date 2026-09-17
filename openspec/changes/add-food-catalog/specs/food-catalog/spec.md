# Delta da Especificação

## Purpose

Permitir que um nutricionista cadastre, individualmente ou em massa via importação de planilha, os alimentos que podem compor uma receita, com dados nutricionais opcionais por porção, e usar esses dados para calcular o total nutricional de uma receita.

## ADDED Requirements

### Requirement: Cadastro Individual de Alimento
O sistema DEVE permitir que um nutricionista autenticado cadastre um novo alimento informando, no mínimo, o nome do alimento, associando o alimento a esse nutricionista. O sistema DEVE permitir informar, opcionalmente, uma porção de referência (quantidade e unidade de medida), o valor calórico (kcal) por porção, a proteína por porção e o carboidrato por porção.

#### Scenario: Cadastro de alimento apenas com nome
- **WHEN** um nutricionista autenticado cadastra um novo alimento informando apenas o nome
- **THEN** o sistema cria o alimento associado a esse nutricionista, sem exigir porção, kcal, proteína ou carboidrato

#### Scenario: Cadastro de alimento com dados nutricionais completos
- **WHEN** um nutricionista autenticado cadastra um novo alimento informando nome, porção de referência, kcal por porção, proteína por porção e carboidrato por porção
- **THEN** o sistema cria o alimento com todos os dados informados associados a essa porção de referência

#### Scenario: Nome ausente rejeitado
- **WHEN** um nutricionista autenticado tenta cadastrar um alimento sem informar o nome
- **THEN** o sistema rejeita a solicitação e indica que o nome é obrigatório

### Requirement: Listagem e Atualização de Alimento
O sistema DEVE permitir que um nutricionista autenticado liste os alimentos ativos que cadastrou e atualize as informações de um alimento que possui, incluindo nome, porção de referência, kcal, proteína e carboidrato por porção.

#### Scenario: Listar alimentos ativos próprios
- **WHEN** um nutricionista autenticado solicita sua lista de alimentos
- **THEN** o sistema retorna apenas os alimentos ativos cadastrados por esse nutricionista

#### Scenario: Atualização de alimento bem-sucedida
- **WHEN** um nutricionista autenticado envia informações atualizadas para um alimento que possui
- **THEN** o sistema salva as informações atualizadas e retorna o registro do alimento atualizado

### Requirement: Desativação de Alimento
O sistema DEVE permitir que um nutricionista autenticado desative um alimento que cadastrou sem remover esse alimento das receitas em que já foi utilizado, e DEVE excluir alimentos desativados da lista de alimentos disponíveis para novas receitas.

#### Scenario: Desativar um alimento
- **WHEN** um nutricionista autenticado desativa um alimento que cadastrou
- **THEN** o sistema marca o alimento como inativo, remove-o da lista de alimentos disponíveis para novas receitas, e mantém sua presença nas receitas já criadas que o utilizam

### Requirement: Download da Planilha Modelo para Importação em Massa
O sistema DEVE permitir que um nutricionista autenticado baixe uma planilha modelo contendo as colunas necessárias para o cadastro em massa de alimentos: nome, porção de referência, kcal por porção, proteína por porção e carboidrato por porção.

#### Scenario: Download da planilha modelo
- **WHEN** um nutricionista autenticado solicita a planilha modelo de importação de alimentos
- **THEN** o sistema disponibiliza para download uma planilha com as colunas nome, porção de referência, kcal por porção, proteína por porção e carboidrato por porção, e sem linhas de dados

### Requirement: Importação em Massa de Alimentos
O sistema DEVE permitir que um nutricionista autenticado importe uma planilha preenchida no formato da planilha modelo para cadastrar múltiplos alimentos de uma vez. O sistema DEVE validar cada linha individualmente: linhas válidas SÃO cadastradas como alimentos associados a esse nutricionista, e linhas inválidas NÃO são cadastradas, sem impedir a importação das demais linhas válidas da mesma planilha.

#### Scenario: Importação totalmente válida
- **WHEN** um nutricionista autenticado importa uma planilha em que todas as linhas preenchidas têm nome e, quando presentes, valores numéricos válidos de porção, kcal, proteína e carboidrato
- **THEN** o sistema cadastra um alimento para cada linha da planilha, associado a esse nutricionista

#### Scenario: Importação com linhas inválidas
- **WHEN** um nutricionista autenticado importa uma planilha em que algumas linhas têm nome ausente ou um valor numérico inválido em kcal, proteína ou carboidrato
- **THEN** o sistema cadastra os alimentos correspondentes às linhas válidas e retorna, para cada linha inválida, o número da linha e o motivo da rejeição, sem cadastrar essas linhas

### Requirement: Cálculo do Total Nutricional da Receita
O sistema DEVE calcular o total de kcal, proteína e carboidrato de uma receita (plano alimentar de `diet-prescription`) somando, para cada alimento incluído na receita que possua porção de referência e o respectivo dado nutricional cadastrados, a contribuição proporcional entre a quantidade utilizada na receita e a porção de referência do alimento, desde que ambas estejam na mesma unidade de medida.

#### Scenario: Total calculado com todos os alimentos tendo dados nutricionais
- **WHEN** uma receita é composta somente por alimentos que possuem porção de referência e dados nutricionais cadastrados, com quantidades na receita na mesma unidade de medida da porção de referência de cada alimento
- **THEN** o sistema calcula o total de kcal, proteína e carboidrato da receita somando a contribuição proporcional de cada alimento, para exibição no PDF exportado pela capacidade `diet-prescription`

#### Scenario: Alimento sem dados nutricionais excluído do total
- **WHEN** uma receita inclui um alimento que não possui porção de referência ou dado nutricional cadastrado
- **THEN** o sistema exclui esse alimento do cálculo do total nutricional e sinaliza que o total apresentado é parcial

#### Scenario: Unidade de medida incompatível excluída do total
- **WHEN** a quantidade de um alimento usada na receita está em uma unidade de medida diferente da porção de referência cadastrada para esse alimento
- **THEN** o sistema exclui esse alimento do cálculo do total nutricional e sinaliza que o total apresentado é parcial

### Requirement: Isolamento de Dados de Alimentos por Nutricionista
O sistema DEVE vincular todo alimento ao nutricionista que o cadastrou (individualmente ou via importação em massa), e DEVE permitir que um nutricionista autenticado leia ou modifique apenas alimentos que pertençam a ele.

#### Scenario: Nutricionista não pode acessar alimento de outro nutricionista
- **WHEN** um nutricionista autenticado solicita listar, atualizar, desativar ou usar em uma receita um alimento cadastrado por outro nutricionista
- **THEN** o sistema nega o acesso
