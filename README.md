# Spring PetClinic — DevOps

Spring Boot 4 sample application used as a DevOps training ground. Demonstrates containerisation, local database provisioning, and Kubernetes deployment.

---

## Stack

| Layer | Technology |
|---|---|
| Framework | Spring Boot 4.0, Spring MVC, Spring Data JPA |
| UI | Thymeleaf, Bootstrap 5, SCSS |
| Database | H2 (default), MySQL 9, PostgreSQL 18 |
| Build | Maven, Gradle |
| Container | Docker / Docker Compose |
| Orchestration | Kubernetes |

---

## Prerequisites

- Java 17+
- Maven **or** Gradle (wrappers included)
- Docker (for local DB or full compose stack)
- `kubectl` (for Kubernetes deployment)

---

## Run locally

### Default (H2 in-memory)

```bash
./mvnw spring-boot:run
# or
./gradlew bootRun
```

App available at `http://localhost:8080`.

### MySQL

```bash
docker compose up mysql -d
./mvnw spring-boot:run -Dspring-boot.run.profiles=mysql
```

### PostgreSQL

```bash
docker compose up postgres -d
./mvnw spring-boot:run -Dspring-boot.run.profiles=postgres
```

---

## Build

```bash
# Packaged JAR
./mvnw package

# Skip tests
./mvnw package -DskipTests
```

---

## Docker Compose

Starts MySQL (3306) and PostgreSQL (5432):

```bash
docker compose up -d
```

Credentials for both engines: `petclinic / petclinic`, database: `petclinic`.

---

## Kubernetes

Deploy a PostgreSQL instance and the PetClinic app:

```bash
kubectl apply -f k8s/db.yml
kubectl apply -f k8s/petclinic.yml
```

- Database: `demo-db` service (PostgreSQL 18, port 5432), credentials stored in a `Secret`.
- App: `petclinic` Deployment (image `dsyer/petclinic`, 1 replica), exposed via `NodePort` on port 80 → container 8080.
- Liveness / Readiness probes: `/livez` and `/readyz`.

---

## Configuration

| Profile | File |
|---|---|
| Default (H2) | `src/main/resources/application.properties` |
| MySQL | `src/main/resources/application-mysql.properties` |
| PostgreSQL | `src/main/resources/application-postgres.properties` |

Key properties:

```properties
database=h2   # switch to mysql or postgres
management.endpoints.web.exposure.include=*
spring.jpa.hibernate.ddl-auto=none
```

---

## Testing

```bash
./mvnw test
```

JMeter test plan available at `src/test/jmeter/petclinic_test_plan.jmx`.

---

## License

See [LICENSE.txt](LICENSE.txt).
