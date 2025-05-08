🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Gerenciar Cartão

Este diagrama descreve as interações para criar, editar e excluir cartões de crédito.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)

    U->>F: Acessa tela de cartões
    F->>B: GET /cards
    B->>DB: Consulta cartões do usuário
    DB-->>B: Lista de cartões
    B-->>F: Resposta: [{ id, nome, limite, diaVencimento, diaFechamento, saldoAtual, instituicao, ativa }]
    F-->>U: Exibe lista
    U->>F: Escolhe ação
    alt Criar
        F->>U: Exibe formulário
        U->>F: Preenche (nome, limite, diaVencimento, diaFechamento, instituicao)
        F->>B: POST /cards
        B->>DB: Cria cartão
        DB-->>B: Cartão criado
        B-->>F: Resposta: Cartão criado
        F-->>U: Exibe "Cartão criado"
    else Editar
        F->>U: Exibe formulário com dados
        U->>F: Edita dados
        F->>B: PUT /cards/{id}
        B->>DB: Atualiza cartão
        DB-->>B: Cartão atualizado
        B-->>F: Resposta: Cartão atualizado
        F-->>U: Exibe "Cartão atualizado"
    else Excluir
        U->>F: Confirma exclusão
        F->>B: DELETE /cards/{id}
        B->>DB: Verifica transações vinculadas
        DB-->>B: Sem transações
        B->>DB: Remove cartão
        DB-->>B: Cartão removido
        B-->>F: Resposta: 204
        F-->>U: Exibe "Cartão excluído"
    end
    F->>B: GET /cards (recarrega lista)
    B-->>F: Nova lista
    F-->>U: Exibe lista atualizada
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de cartões.
2. O frontend envia `GET /cards` para listar cartões.
3. O backend retorna lista de cartões.
4. O frontend exibe a lista.
5. O usuário escolhe:
   - **Criar**: Preenche formulário, envia `POST /cards`, exibe confirmação.
   - **Editar**: Edita formulário, envia `PUT /cards/{id}`, exibe confirmação.
   - **Excluir**: Confirma exclusão, envia `DELETE /cards/{id}`, exibe confirmação.
6. O frontend recarrega a lista com `GET /cards`.

### Fluxos Alternativos

- **A1**: Dados inválidos (criar/editar) → Backend retorna erro 422 ("Dados inválidos").
- **A2**: Cartão com transações (excluir) → Backend retorna erro 409 ("Cartão vinculado").
- **A3**: Erro no banco → Backend retorna erro 500.

### Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios.
- **Backend**: Laravel com Eloquent (`Card`).
  - Endpoints: `GET /cards`, `POST /cards`, `PUT /cards/{id}`, `DELETE /cards/{id}` (controlador `CardController`).
- **Banco**: Tabela `cards` (`id`, `user_id`, `name`, `limit`, `due_day`, `closing_day`, `current_balance`, `institution`, `active`).
- **Segurança**: JWT para autenticação.