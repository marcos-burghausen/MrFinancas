🔙 [Retornar à documentação principal](../../README.md)

# Caso de Uso UML: Exportar para Calendário

## Diagrama de Caso de Uso

```mermaid
classDiagram
    class Usuário {
        +ExportarParaCalendário()
    }
    class GoogleCalendar {
        +CriarEvento()
    }
    Usuário --> GoogleCalendar : usa
    Usuário --> Sistema : interage
```

## Descrição

Permite que o usuário exporte vencimentos de lançamentos para o Google Calendar.

## Atores

- **Primário**: Usuário
- **Secundário**: Google Calendar

## Pré-condições

- Usuário está autenticado.
- Lançamento possui vencimento.

## Pós-condições

- Evento é criado no Google Calendar.
- Confirmação é exibida.

## Fluxo Principal

1. Usuário seleciona lançamento com vencimento.
2. Clica em "Exportar para calendário".
3. Sistema autentica com Google Calendar.
4. Cria evento (data, título, descrição).
5. Exibe confirmação.

## Fluxos Alternativos

- **A1**: Falha na autenticação → Exibe erro e retorna à lista.

## Regras de Negócio

- Apenas lançamentos com vencimento são exportáveis.
- Títulos incluem tipo (ex.: "Despesa: Aluguel").
- Autenticação é armazenada para reutilização.

## Integrações

- Usa API do Google Calendar.
- Integra com lançamentos e faturas.
- Eventos aparecem em relatórios.
