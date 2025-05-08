🔙 [Retornar à documentação principal](../../README.md)

# Documentação da API do MrFinancas

Esta documentação descreve os endpoints disponíveis na API do MrFinancas, seus parâmetros e respostas.

## Base URL

```
https://api.mrfinancas.com/v1
```

## Autenticação

A API do MrFinancas utiliza autenticação baseada em tokens JWT. Para acessar endpoints protegidos, inclua o token no cabeçalho da requisição:

```
Authorization: Bearer {seu_token}
```

Para obter um token, utilize o endpoint de login.

## Endpoints

### Autenticação

#### Registro de Usuário

```
POST /auth/register
```

Registra um novo usuário no sistema.

**Parâmetros do corpo (JSON):**

| Campo | Tipo   | Descrição                   | Obrigatório |
| ----- | ------ | --------------------------- | ----------- |
| nome  | string | Nome completo do usuário    | Sim         |
| email | string | Email único do usuário      | Sim         |
| senha | string | Senha (mínimo 8 caracteres) | Sim         |

**Exemplo de requisição:**

```json
{
  "nome": "João Silva",
  "email": "joao@exemplo.com",
  "senha": "senha123segura"
}
```

**Resposta de sucesso (200):**

```json
{
  "id": "5f8d0e1b2c3d4e5f6g7h8i9j",
  "nome": "João Silva",
  "email": "joao@exemplo.com",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

#### Login

```
POST /auth/login
```

Autentica um usuário e retorna um token JWT.

**Parâmetros do corpo (JSON):**

| Campo | Tipo   | Descrição        | Obrigatório |
| ----- | ------ | ---------------- | ----------- |
| email | string | Email do usuário | Sim         |
| senha | string | Senha do usuário | Sim         |

**Exemplo de requisição:**

```json
{
  "email": "joao@exemplo.com",
  "senha": "senha123segura"
}
```

**Resposta de sucesso (200):**

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "usuario": {
    "id": "5f8d0e1b2c3d4e5f6g7h8i9j",
    "nome": "João Silva",
    "email": "joao@exemplo.com"
  }
}
```

### Transações

#### Listar Transações

```
GET /transacoes
```

Retorna a lista de transações do usuário autenticado.

**Parâmetros de consulta (Query):**

| Parâmetro  | Tipo                | Descrição                                       | Obrigatório      |
| ---------- | ------------------- | ----------------------------------------------- | ---------------- |
| dataInicio | string (YYYY-MM-DD) | Data de início para filtrar                     | Não              |
| dataFim    | string (YYYY-MM-DD) | Data de fim para filtrar                        | Não              |
| categoria  | string              | ID da categoria para filtrar                    | Não              |
| contaId    | string              | ID da conta para filtrar                        | Não              |
| tipo       | string              | Tipo: "receita", "despesa", "cartao", "estorno" | Não              |
| pagina     | number              | Número da página para paginação                 | Não (padrão: 1)  |
| limite     | number              | Quantidade de itens por página                  | Não (padrão: 20) |

**Resposta de sucesso (200):**

```json
{
  "total": 45,
  "paginas": 3,
  "paginaAtual": 1,
  "transacoes": [
    {
      "id": "abc123def456",
      "descricao": "Salário",
      "valor": 3000.0,
      "data": "2023-05-05",
      "tipo": "receita",
      "categoria": {
        "id": "cat123",
        "nome": "Salário",
        "cor": "#28a745"
      },
      "conta": {
        "id": "cont456",
        "nome": "Conta Corrente"
      },
      "observacao": "Salário mensal",
      "dataCriacao": "2023-05-01T10:30:00Z"
    }
  ]
}
```

#### Obter Transação Específica

```
GET /transacoes/{id}
```

Retorna os detalhes de uma transação específica.

**Parâmetros de caminho:**

| Parâmetro | Descrição       |
| --------- | --------------- |
| id        | ID da transação |

