# Mountbank - over the wire test doubles

Execute no diretorio que esta do docker-compose.yml
```shell
docker-compose up -d mountebank
```

Registrando o impostor (Mock)
```shell
# Httpie
http --json POST :2525/imposters < mocks/imposters/accountsAndBacenStubs.json

# cURL
curl -i -X POST -H 'Content-Type: application/json' http://localhost:2525/imposters -d @mocks/imposters/accountsAndBacenStubs.json
```

# Referencia
- [Mountebank Getting Started](https://www.mbtest.org/docs/gettingStarted)