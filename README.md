# Sharp Society — Barber Booking System

A full-stack barber appointment booking web application built with React and Node.js. Customers can browse barbers and services, book appointments, and manage their bookings. Admins can manage availability slots through a dedicated panel.

---

## Screenshots

| | |
|---|---|
| ![Login](Documentation/Login.png) | ![Register](Documentation/Register%20User.png) |
| ![Services](Documentation/Services%20works%20100%25.png) | ![Barbers](Documentation/Get%20Barbers.png) |
| ![User Dashboard](Documentation/Test%201_User%20Display.png) | ![Appointments](Documentation/Get%20My%20Appointments.png) |

---

## Tech Stack

**Frontend**
- React 19 + Vite
- Material UI (MUI) v7
- Framer Motion
- React Router DOM v7
- Axios

**Backend**
- Node.js + Express 5
- MySQL 8 + mysql2
- JWT Authentication
- bcryptjs
- express-validator

---

## Features

- **Authentication** — Register, login, and protected routes with JWT
- **Services Catalogue** — Browse all available services with pricing and duration
- **Barbers** — View barber profiles, specialties, experience, and ratings
- **Booking Flow** — Select barber → service → date/time → confirm
- **My Bookings** — Customers can view and track their appointments
- **Admin Panel** — Manage available time slots
- **Role-based Access** — Separate views and permissions for customers and admins

---

## Project Structure

```
B8IT146-BarberBookingSystem/
├── backend/
│   ├── config/
│   │   └── database.js        # MySQL connection pool
│   ├── middleware/
│   │   └── auth.js            # JWT middleware + role guard
│   ├── routes/
│   │   ├── auth.js            # Register, Login, /me
│   │   ├── barbers.js         # Barber CRUD
│   │   ├── services.js        # Service CRUD
│   │   └── appointments.js    # Appointment booking
│   ├── .env.example
│   └── server.js
├── frontend/
│   ├── src/
│   │   ├── api/               # Axios service modules
│   │   ├── components/        # Navbar, Hero, Logo
│   │   ├── context/           # AuthContext
│   │   ├── layouts/           # AppLayout
│   │   ├── pages/             # All route pages
│   │   ├── theme/             # MUI ThemeProvider
│   │   └── brand/             # Brand config
│   └── vite.config.js
├── Database/
│   └── schema.sql             # Full MySQL schema + seed data
├── Documentation/
│   ├── ERD.md
│   └── Barber_Booking_API.postman_collection.json
└── Testing/
```

---

## Getting Started

### Prerequisites

- Node.js 18+
- MySQL 8

### 1. Database Setup

```bash
mysql -u root -p < Database/schema.sql
```

This creates the `barber_booking` database, all tables, indexes, and seeds barbers and services. One admin account is created — credentials are set in your `.env`.

### 2. Backend

```bash
cd backend
cp .env.example .env
# Edit .env with your MySQL credentials and JWT secret
npm install
npm run dev
```

Backend runs on `http://localhost:5000`

### 3. Frontend

```bash
cd frontend
npm install
npm run dev
```

Frontend runs on `http://localhost:5173`

---

## Environment Variables

Create `backend/.env` based on `.env.example`:

```env
DB_HOST=localhost
DB_PORT=3306
DB_NAME=barber_booking
DB_USER=your_mysql_user
DB_PASSWORD=your_mysql_password
JWT_SECRET=your_jwt_secret
JWT_EXPIRE=7d
PORT=5000
NODE_ENV=development
CORS_ORIGIN=http://localhost:5173
```

---

## API Endpoints

### Auth
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/auth/register` | Public | Register a new customer |
| POST | `/api/auth/login` | Public | Login and receive JWT |
| GET | `/api/auth/me` | Protected | Get current user info |

### Barbers
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | `/api/barbers` | Public | List all barbers |
| GET | `/api/barbers/:id` | Public | Get barber by ID |
| POST | `/api/barbers` | Admin | Create barber |
| PUT | `/api/barbers/:id` | Admin | Update barber |
| DELETE | `/api/barbers/:id` | Admin | Delete barber |

### Services
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| GET | `/api/services` | Public | List all services |
| GET | `/api/services/:id` | Public | Get service by ID |
| POST | `/api/services` | Admin | Create service |
| PUT | `/api/services/:id` | Admin | Update service |
| DELETE | `/api/services/:id` | Admin | Delete service |

### Appointments
| Method | Endpoint | Access | Description |
|--------|----------|--------|-------------|
| POST | `/api/appointments` | Customer | Book an appointment |
| GET | `/api/appointments/my` | Customer | View own appointments |
| GET | `/api/appointments` | Admin | View all appointments |
| PUT | `/api/appointments/:id` | Admin | Update appointment status |
| DELETE | `/api/appointments/:id` | Admin | Cancel appointment |

---

## Database Schema

Four tables with full referential integrity:

```
USERS ──────────────────────────────────────────┐
  id, username, name, email, password,           │
  phone, role (customer|admin)                   │
                                                 │ 1:N
BARBERS ─────────────────────────────────────┐   │
  id, name, specialty, experience_years,     │   │
  bio, image_url, rating, available          │   │
                                             │   ▼
                                        APPOINTMENTS
                                          id, user_id (FK)
                                          barber_id (FK)
                                          service_id (FK)
                                          appointment_date
                                          appointment_time
                                          status (pending|confirmed|
                                                  completed|cancelled)
                                          notes
SERVICES ────────────────────────────────────────┘
  id, name, description, price,           ▲
  duration (mins), image_url, active      │ 1:N
```

All foreign keys use `ON DELETE CASCADE`.

See [Documentation/ERD.md](Documentation/ERD.md) for the full diagram.

---

## Postman Collection

Import `Documentation/Barber_Booking_API.postman_collection.json` into Postman to test all API endpoints. After logging in, copy the returned token and set it as a Bearer Token in the Authorization tab of protected requests.

---

## Academic Context

Developed as the Continuous Assessment (CA) for module **B8IT146 — Database Systems** at **Dublin Business School**. The project demonstrates relational database design, RESTful API development, and a complete full-stack implementation.
