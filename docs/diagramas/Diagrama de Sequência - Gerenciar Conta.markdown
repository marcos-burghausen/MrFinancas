🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Gerenciar Conta

Este diagrama descreve as interações para criar, editar e excluir contas bancárias.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)

    U->>F: Acessa tela de contas
    F->>B: GET /contas
    B->>DB: Consulta contas do usuário
    DB-->>B: Lista de contas
    B-->>F: Resposta: [{ id, nome, saldoInicial, saldoAtual, tipo, cor, instituicao, ativa }]
    F-->>U: Exibe lista
    U->>F: Escolhe ação
    alt Criar
        F->>U: Exibe formulário
        U->>F: Preenche (nome, saldoInicial, tipo, cor, instituicao)
        F->>B: POST /contas
        B->>DB: Cria conta
        DB-->>B: Conta criada
        B-->>F: Resposta: Conta criada
        F-->>U: Exibe "Conta criada"
    else Editar
        F->>U: Exibe formulário com dados
        U->>F: Edita dados
        F->>B: PUT /contas/{id}
        B->>DB: Atualiza conta
        DB-->>B: Conta atualizada
        B-->>F: Resposta: Conta atualizada
        F-->>U: Exibe "Conta atualizada"
    else Excluir
        U->>F: Confirma exclusão
        F->>B: DELETE /contas/{id}
        B->>DB: Verifica transações vinculadas
        DB-->>B: Sem transações
        B->>DB: Remove conta
        DB-->>B: Conta removida
        B-->>F: Resposta: 204
        F-->>U: Exibe "Conta excluída"
    end
    F->>B: GET /contas (recarrega lista)
    B-->>F: Nova lista
    F-->>U: Exibe lista atualizada
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de contas.
2. O frontend envia `GET /contas` para listar contas.
3. O backend retorna lista de contas.
4. O frontend exibe a lista.
5. O usuário escolhe:
   - **Criar**: Preenche formulário, envia `POST /contas`, exibe confirmação.
   - **Editar**: Edita formulário, envia `PUT /contas/{id}`, exibe confirmação.
   - **Excluir**: Confirma exclusão, envia `DELETE /contas/{id}`, exibe confirmação.
6. O frontend recarrega a lista com `GET /contas`.

### Fluxos Alternativos

- **A1**: Dados inválidos (criar/editar) → Backend retorna erro 422 ("Dados inválidos").
- **A2**: Conta com transações (excluir) → Backend retorna erro 409 ("Conta vinculada").
- **A3**: Erro no banco → Backend retorna erro 500.

### Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios.
- **Backend**: Laravel com Eloquent (`Account`).
  - Endpoints: `GET /contas`, `POST /contas`, `PUT /contas/{id}`, `DELETE /contas/{id}` (controlador `AccountController`).
- **Banco**: Tabela `accounts` (`id`, `user_id`, `name`, `initial_balance`, `balance`, `type`, `color`, `institution`, `active`).
- **Segurança**: JWT para autenticação.