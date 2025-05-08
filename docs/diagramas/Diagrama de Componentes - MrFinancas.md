🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Componentes: MrFinanças

Este diagrama descreve os componentes principais do sistema e suas interações, alinhados com a arquitetura cliente-servidor.

## Diagrama de Componentes

```mermaid
componentDiagram
    package "Frontend (Vue.js)" {
        [UI Components] --> [API Client]
        [UI Components] --> [State Management]
        [UI Components] --> [Charts]
        [UI Components] --> [Calendar]
    }
    package "Backend (Laravel)" {
        [API Controllers] --> [Services]
        [API Controllers] --> [Middleware]
        [Services] --> [Models]
        [Services] --> [Queue]
        [Services] --> [Cache]
    }
    package "Database (MySQL)" {
        [Tables]
    }
    package "External Services" {
        [Email Service]
        [Notification Service]
        [Backup Service]
        [Monitoring]
    }

    [API Client] --> [API Controllers] : HTTP (REST)
    [Models] --> [Tables] : SQL
    [Queue] --> [Email Service] : SMTP
    [Queue] --> [Notification Service] : Push
    [Services] --> [Backup Service] : Cloud Storage
    [Services] --> [Cache] : Redis
    [Services] --> [Monitoring] : Metrics
    [UI Components] --> [Email Service] : Verification Links
    [UI Components] --> [Notification Service] : Push Notifications
```

## Descrição

### Componentes

- **Frontend (Vue.js)**:
  - **UI Components**: Interfaces Vue.js com Vuetify.
  - **API Client**: Axios para HTTP.
  - **State Management**: Pinia.
  - **Charts**: Chart.js/D3.js.
  - **Calendar**: FullCalendar.
- **Backend (Laravel)**:
  - **API Controllers**: Gerenciam requisições REST.
  - **Services**: Lógica de negócios.
  - **Middleware**: Autenticação (JWT).
  - **Models**: Eloquent.
  - **Queue**: Laravel Queue.
  - **Cache**: Redis.
- **Database (MySQL)**:
  - **Tables**: Dados do sistema.
- **External Services**:
  - **Email Service**: AWS SES para e-mails.
  - **Notification Service**: Firebase para push.
  - **Backup Service**: Google Drive/AWS S3 para backups.
  - **Monitoring**: New Relic.

### Interações

- **Frontend ↔ Backend**: REST.
- **Backend ↔ Database**: SQL.
- **Backend ↔ Email Service**: SMTP (assíncrono).
- **Backend ↔ Notification Service**: Push via filas.
- **Backend ↔ Backup Service**: Upload/download de arquivos.
- **Backend ↔ Cache**: Redis.
- **Frontend ↔ Email Service**: Links de verificação.
- **Frontend ↔ Notification Service**: Notificações push.

### Notas Técnicas

- **Escalabilidade**: Filas e cache.
- **Segurança**: HTTPS, JWT.
- **PWA**: Modo offline.
