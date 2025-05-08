🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Sequência: Realizar Backup e Restauração

Este diagrama descreve as interações para criar e restaurar backups.

## Diagrama de Sequência

```mermaid
sequenceDiagram
    participant U as Usuário
    participant F as Frontend (Vue.js)
    participant B as Backend (Laravel)
    participant DB as Banco (MySQL)
    participant S as AWS S3

    U->>F: Acessa tela de backup
    F->>U: Exibe opções (criar, restaurar)
    alt Criar Backup
        U->>F: Clica "Criar backup"
        F->>B: POST /backup
        B->>DB: Exporta dados do usuário
        DB-->>B: Dados exportados
        B->>S: Salva arquivo no S3
        S-->>B: URL do arquivo
        B->>DB: Registra backup
        DB-->>B: Backup registrado
        B-->>F: Resposta: { backup_id, urlDownload }
        F-->>U: Exibe "Backup criado"
    else Restaurar Backup
        F->>B: GET /backups
        B-->>F: Lista de backups
        F-->>U: Exibe lista
        U->>F: Seleciona backup
        F->>B: POST /restore (backup_id)
        B->>S: Recupera arquivo do S3
        S-->>B: Arquivo retornado
        B->>DB: Valida integridade
        DB-->>B: Dados válidos
        B->>DB: Restaura dados
        DB-->>B: Dados restaurados
        B-->>F: Resposta: "Backup restaurado"
        F-->>U: Exibe "Backup restaurado"
    end
```

## Descrição

### Fluxo Principal

1. O usuário acessa a tela de backup.
2. O frontend exibe opções: criar ou restaurar.
   - **Criar**:
     - Usuário clica "Criar backup".
     - Frontend envia `POST /backup`.
     - Backend exporta dados, salva no S3, registra backup, retorna `{ backup_id, urlDownload }`.
     - Frontend exibe confirmação.
   - **Restaurar**:
     - Frontend lista backups via `GET /backups`.
     - Usuário seleciona backup, envia `POST /restore`.
     - Backend recupera arquivo, valida, restaura dados, retorna confirmação.
     - Frontend exibe "Backup restaurado".

### Fluxos Alternativos

- **A1**: Erro no S3 → Backend retorna erro 500 e notifica via SNS.
- **A2**: Backup inválido → Backend retorna erro 422 ("Backup inválido").
- **A3**: Erro no banco → Backend retorna erro 500.

### Notas Técnicas

- **Frontend**: Vue.js, Vuetify, Axios.
- **Backend**: Laravel com AWS SDK.
  - Endpoints: `POST /backup`, `POST /restore`, `GET /backups` (controlador `BackupController`).
- **Banco**: Tabela `backups` (`id`, `user_id`, `url`, `created_at`).
- **Integração**: AWS S3 para armazenamento.
- **Segurança**: JWT para autenticação.