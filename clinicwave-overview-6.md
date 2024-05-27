# ClinicWave Full Stack Microservices: Overview 6

## User Authentication Flow

### Identity Provider (IDP) Authentication Flow
- Using `auth0` as the IDP
- auth0 will have its own user store (database or other mechanism)
- UI will submit username and password to the `User Authentication Service`
- User Authentication Service will pass the credentials to auth0 with scope as `openid`
- auth0 will return a JSON response containing:
  - `access_token`
  - `id_token`
  - `refresh_token`
- Token expiry details
- UI will store these tokens and expiry details in `Redux store`

### Traditional Database Authentication Flow
- UI will submit username and password to `User Authentication Service`
- User Authentication Service will pass credentials to `centralized database`
- Database will return `JWT access token` and `ID token`
- UI will store tokens in `Redux store`

### Why Both Flows?
- To support `multi-tenancy` and `single sign-on (SSO)` features
- Multi-tenancy: Different organizations can use the application with different domains (e.g., `clinicwave.com`, `abott.com`)
- SSO: Once logged in, users can access other applications without re-authenticating

### Technical Stack
- Using `OAuth2` + `OpenID Connect` protocols
- OAuth2 provides `access token` (for authorization)
- OpenID Connect provides additional user information (roles, permissions, etc.) via `ID token`

## API Gateway Flow

### First Request
1. UI sends request with `access token` to `API Gateway`
2. API Gateway verifies token validity with auth0 (for first request)
3. If valid, API Gateway `caches the token`
4. API Gateway forwards the request to respective microservice (e.g., Storage Service) via `Eureka registry`
5. Microservice responds, and API Gateway sends the response back to UI

### Subsequent Requests
1. UI sends request with `access token` to `API Gateway`
2. API Gateway verifies the token against the `cached token`
3. If valid, API Gateway checks `token expiry`
4. If not expired, API Gateway forwards the request to respective microservice
5. Microservice responds, and API Gateway sends the response back to UI

### Why API Gateway?
- Microservices are deployed in a `private virtual network` (not publicly accessible)
- `API Gateway` is the only public-facing component
- It acts as a `reverse proxy` and enforces security by token validation

## Deployment Topology and Plan

### Ecosystem Setup (Week 1)
1. Set up the following ecosystem components as `Docker containers` in AWS EC2 instance:
- `Config Server`
- `Registry` (Eureka)
- `API Gateway`
- `Zipkin` (Distributed Tracing)
- `Resilience4j Dashboard`
- `ELK Stack` (Elasticsearch, Logstash, Kibana)
- `Grafana` (Monitoring Dashboard)

### Microservices Implementation (Week 2)
- Implement microservices using `Domain-Driven Design (DDD)` principles
- Set up `CI/CD pipeline` to build and deploy microservices as containers to AWS EC2

### Future Plans
- Discuss deployment plan in more detail
- Implement `DDD` and start the actual implementation