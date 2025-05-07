# [ <- VOLTAR](../../README.md)

# Caso de Uso UML: Configurar Notificações

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +ConfigurarNotificações()
    }
    class SistemaNotificações {
        +EnviarNotificação()
    }
    Usuário --> SistemaNotificações : configura
    Usuário --> Sistema : interage
```

## Descrição

Permite que o usuário configure notificações para vencimentos e metas.

## Atores

- **Primário**: Usuário
- **Secundário**: Sistema de Notificações

## Pré-condições

- Usuário está autenticado.
- Existem lançamentos com vencimentos.

## Pós-condições

- Configurações são salvas.
- Notificações são agendadas.

## Fluxo Principal

1. Usuário acessa tela de notificações.
2. Seleciona tipo (vencimentos, metas).
3. Configura opções (ativar/desativar, antecedência, horário).
4. Sistema valida e salva.
5. Agenda notificações via fila.
6. Exibe confirmação.

## Fluxos Alternativos

- **A1**: Configuração inválida → Exibe erro e retorna ao formulário.

## Regras de Negócio

- Antecedência máxima é 3 dias.
- Horários são ajustados ao fuso horário.
- Notificações push exigem permissão.

## Integrações

- Usa filas do Laravel (Horizon).
- Integra com lançamentos e faturas.
- Suporta push e e-mail.
