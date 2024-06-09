# ClinicWave Full Stack Microservices: Overview 10

## Domain-Driven Design (DDD) and One-to-Many Relationships

## Domain Entities
### Permission Entity
- Represents permissions like `view` or `edit`.
- Contains fields:
  - `id` (auto-generated primary key)
  - `permissionName` (e.g., "view", "edit")

### Role Entity
- Represents roles like `admin` or `user`.
- Contains fields:
  - `id` (auto-generated primary key)
  - `roleName` (e.g., "admin", "user")

### RolePermission Entity (Join Table)
- A join table to map the many-to-many relationship between `Role` and `Permission`.
- Contains fields:
  - `id` (auto-generated sequence ID)
  - `Role` (a reference to the `Role` entity)
  - `Permission` (a reference to the `Permission` entity)

## One-to-Many Relationship Issue
- The issue with the default implementation of the one-to-many relationship in JPA:
  - When the `@OneToMany` annotation is used in the `Role` entity, JPA generates a join table that considers the `Permission` ID as unique.
  - This means the same permission cannot be assigned to multiple roles, which is an issue in real-world scenarios.

### Demonstration
1. The application was started, and the H2 in-memory database console was accessed.
2. Instances of `Permission` (e.g., "view", "edit") and `Role` (e.g., "admin", "user") were created.
3. An attempt was made to assign the same permission (e.g., "view") to multiple roles (e.g., "admin" and "user").
4. A "unique constraint violation" error was encountered, as the generated join table treated the `Permission` ID as unique.

## Solution: Custom Join Table
- To solve the issue, a custom join table `RolePermission` was created with the following columns:
  - `id` (auto-generated sequence ID)
  - `role_id` (foreign key to `Role`)
  - `permission_id` (foreign key to `Permission`)
- By introducing the `id` column, the uniqueness constraint is shifted from the `permission_id` to the `id` column.
- This allows the same permission to be assigned to multiple roles.

### Code Changes
1. The `RolePermission` entity was created to represent the custom join table.
2. The `@OneToMany` annotation was removed from the `Role` entity.
3. A `@OneToMany(mappedBy = "Role")` annotation was added to the `Permission` entity, delegating the relationship management to the `RolePermission` entity.
4. In the application startup code, instances of `Role`, `Permission`, and `RolePermission` were created and persisted to demonstrate the solution.

### Demonstration
1. The H2 in-memory database console was accessed again.
2. Instances of `Permission` (e.g., "view", "edit") and `Role` (e.g., "admin", "user") were created.
3. The same permission (e.g., "view") was assigned to multiple roles (e.g., "admin" and "user") using the `RolePermission` entity.
4. It was verified that the same permission could be assigned to multiple roles without any unique constraint violation errors.

## ClinicWaveUser Entity
- The `ClinicWaveUser` entity was created, which will have a one-to-one relationship with the `Role`.
- The `ClinicWaveUser` entity extends the `Audit` entity to inherit auditing columns like `createdAt`, `createdBy`, `updatedAt`, and `updatedBy`.
- The `ClinicWaveUser` entity consists of the following basic profile information:
  - `id` (auto-generated primary key)
  - `firstName`
  - `lastName`
  - `username`
  - `email`
  - `dateOfBirth` (of type `LocalDate`)
  - `gender` (an enum of type `Gender` with values `MALE` and `FEMALE`)
  - `bio`
  - `status` (boolean)
  - `role` (a one-to-one relationship with the `Role` entity)
- The `password` field is not included in the `ClinicWaveUser` entity, as passwords will be generated through a password creation link sent to the registered email address.

## Future Plans
- In the next step, the `Address` entity will be created, and a one-to-many relationship between `ClinicWaveUser` and `Address` will be established.
- The `Role` will be assigned to the `ClinicWaveUser` either by default or by an administrator during user creation.

## Conclusion
- This development focused on understanding and resolving the one-to-many relationship issue using a custom join table.
- The solution was demonstrated by creating the necessary entities and persisting sample data.
- The `ClinicWaveUser` entity was created, extending the `Audit` entity and using an enum for the `gender` field.
- The next step is to complete the `ClinicWaveUser` entity and establish its relationships with other entities like `Address` and `Role`.
- The importance of following the DDD approach and understanding the bounded context concept was emphasized for the upcoming development.