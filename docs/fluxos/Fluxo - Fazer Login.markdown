🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Fazer Login

Este documento descreve o processo de login de usuário com autenticação tradicional, OAuth e MFA.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de login (Vue.js)"]
    B --> C["Escolhe método"]
    C --> D{"OAuth?"}
    D -->|Sim| E["Frontend redireciona para provedor OAuth"]
    E --> F["Provedor autentica"]
    F --> G{"Bem-sucedida?"}
    G -->|Sim| H["Backend valida usuário em users"]
    H --> I["Gera token JWT"]
    I --> J["Registra ação em logs (ação: login)"]
    J --> K["Frontend redireciona para dashboard"]
    K --> L["Fim"]
    G -->|Não| M["Backend retorna erro 500: 'Falha OAuth'"]
    M --> N["Frontend retorna à tela de login"]
    D -->|Não| O["Insere e-mail e senha"]
    O --> P["Frontend envia POST /api/login"]
    P --> Q{"Credenciais válidas?"}
    Q -->|Sim| R{"MFA ativado? (settings.mfa_enabled)"}
    R -->|Sim| S["Frontend solicita código MFA"]
    S --> T["Usuário insere código"]
    T --> U["Frontend envia POST /api/mfa/verify"]
    U --> V{"Código válido?"}
    V -->|Sim| W["Gera token JWT"]
    W --> J
    V -->|Não| X["Backend retorna erro 422: 'Código inválido'"]
    X --> Y["Frontend retorna à tela de MFA"]
    Y --> T
    R -->|Não| Z["Gera token JWT"]
    Z --> J
    Q -->|Não| AA["Backend retorna erro 401: 'Credenciais inválidas'"]
    AA --> AB["Oferece recuperação de senha"]
    AB --> AC["Usuário solicita via POST /api/password/reset"]
    AC --> AD["Backend envia e-mail com link"]
    AD --> AE["Usuário redefine senha"]
    AE --> N
```

## Descrição do Processo

1. Usuário acessa a tela de login no frontend.
2. Escolhe método:
   - **OAuth**:
     - Redireciona para provedor (Facebook, Google, LinkedIn).
     - Provedor autentica e retorna dados.
     - Backend valida em `users`, gera JWT, registra em `logs`, redireciona para dashboard.
     - Se falhar, exibe erro 500.
   - **Tradicional**:
     - Insere e-mail e senha, envia `POST /api/login`.
     - Backend valida em `users` (bcrypt).
     - Se MFA ativado (`settings.mfa_enabled`):
       - Solicita código (SMS via Twilio ou Google Authenticator).
       - Envia `POST /api/mfa/verify`.
       - Valida código; se válido, gera JWT; se não, erro 422.
     - Se MFA desativado, gera JWT.
     - Registra em `logs`, redireciona para dashboard.
     - Se credenciais inválidas, oferece recuperação via `POST /api/password/reset`.
3. Retorna à tela de login se necessário.

## Regras de Negócio

- Tokens JWT expiram em 24 horas.
- 5 tentativas falhas bloqueiam por 15 minutos.
- Links de redefinição expiram em 1 hora.
- MFA opcional, configurado em `settings`.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios.
- **Backend**: Laravel, Sanctum, Socialite.
- **Endpoints**:
  - `POST /api/login`: `{ email: string, password: string }` → `{ token: string }`.
  - `POST /api/mfa/verify`: `{ code: string }` → `{ token: string }`.
  - `POST /api/password/reset`: `{ email: string }`.
- **Banco**: Tabelas `users`, `settings`, `logs`.
- **Integrações**: Twilio, Google Authenticator, AWS SES, Socialite.
- **Segurança**: JWT, bcrypt, rate limiting.