# Backend Requirements Specification – ALX Airbnb Clone

This document outlines the detailed specifications for three core backend features: **User Authentication**, **Property Management**, and **Booking System**.

---

## 🔐 1. User Authentication

### ✅ Feature Description
Handles user registration, login, and authentication using secure methods (e.g., password hashing and JWT tokens).

### 📌 API Endpoints

| Method | Endpoint              | Description               |
|--------|------------------------|---------------------------|
| POST   | `/api/auth/register`  | Register a new user       |
| POST   | `/api/auth/login`     | Authenticate user         |
| GET    | `/api/auth/profile`   | Get authenticated user profile |

### 📥 Input Specifications

#### `POST /api/auth/register`
```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "password": "secret123",
  "role": "host"
}
```

### POST /api/auth/login
```json
{
  "email": "john@example.com",
  "password": "secret123"
}
```

📤 Output Examples
✅ Successful Login
```json
{
  "token": "jwt-token-here",
  "user": {
    "id": "uuid",
    "email": "john@example.com",
    "role": "host"
  }
}
```
❌ Failed Login
```json
{
  "error": "Invalid email or password"
}
```
✔️ Validation Rules
Email must be unique and properly formatted

Password must be at least 6 characters

Role must be one of: guest, host, admin

🚀 Performance Criteria
Response time: < 500ms

JWT expiry: 24 hours

Passwords stored using bcrypt/Argon2

Max 5 failed login attempts/hour (rate limit)

🏠 2. Property Management
✅ Feature Description
Hosts can create, update, and manage property listings.

📌 API Endpoints
Method	Endpoint	Description
POST	/api/properties	Create a new property listing
PUT	/api/properties/:id	Update an existing property
DELETE	/api/properties/:id	Delete a property
GET	/api/properties/:id	Get property details
GET	/api/properties	List all available properties

📥 Input Example (POST)
json
Copy
Edit
{
  "title": "Beachfront Apartment",
  "description": "Cozy and relaxing place by the ocean",
  "price_per_night": 90,
  "location": "Mombasa, Kenya",
  "amenities": ["WiFi", "Pool", "Kitchen"],
  "images": ["url1", "url2"]
}
📤 Output Example (GET)
json
Copy
Edit
{
  "id": "property-uuid",
  "title": "Beachfront Apartment",
  "price_per_night": 90,
  "host_id": "user-uuid",
  "available": true
}
✔️ Validation Rules
Title: max 100 characters

Price: must be > 0

Images: must be valid URLs

Only property owner (host) can update/delete

🚀 Performance Criteria
GET responses < 300ms

Paginated /properties endpoint

Indexed: location, price, available

🛎️ 3. Booking System
✅ Feature Description
Allows guests to book properties, check availability, and cancel bookings.

📌 API Endpoints
Method	Endpoint	Description
POST	/api/bookings	Create a booking
GET	/api/bookings/:id	Get booking details
DELETE	/api/bookings/:id	Cancel a booking
GET	/api/properties/:id/bookings	View bookings for a property

📥 Input Example (POST)
```json
{
  "property_id": "property-uuid",
  "guest_id": "user-uuid",
  "check_in": "2025-07-15",
  "check_out": "2025-07-18"
}
```
**📤 Output Example**
```json
{
  "booking_id": "booking-uuid",
  "property_id": "property-uuid",
  "status": "confirmed",
  "total_cost": 270,
  "created_at": "2025-06-28T12:00:00Z"
}
```
---

### ✔️ Validation Rules
- Check-in must be today or later

- Check-out must be after check-in

- No double booking for selected dates

- Guests can’t book their own properties

- Atomic operations: booking + payment must succeed together
---

### 🚀 Performance Criteria
Booking creation < 400ms

Database constraints to prevent double booking

Indexed fields: property_id, check_in, check_out

