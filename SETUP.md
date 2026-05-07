# Sewing Shop Management System — Setup & Run Guide

## Prerequisites
- Python 3.8+
- pip

## 1. Clone & Setup Virtual Environment

```bash
cd sewingshop
python -m venv venv
source venv/bin/activate          # macOS/Linux
# or: venv\Scripts\activate        # Windows
```

## 2. Install Dependencies

```bash
pip install -r requirements.txt
```

## 3. Configure Environment

The app comes configured for **SQLite by default** (no database setup needed).

For **PostgreSQL**, edit `.env` or set environment variables:
```
USE_SQLITE=False
DB_NAME=sewingshop_db
DB_USER=postgres
DB_PASSWORD=yourpassword
DB_HOST=localhost
DB_PORT=5432
```

## 4. Run Migrations

```bash
python manage.py migrate
```

## 5. Load Demo Data

```bash
python manage.py seed_data
```

Creates sample customers, employees, orders, and workflows.

## 6. Start Server

```bash
python manage.py runserver
```

## Access

- **App**: http://127.0.0.1:8000/
- **Admin**: http://127.0.0.1:8000/admin/
- **Employee Portal**: http://127.0.0.1:8000/staff/login/
- **Customer Portal**: http://127.0.0.1:8000/portal/

### Credentials

**Admin / Staff**
- Username: `admin`
- Password: `admin123`

**Staff Accounts** (password: `staff123`)
- maria.lopez@stitchpro.com
- carlos.ruiz@stitchpro.com
- ana.torres@stitchpro.com
- luis.morales@stitchpro.com
- sofia.reyes@stitchpro.com
- diego.vega@stitchpro.com

---

## Project Structure

- `shop/` — Main Django app
  - `models.py` — 13 normalized data models
  - `views.py` — All workflow views (dashboard, customers, orders, tickets, etc.)
  - `admin.py` — Django Unfold admin UI
  - `forms.py` — Django forms for all models
  - `urls.py` — URL routing
  - `management/commands/seed_data.py` — Demo data loader
- `sewingshop/` — Django project settings
- `templates/` — HTML templates for shop, employee, and customer portals

---

## Main Workflows

1. **Create Customer** → Orders → Order Items + Measurements
2. **Create Work Tickets** → Production Stages → Task Assignments
3. **Quality Control** → Damage Incidents + Resolution
4. **Payments** → Payment records per order
5. **Delivery** → Mark orders as delivered

---

## Running Without Debug

For production:

```bash
DEBUG=False python manage.py runserver
```

Or set in `.env`:
```
DEBUG=False
```

---

## Troubleshooting

### Port 8000 already in use
```bash
python manage.py runserver 8080
```

### Database errors
```bash
python manage.py migrate --run-syncdb
```

### Reset database
```bash
rm db.sqlite3
python manage.py migrate
python manage.py seed_data
```
