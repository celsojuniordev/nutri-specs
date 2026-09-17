# Proposta

## Por Que

Atualmente, nutricionistas gerenciam cadastros de pacientes, planos alimentares e histórico de avaliação física por meio de ferramentas desconectadas (planilhas, formulários em papel, documentos genéricos). Esta mudança define as especificações centrais do sistema necessárias para que um nutricionista tenha um único sistema para cadastrar pacientes, prescrever dietas que possam ser entregues aos pacientes em PDF, e acompanhar medidas de avaliação física ao longo do tempo para evidenciar resultados. Essas especificações são a base que este repositório usará depois para gerar as especificações de implementação de frontend e backend em outros repositórios.

## O Que Muda

- Adiciona contas de nutricionista com autenticação; cada nutricionista só pode ver e gerenciar seus próprios pacientes (multi-tenant).
- Adiciona cadastro e gestão de pacientes (criar, visualizar, atualizar, desativar pacientes) vinculados ao nutricionista responsável.
- Adiciona prescrição de dieta: um nutricionista pode criar um plano alimentar/dieta ("receita") para um paciente, composto por refeições e itens alimentares, e exportá-lo como um documento PDF.
- Adiciona acompanhamento de avaliação física: registrar peso, medidas de dobras cutâneas (protocolo de Pollock de 7 dobras) e circunferências de segmentos corporais para um paciente, manter um histórico completo de avaliações, e comparar duas avaliações quaisquer para evidenciar a evolução ao longo do tempo.

## Capacidades

### Novas Capacidades
- `nutritionist-auth`: Cadastro de conta do nutricionista, login/autenticação, e controle de acesso baseado em sessão para que cada nutricionista acesse apenas seus próprios dados.
- `patient-management`: Cadastro, visualização, atualização e desativação de pacientes vinculados a um nutricionista específico.
- `diet-prescription`: Criação, atualização e exportação em PDF de um plano alimentar/dieta ("receita") para um paciente.
- `physical-assessment`: Registro de medidas de avaliação física (peso, dobras cutâneas em 7 pontos, circunferências) por paciente, mantendo histórico e permitindo comparação de avaliações ao longo do tempo.

### Capacidades Modificadas
Nenhuma — esta é a primeira mudança do projeto; não há especificações existentes para modificar.

## Impacto

- Estabelece o modelo de domínio inicial do sistema: Nutricionista, Paciente, Plano Alimentar (Receita), Refeição/Item Alimentar, Avaliação Física.
- Nenhum código, API ou sistema existente é afetado — este é um esforço de especificação greenfield que posteriormente orientará a implementação de frontend e backend em repositórios subsequentes.
