# ClinicWave Full Stack Microservices: Overview 3

## Configuration Server and Config Bus
- The configuration server will have the `config bus`
- The `config bus` will connect to the message queue `RabbitMQ`
- `RabbitMQ` will be used as the broker, and a cloud instance will be used for production deployments (following the 12 factor principles)

## Service Registry and Service Discovery
- `Eureka` will be used as the service registry due to its self-healing capability
- Microservices will register themselves with the `Eureka` service registry
- Service discovery will be enabled, allowing microservices to communicate with each other using `service IDs` instead of hardcoded host and port numbers
- This avoids tight coupling and resolves the host and port dependency issues

## API Gateway
- `Spring Cloud API Gateway` will be used for two reasons:
1. Secure the microservices and avoid exposing them directly to client applications
2. Route incoming requests to the underlying microservices with the help of the registry

### API Gateway Flow
1. The `API Gateway` will be a client to the `Eureka` registry
2. The registry will route calls to the underlying microservices via a load balancer (`Ribbon`)
3. The `API Gateway`'s responsibilities:
- Abstract the microservices from the client applications
- Handle security

#### Security Flow
1. Client applications will call the `Authentication Service`
2. The `Authentication Service` will get a token from the `Identity Provider`
3. The `Authentication Service` will provide the token to the client applications
4. Client applications will embed the token in subsequent calls to the `API Gateway`
5. The `API Gateway` will extract the token from the request header and validate it
- If the token is valid, the request will be allowed to proceed to the underlying microservices via the load balancer
- If the token is invalid, the `API Gateway` will reject the request and send a `403 Unauthorized` response

#### Token Caching
- The `API Gateway` will cache valid tokens in a distributed cache (`Redis`) to allow microservices to communicate with each other
- Microservices will fetch the token from `Redis`, embed it in the request, and call the `API Gateway`
- This solves the issue of microservices not having access to the token directly

## Circuit Breaker
- A circuit breaker pattern will be implemented to handle failures
- If a certain number of failures occur (e.g., 5 payment failures), the circuit should open, preventing further requests until the issue is resolved
- The `Resilience4j` dashboard will be used to monitor the health and communication status of microservices

## Resilience Patterns
- Resilience patterns like rate limiting, bulkhead, and cost per service will be implemented using `Resilience4j`

## Monitoring and Tracing
- `Micrometer` will be enabled on each microservice for monitoring purposes, with `Prometheus` as the monitoring system
- `Grafana` will be used to visualize the monitoring data
- `Zipkin` will be used for distributed tracing to understand the flow of calls between microservices

## Distributed Tracing with Zipkin
- `Zipkin` will be used for distributed tracing to understand the flow of calls between microservices
- It will help visualize the communication flow, including when a request started, where it ended, and how the response was given back
- Zipkin will provide a UI to view the communication flows and trace the communication steps of each microservice

### Monitoring Flow
1. Each microservice will have `Micrometer` and `Actuator` dependencies
2. `Micrometer` will expose metrics data
3. `Prometheus` will periodically pull metrics data from Micrometer over HTTP
4. `Grafana` will continuously pull data from Prometheus and display it in a UI