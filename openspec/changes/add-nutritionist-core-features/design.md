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
- **Tipo de quantidade do item alimentar fixado em duas opções ("unidade" ou "porção em gramas")**: em vez de uma unidade de medida livre, a tela de receita restringe a quantidade de cada item a exatamente essas duas opções, tornando a entrada de dados mais simples e previsível para o nutricionista e mais fácil de somar/validar do que uma unidade de medida arbitrária.
- **Busca e cadastro rápido de alimento dependem de `food-catalog`**: a adição de item alimentar em uma refeição consulta o catálogo de alimentos do nutricionista (capacidade `food-catalog`, proposta na change separada `add-food-catalog`, também ainda não arquivada) e permite cadastrar um alimento novo sem sair da tela. Essa é uma dependência entre changes ainda em planejamento; assim como o isolamento de dados (ver Riscos abaixo), deve ser revisada quando ambas as changes forem arquivadas.
- **Login via Google como alternativa ao e-mail/senha**: incorporado a partir de uma revisão de consistência com a especificação de implementação do backend (`nutri-back`, change `add-nutritionist-auth`, já implementada), que decidiu suportar esse método como forma alternativa de acesso. Uma conta pode ser criada automaticamente via Google (sem senha local) ou vinculada a uma conta já existente pelo e-mail, sempre preservando os dados de perfil (nome, empresa) já cadastrados — o mecanismo específico (OAuth/OpenID Connect, formato do token) é decisão de implementação do backend e não é especificado aqui.
- **Campo "Empresa" opcional no perfil do nutricionista**: também incorporado da mesma revisão de consistência; representa a clínica/empresa em que o nutricionista atua, sem criar uma entidade própria nem alterar o modelo multi-tenant por nutricionista.
- **Política mínima de senha (8+ caracteres, letra e número)**: incorporada da mesma revisão como regra de negócio/segurança. Limites de tamanho específicos de implementação (ex.: um máximo ligado ao algoritmo de hash escolhido pelo backend) permanecem apenas na especificação de implementação, não aqui.

## Riscos / Trade-offs

- [Protocolo de avaliação fixo pode não corresponder à prática de todo nutricionista] → Aceito para esta versão conforme decisão explícita do usuário; uma mudança futura pode ampliar o modelo se necessário.
- [Requisitos de exportação em PDF são apenas de conteúdo, não visuais] → As especificações de frontend/backend subsequentes precisarão adicionar decisões de layout/identidade visual; sinalizado aqui como um não objetivo em vez de deixado implícito.
- [Ainda não há fluxos de recuperação de senha/segurança de conta especificados] → Aceitável para uma mudança inicial de funcionalidades centrais; deve ser escopado explicitamente como uma mudança de acompanhamento antes do uso em produção.
- [Descompasso de unidade entre `diet-prescription` e `food-catalog`] → Aqui, o tipo de quantidade do item na receita é restrito a "unidade" ou "porção em gramas"; já a porção de referência de um alimento em `food-catalog` aceita qualquer unidade de medida. Isso pode fazer com que um alimento cadastrado com porção de referência em uma unidade diferente (ex.: "1 fatia", "1 xícara") nunca tenha correspondência exata para entrar no cálculo do total nutricional da receita. Mitigação: sinalizado para revisão conjunta das duas especificações no momento do arquivamento, decidindo se `food-catalog` deve restringir sua porção de referência às mesmas duas opções.
- [Conta criada apenas via Google não tem senha local, e esta especificação não define um fluxo para adicionar uma depois] → Aceito nesta versão, espelhando a mesma lacuna aceita na especificação de implementação do backend; se essa conta tentar se cadastrar novamente pela via tradicional com o mesmo e-mail, é tratada como e-mail duplicado. Resolver isso (ex.: um fluxo de "definir senha") fica para uma mudança futura.