**Resposta de sucesso (200):**

```json
{
  "id": "abc123def456",
  "descricao": "Salário",
  "valor": 3000.0,
  "data": "2023-05-05",
  "tipo": "receita",
  "categoria": {
    "id": "cat123",
    "nome": "Salário",
    "cor": "#28a745"
  },
  "conta": {
    "id": "cont456",
    "nome": "Conta Corrente"
  },
  "observacao": "Salário mensal",
  "dataCriacao": "2023-05-01T10:30:00Z",
  "dataAtualizacao": "2023-05-01T10:30:00Z"
}
```

#### Criar Nova Transação

```
POST /transacoes
```

Cria uma nova transação para o usuário autenticado.

**Parâmetros do corpo (JSON):**

| Campo       | Tipo                | Descrição                                 | Obrigatório |
| ----------- | ------------------- | ----------------------------------------- | ----------- |
| descricao   | string              | Descrição da transação                    | Sim         |
| valor       | number              | Valor da transação                        | Sim         |
| data        | string (YYYY-MM-DD) | Data da transação                         | Sim         |
| tipo        | string              | "receita", "despesa", "cartao", "estorno" | Sim         |
| categoriaId | string              | ID da categoria                           | Sim         |
| contaId     | string              | ID da conta                               | Não         |
| cardId      | string              | ID do cartão (para "cartao")              | Não         |
| observacao  | string              | Observações adicionais                    | Não         |

**Exemplo de requisição:**

```json
{
  "descricao": "Compra no supermercado",
  "valor": 150.75,
  "data": "2023-05-10",
  "tipo": "despesa",
  "categoriaId": "cat789",
  "contaId": "cont456",
  "observacao": "Compras da semana"
}
```

**Resposta de sucesso (201):**

```json
{
  "id": "xyz789abc123",
  "descricao": "Compra no supermercado",
  "valor": 150.75,
  "data": "2023-05-10",
  "tipo": "despesa",
  "categoria": {
    "id": "cat789",
    "nome": "Alimentação",
    "cor": "#dc3545"
  },
  "conta": {
    "id": "cont456",
    "nome": "Conta Corrente"
  },
  "observacao": "Compras da semana",
  "dataCriacao": "2023-05-10T14:22:10Z"
}
```

#### Atualizar Transação

```
PUT /transacoes/{id}
```

Atualiza uma transação existente.

**Parâmetros de caminho:**

| Parâmetro | Descrição       |
| --------- | --------------- |
| id        | ID da transação |

**Parâmetros do corpo (JSON):** Mesmos campos da criação.

**Resposta de sucesso (200):** Objeto da transação atualizada.

#### Excluir Transação

```
DELETE /transacoes/{id}
```

Remove uma transação existente (soft delete).

**Parâmetros de caminho:**

| Parâmetro | Descrição       |
| --------- | --------------- |
| id        | ID da transação |

**Resposta de sucesso (204):** Sem conteúdo.

**Erro (409):**

```json
{
  "erro": true,
  "mensagem": "Transação vinculada a fatura",
  "detalhes": {}
}
```

### Categorias

#### Listar Categorias

```
GET /categorias
```

Retorna todas as categorias do usuário.

**Parâmetros de consulta (Query):**

| Parâmetro | Tipo   | Descrição                                | Obrigatório |
| --------- | ------ | ---------------------------------------- | ----------- |
| tipo      | string | Filtrar por tipo: "receita" ou "despesa" | Não         |

**Resposta de sucesso (200):**

```json
[
  {
    "id": "cat123",
    "nome": "Salário",
    "tipo": "receita",
    "cor": "#28a745",
    "icone": "salary"
  },
  {
    "id": "cat456",
    "nome": "Aluguel",
    "tipo": "despesa",
    "cor": "#dc3545",
    "icone": "home"
  }
]
```

#### Criar Categoria

```
POST /categorias
```

