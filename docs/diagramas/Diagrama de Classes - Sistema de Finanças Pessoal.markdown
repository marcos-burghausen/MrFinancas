🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Classes: Sistema de Finanças Pessoal

Este diagrama modela as entidades principais do sistema, seus atributos, métodos e relacionamentos, alinhados com o backend Laravel (modelos Eloquent) e o banco MySQL.

## Diagrama de Classes

```mermaid
classDiagram
    class User {
        +int id
        +string name
        +string email
        +string password
        +boolean email_verified
        +string avatar
        +string theme
        +timestamp created_at
        +timestamp updated_at
        +register(email, password, name) User
        +verifyEmail(token) boolean
        +updateProfile(data) boolean
    }
    class Account {
        +int id
        +int user_id
        +string name
        +string category
        +string icon
        +string color
        +decimal balance
        +timestamp created_at
        +timestamp updated_at
        +create(user_id, data) Account
        +update(data) boolean
        +delete() boolean
    }
    class Transaction {
        +int id
        +int user_id
        +int account_id
        +int? card_id
        +int category_id
        +string type
        +decimal amount
        +date date
        +string description
        +boolean recurring
        +string recurrence_frequency
        +timestamp created_at
        +timestamp updated_at
        +create(user_id, data) Transaction
        +update(data) boolean
        +delete() boolean
    }
    class Card {
        +int id
        +int user_id
        +int account_id
        +string name
        +decimal limit
        +string icon
        +string color
        +timestamp created_at
        +timestamp updated_at
        +create(user_id, data) Card
        +update(data) boolean
        +delete() boolean
        +getInvoice() Invoice
    }
    class Category {
        +int id
        +int user_id
        +int? parent_id
        +string name
        +string icon
        +string color
        +timestamp created_at
        +timestamp updated_at
        +create(user_id, data) Category
        +update(data) boolean
        +delete() boolean
    }
    class Notification {
        +int id
        +int user_id
        +string type
        +boolean enabled
        +int days_before
        +string time
        +timestamp created_at
        +timestamp updated_at
        +create(user_id, data) Notification
        +update(data) boolean
        +delete() boolean
    }
    class Invoice {
        +int id
        +int card_id
        +date due_date
        +decimal total_amount
        +timestamp created_at
        +timestamp updated_at
        +generate() Invoice
    }

    User ||--o{ Account : owns
    User ||--o{ Transaction : creates
    User ||--o{ Card : owns
    User ||--o{ Category : owns
    User ||--o{ Notification : configures
    Account ||--o{ Transaction : linked
    Account ||--o{ Card : linked
    Card ||--o{ Transaction : linked
    Card ||--o{ Invoice : generates
    Category ||--o{ Transaction : linked
    Category ||--o{ Category : parent
```

## Descrição

### Classes e Relacionamentos

- **User**: Representa o usuário, com autenticação (e-mail/senha, OAuth) e perfil (avatar, tema).
  - Relacionamentos: Possui múltiplas contas, transações, cartões, categorias e notificações.
- **Account**: Representa contas bancárias, com saldo e personalização (ícone, cor).
  - Relacionamentos: Vinculada a um usuário, usada em transações e cartões.
- **Transaction**: Representa lançamentos financeiros (despesas, receitas, cartões, estornos).
  - Relacionamentos: Vinculada a usuário, conta, cartão (opcional) e categoria.
- **Card**: Representa cartões de crédito, com limite e faturas.
  - Relacionamentos: Vinculada a usuário e conta, usada em transações, gera faturas.
- **Category**: Representa categorias/subcategorias, com hierarquia.
  - Relacionamentos: Vinculada a usuário, usada em transações, pode ter subcategorias.
- **Notification**: Representa configurações de notificações (vencimentos, metas).
  - Relacionamentos: Vinculada a usuário.
- **Invoice**: Representa faturas de cartões, com vencimento e total.
  - Relacionamentos: Vinculada a cartão.

### Notas Técnicas

- **Laravel**: Cada classe corresponde a um modelo Eloquent, com tabelas no MySQL.
  - Ex.: `User` mapeia para tabela `users`, com colunas `id`, `name`, `email`, etc.
  - Relacionamentos são implementados com métodos Eloquent (ex.: `hasMany`, `belongsTo`).
- **Atributos**: Refletem colunas do banco, com tipos compatíveis (ex.: `decimal` para valores monetários).
- **Métodos**: Representam ações principais (ex.: `create`, `update`, `delete`), implementadas como métodos nos modelos ou controladores.
- **Soft Deletes**: Usado em todas as classes para evitar exclusão física (coluna `deleted_at`).
- **Criptografia**: Senhas e dados sensíveis (ex.: documentos) são criptografados.
