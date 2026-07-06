# Car Rental Web Application — Project Thesis

## Abstract

This project implements a full-stack Car Rental web application providing a responsive frontend and a RESTful backend for car listing, booking management, user authentication, and owner administration. The system is designed to enable customers to browse and book cars while enabling car owners to manage listings and bookings. It demonstrates practical use of modern web technologies, image handling, authentication, and basic deployment practices.

## Introduction

Car rental services benefit from an online platform that centralizes vehicle listings, availability, pricing, and booking. This project (referred to as CarRental) seeks to build a minimally viable yet production-structured application that supports two primary user roles: customers and owners. Customers can search, view details, and book cars; owners can add/manage cars and view/manage bookings.

## Objectives

- Implement a responsive, accessible frontend using a modern UI toolchain.
- Build a secure backend with authentication, role-based access, and data persistence.
- Support image uploads and optimized delivery for car photos.
- Provide booking lifecycle management (create, read, update, cancel).
- Prepare the application for easy local development and deployment (Vercel/Node hosting).

## Tech Stack

- Frontend: React (Vite), JSX components, CSS for styling, client-side routing.
- Backend: Node.js + Express for REST APIs.
- Database: MongoDB (document models for `Car`, `Booking`, `User`).
- File/Image handling: ImageKit integration (or middleware) for upload and CDN delivery.
- Authentication: JWT-based auth with middleware to protect routes and role checks.
- Deployment: Vercel for frontend, Node hosting or similar for backend (server.js present).

## System Architecture

The project follows a client-server architecture:

- Client (client/): React SPA that consumes backend APIs, handles user sessions and UI interactions. Components are organized by feature and role (owner subfolder for owner-specific UI).
- Server (server/): Express server exposing endpoints grouped by feature (`routes/` for users, bookings, owners). Controllers implement business logic and interact with MongoDB via Mongoose models.
- Shared concerns: `configs/` contains database and image service configuration; `middleware/` provides authentication and file handling (e.g., `multer.js`).

A typical flow: the frontend sends authenticated requests to the backend endpoints; the backend validates tokens, performs DB operations, and returns JSON responses. Image uploads are proxied to ImageKit or stored and served via a CDN.

## Data Models

- User
  - Fields: name, email, passwordHash, role (customer/owner), contact info
  - Responsibilities: register, login, auth token issuance, profile management

- Car
  - Fields: ownerId, make/model, year, description, pricePerDay, images[], availability metadata, location
  - Responsibilities: listing details, images, availability checks

- Booking
  - Fields: userId, carId, startDate, endDate, totalPrice, status (pending/confirmed/cancelled), timestamps
  - Responsibilities: booking lifecycle, status updates, basic conflict/availability checks

## Backend Endpoints (Selected)

- `POST /api/users/register` — Create user account
- `POST /api/users/login` — Authenticate and return JWT
- `GET /api/cars` — List/search cars
- `GET /api/cars/:id` — Car details
- `POST /api/owners/cars` — Owner creates a car listing (protected)
- `PUT /api/owners/cars/:id` — Owner updates a listing (protected)
- `GET /api/bookings` — Retrieve bookings (user or owner context)
- `POST /api/bookings` — Create a booking

Middleware ensures proper authorization and input validation where appropriate.

## Frontend Features

- Responsive layout with a `Navbar`, `Hero`, `FeaturedSection`, `CarCard` listings, and `CarDetails` pages.
- Owner dashboard with `AddCar`, `ManageCars`, `ManageBookings` views.
- Login and registration components with client-side validation and token storage (localStorage or context).
- State management via React Context (`context/AppContext.jsx`) for global app state like current user and auth token.

## Authentication & Security

- Passwords are hashed on the server before persistence.
- JWTs are used for stateless session management; protected routes require a valid token.
- Owner-only endpoints are gated via role checks in `auth` middleware.
- Basic input validation is applied; CSRF is not relevant for pure API + SPA JWT flows but should be considered for cookie-based auth.

## Image Handling

- Server integrates with ImageKit (see `configs/imageKit.js`) or uses `multer` for multipart handling and then uploads to a CDN.
- Images are stored as URLs on `Car.images[]` for fast client delivery.

## Development & Deployment

- Local development: run the backend (`node server/server.js`) and the frontend (`npm run dev` in `client/`). Environment variables manage DB URLs, JWT secrets, and ImageKit credentials.
- Deployment: the `client` folder is configured for Vercel (see `client/vercel.json`), with `server` deployable to a Node-compatible host. Consider using a unified deployment (serverless functions or separate services) in production.

## Installation & Run (Quick)

1. Install dependencies for both client and server:

```bash
# from project root
cd server
npm install
cd ../client
npm install
```

2. Start development servers:

```bash
# backend
cd server
node server.js
# frontend
cd client
npm run dev
```

3. Ensure environment variables are set: `MONGODB_URI`, `JWT_SECRET`, `IMAGEKIT_PRIVATE_KEY`, etc.

## Testing Strategy

- Manual testing flows: user signup/login, add car (owner), create booking, cancel booking, image upload.
- Unit tests are not included by default; add tests for controllers and critical utilities using Jest or Mocha.
- End-to-end tests can be added with Playwright or Cypress to exercise SPA + API flows.

## Limitations

- No payment gateway integration (bookings do not process payments).
- Concurrency/availability checks are basic; high-volume scenarios require transactional checks or a reservation system.
- No email notifications or advanced user verification.
- Security hardening (rate limiting, input sanitization, helmet, CORS policies tuning) should be added for production.

## Future Work

- Integrate payments (Stripe/PayPal) and booking confirmations.
- Add search filters (location, price range, vehicle type) and map-based browsing.
- Improve owner analytics and reporting (earnings, occupancy rates).
- Add role-based dashboards, multi-language support, and better accessibility compliance.

## Conclusion

This CarRental project is a practical full-stack application demonstrating how to build a modern, role-based web service with React and Node.js. It provides a solid foundation to extend into a production-grade system by adding payments, better testing, and deployment pipelines.

## References

- React, Vite documentation
- Express and Mongoose documentation
- ImageKit/docs or alternative image CDN docs

---

*Generated: brief project thesis for the CarRental application.*
