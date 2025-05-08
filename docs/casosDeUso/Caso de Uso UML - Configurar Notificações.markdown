🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso: Configurar Notificações

## Diagrama de Caso de Uso

```mermaid
usecaseDiagram
    actor Usuário
    actor SistemaNotificações
    Usuário --> (Configurar Notificações)
    Usuário --> SistemaNotificações : configura
```

## Descrição

Permite que o usuário configure notificações para vencimentos e metas financeiras.

## Atores

- **Primário**: Usuário
- **Secundário**: Sistema de Notificações (AWS SNS, Firebase)

## Pré-condições

- Usuário está autenticado (JWT válido).
- Existem transações com vencimentos em `transactions`.

## Pós-condições

- Configurações são salvas em `notifications` e `settings`.
- Notificações são agendadas via Laravel Queue.
- Log de configuração é registrado em `logs`.

## Fluxo Principal

1. Usuário acessa a tela de notificações no frontend.
2. Seleciona tipo: vencimentos, metas.
3. Configura: `{ enabled: boolean, days_before: int, time: HH:MM, channel: email|push }`.
4. Frontend envia `POST /api/notifications`.
5. Backend valida: `days_before <= 3`, `time` válido.
6. Salva em `notifications` e atualiza `settings.notification_preferences`.
7. Agenda notificações via Laravel Queue (Horizon).
8. Backend registra ação em `logs` (ação: `configure_notification`).
9. Frontend exibe confirmação.

## Fluxos Alternativos

- **A1**: Configuração inválida → Backend retorna erro 422 ("Configuração inválida, verifique os campos").

## Regras de Negócio

- Antecedência máxima: 3 dias.
- Horários ajustados ao fuso horário do usuário.
- Notificações push exigem permissão (armazenada em `settings`).

## Notas Técnicas

- **Frontend**: Vue.js, Vuetify.
- **Backend**: Laravel, Eloquent (modelo `Notification`).
- **Endpoints**:
  - `POST /api/notifications`: `{ type: string, enabled: boolean, days_before: int, time: string, channel: string }`.
  - `PUT /api/notifications/{id}`: Atualiza configuração.
- **Banco**: Tabela `notifications` (`type`, `enabled`, `days_before`, `time`); `settings` (`notification_preferences`); `logs` (`action`).
- **Integrações**: AWS SNS/Firebase (Notification Service), AWS SES (Email Service).
- **Segurança**: JWT, validação de permissões push.
