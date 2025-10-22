## Class Diagram Description

The core domain of this application revolves around **user management**, **fitness programs**, and **workout tracking**.  

### Main Entities
- **User** — the central actor. All content is owned and scoped to a specific user.  
- **Program** — a collection of workouts defined by a user.  
- **Workout** — an individual exercise routine or activity.  
- **AuthToken** — represents a valid session for an authenticated user.  
- **ProgramWorkout** — an associative entity linking programs and workouts, defining their order within a program.  

### Key Relationships Explained
**User – AuthToken (One-to-Many)**  
- A single `User` can have many active `AuthToken` sessions (e.g., logged in on multiple devices).  
- Each `AuthToken` belongs to exactly one `User`.  

**User – Program (One-to-Many)**  
- A `User` can create and own many `Program` objects.  
- Each `Program` is owned by a single `User`.  

**User – Workout (One-to-Many)**  
- A `User` can create and own many `Workout` objects.  
- Each `Workout` is owned by a single `User`.  

**Program – Workout (Many-to-Many via ProgramWorkout)**  
- Implemented through an associative entity (`ProgramWorkout`), often called a junction table.  
- A `Program` can contain many `Workout` items.  
- A `Workout` can be part of many different `Program` items.  
- The `ProgramWorkout` entity holds additional attributes like `order` (for custom workout sequencing within a program).  

### Attributes
| Entity | Attributes |
|---------|-------------|
| **User** | `id (PK)`, `username`, `email`, `password` |
| **AuthToken** | `id (PK)`, `token`, `expiresAt`, `userId (FK → User)` |
| **Program** | `id (PK)`, `name`, `muscleType`, `userId (FK → User)` |
| **Workout** | `id (PK)`, `title`, `type`, `duration`, `notes`, `userId (FK → User)` |
| **ProgramWorkout** | `order`, `programId (Composite FK → Program)`, `workoutId (Composite FK → Workout)` |

This model ensures **data isolation between users** and provides a **flexible structure** for managing fitness programs and individual workout routines.
![diagram](https://github.com/user-attachments/assets/e927b7a6-e982-4af9-b68e-b93c2979f1bf)
