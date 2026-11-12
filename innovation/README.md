# Innovation: SalonEase APEX app and dashboard

**Option:** User interface + Analytics dashboard
**Tool:** Oracle APEX, connected to the SalonEase Oracle database

## Plan

- **User interface:** staff register clients, book appointments, manage services and record payments. Each action calls a PL/SQL procedure (`register_client`, `book_appointment`, `record_payment`, ...).
- **Dashboard:** daily bookings, monthly revenue, appointment status (completed / cancelled / no-show) and most popular services, read from live database views.
- **Rule:** business rules stay in PL/SQL (procedures and triggers). APEX only shows and sends data.

## To be completed in later phases

- What it does
- How to run it
- Screenshots