Cria uma nova categoria.

**Parâmetros do corpo (JSON):**

| Campo | Tipo         | Descrição                  | Obrigatório                        |
| ----- | ------------ | -------------------------- | ---------------------------------- |
| nome  | string       | Nome da categoria          | Sim                                |
| tipo  | string       | "receita" ou "despesa"     | Sim                                |
| cor   | string (hex) | Cor em formato hexadecimal | Não (padrão definido pelo sistema) |
| icone | string       | Identificador do ícone     | Não                                |

**Resposta de sucesso (201):** Objeto da categoria criada.

#### Atualizar Categoria

```
PUT /categorias/{id}
```

Atualiza uma categoria existente.

**Parâmetros de caminho:**

| Parâmetro | Descrição       |
| --------- | --------------- |
| id        | ID da categoria |

**Parâmetros do corpo (JSON):** Mesmos campos da criação.

**Resposta de sucesso (200):** Objeto da categoria atualizada.

#### Excluir Categoria

```
DELETE /categorias/{id}
```

Remove uma categoria existente.

**Parâmetros de caminho:**

| Parâmetro | Descrição       |
| --------- | --------------- |
| id        | ID da categoria |

**Resposta de sucesso (204):** Sem conteúdo.

### Contas

#### Listar Contas

```
GET /contas
```

Retorna todas as contas do usuário.

**Resposta de sucesso (200):**

```json
[
  {
    "id": "cont123",
    "nome": "Conta Poupança",
    "saldoInicial": 1000.0,
    "saldoAtual": 2500.75,
    "tipo": "poupanca",
    "cor": "#007bff",
    "instituicao": "Banco ABC",
    "ativa": true
  }
]
```

#### Criar Conta

```
POST /contas
```

Cria uma nova conta.

**Parâmetros do corpo (JSON):**

| Campo        | Tipo         | Descrição                               | Obrigatório |
| ------------ | ------------ | --------------------------------------- | ----------- |
| nome         | string       | Nome da conta                           | Sim         |
| saldoInicial | number       | Saldo inicial da conta                  | Sim         |
| tipo         | string       | Tipo da conta (corrente, poupanca, etc) | Sim         |
| cor          | string (hex) | Cor em formato hexadecimal              | Não         |
| instituicao  | string       | Nome da instituição financeira          | Não         |

**Resposta de sucesso (201):** Objeto da conta criada.

#### Atualizar Conta

```
PUT /contas/{id}
```

Atualiza uma conta existente.

**Parâmetros de caminho:**

| Parâmetro | Descrição   |
| --------- | ----------- |
| id        | ID da conta |

**Parâmetros do corpo (JSON):** Mesmos campos da criação.

**Resposta de sucesso (200):** Objeto da conta atualizada.

#### Excluir Conta

```
DELETE /contas/{id}
```

Remove uma conta existente.

**Parâmetros de caminho:**

| Parâmetro | Descrição   |
| --------- | ----------- |
| id        | ID da conta |

**Resposta de sucesso (204):** Sem conteúdo.

### Cartões

#### Listar Cartões

```
GET /cards
```

Retorna todos os cartões do usuário.

**Resposta de sucesso (200):**

```json
[
  {
    "id": "card123",
    "nome": "Cartão Visa",
    "limite": 5000.0,
    "diaVencimento": 10,
    "diaFechamento": 5,
    "saldoAtual": 1200.5,
    "instituicao": "Banco XYZ",
    "ativa": true
  }
]
```

#### Criar Cartão

```
POST /cards
```

Cria um novo cartão.

**Parâmetros do corpo (JSON):**

| Campo         | Tipo   | Descrição                          | Obrigatório |
| ------------- | ------ | ---------------------------------- | ----------- |
| nome          | string | Nome do cartão                     | Sim         |
| limite        | number | Limite de crédito                  | Sim         |
| diaVencimento | number | Dia de vencimento da fatura (1-31) | Sim         |
| diaFechamento | number | Dia de fechamento da fatura (1-31) | Sim         |
| instituicao   | string | Nome da instituição financeira     | Não         |

