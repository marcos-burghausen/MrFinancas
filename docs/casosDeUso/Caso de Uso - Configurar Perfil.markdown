🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Configurar Perfil

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    Usuário --> (Configurar Perfil)
```

## Descrição

Permite que o usuário configure seu perfil, incluindo avatar, senha, dados pessoais e tema da interface.

## Atores

- **Primário**: Usuário

## Pré-condições

- Usuário está autenticado (JWT válido).

## Pós-condições

- Perfil é atualizado em `users`.
- Log de ação é registrado em `logs`.

## Fluxo Principal

1. Usuário acessa a tela de perfil no frontend (Vue.js).
2. Escolhe ação:
   - **Alterar Avatar**:
     - Faz upload (PNG/JPG, ≤2MB) ou escolhe predefinido.
     - Frontend envia `POST /api/profile/avatar`.
     - Backend valida e salva em `users.avatar`.
   - **Alterar Senha**:
     - Insere senha atual e nova.
     - Frontend envia `POST /api/profile/password`.
     - Backend valida (senha atual correta, nova forte) e atualiza `users.password`.
   - **Editar Dados Pessoais**:
     - Edita endereço e documentos.
     - Frontend envia `POST /api/profile/data`.
     - Backend valida e salva (documentos criptografados).
   - **Alterar Tema**:
     - Seleciona tema (claro/escuro).
     - Frontend envia `POST /api/profile/theme`.
     - Backend salva em `users.theme`.
3. Backend registra ação em `logs` (ex.: `update_avatar`, `update_password`).
4. Frontend exibe confirmação e retorna à tela.

## Fluxos Alternativos

- **A1**: Arquivo de avatar inválido → Backend retorna erro 422 ("Formato inválido").
- **A2**: Senha atual incorreta → Backend retorna erro 422 ("Senha atual incorreta").
- **A3**: Dados pessoais inválidos → Backend retorna erro 422 ("Dados inválidos").

## Regras de Negócio

- Avatares: PNG/JPG, ≤2MB.
- Senhas: `^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$`.
- Documentos: Criptografados com AES-256.
- Temas: Claro ou escuro, aplicados imediatamente.

## Notas Técnicas

- **Frontend**: Vue.js com Vuetify, Axios.
- **Backend**: Laravel, Eloquent (modelo `User`).
- **Endpoints**:
  - `POST /api/profile/avatar`: Multipart com arquivo.
  - `POST /api/profile/password`: `{ current_password: string, new_password: string }`.
  - `POST /api/profile/data`: `{ address: string, documents: string }`.
  - `POST /api/profile/theme`: `{ theme: string }`.
- **Banco**: Tabela `users` (`avatar`, `password`, `address`, `documents`, `theme`), `logs` (`action`, `details`).
- **Integrações**: AWS S3 para avatares, notificações para confirmações.
- **Segurança**: JWT, HTTPS, bcrypt, AES-256.