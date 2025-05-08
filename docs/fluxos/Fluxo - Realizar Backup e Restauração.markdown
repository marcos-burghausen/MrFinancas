🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Realizar Backup e Restauração

Este documento descreve o processo de backup e restauração de dados do usuário (contas, transações, categorias).

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de backup (Vue.js)"]
    B --> C["Escolhe ação"]
    C --> D{"Fazer backup?"}
    D -->|Sim| E["Seleciona dados: accounts, transactions, categories"]
    E --> F["Frontend envia POST /api/backup"]
    F --> G["Backend gera JSON criptografado (AES-256)"]
    G --> H["Salva em AWS S3 ou inicia download"]
    H --> I["Registra ação em logs (ação: backup)"]
    I --> J["Frontend exibe: 'Backup concluído'"]
    J --> K["Retorna à tela"]
    K --> L["Fim"]
    D -->|Não| M{"Restaurar dados?"}
    M -->|Sim| N["Faz upload de arquivo JSON"]
    N --> O["Frontend envia POST /api/restore"]
    O --> P["Backend valida e descriptografa"]
    P --> Q{"Arquivo válido?"}
    Q -->|Sim| R["Backend importa dados"]
    R --> S["Atualiza saldos em accounts"]
    S --> T["Registra ação em logs (ação: restore)"]
    T --> U["Frontend exibe: 'Restauração concluída'"]
    U --> K
    Q -->|Não| V["Backend retorna erro 422: 'Arquivo inválido'"]
    V --> W["Frontend retorna ao upload"]
    W --> N
    M -->|Não| K
```

## Descrição do Processo

1. Usuário acessa a tela de backup no frontend.
2. Escolhe ação:
   - **Backup**:
     - Seleciona dados (`accounts`, `transactions`, `categories`).
     - Frontend envia `POST /api/backup`.
     - Backend gera JSON: `{ "accounts": [], "transactions": [], "categories": [] }`, criptografa (AES-256).
     - Salva em AWS S3 ou inicia download.
     - Registra em `logs`, exibe confirmação.
   - **Restauração**:
     - Faz upload de JSON.
     - Frontend envia `POST /api/restore`.
     - Backend descriptografa, valida formato e `user_id`, importa dados.
     - Atualiza `accounts.balance`, registra em `logs`, exibe confirmação.
     - Se inválido, retorna erro 422.
3. Retorna à tela.

## Regras de Negócio

- Backup inclui apenas dados do usuário (`user_id`).
- Restauração sobrescreve dados após confirmação.
- Arquivos criptografados com AES-256.
- JSON segue esquema predefinido.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify.
- **Backend**: Laravel, Eloquent (modelos `Account`, `Transaction`, `Category`).
- **Endpoints**:
  - `POST /api/backup`: `{ types: array }` → `{ file_url: string }`.
  - `POST /api/restore`: Multipart com JSON.
- **Banco**: Tabelas `accounts`, `transactions`, `categories`, `logs`.
- **Integrações**: AWS S3 (Backup Service).
- **Segurança**: JWT, AES-256, HTTPS.