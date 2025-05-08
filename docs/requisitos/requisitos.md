🔙 [Retornar à documentação principal](../../README.md)

# Requisitos do Sistema MrFinancas

Este documento descreve os requisitos funcionais e não funcionais do sistema MrFinancas, um aplicativo de gestão financeira pessoal.

## Requisitos Funcionais

### RF01 - Gestão de Usuários

#### RF01.1 - Cadastro de Usuário

- O sistema deve permitir o cadastro de novos usuários com e-mail, nome e senha.
- O sistema deve oferecer cadastro via redes sociais (Google, Facebook, LinkedIn).
- O sistema deve validar e-mails únicos.
- O sistema deve exigir senhas com pelo menos 8 caracteres, incluindo letras, números e caracteres especiais.

#### RF01.2 - Autenticação de Usuário

- O sistema deve permitir o login através de e-mail e senha.
- O sistema deve permitir o login via redes sociais integradas.
- O sistema deve oferecer recuperação de senha via e-mail.
- O sistema deve implementar autenticação de dois fatores (opcional para o usuário, planejado).

#### RF01.3 - Perfis de Usuário

- O sistema deve suportar os seguintes perfis (planejado):
  - USER: Acesso básico à gestão financeira.
  - TRADER: Acesso ao módulo de investimentos.
  - USER_TRADER: Acesso completo à gestão financeira e investimentos.
  - ADMIN: Acesso administrativo ao sistema.
  - FULL: Acesso completo a todas as funcionalidades.

#### RF01.4 - Gerenciamento de Perfil

- O sistema deve permitir ao usuário editar seus dados pessoais (nome, e-mail, idioma, moeda).
- O sistema deve permitir ao usuário alterar sua senha.
- O sistema deve permitir ao usuário configurar suas preferências de notificação.
- O sistema deve permitir ao usuário fazer upload de avatar personalizado.

### RF02 - Gestão de Contas Bancárias

#### RF02.1 - Cadastro de Contas

- O sistema deve permitir o cadastro de múltiplas contas bancárias.
- O sistema deve registrar nome da conta, instituição financeira, tipo de conta (corrente, poupança, etc.), saldo inicial e cor/ícone.
- O sistema deve permitir marcar contas como ativas ou inativas.
- O sistema deve permitir definir uma conta como principal.

#### RF02.2 - Gerenciamento de Contas

- O sistema deve permitir editar informações da conta.
- O sistema deve permitir excluir contas (com validação de transações existentes).
- O sistema deve calcular o saldo atual com base nas transações.
- O sistema deve permitir reconciliar o saldo com o saldo real do banco (planejado).

#### RF02.3 - Visualização de Contas

- O sistema deve exibir lista de contas com saldos atuais.
- O sistema deve exibir extrato detalhado de transações por conta.
- O sistema deve permitir filtrar transações por período, categoria, valor e descrição.
- O sistema deve permitir buscar transações por descrição.

### RF03 - Gestão de Cartões de Crédito

#### RF03.1 - Cadastro de Cartões

- O sistema deve permitir o cadastro de múltiplos cartões de crédito.
- O sistema deve registrar nome, bandeira, limite, dia de fechamento, dia de vencimento, instituição e cor/ícone.
- O sistema deve permitir vincular cartão a uma conta bancária.
- O sistema deve permitir marcar cartões como ativos ou inativos.

#### RF03.2 - Gerenciamento de Cartões

- O sistema deve permitir editar informações do cartão.
- O sistema deve permitir excluir cartões (com validação de transações existentes).
- O sistema deve calcular fatura atual com base nas transações.
- O sistema deve calcular limite disponível do cartão.

#### RF03.3 - Fatura de Cartão

- O sistema deve gerar faturas mensais por cartão.
- O sistema deve permitir visualizar faturas fechadas e abertas.
- O sistema deve exibir transações agrupadas por categoria na fatura.
- O sistema deve suportar transações parceladas com controle de parcelas.

### RF04 - Gestão de Categorias

#### RF04.1 - Cadastro de Categorias

- O sistema deve permitir o cadastro de categorias de receitas e despesas.
- O sistema deve permitir o cadastro de subcategorias vinculadas a categorias (planejado).
- O sistema deve associar cores e ícones às categorias.
- O sistema deve fornecer categorias padrão pré-cadastradas.

#### RF04.2 - Gerenciamento de Categorias

- O sistema deve permitir editar nome, cor e ícone das categorias.
- O sistema deve permitir desativar categorias (sem excluir transações).
- O sistema deve permitir mesclar categorias (planejado).
- O sistema deve validar exclusão de categorias usadas em transações.

### RF05 - Gestão de Transações Financeiras

#### RF05.1 - Cadastro de Transações

- O sistema deve permitir o cadastro de receitas, despesas e transações de cartão.
- O sistema deve permitir transações em contas bancárias e cartões de crédito.
- O sistema deve registrar descrição, valor, data, categoria, conta/cartão e observações.
- O sistema deve permitir anexar comprovantes às transações (planejado).

#### RF05.2 - Tipos de Transações

- O sistema deve suportar receitas (fixas e variáveis).
- O sistema deve suportar despesas (fixas e variáveis).
- O sistema deve suportar transferências entre contas.
- O sistema deve suportar pagamentos de faturas de cartão.
- O sistema deve suportar estornos e reembolsos.

#### RF05.3 - Transações Recorrentes

- O sistema deve permitir criar transações recorrentes (diárias, semanais, mensais, anuais) (planejado).
- O sistema deve gerar automaticamente transações futuras com base na recorrência (planejado).
- O sistema deve permitir editar série de transações recorrentes (planejado).
- O sistema deve permitir encerrar séries recorrentes (planejado).

