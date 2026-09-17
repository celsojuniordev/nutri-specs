# Tarefas

## 1. Validação da Especificação

- [ ] 1.1 Executar `openspec validate add-anamnesis-questions` e verificar que não reporta erros
- [ ] 1.2 Revisar a especificação `anamnesis` em conjunto com `patient-management` (na change `add-nutritionist-core-features`) e verificar que os conceitos de "pertence ao nutricionista" e "pertence ao paciente" são usados de forma consistente entre as duas
- [ ] 1.3 Verificar que todo requisito da capacidade `anamnesis` possui pelo menos um cenário de caminho de sucesso e, quando aplicável, um cenário de rejeição/erro, e adicionar qualquer cenário faltante encontrado

## 2. Consolidação do Modelo de Domínio

- [ ] 2.1 Documentar as novas entidades (Pergunta de Anamnese, Resposta de Anamnese do Paciente) e sua cadeia de propriedade (Pergunta → Nutricionista; Resposta → Paciente + Pergunta), e verificar que todo campo citado remonta a um requisito específico da especificação `anamnesis`
- [ ] 2.2 Documentar os campos obrigatórios e regras de validação (texto da pergunta obrigatório; no máximo uma resposta vigente por paciente/pergunta; resposta só pode ser registrada para pergunta ativa), e verificar que cada regra cita o requisito de onde vem

## 3. Prontidão para Repasse (Handoff)

- [ ] 3.1 Executar `openspec status --change add-anamnesis-questions` e verificar que `proposal`, `specs`, `design` e `tasks` reportam status `done`
- [ ] 3.2 Ao arquivar `add-nutritionist-core-features`, revisar o requisito "Isolamento de Dados por Nutricionista" de `nutritionist-auth` junto com o requisito equivalente desta capacidade (`anamnesis`) e decidir se devem ser unificados em um único requisito de isolamento, registrando essa decisão como um follow-up
