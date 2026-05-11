# Transação API

API REST desenvolvida com Java e Spring Boot para gerenciamento de transações financeiras e cálculo de estatísticas em tempo real com base nas transações realizadas nos últimos segundos.

A aplicação permite:

- Registrar transações
- Remover todas as transações armazenadas
- Consultar estatísticas das transações
- Filtrar estatísticas por intervalo de tempo

---

# Tecnologias Utilizadas

- Java
- Spring Boot
- Spring Web
- Spring Actuator
- Gradle
- Swagger / OpenAPI

---

# Objetivo da API

A API recebe transações contendo valor e data/hora de ocorrência.  
Com base nessas transações, é possível calcular estatísticas como:

- Soma total
- Média
- Quantidade de transações
- Maior valor
- Menor valor

As estatísticas são calculadas considerando um intervalo em segundos, sendo **60 segundos o valor padrão**.

Para geração das estatísticas foi utilizada a classe nativa do Java:

- `DoubleSummaryStatistics`

Essa abordagem permite realizar cálculos de forma eficiente e otimizada em memória.

Toda a persistência de dados da aplicação é realizada **em memória**, sem utilização de banco de dados.

---

# Endpoints

## Receber Transações

Endpoint responsável por registrar uma nova transação.

```http
POST /transacao
```

### Body da Requisição

```json
{
  "valor": 150.50,
  "dataHora": "2026-05-11T14:30:00-03:00"
}
```

### Parâmetros

| Campo | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| valor | BigDecimal | Sim | Valor da transação |
| dataHora | OffsetDateTime | Sim | Data e hora em que a transação ocorreu |

### Regras

- A transação não pode possuir data futura
- O valor da transação não pode ser negativo
- Apenas transações dentro do intervalo considerado serão utilizadas nas estatísticas

### Respostas

| Código | Descrição                        |
|---|----------------------------------|
| 201 | Transação registrada com sucesso |
| 422 | Dados não podem ser negativos    |
| 400 | Requisição mal formatada         |

---

## Limpar Transações

Remove todas as transações armazenadas em memória.

```http
DELETE /transacao
```

### Respostas

| Código | Descrição |
|---|---|
| 200 | Transações removidas com sucesso |

---

## Calcular Estatísticas

Retorna estatísticas das transações registradas.

```http
GET /estatistica
```

### Query Params

| Parâmetro | Tipo | Obrigatório | Descrição |
|---|---|---|---|
| intervaloSegundos | integer | Não | Intervalo em segundos utilizado no cálculo das estatísticas. Valor padrão: 60 |

### Exemplo

```http
GET /estatistica?120
```

### Exemplo de Resposta

```json
{
  "count": 10,
  "sum": 1500.50,
  "avg": 150.05,
  "min": 50.00,
  "max": 500.00
}
```

### Respostas

| Código | Descrição |
|---|---|
| 200 | Estatísticas calculadas com sucesso |
| 400 | Parâmetro inválido |

---

# Documentação Swagger

Após iniciar a aplicação, a documentação interativa poderá ser acessada em:

```bash
http://localhost:8080/swagger-ui/index.html
```

---

# Estrutura do Projeto

```bash
src
 └── main
     ├── java
     │    └── com.itau.transacao_api
     │         ├── business
     │         │    └── services
     │         ├── controller
     │         │    └── dtos
     │         └── infrastructure
     └── resources
```

---

