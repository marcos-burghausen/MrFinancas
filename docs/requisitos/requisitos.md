🔙 [Retornar à documentação principal](../../README.md)

# Requisitos do Sistema MrFinancas

Este documento descreve os requisitos funcionais e não funcionais do sistema MrFinancas, um aplicativo de gestão financeira pessoal.

## Requisitos Funcionais

### RF01 - Gestão de Usuários

#### RF01.1 - Cadastro de Usuário

- O sistema deve permitir o cadastro de novos usuários com e-mail e senha
- O sistema deve oferecer cadastro via redes sociais (Google, Facebook, LinkedIn)
- O sistema deve validar e-mails únicos
- O sistema deve exigir senhas com pelo menos 8 caracteres, incluindo letras, números e caracteres especiais

#### RF01.2 - Autenticação de Usuário

- O sistema deve permitir o login através de e-mail e senha
- O sistema deve permitir o login via redes sociais integradas
- O sistema deve oferecer recuperação de senha via e-mail
- O sistema deve implementar autenticação de dois fatores (opcional para o usuário)

#### RF01.3 - Perfis de Usuário

- O sistema deve suportar os seguintes perfis:
  - USER: Acesso básico à gestão financeira
  - TRADER: Acesso ao módulo de investimentos
  - USER_TRADER: Acesso completo à gestão financeira e investimentos
  - ADMIN: Acesso administrativo ao sistema
  - FULL: Acesso completo a todas as funcionalidades

#### RF01.4 - Gerenciamento de Perfil

- O sistema deve permitir ao usuário editar seus dados pessoais
- O sistema deve permitir ao usuário alterar sua senha
- O sistema deve permitir ao usuário configurar suas preferências de notificação
- O sistema deve permitir ao usuário fazer upload de avatar personalizado

### RF02 - Gestão de Contas Bancárias

#### RF02.1 - Cadastro de Contas

- O sistema deve permitir o cadastro de múltiplas contas bancárias
- O sistema deve registrar nome da conta, instituição financeira, tipo de conta, saldo inicial e cor/ícone
- O sistema deve permitir marcar contas como ativas ou inativas
- O sistema deve permitir definir uma conta como principal

#### RF02.2 - Gerenciamento de Contas

- O sistema deve permitir editar informações da conta
- O sistema deve permitir excluir contas (com validação de lançamentos existentes)
- O sistema deve calcular o saldo atual com base nos lançamentos
- O sistema deve permitir reconciliar o saldo com o saldo real do banco

#### RF02.3 - Visualização de Contas

- O sistema deve exibir lista de contas com saldos atuais
- O sistema deve exibir extrato detalhado de transações por conta
- O sistema deve permitir filtrar transações por período, categoria e valor
- O sistema deve permitir buscar transações por descrição

### RF03 - Gestão de Cartões de Crédito

#### RF03.1 - Cadastro de Cartões

- O sistema deve permitir o cadastro de múltiplos cartões de crédito
- O sistema deve registrar nome, bandeira, limite, dia de fechamento, dia de vencimento e cor/ícone
- O sistema deve permitir vincular cartão a uma conta bancária
- O sistema deve permitir marcar cartões como ativos ou inativos

#### RF03.2 - Gerenciamento de Cartões

- O sistema deve permitir editar informações do cartão
- O sistema deve permitir excluir cartões (com validação de lançamentos existentes)
- O sistema deve calcular fatura atual com base nos lançamentos
- O sistema deve calcular limite disponível do cartão

#### RF03.3 - Fatura de Cartão

- O sistema deve gerar faturas mensais por cartão
- O sistema deve permitir visualizar faturas fechadas e abertas
- O sistema deve exibir lançamentos agrupados por categoria na fatura
- O sistema deve suportar lançamentos parcelados com controle de parcelas

### RF04 - Gestão de Categorias

#### RF04.1 - Cadastro de Categorias

- O sistema deve permitir o cadastro de categorias de receitas e despesas
- O sistema deve permitir o cadastro de subcategorias vinculadas a categorias
- O sistema deve associar cores e ícones às categorias
- O sistema deve fornecer categorias padrão pré-cadastradas

