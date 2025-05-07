# MrFinancas - Sistema de Gestão Financeira Pessoal

obs:essa documentação ainda esta em desenvolvimento/planejamento

![Logo MrFinancas](https://github.com/marcos-burghausen/MrFinancas/raw/master/assets/logo.png)

## 📌 Status do Projeto

[![Versão](https://img.shields.io/badge/Versão-0.0.5-blue.svg)](https://github.com/marcos-burghausen/MrFinancas/releases)

<!-- [![Licença](https://img.shields.io/badge/Licença-MIT-green.svg)](LICENSE) -->

[![Status](https://img.shields.io/badge/Status-Aguardando%20Início-red.svg)](https://github.com/marcos-burghausen/MrFinancas)
[![Documentação](https://img.shields.io/badge/📚_Documentação-Em_Desenvolvimento-orange)](./docs)

## 📑 Índice

- [Sobre o Projeto](#-sobre-o-projeto)
- [Objetivo](#-objetivo)
- [Funcionalidades](#-funcionalidades-principais)
- [Tecnologias Utilizadas](#%EF%B8%8F-tecnologias-utilizadas)
- [Arquitetura do Sistema](#-arquitetura-do-sistema)
- [Instalação e Configuração](#-instalação-e-configuração)
- [Estrutura do Projeto](#%EF%B8%8F-estrutura-do-projeto)
- [Modelos do Sistema](#-modelos-do-sistema)
- [Documentação](#-documentação-completa)
- [Fluxos do Sistema](#-fluxos-do-sistema)
- [Casos de Uso](#-casos-de-uso)
- [Diagramas](#-diagramas)
- [API](#-api)
- [Testes](#-testes)
- [Diagrama ER](./docs/guias/user_guide.md)
- [Roadmap](#%EF%B8%8F-roadmap)
- [Como Contribuir](#-como-contribuir)
- [Segurança e Privacidade](#-segurança-e-privacidade)
- [FAQ](#-perguntas-frequentes-faq)
- [Licença](#-licença)
- [Contato](#-contato)

## 📋 Sobre o Projeto

Apaixonado pelo mercado financeiro, desenvolvi o MrFinanças a partir de uma necessidade pessoal: criar uma aplicação prática para consolidar meus estudos e atender às metas do meu Plano de Desenvolvimento Individual (PDI). Como usuário de um aplicativo pago de gestão financeira, decidi construir uma solução própria, mais acessível e personalizada.

O MrFinanças é um sistema de gestão financeira pessoal projetado para capacitar pessoas a organizar suas finanças, monitorar receitas e despesas e tomar decisões financeiras mais inteligentes. Simples, eficiente e intuitivo, o projeto reúne funcionalidades essenciais em uma única plataforma, oferecendo uma alternativa prática para o controle financeiro pessoal. Para futuras versões, pretendo expandir o sistema com um módulo de controle de investimentos voltado para profissionais do mercado financeiro, agregando ainda mais valor à gestão patrimonial.

## 🎯 Objetivo

O objetivo do MrFinancas é fornecer uma ferramenta abrangente para o controle de finanças pessoais e, futuramente, para o gerenciamento de investimentos. Ele visa resolver problemas comuns enfrentados por indivíduos ao tentar organizar suas finanças, como:

- Falta de visibilidade sobre despesas e receitas
- Dificuldade em planejar orçamentos
- Organização de contas a pagar e receber
- Acompanhamento de faturas de cartão de crédito
- Monitoramento de investimentos de forma eficiente
- Criação de metas financeiras realistas

## 🚀 Funcionalidades Principais

### 1. **Cadastro de Usuários**

- **Métodos de Cadastro/Login**:
  - Tradicional (e-mail e senha)
  - Facebook
  - Google
  - LinkedIn
- **Perfis de Usuário**:
  - USER: Acesso básico à gestão financeira
  - TRADER: Acesso ao módulo de investimentos
  - USER_TRADER: Acesso completo à gestão financeira e investimentos
  - ADMIN: Acesso administrativo ao sistema
  - FULL: Acesso completo a todas as funcionalidades

### 2. **Lançamentos Financeiros**

- **Tipos de Lançamentos**:
  - Despesas (fixas e variáveis)
  - Receitas (fixas e variáveis)
  - Despesas de cartão de crédito
  - Estornos e reembolsos
- **Características**:
  - Categorização e subcategorização
  - Recorrência (única, mensal, semanal, anual)
  - Anexos de comprovantes
  - Notas e observações
  - Status (pago, pendente, atrasado)

### 3. **Contas e Cartões de Crédito**

- **Contas**:
  - Múltiplas contas bancárias
  - Saldo inicial e atual
  - Histórico de transações
  - Extratos personalizados
- **Cartões de Crédito**:
  - Vinculação a contas bancárias
  - Controle de limite
  - Fechamento e vencimento de fatura
  - Parcelamento de compras

### 4. **Perfil do Usuário**

- Avatar personalizado
- Dados pessoais e endereço
- Preferências de sistema
- Gerenciamento de dispositivos conectados
- Configurações de privacidade

### 5. **Relatórios e Gráficos**

- **Tipos de Relatórios**:
  - Diários, semanais, mensais, trimestrais e anuais
  - Comparativos entre períodos
  - Previsões com base em histórico
- **Visualizações**:
  - Gráficos de pizza para distribuição de despesas
  - Gráficos de linha para evolução financeira
  - Gráficos de barras para comparativos

### 6. **Alertas e Notificações**

- Avisos de vencimento de contas
- Alertas de limite de cartão
- Notificações de estornos
- Avisos de desvios do orçamento
- Lembretes personalizáveis

### 7. **Orçamento Financeiro**

- Planejamento mensal e anual
- Metas por categoria
- Acompanhamento em tempo real
- Recomendações de ajustes
- Comparativo entre planejado e realizado

### 8. **Recursos Adicionais**

- Exportação para calendários (Google, Outlook)
- Backup e restauração de dados
- Sincronização entre dispositivos
- Modo offline com sincronização posterior
- Importação de extratos bancários (CSV, OFX)

## 🛠️ Tecnologias Utilizadas

### Frontend

- **Framework Principal**: Vue.js 3
- **UI Framework**: Vuetify 3
- **Gerenciamento de Estado**: Pinia
- **Roteamento**: Vue Router
- **HTTP Client**: Axios
- **Gráficos**: Chart.js e D3.js
- **Calendário**: FullCalendar
- **Formatação de Datas**: Moment.js

### Backend

- **Linguagem**: PHP 8.2+
- **Framework**: Laravel 10
- **Banco de Dados**: MySQL 8.0
- **Cache**: Redis
- **API**: RESTful com autenticação JWT
- **Filas e Jobs**: Laravel Queue

### DevOps

- **Controle de Versão**: Git e GitHub
- **CI/CD**: GitHub Actions
- **Containerização**: Docker
- **Hospedagem**: AWS (Amazon Web Services)
- **Monitoramento**: New Relic

### Ferramentas de Desenvolvimento

- **IDE**: Visual Studio Code, PHPStorm
- **Testes**: PHPUnit, Jest
- **Linting**: ESLint, PHP_CodeSniffer
- **Documentação**: OpenAPI (Swagger)

## 🏗 Arquitetura do Sistema

O MrFinancas utiliza uma arquitetura cliente-servidor com separação clara entre frontend e backend:

### Camada de Apresentação (Frontend)

- Interface responsiva baseada em componentes Vue.js
- PWA (Progressive Web App) para funcionamento offline
- Design mobile-first com adaptação para todos os dispositivos

### Camada de Aplicação (Backend)

- API RESTful para comunicação com o frontend
- Controladores para lógica de negócios
- Serviços para operações complexas
- Middleware para autenticação e autorização

### Camada de Dados

- Modelos Eloquent ORM
- Migrações e seeds para estrutura de banco
- Validação de dados com regras de negócio

### Integração

- Webhooks para serviços externos
- WebSockets para atualizações em tempo real
- Filas assíncronas para operações pesadas

## 📦 Instalação e Configuração

### Pré-requisitos

- Node.js (v16+)
- PHP 8.2+
- Composer
- MySQL 8.0
- Redis (opcional, para cache)
- Git

### Backend

1. Clone o repositório:

```bash
git clone https://github.com/marcos-burghausen/MrFinancas.git
cd MrFinancas/backend
```

2. Instale as dependências:

```bash
composer install
```

3. Configure o ambiente:

```bash
cp .env.example .env
php artisan key:generate
```

4. Configure o banco de dados no arquivo `.env`

5. Execute as migrações:

```bash
php artisan migrate --seed
```

6. Inicie o servidor:

```bash
php artisan serve
```

### Frontend

1. Navegue para o diretório frontend:

```bash
cd ../frontend
```

2. Instale as dependências:

```bash
npm install
```

3. Configure o ambiente:

```bash
cp .env.example .env.local
```

4. Inicie o servidor de desenvolvimento:

```bash
npm run dev
```

5. Para build de produção:

```bash
npm run build
```

## 🗂️ Estrutura do Projeto

```
MrFinancas/
├── backend/              # Código backend (Laravel)
│   ├── app/              # Core da aplicação
│   │   ├── Console/      # Comandos de CLI
│   │   ├── Exceptions/   # Manipulação de exceções
│   │   ├── Http/         # Controladores, Middleware, Requests
│   │   ├── Models/       # Modelos Eloquent
│   │   └── Services/     # Serviços da aplicação
│   ├── config/           # Configurações
│   ├── database/         # Migrações e seeds
│   ├── routes/           # Definição de rotas
│   └── tests/            # Testes automatizados
├── frontend/             # Código frontend (Vue.js)
│   ├── public/           # Arquivos públicos
│   ├── src/              # Código fonte
│   │   ├── assets/       # Recursos estáticos
│   │   ├── components/   # Componentes Vue
│   │   ├── layouts/      # Layouts da aplicação
│   │   ├── pages/        # Páginas da aplicação
│   │   ├── router/       # Configuração de rotas
│   │   ├── services/     # Serviços e APIs
│   │   ├── store/        # Gerenciamento de estado (Pinia)
│   │   └── utils/        # Utilitários
│   └── tests/            # Testes de frontend
├── docs/                 # Documentação
│   ├── api/              # Documentação da API
│   ├── diagramas/        # Diagramas UML
│   ├── fluxos/           # Fluxos do sistema
│   ├── guias/            # Guias de utilização
│   ├── imagens/          # Imagens da documentação
│   └── requisitos/       # Documentação de requisitos
└── docker/               # Configurações Docker
    ├── mysql/            # Configurações do MySQL
    ├── nginx/            # Configurações do Nginx
    ├── php/              # Configurações do PHP
    └── redis/            # Configurações do Redis
```

## 📊 Modelos do Sistema

### Diagrama de Entidade-Relacionamento

[![Diagrama ER](https://img.shields.io/badge/Diagrama_ER-Atualizado-green?logo=diagramsdotnet&logoColor=white)](https://github.com/marcos-burghausen/MrFinancas/raw/master/docs/imagens/diagrama_er.png)

### Interface do Usuário

[![UI Design](https://img.shields.io/badge/Interface-Protótipo_1.0-blue?logo=figma&logoColor=white)](https://github.com/marcos-burghausen/MrFinancas/raw/master/assets/interface.png)

### Protótipos de Tela

- [Dashboard Principal](./docs/wireframes/dashboard.md)
- [Tela de Lançamentos](./docs/wireframes/lancamentos.md)
- [Relatórios Financeiros](./docs/wireframes/relatorios.md)
- [Configurações de Perfil](./docs/wireframes/perfil.md)

## 📚 Documentação Completa

A documentação completa do projeto está disponível nos seguintes locais:

- **Documentação online**: [docs.mrfinancas.com](https://docs.mrfinancas.com) (em breve)
- **Documentação local**: Diretório `/docs` do repositório
- **Wiki do GitHub**: [wiki](https://github.com/marcos-burghausen/MrFinancas/wiki) (em breve)

## 🔄 Fluxos do Sistema

Todos os fluxos principais do sistema estão documentados em detalhe:

- [Cadastro de Usuário](./docs/fluxos/cadastroUsuario.md)
- [Login e Autenticação](./docs/fluxos/login.md)
- [Gerenciamento de Contas](./docs/fluxos/gerenciamentoContas.md)
- [Gerenciamento de Cartão de Crédito](./docs/fluxos/gerenciamento_de_cartoes.md)
- [Gerenciamento de Categorias](./docs/fluxos/gerenciamentoCategorias.md)
- [Configurações de Notificações](./docs/fluxos/configuracaoNotificacoes.md)
- [Exportação para Calendário](./docs/fluxos/exportacaoCalendario.md)
- [Configuração de Perfil](./docs/fluxos/configuracaoPerfil.md)
- [Backup e Restauração](./docs/fluxos/backup.md)
- [Geração de Relatórios](./docs/fluxos/geracao_relatorios.md)
- [Planejamento Orçamentário](./docs/fluxos/planejamento_orcamentario.md)

## 👤 Casos de Uso

Os casos de uso detalham as interações do usuário com o sistema:

- [Cadastrar Usuário](./docs/casos_uso/cadastrar_usuario.md)
- [Fazer Login](./docs/casos_uso/fazer_login.md)
- [Criar Lançamento](./docs/casos_uso/criar_lancamento.md)
- [Visualizar/Editar/Excluir Lançamento](./docs/casos_uso/gerenciar_lancamento.md)
- [Gerar Relatório](./docs/casos_uso/gerar_relatorio.md)
- [Gerenciar Conta](./docs/casos_uso/gerenciar_conta.md)
- [Gerenciar Cartão](./docs/casos_uso/gerenciar_cartao.md)
- [Visualizar Fatura](./docs/casos_uso/visualizar_fatura.md)
- [Gerenciar Categoria/Subcategoria](./docs/casos_uso/gerenciar_categoria.md)
- [Configurar Notificações](./docs/casos_uso/configurar_notificacoes.md)
- [Exportar para Calendário](./docs/casos_uso/exportar_calendario.md)
- [Realizar Backup/Restauração](./docs/casos_uso/backup_restauracao.md)
- [Criar e Gerenciar Metas](./docs/casos_uso/gerenciar_metas.md)
- [Importar Extrato Bancário](./docs/casos_uso/importar_extrato.md)

## 📐 Diagramas

Os diagramas UML fornecem uma visualização técnica do sistema:

- [Diagrama de Classes](./docs/diagramas/classes.md)
- [Diagrama de Componentes](./docs/diagramas/componentes.md)
- [Diagrama de Implantação](./docs/diagramas/implantacao.md)
- [Sequência: Cadastrar Usuário](./docs/diagramas/sequencia-cadastrar-usuario.md)
- [Sequência: Criar Lançamento](./docs/diagramas/sequencia-criar-lancamento.md)
- [Sequência: Gerar Relatório](./docs/diagramas/sequencia-gerar-relatorio.md)
- [Sequência: Processar Fatura](./docs/diagramas/sequencia-processar-fatura.md)

## 🔌 API

A documentação completa da API REST está disponível para integrações:

- [Visão Geral da API](./docs/api/visao_geral.md)
- [Autenticação e Autorização](./docs/api/autenticacao.md)
- [Endpoints de Usuários](./docs/api/endpoints_usuarios.md)
- [Endpoints de Lançamentos](./docs/api/endpoints_lancamentos.md)
- [Endpoints de Contas](./docs/api/endpoints_contas.md)
- [Endpoints de Cartões](./docs/api/endpoints_cartoes.md)
- [Endpoints de Categorias](./docs/api/endpoints_categorias.md)
- [Endpoints de Relatórios](./docs/api/endpoints_relatorios.md)
- [Documentação Swagger](./docs/api/swagger.md)

## 🧪 Testes

O projeto inclui uma suíte de testes automatizados:

### Testes de Backend

```bash
cd backend
php artisan test
```

### Testes de Frontend

```bash
cd frontend
npm run test
```

### Cobertura de Testes

```bash
cd backend
php artisan test --coverage
```

## 🗺️ Roadmap

Nossa visão para o futuro do MrFinancas:

![Versão](https://img.shields.io/badge/Versão-1.0.0-blue.svg)
![MVP Status](https://img.shields.io/badge/MVP-Q4_2025-blue)
[![Status](https://img.shields.io/badge/Status-Em%20Planejamento-yellow.svg)](https://github.com/marcos-burghausen/MrFinancas)

- [ ] Autenticação de usuários (e-mail/senha).
- [ ] Cadastro de receitas/despesas e categorização.
- [ ] Dashboard com gráficos simples (Chart.js).
- [ ] Controle básico de contas e cartões.
- [ ] Notificações por e-mail (vencimentos).

![Versão](https://img.shields.io/badge/Versão-1.1-blue.svg)
![Release Timeline](https://img.shields.io/badge/Release-Q1_2026-blue)

<!-- [![Status](https://img.shields.io/badge/Status-Em%20Planejamento-yellow.svg)](https://github.com/marcos-burghausen/MrFinancas) -->

- [ ] Implementação de notificações para pagamentos próximos
- [ ] Melhorias na interface de usuário e experiência

![Versão](https://img.shields.io/badge/Versão-1.2-blue.svg)
![Release Timeline](https://img.shields.io/badge/Release-Q2_2026-blue)

<!-- [![Status](https://img.shields.io/badge/Status-Em%20Planejamento-yellow.svg)](https://github.com/marcos-burghausen/MrFinancas) -->

- [ ] Integração com instituições financeiras via Open Banking
- [ ] Importação automática de transações

![Versão](https://img.shields.io/badge/Versão-2.0-blue.svg)
![Release Timeline](https://img.shields.io/badge/Release-Q3_2026-blue)

<!-- [![Status](https://img.shields.io/badge/Status-Em%20Planejamento-yellow.svg)](https://github.com/marcos-burghausen/MrFinancas) -->

- [ ] Aplicativo móvel nativo para iOS e Android
- [ ] Sincronização em tempo real entre dispositivos

![Versão](https://img.shields.io/badge/Versão-2.5-blue.svg)
![Release Timeline](https://img.shields.io/badge/Release-Q4_2026-blue)

<!-- [![Status](https://img.shields.io/badge/Status-Em%20Planejamento-yellow.svg)](https://github.com/marcos-burghausen/MrFinancas) -->

- [ ] Sistema de metas financeiras avançado
- [ ] Recomendações personalizadas de economia

### Versão 3.0 (2027)

![Versão](https://img.shields.io/badge/Versão-3.0-blue.svg)
![Release Timeline](https://img.shields.io/badge/Release-Q4_2027-blue)

<!-- [![Status](https://img.shields.io/badge/Status-Em%20Planejamento-yellow.svg)](https://github.com/marcos-burghausen/MrFinancas) -->

- [ ] Módulo completo de investimentos
- [ ] Análise preditiva e inteligência artificial

## 👥 Como Contribuir

Sua contribuição é bem-vinda! Siga os passos:

1. Verifique as [issues abertas](https://github.com/marcos-burghausen/MrFinancas/issues) ou crie uma nova
2. Faça um fork do projeto
3. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
4. Faça commit das suas alterações (`git commit -m 'Add some AmazingFeature'`)
5. Faça push para a branch (`git push origin feature/AmazingFeature`)
6. Abra um Pull Request

Para mais detalhes, consulte nosso [Guia de Contribuição](./docs/guias/guia_contribuicao.md).

## 🔒 Segurança e Privacidade

O MrFinancas prioriza a segurança dos dados financeiros dos usuários:

- Criptografia de dados sensíveis
- Autenticação de dois fatores
- Conformidade com LGPD/GDPR
- Backups regulares e automáticos
- Monitoramento de atividades suspeitas

Para reportar vulnerabilidades de segurança, entre em contato diretamente via security@mrfinancas.com.

## ❓ Perguntas Frequentes (FAQ)

**P: O MrFinancas é gratuito?**
R: Sim, o MrFinancas possui uma versão gratuita com funcionalidades básicas. Existem planos premium com recursos avançados.

**P: Posso acessar meus dados offline?**
R: Sim, o aplicativo web funciona como PWA, permitindo acesso offline e sincronização posterior.

**P: Como são armazenados meus dados financeiros?**
R: Todos os dados são criptografados e armazenados em servidores seguros. Você tem controle total sobre seus dados.

<!-- **P: O sistema integra com meu banco?**
R: Estamos desenvolvendo integrações com os principais bancos através de APIs de Open Banking. -->

Para mais perguntas, consulte nossa [FAQ completa](./docs/faq.md).

## 📜 Licença

Este projeto está licenciado sob a [Licença MIT](LICENSE).

## 📞 Contato

Marcos Burghausen - [GitHub](https://github.com/marcos-burghausen)

Link do Projeto: [https://github.com/marcos-burghausen/MrFinancas](https://github.com/marcos-burghausen/MrFinancas)

---

⭐️ Se este projeto foi útil para você, considere deixar uma estrela no GitHub!
