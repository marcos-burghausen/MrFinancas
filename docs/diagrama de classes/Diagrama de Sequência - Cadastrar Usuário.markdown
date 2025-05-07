🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Cadastrar Usuário

Este diagrama descreve as interações entre o usuário, o frontend (Vue.js), o backend (Laravel) e o sistema de e-mails para o cadastro tradicional com verificação de e-mail.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)
    participant E as Sistema de E-mails

    U->>F: Acessa tela de cadastro
    F->>U: Exibe formulário
    U->>F: Preenche formulário (e-mail, senha, nome)
    F->>B: POST /api/register (e-mail, senha, nome)
    B->>DB: Verifica se e-mail é único
    DB-->>B: E-mail disponível
    B->>DB: Cria usuário (status: não verificado)
    DB-->>B: Usuário criado
    B->>E: Envia e-mail de verificação
    E-->>B: E-mail enviado
    B-->>F: Resposta: "Verifique seu e-mail"
    F-->>U: Exibe mensagem
    U->>E: Clica no link de verificação
    E->>B: GET /api/verify-email?token
    B->>DB: Valida token
    DB-->>B: Token válido
    B->>DB: Marca usuário como verificado
    DB-->>B: Usuário atualizado
    B-->>U: Redireciona para login
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de cadastro no frontend (Vue.js).
2. Preenche o formulário (e-mail, senha, nome) e envia.
3. O frontend faz uma requisição POST ao backend (endpoint `/api/register`).
4. O backend verifica se o e-mail é único no banco.
5. Cria o usuário com status "não verificado" e salva no banco.
6. Envia um e-mail de verificação via sistema de e-mails (ex.: Laravel Mail).
7. Retorna mensagem ao frontend, que exibe "Verifique seu e-mail".
8. O usuário clica no link de verificação, acionando o endpoint `/api/verify-email`.
9. O backend valida o token, atualiza o status do usuário e redireciona para a tela de login.

### Fluxos Alternativos

- **A1**: E-mail já registrado → Backend retorna erro 422 ("E-mail já registrado").
- **A2**: Token inválido → Backend retorna erro ("Link inválido") e oferece reenvio.
- **A3**: Erro no envio de e-mail → Backend registra falha e notifica ADMIN (futuro).

### Notas Técnicas

- **Frontend**: Usa Vue.js com Axios para requisições HTTP.
- **Backend**: Usa Laravel com Eloquent para manipulação do modelo `User`.
  - Endpoint: `POST /api/register` (controlador `AuthController`).
  - Endpoint: `GET /api/verify-email` (valida token).
- **Banco**: Tabela `users` com colunas `email`, `password`, `name`, `email_verified`.
- **E-mails**: Usa Laravel Mail com filas (ex.: Redis) para envio assíncrono.
- **Segurança**: Senha é criptografada com bcrypt; token de verificação é único e temporário.
