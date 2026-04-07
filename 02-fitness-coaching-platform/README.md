# Fitness Coaching Platform - Database Design

---

## 📌 Overview

The goal of this design was to model a real-world fitness coaching platform where:

- Trainers manage multiple clients through structured coaching programs
- Clients can either enroll in long-term coaching plans or book one-time consultations
- Sessions (calls/meetings) are scheduled and tracked separately
- Payments support both subscription-based and one-time flows
- Client progress is tracked over time through check-ins, including weight, measurements, and trainer feedback

---

## 🔗 Eraser Whiteboard

👉 View the interactive diagram: **[Open in Eraser](https://app.eraser.io/workspace/2kRCIDf2MTgQRWtFpGR4)**

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
- `trainers` and `clients` extend users via 1–1 relationships
- Avoids duplication while supporting role-specific data

---

### 2. Plans vs Subscriptions

- `plans` represent static offerings created by trainers
- `subscriptions` represent client enrollments over time

Key design decisions:

- Stored `price_at_purchase` to preserve historical accuracy
- Stored `start_date` and `end_date` for lifecycle tracking

---

### 3. Dual Engagement Model (Subscriptions + Consultations)

- `subscriptions` → long-term coaching
- `consultations` → one-time services

---

### 4. Sessions as a Unified Interaction Layer

- `sessions` represent actual interactions (calls/meetings)
- Linked to either:
    - a subscription
    - or a consultation

Key design decisions:

- Avoided duplication by not separating consultation sessions
- Stored scheduling and execution details here (scheduled_at, mode, etc.)

---

### 5. Polymorphic Payments System

- `payments` can be linked to:
    - `subscriptions` (recurring plans)
    - `consultations` (one-time payments)

Key design decisions:

- Supports multiple payment attempts (pending, failed, retry)
- Includes `transaction_id` and `paid_at` for tracking
- Ensures flexibility without duplicating payment logic

---

### 6. Time-Series Progress Tracking (Check-Ins)

- `check_ins` store progress over time, not just current state

Includes:

- weight
- body measurements (chest, waist, hips, etc.)
- client notes
- trainer feedback

Key design decisions:

- Tied strictly to subscriptions to ensure structured coaching
- Avoids overwriting data and enables historical analysis

---

### 7. Online and Offline Coaching Support

- `plans` define delivery mode (`online`, `offline`, `hybrid`)
- `sessions` specify actual execution mode

---

## ⚙️ Key Relationships

- `users -> trainers` (1:1)
- `users -> clients` (1:1)

- `trainers -> plans` (1:M)
- `clients -> subscriptions` (1:M)
- `plans -> subscriptions` (1:M)

- `clients -> consultations` (1:M)
- `trainers -> consultations` (1:M)

- `subscriptions -> sessions` (1:M)
- `consultations -> sessions` (1:M)

- `subscriptions -> payments` (1:M)
- `consultations -> payments` (1:M)

- `subscriptions -> check_ins` (1:M)
