# Projeto de Tolerância a Falhas

Este projeto demonstra a implementação de um sistema distribuído com microserviços que incorporam diferentes técnicas de tolerância a falhas, como circuit breakers, retries automáticos e falhas simuladas (omissões, erros e crashes). O objetivo é estudar e validar o comportamento de aplicações em cenários de falhas.

## Microserviços

O projeto é composto pelos seguintes microserviços:

1. **E-commerce**: Gerencia compras de produtos e interage com os outros microserviços.
2. **Exchange**: Retorna taxas de câmbio aleatórias.
3. **Fidelity**: Processa o sistema de bonificação do cliente.
4. **Store**: Gerencia os produtos e realiza vendas.

## Tecnologias Utilizadas

- **Java 17**
- **Spring Boot**
- **Docker** e **Docker Compose**
- **Maven**
- Banco de dados integrado ao microsserviço `Store` e ao `Fidelity` via JPA/Hibernate

## Requisitos

Antes de iniciar, certifique-se de ter:

- **Docker** e **Docker Compose** instalados
- Porta **8080** e sequentes livres no host
- Recurso mínimo de 4 GB de RAM no ambiente para rodar os containers

## Estrutura de Diretórios

```plaintext
/
|-- ecommerce/
|   |-- Dockerfile
|   |-- pom.xml
|   |-- src/
|
|-- exchange/
|   |-- Dockerfile
|   |-- pom.xml
|   |-- src/
|
|-- fidelity/
|   |-- Dockerfile
|   |-- pom.xml
|   |-- src/
|
|-- store/
|   |-- Dockerfile
|   |-- pom.xml
|   |-- src/
|
|-- docker-compose.yml
```

## Configuração e Execução

1. **Clone o repositório:**

   ```bash
   git clone <url-do-repositorio>
   cd <diretorio-do-projeto>
   ```

2. **Build e Start do Sistema:**

   Execute o comando abaixo para construir as imagens e iniciar os containers:

   ```bash
   docker-compose up --build
   ```

   O Docker Compose irá:
   - Construir cada microserviço a partir do respectivo Dockerfile
   - Publicar os containers nas seguintes portas:
     - E-commerce: [http://localhost:8080](http://localhost:8080)
     - Store: [http://localhost:8081](http://localhost:8081)
     - Exchange0: [http://localhost:8082](http://localhost:8082)
     - Exchange1: [http://localhost:8083](http://localhost:8083)
     - Fidelity: [http://localhost:8084](http://localhost:8084)

## Técnicas de Tolerância a Falhas

As seguintes técnicas de tolerância a falhas foram implementadas no projeto:

1. **Circuit Breaker**: Interrompe chamadas repetidas para serviços que falharam, evitando sobrecarga.
2. **Retries Automáticos**: Reenvio automático de requisições em caso de falha temporária.
3. **Fallbacks**: Define respostas padrão em casos de falhas.
4. **Simulação de Falhas**: Testa o comportamento do sistema em cenários como indisponibilidade, erros e crashes.

## Testando o Sistema

- **E-commerce:** Para realizar uma compra, envie uma requisição POST ao endpoint `/buy` com o seguinte corpo JSON:

  ```json
  {
    "idProduct": 1,
    "idUsuario": 2,
    "ft": false
  }
  ```

- **Exchange:** Consulte as taxas de câmbio acessando os endpoints `http://localhost:8082` ou `http://localhost:8083`.

### Exemplos de Requisições

#### Usando `curl`

- **E-commerce:**

  ```bash
  curl -X POST http://localhost:8080/buy -H "Content-Type: application/json" -d '{"idProduct" : 1, "idUsuario" : 2, "ft" : false}'
  ```

- **Store:**

  ```bash
  curl -X GET http://localhost:8081/products
  ```

## Logs e Monitoramento

- Para verificar os logs de cada container:

  ```bash
  docker-compose logs <nome-do-servico>
  ```

- Para acessar o container diretamente:

  ```bash
  docker exec -it <nome-do-container> bash
  ```