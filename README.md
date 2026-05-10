# Auth-NodeJs

A Node.js authentication API built with Express, MongoDB, JWT, OTP email verification, and refresh token sessions.

## Features

- User registration with email verification via OTP
- Secure login with JWT access token and refresh token cookie
- Refresh access tokens using a stored refresh token session
- Logout from current session and all devices
- Protected user profile endpoint (`/api/auth/get-me`)
- Gmail OAuth2 email sending for OTP delivery

## Tech stack

- Node.js
- Express
- MongoDB / Mongoose
- JSON Web Tokens (`jsonwebtoken`)
- Nodemailer with Gmail OAuth2
- Cookie-based refresh tokens
- Morgan request logging

## Getting started

### Prerequisites

- Node.js 18+ installed
- MongoDB database available
- Gmail account with OAuth2 credentials

### Install dependencies

```bash
npm install
```

### Environment variables

Create a `.env` file at the project root with the following values:

```env
MONGO_URI=your-mongodb-uri
JWT_SECRET=your-jwt-secret
GOOGLE_CLIENT_ID=your-google-client-id
GOOGLE_CLIENT_SECRET=your-google-client-secret
GOOGLE_REFRESH_TOKEN=your-google-refresh-token
GOOGLE_USER=your-google-email@example.com
```

### Run the app

```bash
npm run dev
```

The server listens on `http://localhost:3000`.

## API endpoints

Base path: `/api/auth`

### Register user

- `POST /api/auth/register`
- Body:
  - `username` (string)
  - `email` (string)
  - `password` (string)

Returns a success message and sends an OTP email for verification.

### Verify email

- `GET /api/auth/verify-email`
- Body:
  - `email` (string)
  - `otp` (string)

Marks the user as verified if the OTP matches.

### Login

- `POST /api/auth/login`
- Body:
  - `email` (string)
  - `password` (string)

Returns an access token and sets a secure `refreshToken` cookie.

### Get current user

- `GET /api/auth/get-me`
- Headers:
  - `Authorization: Bearer <accessToken>`

Returns the authenticated user's basic profile.

### Refresh token

- `GET /api/auth/refresh-token`

Uses the refresh token cookie to return a new access token and rotate the refresh token.

### Logout current session

- `GET /api/auth/logout`

Revokes the current refresh token session and clears the cookie.

### Logout from all devices

- `GET /api/auth/logout-all`

Revokes all active sessions for the authenticated user and clears the cookie.

## Project structure

- `server.js` — app entry point
- `src/app.js` — Express app configuration
- `src/config/` — environment and database configuration
- `src/controllers/` — authentication business logic
- `src/models/` — Mongoose schemas for users, sessions, and OTPs
- `src/routes/` — auth routes
- `src/services/` — email service implementation
- `src/utils/` — utility functions for OTP generation and templates

## Notes

- Passwords are hashed using SHA-256 before storage.
- Email verification is required before login.
- Refresh tokens are stored as hashed session records for security.
- Cookies are configured with `httpOnly`, `secure`, and `sameSite: "strict"`.
