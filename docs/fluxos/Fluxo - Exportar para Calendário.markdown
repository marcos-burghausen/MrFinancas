🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Exportar para Calendário

Este documento descreve o processo de exportação de vencimentos de transações para o Google Calendar.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de transações (Vue.js)"]
    B --> C["Seleciona transação com vencimento"]
    C --> D["Clica em 'Exportar para calendário'"]
    D --> E["Frontend verifica token OAuth em localStorage"]
    E --> F{"Token válido?"}
    F -->|Sim| G["Frontend envia POST /api/calendar/export"]
    F -->|Não| H["Frontend autentica com Google Calendar (OAuth 2.0)"]
    H --> I{"Autenticação bem-sucedida?"}
    I -->|Sim| G
    I -->|Não| J["Frontend exibe erro 401: 'Falha na autenticação'"]
    J --> K["Retorna à lista de transações"]
    G --> L["Backend valida transação em transactions"]
    L --> M{"Transação válida?"}
    M -->|Sim| N["Backend cria evento via Google Calendar API"]
    N --> O["Salva evento_id em calendar_events"]
    O --> P["Registra ação em logs (ação: export_calendar)"]
    P --> Q["Frontend exibe: 'Evento exportado com sucesso'"]
    Q --> K
    M -->|Não| R["Backend retorna erro 404: 'Transação inválida'"]
    R --> S["Frontend exibe erro"]
    S --> K
    K --> T["Fim"]
```

## Descrição do Processo

1. Usuário acessa a tela de transações no frontend (Vue.js com Vuetify).
2. Seleciona uma transação com data de vencimento (`transactions.date`).
3. Clica em “Exportar para calendário”.
4. Frontend verifica token OAuth em `localStorage`:
   - Se válido, prossegue.
   - Se inválido, autentica com Google Calendar (OAuth 2.0, scope: `https://www.googleapis.com/auth/calendar.events`).
5. Se autenticação falhar, exibe erro 401 e retorna à lista.
6. Frontend envia `POST /api/calendar/export` com `{ transaction_id }`.
7. Backend valida `transaction_id` em `transactions`:
   - Se válido, cria evento no Google Calendar:
     - `summary`: “{type}: {description}” (ex.: “Despesa: Aluguel”).
     - `start`/`end`: `transactions.date` (formato ISO 8601).
     - `description`: “Valor: R${amount}, Conta: {account.name}”.
   - Salva `event_id` em `calendar_events` para rastreamento.
   - Registra ação em `logs` (ação: `export_calendar`).
   - Frontend exibe confirmação.
8. Se transação inválida, backend retorna erro 404, e frontend exibe erro.
9. Retorna à lista de transações.

## Regras de Negócio

- Apenas transações com `date` não nulo são exportáveis.
- Títulos do evento: “Despesa: {description}”, “Receita: {description}”, “Cartão: {description}”.
- Eventos criados no calendário padrão do usuário.
- Token OAuth armazenado em `localStorage` para reutilização.
- Eventos vinculados a transações via `calendar_events`.

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios, Google API Client (`gapi`).
- **Backend**: Laravel, Eloquent (modelos `Transaction`, `CalendarEvent`), Google Calendar API (PHP Client Library).
- **Endpoint**: `POST /api/calendar/export`: `{ transaction_id: int }` → `{ event_id: string }`.
- **Banco**: 
  - Tabelas: `transactions` (`id`, `type`, `description`, `amount`, `date`, `account_id`), `calendar_events` (`transaction_id`, `event_id`, `user_id`), `logs` (`action`, `details`, `user_id`).
  - Relacionamentos: `calendar_events` → `transactions` (1:1).
- **Integrações**: 
  - Google Calendar API (scope: `https://www.googleapis.com/auth/calendar.events`).
  - Componente: Calendar Service (Diagrama de Componentes).
- **Segurança**: 
  - JWT para autenticação da API.
  - OAuth 2.0 para Google Calendar.
  - HTTPS para todas as comunicações.
  - Validação de `user_id` para garantir propriedade dos dados.

## Integrações

- Integra com `transactions` para recuperar dados de vencimento.
- Eventos exportados aparecem em relatórios de vencimentos.
- Usa AWS SNS para notificar falhas críticas (ex.: erro na API do Google).
- Componente de log (Log Service) registra todas as ações.

## Alinhamento com Caso de Uso

Este fluxo corresponde ao `Caso de Uso - Exportar para Calendário`, detalhando a autenticação OAuth, criação de eventos e tratamento de erros, com maior granularidade técnica (ex.: endpoints, tabelas).