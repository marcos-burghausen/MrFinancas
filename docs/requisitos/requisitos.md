# [ <- VOLTAR](../../README.md)

# Requisitos do Aplicativo de Finanças Pessoal

## 1. Introdução

Este documento define os requisitos funcionais e não funcionais para o desenvolvimento de um aplicativo de finanças pessoais, com foco na versão inicial (v1) e preparação para futuras expansões (ex.: controle de investimentos).

**Stack Tecnológica:**

- **Frontend:** Vue.js 3
- **Backend:** Laravel
- **Containerização:** Docker
- **Banco de dados:** MySQL

## 2. Requisitos Funcionais

### 2.1. Gerenciamento de Usuários

| ID    | Requisito                                                                                                                                                                                                                                                                                               | Versão             |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| RF1.1 | O sistema deve permitir cadastro de usuários com e-mail/senha ou via OAuth (Facebook, Google, LinkedIn)                                                                                                                                                                                                 | Facebook na v1     |
| RF1.2 | O sistema deve exigir verificação de e-mail para cadastros tradicionais                                                                                                                                                                                                                                 | v1                 |
| RF1.3 | O sistema deve oferecer recuperação de senha via e-mail                                                                                                                                                                                                                                                 | v1                 |
| RF1.4 | O sistema deve suportar autenticação multifator (MFA) opcional (ex.: SMS, app autenticador)                                                                                                                                                                                                             | v1                 |
| RF1.5 | O sistema deve permitir logout de sessões ativas em outros dispositivos                                                                                                                                                                                                                                 | v1                 |
| RF1.6 | O sistema deve suportar diferentes tipos de usuários (USER, TRADER, USER_TRADER, ADMIN, FULL)                                                                                                                                                                                                           | Somente USER na v1 |
| RF1.7 | O sistema deve permitir ao usuário gerenciar seu perfil, incluindo:<br>- Alteração de avatar (upload ou escolha predefinida)<br>- Alteração de senha<br>- Configuração de notificações (vencimentos, antecedência de até 3 dias, horário)<br>- Cadastro/edição de dados pessoais (endereço, documentos) | v1                 |
| RF1.8 | O sistema deve permitir desativar notificações específicas                                                                                                                                                                                                                                              | v1                 |

### 2.2. Gerenciamento de Lançamentos

| ID    | Requisito                                                                                                                                                                                                                             | Versão |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ |
| RF2.1 | O sistema deve permitir cadastro de lançamentos (despesas, receitas, despesas de cartão de crédito, estornos)                                                                                                                         | v1     |
| RF2.2 | Cada lançamento deve ser vinculado a uma conta ou cartão de crédito                                                                                                                                                                   | v1     |
| RF2.3 | Cada lançamento deve ser associado a uma categoria/subcategoria                                                                                                                                                                       | v1     |
| RF2.4 | O sistema deve suportar lançamentos recorrentes (ex.: diário, semanal, mensal)                                                                                                                                                        | v1     |
| RF2.5 | O sistema deve permitir marcar lançamentos como pendente, efetivado ou cancelado                                                                                                                                                      | v1     |
| RF2.6 | O sistema deve suportar transferências entre contas                                                                                                                                                                                   | v1     |
| RF2.7 | O sistema deve permitir importação/exportação de lançamentos em CSV                                                                                                                                                                   | v1     |
| RF2.8 | O sistema deve exibir relatórios financeiros, incluindo:<br>- Gráficos de despesas/receitas por categoria/subcategoria<br>- Balanço mensal (receitas - despesas)<br>- Histórico de lançamentos filtrável por data, categoria ou conta | v1     |

### 2.3. Gerenciamento de Contas e Cartões

| ID    | Requisito                                                                                                                           | Versão             |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------- | ------------------ |
| RF3.1 | O sistema deve permitir cadastro/edição de contas com nome, categoria, ícone e cor personalizáveis                                  | v1                 |
| RF3.2 | O sistema deve permitir cadastro/edição de cartões de crédito vinculados a uma conta, com nome, limite, ícone e cor personalizáveis | v1                 |
| RF3.3 | O sistema deve gerenciar faturas de cartões, exibindo gastos acumulados e vencimentos                                               | v1                 |
| RF3.4 | O sistema deve suportar conciliação bancária (comparação de lançamentos com extratos importados)                                    | v1                 |
| RF3.5 | O sistema deve permitir configuração de múltiplas moedas (preparação para internacionalização)                                      | Preparação para v2 |

### 2.4. Gerenciamento de Categorias

| ID    | Requisito                                                                                             | Versão |
| ----- | ----------------------------------------------------------------------------------------------------- | ------ |
| RF4.1 | O sistema deve permitir cadastro/edição de categorias e subcategorias com ícone e cor personalizáveis | v1     |
| RF4.2 | O sistema deve oferecer categorias/subcategorias predefinidas (ex.: Alimentação, Transporte)          | v1     |
| RF4.3 | O sistema deve suportar hierarquia de subcategorias (múltiplos níveis)                                | v1     |
| RF4.4 | O sistema deve validar para evitar categorias duplicadas ou conflitantes                              | v1     |

### 2.5. Notificações e Integrações

| ID    | Requisito                                                                                                    | Versão               |
| ----- | ------------------------------------------------------------------------------------------------------------ | -------------------- |
| RF5.1 | O sistema deve enviar notificações de vencimentos (no dia ou até 3 dias antes, com horário configurável)     | v1                   |
| RF5.2 | O sistema deve permitir integração com calendários externos (ex.: Google Calendar) para exportar vencimentos | v1                   |
| RF5.3 | O sistema deve suportar notificações push (mobile) e por e-mail                                              | Somente e-mail na v1 |

