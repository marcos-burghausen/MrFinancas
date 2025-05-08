🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Configurar Perfil

Este diagrama descreve as interações para configurar o perfil do usuário.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)
    participant S as AWS S3

    U->>F: Acessa tela de perfil
    F->>B: GET /profile
    B->>DB: Consulta dados do usuário
    DB-->>B: Dados do usuário
    B-->>F: Resposta: { id, nome, email, avatar, idioma, moeda }
    F-->>U: Exibe formulário com dados
    U->>F: Escolhe ação
    alt Editar Perfil
        U->>F: Edita (nome, email, idioma, moeda)
        F->>B: PUT /profile
        B->>DB: Atualiza dados
        DB-->>B: Dados atualizados
        B-->>F: Resposta: Perfil atualizado
        F-->>U: Exibe "Perfil atualizado"
    else Alterar Senha
        U->>F: Preenche (senhaAtual, novaSenha)
        F->>B: PUT /profile/password
        B->>DB: Valida senha atual
        DB-->>B: Senha válida
        B->>DB: Atualiza senha
        DB-->>B: Senha atualizada
        B-->>F: Resposta: "Senha atualizada"
        F-->>U: Exibe "Senha atualizada"
    else Atualizar Avatar
        U->>F: Seleciona imagem
        F->>B: PUT /profile/avatar (multipart/form-data)
        B->>S: Salva imagem no S3
        S-->>B: URL da imagem
        B->>DB: Atualiza avatar
        DB-->>B: Avatar atualizado
        B-->>F: Resposta: { avatar }
        F-->>U: Exibe "Avatar atualizado"
    end
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de perfil.
2. O frontend envia `GET /profile` para obter dados.
3. O backend retorna dados do usuário.
4. O frontend exibe formulário com valores atuais.
5. O usuário escolhe:
   - **Editar Perfil**: Edita dados, envia `PUT /profile`, exibe confirmação.
   - **Alterar Senha**: Preenche senhas, envia `PUT /profile/password`, exibe confirmação.
   - **Atualizar Avatar**: Seleciona imagem, envia `PUT /profile/avatar`, exibe confirmação.
6. O frontend atualiza a interface com os novos dados.

### Fluxos Alternativos

- **A1**: Dados inválidos (editar) → Backend retorna erro 422 ("Dados inválidos").
- **A2**: Senha atual incorreta → Backend retorna erro 401 ("Senha incorreta").
- **A3**: Erro no S3 (avatar) → Backend retorna erro 500 e notifica via SNS.

### Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios.
- **Backend**: Laravel com Eloquent (`User`).
  - Endpoints: `GET /profile`, `PUT /profile`, `PUT /profile/password`, `PUT /profile/avatar` (controlador `ProfileController`).
- **Banco**: Tabela `users` (`id`, `name`, `email`, `avatar`, `language`, `currency`).
- **Integração**: AWS S3 para avatars.
- **Segurança**: JWT para autenticação; bcrypt para senhas.