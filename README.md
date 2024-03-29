# Desafio Itau

**APIs**:
Accounts:
- Consulta Saldo e faz transferencia
Customers: (Cadastro Mock)
- Consulta dados dos clientes
Bacen:
- Notifica sobre as transacoes realizadas

---
Recursos da API

| Method  | Path  | Description  |
| :-----: | :---- | :----------- |
| GET     | /api/v1/accounts/{customerId}/balance         | Consulta saldo atual em conta do cliente            |
| POST    | /api/v1/accounts/{customerId}/transfers       | Realiza transferencia entre contas                  |
| PATCH   | /api/v1/accounts/{customerId}/transfer-limits | (Opcional) Atualiza o limite de transferencia diaria do cliente|


## [Diagramas de Sequencia](./diagramas/README.md)

## [Arquitetura de Solucao AWS](./diagramas/solutions_architect/desafio_itau_drawio.png)

## Getting Started

### Ordem de exeucao

1. Docker Compose. [docker-compose.yml]
2. Projeto Contas. [mc-accounts]
3. Testes automatizados. [automated-tests]

### Containers:
- mountebank: Mocks/Stubs
- postgresql: Database

### Execute no diretorio que esta do docker-compose.yml
```shell
docker-compose up -d
```

### Registrando o impostor (Mock)
```shell
# Httpie
http --json POST :2525/imposters < mocks/imposters/accountsAndBacenStubs.json

# cURL
curl -i -X POST -H 'Content-Type: application/json' http://localhost:2525/imposters -d @mocks/imposters/accountsAndBacenStubs.json
```

> Com os containers executando e projeto Spring sendo executado segue as requests:

### Requests

### cURL

```shell
# GET Balance Alice
curl --request GET \
  --url http://localhost:8090/api/v1/accounts/0ab4165471c445918697d3d620b0db2a/balance \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: insomnia/8.6.1'

# GET Balance Bob
curl --request GET \
  --url http://localhost:8090/api/v1/accounts/0ab4165471c445918697d3d620b0db2b/balance \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: insomnia/8.6.1'

# POST Transfer Bob to Alice
curl --request POST \
  --url http://localhost:8090/api/v1/accounts/0ab4165471c445918697d3d620b0db2b/transfers \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: insomnia/8.6.1' \
  --data '{
  "toAccount": {
    "fullName": "Alice",
    "agency": 4321,
    "accountNumber": 856087
  },
  "amount": 100.00
}'

# POST Transfer Alice to Bob
curl --request POST \
  --url http://localhost:8090/api/v1/accounts/0ab4165471c445918697d3d620b0db2b/transfers \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: insomnia/8.6.1' \
  --data '{
  "toAccount": {
    "fullName": "Bob",
    "agency": 1234,
    "accountNumber": 1230019
  },
  "amount": 100.00
}'

# Update Transfer Limit BOB
curl --request PATCH \
  --url http://localhost:8090/api/v1/accounts/0ab4165471c445918697d3d620b0db2b/transfer-limits \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: insomnia/8.6.1' \
  --data '{
  "amount": 1000.00
}'

# Update Transfer Limit Alice
curl --request PATCH \
  --url http://localhost:8090/api/v1/accounts/0ab4165471c445918697d3d620b0db2a/transfer-limits \
  --header 'Content-Type: application/json' \
  --header 'User-Agent: insomnia/8.6.1' \
  --data '{
  "amount": 1000.00
}'
```


```shell
# Httpie
# GET Balance BOB
http --json GET :8090/api/v1/accounts/0ab4165471c445918697d3d620b0db2b/balance

# GET Balance Alice
http --json GET :8090/api/v1/accounts/0ab4165471c445918697d3d620b0db2a/balance

# Transferencia de Bob para Alice
http --json POST :8090/api/v1/accounts/0ab4165471c445918697d3d620b0db2b/transfers < mc-accounts/requestPayloads/transferFromBobToAlice.json

# Atualizar Limite diario da Alice p
http --json PATCH :8090/api/v1/accounts/0ab4165471c445918697d3d620b0db2a/transfer-limits < mc-accounts/requestPayloads/updateTransferLimit.json
```

### Spring Boot 3.2 Reactive WebFlux (Accounts)

#### Usage:
- IntentelliJ IDEA 2023.2
- Java 17+
- Liquibase Database Migrations
- PostgreSQL
- Design de desenvolvimento em Camadas: Controller, Service e repository

#### Localizacao no projeto WebFlux

> Routes: api.http.resources.AccountResource.class

> Handler/Controller: handlers.AccountHandler.class

> Service: services.impl.AccountServiceImpl.class

Exemplo:

![Resource/Controller](./images/resource.png)

## AWS
- [AWS ADOT Collector](https://aws-otel.github.io/docs/introduction)

## WebFlux
- [Function Handler Validation](https://docs.spring.io/spring-framework/reference/web/webflux-functional.html#webflux-fn-handler-validation)

## Rest Assured
- [Rest Assured usage](https://github.com/rest-assured/rest-assured/wiki/Usage)

## Resilience4j
- [circuitbreaker](https://resilience4j.readme.io/docs/circuitbreaker)
- [Resilience4j Getting Started configuration](https://resilience4j.readme.io/docs/getting-started-3#configuration)
- [Resilience4j configuration](https://resilience4j.readme.io/docs/circuitbreaker#create-and-configure-a-circuitbreaker)

