# Hotel Reservation System

A ServiceNow scoped application that allows the public to book hotel rooms and sports activities directly online, with no intermediaries required. Built as a team project on the ServiceNow platform, the system serves guests, registered users, receptionists, and administrators through a combination of Service Portal and platform interfaces.

---

## Table of Contents

- [Overview](#overview)
- [User Types and Access](#user-types-and-access)
- [Data Model](#data-model)
- [Core Features](#core-features)
- [Business Rules and Logic](#business-rules-and-logic)
- [Application Architecture](#application-architecture)
- [Team and Roles](#team-and-roles)
- [Development Workflow](#development-workflow)
- [Setup and Deployment](#setup-and-deployment)
- [Final Deliverables](#final-deliverables)

---

## Overview

The Hotel Reservation System is a fully functional booking platform built within a single ServiceNow scoped application. It enables customers to browse available rooms, make and manage reservations, and look up bookings without creating an account, while giving staff the tools to manage inventory, process reservations, send notifications, and generate reports.

| Property | Value |
|----------|-------|
| Application | Hotel Reservation System |
| Scope | `x_2206701_hotel_0` |
| Platform | ServiceNow |
| Interface | Service Portal (customers) and Platform (staff) |

---

## User Types and Access

The application defines three categories of users with distinct access levels.

| User Type | Portal Access | Platform Access | Capabilities |
|-----------|:-------------:|:---------------:|--------------|
| Guests / Users | Yes | No | Browse rooms, book reservations, look up bookings by reservation number and email |
| Receptionists | Yes | Yes | Manage all reservations, update room and reservation availability, close or delete reservations |
| Administrators | Yes | Yes | All receptionist capabilities, plus manage users, business days, holidays, imports, and reports |

Guests may be either registered users (with an account) or anonymous guests. Administrators inherit all receptionist permissions.

---

## Data Model

The application is built on five core tables.

| Table | Technical Name | Purpose |
|-------|----------------|---------|
| Rooms | `x_2206701_hotel_0_rooms` | Room catalog: name, type, capacity, rate, and availability status |
| Scheduled Reservations | `x_2206701_hotel_0_scheduled_reservations` | Guest bookings: room, dates, number of days, guests, total payment, status |
| Calendar Days | `x_2206701_hotel_0_calendar_days` | Business days, holidays, and events with associated discount percentages |
| Sports Activities | `x_2206701_hotel_0_sports_activities` | Catalog of hotel sports activities with date, time, and rate |
| Users | `x_2206701_hotel_0_users` | Extends the base `sys_user` table with profile fields (age, experience, assigned group) |

### Room Types

Single, Double, Triple, Quad, Queen, King, Executive Suite, Presidential Suite.

### Reservation Status Values

Pending, Confirmed, Checked-in, Checked-out, Cancelled, No-show.

---

## Core Features

- Room browsing with current-day availability
- Room reservation for both registered users and guests
- Sports activity booking, limited to one activity per booking
- Server-calculated booking estimates based on room rate, number of nights, and event-based discounts
- Enforcement of a 72-hour cancellation window before check-in
- Reservation updates permitted at any time
- Guest reservation lookup using reservation number and email
- Automated email notifications to receptionists on booking and update events
- Monthly gross sales reporting with filtering and export
- Administrator batch data import
- Role-based access control across all tables and modules

---

## Business Rules and Logic

The backend enforces booking integrity on the server rather than relying on the client.

| Rule | Description |
|------|-------------|
| Date validation | Check-out date must be strictly after the check-in date |
| One room per reservation | Each reservation references exactly one room |
| Number of days | Automatically calculated from check-in and check-out dates |
| Total payment | Automatically calculated from room rate, number of nights, and applicable event discounts |
| Cancellation window | Cancellations are only permitted at least 72 hours before check-in |
| User resolution | Signed-in customers are securely linked to their user record; guest bookings leave this reference blank |

Availability and pricing logic is centralized so that all booking screens call shared functions rather than duplicating logic.

---

## Application Architecture

The system separates responsibilities across clear boundaries to avoid duplicated work.

- Reservation operations and booking rules are owned by the reservation logic layer.
- Availability checks and pricing calculations are owned by a dedicated availability and pricing layer, exposed as reusable functions.
- Authorization and access control are handled through roles, groups, and access control lists.
- The database schema is maintained as a single shared source of table and field definitions.
- Frontend screens consume backend functions through documented interfaces rather than reimplementing logic.

---

## Team and Roles

| Role | Responsibility | Member |
|:----:|----------------|--------|
| 1 | Frontend Developer: main website and My Reservations | Garcia |
| 2 | Frontend Developer: booking forms | Colo |
| 3 | Backend Developer: reservation logic | Sarsonas |
| 4 | Backend Developer: room availability, pricing, and sports activities | Cauban |
| 5 | Database Developer: tables and fields | Habon |
| 6 | User roles and permissions | Abella |
| 7 | Automation and email notifications | Lapis |
| 8 | Reports and data import | Igonia |
| 9 | Integration, receptionist tools, and final delivery | Paton-og |

---

## Development Workflow

The project follows a branch-and-pull-request model backed by ServiceNow source control.

1. Each member develops on their own feature branch (for example, `role3-sarsonas-reservation-logic`).
2. Changes are committed to the branch through ServiceNow source control.
3. A pull request is opened from the feature branch into `main`.
4. The integration lead (Role 9) reviews each pull request and merges it into `main`.
5. Integration is performed continuously, so conflicts are resolved early rather than at the end.

Direct commits to `main` are avoided. A mergeable branch is treated as ready for review, not as functional approval.

---

## Setup and Deployment

1. Ensure the base `sys_user` table is configured as extensible in the target instance, as the Users table extends it. This is a global prerequisite and may not be included in the scoped application export.
2. Import the scoped application into the ServiceNow instance.
3. Verify tables, fields, choices, references, defaults, and form layouts.
4. Configure roles, groups, access control lists, and module visibility.
5. Load sample data (rooms, calendar days with discounts, and sports activities) for testing.
6. Run end-to-end tests across all four user types before final delivery.

---

## Final Deliverables

- Exported XML of the completed application
- A 20 to 30 minute presentation of the application
