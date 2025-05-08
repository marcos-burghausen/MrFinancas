🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Configurar Perfil

Este documento descreve o processo de configuração do perfil do usuário, incluindo avatar, senha, dados pessoais e temas.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de perfil (Vue.js)"]
    B --> C["Escolhe ação"]
    C --> D{"Alterar avatar?"}
    D -->|Sim| E["Faz upload ou escolhe avatar predefinido"]
    E --> F["Frontend envia POST /api/profile/avatar"]
    F --> G{"Arquivo válido? (PNG/JPG, <=2MB)"}
    G -->|Sim| H["Backend salva avatar em users.avatar"]
    H --> I["Registra ação em logs (ação: update_avatar)"]
    I --> J["Frontend exibe: 'Avatar atualizado'"]
    J --> K["Retorna ao perfil"]
    G -->|Não| L["Backend retorna erro 422: 'Formato inválido'"]
    L --> M["Frontend retorna ao formulário"]
    M --> E
    D -->|Não| N{"Alterar senha?"}
    N -->|Sim| O["Insere senha atual e nova"]
    O --> P["Frontend envia POST /api/profile/password"]
    P --> Q{"Senhas válidas? (atual correta, nova forte)"}
    Q -->|Sim| R["Backend atualiza users.password"]
    R --> S["Registra ação em logs (ação: update_password)"]
    S --> T["Frontend exibe: 'Senha atualizada'"]
    T --> K
    Q -->|Não| U["Backend retorna erro 422: 'Senha atual incorreta'"]
    U --> V["Frontend retorna ao formulário"]
    V --> O
    N -->|Não| W{"Editar dados pessoais?"}
    W -->|Sim| X["Edita: endereço, documentos"]
    X --> Y["Frontend envia POST /api/profile/data"]
    Y --> Z["Backend valida e salva em users"]
    Z --> AA["Registra ação em logs (ação: update_data)"]
    AA --> AB["Frontend exibe: 'Dados salvos'"]
    AB --> K
    W -->|Não| AC{"Alterar tema?"}
    AC -->|Sim| AD["Seleciona tema: claro/escuro"]
    AD --> AE["Frontend envia POST /api/profile/theme"]
    AE --> AF["Backend salva em users.theme"]
    AF --> AG["Registra ação em logs (ação: update_theme)"]
    AG --> K
    AC -->|Não| K
    K --> AH["Fim"]
```

## Descrição do Processo

1. Usuário acessa a tela de perfil no frontend.
2. Escolhe uma ação:
   - **Alterar Avatar**:
     - Faz upload (PNG/JPG, ≤2MB) ou escolhe predefinido.
     - Frontend envia `POST /api/profile/avatar`.
     - Backend valida e salva em `users.avatar`.
     - Registra em `logs` e exibe confirmação.
   - **Alterar Senha**:
     - Insere senha atual e nova (mínimo 8 caracteres, letras e números).
     - Frontend envia `POST /api/profile/password`.
     - Backend valida (senha atual correta, nova forte) e atualiza `users.password`.
     - Registra em `logs` e exibe confirmação.
   - **Editar Dados Pessoais**:
     - Edita endereço e documentos (criptografados).
     - Frontend envia `POST /api/profile/data`.
     - Backend valida e salva em `users`.
     - Registra em `logs` and exibe confirmação.
   - **Alterar Tema**:
     - Seleciona tema (claro/escuro).
     - Frontend envia `POST /api/profile/theme`.
     - Backend salva em `users.theme`.
     - Registra em `logs` e aplica tema.
3. Retorna à tela de perfil.

## Regras de Negócio

- Avatares: PNG/JPG, ≤2MB.
- Senhas: `^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$`.
- Documentos: Criptografados com AES-256.
- Temas: Aplicados imediatamente, salvos em `users.theme`.

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