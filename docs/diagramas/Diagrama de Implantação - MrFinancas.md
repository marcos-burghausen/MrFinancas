🔙 [Retornar à documentação principal](../../README.md)

# Diagrama de Implantação: MrFinanças

Este diagrama descreve a infraestrutura em AWS com Docker.

## Diagrama de Implantação

```mermaid
deploymentDiagram
    node "AWS Cloud" {
        node "VPC" {
            node "EC2 (Backend)" {
                [Laravel App]
                [Nginx]
                [PHP-FPM]
            }
            node "S3 + CloudFront (Frontend)" {
                [Vue.js App]
            }
            node "RDS (MySQL)" {
                [Database]
            }
            node "ElastiCache (Redis)" {
                [Cache]
            }
        }
        node "SES" {
            [Email Service]
        }
        node "SNS" {
            [Push Notification Service]
        }
        node "S3 (Backups)" {
            [Cloud Storage Service]
        }
        node "New Relic" {
            [Monitoring]
        }
    }
    node "GitHub" {
        [Repository]
        [GitHub Actions]
    }

    [Vue.js App] --> [Laravel App] : HTTPS (REST)
    [Laravel App] --> [Database] : SQL
    [Laravel App] --> [Cache] : Redis
    [Laravel App] --> [Email Service] : SMTP
    [Laravel App] --> [Push Notification Service] : Push
    [Laravel App] --> [Cloud Storage Service] : File Transfer
    [Laravel App] --> [Monitoring] : Metrics
    [GitHub Actions] --> [EC2 (Backend)] : Deploy
    [GitHub Actions] --> [S3 + CloudFront (Frontend)] : Deploy
```

## Descrição

### Nós de Implantação

- **AWS Cloud**:
  - **VPC**:
    - **EC2 (Backend)**: Laravel com Docker.
    - **S3 + CloudFront (Frontend)**: Vue.js com CDN.
    - **RDS (MySQL)**: Banco.
    - **ElastiCache (Redis)**: Cache.
  - **SES**: E-mails.
  - **SNS**: Notificações push.
  - **S3 (Backups)**: Armazenamento de backups.
  - **New Relic**: Monitoramento.
- **GitHub**:
  - **Repository**: Código.
  - **GitHub Actions**: CI/CD.

### Interações

- **Frontend ↔ Backend**: HTTPS.
- **Backend ↔ Database**: SQL.
- **Backend ↔ Cache**: Redis.
- **Backend ↔ Email Service**: SMTP.
- **Backend ↔ Push Notification Service**: SNS.
- **Backend ↔ Cloud Storage Service**: S3.
- **Backend ↔ Monitoring**: Metrics.
- **GitHub Actions ↔ AWS**: Deploy.

### Notas Técnicas

- **Docker**: Containers no EC2.
- **Escalabilidade**: Auto Scaling, réplicas RDS.
- **Segurança**: VPC, IAM, HTTPS.
- **CI/CD**: GitHub Actions.