#### RF04.2 - Gerenciamento de Categorias

- O sistema deve permitir editar nome, cor e ícone das categorias
- O sistema deve permitir desativar categorias (sem excluir lançamentos)
- O sistema deve permitir mesclar categorias
- O sistema deve validar exclusão de categorias usadas em lançamentos

### RF05 - Gestão de Lançamentos Financeiros

#### RF05.1 - Cadastro de Lançamentos

- O sistema deve permitir o cadastro de receitas e despesas
- O sistema deve permitir lançamentos em contas bancárias e cartões de crédito
- O sistema deve registrar descrição, valor, data, categoria, conta/cartão e observações
- O sistema deve permitir anexar comprovantes aos lançamentos

#### RF05.2 - Tipos de Lançamentos

- O sistema deve suportar receitas (fixas e variáveis)
- O sistema deve suportar despesas (fixas e variáveis)
- O sistema deve suportar transferências entre contas
- O sistema deve suportar pagamentos de faturas de cartão
- O sistema deve suportar estornos e reembolsos

#### RF05.3 - Lançamentos Recorrentes

- O sistema deve permitir criar lançamentos recorrentes (diários, semanais, mensais, anuais)
- O sistema deve gerar automaticamente lançamentos futuros com base na recorrência
- O sistema deve permitir editar série de lançamentos recorrentes
- O sistema deve permitir encerrar séries recorrentes

#### RF05.4 - Gerenciamento de Lançamentos

- O sistema deve permitir editar informações dos lançamentos
- O sistema deve permitir excluir lançamentos
- O sistema deve permitir marcar lançamentos como pagos/recebidos
- O sistema deve permitir adiar lançamentos para outra data

### RF06 - Relatórios e Análises

#### RF06.1 - Relatórios Básicos

- O sistema deve gerar relatório de receitas e despesas por período
- O sistema deve gerar relatório de gastos por categoria
- O sistema deve gerar relatório de evolução patrimonial
- O sistema deve gerar extrato consolidado de todas as contas

#### RF06.2 - Gráficos e Visualizações

- O sistema deve exibir gráfico de pizza de despesas por categoria
- O sistema deve exibir gráfico de barras comparando receitas e despesas
- O sistema deve exibir gráfico de linha de evolução patrimonial
- O sistema deve exibir dashboard com resumo financeiro

#### RF06.3 - Análises Avançadas

- O sistema deve calcular médias de gastos por categoria
- O sistema deve identificar tendências de gastos
- O sistema deve comparar períodos (mês atual vs. anterior, ano atual vs. anterior)
- O sistema deve fornecer previsões com base em lançamentos recorrentes

### RF07 - Orçamento e Planejamento

#### RF07.1 - Definição de Orçamento

- O sistema deve permitir definir valores de orçamento por categoria
- O sistema deve permitir definir orçamentos mensais e anuais
- O sistema deve permitir copiar orçamento do mês anterior
- O sistema deve permitir ajustar orçamento durante o período

#### RF07.2 - Acompanhamento de Orçamento

- O sistema deve exibir comparativo entre valores orçados e realizados
- O sistema deve calcular percentual de utilização do orçamento
- O sistema deve alertar quando gastos ultrapassarem o orçamento
- O sistema deve exibir projeção de gastos com base no histórico

#### RF07.3 - Metas Financeiras

- O sistema deve permitir criar metas de economia
- O sistema deve permitir definir prazo e valor alvo da meta
- O sistema deve calcular progresso da meta com base em lançamentos vinculados
- O sistema deve exibir projeção de cumprimento da meta

### RF08 - Alertas e Notificações

#### RF08.1 - Configuração de Alertas

- O sistema deve permitir configurar notificações para contas a pagar/receber
- O sistema deve permitir configurar alertas de saldo baixo em contas
- O sistema deve permitir configurar alertas de limite de cartão
- O sistema deve permitir definir antecedência para notificações (1-7 dias)

#### RF08.2 - Entrega de Notificações

- O sistema deve enviar notificações por e-mail
- O sistema deve exibir notificações no aplicativo
- O sistema deve suportar notificações push no navegador
- O sistema deve permitir marcar notificações como lidas

###
