🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Fazer Login

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    actor ProvedorOAuth
    Usuário --> (Fazer Login)
    Usuário --> ProvedorOAuth : usa
```

## Descrição

Permite que o usuário acesse o sistema via e-mail/senha ou OAuth (Facebook, Google, LinkedIn), com suporte a autenticação multifator (MFA) e recuperação de senha.

## Atores

- **Primário**: Usuário
- **Secundário**: Provedor OAuth

## Pré-condições

- Usuário possui conta verificada (tradicional) ou registrada (OAuth).
- Provedores OAuth estão configurados no backend.

## Pós-condições

- Usuário recebe token JWT e é redirecionado ao dashboard.
- Senha é redefinida (se recuperação solicitada).
- Log de login é registrado na tabela `logs`.

## Fluxo Principal

1. Usuário acessa a tela de login no frontend (Vue.js).
2. Usuário escolhe método:
   - **Tradicional**:
     - Insere e-mail e senha no formulário.
     - Frontend envia `POST /api/login` com `{ email, password }`.
     - Backend valida credenciais na tabela `users` (email único, senha com bcrypt).
     - Se MFA ativado (verificado em `settings.mfa_enabled`), solicita código via SMS (Twilio) ou app autenticador (Google Authenticator).
     - Backend gera token JWT (Laravel Sanctum) e retorna ao frontend.
     - Frontend redireciona para o dashboard.
   - **OAuth**:
     - Usuário clica em botão OAuth (ex.: "Entrar com Google").
     - Frontend redireciona para provedor via Laravel Socialite.
     - Provedor autentica e retorna dados (e-mail, nome).
     - Backend valida usuário em `users`, cria se necessário, gera token JWT.
     - Frontend redireciona para o dashboard.
3. Backend registra login na tabela `logs` (ação: `login`, detalhes: e-mail, timestamp).

## Fluxos Alternativos

- **A1**: Credenciais inválidas → Backend retorna erro 401 ("E-mail ou senha inválidos") após 5 tentativas, bloqueia temporariamente.
- **A2**: Código MFA inválido → Backend retorna erro 422 ("Código inválido") e solicita novo código.
- **A3**: Recuperação de senha → Usuário clica em "Esqueceu a senha?", backend envia link via `POST /api/password/reset`, usuário redefine senha e retorna ao login.
- **A4**: Falha OAuth → Backend retorna erro 500 ("Falha na autenticação OAuth") e redireciona à tela de login.

## Regras de Negócio

- Usuários não verificados (`users.email_verified = false`) não podem logar (tradicional).
- MFA é opcional, configurado em `settings.mfa_enabled`.
- Links de redefinição expiram em 1 hora.
- Tokens JWT expiram em 24 horas.

## Notas Técnicas

- **Frontend**: Vue.js com Vuetify (formulário reativo), Axios para requisições.
- **Backend**: Laravel com Sanctum (tokens), Socialite (OAuth), Eloquent (modelo `User`).
- **Endpoints**:
  - `POST /api/login`: `{ email: string, password: string }` → `{ token: string }`.
  - `POST /api/password/reset`: `{ email: string }` → Envia link.
- **Banco**: Tabela `users` (`email`, `password`, `email_verified`); `settings` (`mfa_enabled`, `mfa_method`); `logs` (`action`, `details`).
- **Integrações**:
  - Twilio/Google Authenticator para MFA.
  - Laravel Socialite para OAuth.
  - AWS SES (via Email Service) para e-mails de recuperação.
- **Segurança**: HTTPS, bcrypt para senhas, rate limiting (5 tentativas).
