# Design

## Contexto

Esta é a primeira mudança em um repositório de especificações greenfield (`nutri-specs`). Ainda não existe uma base de código, backend ou frontend — estas especificações são a fonte que futuramente vai orientar especificações de implementação separadas de frontend e backend/repositórios. Veja proposal.md - Por Que para a motivação.

As quatro capacidades (`nutritionist-auth`, `patient-management`, `diet-prescription`, `physical-assessment`) compartilham um único modelo de domínio e estão sendo especificadas juntas porque dependem umas das outras de ponta a ponta: um paciente pertence a um nutricionista, e tanto os planos alimentares quanto as avaliações físicas pertencem a um paciente.

## Objetivos / Não Objetivos

**Objetivos:**
- Estabelecer um modelo de domínio único e consistente (Nutricionista, Paciente, Plano Alimentar, Avaliação Física) sobre o qual futuras especificações de frontend/backend possam se apoiar sem precisar redefinir as regras de domínio.
- Manter os dados de cada capacidade sempre vinculados ao nutricionista proprietário (multi-tenant por nutricionista).
- Manter o protocolo de avaliação física concreto (dobras cutâneas de Pollock em 7 pontos + um conjunto fixo de circunferências) para que as especificações seguintes tenham um formato de dados inequívoco para implementar.

**Não Objetivos:**
- Não especificar métricas calculadas de composição corporal (ex.: fórmulas de percentual de gordura corporal) — apenas a captura e comparação de medidas brutas estão no escopo desta mudança.
- Não especificar o layout visual/identidade do PDF — apenas seu conteúdo obrigatório (paciente, nutricionista, refeições, itens alimentares, quantidades).
- Não especificar fluxos de recuperação de senha, verificação de e-mail ou autenticação multifator — apenas cadastro, login, logout e isolamento de dados por nutricionista.
- Não especificar escolhas de tecnologia de implementação de frontend ou backend — isso pertence às especificações de frontend/backend subsequentes que este repositório vai gerar depois.

## Decisões

- **Modelo de propriedade**: Todo registro de Paciente pertence a exatamente um Nutricionista; todo Plano Alimentar e Avaliação Física pertence a exatamente um Paciente (e, transitivamente, ao nutricionista daquele paciente). Isso mantém a regra de autorização em `nutritionist-auth` simples e uniforme entre as capacidades: um nutricionista só pode ler/escrever registros que remontem à sua própria conta.
- **Protocolo de avaliação fixado em Pollock 7 dobras + circunferências padrão**: escolhido (conforme decisão do usuário) em vez de uma lista de campos aberta/flexível, para que a especificação forneça às implementações seguintes um formato de dados concreto e testável, em vez de uma estrutura arbitrária de chave-valor. Alternativa considerada: campos de medida totalmente flexíveis definidos pelo usuário — rejeitada para esta mudança porque empurraria a decisão de modelagem de dados para depois e tornaria o comportamento de comparação mais difícil de especificar com precisão.
- **Desativação em vez de exclusão para pacientes**: pacientes são desativados de forma reversível (soft-delete, conforme `patient-management`) em vez de excluídos, para que o histórico de dieta e avaliação de um paciente nunca seja perdido mesmo que o nutricionista pare de atendê-lo ativamente.
- **Histórico de plano alimentar é append-only do ponto de vista do paciente**: atualizar um plano alimentar o edita no lugar (conforme `diet-prescription`); criar uma nova prescrição ao longo do tempo é o que gera histórico, espelhando como as avaliações físicas acumulam histórico. Isso mantém a semântica de histórico das duas capacidades consistente.

## Riscos / Trade-offs

- [Protocolo de avaliação fixo pode não corresponder à prática de todo nutricionista] → Aceito para esta versão conforme decisão explícita do usuário; uma mudança futura pode ampliar o modelo se necessário.
- [Requisitos de exportação em PDF são apenas de conteúdo, não visuais] → As especificações de frontend/backend subsequentes precisarão adicionar decisões de layout/identidade visual; sinalizado aqui como um não objetivo em vez de deixado implícito.
- [Ainda não há fluxos de recuperação de senha/segurança de conta especificados] → Aceitável para uma mudança inicial de funcionalidades centrais; deve ser escopado explicitamente como uma mudança de acompanhamento antes do uso em produção.
