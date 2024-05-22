# MedSymphony Full Stack Microservices: Overview

## Overview

This session focuses on developing a cloud-native application using Spring Boot microservices, container management, and DevOps CI/CD processes. The application domain is a hospital administration system called MedSymphony.

## Project Overview

- **Domain**: Healthcare (Hospital Administration System)
- **Project Name**: MedSymphony
- **Deployment**: Heroku, Google Cloud Platform (GCP), AWS

## Application Features

- **User Roles**:
  - Super Admin: superuser with all permissions.
  - Administrator: Manages doctors, nurses, infrastructure, and patients.
  - Doctor: Can perform specific activities related to patient care.

## Technical Stack

### Backend

- **Framework**: Spring Boot 3.x
- **Architecture**: Microservices
- **Ecosystem**: Spring Cloud
- **Container Management**: Docker
- **Cloud Platforms**: AWS and GCP

### Frontend

- **Framework**: React 18.x
- **Features**: React Hooks, Redux

### Tools and Technologies

- **Source Code Repository**: GitHub
- **Development IDEs**: Visual Studio Code (UI), IntelliJ IDEA (Backend)
- **Database**: PostgreSQL, MongoDB
- **Build Tools**: Maven, Gradle
- **Deployment Services**:
  - AWS EC2 for AMI
  - AWS S3 for Blob Storage
  - AWS Elastic Beanstalk for microservices deployment
- **Testing**:
  - Unit Testing: JUnit 5.x
  - Mocking: Mockito
- **Logging**: SLF4J with TCP Socket Appender (for ELK stack integration)
- **Code Analysis**: PMD, Emma, SonarQube

## Tools for Development and Maintenance

- **Version Control**: Git, GitHub
- **CI/CD**: Jenkins
- **Infrastructure Provisioning**: Ansible
- **Infrastructure**: GCP, AWS, Docker, Kubernetes
- **Observability**: ELK Stack, Prometheus, Grafana, Zipkin
- **Messaging**: RabbitMQ, Kafka
- **SFTP Client**: WinSCP, MobaXterm (with PuTTY)
- **Testing Tools**: Postman (API Testing), Swagger UI/OpenAPI 3 (Documentation)

## Microservices Architecture

- **Principles**:
  - Each microservice has its own source code repository and database.
  - Microservices are deployed as Docker containers.
  - Following the 12-factor principles for microservices development.

## Logging and Monitoring

- **Distributed Log Management**: ELK Stack (Elasticsearch, Logstash, Kibana)
- **Monitoring**: Grafana

## Patterns and Methodologies

- **Domain-Driven Design (DDD)**:
  - Identify business capabilities and convert them into domains.
  - Shared components across microservices when necessary.
- **Behavior-Driven Development (BDD)**
- **Test-Driven Development (TDD)**

## Key Components and Integration

- **Service Registry**: Eureka Config Server
- **API Gateway**: Spring Cloud API Gateway
- **Resilience**: Resilience4j (for API rate limiting and thresholds)
- **Distributed Tracing**: Micrometer, Zipkin

## Frontend Development

- **Prerequisites**: HTML, CSS, JavaScript, Bootstrap
- **Development Focus**:
  - React application development with dedicated branches for UI code.
  - CI/CD integration for automatic deployment.

## Domain-Driven Development (DDD)

- **Timeframe**: Approximately 4 days (1 week) for implementation.
- **Challenges**:
  1. **Handling Foreign Key Associations**: How to manage relations between entities.
  2. **Defining Domain Boundaries**: Identifying clear boundaries for each domain.

## Infrastructure Setup

- **CI/CD Orchestrator**: Set up on an AWS machine.
- **Git Repositories**: Create for each microservice.
- **S3 Storage**: Utilize for various storage needs.
- **Docker**: Install and configure for container management.
- **Cluster Setup**: Potential future task; for now, focus is on container management.

## Cloud Native Applications

**Steps to Implement**:
  - **Microservices Development**.
  - **Container Management**.
  - **DevOps CI/CD Process**.
  - **Continuous Delivery to Cloud Platforms**.
  - **12-Factor Principles**: Follow these guidelines for cloud-native application development.

## Kubernetes Architecture

- **Deployment**: Use Google Cloud Platform for Kubernetes cluster setup.
- **Objective**: Deploy microservices into Kubernetes clusters.

## Development Patterns

1. **Decomposition Pattern**: Start with domain-driven development.
2. **Integration Patterns**:
   - **API Gateway**: Secure microservices and expose them through a public API gateway.
   - **Load Balancer**: Use AWS Application Load Balancer to manage traffic.
   - **Token Validation**: Implement filters in the API Gateway for token validation.
3. **Aggregator Pattern**:
   - **Use Case**: Managing patient appointments and payment transactions.
   - **Event-Driven Communication**: Ensures distributed transaction management and rollback if necessary.
4. **Database Patterns**:
   - **Shared Database for Services**: Apply selectively for certain microservices.
5. **Event-Driven Patterns**:
   - **CQRS and Saga**: Implement for managing distributed transactions.
6. **Observability Patterns**:
   - **Log Aggregation**: Use ELK stack (Elasticsearch, Logstash, Kibana).
   - **Performance Metrics**: Use Prometheus and Grafana.
   - **Distributed Tracing**: Implement Zipkin for tracing microservice communication.
7. **Cross-Cutting Concerns**:
   - **External Configuration**: Use Config Server with Spring Cloud Bus.
   - **Service Discovery**: Use Eureka for self-healing and preservation of microservices.
   - **Resilience Patterns**: Implement patterns like Circuit Breaker and Bulkhead using Resilience4j.

## Containerization and Deployment

- **Docker Containers**: All components (e.g., ELK stack, Config Server, Prometheus) should be containerized.
- **Kubernetes Deployment**: Deploy dockerized containers into Kubernetes clusters.

## Implementation Plan

1. **Microservices Development**
2. **Integration with Ecosystem**:
   - Set up Config Server, Registry, API Gateway, Observability tools, etc.
3. **Containerization**: Dockerize microservices and ecosystem components.
4. **Kubernetes Deployment**: Deploy everything into Kubernetes clusters.

## Additional Details

- **Prerequisites**:
- **Backend**: Java 8, Spring Core, Spring MVC, Spring Boot (optional), and Microservices (optional).
- **Frontend**: Knowledge of React.js (optional).
- **Cloud Services**: Use mostly free-tier services on AWS.