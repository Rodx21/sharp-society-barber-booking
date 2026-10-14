# Sharp Society — Barber Booking System

> Full-stack appointment booking platform for barbershops. Customers browse barbers and services, book appointments, and manage their schedule. Admins control availability and appointment status through a dedicated panel.

-----

## Tech Stack

|Layer     |Technology                                                         |
|----------|-------------------------------------------------------------------|
|Frontend  |React 19, Vite, Material UI v7, Framer Motion, React Router DOM v7 |
|Backend   |Node.js, Express 5, JWT Authentication, bcryptjs, express-validator|
|Database  |MySQL 8, relational schema with FK constraints and seed data       |
|API Client|Axios                                                              |
|Testing   |Postman collection (full coverage)                                 |

-----

## Features

- **Auth** — Register, login, JWT-protected routes, persistent session
- **Role-based access** — Separate views and permissions for `customer` and `admin`
- **Services Catalogue** — Browse services with name, description, price, and duration
- **Barbers** — View profiles, specialties, experience years, and ratings
- **Booking Flow** — Select barber → service → date/time → confirm
- **My Bookings** — Customers view and track their own appointments
- **Admin Panel** — Manage available time slots and update appointment statuses (pending / confirmed / completed / cancelled)

-----

## Screenshots

|Login                            |Register                                      |
|---------------------------------|----------------------------------------------|
|![Login](Documentation/Login.png)|![Register](Documentation/Register%20User.png)|

|Services                                                |Barbers                                    |
|--------------------------------------------------------|-------------------------------------------|
|![Services](Documentation/Services%20works%20100%25.png)|![Barbers](Documentation/Get%20Barbers.png)|

|User Dashboard                                         |My Appointments                                           |
|-------------------------------------------------------|----------------------------------------------------------|
|![Dashboard](Documentation/Test%201_User%20Display.png)|![Appointments](Documentation/Get%20My%20Appointments.png)|

-----

## Project Structure

```
sharp-society-barber-booking/
├── backend/
│   ├── config/
│   │   └── database.js         # MySQL connection pool
│   ├── middleware/
│   │   └── auth.js             # JWT middleware + role guard
│   ├── routes/
│   │   ├── auth.js             # Register · Login · /me
│   │   ├── barbers.js          # Barber CRUD
│   │   ├── services.js         # Service CRUD
│   │   └── appointments.js     # Appointment booking & management
│   ├── .env.example
│   └── server.js
├── frontend/
│   ├── src/
│   │   ├── api/                # Axios service modules
│   │   ├── components/         # Navbar, Hero, Logo
│   │   ├── context/            # AuthContext (JWT session)
│   │   ├── layouts/            # AppLayout
│   │   ├── pages/              # All route pages
│   │   ├── theme/              # MUI ThemeProvider
│   │   └── brand/              # Brand config
│   └── vite.config.js
├── Database/
│   └── schema.sql              # Full MySQL schema + seed data
├── Documentation/
│   ├── ERD.md
│   └── Barber_Booking_API.postman_collection.json
└── Testing/
```

-----

## API Reference

### Auth

|Method|Endpoint            |Access   |Description          |
|------|--------------------|---------|---------------------|
|POST  |`/api/auth/register`|Public   |Register new customer|
|POST  |`/api/auth/login`   |Public   |Login — returns JWT  |
|GET   |`/api/auth/me`      |Protected|Get current user     |

### Barbers

|Method|Endpoint          |Access|Description     |
|------|------------------|------|----------------|
|GET   |`/api/barbers`    |Public|List all barbers|
|GET   |`/api/barbers/:id`|Public|Get barber by ID|
|POST  |`/api/barbers`    |Admin |Create barber   |
|PUT   |`/api/barbers/:id`|Admin |Update barber   |
|DELETE|`/api/barbers/:id`|Admin |Delete barber   |

### Services

|Method|Endpoint           |Access|Description      |
|------|-------------------|------|-----------------|
|GET   |`/api/services`    |Public|List all services|
|GET   |`/api/services/:id`|Public|Get service by ID|
|POST  |`/api/services`    |Admin |Create service   |
|PUT   |`/api/services/:id`|Admin |Update service   |
|DELETE|`/api/services/:id`|Admin |Delete service   |

### Appointments

|Method|Endpoint               |Access  |Description          |
|------|-----------------------|--------|---------------------|
|POST  |`/api/appointments`    |Customer|Book an appointment  |
|GET   |`/api/appointments/my` |Customer|View own appointments|
|GET   |`/api/appointments`    |Admin   |View all appointments|
|PUT   |`/api/appointments/:id`|Admin   |Update status        |
|DELETE|`/api/appointments/:id`|Admin   |Cancel appointment   |


> Full Postman collection available at `Documentation/Barber_Booking_API.postman_collection.json`.  
> After login, set the returned token as a **Bearer Token** in the Authorization header.

-----

## Database Schema

Four tables with full referential integrity and `ON DELETE CASCADE` on all foreign keys.

```
USERS
  id · username · name · email · password · phone · role (customer | admin)

BARBERS
  id · name · specialty · experience_years · bio · image_url · rating · available

SERVICES
  id · name · description · price · duration (mins) · image_url · active

APPOINTMENTS
  id · user_id (FK) · barber_id (FK) · service_id (FK)
  appointment_date · appointment_time
  status (pending | confirmed | completed | cancelled)
  notes
```

See <Documentation/ERD.md> for the full entity relationship diagram.

-----

## Getting Started

### Prerequisites

- Node.js 18+
- MySQL 8

### 1. Database

```bash
mysql -u root -p < Database/schema.sql
```

Creates the `barber_booking` database, all tables, indexes, and seeds barbers, services, and one admin account.

### 2. Backend

```bash
cd backend
cp .env.example .env
# Fill in your MySQL credentials and JWT secret
npm install
npm run dev
# Runs on http://localhost:5000
```

### 3. Frontend

```bash
cd frontend
npm install
npm run dev
# Runs on http://localhost:5173
```

### Environment Variables

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

-----

## Roadmap

- [ ] Deploy frontend to Vercel + backend to Railway
- [ ] Email confirmations via Nodemailer
- [ ] Real-time slot availability with Socket.io
- [ ] Stripe payment integration
- [ ] Mobile-responsive polish pass