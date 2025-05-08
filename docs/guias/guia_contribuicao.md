🔙 [Retornar à documentação principal](../../README.md)

# Guia de Contribuição para o MrFinancas

Obrigado pelo seu interesse em contribuir para o MrFinanças, um aplicativo de gestão financeira pessoal! Este guia fornece diretrizes para ajudar você a contribuir de forma eficaz.

## Código de Conduta

Ao contribuir, você concorda em seguir nosso [Código de Conduta](./CODE_OF_CONDUCT.md), mantendo um ambiente respeitoso, inclusivo e colaborativo para todos.

## Como Posso Contribuir?

### Reportando Bugs

Se encontrar um bug:

1. Verifique se o problema já foi reportado nas [issues](https://github.com/SEU_USUARIO/MrFinancas/issues).
2. Abra uma nova issue com:
   - Título descritivo (e.g., "Erro ao criar transação com valor negativo").
   - Passos para reproduzir o problema.
   - Comportamento esperado vs. atual.
   - Screenshots ou logs, se aplicável.
   - Ambiente: navegador, sistema operacional, versão da aplicação.

### Sugerindo Melhorias

Para propor novas funcionalidades ou melhorias:

1. Consulte as [issues](https://github.com/SEU_USUARIO/MrFinancas/issues) e [requisitos](./docs/requisitos.md) para evitar duplicatas.
2. Abra uma issue descrevendo:
   - A melhoria proposta (e.g., "Adicionar suporte a transações recorrentes").
   - Benefícios para o projeto (e.g., alinhamento com RF05.3).
   - Possíveis desafios técnicos.

### Contribuindo com Código

1. Fork o repositório e crie um branch a partir da `main` (e.g., `feat/add-recurring-transactions`).
2. Adicione testes para novas funcionalidades.
3. Atualize a documentação (e.g., `./docs/documentacao_api.md`, fluxogramas, diagramas de sequência).
4. Certifique-se de que os testes passam.
5. Siga os padrões de estilo do projeto.
6. Envie um pull request (PR) vinculando à issue correspondente.

### Contribuindo com Documentação

- Atualize arquivos em `./docs/` (e.g., `requisitos.md`, `documentacao_api.md`).
- Adicione ou refine fluxogramas e diagramas de sequência em `./docs/fluxos/` e `./docs/sequencias/`.
- Use Markdown e Mermaid para diagramas.
- Exemplo: Adicionar um novo diagrama de sequência para um endpoint em `./docs/sequencias/`.

## Configuração do Ambiente de Desenvolvimento

### Pré-requisitos

- **PHP** 8.1 ou superior
- **Composer** 2.x
- **Node.js** 16.x ou superior
- **NPM** 8.x ou **Yarn** 1.x
- **MySQL** 8.0 ou superior
- **Redis** (para filas)
- Conta AWS (para S3, SNS, SES)
- Conta Google Cloud (para Calendar API)

### Configuração Inicial

1. **Clone o repositório**:

```bash
git clone https://github.com/SEU_USUARIO/MrFinancas.git
cd MrFinancas
```

2. **Instale dependências do backend**:

```bash
composer install
```

3. **Instale dependências do frontend**:

```bash
cd frontend
npm install
# ou
yarn install
```

4. **Configure variáveis de ambiente**:

```bash
cp .env.example .env
```

Edite `.env` com:

- Credenciais MySQL (`DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`).
- Chaves AWS (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_DEFAULT_REGION`, `AWS_BUCKET`).
- Google OAuth (`GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET`).
- Configurações de e-mail (`MAIL_MAILER`, `MAIL_HOST`, `MAIL_USERNAME`, `MAIL_PASSWORD`).

5. **Gere a chave da aplicação**:

```bash
php artisan key:generate
```

6. **Execute migrações do banco**:

```bash
php artisan migrate
```

7. **Inicie o servidor backend**:

```bash
php artisan serve
```

8. **Inicie o servidor frontend**:

```bash
cd frontend
npm run dev
# ou
yarn dev
```

9. **Configure Redis** (opcional, para filas):

   - Instale Redis localmente ou use um serviço gerenciado.
   - Atualize `.env` (`QUEUE_CONNECTION=redis`, `REDIS_HOST`, `REDIS_PASSWORD`, `REDIS_PORT`).

10. **Configure Google Calendar API**:
    - Crie um projeto no [Google Cloud Console](https://console.cloud.google.com/).
    - Habilite a API do Google Calendar.
    - Gere credenciais OAuth 2.0 e adicione ao `.env`.

## Estrutura do Projeto

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
└── .env.example          # Modelo de variáveis de ambiente
```

## Diretrizes de Estilo

### Estilo de Código

- **Backend (PHP/Laravel)**:

  - Siga [PSR-12](https://www.php-fig.org/psr/psr-12/).
  - Use [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) (`composer run-script lint`).
  - Nomeie modelos Eloquent no singular (e.g., `Transaction`, não `Transactions`).
  - Use camelCase para métodos e snake_case para colunas do banco.

- **Frontend (Vue.js)**:

  - Use [Prettier](https://prettier.io/) para formatação.
  - Siga [ESLint](https://eslint.org/) com regras do projeto (`npm run lint`).
  - Nomeie componentes em PascalCase (e.g., `TransactionForm.vue`).
  - Use camelCase para variáveis e funções.

- **Comentários**:
  - Comente lógica complexa ou integrações (e.g., Google Calendar API).
  - Use PHPDoc para métodos no backend.

### Commits

- Siga [Conventional Commits](https://www.conventionalcommits.org/).
- Exemplos:
  - `feat(transactions): adiciona endpoint para transações recorrentes`
  - `fix(cards): corrige cálculo de limite disponível`
  - `docs(api): atualiza documentação de POST /calendar/export`
  - `test(profile): adiciona testes para PUT /profile/avatar`

### Documentação

- Atualize `./docs/documentacao_api.md` para novos endpoints.
- Adicione fluxogramas ou diagramas de sequência em `./docs/fluxos/` ou `./docs/sequencias/` para novas funcionalidades.
- Refira-se aos requisitos em `./docs/requisitos.md` (e.g., RF05 para transações).
- Use Markdown e Mermaid para consistência.

## Processo de Review

- Todo pull request deve:
  - Estar vinculado a uma issue.
  - Passar em todos os testes (backend e frontend).
  - Incluir atualizações na documentação, se aplicável.
- Revisores fornecerão feedback construtivo dentro de 48 horas.
- Resolva comentários antes da mesclagem.

## Testes

- **Backend**: Use PHPUnit para testes unitários e de integração.
  - Execute: `php artisan test`
  - Cubra controladores, modelos e serviços.
- **Frontend**: Use Jest ou Vitest para testes de componentes.
  - Execute: `cd frontend && npm test`
  - Cubra componentes e chamadas à API.
- Certifique-se de que a cobertura de testes é mantida (mínimo 80%).

## Recursos Adicionais

- [Requisitos do Sistema](./docs/requisitos.md)
- [Documentação da API](./docs/documentacao_api.md)
- [Diagramas de Sequência](./docs/sequencias/)
- [Fluxogramas](./docs/fluxos/)
- [Laravel Documentation](https://laravel.com/docs)
- [Vue.js Documentation](https://vuejs.org/guide/introduction.html)
- [Google Calendar API](https://developers.google.com/calendar/api)

## Dúvidas?

- Abra uma [issue](https://github.com/SEU_USUARIO/MrFinancas/issues) no GitHub.
- Entre em contato via [contato@mrfinancas.com](mailto:contato@mrfinancas.com).

Agradecemos sua contribuição para tornar o MrFinanças ainda melhor!
