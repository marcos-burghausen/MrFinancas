🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Configurar Notificações

Este diagrama descreve as interações para configurar notificações.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)

    U->>F: Acessa tela de notificações
    F->>B: GET /notifications
    B->>DB: Consulta configurações
    DB-->>B: Configurações atuais
    B-->>F: Resposta: { notificacoes: { email, push } }
    F-->>U: Exibe formulário com configurações
    U->>F: Edita configurações (email.saldoBaixo, push.vencimentoFatura, etc.)
    F->>B: POST /notifications
    B->>DB: Atualiza configurações
    DB-->>B: Configurações atualizadas
    B-->>F: Resposta: Configurações atualizadas
    F-->>U: Exibe "Configurações salvas"
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de notificações.
2. O frontend envia `GET /notifications` para obter configurações atuais.
3. O backend retorna configurações (`email`, `push`).
4. O frontend exibe formulário com valores atuais.
5. O usuário edita configurações (e.g., ativa `email.saldoBaixo`).
6. O frontend envia `POST /notifications`.
7. O backend atualiza as configurações no banco.
8. Retorna confirmação, e o frontend exibe "Configurações salvas".

### Fluxos Alternativos

- **A1**: Dados inválidos → Backend retorna erro 422 ("Dados inválidos").
- **A2**: Erro no banco → Backend retorna erro 500.

### Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios.
- **Backend**: Laravel com Eloquent (`NotificationSetting`).
  - Endpoints: `GET /notifications`, `POST /notifications` (controlador `NotificationController`).
- **Banco**: Tabela `notification_settings` (`user_id`, `email_settings`, `push_settings`).
- **Segurança**: JWT para autenticação.
- **Integração**: Usa AWS SNS para notificações push (assíncrono).