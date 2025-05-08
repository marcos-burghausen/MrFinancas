🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Entidade-Relacionamento: MrFinanças

Este diagrama modela as tabelas do banco de dados MySQL, seus atributos e relacionamentos, alinhados com os modelos Eloquent do Laravel e os casos de uso.

## Diagrama ER

```mermaid
erDiagram
    users ||--o{ accounts : owns
    users ||--o{ transactions : creates
    users ||--o{ cards : owns
    users ||--o{ categories : owns
    users ||--o{ notifications : configures
    users ||--o{ settings : configures
    users ||--o{ logs : generates
    accounts ||--o{ transactions : linked
    accounts ||--o{ cards accadf9fb-42e8-4e14-8f3b-2f5f8a1e9e9a
    cards ||--o{ transactions : linked
    cards ||--o{ invoices : generates
    categories ||--o{ transactions : linked
    categories ||--o{ categories : parent

    users {
        int id PK
        string name
        string email UK
        string password
        boolean email_verified
        string avatar
        string theme
        timestamp created_at
        timestamp updated_at
        timestamp deleted_at
    }
    accounts {
        int id PK
        int user_id FK
        string name
        string category
        string icon
        string color
        decimal balance
        timestamp created_at
        timestamp updated_at
        timestamp deleted_at
    }
    transactions {
        int id PK
        int user_id FK
        int account_id FK
        int card_id FK, NULL
        int category_id FK
        string type
        decimal amount
        date date
        string description
        string status
        boolean recurring
        string frequency
        date end_date
        timestamp created_at
        timestamp updated_at
        timestamp deleted_at
    }
    cards {
        int id PK
        int user_id FK
        int account_id FK
        string name
        decimal limit
        date closing_date
        date due_date
        string icon
        string color
        timestamp created_at
        timestamp updated_at
        timestamp deleted_at
    }
    categories {
        int id PK
        int user_id FK
        int parent_id FK, NULL
        string name
        string icon
        string color
        timestamp created_at
        timestamp updated_at
        timestamp deleted_at
    }
    notifications {
        int id PK
        int user_id FK
        string type
        boolean enabled
        int days_before
        string time
        timestamp created_at
        timestamp updated_at
        timestamp deleted_at
    }
    invoices {
        int id PK
        int card_id FK
        date due_date
        decimal total_amount
        timestamp created_at
        timestamp updated_at
        timestamp deleted_at
    }
    settings {
        int id PK
        int user_id FK
        boolean mfa_enabled
        string mfa_method
        string notification_preferences
        timestamp created_at
        timestamp updated_at
        timestamp deleted_at
    }
    logs {
        int id PK
        int user_id FK
        string action
        string details
        timestamp created_at
    }
```

## Descrição

### Tabelas e Relacionamentos

- **users**: Dados de usuários (autenticação, perfil).
  - Relacionamentos: 1:N com `accounts`, `transactions`, `cards`, `categories`, `notifications`, `settings`, `logs`.
- **accounts**: Contas bancárias.
  - Relacionamentos: 1:N com `transactions`, `cards`.
- **transactions**: Lançamentos financeiros.
  - Relacionamentos: N:1 com `users`, `accounts`, `cards` (opcional), `categories`.
- **cards**: Cartões de crédito.
  - Relacionamentos: N:1 com `users`, `accounts`; 1:N com `transactions`, `invoices`.
- **categories**: Categorias/subcategorias.
  - Relacionamentos: N:1 com `users`; 1:N com `transactions`; auto-relacionamento.
- **notifications**: Configurações de notificações.
  - Relacionamentos: N:1 com `users`.
- **invoices**: Faturas de cartões.
  - Relacionamentos: N:1 com `cards`.
- **settings**: Preferências do usuário (MFA, notificações).
  - Relacionamentos: N:1 com `users`.
- **logs**: Registros de auditoria (ex.: logins, transações).
  - Relacionamentos: N:1 com `users`.

### Notas Técnicas

- **Chaves**:
  - Primárias (PK): `id`.
  - Únicas (UK): `email` em `users`.
  - Estrangeiras (FK): Referenciam `id`.
- **Tipos**:
  - `decimal(15,2)` para valores monetários.
  - `string` para textos (ex.: `varchar(255)`).
  - `date` para datas.
- **Soft Deletes**: `deleted_at` para exclusão lógica (exceto `logs`).
- **Índices**:
  - Índices em `user_id`, `account_id`, `card_id`, `category_id`.
  - Índice único em `users.email`.
- **Criptografia**: `users.password` usa bcrypt; `settings` dados sensíveis (ex.: MFA secrets) são criptografados.
- **Laravel**: Modelos Eloquent com relacionamentos (`hasMany`, `belongsTo`).