### 2.6. Experiência do Usuário

| ID    | Requisito                                                                                | Versão  |
| ----- | ---------------------------------------------------------------------------------------- | ------- |
| RF6.1 | O sistema deve oferecer suporte a temas claro/escuro                                     | v1      |
| RF6.2 | O sistema deve permitir uso offline, com sincronização posterior                         | Após v1 |
| RF6.3 | O sistema deve oferecer metas financeiras (ex.: economizar X por mês) com acompanhamento | v1      |
| RF6.4 | O sistema deve permitir backup/restauração de dados do usuário                           | v1      |

### 2.7. Administração (Preparação para v2)

| ID    | Requisito                                                                                                        | Versão             |
| ----- | ---------------------------------------------------------------------------------------------------------------- | ------------------ |
| RF7.1 | O sistema deve suportar RBAC (controle de acesso baseado em papéis) para futuros tipos de usuários (ADMIN, FULL) | Preparação para v2 |
| RF7.2 | O sistema deve registrar logs de atividades críticas (ex.: alterações de lançamentos)                            | Preparação para v2 |

## 3. Requisitos Não Funcionais

### 3.1. Desempenho

| ID     | Requisito                                                                               | Versão |
| ------ | --------------------------------------------------------------------------------------- | ------ |
| RNF1.1 | O sistema deve responder a interações do usuário em até 2 segundos em condições normais | v1     |
| RNF1.2 | O sistema deve suportar até 1.000 usuários simultâneos na v1                            | v1     |

### 3.2. Segurança

| ID     | Requisito                                                            | Versão |
| ------ | -------------------------------------------------------------------- | ------ |
| RNF2.1 | Todas as comunicações devem usar HTTPS (SSL/TLS)                     | v1     |
| RNF2.2 | Senhas devem ser armazenadas com criptografia bcrypt                 | v1     |
| RNF2.3 | Dados sensíveis (ex.: documentos) devem ser criptografados no banco  | v1     |
| RNF2.4 | O sistema deve implementar proteção contra CSRF, XSS e SQL Injection | v1     |
| RNF2.5 | O sistema deve validar todas as entradas no frontend e backend       | v1     |

### 3.3. Escalabilidade

| ID     | Requisito                                                                       | Versão |
| ------ | ------------------------------------------------------------------------------- | ------ |
| RNF3.1 | O sistema deve ser modular para suportar novos módulos (ex.: investimentos)     | v1     |
| RNF3.2 | O sistema deve usar cache (ex.: Redis) para consultas frequentes                | v1     |
| RNF3.3 | O sistema deve suportar processamento assíncrono (ex.: filas para notificações) | v1     |

### 3.4. Usabilidade

| ID     | Requisito                                                                                         | Versão |
| ------ | ------------------------------------------------------------------------------------------------- | ------ |
| RNF4.1 | A interface deve ser responsiva (desktop, tablet, mobile)                                         | v1     |
| RNF4.2 | O sistema deve seguir diretrizes de acessibilidade (ex.: WCAG 2.1)                                | v1     |
| RNF4.3 | O sistema deve ser intuitivo, com tempo de aprendizado inferior a 10 minutos para usuários comuns | v1     |

### 3.5. Manutenibilidade

| ID     | Requisito                                                                                      | Versão |
| ------ | ---------------------------------------------------------------------------------------------- | ------ |
| RNF5.1 | O código deve seguir padrões de boas práticas (ex.: PSR-12 para PHP, ESLint para JavaScript)   | v1     |
| RNF5.2 | O sistema deve ter cobertura de testes automatizados (unitários, integração) de pelo menos 80% | v1     |
| RNF5.3 | O sistema deve usar CI/CD para testes e deploys automáticos                                    | v1     |

### 3.6. Compatibilidade

| ID     | Requisito                                                                              | Versão |
| ------ | -------------------------------------------------------------------------------------- | ------ |
| RNF6.1 | O sistema deve ser compatível com navegadores modernos (Chrome, Firefox, Safari, Edge) | v1     |
| RNF6.2 | O sistema deve suportar dispositivos iOS e Android (via PWA ou app nativo no futuro)   | v1     |

## 4. Considerações

- Os requisitos foram estruturados para a v1, com preparação para expansões (ex.: investimentos, tipos de usuários adicionais).
- Fluxogramas serão usados para mapear fluxos de usuário, seguidos por diagramas UML (casos de uso, classes) na fase de design técnico.
- A stack (Vue.js 3, Laravel, Docker, MySQL) é adequada para os requisitos listados.

## 5. Matriz de Rastreabilidade

| Módulo         | Total Requisitos | Requisitos v1 | Preparação v2 |
| -------------- | ---------------- | ------------- | ------------- |
| Usuários       | 8                | 8             | 1             |
| Lançamentos    | 8                | 8             | 0             |
| Contas/Cartões | 5                | 4             | 1             |
| Categorias     | 4                | 4             | 0             |
| Notificações   | 3                | 3             | 0             |
| UX             | 4                | 3             | 1             |
| Administração  | 2                | 0             | 2             |
| **Total RF**   | **34**           | **30**        | **4**         |
| **Total RNF**  | **17**           | **17**        | **0**         |

---

_Última atualização: Maio 2025_
