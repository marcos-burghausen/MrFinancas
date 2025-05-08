🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Exportar para Calendário

Este diagrama descreve as interações para exportar uma transação para o Google Calendar.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)
    participant G as Google Calendar

    U->>F: Acessa tela de transações
    F->>B: GET /transacoes
    B-->>F: Lista de transações
    F-->>U: Exibe lista
    U->>F: Seleciona transação e clica "Exportar"
    F->>F: Verifica token OAuth (localStorage)
    alt Token válido
        F->>B: POST /calendar/export (id)
    else Token inválido
        F->>G: Autentica via OAuth 2.0
        G-->>F: Token retornado
        F->>F: Armazena token
        F->>B: POST /calendar/export (id)
    end
    B->>DB: Valida transação
    DB-->>B: Transação válida
    B->>G: Cria evento (summary, start, end)
    G-->>B: Evento criado
    B->>DB: Salva evento_id
    DB-->>B: Evento salvo
    B-->>F: Resposta: { event_id, transacao_id }
    F-->>U: Exibe "Evento exportado"
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de transações.
2. O frontend lista transações via `GET /transacoes`.
3. O usuário seleciona uma transação e clica "Exportar para calendário".
4. O frontend verifica o token OAuth em `localStorage`:
   - Se válido, envia `POST /calendar/export`.
   - Se inválido, autentica com Google Calendar (OAuth 2.0), armazena token, e envia `POST /calendar/export`.
5. O backend valida a transação no banco.
6. Cria um evento no Google Calendar.
7. Salva o `event_id` na tabela `calendar_events`.
8. Retorna confirmação, e o frontend exibe "Evento exportado".

### Fluxos Alternativos

- **A1**: Transação inválida → Backend retorna erro 404 ("Transação não encontrada").
- **A2**: Falha na autenticação OAuth → Frontend exibe erro 401.
- **A3**: Erro no Google Calendar → Backend retorna erro 500 e notifica via AWS SNS.

### Notas Técnicas

- **Frontend**: Vue.js, Google API Client (`gapi`), Axios.
- **Backend**: Laravel com Google Calendar API (PHP Client).
  - Endpoint: `POST /calendar/export` (controlador `CalendarController`).
- **Banco**: Tabelas `transactions`, `calendar_events` (`transaction_id`, `event_id`).
- **Segurança**: JWT para API, OAuth 2.0 para Google Calendar.
- **Integração**: Google Calendar (scope: `https://www.googleapis.com/auth/calendar.events`).