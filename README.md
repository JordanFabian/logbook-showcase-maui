# LogbookApp: Offline-First Aviation SaaS (Showcase)

*Read this in other languages: [English](README.md), [Português](README.pt-BR.md).*

> **Note on Proprietary Software:** This repository serves as a technical showcase and architectural overview. The full source code is part of a proprietary commercial project. For technical evaluations, live demonstrations, or partnership inquiries, please contact me directly.

## Overview
LogbookApp is a high-performance, cross-platform mobile application engineered specifically for the aviation industry. It solves the critical problem of manual flight logging by automating flight time calculations and providing a robust, offline-first environment for pilots to manage their aeronautical data.

Currently, the core offline engine (MVP) is 100% complete and fully functional, while the Cloud/SaaS synchronization layer is in the final stages of development.

## Key Technical & Architectural Features

### 1. Offline-First Architecture & Data Management
Aviation requires software that works flawlessly in airplane mode or remote hangars. LogbookApp implements a robust local data layer:
* **Secured Multi-Tiered Access:** Secured by a cloud login and a dedicated offline numeric PIN flow to protect sensitive data on the device.
* **Engineered Local SQLite:** Highly optimized database structure to manage datasets of over **4,000 official ANAC (Brazilian Civil Aviation Agency) airport records**.
* **Zero UI Blocking:** Implemented asynchronous search and CRUD operations using the MVVM pattern to ensure a fluid interface regardless of database load.

### 2. Geospatial Engineering & Automated Reporting
* **Intelligent Flight Planning:** The automated "New Flight" creation flow uses aircraft performance data to provide real-time distance and estimated flight time calculations between ICAO codes.
* **Automatic Day/Night Division:** The algorithm uses local time and geographic coordinates to automatically compute the division between day and night flight hours.
* **Haversine Formula Implementation:** A central engineering feature used to compute exact spherical distances between airports on a device, ensuring high accuracy for flight time predictions.
* **METAR/TAF Integration:** The app retrieves and displays meteorological reports for origin and destination aerodromes on-device when online.

### 3. Native Gestures & UX
* **MVVM Implementation:** Strictly follows the Model-View-ViewModel pattern for a clean, testable codebase.
* **High-Fidelity Interaction:** Implements native-feeling physics-based swipe-to-delete/edit gestures on the main list, providing immediate, fluid user feedback.
* **Data Capture:** Features rich data entry capabilities, including on-device receipt photography and native signature capture.
* **CSV/PDF Export:** Includes native stream handling to export auditable reports in CSV or PDF formats directly from the device.

### 4. Enterprise-Grade Security & Non-Destructive Auditing (ANAC Compliance)
Security and data integrity are treated as architectural cornerstones. A central tenet is **non-destructive data handling**.
* **The "Black Box" Audit Trail:** Even when the UI presents deletion or edit confirmations, the underlying database does not physically remove records. The system utilizes **soft-deletes** and maintains a dedicated 'Audit Log' (the 'Black Box'). All original flight data remains preserved on the device (and soon to the cloud) for ANAC audit compliance, regardless of pilot modifications.
* **Enforced Editing Rationale:** An 'Edit' workflow cannot be finalized until the pilot inputs a justificative reason, ensuring a clear, auditable trail of change for every modified signed record.
* **Input Sanitization:** Heavy input validation and sanitization are applied across all forms to prevent common injection attacks in the local database.

## Tech Stack
* **Framework:** .NET MAUI / C# / XAML
* **Architecture:** MVVM (Model-View-ViewModel)
* **Local Database:** SQLite
* **Geospatial & ETL:** Haversine Algorithms, Apache Hop pipeline for ANAC data import
* **Data Science Tools:** Python for exploratory data analysis
* **Target Platforms:** Android, iOS (Cross-platform)

## Workflow & Architectural Showcase

*(This first GIF demonstrates the complete user journey and auditing architecture. It starts with the secure cloud login and PIN validation, proceeds through the initial pilot and aircraft setup, and showcases the intelligence of creating a new flight—featuring autocomplete, real-time METAR retrieval, automatic Haversine calculations, and signature capture. It concludes by highlighting the high-fidelity swipe gestures and the non-destructive "Black Box" auditing feature, where edits require an ANAC-compliant justification and deletions are safely archived in the background.)*

![recording-2026-04-07-16-01-32](https://github.com/user-attachments/assets/f53fafc2-5980-4f24-af9b-77d9c096ac13)


*(This second GIF focuses entirely on the interactive Dashboard and reporting engine. It highlights the real-time aggregation of flight metrics and financial statistics. It also demonstrates how pilots can select specific date ranges to instantly generate and export formatted PDF reports directly from the device's local storage.)*

![recording-2026-04-07-16-04-41](https://github.com/user-attachments/assets/bdb59b6f-cf4f-4666-9bc2-73bcee572175)


## Current Development Status
- [x] **Phase 1: Core Offline MVP** - Database, CRUD, UI/UX, and geospatial calculations (Completed).
- [x] **Phase 2: On-Device PDF Reporting** - PDF report generation with data range selection (Completed).
- [ ] **Phase 3: SaaS Layer & Auditing Cloud Sync** - Bi-directional cloud synchronization of the on-device "Black Box" data trail, user authentication, and recurring subscription models (In finalization).

## Contact & Opportunities
I am a Software Engineer specializing in .NET MAUI, C#, and auditable system architectures. I am currently open to international remote opportunities or strategic partnerships for this product.

* **LinkedIn:** https://www.linkedin.com/in/jordan-fabian/
* **Email:** jordanmaycon@gmail.com
