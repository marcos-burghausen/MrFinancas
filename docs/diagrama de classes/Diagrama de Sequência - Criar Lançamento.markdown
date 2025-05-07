🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Criar Lançamento

Este diagrama descreve as interações entre o usuário, o frontend (Vue.js), o backend (Laravel) e o banco (MySQL) para criar um lançamento financeiro.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)

    U->>F: Acessa tela de lançamentos
    F->>U: Exibe formulário
    U->>F: Preenche formulário (tipo, valor, data, conta, categoria)
    F->>B: POST /api/transactions (tipo, valor, data, conta_id, categoria_id)
    B->>DB: Valida conta e categoria
    DB-->>B: Conta e categoria válidas
    B->>DB: Cria transação
    DB-->>B: Transação criada
    B->>DB: Atualiza saldo da conta
    DB-->>B: Saldo atualizado
    B-->>F: Resposta: "Lançamento salvo"
    F-->>U: Exibe mensagem
    U->>F: Retorna à lista
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de lançamentos no frontend.
2. Preenche o formulário (tipo: despesa, valor, data, conta, categoria) e envia.
3. O frontend faz uma requisição POST ao backend (endpoint `/api/transactions`).
4. O backend valida:
   - Conta e categoria existem e pertencem ao usuário.
   - Valor é positivo.
5. Cria a transação no banco (tabela `transactions`).
6. Atualiza o saldo da conta (tabela `accounts`).
7. Retorna mensagem ao frontend, que exibe "Lançamento salvo".
8. O usuário retorna à lista de lançamentos.

### Fluxos Alternativos

- **A1**: Dados inválidos (ex.: conta inexistente) → Backend retorna erro 422 ("Dados inválidos").
- **A2**: Saldo insuficiente → Backend emite alerta, mas permite salvar (configurável).
- **A3**: Erro no banco → Backend retorna erro 500 e registra falha.

### Notas Técnicas

- **Frontend**: Usa Vue.js com formulário reativo (ex.: Vuetify) e Axios.
- **Backend**: Usa Laravel com Eloquent para modelos `Transaction` e `Account`.
  - Endpoint: `POST /api/transactions` (controlador `TransactionController`).
  - Validação: Usa `Request` com regras (ex.: `amount: required|numeric|min:0`).
- **Banco**:
  - Tabela `transactions`: `user_id`, `account_id`, `category_id`, `type`, `amount`, `date`.
  - Tabela `accounts`: `balance` atualizado com transação.
- **Transações**: Usa transações SQL para garantir consistência (salvar transação e atualizar saldo).
- **Segurança**: Requisição exige autenticação (Laravel Sanctum).