**Resposta de sucesso (201):** Objeto do cartão criado.

#### Atualizar Cartão

```
PUT /cards/{id}
```

Atualiza um cartão existente.

**Parâmetros de caminho:**

| Parâmetro | Descrição    |
| --------- | ------------ |
| id        | ID do cartão |

**Parâmetros do corpo (JSON):** Mesmos campos da criação.

**Resposta de sucesso (200):** Objeto do cartão atualizado.

#### Excluir Cartão

```
DELETE /cards/{id}
```

Remove um cartão existente.

**Parâmetros de caminho:**

| Parâmetro | Descrição    |
| --------- | ------------ |
| id        | ID do cartão |

**Resposta de sucesso (204):** Sem conteúdo.

### Faturas

#### Listar Faturas de um Cartão

```
GET /cards/{id}/invoices
```

Retorna as faturas de um cartão específico.

**Parâmetros de caminho:**

| Parâmetro | Descrição    |
| --------- | ------------ |
| id        | ID do cartão |

**Parâmetros de consulta (Query):**

| Parâmetro  | Tipo                | Descrição                           | Obrigatório      |
| ---------- | ------------------- | ----------------------------------- | ---------------- |
| dataInicio | string (YYYY-MM-DD) | Data de início para filtrar faturas | Não              |
| dataFim    | string (YYYY-MM-DD) | Data de fim para filtrar faturas    | Não              |
| pagina     | number              | Número da página para paginação     | Não (padrão: 1)  |
| limite     | number              | Quantidade de itens por página      | Não (padrão: 20) |

**Resposta de sucesso (200):**

```json
{
  "total": 12,
  "paginas": 1,
  "paginaAtual": 1,
  "faturas": [
    {
      "id": "inv123",
      "cartaoId": "card123",
      "dataVencimento": "2023-06-10",
      "dataFechamento": "2023-06-05",
      "valor": 1200.5,
      "status": "aberta",
      "transacoes": [
        {
          "id": "trans789",
          "descricao": "Compra online",
          "valor": 300.25,
          "data": "2023-06-01"
        }
      ]
    }
  ]
}
```

### Relatórios

#### Resumo por Período

```
GET /relatorios/resumo
```

Retorna um resumo financeiro do período especificado.

**Parâmetros de consulta (Query):**

| Parâmetro  | Tipo                | Descrição              | Obrigatório |
| ---------- | ------------------- | ---------------------- | ----------- |
| dataInicio | string (YYYY-MM-DD) | leachingData de início | Sim         |
| dataFim    | string (YYYY-MM-DD) | Data de fim            | Sim         |

**Resposta de sucesso (200):**

```json
{
  "totalReceitas": 5000.0,
  "totalDespesas": 3250.75,
  "saldo": 1749.25,
  "receitasPorCategoria": [
    {
      "categoria": {
        "id": "cat123",
        "nome": "Salário",
        "cor": "#28a745"
      },
      "valor": 4500.0,
      "percentual": 90
    }
  ],
  "despesasPorCategoria": [
    {
      "categoria": {
        "id": "cat789",
        "nome": "Alimentação",
        "cor": "#dc3545"
      },
      "valor": 1200.5,
      "percentual": 36.93
    }
  ]
}
```

#### Fluxo de Caixa Mensal

```
GET /relatorios/fluxo-caixa
```

Retorna o fluxo de caixa mensal para o período especificado.

**Parâmetros de consulta (Query):**

| Parâmetro | Tipo   | Descrição  | Obrigatório                                     |
| --------- | ------ | ---------- | ----------------------------------------------- |
| ano       | number | Ano        | Sim                                             |
| mes       | number | Mês (1-12) | Não (retorna o ano inteiro se não especificado) |

