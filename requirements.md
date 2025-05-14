# Airbnb Clone - Technical & Functional Requirements

This document outlines the functional and technical specifications for three core backend features of the Airbnb Clone: **User Authentication**, **Property Management**, and the **Booking System**.

---

## 1. User Authentication

### Functional Requirements
- Users can register with personal details (name, email, password).
- Users can log in using email and password.
- Authenticated users can access protected routes.
- Passwords must be securely hashed.
- Token-based authentication (JWT) should be implemented.

### API Endpoints

| Method | Endpoint         | Description                      |
|--------|------------------|----------------------------------|
| POST   | `/api/register`  | Register new user                |
| POST   | `/api/login`     | Authenticate user                |
| GET    | `/api/profile`   | Get user profile (auth required) |

### Input/Output Example

**Register Input**
let's take a simple input example
```json
{
  "first_name": "Thierno",
  "last_name": "Diallo",
  "email": "thierno@example.com",
  "password": "Diallo00",
  "phone_number": "+224625213865"
}
```
**Register Output**
```json
{
  "message": "User registered successfully",
}
```

### Validation Rules
- Email must be valid and unique.
- Password must be at least 8 characters (letters, symbols and numbers).
- Required fields: first_name, last_name, email, password.

### Performance Criteria
- Registration and login responses < 500ms or nearly.
- Rate-limiting for login <= 2 times to prevent brute-force attacks.

---

## 2. Property Management

### Functional Requirements
- Hosts can add new properties.
- Hosts can update or delete their own properties.
- Users can browse all properties.
- Properties must be linked to the host (user_id).

### API Endpoints

| Method | Endpoint               | Description                     |
|--------|------------------------|---------------------------------|
| POST   | `/api/properties`      | Create new property (host only) |
| GET    | `/api/properties`      | List all properties             |
| GET    | `/api/properties/:id`  | Get property details            |
| PUT    | `/api/properties/:id`  | Update property (host only)     |
| DELETE | `/api/properties/:id`  | Delete property (host only)     |

### 📤 Input/Output Example

**Create Property Input**
```json
{
  "name": "Beachside Villa",
  "description": "Ocean view, 2 bedrooms",
  "location": "Los Angeles, CA",
  "price_per_night": 150.00
}
```

**Create Property Output**
```json
{
  "message": "Property listed successfully",
  "property_id": "prop-001"
}
```

### Validation Rules
- Name, location, and price_per_night are required.
- Price must be a positive decimal value.

### Performance Criteria
- Property listing should load within 1s or 2 at max.
- Support pagination and search filtering (location, price).

---

## 3. Booking System

###  Functional Requirements
- Users can book available properties.
- Overlapping bookings must be prevented.
- Users can view their booking history.
- Hosts can view bookings for their properties.

### API Endpoints

| Method | Endpoint               | Description               |
|--------|------------------------|---------------------------|
| POST   | `/api/bookings`        | Create a new booking      |
| GET    | `/api/bookings`        | Get bookings (auth user)  |
| GET    | `/api/bookings/:id`    | Get booking by ID         |

### Input/Output Example

**Booking Input**
```json
{
  "property_id": "prop-001",
  "start_date": "2025-06-01",
  "end_date": "2025-06-05"
}
```

**Booking Output**
```json
{
  "message": "Booking confirmed",
  "booking_id": "book-001"
}
```

### Validation Rules
- Start date must be before end date.
- Property must not be booked for overlapping dates.
- Authenticated user required.

### Performance Criteria
- Check availability in < 300ms.
- Prevent duplicate concurrent bookings using transactions or locks.

---