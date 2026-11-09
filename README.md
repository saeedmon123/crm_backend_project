# CRM Backend – Django & Django Ninja

A backend REST API for a **Customer Relationship Management (CRM)** system, built with **Django** and **Django Ninja**.
It manages customers, leads, interactions, orders, payments, subscriptions, loyalty programs and promotions, and includes
data-driven features such as **product recommendations**, **sales forecasting** and **dynamic pricing**.

Developed during my backend development internship at **Ciatek IT Services** (2024).

---

## Features

**Customer management**
- Customers, leads and customer profiles
- Customer feedback collection

**Interactions & follow-ups**
- Log every interaction with a customer (type, outcome, responsible user)
- Schedule and clear follow-ups
- Interaction analytics: counts by type, outcome and responsible user

**Sales**
- Products with inventory tracking and categories
- Orders, order items and payments
- Automatic low-stock email alerts (management command)

**Subscriptions & loyalty**
- Subscription plans, subscribed customers and active-subscription listing
- Subscription renewal
- Loyalty thresholds, loyalty points and point redemption

**Promotions**
- Category-based promotions with percentage or fixed discounts, date ranges and usage limits
- Promotion redemption tracking

**Data-driven features**
- **Product recommendations** based on a customer's purchase history (TF-IDF + cosine similarity, scikit-learn)
- **Sales forecasting** for the next 30 days per product category (ARIMA, statsmodels)
- **Dynamic pricing**: raises prices of products whose stock falls below a threshold

---

## Tech Stack

| Layer | Technologies |
|---|---|
| Framework | Django 5.1, Django Ninja, Django REST Framework |
| Validation | Pydantic schemas |
| Data / ML | scikit-learn, statsmodels, pandas, NumPy |
| Database | SQLite (development) |
| Tooling | django-extensions, Django admin |

---

## Project Structure

```
crm_backend_project/
├── crm_backend/
│   ├── crm/                     # Main CRM app
│   │   ├── models.py            # Data models
│   │   ├── schemas.py           # Pydantic request/response schemas
│   │   ├── api.py               # All API endpoints (Django Ninja)
│   │   ├── admin.py             # Django admin registration
│   │   ├── management/commands/
│   │   │   └── check_inventory.py   # Low-stock email alerts
│   │   └── test_*.py            # Unit tests
│   ├── crm_backend/             # Project settings and URLs
│   └── manage.py
├── requirements.txt
└── README.md
```

---

## API Overview

All endpoints are served under `/api/`. Interactive documentation (Swagger UI) is generated automatically by Django Ninja at:

```
http://127.0.0.1:8000/api/docs
```

| Area | Main endpoints |
|---|---|
| Customers | `GET /customers`, `POST /customers/`, `GET /customerprofiles`, `POST /profiles`, `PATCH /customers/{id}/loyalty`, `PATCH /customers/{id}/subscription` |
| Leads | `GET /leads` |
| Interactions | `GET /interactions`, `POST /interactions/`, `GET /interaction-analysis/`, `POST /followup/`, `DELETE /clearfollowup` |
| Products | `GET /products`, `GET /adjust-pricing/`, `GET /recommend-products` |
| Orders & payments | `GET /orders`, `POST /orders/`, `GET /orderitems`, `GET /payments`, `POST /payments/` |
| Subscriptions | `GET /subscriptions`, `POST /subscriptions/`, `GET /activesubscriptions/`, `PATCH /renewals/` |
| Loyalty | `GET /loyaltythresholds`, `POST /loyaltythresholds/`, `GET /loyaltymodels` |
| Promotions | `GET /promotions`, `POST /promotions/`, `GET /promotionredemptions` |
| Feedback | `GET /feedbacks`, `POST /feedbacks/` |
| Forecasting | `GET /sales_forecase?category=...` |

---

## Getting Started

### Prerequisites
- Python 3.10 or newer
- Git

### Installation

```bash
git clone https://github.com/saeedmon123/crm_backend_project.git
cd crm_backend_project

python -m venv venv
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate

pip install -r requirements.txt
```

### Environment variables (optional)

The project runs locally with safe defaults. For email alerts or deployment, set:

| Variable | Description | Default |
|---|---|---|
| `DJANGO_SECRET_KEY` | Django secret key | development key |
| `DJANGO_DEBUG` | `True` / `False` | `True` |
| `DJANGO_ALLOWED_HOSTS` | Comma-separated hosts | empty |
| `EMAIL_HOST_USER` | SMTP email address | empty |
| `EMAIL_HOST_PASSWORD` | SMTP password / app password | empty |

### Run the server

```bash
cd crm_backend
python manage.py migrate
python manage.py createsuperuser   # optional, for the admin panel
python manage.py runserver
```

- API docs: http://127.0.0.1:8000/api/docs
- Admin panel: http://127.0.0.1:8000/admin/

### Low-stock alerts

```bash
python manage.py check_inventory
```

Sends an email alert for every product with fewer than 10 items in stock.

### Run tests

```bash
python manage.py test crm
```

---

## Author

**Mohamad Said Monawar**
[GitHub](https://github.com/saeedmon123) · [LinkedIn](https://www.linkedin.com/in/mohamad-said-monawar-7a1145292/)