**Resposta de sucesso (200):**

```json
[
  {
    "data": "2023-05",
    "receitas": 5000.0,
    "despesas": 3250.75,
    "saldo": 1749.25
  }
]
```

### Calendário

#### Exportar Transação para Calendário

```
POST /calendar/export
```

Exporta uma transação como evento no Google Calendar.

**Parâmetros do corpo (JSON):**

| Campo | Tipo   | Descrição       | Obrigatório |
| ----- | ------ | --------------- | ----------- |
| id    | string | ID da transação | Sim         |

**Exemplo de requisição:**

```json
{
  "id": "trans789"
}
```

**Resposta de sucesso (201):**

```json
{
  "event_id": "event123456789",
  "transacao_id": "trans789",
  "calendario": "primary",
  "summary": "Despesa: Compra no supermercado",
  "data": "2023-05-10"
}
```

**Erro (401):**

```json
{
  "erro": true,
  "mensagem": "Falha na autenticação com Google Calendar",
  "detalhes": {}
}
```

**Erro (404):**

```json
{
  "erro": true,
  "mensagem": "Transação não encontrada",
  "detalhes": {}
}
```

### Notificações

#### Listar Configurações de Notificações

```
GET /notifications
```

Retorna as configurações de notificações do usuário.

**Resposta de sucesso (200):**

```json
{
  "notificacoes": {
    "email": {
      "saldoBaixo": true,
      "vencimentoFatura": true,
      "relatorioSemanal": false
    },
    "push": {
      "saldoBaixo": true,
      "vencimentoFatura": false,
      "relatorioSemanal": false
    }
  }
}
```

#### Configurar Notificações

```
POST /notifications
```

Atualiza as configurações de notificações do usuário.

**Parâmetros do corpo (JSON):**

| Campo                  | Tipo    | Descrição                              | Obrigatório |
| ---------------------- | ------- | -------------------------------------- | ----------- |
| email                  | object  | Configurações de notificação por email | Não         |
| email.saldoBaixo       | boolean | Notificar saldo baixo                  | Não         |
| email.vencimentoFatura | boolean | Notificar vencimento de fatura         | Não         |
| email.relatorioSemanal | boolean | Notificar relatório semanal            | Não         |
| push                   | object  | Configurações de notificação push      | Não         |
| push.saldoBaixo        | boolean | Notificar saldo baixo                  | Não         |
| push.vencimentoFatura  | boolean | Notificar vencimento de fatura         | Não         |
| push.relatorioSemanal  | boolean | Notificar relatório semanal            | Não         |

**Exemplo de requisição:**

```json
{
  "email": {
    "saldoBaixo": true,
    "vencimentoFatura": true,
    "relatorioSemanal": false
  },
  "push": {
    "saldoBaixo": true,
    "vencimentoFatura": false,
    "relatorioSemanal": false
  }
}
```

**Resposta de sucesso (200):** Objeto das configurações atualizadas.

### Backup e Restauração

#### Criar Backup

```
POST /backup
```

Cria um backup dos dados do usuário.

**Resposta de sucesso (201):**

```json
{
  "backup_id": "backup123",
  "dataCriacao": "2023-05-10T14:22:10Z",
  "tamanho": 102400,
  "urlDownload": "https://s3.amazonaws.com/backups/backup123.zip"
}
```

#### Restaurar Backup

```
POST /restore
```

Restaura os dados a partir de um backup.

**Parâmetros do corpo (JSON):**

| Campo     | Tipo   | Descrição    | Obrigatório |
| --------- | ------ | ------------ | ----------- |
| backup_id | string | ID do backup | Sim         |

**Exemplo de requisição:**

```json
{
  "backup_id": "backup123"
}
```

**Resposta de sucesso (200):**

```json
{
  "mensagem": "Backup restaurado com sucesso",
  "dataRestauracao": "2023-05-10T14:30:00Z"
}
```

### Perfil

