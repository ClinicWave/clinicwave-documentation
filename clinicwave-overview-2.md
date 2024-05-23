# ClinicWave Full Stack Microservices: Overview 2

## Build Tool
- Either Maven 3 or Gradle will be used as the build tool.
- A couple of microservices will use Maven, and a couple will use Gradle.

## Domain/Entity Implementation
- Start with Domain-Driven Design (DDD) to compose business scenario-specific domains or entities.
- Domains/entities will be composed based on the business requirements provided by the client.
- Use JPA to implement the domains/entities.
- Example: For a self sign-up feature, the User Management Service will need to implement a domain/entity containing fields like firstName, lastName, email, userID, mobileNumber, userState (locked/unlocked), and auditing fields (createdAt, modifiedAt, etc.).
- Auditing should be implemented as a common, generic component for all domains/entities.

## Data Access Layer (DAO/Repository)
- Implement the DAO layer or repository layer using Spring Data JPA.
- Spring Data JPA provides lightweight repositories with enterprise capabilities like searching, sorting, pagination, etc.

## Unit Testing
- Write unit test cases for the DAO/repository layer using JUnit.
- Use Mockito for mocking dependencies during unit testing to avoid tampering with the original data store.

## Service Layer
- Implement the service layer, which will have a dependency on the DAO/repository layer.
- The service layer will handle data conversions (between DTOs/POJOs and entities), exception translations, and transaction management.
- Use Spring's transaction management in the service layer.

## Properties File
- Configure the data source properties (driver class name, URL, username, password, etc.) in a properties file.

## Controller Layer
- Implement the controller layer using Spring REST.
- Bind user-submitted data to request beans.
- Implement JSR 303 validations in the controller layer.
- If validation fails, return user-friendly error messages.
- If validation passes, submit data to the service layer for transaction management.
- Implement resilience patterns (Circuit Breaker, Rate Limiting, Bulkhead, etc.) using Resilience4j.
- Document APIs using Swagger/OpenAPI 3.

## Logging, Exception Handling, and Integration Testing
- Implement logging using SLF4J with TCP Socket Appender across all layers.
- Implement global exception handling in the controller layer.
- Perform integration testing using tools like REST Assured, RestTemplate, or WebClient.

## Pushing Code to Source Repository
- Push the entire codebase to the source code repository (e.g., GitHub).
- Follow the 12-factor app principles.

## 12-Factor App Principles
- Every microservice should have its own dedicated source code repository (single codebase).
- Developers will write the Dockerfile and Docker Compose file for each microservice.
- After development, the code will be pushed to the Git repository (e.g., GitHub).

## Next Steps

### Defining Bounded Contexts
- Use bounded contexts to define the boundaries for each microservice based on business capabilities.
- This is the decomposition pattern in microservices.

### Implementing Multiple Microservices
- Implement multiple microservices following the same pattern (e.g., for doctors, patients, departments, etc.).

### Continuous Integration and Deployment
- Push the code to the source code repository (e.g., GitHub).
- Set up a CI/CD orchestrator (e.g., Jenkins) on an AWS machine.
- Create Git repositories for each microservice.
- Use Jenkins to build the source code, create Docker images, and deploy to EC2 instances using Ansible.
- Ansible will move the Docker images to the target EC2 instances and run them as containers.

### Microservices Ecosystem
- Set up the microservices ecosystem components:
- Config Server
- Service Registry (Eureka)
- API Gateway
- Observability tools (ELK Stack, Prometheus, Grafana, Zipkin)
- Set up the distributed configuration management system (Config Server) along with Spring Cloud Bus.
- Use RabbitMQ as the message broker for the Pub/Sub model.
- Implement the Config Server and RabbitMQ as Docker containers.
- Set up the Service Registry (Eureka) in replication mode.
- Implement the API Gateway for securing microservices and exposing a public API.
- Use an Identity Provider (IDP) like Auth0 or Keycloak for authentication and authorization.
- Auth0 is a cloud-hosted solution with a lifetime trial (limited resources).
- Keycloak is an open-source, on-premises solution implemented in Spring Boot.
- Use a load balancer (e.g., AWS Application Load Balancer) for traffic management.

### Integration Patterns
- Implement integration patterns:
  - API Gateway for securing microservices and exposing a public API.
  - Load Balancer (e.g., AWS Application Load Balancer) for traffic management.
  - Token Validation in the API Gateway.

### Other Patterns
- Aggregator Pattern for managing patient appointments and payment transactions using event-driven communication.
- Database Patterns: Shared database for certain microservices.
- Event-Driven Patterns: CQRS and Saga for managing distributed transactions.
- Observability Patterns: Log Aggregation (ELK Stack), Performance Metrics (Prometheus, Grafana), and Distributed Tracing (Zipkin).
- Cross-Cutting Concerns: External Configuration (Config Server, Spring Cloud Bus), Service Discovery (Eureka), Resilience Patterns (Circuit Breaker, Bulkhead with Resilience4j).

### Containerization and Deployment
- Containerize all components (microservices, ELK Stack, Config Server, Prometheus, etc.) using Docker.
- Deploy the containerized components into Kubernetes clusters.

### Implementation Plan
1. Develop microservices from scratch.
2. Integrate microservices with the ecosystem components (Config Server, Registry, API Gateway, Observability tools, etc.).
3. Containerize the microservices and ecosystem components using Docker.
4. Deploy everything into Kubernetes clusters.

## Observability and Monitoring
- Use ELK Stack (Elasticsearch, Logstash, Kibana) for distributed log management.
- Implement Prometheus and Grafana for monitoring and performance metrics.
- Integrate each microservice with Prometheus for exposing metrics.
- Grafana will fetch metrics from the Prometheus endpoints of all microservices.

## Caching
- Redis will be used for caching purposes (specific use case not mentioned).

## Databases
- Three databases will be used:
  1. H2 (in-memory database)
  2. PostgreSQL
  3. MongoDB

## Next Steps
- Explanation of security aspects (how microservices are secured, intercommunication).
- Discussion of caching with Redis.
- Deployment topology and strategies.
- Load balancing details (API Gateway, AWS Application Load Balancer).