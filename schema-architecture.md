## Section 1: Architecture summary

This Spring Boot application uses both MVC and REST controllers. Thymeleaf templates are used for the Admin and Doctor dashboards, while REST APIs serve all other modules. The application interacts with two databases—MySQL (for patient, doctor, appointment, and admin data) and MongoDB (for prescriptions). All controllers route requests through a common service layer, which in turn delegates to the appropriate repositories. MySQL uses JPA entities while MongoDB uses document models

## Section 2: Numbered flow of data and control

1. User accesses AdminDashboard or Appointment pages via the frontend user interface to initiate an action or view clinical records.

2. The action is routed to the appropriate Thymeleaf controller for web page view resolution or a REST controller for asynchronous JSON data requests.

3. The controller handles the incoming HTTP request, triggers initial validation constraints on incoming payloads, and delegates the operation to the business service layer.

4. The service layer processes the underlying clinical business logic, enforces operational rules like double-booking prevention, and routes the data requests to the appropriate database repository.

5. For structured relational data like administrators, patients, doctors, or appointment scheduling details, the service layer communicates with Spring Data JPA repositories to execute SQL operations against the MySQL database.

6. For semi-structured medical details like patient prescriptions, the service layer communicates with Spring Data MongoDB repositories to perform CRUD operations on flexible JSON document collections.

7. The repository layer passes the resulting database records back up through the service layer to the controller, which either renders the dynamic Thymeleaf template view or returns a clean JSON response payload directly to the client browser interface.
