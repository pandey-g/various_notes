Great! Let's go step-by-step and clearly explain how **JWT (JSON Web Token)** works in RESTful APIs:

---

## 🔐 What is JWT?

JWT stands for **JSON Web Token**.
It is a **compact, self-contained token** used to securely transmit information between the client and server as a **JSON object**.
It is **digitally signed**, so the server can **verify** it hasn’t been tampered with.

---

## 🔄 Typical JWT Flow in a REST API

### 🧭 1. **Client Logs In**

The client sends their credentials (e.g., email & password) to the server:

```http
POST /login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}
```

---

### ✅ 2. **Server Verifies Credentials & Issues JWT**

If credentials are valid:

* The server generates a **JWT**.
* The JWT includes **claims** (data like user ID or role).
* It is **signed** using a secret key (`HS256`) or private key (`RS256`).
* The server sends it back to the client.

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

---

### 🔐 3. **Client Stores the JWT**

The client stores the token securely:

* In **memory** (temporary)
* In **localStorage** or **sessionStorage**
* In **HttpOnly cookie** (more secure, prevents XSS)

---

### 📥 4. **Client Sends JWT with Each Request**

The client attaches the JWT to **every request** (usually in the `Authorization` header):

```http
GET /profile
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

### 🧠 5. **Server Verifies JWT**

* The server **doesn’t store the session**.
* It uses the secret key to **verify the JWT** signature.
* If valid, it reads the data (e.g., user ID, roles) from the token.
* It proceeds with the request logic.

---

### 🧼 6. **Token Expiry and Refresh**

* JWT usually includes an **expiration time (`exp`)**.
* After expiration, the client must **re-login** or use a **refresh token flow** (if implemented).

---

## 🔍 JWT Structure

A JWT has **three parts**, separated by dots (`.`):

```
<Header>.<Payload>.<Signature>
```

### Example:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.  ← Header
eyJ1c2VyX2lkIjoxMjMsInJvbGUiOiJhZG1pbiJ9. ← Payload
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c ← Signature
```

### Decoded:

* **Header**: algorithm and token type

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

* **Payload (Claims)**: the actual data

```json
{
  "user_id": 123,
  "role": "admin",
  "exp": 1712345678
}
```

* **Signature**: ensures data is untampered

---

## ✅ Advantages of JWT in REST

| Advantage           | Description                                               |
| ------------------- | --------------------------------------------------------- |
| 🔐 Stateless Auth   | Server does **not need to store** session info            |
| ⚡ Fast              | No DB lookup needed per request (only token verification) |
| 📦 Self-contained   | Carries all user info needed in the token itself          |
| 🌐 Widely Supported | Supported by all major web frameworks                     |

---

## 🚨 Caveats / Best Practices

* **Never store JWT in localStorage if possible** → use **HttpOnly cookies** to prevent XSS.
* **Always use HTTPS** to transmit JWT.
* Set short `exp` and use **refresh tokens** for long sessions.
* Don't put sensitive info (e.g., passwords) in the payload. It's **encoded, not encrypted**.

---

## 🧠 Analogy

Think of JWT like a **movie ticket**:

* It’s issued once at login (the box office).
* You show it every time (request to enter the theatre).
* The ticket is **signed** (barcode) to prevent forgery.
* The gatekeeper (server) scans and verifies it, then lets you in.

---

Would you like to see:

* A code example in FastAPI or Flask?
* How to verify JWT manually?
* How to implement refresh tokens?
