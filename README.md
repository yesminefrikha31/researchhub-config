# ResearchHub — Config Repository

Centralized configuration files for the microservices of the
[researchhub](https://github.com/YOUR-USERNAME/researchhub) project, served via
**Spring Cloud Config Server**.

## Why a separate repository?

The Config Server clones this repo at runtime (Git mode) to serve
configuration to each microservice that requests it on startup. Separating
config from code allows:
- configuration to evolve independently from the code release cycle
- avoiding re-cloning the entire application codebase on every config refresh
- reflecting the separation of concerns expected in a microservices
  environment (config managed separately from code)

## Structure

| File                              | Served to                                 |
|------------------------------------|--------------------------------------------|
| `application.properties`          | **All** services (shared config)          |
| `auth-service.properties`         | `auth-service` only                       |
| `encadrement-service.properties`  | `encadrement-service` only                |
| `api-gateway.properties`          | `api-gateway` only                        |

The file name matches exactly the `spring.application.name` value of the
consuming microservice.

## How a microservice fetches its config

Each microservice points to the Config Server in its own local
`application.properties`:

```properties
spring.application.name=auth-service
spring.config.import=optional:configserver:http://localhost:8888
```

On startup, `auth-service` fetches `application.properties` (shared config)
**then** `auth-service.properties` (service-specific config) from this repo,
through the Config Server.
