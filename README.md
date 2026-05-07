# StitchPro — Sewing Shop Management System
## Django + Django Unfold + PostgreSQL/SQLite

---

## Quick Start (SQLite — easiest)

```bash
# 1. Clone / extract the project
cd sewingshop

# 2. Create virtual environment
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure environment
cp .env.example .env
# .env already has USE_SQLITE=True — no database setup needed!

# 5. Run migrations
python manage.py migrate

# 6. Load demo data
python manage.py seed_data

# 7. Start server
python manage.py runserver
```

Open: http://127.0.0.1:8000/
Admin: http://127.0.0.1:8000/admin/
Login: admin / admin123

---

## PostgreSQL Setup

Edit `.env`:
```
USE_SQLITE=False
DB_NAME=sewingshop_db
DB_USER=postgres
DB_PASSWORD=yourpassword
DB_HOST=localhost
DB_PORT=5432
```

Create the database:
```bash
psql -U postgres -c "CREATE DATABASE sewingshop_db;"
```

Then run migrations:
```bash
python manage.py migrate
python manage.py seed_data
python manage.py runserver
```

---

## Project Structure

```
sewingshop/
├── manage.py
├── requirements.txt
├── .env.example
├── sewingshop/
│   ├── settings.py        # Django + Unfold config
│   ├── urls.py
│   └── wsgi.py
├── shop/
│   ├── models.py          # 13 normalized models
│   ├── admin.py           # Full Unfold admin
│   ├── views.py           # All workflow views
│   ├── forms.py           # All forms
│   ├── urls.py
│   ├── apps.py
│   ├── migrations/
│   └── management/commands/
│       └── seed_data.py   # Demo data
└── templates/
    ├── registration/login.html
    └── shop/
        ├── base.html
        ├── dashboard.html
        ├── customer_*.html
        ├── order_*.html
        ├── ticket_*.html
        ├── employee_list.html
        └── report.html
```

---

## Models / Database Tables

| # | Table | Purpose |
|---|-------|---------|
| 1 | Customer | Stores customer info |
| 2 | Employee | Staff and workers |
| 3 | EmployeeAvailability | Availability tracking |
| 4 | ProductionStage | Reference stages |
| 5 | CustomerOrder | Main orders |
| 6 | OrderItem | Garments within orders |
| 7 | Measurement | Garment measurements |
| 8 | WorkTicket | Production tickets |
| 9 | TaskAssignment | Employee-ticket assignment |
| 10 | TicketStatusHistory | Stage change audit log |
| 11 | DamageIncident | Damage/incident tracking |
| 12 | Payment | Payment transactions |
| 13 | Delivery | Delivery records |

---

## Workflows Implemented

**Workflow 1 — Customer Order Creation**
1. Register customer → `/customers/new/`
2. Create order → `/orders/new/`
3. Add garments → `/orders/<id>/items/add/`
4. Add measurements → `/items/<id>/measurements/add/`

**Workflow 2 — Ticket & Production**
1. Create work ticket → `/tickets/new/<item_id>/`
2. Assign employee → ticket detail page
3. Update production stage → ticket detail page
4. History recorded automatically

**Workflow 3 — Completion & Delivery**
1. Mark ticket completed → update stage to "Delivered"
2. Record payment → order detail page
3. Schedule delivery → order detail page
4. Confirm delivery → marks order as delivered

**Workflow 4 — Damage Incident**
1. Report incident → ticket detail page
2. Ticket auto-blocked
3. Manager resolves incident
4. Ticket unblocked

**Workflow 5 — Payment**
1. Record payment from order detail
2. Payment status auto-updates

**Workflow 6 — Delivery**
1. Schedule delivery from order detail
2. Confirm delivery — closes order

---

## Admin Panel Features (Django Unfold)

- Custom sidebar navigation with icons
- Color-coded status badges
- Inline editing for all related models
- Search and filtering on all models
- Date hierarchy on time-based models
- Custom purple brand theme
