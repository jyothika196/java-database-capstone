## MySQL Database Design

List of Clinic System Tables
admin

patients

clinic_locations

doctors

appointments

payments

Detailed Table Schemas
Table: admin
id: INT, Primary Key, Auto Increment, Not Null

username: VARCHAR(50), Unique, Not Null

password_hash: VARCHAR(255), Not Null

email: VARCHAR(100), Unique, Not Null

created_at: TIMESTAMP, Default CURRENT_TIMESTAMP

Table: patients
id: INT, Primary Key, Auto Increment, Not Null

first_name: VARCHAR(50), Not Null

last_name: VARCHAR(50), Not Null

email: VARCHAR(100), Unique, Not Null

phone: VARCHAR(20), Not Null

dob: DATE, Not Null

gender: VARCHAR(15), Nullable

created_at: TIMESTAMP, Default CURRENT_TIMESTAMP

Table: clinic_locations
id: INT, Primary Key, Auto Increment, Not Null

name: VARCHAR(100), Unique, Not Null

address: VARCHAR(255), Not Null

phone: VARCHAR(20), Nullable

Table: doctors
id: INT, Primary Key, Auto Increment, Not Null

first_name: VARCHAR(50), Not Null

last_name: VARCHAR(50), Not Null

specialization: VARCHAR(100), Not Null

email: VARCHAR(100), Unique, Not Null

phone: VARCHAR(20), Nullable

location_id: INT, Foreign Key → clinic_locations(id), On Delete Set Null

is_active: TINYINT(1), Default 1

Table: appointments
id: INT, Primary Key, Auto Increment, Not Null

doctor_id: INT, Foreign Key → doctors(id), On Delete Restrict

patient_id: INT, Foreign Key → patients(id), On Delete Cascade

appointment_time: DATETIME, Not Null

status: INT (0 = Scheduled, 1 = Completed, 2 = Cancelled), Default 0

Table: payments
id: INT, Primary Key, Auto Increment, Not Null

appointment_id: INT, Foreign Key → appointments(id), On Delete Cascade, Unique

amount: DECIMAL(10, 2), Not Null

payment_status: INT (0 = Pending, 1 = Paid, 2 = Refunded), Default 0

payment_date: DATETIME, Nullable

Design Considerations & Business Logic
1. Data Validation (Email & Phone)
Email & Phone Validation: While the database enforces string limits and uniqueness, format validation should be handled in the application/backend code. Validating emails (via Regex) and phone numbers (via libraries like libphonenumber) in your backend code ensures clean input before it ever touches your database, preventing query execution errors.

2. Patient Deletion Behavior
Cascade vs. Audit Trail: In this schema, patient_id in the appointments table is configured with ON DELETE CASCADE. This means if a patient record is completely deleted, all of their historical appointments (and associated payments) will automatically be removed to maintain referential integrity.

Clinical System Best Practice: In a real-world healthcare application, hard-deleting patient records is heavily discouraged due to medical auditing laws. Instead, you should implement a Soft Delete mechanism by adding an is_deleted (TINYINT/Boolean) column to your patients table, allowing you to hide deleted records from users while preserving historical medical files.

3. Overlapping Doctor Appointments
Prevention Logic: A doctor should never have overlapping appointments.

Implementation: This is best validated at the application level right before an appointment is booked. Your backend should query the database to verify if the selected doctor already has an active appointment block at that specific time.

Database level: If appointments have fixed duration blocks (e.g., 30 minutes), you can also enforce a unique index constraint across (doctor_id, appointment_time) to block duplicate bookings at the exact same start time directly at the database layer.

## MongoDB Collection Design

To complement the MySQL relational schema, a NoSQL document store (like MongoDB) is perfect for handling highly clinical, semi-structured, and variable data.

While core business processes like booking and billing belong in MySQL, clinical data like prescriptions are best stored in NoSQL. This is because a single prescription might contain multiple medications, varying dosage instructions, refill trackings, and pharmacy details that would otherwise require multiple complex table joins in a relational system.

{
  "_id": "ObjectId('6697b7b1e4b0a8a8f1a2b3c4')",
  "prescription_number": "RX-2026-9842",
  "appointment_id": 1042,
  "patient": {
    "patient_id": 450,
    "full_name": "Jane Doe",
    "dob": "1994-08-12"
  },
  "doctor": {
    "doctor_id": 18,
    "full_name": "Dr. Sarah Jenkins",
    "specialization": "Pediatrics"
  },
  "medications": [
    {
      "name": "Amoxicillin",
      "dosage": "500mg",
      "route": "Oral",
      "frequency": "Three times daily",
      "duration_days": 10,
      "quantity_dispensed": 30,
      "refills_allowed": 0,
      "instructions": "Take with food. Complete the full course even if symptoms improve."
    },
    {
      "name": "Ibuprofen",
      "dosage": "400mg",
      "route": "Oral",
      "frequency": "Every 6 hours as needed for pain or fever",
      "duration_days": 5,
      "quantity_dispensed": 20,
      "refills_allowed": 1,
      "instructions": "Take with a full glass of water."
    }
  ],
  "pharmacy": {
    "name": "CVS Pharmacy #4102",
    "phone": "555-0199",
    "address": "100 Main St, Suite B"
  },
  "metadata": {
    "issued_at": "2026-07-17T10:30:00Z",
    "expires_at": "2027-07-17T10:30:00Z",
    "status": "active",
    "tags": ["pediatric", "antibiotic", "nsaid"],
    "revision_history": [
      {
        "updated_at": "2026-07-17T10:35:00Z",
        "modified_by": "Dr. Sarah Jenkins",
        "change_description": "Corrected ibuprofen dosage from 200mg to 400mg."
      }
    ]
  }
}

Why this design works:
Hybrid Integration: The appointment_id, patient_id, and doctor_id fields act as lightweight foreign key bridges back to your relational MySQL tables.

Flexible Arrays: Patients are often prescribed multiple medications at once. Storing these as an array of embedded documents inside the single prescription object avoids creating a heavy prescription_items join table.

Audit Trail Tracking: The metadata.revision_history nested array keeps a chronological log of changes directly within the document, ensuring medical compliance records stay contextually unified.
