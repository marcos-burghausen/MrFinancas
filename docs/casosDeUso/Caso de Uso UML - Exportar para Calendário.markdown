🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Exportar para Calendário

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    actor GoogleCalendar
    Usuário --> (Exportar para Calendário)
    Usuário --> GoogleCalendar : usa
```

## Descrição

Permite que o usuário exporte vencimentos de transações para o Google Calendar.

## Atores

- **Primário**: Usuário
- **Secundário**: Google Calendar

## Pré-condições

- Usuário está autenticado (JWT válido).
- Transação possui vencimento (`transactions.date`).

## Pós-condições

- Evento é criado no Google Calendar.
- Confirmação é exibida.
- Log de exportação é registrado em `logs`.

## Fluxo Principal

1. Usuário seleciona transação com vencimento na tela de transações.
2. Clica em "Exportar para calendário".
3. Frontend autentica com Google Calendar (OAuth 2.0, scope: `calendar.events`).
4. Frontend envia `POST /api/calendar/export` com `{ transaction_id }`.
5. Backend recupera transação de `transactions` e cria evento:
   - `summary`: "Despesa: {description}" ou "Receita: {description}".
   - `start`/`end`: `transactions.date`.
   - `description`: "Valor: R${amount}".
6. Backend registra ação em `logs` (ação: `export_calendar`, detalhes: `transaction_id`).
7. Frontend exibe confirmação.

## Fluxos Alternativos

- **A1**: Falha na autenticação → Frontend exibe erro 401 ("Falha ao autenticar com Google Calendar").

## Regras de Negócio

- Apenas transações com `date` são exportáveis.
- Títulos: "Despesa: Aluguel", "Receita: Salário".
- Autenticação é armazenada para reutilização.

## Notas Técnicas

- **Frontend**: Vue.js, Axios, Google Calendar API client.
- **Backend**: Laravel, Eloquent (modelo `Transaction`).
- **Endpoint**: `POST /api/calendar/export`: `{ transaction_id: int }` → `{ event_id: string }`.
- **Banco**: Tabela `transactions` (`id`, `description`, `amount`, `date`); `logs` (`action`, `details`).
- **Integrações**: Google Calendar API (scope: `https://www.googleapis.com/auth/calendar.events`).
- **Segurança**: JWT, OAuth 2.0.
