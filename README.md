
---

#### Ticket Breakdown: Healthcare Management System - Hibernate Implementation

---

### **Ticket 1: Implement Doctor Class and DoctorRepository**

#### **Description:**
Implement the `Doctor` class and `DoctorRepositoryImpl` to manage doctor-related data using Hibernate.

#### **Tasks:**

1. **Create Doctor Model Class:**
   - **Path**: `com.healthcaremanagement.model`
   - **Class Name**: `Doctor`
   - **Attributes**:
     - `int doctorId`: Represents the unique identifier for each doctor.
     - `String firstName`: The first name of the doctor.
     - `String lastName`: The last name of the doctor.
     - `String specialty`: The doctor's area of specialization (e.g., Cardiology, Pediatrics).
     - `String email`: The doctor's email address.
   - **Hibernate Annotations**:
     - Use `@Entity` and `@Table(name = "Doctors")` annotations to map the class to the `Doctors` table. This tells Hibernate that this class represents a database table.
     - Annotate `doctorId` with `@Id` and `@GeneratedValue(strategy = GenerationType.IDENTITY)` to indicate that this is the primary key and will auto-increment in the database.
     - Use `@Column` annotations for each field to map them to their respective columns in the `Doctors` table. Example: `@Column(name = "FirstName")` for `firstName`.
   - **Getters and Setters**:
     - Implement getters and setters for each attribute. This is crucial for Hibernate to access the fields during database operations.

   **Good to Know**:
   - **Primary Key Strategy**: Using `GenerationType.IDENTITY` ensures that the database handles the generation of the primary key values, which is standard for auto-increment fields.
   - **Entity Lifecycle**: Understanding the lifecycle of a Hibernate entity (Transient, Persistent, Detached) is important when dealing with CRUD operations.

2. **Implement DoctorRepository:**
   - **Path**: `com.healthcaremanagement.repository`
   - **Class Name**: `DoctorRepositoryImpl`
   - **Methods**:
     - **Create Method**: `void createDoctor(Doctor doctor)`
       - Use Hibernate’s `session.save(doctor)` method to insert a new doctor into the `Doctors` table.
       - **Transaction**: Wrap the save operation in a transaction (`Transaction transaction = session.beginTransaction(); transaction.commit();`). This ensures that the database operation is atomic.
     - **Read Method**: `Doctor getDoctorById(int doctorId)`
       - Use `session.get(Doctor.class, doctorId)` to retrieve a doctor by ID. This method returns `null` if the entity is not found.
       - No transaction is needed for this read operation, but you must ensure the session is properly opened and closed.
     - **Update Method**: `void updateDoctor(Doctor doctor)`
       - Use `session.update(doctor)` to update the existing doctor details in the database.
       - **Transaction**: Like the create method, wrap the update operation in a transaction to ensure changes are persisted correctly.
     - **Delete Method**: `void deleteDoctor(int doctorId)`
       - Use `session.delete(doctor)` to remove a doctor from the database.
       - **Transaction**: Deletion should also be wrapped in a transaction.
     - **List All Method**: `List<Doctor> getAllDoctors()`
       - Use HQL: `session.createQuery("from Doctor", Doctor.class).list()` to retrieve all doctors.
       - No transaction is needed for this operation, but ensure the session is opened and closed properly.

   **Good to Know**:
   - **Transactions**: Always start a transaction before making changes to the database and commit after the operation to ensure data integrity.
   - **HQL vs. SQL**: Hibernate Query Language (HQL) is similar to SQL but operates on entities rather than tables, making it more object-oriented.
   - **Session Management**: Make sure to open and close sessions properly to avoid memory leaks.

3. **Test the Doctor Functionality:**
   - Write simple test cases or use the console to:
     - **Create**: Insert a new doctor record using `createDoctor()`.
     - **Read**: Retrieve a doctor by ID using `getDoctorById()`.
     - **Update**: Modify a doctor’s details using `updateDoctor()`.
     - **Delete**: Remove a doctor by ID using `deleteDoctor()`.
     - **List All**: Retrieve all doctors using `getAllDoctors()`.
   - Ensure that the `DoctorRepositoryImpl` methods interact correctly with the database and perform the expected operations.

   **Good to Know**:
   - **Testing Transactions**: Ensure that the transactions are committing as expected by checking the database after each operation.
   - **Error Handling**: Consider adding basic error handling in your methods to handle potential issues like non-existent records or database connectivity problems.


