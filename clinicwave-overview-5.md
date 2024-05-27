# ClinicWave Full Stack Microservices: Overview 5

## User Management Service

- The first service to be implemented is the `User Management Service`.
- It will have a `Relational Database Management System (RDBMS)` database to store users, roles, and permissions.
- The User Management Service will handle operations like:
  - `User registration` (self-registration and add user)
  - `Change password`
  - `Reset password`
  - `Forget password`
  - `Unlock user`
  - `Enable user`
  - `Provisioning` (assigning roles)
  - `Deprovisioning` (removing roles)

### User Registration Flow

- `Self-registration`:
  - Users can register themselves from the UI.
  - By default, the role `"self-registration"` will be assigned to these users.
  - Self-registration users can only view their profile after logging in.
- `Add user`:
  - Users will be added by the administrator or super administrator.
  - The add user flow will involve the `Storage Service` and `Notification Service`.

#### Notification Service

- The `Notification Service` will use an `SMTP server` (e.g., Gmail) to send email notifications.
- For SMS notifications, the `Twilio API` will be used.
- Email notifications will be sent for user registration and password change.
- SMS notifications will be sent for appointment confirmations and payments.

#### Storage Service

- The `Storage Service` will store documents (e.g., certificates, PDFs) in a `NoSQL database (MongoDB)`.
- It will be called during the add user flow to store user documents.

#### User Verification Process

- After successful registration, a `confirmation code` will be generated and stored in a `Redis cache`.
- The confirmation code will be sent to the user's email along with a password change link.
- The code will be valid for `7 days`.
- After the user changes the password using the link, the confirmation code will be revoked from the cache, and the user's status will be updated to active.

## User Authentication Service

- This service will handle user authentication and token management using `Spring Security`.
- Operations:
  - `Authenticate user`
  - `Renew token`
  - `Validate token`
  - Retrieve user session details (roles, permissions, etc.)

### Token Management

- `JSON Web Tokens (JWT)` will be used for authentication and authorization.
- `OAuth 2.0` and `OpenID Connect` will be implemented.
- Three types of tokens will be used:
  - `ID Token` (JWT containing user information)
  - `Access Token`
  - `Refresh Token`
- Tokens will be stored in a `Token Store database` for analytics and reporting.
- A `Redis cache` will be used for storing active tokens.

### Spring Security Implementation

- Custom Spring Security filters, password generators, user details service, and permission evaluators will be implemented.
- In-depth Spring Security implementation will be covered.

## Next Steps

- Discussion on the `Department Service` and token exchange mechanisms.
- Introduction of the `Gateway service` for token management and filtering.
- Start with `Domain-Driven Design (DDD)`.
- Parallel implementation of the backend and frontend (UI) using `React.js`.
- Deployment on `AWS` initially, and later migration to `Google Cloud Platform (GCP)`.
