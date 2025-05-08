🔙 [Retornar à documentação principal](../../README.md)

# Fluxo: Configurar Notificações

Este documento descreve o processo de configuração de notificações para vencimentos de transações e metas financeiras.

## Diagrama de Fluxo

```mermaid
flowchart TD
    A["Início"] --> B["Usuário acessa tela de notificações (Vue.js)"]
    B --> C["Seleciona tipo: vencimentos, metas"]
    C --> D["Configura: { enabled: boolean, days_before: int, time: HH:MM, channel: email|push }"]
    D --> E["Frontend envia POST /api/notifications"]
    E --> F["Backend valida: days_before <= 3, time válido"]
    F --> G{"Configuração válida?"}
    G -->|Sim| H["Salva em notifications e settings.notification_preferences"]
    H --> I["Agenda notificações via Laravel Queue (Horizon)"]
    I --> J["Registra ação em logs (ação: configure_notification)"]
    J --> K["Frontend exibe: 'Configuração salva'"]
    K --> L["Retorna à tela de configurações"]
    L --> M["Fim"]
    G -->|Não| N["Backend retorna erro 422: 'Dados inválidos'"]
    N --> O["Frontend exibe erro e retorna ao formulário"]
    O --> D
```

## Descrição do Processo

1. Usuário acessa a tela de notificações no frontend (Vue.js com Vuetify).
2. Seleciona o tipo de notificação (vencimentos ou metas).
3. Configura opções:
   - Ativar/desativar (`enabled`).
   - Antecedência (0-3 dias, `days_before`).
   - Horário de envio (`time`, formato HH:MM).
   - Canal (email ou push, `channel`).
4. Frontend envia requisição `POST /api/notifications`.
5. Backend valida:
   - `days_before` ≤ 3.
   - `time` no formato HH:MM (00:00-23:59).
   - `channel` válido.
6. Se válido:
   - Salva configuração em `notifications` e atualiza `settings.notification_preferences`.
   - Agenda notificações via Laravel Queue (Horizon).
   - Registra ação em `logs` (ação: `configure_notification`).
   - Frontend exibe confirmação.
7. Se inválido:
   - Backend retorna erro 422.
   - Frontend exibe erro e retorna ao formulário.

## Regras de Negócio

- Notificações de vencimentos baseadas em `transactions.date` ou `invoices.due_date`.
- Antecedência máxima: 3 dias.
- Horários ajustados ao fuso horário do usuário (armazenado em `settings`).
- Notificações push exigem permissão (armazenada em `settings.notification_preferences`).
- Configurações podem ser desativadas por tipo.

## Notas Técnicas

- **Frontend**: Vue.js com Vuetify (formulários reativos), Axios para requisições.
- **Backend**: Laravel com Eloquent (modelos `Notification`, `Setting`), Horizon para filas.
- **Endpoint**: `POST /api/notifications`: `{ type: string, enabled: boolean, days_before: int, time: string, channel: string }`.
- **Banco**: Tabelas `notifications` (`type`, `enabled`, `days_before`, `time`, `user_id`), `settings` (`notification_preferences`), `logs` (`action`, `details`).
- **Integrações**:
  - AWS SNS/Firebase (Notification Service) para push.
  - AWS SES (Email Service) para e-mails.
  - Integra com `transactions` e `invoices` para vencimentos.
- **Segurança**: JWT para autenticação, HTTPS, validação de permissões push.