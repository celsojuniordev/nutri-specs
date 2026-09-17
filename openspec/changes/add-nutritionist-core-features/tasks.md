# Tarefas

## 1. Validação da Especificação

- [ ] 1.1 Executar `openspec validate add-nutritionist-core-features` e verificar que não reporta erros (os avisos de `--strict` pedindo "SHALL/MUST" literal são uma verificação específica para specs em inglês e não se aplicam aqui, já que as specs deste projeto são escritas em PT-BR)
- [ ] 1.2 Revisar em conjunto os quatro deltas de especificação (`nutritionist-auth`, `patient-management`, `diet-prescription`, `physical-assessment`) e verificar que não existem requisitos contraditórios entre eles (ex.: as regras de propriedade definidas em `nutritionist-auth` correspondem a como `patient-management`, `diet-prescription` e `physical-assessment` descrevem o acesso aos seus registros)
- [ ] 1.3 Verificar que todo requisito nos quatro deltas de especificação possui pelo menos um cenário de caminho de sucesso e pelo menos um cenário de rejeição/erro, e adicionar qualquer cenário faltante encontrado

## 2. Consolidação do Modelo de Domínio

- [ ] 2.1 Documentar o modelo de domínio consolidado (Nutricionista, Paciente, Plano Alimentar, Refeição, Item Alimentar, Avaliação Física) e a cadeia de propriedade entre eles, e verificar que todo campo citado no documento remonta a um requisito específico em um dos quatro deltas de especificação
- [ ] 2.2 Documentar os campos obrigatórios e regras de validação de cada entidade (campos obrigatórios do paciente; peso obrigatório e regra de valor positivo para peso/dobras cutâneas/circunferências na avaliação; estrutura de refeições/itens alimentares e requisitos de conteúdo do PDF do plano alimentar), e verificar que cada regra cita o requisito de onde vem

## 3. Prontidão para Repasse (Handoff)

- [ ] 3.1 Executar `openspec status --change add-nutritionist-core-features` e verificar que `proposal`, `specs`, `design` e `tasks` reportam status `done`
- [ ] 3.2 Confirmar que a proposta, as especificações e o design desta mudança são suficientes para começar a gerar as especificações de frontend e backend subsequentes (conforme o modelo de domínio e as regras de validação documentadas na seção 2) e registrar qualquer lacuna encontrada para uma mudança de acompanhamento
