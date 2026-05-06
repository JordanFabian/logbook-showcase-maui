# LogbookApp: Offline-First Aviation SaaS (Showcase)

*Read this in other languages: [English](README.md), [Português](README.pt-BR.md).*

> **Note on Proprietary Software:** This repository serves as a technical showcase and architectural overview. The full source code is part of a proprietary commercial project. For technical evaluations, live demonstrations, or partnership inquiries, please contact me directly.

## Overview
LogbookApp is a high-performance, cross-platform mobile SaaS engineered specifically for the aviation industry. It solves the critical problem of manual flight logging by automating complex flight time calculations, generating official auditable documents, and providing a robust, offline-first environment for pilots to manage their aeronautical data.

Currently, both the core offline engine and the Cloud/SaaS synchronization layer are fully operational, delivering an enterprise-grade experience from the remote hangar to the cloud.

## Key Technical & Architectural Features

### 1. Offline-First Architecture & Encrypted Data Management
Aviation requires software that works flawlessly in airplane mode. LogbookApp implements a highly secure, offline-capable local data layer:
* **AES-Encrypted Local SQLite:** The on-device database is fully encrypted using dynamic session keys, protecting sensitive pilot data and preventing unauthorized local extraction.
* **Bi-Directional Cloud Sync:** A resilient background service (`SyncService`) securely communicates with an ASP.NET Core API via JWT, syncing flight logs and fleet data only when a stable connection is detected.
* **Large Dataset Handling:** Highly optimized database structure to instantly query over **4,000 official ANAC (Brazilian Civil Aviation Agency) airport records** without blocking the UI thread.

### 2. Aviation Engineering & Physics Algorithms
* **Dynamic Weight & Balance (W&B) Engine:** Calculates Center of Gravity (CG) in real-time based on customizable passenger/cargo stations and the specific aircraft envelope. Includes visual MTOW (Maximum Takeoff Weight) warnings to prevent overloading.
* **Smart Maintenance Tracking (CTM):** Automatically monitors total airframe hours against overhaul triggers, visually flagging aircraft status (Available, Maintenance Alert, or Grounded/AOG) in the digital hangar.
* **Haversine Formula & Flight Planning:** Uses aircraft performance metrics (cruising speed) and exact spherical distances between ICAO coordinates to provide real-time estimated flight times.
* **Human-Readable METAR Translator:** Retrieves raw meteorological strings from external APIs and parses them into easily readable, decoded formats for quick line-of-flight assessment.

### 3. Native Gestures, UX & Official Reporting
* **QuestPDF Generation:** Includes native stream handling utilizing the QuestPDF engine to export pixel-perfect, ANAC-standard Logbook Reports (CIV) directly from the device.
* **High-Fidelity Interaction:** Implements native-feeling physics-based swipe-to-delete/edit gestures on the main list, providing immediate, fluid user feedback.
* **Rich Data Capture:** Features digital signature capture for instructor endorsements and on-device receipt photography.

### 4. Enterprise-Grade Security & Non-Destructive Auditing (ANAC Compliance)
Security and data integrity are treated as architectural cornerstones. A central tenet is **non-destructive data handling**.
* **The "Black Box" Audit Trail:** The system utilizes **soft-deletes** and maintains a dedicated 'Audit Log'. All original flight data remains preserved on the device and cloud for ANAC audit compliance, regardless of pilot modifications.
* **Enforced Editing Rationale:** An 'Edit' workflow cannot be finalized until the pilot inputs a justificative reason, ensuring a clear, auditable trail of change for every modified signed record.
* **Input Sanitization:** Heavy input validation and algorithmic sanitization are applied across all forms to prevent injection attacks and ensure database integrity.

## Tech Stack
**Frontend (Mobile App):**
* **Framework:** .NET MAUI / C# / XAML
* **Architecture:** strict MVVM (Model-View-ViewModel)
* **Local Database:** Encrypted SQLite (AES)
* **Reporting:** QuestPDF

**Backend (Cloud SaaS):**
* **Framework:** ASP.NET Core Web API
* **ORM:** Entity Framework Core (EF Core)
* **Database:** PostgreSQL
* **Security:** JWT Authentication, BCrypt Password Hashing

**Data Engineering:**
* **Geospatial:** Haversine Algorithms
* **Pipeline:** Apache Hop & Python for official ANAC aeronautical data import

## Workflow & Architectural Showcase

*(This first GIF demonstrates the complete user journey and auditing architecture. It starts with the secure cloud login and PIN validation, proceeds through the initial pilot and aircraft setup, and showcases the intelligence of creating a new flight—featuring autocomplete, real-time METAR retrieval, automatic Haversine calculations, and signature capture. It concludes by highlighting the high-fidelity swipe gestures and the non-destructive "Black Box" auditing feature, where edits require an ANAC-compliant justification and deletions are safely archived in the background.)*

![recording-2026-04-07-16-01-32](https://github.com/user-attachments/assets/f53fafc2-5980-4f24-af9b-77d9c096ac13)


*(This second GIF focuses entirely on the interactive Dashboard and reporting engine. It highlights the real-time aggregation of flight metrics and financial statistics. It also demonstrates how pilots can select specific date ranges to instantly generate and export formatted PDF reports directly from the device's local storage.)*

![recording-2026-04-07-16-04-41](https://github.com/user-attachments/assets/bdb59b6f-cf4f-4666-9bc2-73bcee572175)


## Current Development Status
- [x] **Phase 1: Core Offline MVP** - Database, CRUD, UI/UX, and geospatial calculations.
- [x] **Phase 2: On-Device PDF Reporting** - Official PDF report generation using QuestPDF.
- [x] **Phase 3: SaaS Layer & Cloud Sync** - Bi-directional synchronization, JWT authentication, and PostgreSQL backend implementation.
- [x] **Phase 4: Aviation Physics Integration** - Dynamic Weight & Balance calculator and Maintenance (CTM) tracking.
- [ ] **Phase 5: Feature Expansion & Monetization** - Enterprise subscription tiers and automated day/night split algorithms.

## Contact & Opportunities
I am a Software Engineer specializing in .NET MAUI, C#, and auditable system architectures. I am currently open to international remote opportunities or strategic partnerships for this product.

* **LinkedIn:** https://www.linkedin.com/in/jordan-fabian/
* **Email:** jordanmaycon@gmail.com
