# ClinicWave Full Stack Microservices: Overview 13

## Introduction
- The domains like notification, department, doctor, patient, and document were completed in the previous session.
- The goal for the current session is to work on the doctor schedule and appointment domain.

## Doctor Schedule Domain

### Database Design Considerations
- The approach starts with a relational database management system (RDBMS) for designing the database.
- Later, the bounded context concept will be applied, which might break the relationships and require identifying boundary sizes.

### Entities and Relationships
- A class called `DoctorSchedule` is proposed to contain the following:
  - A one-to-one relationship with a `ClinicWaveUser` entity
  - A one-to-many relationship with `DoctorAvailableDayTime`
  - Message
  - DoctorScheduleStatus

### Initial Implementation
- An `DoctorAvailableDay` entity is created with an ID and dayOfWeek fields.
- A `DoctorSchedule` entity is created with the following fields:
  - `doctor` (ClinicWaveUser)
  - `doctorAvailableDayTimeSet` (Set of `DoctorAvailableDayTime` with an eager fetch type and cascade type `ALL`)
  - `message` (String)
  - `doctorScheduleStatus` (DoctorScheduleStatus)

### Database Table Generation
- The application is run, which creates the database tables based on the entities.
- The connection to the H2 console is established, and the generated tables are verified:
- `doctor_available_day` table with ID and dayOfWeek columns
- `doctor_schedule` table with columns for ID, doctor ID, message, and doctor schedule status ID
- A join table named `doctor_schedule_doctor_available_day_time` with columns for doctor schedule ID and doctor available day time ID

### Identifying Issues
- An issue is identified with the generated join table (`doctor_schedule_doctor_available_day_time`).
- It has only two columns (doctor schedule ID and doctor available day time ID), which is not sufficient to handle multiple doctors with overlapping available days.

## Proposed Solution
- To address the issue, a separate table called `doctor_available_time` is proposed to store the start time and end time for each day.
- This table would have a one-to-many relationship with the `doctor_available_day` table, allowing multiple time slots for a single day.
- The `doctor_available_day_time` table would then have columns for doctor schedule ID, doctor available day ID, and doctor available time ID, mapping all three entities together.

### Implementation Plan
- A new entity called `DoctorAvailableTime` is created with `startTime` and `endTime` fields.
- A new table called `doctor_available_day_time` will be created with columns for doctor schedule ID, doctor available day ID, and doctor available time ID.
- This table will act as a mapper table, joining the `DoctorSchedule`, `DoctorAvailableDay`, and `DoctorAvailableTime` entities together.

## Designing the DoctorAvailableDayTime Table

### Columns in the DoctorAvailableDayTime Table
- The `doctor_available_day_time` table should contain the following columns:
- Doctor Schedule ID
- Doctor Available Day ID
- Doctor Available Time ID

### Mapping Relationships
- The `doctor_available_day_time` table will act as a mapper table, joining the following entities:
- `DoctorSchedule`
- `DoctorAvailableDay`
- `DoctorAvailableTime`

### Implementation Steps
1. Create a new entity called `DoctorAvailableTime` with `startTime` and `endTime` fields.
2. In the `DoctorSchedule` entity, instead of having a `Set<DoctorAvailableDay>`, use a `Set<DoctorAvailableDayTime>`.
3. Configure the mapping relationships in the `DoctorAvailableDayTime` entity:
   - One-to-many relationship with `DoctorSchedule` (mapped by `doctorSchedule`)
   - Many-to-one relationship with `DoctorAvailableDay`
   - Many-to-one relationship with `DoctorAvailableTime`

### Database Table Generation
- After running the application, the generated tables are verified in the H2 console:
  - `doctor_available_day` table
  - `doctor_available_time` table
  - `doctor_available_day_time` table (with columns for doctor schedule ID, doctor available day ID, and doctor available time ID)
- No join table between `doctor_schedule` and `doctor_available_day_time` (as it's mapped directly)

## Designing the Appointment Entity

### Appointment Entity Fields
- The `Appointment` entity will have the following fields:
  - `id` (auto-generated)
  - `patient` (one-to-one relationship with `ClinicWaveUser`)
  - `doctor` (one-to-one relationship with `ClinicWaveUser`)
  - `department` (one-to-one relationship)
  - `appointmentDate` (LocalDate)
  - `appointmentTime` (LocalTime)
  - `message` (String)
  - `appointmentStatus` (AppointmentStatus Entity)

### Appointment Use Case
- When creating an appointment, the patient name will come from the `ClinicWaveUser` entity (patient).
- The department will determine the list of available doctors.
- Once the doctor is selected, the available time slots for that doctor will be displayed based on the doctor's schedule.
- The appointment date, time, and any additional message can be provided.
- The appointment status will be set (e.g., ACTIVE).

### Implementation Steps
1. Create the `Appointment` entity with the required fields and relationships.
2. Run the application to generate the `appointment` table in the database.
3. Verify the generated table structure in the H2 console.

## Next Steps
- The next step is to apply the bounded context concept and derive microservices from the identified subdomains.
- This will be covered in the next session, which is important for understanding the overall architecture and microservices design.

## Conclusion
- The session concludes with a reminder to attend the next session, where the bounded contexts and microservices derivation will be learned.
