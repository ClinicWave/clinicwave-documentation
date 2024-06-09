# Detailed Notes: Spring Boot Full Stack Microservices Training - Session 12

## Domain Modeling

### User Authentication and User Management

- Completed the user authentication and user management domains in the previous session.
- Implemented user profile, token, auditing information, address, and related entities.

## Document Service

- Designed the `Document` entity with the following fields:
  - `id`: Unique identifier for the document (auto-generated).
  - `title`: Title of the document (non-nullable).
  - `description`: Description of the document.
  - `path`: Path of the document (non-nullable).
  - `documentKey`: Document key or password (non-nullable).
  - `userId`: User ID associated with the document (transient).
  - `documentStatus`: One-to-one relationship with `DocumentStatus` entity.
- Designed the `DocumentStatus` entity with the following fields:
  - `id`: Unique identifier for the document status (auto-generated).
  - `status`: Enum representing the status of the document (DRAFT, PUBLISHED, ARCHIVED).
- Established a one-to-one relationship between `Document` and `DocumentStatus` using the `@OneToOne` annotation and cascade types.

## Notification Service

- Designed the `Notification` entity with the following fields:
  - `id`: Unique identifier for the notification (auto-generated).
  - `title`: Title of the notification (non-nullable).
  - `body`: Body or content of the notification.
  - `notificationTime`: Timestamp of when the notification was sent (non-nullable).
  - `expiryTime`: Expiration date/time for the notification (non-nullable).
  - `createdTime` and `lastModifiedTime`: Timestamps for notification creation and modification.
  - `isSpecificToUser`: Flag indicating if the notification is specific to a user or not.
  - `isActive`: Flag indicating if the notification is active or not (default: true).
  - `notificationType`: One-to-one relationship with `NotificationType` entity.
- Designed the `NotificationType` entity with the following fields:
  - `id`: Unique identifier for the notification type (auto-generated).
  - `type`: Enum representing the type of notification (SMS, EMAIL, WEB).
- Established a one-to-one relationship between `Notification` and `NotificationType` using the `@OneToOne` annotation.

## Department and UserDepartment Entities

- Designed the `Department` entity with the following fields:
  - `id`: Unique identifier for the department (auto-generated).
  - `name`: Name of the department (non-nullable).
  - `description`: Description of the department.
  - `departmentStatus`: One-to-one relationship with `DepartmentStatus` entity.
- Designed the `DepartmentStatus` entity with the following fields:
  - `id`: Unique identifier for the department status (auto-generated).
  - `status`: Enum representing the status of the department (ACTIVE, INACTIVE).
- Established a one-to-one relationship between `Department` and `DepartmentStatus` using the `@OneToOne` annotation and cascade types.
- Decided to use the `ClinicWaveUser` entity for doctors, patients, and other user types, segregated by role and user type.
- Introduced the `UserDepartment` entity to map users (doctors, staff, etc.) to their respective departments.
- `UserDepartment` has the following fields:
    - `id`: Unique identifier for the user-to-department mapping (auto-generated).
    - `clinicWaveUser`: One-to-one relationship with `ClinicWaveUser` (non-nullable).
    - `department`: One-to-one relationship with `Department` (non-nullable).
- Established one-to-one relationships between `UserDepartment` and `ClinicWaveUser`, `UserDepartment` and `Department` using the `@OneToOne` annotation and join columns.
- Patient users will not have an entry in the `UserDepartment` table, as they do not belong to any department.

## Doctor Schedule

- Discussed the design for the `DoctorSchedule` entity.
- Identified the need for a separate `DoctorScheduleDay` table to store fixed days (Monday, Tuesday, etc.).
- Proposed a one-to-many relationship between `DoctorSchedule` and `DoctorScheduleDay` to represent a doctor's availability on multiple days.
- Decided to discuss and implement the doctor schedule use case in the next session.

## Next Steps

- Complete the design for the doctor schedule, appointment, and payment gateway entities.
- Apply the bounded context pattern to derive microservices from the domain model.
- Start the microservices implementation from the following session.