#### RF05.4 - Gerenciamento de Transações

- O sistema deve permitir editar informações das transações.
- O sistema deve permitir excluir transações.
- O sistema deve permitir marcar transações como pagas/recebidas (planejado).
- O sistema deve permitir adiar transações para outra data (planejado).

### RF06 - Relatórios e Análises

#### RF06.1 - Relatórios Básicos

- O sistema deve gerar relatório de receitas e despesas por período.
- O sistema deve gerar relatório de gastos por categoria.
- O sistema deve gerar relatório de evolução patrimonial (planejado).
- O sistema deve gerar extrato consolidado de todas as contas (planejado).

#### RF06.2 - Gráficos e Visualizações

- O sistema deve exibir gráfico de pizza de despesas por categoria.
- O sistema deve exibir gráfico de barras comparando receitas e despesas.
- O sistema deve exibir gráfico de linha de evolução patrimonial.
- O sistema deve exibir dashboard com resumo financeiro (planejado).

#### RF06.3 - Análises Avançadas

- O sistema deve calcular médias de gastos por categoria.
- O sistema deve identificar tendências de gastos (planejado).
- O sistema deve comparar períodos (mês atual vs. anterior, ano atual vs. anterior) (planejado).
- O sistema deve fornecer previsões com base em transações recorrentes (planejado).

### RF07 - Orçamento e Planejamento

#### RF07.1 - Definição de Orçamento

- O sistema deve permitir definir valores de orçamento por categoria (planejado).
- O sistema deve permitir definir orçamentos mensais e anuais (planejado).
- O sistema deve permitir copiar orçamento do mês anterior (planejado).
- O sistema deve permitir ajustar orçamento durante o período (planejado).

#### RF07.2 - Acompanhamento de Orçamento

- O sistema deve exibir comparativo entre valores orçados e realizados (planejado).
- O sistema deve calcular percentual de utilização do orçamento (planejado).
- O sistema deve alertar quando gastos ultrapassarem o orçamento (planejado).
- O sistema deve exibir projeção de gastos com base no histórico (planejado).

#### RF07.3 - Metas Financeiras

- O sistema deve permitir criar metas de economia (planejado).
- O sistema deve permitir definir prazo e valor alvo da meta (planejado).
- O sistema deve calcular progresso da meta com base em transações vinculadas (planejado).
- O sistema deve exibir projeção de cumprimento da meta (planejado).

### RF08 - Alertas e Notificações

#### RF08.1 - Configuração de Alertas

- O sistema deve permitir configurar notificações para contas a pagar/receber.
- O sistema deve permitir configurar alertas de saldo baixo em contas.
- O sistema deve permitir configurar alertas de limite de cartão.
- O sistema deve permitir definir antecedência para notificações (1-7 dias).

#### RF08.2 - Entrega de Notificações

- O sistema deve enviar notificações por e-mail.
- O sistema deve exibir notificações no aplicativo.
- O sistema deve suportar notificações push no navegador.
- O sistema deve permitir marcar notificações como lidas (planejado).

### RF09 - Exportação para Calendário

- O sistema deve permitir exportar transações com vencimento para o Google Calendar.
- O sistema deve criar eventos com descrição, data e detalhes da transação.
- O sistema deve suportar autenticação OAuth para integração com Google Calendar.
- O sistema deve registrar eventos exportados para evitar duplicações.

### RF10 - Backup e Restauração

- O sistema deve permitir criar backups dos dados do usuário.
- O sistema deve fornecer URL para download do backup.
- O sistema deve permitir restaurar dados a partir de um backup.
- O sistema deve validar integridade do backup antes da restauração.

## Requisitos Não Funcionais

### RNF01 - Segurança

- O sistema deve usar autenticação JWT para proteger endpoints da API.
- O sistema deve criptografar senhas com algoritmos seguros (e.g., bcrypt).
- O sistema deve usar HTTPS para todas as comunicações.
- O sistema deve implementar validação de propriedade de dados (user_id).

### RNF02 - Performance

- O sistema deve responder a requisições em até 2 segundos em 95% dos casos.
- O sistema deve suportar até 10.000 usuários simultâneos.
- O sistema deve implementar paginação para endpoints com listas de dados.

### RNF03 - Escalabilidade

- O sistema deve ser hospedado em infraestrutura escalável (e.g., AWS).
- O sistema deve usar filas (e.g., AWS SQS) para notificações assíncronas.
- O sistema deve suportar aumento de carga com mínima intervenção manual.

### RNF04 - Disponibilidade

- O sistema deve garantir 99.9% de uptime mensal.
- O sistema deve implementar monitoramento de erros e alertas (e.g., AWS CloudWatch).
- O sistema deve realizar backups automáticos diários dos dados.

### RNF05 - Usabilidade

- O sistema deve ter interface responsiva para dispositivos móveis e desktops.
- O sistema deve seguir padrões de acessibilidade (e.g., WCAG 2.1).
- O sistema deve suportar português como idioma principal, com opção de inglês.

### RNF06 - Integração

- O sistema deve integrar com Google Calendar via OAuth 2.0.
- O sistema deve suportar integração com serviços de e-mail (e.g., AWS SES).
- O sistema deve permitir futuras integrações com APIs bancárias (planejado).

## Notas

- Requisitos marcados como “planejado” ainda não estão implementados na API ou fluxos, mas são planejados para fases futuras.
- O sistema usa “transações” como termo padrão em vez de “lançamentos” para consistência com a API e fluxos.
- O módulo de investimentos (perfil TRADER) será implementado em iteração futura.