---

### **Ticket 2: Implement Appointment Class and AppointmentRepository**

#### **Description:**
Implement the `Appointment` class and `AppointmentRepositoryImpl` to manage appointment-related data using Hibernate.

#### **Tasks:**

1. **Create Appointment Model Class:**
   - **Path**: `com.healthcaremanagement.model`
   - **Class Name**: `Appointment`
   - **Attributes**:
     - `int appointmentId`: Represents the unique identifier for each appointment.
     - `int patientId`: The ID of the patient who has the appointment.
     - `int doctorId`: The ID of the doctor who will see the patient.
     - `String appointmentDate`: The date and time of the appointment in the format `YYYY-MM-DD`.
     - `String notes`: Any notes related to the appointment.
   - **Hibernate Annotations**:
     - Use `@Entity` and `@Table(name = "Appointments")` annotations to map the class to the `Appointments` table.
     - Annotate `appointmentId` with `@Id` and `@GeneratedValue(strategy = GenerationType.IDENTITY)` to indicate that this is the primary key and will auto-increment in the database.
     - Use `@Column` annotations to map each field to its respective column in the `Appointments` table. Example: `@Column(name = "AppointmentDate")` for `appointmentDate`.
   - **Relationships**:
     - If needed, you can establish relationships between `Appointment`, `Patient`, and `Doctor` using `@ManyToOne` or `@OneToMany` annotations. However, for simplicity, keep these as basic foreign key fields for now, with the possibility of extending them later.
   - **Getters and Setters**:
     - Implement getters and setters for each attribute.

   **Good to Know**:
   - **Relationships**: When ready to extend, consider using `@ManyToOne` for `patientId` and `doctorId` to establish relationships between entities.
   - **Date Handling**: Ensure the date format is consistent with what is expected by the database and the application.

2. **Implement AppointmentRepository:**
   - **Path**: `com.healthcaremanagement.repository`
   - **Class Name**: `AppointmentRepositoryImpl`
   - **Methods**:
     - **Create Method**: `void createAppointment(Appointment appointment)`
       - Use Hibernate’s `session.save(appointment)` method to insert a new appointment into the `Appointments` table.
       - **Transaction**: Wrap the save operation in a transaction to ensure atomicity.
     - **Read Method**: `Appointment getAppointmentById(int appointmentId)`
       - Use `session.get(Appointment.class, appointmentId)` to retrieve an appointment by ID.
       - No transaction needed for this read operation, but proper session management is required.
     - **Update Method**: `void updateAppointment(Appointment appointment)`
       - Use `session.update(appointment)` to update the existing appointment details in the database.
       - **Transaction**: Wrap the update operation in a transaction to ensure data consistency.
     - **Delete Method**: `void deleteAppointment(int appointmentId)`
       - Use `session.delete(appointment)` to remove an appointment from the database.
       - **Transaction**: Ensure the deletion is within a transaction.
     - **List All Method**: `List<Appointment> getAllAppointments()`
       - Use HQL: `session.createQuery("from Appointment", Appointment.class).list()` to retrieve all appointments.
       - No transaction needed for this operation, but ensure sessions are properly handled.

   **Good to Know**:
   - **Transaction Management**: Always encapsulate create, update, and delete operations within transactions to maintain data integrity.
   - **Session Handling**: Properly open and close sessions to avoid memory leaks and ensure connections are returned to the pool.

3. **Test the Appointment Functionality:**
   - Write simple test cases or use the console to:
     - **Create**: Insert a new appointment record using `createAppointment()`.
     - **Read**: Retrieve an appointment by ID using `getAppointmentById()`.
     - **Update**: Modify an appointment’s details using `updateAppointment()`.
     - **Delete**: Remove an appointment by ID using `deleteAppointment()`.
     - **List All**: Retrieve all appointments using `getAllAppointments()`.
   - Ensure that the `AppointmentRepositoryImpl` methods interact correctly with the database and perform the expected operations.

   **Good to Know**:
   - **Testing Transactions**: Ensure that transactions are committed successfully by checking the database state after each operation.
   - **Date and Time Management**: Ensure that the date format and handling are consistent throughout the application.

---

### **Ticket 3: Refactor Main Class to Include Doctors and Appointments**

