# Desafio Itau

**APIs**:
Accounts:
- Consulta Saldo e faz transferencia
Customers: (Cadastro Mock)
- Consulta dados dos clientes
Bacen:
- Notifica sobre as transacoes realizadas

| Method  | Path | Description |
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

### Spring Boot 3.2 Reactive WebFlux (Accounts)

Usage:
- IntentelliJ IDEA 2023.2
- Java 17+
- Liquibase Database Migrations
- PostgreSQL


## WebFlux
- [Function Handler Validation](https://docs.spring.io/spring-framework/reference/web/webflux-functional.html#webflux-fn-handler-validation)

## Rest Assured
- [Rest Assured usage](https://github.com/rest-assured/rest-assured/wiki/Usage)

## Resilience4j
- [circuitbreaker](https://resilience4j.readme.io/docs/circuitbreaker)
- [Resilience4j Getting Started configuration](https://resilience4j.readme.io/docs/getting-started-3#configuration)
- [Resilience4j configuration](https://resilience4j.readme.io/docs/circuitbreaker#create-and-configure-a-circuitbreaker)

