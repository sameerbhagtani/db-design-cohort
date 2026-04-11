# Smart Elevator Control System - Database Design

---

## 📌 Overview

This project models a **Smart Elevator Control System** designed for large-scale buildings such as corporate offices, malls, airports, and residential complexes.

The system supports multiple buildings, each containing multiple floors, elevator shafts, and elevators. It is built to handle real-time elevator requests, assign elevators efficiently, track elevator movements, and maintain operational history.

The design focuses on clearly separating different concerns:

- **Requests** (user intent)
- **Assignments** (system decision)
- **Ride Logs** (actual execution)
- **Maintenance Logs** (historical tracking)

This ensures the system remains scalable, maintainable, and aligned with real-world elevator operations.

---

## 🔗 Eraser Whiteboard

👉 View the interactive diagram: **[Open in Eraser](https://app.eraser.io/workspace/ucPsQWxG1OjMPijikRTG)**

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

### 1. Clear Separation of Concerns

- `floor_requests` represent user intent (pressing a button)
- `assigned_elevator_id` captures the system's decision
- `ride_logs` track actual elevator movement (execution)
- Avoids mixing multiple responsibilities into a single table

---

### 2. Elevator Lifecycle Modeling

- Requests move through a lifecycle: `pending -> assigned -> completed`
- `assigned_at` and `completed_at` help track performance and delays
- `ride_log_id` links execution back to requests

---

### 3. Physical vs Logical Modeling

- `elevator_shafts` represent physical structure
- `elevators` represent moving entities inside shafts
- Maintains a clean real-world abstraction

---

### 4. Zone-Based Elevator Organization

- Buildings are divided into `zones`
- Each zone covers a range of floors
- `elevator_shafts` are grouped under zones for better control and scalability

---

### 5. Ride Logs for Execution Tracking

- Each `ride_log` represents one elevator trip
- Tracks:
    - start floor -> end floor
    - start time -> end time
- One ride can fulfill multiple requests

---

### 6. Maintenance History Tracking

- `maintenance_logs` track elevator downtime using `started_at` and `ended_at`
- Supports both `scheduled` and `emergency` maintenance
- Preserves full historical data without overwriting current state

---

### 7. Data Integrity and Normalization

- Avoided redundant fields like `building_id` in elevators
- Used proper foreign keys to maintain relationships
- Ensured scalable design without unnecessary duplication

---

## ⚙️ Key Relationships

- `buildings -> floors` (1:M)
- `buildings -> zones` (1:M)

- `zones -> elevator_shafts` (1:M)
- `elevator_shafts -> elevators` (1:1)

- `floors -> floor_requests` (1:M)

- `elevators -> floor_requests` (1:M) _(via assignment)_
- `elevators -> ride_logs` (1:M)

- `ride_logs -> floor_requests` (1:M)

- `floors -> ride_logs` (1:M) _(start_floor_id, end_floor_id)_

- `elevators -> maintenance_logs` (1:M)
