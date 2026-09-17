# Tarefas

## 1. Validação da Especificação

- [ ] 1.1 Executar `openspec validate add-food-catalog` e verificar que não reporta erros
- [ ] 1.2 Revisar a especificação `food-catalog` em conjunto com `diet-prescription` (na change `add-nutritionist-core-features`) e verificar que a estrutura de "refeição com itens alimentares e quantidade" descrita em `diet-prescription` é compatível com o alimento e a porção de referência descritos aqui
- [ ] 1.3 Verificar que todo requisito da capacidade `food-catalog` possui pelo menos um cenário de caminho de sucesso e, quando aplicável, um cenário de rejeição/erro ou de exclusão parcial, e adicionar qualquer cenário faltante encontrado

## 2. Consolidação do Modelo de Domínio

- [ ] 2.1 Documentar a entidade Alimento (nome, porção de referência, kcal/proteína/carboidrato por porção) e sua cadeia de propriedade (Alimento → Nutricionista), e verificar que todo campo citado remonta a um requisito específico da especificação `food-catalog`
- [ ] 2.2 Documentar a regra de cálculo do total nutricional da receita (contribuição proporcional por alimento, exclusão por dados ausentes ou unidade incompatível, sinalização de total parcial) com um exemplo numérico, e verificar que a regra documentada corresponde exatamente aos cenários da especificação

## 3. Prontidão para Repasse (Handoff)

- [ ] 3.1 Executar `openspec status --change add-food-catalog` e verificar que `proposal`, `specs`, `design` e `tasks` reportam status `done`
- [ ] 3.2 Ao arquivar `add-nutritionist-core-features`, revisar o requisito de exportação em PDF de `diet-prescription` junto com o requisito "Cálculo do Total Nutricional da Receita" desta capacidade, incorporando a exibição dos totais nutricionais ao requisito de PDF e registrando essa decisão como um follow-up
