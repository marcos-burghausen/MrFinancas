🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Cadastrar Usuário

Este documento descreve o processo de cadastro de usuário com autenticação tradicional ou OAuth.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de cadastro (Vue.js)"]
    B --> C["Escolhe método"]
    C --> D{"OAuth?"}
    D -->|Sim| E["Frontend redireciona para provedor OAuth"]
    E --> F["Provedor autentica"]
    F --> G{"Bem-sucedida?"}
    G -->|Sim| H["Backend cria/atualiza usuário em users"]
    H --> I["Envia e-mail de boas-vindas via AWS SES"]
    I --> J["Registra ação em logs (ação: register)"]
    J --> K["Frontend redireciona para dashboard"]
    K --> L["Fim"]
    G -->|Não| M["Backend retorna erro 500: 'Falha OAuth'"]
    M --> N["Frontend retorna à tela de cadastro"]
    D -->|Não| O["Preenche: { email, password, name }"]
    O --> P["Frontend envia POST /api/register"]
    P --> Q{"Dados válidos?"}
    Q -->|Sim| R["Backend cria usuário com email_verified=false"]
    R --> S["Envia e-mail de verificação via AWS SES"]
    S --> T["Registra ação em logs (ação: register)"]
    T --> U["Frontend exibe: 'Verifique seu e-mail'"]
    U --> V["Usuário clica em GET /api/verify-email"]
    V --> W{"Link válido?"}
    W -->|Sim| X["Backend marca email_verified=true"]
    X --> Y["Frontend redireciona para login"]
    Y --> L
    W -->|Não| Z["Backend retorna erro 422: 'Link inválido'"]
    Z --> L
    Q -->|Não| AA["Backend retorna erro 422: 'E-mail já registrado'"]
    AA --> AB["Frontend retorna ao formulário"]
    AB --> O
```

## Descrição do Processo

1. Usuário acessa a tela de cadastro no frontend.
2. Escolhe método:
   - **OAuth**:
     - Redireciona para provedor (Facebook, Google, LinkedIn).
     - Provedor autentica, retorna `{ email, name }`.
     - Backend cria/atualiza em `users` (`email_verified=true`), envia e-mail de boas-vindas, registra em `logs`, redireciona para dashboard.
     - Se falhar, exibe erro 500.
   - **Tradicional**:
     - Preenche formulário: `{ email, password, name }`.
     - Frontend envia `POST /api/register`.
     - Backend valida: e-mail único, senha forte (`^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$`).
     - Cria usuário (`email_verified=false`), envia e-mail de verificação, registra em `logs`.
     - Usuário clica em link (`GET /api/verify-email`); se válido, marca `email_verified=true`, redireciona para login; se não, exibe erro 422.
3. Retorna ao formulário se inválido.

## Regras de Negócio

- E-mails únicos (`users.email`).
- Senhas: Mínimo 8 caracteres, letras e números.
- Links de verificação expiram em 24 horas.
- OAuth não requer verificação.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios.
- **Backend**: Laravel, Socialite, Eloquent (modelo `User`).
- **Endpoints**:
  - `POST /api/register`: `{ email: string, password: string, name: string }`.
  - `GET /api/verify-email?token`: Valida.
- **Banco**: Tabelas `users`, `logs`.
- **Integrações**: AWS SES, Socialite.
- **Segurança**: JWT, bcrypt, HTTPS.