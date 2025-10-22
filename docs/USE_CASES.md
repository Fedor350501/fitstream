## FitStream – Use Case Analysis

This document describes the primary **use case scenarios** for the Fitness App, covering authentication, workout creation, and program management processes.

---

### Actors

- **Unauthenticated User (Guest)** — a user who has not logged in. Can only access the login and registration pages.  
- **Authenticated User** — a registered user who has successfully logged into the system. This is the primary actor for all core functionalities related to fitness programs and workouts.

---

### UC1: Login

**Actor:** Unauthenticated User  
**Precondition:** The user is on the login page.

**Flow of Events:**
1. The user enters their username and password into the login form.  
2. The user clicks the **Login** button.  
3. The frontend performs basic client-side validation (e.g., fields are not empty).  
4. The frontend sends a `POST /api/v1/auth/login` request to the Auth Service with the entered credentials.  
5. The Auth Service receives the request:  
   a. Finds the user by username in the User table (e.g., PostgreSQL).  
   b. Compares the provided password with the stored password hash using a secure algorithm (e.g., bcrypt).  
6. If the credentials are valid:  
   a. The service generates a JWT (JSON Web Token) containing the user's ID.  
   b. The JWT is set as an HTTP-only cookie or returned for frontend storage.  
7. The frontend receives the successful response and redirects the user to their dashboard or program list page.  

**Alternative Flow (A): Invalid credentials**
- The Auth Service returns a `401 Unauthorized` response.  
- The frontend displays “Invalid username or password.”  

**Postcondition:** The user is authenticated, and a session is active.

---

### UC2: Create Workout

**Actor:** Authenticated User  
**Precondition:** The user is logged in and on a page where they can create a new workout (e.g., “My Workouts” or “Create New Workout”).  

**Flow of Events:**
1. The user clicks **Add New Workout** or opens the creation form.  
2. The frontend displays fields for title, type (e.g., Cardio, Strength), duration, and notes.  
3. The user fills in workout details.  
4. The user clicks **Save** or **Create Workout**.  
5. The frontend validates inputs (e.g., title not empty, duration numeric).  
6. The frontend sends a `POST /api/v1/workouts` request to the Workout Service, including the JWT in the Authorization header.  
7. The Workout Service:  
   a. Validates the JWT.  
   b. Validates the input data.  
   c. Creates a new record in the Workout table with the user’s ID as owner.  
8. The service returns `201 Created` with the new workout details.  
9. The frontend shows “Workout created successfully” and updates the list or redirects.  

**Alternative Flow (A): Validation fails**
- The service returns `400 Bad Request` with validation messages.  
- The frontend displays the errors near the affected fields.  

**Postcondition:** A new workout is created and linked to the authenticated user.

---

### UC3: Add Workout to Program

**Actor:** Authenticated User  
**Precondition:** The user is logged in, owns at least one Program and one Workout, and is viewing a Program management page.  

**Flow of Events:**
1. The user opens the details page of an existing Program.  
2. The user clicks **Add Workout to Program**.  
3. The frontend shows available workouts not yet in that Program.  
4. The user selects one or more workouts.  
5. The user confirms the selection.  
6. The frontend sends a `POST /api/v1/programs/{programId}/workouts` request to the Program Service, including workout IDs and the JWT.  
7. The Program Service:  
   a. Validates the JWT and program ownership.  
   b. For each `workoutId`:  
      i. Verifies the workout exists and belongs to the same user.  
      ii. Checks for duplicates in the Program.  
      iii. Creates a new `ProgramWorkout` record linking the Program and Workout, with sequence order.  
8. The service returns `200 OK` or `204 No Content`.  
9. The frontend updates the Program view with the new workouts.  

**Alternative Flow (A): Invalid ownership or ID**
- The service returns `403 Forbidden` or `404 Not Found`.  
- The frontend displays “Could not add workout, check permissions or if workout exists.”  

**Alternative Flow (B): Duplicate workout**
- The service may ignore duplicates or return `409 Conflict`.  
- The frontend indicates which workouts were already included.  

**Postcondition:** Selected workouts are linked to the Program through `ProgramWorkout` entries and visible in the Program details.
