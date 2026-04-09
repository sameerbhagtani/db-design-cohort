# Clinic Appointment and Diagnostics Platform - Database Design

---

## 📌 Overview

This database models a clinic appointment and diagnostics platform, covering the complete lifecycle of a patient’s interaction with the clinic.

It supports:

- Patient and doctor management with role-based user modeling
- Appointment booking and tracking
- Actual consultations (visits) between doctors and patients
- Diagnostic test prescriptions and report generation
- Payment handling for consultations and tests

The design separates planned events (appointments) from actual events (consultations), and models diagnostics as a multi-step process (test types -> tests -> reports), ensuring clarity, flexibility, and real-world accuracy.

---

## 🔗 Eraser Whiteboard

👉 View the interactive diagram: **[Open in Eraser](https://app.eraser.io/workspace/haX4b4xF3cm5o0s3DNIZ)**

---

## 🧠 ER Diagram Preview

![ER Diagram](./schema.png)

---

## 🧾 Schema Code

The schema is written using **Eraser DSL (diagram-as-code)**.

👉 View the file here: **[schema.eraser](./schema.eraser)**

You can open and edit it using Eraser:

- Go to https://app.eraser.io
- Create a new diagram
- Paste the contents of `schema.eraser`

---

## 🏗️ Design Highlights

### 1. Unified User Model with Role-Based Extension

- Single `users` table handles authentication and identity
- `doctors` and `patients` extend users via 1–1 relationships
- Avoids duplication while supporting role-specific data

---

### 2. Appointment vs Consultation Separation

- `appointments` represent scheduled bookings
- `consultations` represent actual visits

Key design decisions:

- Not all appointments result in consultations (no-shows, cancellations)
- Consultations can exist without appointments (walk-ins)
- Optional 1:1 relationship maintained via `appointment_id`

---

### 3. Doctor Structuring with Departments and Specializations

- `departments` represent high-level doctor grouping
- `specializations` represent granular expertise
- `doctor_specializations` enables many-to-many mapping

This allows flexible and scalable doctor categorization.

---

### 4. Diagnostics Modeling with Type vs Instance Separation

- `test_types` store reusable test definitions (e.g., Blood Test)
- `tests` represent actual prescribed tests per consultation

Key design decisions:

- Avoids duplication of test data
- Enables consistent pricing and standardization

---

### 5. Reports as Results of Tests

- Each `report` is linked one-to-one with a `test`
- Stores both report file reference (`report_url`) and summary

Ensures clear separation between actions (tests) and outcomes (reports).

---

### 6. Flexible Payment System

- `payments` can be linked to either:
    - a consultation, or
    - a diagnostic test

Key design decisions:

- Supports multiple billing scenarios
- Includes `transaction_id` for external payment tracking
- Stores status and method for full payment lifecycle

---

## ⚙️ Key Relationships

- `users -> doctors` (1:1)
- `users -> patients` (1:1)

- `departments -> doctors` (1:M)
- `doctors -> doctor_specializations` (1:M)
- `specializations -> doctor_specializations` (1:M)

- `patients -> appointments` (1:M)
- `doctors -> appointments` (1:M)

- `patients -> consultations` (1:M)
- `doctors -> consultations` (1:M)
- `appointments -> consultations` (1:1)

- `consultations -> tests` (1:M)
- `test_types -> tests` (1:M)

- `tests -> reports` (1:1)

- `consultations -> payments` (1:M)
- `tests -> payments` (1:M)
