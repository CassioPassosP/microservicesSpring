# Delivery

Monorepo de um sistema de delivery baseado em microsservicos Spring Boot. O projeto esta organizado em aplicacoes independentes, cada uma com seu proprio `pom.xml`, wrapper Maven e ciclo de execucao.

> **Status atual:** a base dos servicos foi criada, mas o repositorio ainda nao possui endpoints REST, regras de negocio ou rotas configuradas no API Gateway. O README descreve o estado atual do codigo para facilitar a evolucao do projeto.

## Arquitetura

```text
Cliente
   |
   v
api-gateway
   |
   +--> auth-service
   +--> user-service
   +--> store-service
   +--> product-service
   +--> cart-service
   +--> cupon-service
   +--> order-service
```

### Servicos

| Servico | Responsabilidade planejada | Estado atual |
| --- | --- | --- |
| `api-gateway` | Ponto de entrada e roteamento das requisicoes | Spring Cloud Gateway adicionado; rotas ainda nao configuradas |
| `auth-service` | Autenticacao e seguranca | Spring Security, validacao e JPA adicionados; fluxo ainda nao implementado |
| `user-service` | Cadastro e gerenciamento de usuarios | JPA, PostgreSQL e OpenFeign adicionados |
| `store-service` | Lojas e estabelecimentos | JPA, PostgreSQL e OpenFeign adicionados |
| `product-service` | Produtos e catalogo | JPA, PostgreSQL e OpenFeign adicionados |
| `cart-service` | Carrinho de compras | JPA e PostgreSQL adicionados |
| `cupon-service` | Cupons e descontos | JPA e PostgreSQL adicionados |
| `order-service` | Pedidos e orquestracao do checkout | JPA, PostgreSQL e OpenFeign adicionados |

## Tecnologias

- Java 21
- Spring Boot 4.1.0
- Spring Cloud 2025.1.2 nos servicos que usam recursos Cloud
- Spring Web MVC e WebFlux no API Gateway
- Spring Data JPA
- Spring Security no `auth-service`
- Spring Cloud OpenFeign para comunicacao entre servicos
- PostgreSQL como banco de dados planejado
- Lombok
- Maven Wrapper

## Pre-requisitos

- JDK 21 configurado no `PATH`
- Git
- PostgreSQL, quando os servicos com JPA forem executados
- Maven nao e obrigatorio: cada modulo possui `mvnw` e `mvnw.cmd`

Confirme o Java instalado:

```bash
java -version
```

## Como executar

Cada servico deve ser executado a partir da sua propria pasta. Em Linux/macOS:

```bash
cd user-service
./mvnw spring-boot:run
```

No Windows PowerShell:

```powershell
Set-Location user-service
.\mvnw.cmd spring-boot:run
```

Para executar outro modulo, substitua `user-service` pelo nome da pasta correspondente:

```text
api-gateway
 auth-service
 cart-service
 cupon-service
 order-service
 product-service
 store-service
 user-service
```

Atualmente, somente o `user-service` declara uma porta customizada (`8082`). Os demais servicos usam a porta padrao do Spring Boot (`8080`) enquanto nao forem configurados com portas diferentes. Portanto, nao e possivel executar todos simultaneamente sem ajustar `server.port` em cada `application.properties`.

## Configuracao

Os arquivos de configuracao ficam em `src/main/resources/application.properties` dentro de cada servico.

Antes de executar os servicos que usam JPA/PostgreSQL, configure pelo menos:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/delivery
spring.datasource.username=postgres
spring.datasource.password=sua-senha
spring.jpa.hibernate.ddl-auto=update
```

Em um ambiente real, mantenha senhas e outros segredos fora do repositorio, usando variaveis de ambiente ou um gerenciador de segredos. A configuracao atual ainda nao define essas propriedades.

O `api-gateway` tambem precisa de rotas no arquivo de configuracao antes de encaminhar requisicoes para os servicos.

## Testes

Os modulos possuem testes basicos de carregamento do contexto. Execute os testes dentro do modulo desejado:

```bash
cd auth-service
./mvnw test
```

No Windows PowerShell:

```powershell
Set-Location auth-service
.\mvnw.cmd test
```

Para compilar sem executar os testes:

```bash
./mvnw clean package -DskipTests
```

## Estrutura do repositorio

```text
Delivery/
|-- api-gateway/
|-- auth-service/
|-- cart-service/
|-- cupon-service/
|-- order-service/
|-- product-service/
|-- store-service/
|-- user-service/
|-- .github/
|-- .gitignore
`-- README.md
```

Cada servico segue a estrutura padrao do Spring Boot:

```text
<servico>/
|-- pom.xml
|-- mvnw
|-- mvnw.cmd
`-- src/
    |-- main/
    |   |-- java/
    |   `-- resources/
    `-- test/
```

## Proximos passos

- Definir portas e configuracoes por ambiente.
- Criar os schemas e conexoes do PostgreSQL.
- Implementar entidades, repositorios, servicos e controllers.
- Configurar autenticacao, autorizacao e emissao/validacao de tokens.
- Definir as rotas do API Gateway.
- Implementar a comunicacao entre servicos com contratos bem definidos.
- Adicionar Docker Compose para subir a infraestrutura local.
- Expandir os testes unitarios e de integracao.

## Licenca

Nenhuma licenca foi definida neste repositorio ate o momento.
