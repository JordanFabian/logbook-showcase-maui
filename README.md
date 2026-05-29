> **Note on Proprietary Software:** This repository serves as a technical showcase and architectural overview. The full source code is part of a proprietary commercial project. For technical evaluations, live demonstrations, or partnership inquiries, please contact me directly.

## Overview
LogbookApp (FlightHangar) is a high-performance, cross-platform mobile SaaS engineered specifically for the aviation industry. It solves the critical problem of manual flight logging by automating complex flight time calculations, generating official auditable documents, and providing a robust, offline-first environment for pilots to manage their aeronautical data.

Currently, both the core offline engine and the Cloud/SaaS synchronization layer are fully operational, delivering an enterprise-grade experience from the remote hangar to the cloud, packed with automation, telemetry, and strict compliance features.

## Key Technical & Architectural Features

### 1. Offline-First Architecture & Military-Grade Security
Aviation requires software that works flawlessly in airplane mode or remote hangars. LogbookApp implements a highly secure local data layer:
* **AES-Encrypted Local SQLite:** The on-device database is fully encrypted using dynamic session keys, protecting sensitive pilot data and preventing unauthorized local extraction.
* **PBKDF2 Key Derivation & Biometrics:** Local access is guarded by a 100,000-iteration PBKDF2 hashed PIN and native biometric authentication (Fingerprint/FaceID).
* **Bi-Directional Cloud Sync:** A resilient background service (`SyncService`) securely communicates with an ASP.NET Core API via JWT, syncing flight logs and fleet data silently when a stable connection is detected.
* **Zero UI Blocking:** Asynchronous search and CRUD operations using strict MVVM patterns ensure a fluid interface regardless of database load.

### 2. Aviation Engineering & Physics Algorithms
* **Dynamic Weight & Balance (W&B) Engine:** Calculates Center of Gravity (CG) in real-time based on customizable passenger/cargo stations and the specific aircraft envelope. Includes visual MTOW warnings to prevent structural overloading.
* **Smart Maintenance Tracking (CTM):** Automatically monitors total airframe hours against 50h/100h overhaul triggers, visually flagging aircraft status (Available, Maintenance Alert, or Grounded/AOG).
* **Haversine Formula & Flight Planning:** Uses aircraft performance metrics and exact spherical distances between ICAO coordinates to provide real-time estimated flight times and True Heading calculations.
* **Automated Day/Night Split:** Integrates with the Sunrise-Sunset API to automatically calculate the exact division of diurnal and nocturnal flight hours based on geographic coordinates and takeoff time.

### 3. Automation & AI Integrations
* **Tachometer OCR Scanner:** Utilizes `Plugin.Maui.OCR` to extract flight hours directly from the aircraft's physical panel via the device's camera, eliminating manual data entry.
* **QR Code Flight Sharing:** Pilots can instantly share complex flight logs offline using locally generated JSON payloads encoded into QR Codes.
* **.ics Roster Import:** A custom parser that reads corporate monthly rosters (iCalendar files), automatically extracting ICAO codes, dates, and times to batch-create flight plans.
* **METAR Translator:** Retrieves raw meteorological strings from external APIs and parses them into easily readable, decoded formats (Wind, Temp, QNH) for quick line-of-flight assessment.

### 4. Telemetry, Native UX & Monetization
* **Tactical Map HUD:** A custom `MapPage` featuring a translucid Glass Cockpit overlay, rendering singular flight paths or a global heat map of the pilot's entire career utilizing custom OpenAIP aviation tiles.
* **Microcharts Dashboards:** Renders dynamic Donut and Bar charts natively using SkiaSharp to visualize operational metrics (VFR/IFR, Night/Day) and financial data (Per Diem, Fuel costs).
* **Entitlement Service (Paywall):** A seamless interception architecture that guards premium features behind a SaaS subscription model, maximizing conversion through a "Value Trap" strategy.

### 5. Enterprise-Grade Auditing & Compliance (ANAC/FAA)
* **QuestPDF Generation:** Native stream handling utilizing the QuestPDF engine to export pixel-perfect, ANAC-standard Logbook Reports (CIV) and professional Flight CVs directly from the device.
* **The "Black Box" Audit Trail:** The system utilizes **soft-deletes** and maintains a dedicated 'Audit Log'. All original flight data remains preserved on the device and cloud for audit compliance, regardless of pilot modifications.
* **Rich Data Capture:** Features digital signature capture for instructor endorsements and on-device receipt photography for fuel/tax expenses.
* **Input Sanitization:** Heavy input validation and algorithmic sanitization are applied across all forms to prevent injection attacks and ensure database integrity.

## Tech Stack
**Frontend (Mobile SaaS):**
* **Framework:** .NET 8 / MAUI / C# / XAML
* **Architecture:** Strict MVVM (Model-View-ViewModel)
* **Local Database:** Encrypted SQLite-net-sqlcipher
* **Hardware Integrations:** Biometrics, Camera (OCR/QR), Geolocation, SecureStorage
* **UI/UX:** Microcharts (SkiaSharp), ZXing, QuestPDF
* **Target Platforms:** Android, iOS (Cross-platform)

**Backend (Cloud Sync & API):**
* **Framework:** ASP.NET Core Web API
* **ORM:** Entity Framework Core (EF Core)
* **Database:** PostgreSQL
* **Security:** JWT Authentication, BCrypt Password Hashing

## Workflow & Architectural Showcase

*(This first GIF demonstrates the complete user journey and auditing architecture. It starts with the secure cloud login and PIN validation, proceeds through the initial pilot and aircraft setup, and showcases the intelligence of creating a new flight—featuring autocomplete, real-time METAR retrieval, automatic Haversine calculations, and signature capture. It concludes by highlighting the high-fidelity swipe gestures and the non-destructive "Black Box" auditing feature, where edits require an ANAC-compliant justification and deletions are safely archived in the background.)*

![recording-2026-04-07-16-01-32](https://github.com/user-attachments/assets/f53fafc2-5980-4f24-af9b-77d9c096ac13)


*(This second GIF focuses entirely on the interactive Dashboard and reporting engine. It highlights the real-time aggregation of flight metrics and financial statistics. It also demonstrates how pilots can select specific date ranges to instantly generate and export formatted PDF reports directly from the device's local storage.)*

![recording-2026-04-07-16-04-41](https://github.com/user-attachments/assets/bdb59b6f-cf4f-4666-9bc2-73bcee572175)

## Current Development Status
- [x] **Phase 1: Core Offline MVP** - Database, CRUD, MVVM UI/UX, and Haversine calculations.
- [x] **Phase 2: Aviation Physics** - Dynamic Weight & Balance (CG), Maintenance tracking (CTM), and Automated Day/Night split.
- [x] **Phase 3: Automation & Telemetry** - OCR Tachometer scanner, QR Code sharing, `.ics` corporate roster import, and Tactical HUD Maps.
- [x] **Phase 4: SaaS Layer & Cloud Sync** - Paywall entitlement service, PBKDF2/Biometric security, and bi-directional background synchronization.
- [x] **Phase 5: Official Reporting** - On-device ANAC-standard PDF report and CV generation using QuestPDF with digital signatures.
- [ ] **Phase 6: UI/UX Overhaul & Launch** - Visual redesign App Store / Google Play deployment.

## Contact & Opportunities
For technical evaluations, live demonstrations, or inquiries regarding strategic partnerships and licensing, please reach out directly. I am open to exploring enterprise-level software integrations and remote development opportunities.


jordanmaycon@gmail.com
