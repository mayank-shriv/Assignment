# 🔐 The Silent Server - Authentication Debugging Assignment

**Completed by:** Mayank  
**Email:** shrivastavamayank012@gmail.com  
**Date:** February 13, 2026

A debugging challenge requiring fixing 6 intentional bugs in a Node.js authentication flow (Login → OTP → JWT → Protected Routes) to obtain a unique success flag.

---

## 🎯 Success Flag

```
FLAG-c2hyaXZhc3RhdmFtYXlhbmswMTJAZ21haWwuY29tX0NPTVBMRVRFRF9BU1NJR05NRU5U
```

**Decoded:** `shrivastavamayank012@gmail.com_COMPLETED_ASSIGNMENT`

---

## ✅ Assignment Completion Status

### All Tasks Completed Successfully:

**📬 Postman Workspace:**  
[🔗 View All API Tests & Complete Documentation](https://shrivastavamayank01922-3188860.postman.co/workspace/Mayank-Shrivastava's-Workspace~96efe92f-0079-490b-825d-c7118c8df272/collection/51953107-c8b9a920-1102-4d3a-9a8d-135f61a9e43e?action=share&source=copy-link&creator=51953107)

> [!NOTE]
> The Postman workspace contains all 4 API requests with saved examples, test results, and detailed documentation for each endpoint.

- ✅ **Task 1:** Login endpoint (`/auth/login`) - Working
- ✅ **Task 2:** OTP verification (`/auth/verify-otp`) - Working
- ✅ **Task 3:** JWT token generation (`/auth/token`) - Working
- ✅ **Task 4:** Protected route access (`/protected`) - Working

---

## 🚀 Quick Start

```bash
npm install
npm start
```

Server runs at: `http://localhost:3000`

---

## 🔄 Authentication Flow

```
1. Login (POST /auth/login)
   ↓
   Input: email + password
   Output: loginSessionId + OTP (logged to console)

2. Verify OTP (POST /auth/verify-otp)
   ↓
   Input: loginSessionId + OTP
   Output: Success message + session_token cookie

3. Get JWT (POST /auth/token)
   ↓
   Input: session_token cookie (automatic)
   Output: JWT access_token

4. Access Protected Route (GET /protected)
   ↓
   Input: JWT in Authorization header
   Output: User data + Success Flag
```

---

## 🐛 Bugs Identified and Fixed

### Bug #1: Logger Middleware Missing `next()`
**File:** `middleware/logger.js` | **Line:** ~8  
**Issue:** Middleware didn't call `next()`, blocking all requests  
**Fix:** Added `next()` call to pass control to subsequent middleware

```diff
-app.use((req, res) => {
+app.use((req, res, next) => {
   console.log(`${req.method} ${req.path}`);
+  next();
 });
```

---

### Bug #2: Incorrect OTP Logging
**File:** `server.js` | **Line:** 53  
**Issue:** Logged sessionId instead of actual OTP value  
**Fix:** Changed variable from `sessionId` to `otp`

```diff
-console.log(`[OTP] Session ${sessionId} generated with OTP: ${sessionId}`);
+console.log(`[OTP] Session ${sessionId} generated with OTP: ${otp}`);
```

---

### Bug #3: Missing Cookie Parser Middleware
**File:** `server.js` | **Line:** 18  
**Issue:** Express couldn't parse cookies without cookie-parser middleware  
**Fix:** Added cookie-parser middleware

```diff
 app.use(express.json());
+app.use(cookieParser());
```

---

### Bug #4: Token Endpoint Reading Wrong Source
**File:** `server.js` | **Line:** 113  
**Issue:** Endpoint tried to read from Authorization header instead of cookies  
**Fix:** Changed to read `session_token` from cookies

```diff
-const token = req.headers.authorization?.split(' ')[1];
+const sessionToken = req.cookies.session_token;
```

---

### Bug #5: Incorrect Variable Name in JWT Payload
**File:** `server.js` | **Line:** 133  
**Issue:** Variable `token` didn't exist, should be `sessionToken`  
**Fix:** Updated variable name

```diff
 const payload = { 
   email: session.email, 
-  sessionId: token
+  sessionId: sessionToken
 };
```

---

### Bug #6: Auth Middleware Missing `next()`
**File:** `middleware/auth.js` | **Line:** ~14  
**Issue:** Middleware didn't call `next()` after successful JWT validation  
**Fix:** Added `next()` call

```diff
 if (decoded) {
   req.user = decoded;
+  next();
 }
```

---

## 🧪 Test Results

### Task 1: Login
```json
POST /auth/login
Request:
{
  "email": "shrivastavamayank012@gmail.com",
  "password": "password123"
}

Response (200 OK):
{
  "message": "OTP sent",
  "loginSessionId": "fifddp"
}

Server Log: [OTP] Session fifddp generated with OTP: 548838
```

### Task 2: Verify OTP
```json
POST /auth/verify-otp
Request:
{
  "loginSessionId": "fifddp",
  "otp": "548838"
}

Response (200 OK):
{
  "message": "OTP verified",
  "sessionId": "fifddp"
}

Cookie Set: session_token=fifddp
```

### Task 3: Get JWT Token
```json
POST /auth/token
Request: (Cookie automatically sent)

Response (200 OK):
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "expires_in": 900
}
```

### Task 4: Access Protected Route
```json
GET /protected
Request Header: Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...

Response (200 OK):
{
  "message": "Access granted",
  "user": {
    "email": "shrivastavamayank012@gmail.com",
    "sessionId": "fifddp",
    "iat": 1770962401,
    "exp": 1770963301
  },
  "success_flag": "FLAG-c2hyaXZhc3RhdmFtYXlhbmswMTJAZ21haWwuY29tX0NPTVBMRVRFRF9BU1NJR05NRU5U"
}
```

---

## 🛠️ Technologies Used

- **Node.js** - Runtime environment
- **Express.js** - Web framework
- **JSON Web Tokens (JWT)** - Token-based authentication
- **Cookie Parser** - Cookie handling middleware
- **Postman** - API testing tool

---

## 📚 Key Concepts Demonstrated

1. Multi-step authentication flow
2. OTP (One-Time Password) verification
3. Cookie-based session management
4. JWT token generation and validation
5. Bearer token authorization
6. Middleware debugging and implementation
7. RESTful API design
8. Security best practices

---

## 📝 Conclusion

All 6 bugs have been successfully identified and fixed. The authentication flow is now working end-to-end, as proven by the successful generation of the unique success flag.

**Contact:** shrivastavamayank012@gmail.com
