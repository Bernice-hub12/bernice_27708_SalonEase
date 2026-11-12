# SalonEase: Salon Appointment and Management System

**Student:** [IGIRANEZA Bernice Gentille] | **ID:** [27708]
**Course:** INSY 8311 – Database Development with PL/SQL | **Instructor:** Eric Maniraguha
**Institution:** Adventist University of Central Africa (AUCA), Faculty of Information Technology
**Academic year:** 2026–2027, Semester I | **Capstone:** Final Exam (/40)

---

## 1. Problem statement

Many small salons still run on paper notebooks. Customers phone or walk in, and the receptionist writes a name and a time in a book. This causes four recurring problems:

- Two customers are booked with the same stylist at the same time (double booking).
- Customer, service and payment records are scattered and easy to lose.
- The owner cannot quickly tell how much the salon earned today or this month.
- Nobody can see which services are most popular, or how many appointments are cancelled or missed.

SalonEase puts customers, stylists, services, appointments and payments into one Oracle database, with business rules enforced by PL/SQL, so the salon stays organised and the owner can see how the business is doing.

## 2. Objectives

**Main objective:** Design and build an Oracle PL/SQL database system that manages salon appointments, services and payments, prevents double booking, and reports revenue and service demand.

**Specific objectives:**
1. Model the salon booking process (register client → choose service → choose stylist → pick date and time → complete service → record payment) as a BPMN/UML swimlane diagram.
2. Design a normalized (3NF) database for clients, stylists, services, appointments and payments, loaded with real data.
3. Implement PL/SQL procedures, functions, cursors, a package and triggers that prevent double booking, calculate totals, and record completed and cancelled appointments.
4. Build an interactive layer (user interface + dashboard) on the Oracle database for daily and monthly revenue, bookings and popular services.
5. Record every important change in an audit log.

## 3. Proposed solution: three connected parts

| Part | What it does | Tool |
|---|---|---|
| **User interface (UI)** | Salon staff register clients, book appointments, manage services and record payments. Every action calls a PL/SQL procedure. | Oracle APEX |
| **Database** | Stores clients, stylists, services, appointments and payments. Triggers and procedures enforce the business rules. | Oracle Database (PDB) + PL/SQL |
| **Dashboard** | Shows daily bookings, monthly revenue, appointment statistics and popular services from live data. | Oracle APEX charts (reads database views) |

Business rules stay in PL/SQL. APEX only displays data and calls procedures.

## 4. Data source

- **Main dataset:** Hair Salon No-Show Dataset, Kaggle (author: frederickferguson), from a hair salon in Toronto, March to July 2018.
  Link: (https://www.kaggle.com/datasets/frederickferguson/hair-salon-no-show-data-set?)
- **File:** `data/hair_salon_no_show_wrangled_df.csv`: **1,952 booking records, 22 columns**, 224 no-shows (11.5%). No client names, phone numbers or dates; stylist first names are replaced with codes in the database.
- **Licence:** CC0: Public Domain
- **Supplement:** a simple survey of salons in Kigali for services and prices, and generated sample records (clients, dates, payments), each labelled in `data/README.md`.

See `data/README.md` for the full column list and how each column maps to the planned tables.

## 5. Innovation

Option chosen: **User interface** combined with **Analytics dashboard**, built in **Oracle APEX** on the live Oracle database.

- Staff book and manage appointments in the APEX app; each action calls a PL/SQL procedure.
- A dashboard page shows daily bookings, monthly revenue, appointment status and the most popular services.
- It will be demonstrated live in Phase VII and documented in `innovation/`.

## 6. Planned tables

`CLIENT`, `STYLIST`, `SERVICE`, `STYLIST_SERVICE`, `STYLIST_SCHEDULE`, `APPOINTMENT`, `APPOINTMENT_SERVICE`, `PAYMENT`, `AUDIT_LOG`

## 7. Planned PL/SQL features

| Feature | Where it is used |
|---|---|
| Procedure `register_client` | The receptionist registers a new customer. |
| Procedure `book_appointment` | Books a service with a stylist at a date and time after checking availability. |
| Procedure `cancel_appointment` / `complete_appointment` | Records cancelled and completed appointments. |
| Procedure `record_payment` | Records payment after the service. |
| Function `is_stylist_available` | Checks a stylist is free before a booking is saved. |
| Function `calc_appointment_total` | Adds up the price of all services in one appointment. |
| Cursors | Loop over completed appointments to produce daily and monthly revenue and a popular-services ranking. |
| Package `salonease_pkg` | Groups the booking, payment and report logic in one place. |
| Trigger (double booking) | Blocks two appointments for the same stylist at overlapping times. |
| Trigger (payment check) | Blocks payment for a cancelled appointment. |
| Restriction rule trigger | Blocks bookings in the past or outside opening hours. |
| Audit trigger → `AUDIT_LOG` | Records who changed or cancelled an appointment and when. |
| Exceptions | Custom errors such as `stylist_not_available` and `invalid_appointment_time`. |

## 8. Repository structure

```
firstname_studentID_salonEase/
├── README.md
├── data/                 raw dataset + data/README.md
├── docs/
│   ├── research/         research_log.md
│   ├── proposal/         SalonEase_Proposal.docx
│   └── presentation/     SalonEase_Presentation.pptx (+ slide_outline.md)
└── innovation/           APEX app and dashboard docs (later phases)
```

## 9. References (APA)

Fadil, N. F. N. (2025). *E-appointment system development for small business: Case study of Jajja Chinta Hair Salon using Shneiderman's Eight Golden Rules theory* [Bachelor's project report, Universiti Teknologi MARA]. https://ir.uitm.edu.my/133935/1/133935.pdf

Fresha. (2026, February 2). *The hidden cost of cancellations: How no-shows are reshaping the beauty industry*. https://www.fresha.com/blog/cancellations-cost-study

Fresha. (n.d.). *Fresha: Booking platform for salons and spas* . https://www.fresha.com

frederickferguson. (n.d.). *Hair Salon No-Show Dataset* [Data set]. Kaggle. [paste Kaggle URL]

Lim, K. H., & Mat Nawi, R. (2023). Development of a web-based application to enhance salon management for L Six Hair Studio. *Applied Information Technology and Computer Science, 4*(2), 1768–1786. https://doi.org/10.30880/aitcs.2023.04.02.099

Nasir, M., Summerfield, N., Dag, A., & Oztekin, A. (2020). A service analytic approach to studying patient no-shows. *Service Business, 14*(2), 287–313. https://doi.org/10.1007/s11628-020-00415-8

Oracle. (n.d.). PL/SQL triggers. In *Database PL/SQL language reference* (Release 18). https://docs.oracle.com/en/database/oracle/oracle-database/18/lnpls/plsql-triggers.html

Yocale. (2025, January 22). *Unlocking insights: Booking behavior analysis*. https://www.yocale.com/blog/booking-behavior-analysis

Details, key findings and gaps for each source: `docs/research/research_log.md`.
