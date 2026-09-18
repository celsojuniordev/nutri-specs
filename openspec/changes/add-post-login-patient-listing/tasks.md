# Tarefas

## 1. Validação da Especificação

- [x] 1.1 Executar `openspec validate add-post-login-patient-listing` e verificar que não reporta erros
- [x] 1.2 Revisar o novo requisito "Destino Padrão Após Login" (capacidade `nutritionist-auth`) junto com o requisito "Listagem e Visualização de Pacientes" (capacidade `patient-management`, na change `add-nutritionist-core-features`) e verificar que ambos descrevem a mesma listagem (pacientes ativos do nutricionista autenticado), sem introduzir uma regra de filtragem ou ordenação diferente
- [x] 1.3 Verificar que o requisito "Destino Padrão Após Login" possui cenários cobrindo login por e-mail/senha, login via Google e o caso sem pacientes cadastrados, e adicionar qualquer cenário faltante encontrado

## 2. Coordenação de Arquivamento

- [ ] 2.1 Ao arquivar esta change, arquivá-la junto com (ou depois de) `add-nutritionist-core-features`, já que o requisito adicionado aqui depende da capacidade `patient-management` definida naquela change; verificar que a capacidade `nutritionist-auth` resultante no System Spec principal contém tanto os requisitos de `add-nutritionist-core-features` quanto o requisito "Destino Padrão Após Login" desta change, sem duplicação de propósito

## 3. Prontidão para Repasse (Handoff)

- [x] 3.1 Executar `openspec status --change add-post-login-patient-listing` e verificar que `proposal`, `specs` e `tasks` reportam status `done` (esta change não possui `design.md` - não há decisão técnica cross-cutting, dependência externa nova, ou ambiguidade que justifique um design; é uma única regra de negócio conectando duas capacidades já especificadas)
- [x] 3.2 Confirmar que o requisito "Destino Padrão Após Login" é suficiente para orientar a especificação de frontend subsequente sobre para onde a sessão autenticada deve levar o nutricionista, e registrar qualquer lacuna encontrada para uma mudança de acompanhamento