#### **Description:**
Refactor the `Main` class to include options for managing doctors and appointments in addition to patients.

#### **Tasks:**

1. **Update the Main Menu:**
   - In the `Main` class, update the main menu to include options for managing doctors and appointments.
   - The menu should display options like:
     - 1. Manage Patients
     - 2. Manage Doctors
     - 3. Manage Appointments

   **Good to Know**:
   - **Menu Navigation**: Ensure that the user is able to navigate back to the main menu after completing operations for each entity.
   - **Input Validation**: Consider adding basic input validation to handle unexpected user inputs.

2. **Implement `manageDoctors()` Method:**
   - Add a new method `private static void manageDoctors(DoctorService doctorService, Scanner scanner)` that will handle CRUD operations for doctors.
   - Use the structure provided in the existing `managePatients()` method as a guide.
   - The method should include a sub-menu that allows the user to:
     - Create a new doctor.
     - Read a doctor’s details by ID.
     - Update a doctor’s details.
     - Delete a doctor by ID.

   **Good to Know**:
   - **Reusability**: Reuse the patterns established in `managePatients()` to streamline the implementation.
   - **Service Layer**: Make sure that the service layer is used to interact with the repository and handle business logic.

3. **Implement `manageAppointments()` Method:**
   - Add a new method `private static void manageAppointments(AppointmentService appointmentService, Scanner scanner)` that will handle CRUD operations for appointments.
   - The structure should follow the same pattern as `managePatients()` and `manageDoctors()`.
   - The method should include a sub-menu that allows the user to:
     - Create a new appointment.
     - Read an appointment’s details by ID.
     - Update an appointment’s details.
     - Delete an appointment by ID.

   **Good to Know**:
   - **Consistency**: Maintain a consistent approach to how CRUD operations are handled across patients, doctors, and appointments.
   - **User Feedback**: Provide clear feedback to the user after each operation, such as confirmation messages for successful creation, updates, or deletion.

4. **Integrate the New Methods:**
   - In the main switch statement inside the `Main` method, integrate the new `manageDoctors()` and `manageAppointments()` methods so they are invoked based on the user's menu choice.
   - Ensure that selecting “2” from the main menu navigates to the doctor management sub-menu, and selecting “3” navigates to the appointment management sub-menu.

   **Good to Know**:
   - **Code Structure**: Keep the `Main` class well-organized by separating concerns (menu handling, CRUD operations, etc.).
   - **Error Handling**: Add error handling to manage invalid IDs or operations that fail due to database constraints.

5. **Testing and Debugging:**
   - Run the application and test each menu option to ensure they navigate correctly to the sub-menus.
   - Test creating, reading, updating, and deleting records for all three entities (patients, doctors, and appointments).
   - Debug any issues that arise during testing, ensuring that all CRUD operations are fully functional.

   **Good to Know**:
   - **End-to-End Testing**: Make sure to test the complete flow for each entity to verify that the application handles all operations as expected.
   - **Database Consistency**: Ensure that the database remains consistent even after multiple CRUD operations across different entities.

---

### Structure Provided to Start

#### **Project Structure:**

```
/src/main/java/com/healthcaremanagement/
├── dao/
│   ├── DatabaseConnection.java (if needed for non-Hibernate setup)
│   ├── PatientRepositoryImpl.java (starting point for repository)
│
├── model/
│   ├── Patient.java (already implemented)
│
├── service/
│   ├── PatientService.java (already implemented)
│
└── Main.java (starter code for managing patients)
```

#### **Starter Files Provided:**
- `Main.java`: Basic setup with a menu for managing patients.
- `Patient.java`: The model class for `Patient`.
- `PatientRepositoryImpl.java`: The repository class for managing `Patient` data using Hibernate.
- `PatientService.java`: The service layer for managing `Patient` operations.

### Final Notes:
- **Annotations**: Pay attention to Hibernate annotations (`@Entity`, `@Table`, `@Id`, etc.) to ensure proper mapping between your model classes and the database tables.
- **Transactions**: Always use transactions for create, update, and delete operations to maintain data integrity.
- **Session Management**: Properly manage Hibernate sessions to avoid memory leaks and ensure efficient use of database connections.
- **Testing**: Test each functionality thoroughly to ensure that the application behaves as expected and interacts correctly with the database.

