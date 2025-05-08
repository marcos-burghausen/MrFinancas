🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Fazer Login

Este diagrama descreve as interações para o login do usuário com e-mail e senha.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)

    U->>F: Acessa tela de login
    F->>U: Exibe formulário
    U->>F: Preenche e-mail e senha
    F->>B: POST /auth/login (email, senha)
    B->>DB: Busca usuário por e-mail
    DB-->>B: Usuário encontrado
    B->>B: Valida senha (bcrypt)
    B->>DB: Gera token JWT
    DB-->>B: Token gerado
    B-->>F: Resposta: { token, usuario }
    F->>F: Armazena token (localStorage)
    F-->>U: Redireciona para dashboard
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de login no frontend.
2. Preenche e-mail e senha e envia o formulário.
3. O frontend envia `POST /auth/login` com `{ email, senha }`.
4. O backend busca o usuário no banco pela coluna `email`.
5. Valida a senha usando bcrypt.
6. Gera um token JWT e retorna `{ token, usuario: { id, nome, email } }`.
7. O frontend armazena o token em `localStorage`.
8. Redireciona o usuário para o dashboard.

### Fluxos Alternativos

- **A1**: E-mail não encontrado → Backend retorna erro 401 ("Credenciais inválidas").
- **A2**: Senha incorreta → Backend retorna erro 401 ("Credenciais inválidas").
- **A3**: Erro no banco → Backend retorna erro 500 e registra falha.

### Notas Técnicas

- **Frontend**: Vue.js com Vuetify para formulário, Axios para requisições.
- **Backend**: Laravel com Sanctum para autenticação.
  - Endpoint: `POST /auth/login` (controlador `AuthController`).
  - Modelo: `User` (colunas: `id`, `email`, `password`, `name`).
- **Banco**: Tabela `users` para autenticação.
- **Segurança**: Senha criptografada com bcrypt; token JWT com expiração.
- **Integração**: Suporta login social (planejado, e.g., `/auth/google`).