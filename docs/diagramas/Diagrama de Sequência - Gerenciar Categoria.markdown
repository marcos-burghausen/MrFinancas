🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Gerenciar Categoria

Este diagrama descreve as interações para criar, editar e excluir categorias.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)

    U->>F: Acessa tela de categorias
    F->>B: GET /categorias?tipo
    B->>DB: Consulta categorias do usuário
    DB-->>B: Lista de categorias
    B-->>F: Resposta: [{ id, nome, tipo, cor, icone }]
    F-->>U: Exibe lista
    U->>F: Escolhe ação
    alt Criar
        F->>U: Exibe formulário
        U->>F: Preenche (nome, tipo, cor, icone)
        F->>B: POST /categorias
        B->>DB: Cria categoria
        DB-->>B: Categoria criada
        B-->>F: Resposta: Categoria criada
        F-->>U: Exibe "Categoria criada"
    else Editar
        F->>U: Exibe formulário com dados
        U->>F: Edita dados
        F->>B: PUT /categorias/{id}
        B->>DB: Atualiza categoria
        DB-->>B: Categoria atualizada
        B-->>F: Resposta: Categoria atualizada
        F-->>U: Exibe "Categoria atualizada"
    else Excluir
        U->>F: Confirma exclusão
        F->>B: DELETE /categorias/{id}
        B->>DB: Verifica transações vinculadas
        DB-->>B: Sem transações
        B->>DB: Remove categoria
        DB-->>B: Categoria removida
        B-->>F: Resposta: 204
        F-->>U: Exibe "Categoria excluída"
    end
    F->>B: GET /categorias (recarrega lista)
    B-->>F: Nova lista
    F-->>U: Exibe lista atualizada
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de categorias.
2. O frontend envia `GET /categorias` com filtro opcional `tipo`.
3. O backend retorna lista de categorias.
4. O frontend exibe a lista.
5. O usuário escolhe:
   - **Criar**: Preenche formulário, envia `POST /categorias`, exibe confirmação.
   - **Editar**: Edita formulário, envia `PUT /categorias/{id}`, exibe confirmação.
   - **Excluir**: Confirma exclusão, envia `DELETE /categorias/{id}`, exibe confirmação.
6. O frontend recarrega a lista com `GET /categorias`.

### Fluxos Alternativos

- **A1**: Dados inválidos (criar/editar) → Backend retorna erro 422 ("Dados inválidos").
- **A2**: Categoria com transações (excluir) → Backend retorna erro 409 ("Categoria vinculada").
- **A3**: Erro no banco → Backend retorna erro 500.

### Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios.
- **Backend**: Laravel com Eloquent (`Category`).
  - Endpoints: `GET /categorias`, `POST /categorias`, `PUT /categorias/{id}`, `DELETE /categorias/{id}` (controlador `CategoryController`).
- **Banco**: Tabela `categories` (`id`, `user_id`, `name`, `type`, `color`, `icon`).
- **Segurança**: JWT para autenticação.