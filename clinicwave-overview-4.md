# ClinicWave Full Stack Microservices: Overview 4

### Business Analysis and Application Structure

#### Self Sign Up Module
- The first module to be implemented is the `Self Sign Up` module.
- Only `patients` or `guardians` of patients can perform self sign-up.
- `Doctors`, `administrators`, and other hospital staff will be registered by the administrator.

#### Application Setup and Super Admin
- During application startup, a `super admin` user will be created dynamically.
- The super admin user will have the ability to create other `administrator` users.
- The super admin will log in to the `dashboard` and manage administrators.

#### User Management Service
- A `User Management Service` will be implemented to handle user registration.
- This service will contain the following entities:
  - `User`
  - `Role`
  - `Permission`
- The `User` entity will have a `one-to-many` relationship with the `Role` entity.
- The `Role` entity will have a `one-to-many` relationship with the `Permission` entity.
- Complex relationships within a microservice boundary are allowed, but these entities cannot be shared across other microservices.

#### Database Selection
- For complex relationships, a `relational database` (e.g., `PostgreSQL`) will be used.
- The User Management Service will use a `PostgreSQL` database to store users, roles, and permissions.

#### User Registration Flow
- When registering a user (e.g., doctor, patient), the `User Management Service` will be called.
- The user's role will be assigned during registration, either through a `dropdown` or based on the module context.
- User profile documents (e.g., avatar, experience certificates) will be uploaded to an `AWS S3` bucket.
- The `S3 object URL` will be mapped to the corresponding user entity in the database.

#### S3 Bucket Structure
- A separate folder will be created in the `S3 bucket` for each user, using a unique identifier (e.g., `email`).
- User documents will be stored within their respective folder in the S3 bucket.

#### Separate Databases Consideration
- There is a consideration to have `separate databases` for different user types (e.g., doctors, patients).
- This would avoid a single centralized database from becoming overloaded.
- However, this approach may impact centralized authentication and `role-based access control (RBAC)`.

#### Role-Based Access Control (RBAC) and Attribute-Based Access Control (ABAC)
- `RBAC` will be implemented to display the dashboard based on user roles.
- `ABAC` will be used to control access to specific modules or components within the dashboard.
- `Spring Security` will be utilized for implementing RBAC and ABAC.
- `Custom permission evaluators` will be developed to handle fine-grained access control.

#### Alert Boxes
- `Alert boxes` will be displayed to users based on their access permissions.
- If a user attempts to access a module or component they are not authorized for, an alert box will be shown.

#### Authentication and Authorization
- An `Authentication Service` will be implemented, likely utilizing an `Identity Provider`.
- The Authentication Service will provide `tokens` to client applications.
- The `API Gateway` will extract and validate tokens from incoming requests, allowing or denying access accordingly.

## User Authentication and Authorization Flow

### Authentication Service
- The Authentication Service will be implemented `outside` the API Gateway as a `separate microservice`.
- It will be configured in a `private network/VPC`, but with a `public IP address` for external access.
- The UI applications will call the Authentication Service to get the `JWT token`.

### User Authentication Flow
1. The user enters their `username` and `password` on the login screen of the UI.
2. The UI's login service component calls the Authentication Service with the provided credentials.
3. The Authentication Service verifies the credentials against the `centralized authentication database`.
4. If the credentials are valid, the Authentication Service fetches the user's `roles` and `permissions`.
5. The Authentication Service generates a `JWT token` with the user's information, token validity, creation time, and expiry time.
6. The token is stored in a `Redis cache` along with the user's details.
7. The token is sent back to the UI, which stores it in the `token store context`.

### Token Usage
- For subsequent requests from the UI, the token is retrieved from the `token store context` and appended to the request headers as a `Bearer token`.
- The API Gateway verifies the token's validity by checking the `centralized authentication database system` (Redis cache).
- If the token is valid, the request is forwarded to the respective microservice.

## User Session Management
- For auditing purposes, a separate `user session database` (potentially `MongoDB`) will be implemented to store user session information.
- This database will keep track of user `login/logout times`, `session durations`, and other relevant details.
- The user session data will be used to generate reports and visualizations using `Grafana` or other monitoring tools.

## Flexibility and Extensibility
- Separating the Authentication Service from the API Gateway allows for easy integration with `external Identity Providers (IDPs)` in the future.
- Instead of modifying the API Gateway, only the Authentication Service needs to be updated to connect with the desired IDP.
- This separation of concerns follows the `single responsibility principle` and promotes maintainability.

The Authentication Service sits `outside` the API Gateway and has a `public IP` for external access. The UI calls the Authentication Service to get the `JWT token`, which is then used for subsequent requests through the API Gateway. The token is validated against the `centralized authentication database` (Redis cache) by the API Gateway before forwarding requests to the respective microservices.