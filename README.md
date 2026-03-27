# Pizzaria Service (Portfolio)

RESTful API for order management built with **Spring Boot** and **PostgreSQL**. The system is designed around an **event-driven architecture** using **RabbitMQ** to support asynchronous processing, better responsiveness, and scalability.

## Features (High Level)

- Designed for order management via RESTful endpoints
- Persistence with PostgreSQL using Spring Data JPA
- Asynchronous workflows with RabbitMQ (event-driven processing)
- Automatic schema updates for development with `spring.jpa.hibernate.ddl-auto=update`

## Tech Stack

- Java 21
- Spring Boot 3.2.x
  - `spring-boot-starter-web` (REST API)
  - `spring-boot-starter-data-jpa` (JPA + Hibernate)
  - `spring-boot-starter-amqp` (RabbitMQ)
- PostgreSQL
- Maven (via `mvnw`)

## Architecture Overview

1. A client sends a request to the REST API to create or update order data.
2. The application persists relevant information in PostgreSQL.
3. Key domain events (e.g., "OrderCreated") are published to RabbitMQ.
4. Consumers process those events asynchronously (e.g., validation, fulfillment, notifications), decoupling API responsiveness from background work.

## Requirements

- Java 21
- Maven Wrapper (`./mvnw`)
- PostgreSQL running locally (or reachable from the app)
- RabbitMQ running locally (or reachable from the app)

## Configuration

The project uses the following database settings in `src/main/resources/application.properties`:

- `spring.datasource.url=jdbc:postgresql://localhost:5432/pizzaria_db`
- `spring.datasource.username=${DB_USER}`
- `spring.datasource.password=${DB_PASSWORD}`

Set environment variables before starting the app:

- `DB_USER`
- `DB_PASSWORD`

RabbitMQ connection properties can be added to `application.properties` as needed. For local development, defaults (like `localhost` with standard ports) are typically sufficient.

## How to Run

1. Ensure PostgreSQL is available and the database exists:
   - Database: `pizzaria_db`
2. Ensure RabbitMQ is available (local defaults are usually enough for development).
3. Export environment variables:

   ```bash
   export DB_USER="your_user"
   export DB_PASSWORD="your_password"
   ```

4. Start the application:

   ```bash
   ./mvnw spring-boot:run
   ```

5. The REST API will start on the default Spring Boot port (`http://localhost:8080`).

## Testing

Run unit/integration tests with Maven:

```bash
./mvnw test
```

## Notes 

- The project structure is ready for a full order domain (controllers, services, repositories) and RabbitMQ consumers/producers.
- The intent is to keep API endpoints fast by offloading non-critical work to asynchronous RabbitMQ-driven flows.

