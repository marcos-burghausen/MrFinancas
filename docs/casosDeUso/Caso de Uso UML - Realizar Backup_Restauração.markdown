🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Realizar Backup e Restauração

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    actor SistemaExternoBackup
    Usuário --> (Realizar Backup)
    Usuário --> (Realizar Restauração)
    Usuário --> SistemaExternoBackup : usa
```

## Descrição

Permite que o usuário realize backup ou restauração de dados (contas, transações, categorias).

## Atores

- **Primário**: Usuário
- **Secundário**: Sistema Externo de Backup (Google Drive, AWS S3)

## Pré-condições

- Usuário está autenticado (JWT válido).
- Existem dados em `accounts`, `transactions`, `categories`.

## Pós-condições

- Backup é gerado e baixado/armazenado.
- Dados são restaurados em `accounts`, `transactions`, `categories`.
- Log de ação é registrado em `logs`.

## Fluxo Principal

1. Usuário acessa a tela de backup no frontend.
2. Escolhe ação:
   - **Backup**:
     - Seleciona dados (`accounts`, `transactions`, `categories`).
     - Frontend envia `POST /api/backup`.
     - Backend gera arquivo JSON: `{ "accounts": [], "transactions": [], "categories": [] }`.
     - Arquivo é criptografado (AES-256) e enviado para AWS S3 ou baixado.
   - **Restauração**:
     - Usuário faz upload de arquivo JSON.
     - Frontend envia `POST /api/restore` com arquivo.
     - Backend descriptografa, valida (formato, `user_id`) e importa dados.
     - Atualiza saldos em `accounts`.
3. Backend registra ação em `logs` (ação: `backup`, `restore`).
4. Frontend exibe confirmação.

## Fluxos Alternativos

- **A1**: Arquivo inválido → Backend retorna erro 422 ("Formato de arquivo inválido").
- **A2**: Erro no download → Frontend exibe erro 500 ("Falha ao baixar") e oferece retry.

## Regras de Negócio

- Backup inclui apenas dados do usuário (`user_id`).
- Restauração sobrescreve dados após confirmação.
- Arquivos são criptografados com AES-256.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify (upload/download).
- **Backend**: Laravel, Eloquent (modelos `Account`, `Transaction`, `Category`).
- **Endpoints**:
  - `POST /api/backup`: `{ types: array }` → `{ file_url: string }`.
  - `POST /api/restore`: Multipart com arquivo JSON.
- **Banco**: Tabelas `accounts`, `transactions`, `categories`, `logs`.
- **Integrações**: AWS S3/Google Drive (Backup Service).
- **Segurança**: AES-256, JWT, HTTPS.
