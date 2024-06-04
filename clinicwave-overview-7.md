# ClinicWave Full Stack Microservices: Overview 7

## Deployment Topology and Architecture

### Core Services

1. `User Management Service`
2. `User Authentication Service`
3. `Document Service`
4. `Notification Service`
5. `Admin Service`
6. `Department Service`
7. `Address Service`
8. `Doctor Service`
9. `Doctor Schedule Service`
10. `Patient Service`
11. `Appointment Service`

### Ecosystem Services

1. `IDP Setup`
2. `Config Server Setup`
3. `Eureka with Cluster Setup`
4. `Git Repository Setup`
5. `Spring Cloud API Gateway with Security Constraint Validation`
6. `ELK (Elastic Search, Logstash, Kibana) Setup`
7. `Zipkin Setup`
8. `Prometheus Setup`
9. `Grafana Setup`

### Databases and Storage

1. `PostgreSQL` (Docker Image)
2. `MongoDB` (Docker Image)
3. `Keystone` (Object Store) - Optional
4. `H2` (In-Memory Database)
5. `Amazon S3` (AWS Managed Object Store)

### Infrastructure (Dev Environment)

1. `AWS EC2` (Compute Engine) - 2 vCPU, 2 GB RAM
2. `AWS Application Load Balancer`
3. `Docker` (Container Engine)
4. `Minikube or KubeAdm` (Kubernetes Cluster)
5. `Jenkins` (CI/CD Orchestrator)
6. `Ansible` (Infrastructure Provisioner)
7. `Fargate` (AWS Managed Kubernetes Service) - Optional
8. `NGINX` (Web Server for Static Content)
9. `RabbitMQ` (Cloud Managed Instance for Messaging)
10. `Twilio` (API for SMS Messaging)
11. `SMTP` (Google SMTP for Email)

### Infrastructure (Prod Environment)

1. `GCP Kubernetes Engine (GKE)`
2. `Ingress` (Load Balancer or Reverse Proxy)

### Deployment Strategy

The deployment strategy will be discussed in the next session, followed by the start of Domain-Driven Design (DDD).