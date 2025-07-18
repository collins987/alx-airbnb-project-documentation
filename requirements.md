# Backend Requirements for Key Features

## 1. User Authentication
- **Endpoints**
  - `POST /auth/register`: input `{email, password, first_name, last_name}` → output `{user, token}`.
  - `POST /auth/login`: input `{email, password}` → output `{token, userId}`.
- **Validation**: email format; password length ≥ 8; unique email.
- **Security**: passwords hashed (bcrypt), tokens expire after 1 h; rate limit 5/min/IP.
- **Performance**: response < 200 ms.

## 2. Property Management
- **Endpoints**
  - `POST /properties`: hosts supply metadata and image URLs → returns `{property_id}`.
  - `GET /properties`: supports filters (location, price_range, guests, amenities) + pagination.
  - `PUT /properties/{id}`, `DELETE /properties/{id}` (owner-only).
- **Validation**: price > 0, availability dates valid, image URL format.
- **Performance**: filter requests < 500 ms with Redis caching.

## 3. Booking System
- **Endpoints**
  - `POST /bookings`: body `{property_id, start_date, end_date}` → creates pending booking, blocks dates.
  - `GET /bookings/{id}`: returns booking details.
  - `PATCH /bookings/{id}/cancel`: allowed if >48 h before start.
- **Validation**: date overlap checks, cancellation policy enforced.
- **Performance**: availability check < 300 ms, proper DB indexing.