#### Obter Perfil do Usuário

```
GET /profile
```

Retorna os dados do perfil do usuário autenticado.

**Resposta de sucesso (200):**

```json
{
  "id": "5f8d0e1b2c3d4e5f6g7h8i9j",
  "nome": "João Silva",
  "email": "joao@exemplo.com",
  "avatar": "https://s3.amazonaws.com/avatars/joao.png",
  "idioma": "pt-BR",
  "moeda": "BRL"
}
```

#### Atualizar Perfil

```
PUT /profile
```

Atualiza os dados do perfil do usuário.

**Parâmetros do corpo (JSON):**

| Campo  | Tipo   | Descrição                     | Obrigatório |
| ------ | ------ | ----------------------------- | ----------- |
| nome   | string | Nome completo do usuário      | Não         |
| email  | string | Email do usuário              | Não         |
| idioma | string | Código do idioma (ex.: pt-BR) | Não         |
| moeda  | string | Código da moeda (ex.: BRL)    | Não         |

**Exemplo de requisição:**

```json
{
  "nome": "João Silva",
  "idioma": "pt-BR",
  "moeda": "BRL"
}
```

**Resposta de sucesso (200):** Objeto do perfil atualizado.

#### Atualizar Senha

```
PUT /profile/password
```

Atualiza a senha do usuário.

**Parâmetros do corpo (JSON):**

| Campo      | Tipo   | Descrição                      | Obrigatório |
| ---------- | ------ | ------------------------------ | ----------- |
| senhaAtual | string | Senha atual do usuário         | Sim         |
| novaSenha  | string | Nova senha (mín. 8 caracteres) | Sim         |

**Exemplo de requisição:**

```json
{
  "senhaAtual": "senha123segura",
  "novaSenha": "nova456segura"
}
```

**Resposta de sucesso (200):**

```json
{
  "mensagem": "Senha atualizada com sucesso"
}
```

#### Atualizar Avatar

```
PUT /profile/avatar
```

Atualiza o avatar do usuário.

**Parâmetros do corpo (multipart/form-data):**

| Campo  | Tipo | Descrição         | Obrigatório |
| ------ | ---- | ----------------- | ----------- |
| avatar | file | Arquivo de imagem | Sim         |

**Resposta de sucesso (200):**

```json
{
  "avatar": "https://s3.amazonaws.com/avatars/joao_novo.png"
}
```

## Códigos de Status

| Código | Descrição                                  |
| ------ | ------------------------------------------ |
| 200    | Sucesso                                    |
| 201    | Recurso criado com sucesso                 |
| 204    | Operação concluída sem conteúdo de retorno |
| 400    | Requisição inválida ou malformada          |
| 401    | Não autenticado                            |
| 403    | Sem permissão para acessar o recurso       |
| 404    | Recurso não encontrado                     |
| 409    | Conflito (ex.: recurso vinculado)          |
| 422    | Erro de validação                          |
| 429    | Limite de requisições excedido             |
| 500    | Erro interno do servidor                   |

## Tratamento de Erros

Em caso de erro, a API retorna um objeto JSON com os seguintes campos:

```json
{
  "erro": true,
  "mensagem": "Descrição do erro",
  "detalhes": {}
}
```

## Paginação

Endpoints que retornam listas de recursos suportam paginação através dos parâmetros `pagina` e `limite`. A resposta incluirá os metadados de paginação:

```json
{
  "total": 100,
  "paginas": 5,
  "paginaAtual": 1,
  "items": [...]
}
```

## Versionamento

A API é versionada através do prefixo na URL base. A versão atual é v1.

## Limites de Requisição

A API implementa limites de taxa:

- 100 requisições por minuto por usuário.
- Após exceder, retorna 429 (Too Many Requests).

## Contato

Para suporte técnico ou dúvidas:

- Email: api@mrfinancas.com
- Documentação: https://docs.mrfinancas.com
