🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Visualizar, Editar e Excluir Transação

Este diagrama descreve as interações para visualizar, editar e excluir transações financeiras.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)

    U->>F: Acessa tela de transações
    F->>B: GET /transacoes?dataInicio&dataFim&categoria&contaId&tipo
    B->>DB: Consulta transações com filtros
    DB-->>B: Lista de transações
    B-->>F: Resposta: { total, paginas, paginaAtual, transacoes }
    F-->>U: Exibe lista
    U->>F: Seleciona transação e escolhe ação
    alt Visualizar
        F->>B: GET /transacoes/{id}
        B->>DB: Busca transação
        DB-->>B: Transação encontrada
        B-->>F: Resposta: { id, descricao, valor, data, tipo, categoria, conta }
        F-->>U: Exibe detalhes em modal
    else Editar
        F->>U: Exibe formulário com dados
        U->>F: Edita dados
        F->>B: PUT /transacoes/{id} (descricao, valor, data, tipo, categoriaId, contaId)
        B->>DB: Valida dados
        DB-->>B: Dados válidos
        B->>DB: Atualiza transação
        B->>DB: Atualiza saldo da conta
        DB-->>B: Transação e saldo atualizados
        B-->>F: Resposta: Transação atualizada
        F-->>U: Exibe "Transação atualizada"
    else Excluir
        U->>F: Confirma exclusão
        F->>B: DELETE /transacoes/{id}
        B->>DB: Verifica dependências (faturas)
        DB-->>B: Sem dependências
        B->>DB: Remove transação (soft delete)
        B->>DB: Atualiza saldo da conta
        DB-->>B: Transação removida
        B-->>F: Resposta: 204
        F-->>U: Exibe "Transação excluída"
    end
    F->>B: GET /transacoes (recarrega lista)
    B-->>F: Nova lista
    F-->>U: Exibe lista atualizada
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de transações.
2. O frontend envia `GET /transacoes` com filtros (`dataInicio`, `dataFim`, `categoria`, `contaId`, `tipo`).
3. O backend retorna lista paginada de transações.
4. O frontend exibe a lista em tabela Vuetify.
5. O usuário seleciona uma transação e escolhe:
   - **Visualizar**: Envia `GET /transacoes/{id}`, exibe detalhes em modal.
   - **Editar**: Exibe formulário, envia `PUT /transacoes/{id}`, atualiza transação e saldo, exibe confirmação.
   - **Excluir**: Confirma exclusão, envia `DELETE /transacoes/{id}`, remove transação (soft delete), atualiza saldo, exibe confirmação.
6. O frontend recarrega a lista com `GET /transacoes`.

### Fluxos Alternativos

- **A1**: Nenhuma transação encontrada → Backend retorna erro 404 ("Nenhuma transação encontrada").
- **A2**: Dados inválidos (editar) → Backend retorna erro 422 ("Dados inválidos").
- **A3**: Transação vinculada (excluir) → Backend retorna erro 409 ("Transação vinculada").
- **A4**: Erro no banco → Backend retorna erro 500 e registra falha.

### Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios.
- **Backend**: Laravel com Eloquent (`Transaction`, `Account`, `Invoice`).
  - Endpoints: `GET /transacoes`, `GET /transacoes/{id}`, `PUT /transacoes/{id}`, `DELETE /transacoes/{id}` (controlador `TransactionController`).
- **Banco**: Tabelas `transactions` (`id`, `type`, `amount`, `date`, `account_id`, `category_id`, `deleted_at`), `accounts` (`balance`), `invoices` (`card_id`).
- **Segurança**: JWT no header `Authorization: Bearer {token}`.
- **Transações**: Usa transações SQL para consistência.