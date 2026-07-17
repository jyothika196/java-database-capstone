## Admin User Stories

1. Admin Login
Title:
As an admin, I want to log into the portal with my username and password, so that I can manage the platform securely.

Acceptance Criteria:

Secure login screen accepts a valid admin username and password.

Successful authentication redirects the admin to the admin dashboard.

Invalid credentials display an error message and deny access.

Priority: High
Story Points: 3
Notes:

Password inputs must be masked on screen.

2. Admin Logout
Title:
As an admin, I want to log out of the portal, so that I can protect system access.

Acceptance Criteria:

A clear "Log Out" option is accessible from the navigation bar on any admin page.

Clicking log out invalidates the current user session immediately.

The user is redirected back to the login screen and cannot use the browser's back button to re-enter.

Priority: High
Story Points: 2
Notes:

Session tokens should be completely destroyed on both the client and server sides.

3. Add Doctors
Title:
As an admin, I want to add new doctors to the portal, so that they can be registered in the system.

Acceptance Criteria:

An input form is available containing fields for the doctor's name, specialization, and contact info.

Submitting valid form data creates a new doctor record in the database.

The system prevents duplicate doctor registrations based on unique fields like email or license number.

Priority: High
Story Points: 5
Notes:

Requires backend form field validation before database insertion.

4. Delete Doctor Profile
Title:
As an admin, I want to delete a doctor's profile from the portal, so that inactive or departed staff records are removed.

Acceptance Criteria:

A "Delete" button is present on individual doctor profile management pages.

Clicking "Delete" prompts a confirmation dialog to prevent accidental removal.

Confirming removes the doctor profile from the active view or marks it as inactive.

Priority: Medium
Story Points: 3
Notes:

Consider a soft delete flag in the database to preserve historical appointment records.

5. Track Usage Statistics
Title:
As an admin, I want to run a stored procedure in the MySQL CLI, so that I can get the number of appointments per month and track usage statistics.

Acceptance Criteria:

The MySQL database contains a defined stored procedure for aggregating appointments by month.

The procedure can be successfully executed via the command-line interface.

The query output accurately calculates and cleanly displays monthly totals.

Priority: Medium
Story Points: 5
Notes:

Ensure the database aggregation correctly handles boundaries between different calendar years.

## Doctor User Stories

1. View Appointment Schedule
Title:
As a doctor, I want to view my daily appointment schedule, so that I know which patients I am seeing and at what times.

Acceptance Criteria:

A dashboard view displays all booked appointments for the current date in chronological order.

Each appointment entry displays the patient's name, appointment time, and reason for the visit.

The doctor can toggle the view to see weekly or monthly schedules.

Priority: High
Story Points: 5
Notes:

The schedule should refresh automatically or have a quick manual refresh button.

2. Set Availability Slots
Title:
As a doctor, I want to set and update my weekly availability slots, so that patients can only book me when I am free.

Acceptance Criteria:

An interactive calendar interface allows the doctor to select days and time ranges for work hours.

Saving changes updates the available slots exposed to the patient booking interface.

The system blocks patients from booking slots that have been marked as unavailable.

Priority: High
Story Points: 5
Notes:

Need to prevent doctors from marks slots as unavailable if a patient has already booked them without a cancellation workflow.

3. Update Patient Medical Notes
Title:
As a doctor, I want to add and update medical notes during an appointment, so that the patient's health record remains accurate.

Acceptance Criteria:

A text entry field is accessible within the active patient's appointment session screen.

Saving the notes appends them permanently to the specific patient's electronic medical record history.

The system displays timestamped historical notes from previous visits for context.

Priority: High
Story Points: 5
Notes:

Medical notes should be immutable once finalized to ensure legal compliance.

4. Write Digital Prescriptions
Title:
As a doctor, I want to write and issue digital prescriptions, so that patients can fulfill their medication orders.

Acceptance Criteria:

A prescription module allows searching for medications and selecting dosages/frequencies.

The doctor can digitally sign and issue the completed prescription form.

The issued prescription is instantly attached to the patient's account portal.

Priority: Medium
Story Points: 8
Notes:

Include a dropdown list of standard medications to minimize manual typing errors.

5. Cancel Appointment with Notification
Title:
As a doctor, I want to cancel an upcoming appointment when emergencies arise, so that the patient is notified immediately.

Acceptance Criteria:

A "Cancel Appointment" option is available on any scheduled slot detail panel.

Canceling requires entering a brief reason that is logged in the system.

Triggering the cancellation frees up the time slot and sends an automated notification alert to the patient.

Priority: Medium
Story Points: 3
Notes:

Notification can be handled via an internal inbox system or simulated email trigger.

## Patient User Stories

1. Patient Registration
Title:
As a patient, I want to register for an account on the portal, so that I can access booking and medical features.

Acceptance Criteria:

A sign-up page collects patient details including full name, email, password, and date of birth.

System validates that the password meets security strength requirements and the email is unique.

Successful registration creates a patient profile and automatically logs the user in.

Priority: High
Story Points: 5
Notes:

Implement client-side validation for immediate input feedback to the user.

2. Search for Doctors
Title:
As a patient, I want to search for doctors by specialization, so that I can find the right provider for my medical needs.

Acceptance Criteria:

A search bar allows patients to filter available doctors by medical specialty or name.

Search results display matching doctor profiles along with their qualifications and specializations.

Clicking a doctor's profile card opens their detailed information page and availability.

Priority: High
Story Points: 5
Notes:

A default view should list all doctors if no search filter is applied.

3. Book an Appointment
Title:
As a patient, I want to book an available slot with a selected doctor, so that I can secure a time for a medical consultation.

Acceptance Criteria:

The doctor's profile page displays an interactive grid of their open, unbooked time slots.

Selecting a slot and clicking "Confirm Book" reserves the appointment.

The booked slot is instantly marked as unavailable to prevent double-booking.

Priority: High
Story Points: 8
Notes:

Ensure database transactions handle race conditions if two patients attempt to book the exact same slot simultaneously.

4. View Booking History
Title:
As a patient, I want to view my upcoming and past appointments, so that I can keep track of my medical visits.

Acceptance Criteria:

A dedicated "My Appointments" tab lists scheduled future visits prominently.

A separate section or toggle lists completed historical appointments.

Each listed item displays the doctor's name, date, time, and current booking status (e.g., Confirmed, Canceled).

Priority: Medium
Story Points: 3
Notes:

Sort upcoming appointments closest to the current date first.

5. Cancel a Booked Appointment
Title:
As a patient, I want to cancel an upcoming appointment if my plans change, so that the time slot is freed up for others.

Acceptance Criteria:

A "Cancel" button is available next to any upcoming appointment in the dashboard view.

A confirmation prompt ensures the action is intentional before executing.

Confirming removes the appointment from the upcoming list and marks the slot as open on the doctor's schedule.

Priority: Medium
Story Points: 3
Notes:

You can add a business rule constraint preventing cancellations within 24 hours of the appointment time.
