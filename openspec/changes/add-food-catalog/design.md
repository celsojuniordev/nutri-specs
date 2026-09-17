# Design

## Contexto

Esta mudança estende o repositório de especificações `nutri-specs` com a capacidade `food-catalog`, usada por `diet-prescription` (proposta em `add-nutritionist-core-features`, ainda não arquivada) para compor receitas e, opcionalmente, calcular seus totais nutricionais. Veja proposal.md - Por Que para a motivação.

Como `add-nutritionist-core-features` ainda não foi aplicada/arquivada, `openspec/specs/diet-prescription` ainda não existe. Por isso, o comportamento de cálculo do total nutricional é declarado como requisito da própria capacidade `food-catalog` nesta mudança, referenciando o PDF de `diet-prescription` apenas como o local de exibição do resultado.

## Objetivos / Não Objetivos

**Objetivos:**
- Permitir cadastro de alimentos individualmente e em massa, reduzindo o esforço de popular um catálogo grande de alimentos.
- Tornar os dados nutricionais por porção opcionais, sem bloquear o cadastro de um alimento que não os tenha.
- Definir uma regra clara e testável para como o total nutricional de uma receita é calculado a partir dos alimentos que a compõem.

**Não Objetivos:**
- Não especificar conversão automática entre unidades de medida diferentes (ex.: gramas para mililitros, ou unidades para gramas) — quando a unidade da quantidade usada na receita difere da unidade da porção de referência do alimento, esse alimento é simplesmente excluído do total, conforme já definido no requisito de cálculo.
- Não especificar uma tabela nutricional de referência pré-carregada (ex.: TACO/USDA) — os valores nutricionais são sempre informados manualmente pelo nutricionista, individualmente ou via planilha.
- Não especificar o layout visual da seção de totais no PDF — apenas que os totais (quando calculáveis) fiquem disponíveis para exibição, como já é não objetivo de layout em `diet-prescription`.
- Não especificar formato de arquivo além de planilha (ex.: não cobre importação via CSV cru, JSON, etc. como formato alternativo).

## Decisões

- **Alimentos pertencem ao nutricionista, seguindo o mesmo modelo multi-tenant**: cada nutricionista mantém seu próprio catálogo de alimentos, consistente com as decisões já tomadas para pacientes, planos alimentares, avaliações e perguntas de anamnese.
- **Dados nutricionais são todos opcionais e por porção de referência**: em vez de exigir uma unidade fixa (ex.: sempre por 100g), o alimento carrega sua própria porção de referência (quantidade + unidade) e os nutrientes correspondem a essa porção. Alternativa considerada: fixar todos os valores nutricionais em "por 100g" — rejeitada porque obrigaria o nutricionista a sempre converter manualmente antes de cadastrar, aumentando o atrito e o risco de erro.
- **Importação em massa é linha a linha, parcial em caso de erro**: conforme decisão do usuário, uma planilha com algumas linhas inválidas ainda cadastra as linhas válidas, reportando as inválidas individualmente. Alternativa considerada: importação tudo-ou-nada — rejeitada por decisão explícita do usuário, que priorizou não perder o trabalho de preencher uma planilha grande por causa de poucos erros pontuais.
- **Total nutricional exclui, em vez de bloquear, alimentos sem dados suficientes**: um alimento sem porção/nutrientes cadastrados, ou com unidade incompatível com a quantidade usada na receita, é excluído do cálculo e o total é sinalizado como parcial, em vez de impedir a geração do PDF. Isso evita que a ausência de dados nutricionais em um único alimento invalide a exportação de toda a receita.

## Riscos / Trade-offs

- [Totais parciais podem ser mal interpretados como totais completos se o sinalizador de "parcial" não for destacado o suficiente no PDF] → Fica registrado aqui como requisito de que o sistema DEVE sinalizar quando o total é parcial; o destaque visual específico é decisão de `diet-prescription`/design de PDF, fora do escopo desta especificação.
- [Sem conversão de unidades, o nutricionista pode cadastrar a porção de referência em uma unidade e usar o alimento na receita em outra, perdendo o alimento do total sem perceber o motivo] → Mitigado pelo requisito de sinalizar exclusões do total; uma melhoria futura pode listar explicitamente quais alimentos foram excluídos e por quê.
- [Requisito de total nutricional depende de comportamento de `diet-prescription` (PDF) que ainda não foi arquivado] → Sinalizado aqui para revisão conjunta das duas especificações no momento do arquivamento de `add-nutritionist-core-features`, assim como já ocorre com `nutritionist-auth` e `anamnesis`.
