# Proposta

## Por Que

Hoje, ao montar um plano alimentar, o nutricionista não tem uma tela própria para cadastrar os alimentos que podem compor uma receita, nem uma forma rápida de cadastrar muitos alimentos de uma vez. Além disso, quando o nutricionista informa dados nutricionais por porção de cada alimento (kcal, proteína, carboidrato), o PDF da receita pode somar esses valores e mostrar o total nutricional do plano alimentar, agregando valor ao acompanhamento do paciente.

## O Que Muda

- Adiciona uma tela própria para o nutricionista cadastrar alimentos, item por item, com nome (obrigatório) e, opcionalmente, porção de referência, kcal por porção, proteína por porção e carboidrato por porção.
- Adiciona cadastro em massa de alimentos: o nutricionista pode baixar uma planilha modelo, preenchê-la e importá-la para cadastrar vários alimentos de uma vez. Linhas válidas são cadastradas normalmente; linhas com erro são reportadas individualmente, sem bloquear o restante da importação.
- Adiciona listagem, atualização e desativação de alimentos cadastrados.
- Adiciona o cálculo do total nutricional (kcal, proteína, carboidrato) de uma receita, somando a contribuição proporcional de cada alimento incluído que possua dados nutricionais cadastrados, para exibição no PDF exportado da receita (capacidade `diet-prescription`).

## Capacidades

### Novas Capacidades
- `food-catalog`: Cadastro individual e em massa (via importação de planilha) de alimentos, com dados nutricionais opcionais por porção, e cálculo do total nutricional de uma receita a partir dos alimentos que a compõem.

### Capacidades Modificadas
Nenhuma — a capacidade `diet-prescription` ainda não existe em `openspec/specs/` (a change `add-nutritionist-core-features` que a introduz ainda não foi aplicada/arquivada). O requisito de exibir o total nutricional no PDF é declarado nesta mudança dentro da capacidade `food-catalog`, referenciando o comportamento de exportação de `diet-prescription`; quando `add-nutritionist-core-features` for arquivada, o requisito de exportação em PDF de `diet-prescription` deve ser revisado junto com este para incorporar a exibição dos totais.

## Impacto

- Estende o modelo de domínio com Alimento (nome, porção de referência, kcal/proteína/carboidrato por porção), vinculado ao nutricionista que o cadastrou.
- Afeta o conteúdo do PDF gerado por `diet-prescription`, que passa a poder exibir totais nutricionais quando os alimentos da receita tiverem esses dados cadastrados.
- Não afeta código, API ou sistema existente — este repositório ainda é apenas de especificações (greenfield).
