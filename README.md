# Airbnb Backend API (Spring Boot)

Backend REST APIs for an Airbnb-like hotel management and booking system: inventory management, hotel browsing, booking flow, user authentication, profile management, and Stripe payments.

## Tech stack

- **Java**: 23
- **Framework**: Spring Boot 3.x (Web, Data JPA, Security)
- **Database**: PostgreSQL
- **Auth**: JWT (JJWT)
- **Payments**: Stripe
- **API docs**: Swagger UI (Springdoc OpenAPI)
- **Build**: Maven (with Maven Wrapper)
- **Container**: Docker

## Project structure

```text
.
├─ pom.xml
├─ mvnw / mvnw.cmd
├─ .mvn/
├─ Dockerfile
└─ src/
   ├─ main/
   │  ├─ java/com/aman/AirBnb/AirBnb/
   │  │  ├─ Advice/
   │  │  ├─ Config/
   │  │  ├─ Controller/
   │  │  ├─ Dto/
   │  │  ├─ Entities/
   │  │  ├─ Enums/
   │  │  ├─ Exceptions/
   │  │  ├─ Repositories/
   │  │  ├─ Security/
   │  │  ├─ Service/
   │  │  ├─ Strategy/
   │  │  ├─ Utils/
   │  │  └─ AirBnbApplication.java
   │  └─ resources/
   │     └─ application.properties
   └─ test/
      └─ java/...
```

## Configuration (environment variables)

This project is configured via environment variables referenced in `src/main/resources/application.properties`.

Set these before running:

- **DB_URL**: JDBC URL (example: `jdbc:postgresql://localhost:5432/airbnb`)
- **DB_USERNAME**
- **DB_PASSWORD**
- **JWT_SECRET**: secret used to sign JWT tokens
- **FRONTEND_URL**: allowed frontend origin / URL used by the backend (CORS / redirects depending on implementation)
- **STRIPE_SECRET_KEY**
- **STRIPE_WEBHOOK_SECRET**

Notes:
- Do **not** commit real secrets to GitHub.
- The API base path is **`/api/v1`** and default port is **`8080`**.

## Run locally (recommended for development)

### Prerequisites

- Java 23 installed (or a compatible runtime configured for this project)
- PostgreSQL running

### Start the app

From the project root:

```bash
./mvnw spring-boot:run
```

On Windows (PowerShell):

```powershell
.\mvnw.cmd spring-boot:run
```

The API will be available at:
- **Base URL**: `http://localhost:8080/api/v1`

## Run with Docker

This repo includes a `Dockerfile` that builds the app inside the container.

Build:

```bash
docker build -t airbnb-backend .
```

Run (pass environment variables to the container):

```bash
docker run --rm -p 8080:8080 ^
  -e DB_URL="jdbc:postgresql://host.docker.internal:5432/airbnb" ^
  -e DB_USERNAME="postgres" ^
  -e DB_PASSWORD="password" ^
  -e JWT_SECRET="change-me" ^
  -e FRONTEND_URL="http://localhost:3000" ^
  -e STRIPE_SECRET_KEY="sk_test_..." ^
  -e STRIPE_WEBHOOK_SECRET="whsec_..." ^
  airbnb-backend
```

Tip (Windows + Docker Desktop):
- Use `host.docker.internal` to reach Postgres running on your host machine.

## API documentation (Swagger UI)

When the app is running, open Swagger UI:
- `http://localhost:8080/api/v1/swagger-ui/index.html`

## API overview

All endpoints below are relative to **`/api/v1`**.

### Admin Inventory
- **GET** `/admin/inventory/rooms/{roomId}` - Retrieve inventory of a room
- **PATCH** `/admin/inventory/rooms/{roomId}` - Update inventory for a room
- **PUT** `/admin/hotels/{hotelId}/rooms/{roomId}` - Update a room

### Booking Flow
- **GET** `/bookings/{bookingId}/status` - Check booking status
- **GET** `/admin/hotels/{hotelId}/reports` - Generate hotel booking report
- **GET** `/admin/hotels/{hotelId}/bookings` - Get all bookings
- **POST** `/bookings/{bookingId}/payments` - Initiate payment
- **POST** `/bookings/{bookingId}/cancel` - Cancel a booking
- **POST** `/bookings/{bookingId}/addGuests` - Add guests to a booking
- **POST** `/bookings/init` - Initialize a new booking

### Booking Guests
- **DELETE** `/users/guests/{guestId}` - Remove a guest
- **GET** `/users/guests` - Get my guests
- **POST** `/users/guests` - Add a guest
- **PUT** `/users/guests/{guestId}` - Update a guest

### Hotel Browse
- **GET** `/hotels/{hotelId}/info` - Get hotel details
- **GET** `/hotels/search` - Search for hotels

### Hotel Management
- **DELETE** `/admin/hotels/{hotelId}` - Delete a hotel
- **GET** `/admin/hotels/{hotelId}` - Get hotel by ID
- **GET** `/admin/hotels` - Get all admin hotels
- **PATCH** `/admin/hotels/{hotelId}/activate` - Activate a hotel
- **POST** `/admin/hotels` - Create a hotel
- **PUT** `/admin/hotels/{hotelId}` - Update hotel details

### Room Admin Management
- **DELETE** `/admin/hotels/{hotelId}/rooms/{roomId}` - Delete a room
- **GET** `/admin/hotels/{hotelId}/rooms/{roomId}` - Get room details
- **GET** `/admin/hotels/{hotelId}/rooms` - Retrieve all rooms
- **POST** `/admin/hotels/{hotelId}/rooms` - Create a room
- **PUT** `/admin/hotels/{hotelId}/rooms/{roomId}` - Update a room

### User Authentication
- **POST** `/auth/signup` - User signup
- **POST** `/auth/refresh` - Refresh access token
- **POST** `/auth/login` - User login

### User Profile
- **GET** `/users/profile` - Get my profile
- **PATCH** `/users/profile` - Update my profile
- **GET** `/users/myBookings` - Get my bookings
- **GET** `/users/guests` - Get my guests
- **POST** `/users/guests` - Add a guest
- **PUT** `/users/guests/{guestId}` - Update a guest
- **DELETE** `/users/guests/{guestId}` - Remove a guest

### Webhook
- **POST** `/webhook/payment` - Stripe webhook to capture payments

## Database schema

![Schema](https://github.com/user-attachments/assets/bc209296-e0f2-48f9-a7ae-65d084e4cb6c)

