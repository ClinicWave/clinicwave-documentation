# ClinicWave Full Stack Microservices: Overview 11

## User Management and Access Control

### Role Management and User Management Completed
- In the previous session, role management and user management were completed.

### Address Domain
- The address domain was taken up in this session.
- An `Address` entity was created with fields like addressLine, country, city, state, province, postal code.
- A one-to-one relationship was established between `User` and `Address` entities, where each user will have only one postal address.

### User Type Entity
- A new entity `UserType` was created to represent the type of user (admin, doctor, patient, etc.).
- `UserType` will have a one-to-one relationship with the `User` entity.

### Document Entity and Document Type
- The `Document` entity was introduced, which will have fields like id, user ID, title, description, path, and, documentKey.
- Before implementing the `Document` entity, authentication entities were covered.

### Authentication Entities

#### ClinicWaveAuthentication Entity
- A new entity `ClinicWaveAuthentication` was created with fields like username and password.
- This entity represents the authentication information needed for user login.

#### Token Entity
- A new entity `Token` was created, extending `Audit`.
- This entity will store information related to the authentication token, such as access token, ID token, refresh token, token type, and expiry time.

#### TokenAudit Entity
- A new entity `TokenAudit` was created to store audit information related to the authentication token.
- It will have fields like store (IDP or DB), token refresh time, old access token, new access token, and subject (user for whom the token was generated).
- `Token` will have a one-to-one relationship with `TokenAudit`.

## Key Points

1. **Authentication Flow**
   - User will be created by the admin
   - Email will be triggered with a confirmation link
   - User clicks on the confirmation link to activate the account
   - User enters password and confirms it
   - `ClinicWaveAuthentication` table is updated with username and password

2. **User Status**
   - User status flags like `accountLocked`, `accountEnabled`, `accountDisabled` need to be maintained
   - These flags will be checked during authentication to allow/deny login

3. **Token Management**
   - Access token, ID token, and refresh token will be generated and stored in the `Token` table
   - Refresh token will be used to extend the lifetime of the access token or generate a new one (policy decision pending)
   - `TokenAudit` table will store details about token refreshes, old/new tokens, and the authentication store used
