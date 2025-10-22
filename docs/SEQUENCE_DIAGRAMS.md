# Sequence Diagrams for Fitness App

This document illustrates sequence interactions between system components for core Fitness App use cases. Diagrams show message flow among the user interface, backend services, and storage.

---

## UC1: User Login

**Participants**
- **User** — the individual interacting with the application.  
- **Frontend** — client-side application (web or mobile).  
- **Auth Service** — backend service responsible for authentication (login, token issuance).  
- **User Service** — backend service managing user data and credentials.  
- **Database** — persistent storage (e.g., PostgreSQL) for users and auth data.

**Sequence**
![loginSEQ](https://github.com/user-attachments/assets/fe7cf119-5aa0-4cbf-a321-369a518db7b2)

---

## UC2: Create Workout

**Participants**
- **User** — the individual creating a workout.  
- **Frontend** — client-side application presenting the creation form.  
- **Workout Service** — backend service managing workout entities.  
- **User Service** — implicitly involved for user validation/ownership checks (via Auth Service).  
- **Database** — persistent storage (e.g., PostgreSQL) for workout records.

**Sequence**
![CreateWorkoutSEQ](https://github.com/user-attachments/assets/cf213aa9-ab79-4a7a-9edb-07e35035a4a2)

---

## UC3: Add Workout to Program

**Participants**
- **User** — the individual adding a workout to a program.  
- **Frontend** — client-side application displaying program and workout lists.  
- **Program Service** — backend service managing programs and ProgramWorkout relations.  
- **Workout Service** — backend service providing workout details and validation.  
- **Database** — persistent storage (e.g., PostgreSQL) for program/workout relationships and junction tables.

**Sequence**
![addWorToProgSEQ](https://github.com/user-attachments/assets/3f98accf-1b28-4342-a432-abed984d8430)
