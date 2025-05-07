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

| Campo | Tipo   | Descrição                   |
| ----- | ------ | --------------------------- |
| nome  | string | Nome completo do usuário    |
| email | string | Email único do usuário      |
| senha | string | Senha (mínimo 8 caracteres) |

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

| Campo | Tipo   | Descrição        |
| ----- | ------ | ---------------- |
| email | string | Email do usuário |
| senha | string | Senha do usuário |

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

| Parâmetro  | Tipo                | Descrição                                 | Obrigatório      |
| ---------- | ------------------- | ----------------------------------------- | ---------------- |
| dataInicio | string (YYYY-MM-DD) | Data de início para filtrar               | Não              |
| dataFim    | string (YYYY-MM-DD) | Data de fim para filtrar                  | Não              |
| categoria  | string              | ID da categoria para filtrar              | Não              |
| tipo       | string              | Tipo de transação: "receita" ou "despesa" | Não              |
| pagina     | number              | Número da página para paginação           | Não (padrão: 1)  |
| limite     | number              | Quantidade de itens por página            | Não (padrão: 20) |

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
    // ... outras transações
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

| Campo       | Tipo                | Descrição              | Obrigatório |
| ----------- | ------------------- | ---------------------- | ----------- |
| descricao   | string              | Descrição da transação | Sim         |
| valor       | number              | Valor da transação     | Sim         |
| data        | string (YYYY-MM-DD) | Data da transação      | Sim         |
| tipo        | string              | "receita" ou "despesa" | Sim         |
| categoriaId | string              | ID da categoria        | Sim         |
| contaId     | string              | ID da conta            | Sim         |
| observacao  | string              | Observações adicionais | Não         |

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

Remove uma transação existente.

**Parâmetros de caminho:**

| Parâmetro | Descrição       |
| --------- | --------------- |
| id        | ID da transação |

**Resposta de sucesso (204):** Sem conteúdo

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
  // ... outras categorias
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
  // ... outras contas
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

### Relatórios

#### Resumo por Período

```
GET /relatorios/resumo
```

Retorna um resumo financeiro do período especificado.

**Parâmetros de consulta (Query):**

| Parâmetro  | Tipo                | Descrição      | Obrigatório |
| ---------- | ------------------- | -------------- | ----------- |
| dataInicio | string (YYYY-MM-DD) | Data de início | Sim         |
| dataFim    | string (YYYY-MM-DD) | Data de fim    | Sim         |

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
    // ... outras categorias
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
    // ... outras categorias
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
  // ... outros meses
]
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
| 422    | Erro de validação                          |
| 500    | Erro interno do servidor                   |

## Tratamento de Erros

Em caso de erro, a API retorna um objeto JSON com os seguintes campos:

```json
{
  "erro": true,
  "mensagem": "Descrição do erro",
  "detalhes": {} // Detalhes adicionais do erro, quando disponíveis
}
```

## Paginação

Endpoints que retornam listas de recursos suportam paginação através dos parâmetros de consulta `pagina` e `limite`. A resposta incluirá os metadados de paginação:

```json
{
  "total": 100,      // Total de registros
  "paginas": 5,      // Total de páginas
  "paginaAtual": 1,  // Página atual
  "items": [...]     // Itens da página atual
}
```

## Versionamento

A API é versionada através do prefixo na URL base. A versão atual é v1.

## Limites de Requisição

A API implementa limites de taxa para proteger os recursos do servidor:

- 100 requisições por minuto por usuário
- Após exceder o limite, será retornado o código de status 429 (Too Many Requests)

## Contato

Para suporte técnico ou dúvidas sobre a API, entre em contato:

- Email: api@mrfinancas.com
- Documentação detalhada: https://docs.mrfinancas.com
