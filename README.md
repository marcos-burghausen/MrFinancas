# Introdução

## Objetivo

O objetivo do aplicativo é fornecer uma ferramenta abrangente para o controle de finanças pessoais e, futuramente, para o gerenciamento de investimentos. Ele visa resolver problemas comuns enfrentados por indivíduos ao tentar organizar suas finanças, como a falta de visibilidade sobre despesas e receitas, dificuldade em planejar orçamentos e a necessidade de monitorar investimentos de forma eficiente.

## Funcionalidades Principais

1. **Cadastro de Usuários**:
      - Métodos de Cadastro/Login: Tradicional, Facebook, Google e LinkedIn.
      - Perfis de Usuário: USER, TRADER, USER_TRADER, ADMIM, FULL.

2. **Lançamentos**:
      - Tipos de Lançamentos: Despesas, receitas, despesas de cartão de crédito, estornos.
      - Categorias e Subcategorias: Específicas para receitas e despesas.
      - Vinculação: Lançamentos vinculados a contas ou cartões de crédito.

3. **Contas e Cartões de Crédito**:
      - Cadastro de Contas: Usuário pode cadastrar contas bancárias.
      - Cadastro de Cartões de Crédito: Cartões vinculados a contas.
      - Categorias: Contas e cartões possuem categorias e subcategorias.
      - Ícones: Ícones personalizados com cores escolhidas pelo usuário.

4. **Perfil do Usuário**:
      - Avatar: Inserir/alterar avatar.
      - Dados Pessoais: Inserir/alterar endereço, documentos, etc.
      - Troca de Senha: Opção para trocar senha.
      - Notificações: Escolher se deseja receber notificações e quando (no dia ou até 3 dias antes).

5. **Relatórios e Gráficos**:
      - Relatórios: Mensais, trimestrais e anuais.
      - Gráficos: Visualização de despesas e receitas.

6. **Alertas e Notificações**:
      - Alertas: Vencimentos de contas, limites de gastos, estornos de cartão de crédito.
      - Notificações: Mudanças significativas nas finanças.

7. **Orçamento**:
      - Planejamento: Ferramentas para planejamento de orçamento mensal e anual.
      - Monitoramento: Monitorar cumprimento do orçamento e alertar sobre desvios.

# Documentação do Aplicativo de Finanças Pessoal

## Fluxos de Usuário

- [Cadastro de Usuário](./docs/fluxos/cadastroUsuario.md)
- [Login](./docs/fluxos/login.md)
- [Gerenciamento de Contas](./docs/fluxos/gerenciamentoContas.md)
- [Gerenciamento de Cartão de Crédito](./docs/fluxos/gerenciamento%20de%20cartoes.md)
- [Gerenciamento de Categorias](./docs/fluxos/gerenciamentoCategorias.md)
- [Configurações de Notificações](./docs/fluxos/configuracaoNotificacoes.md)
- [Exportação para Calendario](./docs/fluxos/exportaçãoCalendario.md)
- [Configuração de Perfil](./docs/fluxos/configuracaoPerfil)
- [Backup e Restauração](./docs/fluxos/backup.md)

## Casos de Uso

- [Cadastrar Usuário]()
- [Fazer Login]()
- [Criar Lançamento]()
- [Visualizar/Editar/Excluir Lançamento]()
- [Gerar Relatório]()
- [Gerenciar Conta]()
- [Gerenciar Cartão]()
- [Visualizar Fatura]()
- [Gerenciar Categoria/Subcategoria]()
- [Configurar Notificações]()
- [Exportar para Calendário]()
- [Realizar Backup/Restauração]()

## Diagramas

- [Diagrama de Classes](./diagramas/classes.md)
- [Sequência: Cadastrar Usuário](./diagramas/sequencia-cadastrar-usuario.md)
- [Sequência: Criar Lançamento](./diagramas/sequencia-criar-lancamento.md)

## Requisitos

- [Requisitos Funcionais e Não Funcionais](./docs/requisitos/requisitos.md)

## API

- [Documentação da API](./docs/api/swagger.yaml)

## Guias

- [Configuração do Ambiente](./docs/guias/setup.md)
