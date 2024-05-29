# ClinicWave Full Stack Microservices: Overview 9

## Entity Identification

1. Identified the following entities/subdomains for the application:
   - Audit
   - Role
   - Permission
   - User
   - User Type
   - Address
   - Document
   - Document Type
   - Notification
   - Notification Type
   - Token
   - Authentication
   - Department
   - Doctor
   - Doctor Schedule
   - Patient
   - Appointment

## Audit Entity

To handle auditing of entities, an abstract `Audit` class has been created, which extends the following properties:

  - `createdAt` (LocalDateTime, non-nullable, not updatable)
  - `createdBy` (String, non-nullable, not updatable, max length 20)
  - `updatedAt` (LocalDateTime, insertable = false)
  - `updatedBy` (String, insertable = false, max length 20)

The `Audit` class is annotated with `@MappedSuperclass` and `@EntityListeners(AuditingEntityListener.class)`, which enables automatic tracking of the creation and modification timestamps and the associated users.

## AuditorAwareImpl

The `AuditorAwareImpl` class implements the `AuditorAware` interface and provides the current auditor (user) information. It retrieves the user details from the `SecurityContextHolder` and returns the username if available, or a default value of "user" if the principal is not an instance of `UserDetails`.

The `AuditorAwareImpl` is registered as a Spring component with the name `auditorAwareImpl`.

## Enable Auditing in Main class

The `@EnableJpaAuditing(auditorAwareRef = "auditorAwareImpl")` annotation is added to the `Main` class, enabling the auditing feature and associating it with the `AuditorAwareImpl` component.

## Role Entity

1. Created a `Role` class implementing `Serializable`.
2. The `Role` entity extends the `Audit` class.
3. Added the following properties:
    - `id` (primary key, auto-generated)
    - `name` (unique, not nullable)
    - `permissionSet` (a set of `Permission` objects)
4. Below is the issue of maintaining a one-to-many relationship between `Role` and `Permission` in a microservices' architecture.

## Permission Entity

1. Created a `Permission` class implementing `Serializable`.
2. The `Permission` entity extends the `Audit` class.
3. Added the following properties:
   - `id` (primary key, auto-generated)
   - `name` (unique, not nullable)
4. Used Lombok annotations to generate boilerplate code (getters, setters, constructors, etc.).
5. Configured the entity name using `@Entity(name = "Permission")`.

## Issue with One-to-Many Relationship

When using a one-to-many relationship between `Role` and `Permission`, the database will automatically generate a join table to handle the mapping. This join table typically contains two columns: `roleId` and `permissionId`.

The problem arises because the `permissionId` column in the join table is treated as a unique identifier. This means that a particular permission can only be assigned to one role. If you try to assign the same permission to another role, the database will throw a non-unique identifier or foreign key constraint violation error.

For example, let's assume the following scenario:

1. Role 1 (e.g., Doctor) is assigned the "View" permission.
2. Role 2 (e.g., Admin) also needs to be assigned the "View" permission.

However, since the "View" permission is already assigned to Role 1, the database will not allow assigning the same "View" permission to Role 2 due to the uniqueness constraint on the `permissionId` column in the join table.

This limitation restricts the flexibility of assigning the same permission to multiple roles, which is often a requirement in real-world applications.

## Identified two solutions
1. Using a join table
    - One solution to the one-to-many relationship issue between `Role` and `Permission` is to use a join table with an additional column to store the mapping.
2. Creating a separate `RolePermission` entity to hold the mapping
    - Another solution is to create a separate `UserRolePermission` entity to handle the mapping between `UserRole` and `UserPermission`. This entity would act as a bridge between the two entities.

## Mapping Solution #1: Using a join table

To implement the "Using a join table" solution for the one-to-many relationship between `Role` and `Permission`, follow these steps:

1. **Create a New Join Table Entity**
    - Create a new entity class called `RolePermissionMapping`.
    - This class will store the mapping between roles and permissions.
    - It should extend the `Audit` class and implement `Serializable`.
    - It should have the following properties:
        - `id`: An `@EmbeddedId` of type `RolePermissionMappingId` (a composite primary key).
        - `role`: A `@ManyToOne` relationship with the `Role` entity, mapped by `id.roleId`.
        - `permission`: A `@ManyToOne` relationship with the `Permission` entity, mapped by `id.permissionId`.

2. **Create a Composite Primary Key Class**
    - Create an `@Embeddable` class called `RolePermissionMappingId` to serve as the composite primary key for the `RolePermissionMapping` entity.
    - This class should have the following properties:
        - `roleId`: A `Long` representing the role's ID.
        - `permissionId`: A `Long` representing the permission's ID.
    - Implement `Serializable` and include necessary constructors, getters, and setters.

3. **Update the `Role` Entity**
    - In the `Role` entity, replace the `Set<Permission> permissionSet` with `Set<RolePermissionMapping> rolePermissionMappings`.
    - Annotate the `rolePermissionMappings` with `@OneToMany(mappedBy = "role", cascade = CascadeType.ALL)`.

4. **No Changes Required for the `Permission` Entity**
    - The `Permission` entity remains unchanged.

5. **Configure the Join Table**
    - The join table `role_permission_mapping` will store the mapping between roles and permissions.
    - The composite primary key `RolePermissionMappingId` ensures that a role can be assigned multiple permissions, and a permission can be assigned to multiple roles.

6. **Assign Permissions to Roles**
    - To assign permissions to a role, create instances of `RolePermissionMapping` and add them to the `rolePermissionMappings` set in the `Role` entity.

By following these steps, you will have implemented the "Using a join table" solution, allowing roles to be assigned multiple permissions and permissions to be assigned to multiple roles without violating uniqueness constraints.

## Mapping Solution #2: RolePermission Entity

1. Decided to create a separate `RolePermission` entity to handle the mapping between `Role` and `Permission`.
2. `RolePermission` will have the following properties:
   - `id` (primary key, auto-generated)
   - `roleId` (foreign key to `Role`)
   - `permissionId` (foreign key to `Permission`)
3. This solution ensures that the same permission can be assigned to multiple roles without violating uniqueness constraints.

## Next Step

- Implement the `RolePermission` entity.