# SATHI API Documentation

Complete API reference for the SATHI (CRPF Mental Health Monitoring System) backend services.

## 📋 Table of Contents

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [Base Configuration](#base-configuration)
4. [Error Handling](#error-handling)
5. [Rate Limiting](#rate-limiting)
6. [API Endpoints](#api-endpoints)
7. [Examples](#examples)

## 🌟 Overview

The SATHI API is a RESTful service built with Flask that provides comprehensive mental health monitoring capabilities for CRPF personnel. The API is organized into several modules:

- **Authentication**: User login, session management, and authorization
- **Admin**: Administrative functions, dashboard, and reporting
- **Survey**: Questionnaire management and survey responses
- **Image**: Face recognition, emotion detection, and image processing
- **Monitor**: System monitoring and health checks

### Base URL

```
# Development
http://localhost:5000/api

# Production
https://api.sathi.crpf.gov.in/api
```

### Content Type

All requests should use `Content-Type: application/json` header unless specified otherwise.

## 🔐 Authentication

### Authentication Methods

SATHI uses session-based authentication with secure cookies:

```http
POST /api/auth/login
Content-Type: application/json

{
  "username": "admin_user",
  "password": "secure_password"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Login successful",
  "user": {
    "username": "admin_user",
    "role": "admin",
    "permissions": ["read", "write", "admin"]
  },
  "session_id": "sess_12345..."
}
```

### Session Management

- Sessions expire after 30 minutes of inactivity
- Session cookies are HttpOnly and Secure
- CSRF protection is enabled for all state-changing operations

### Authorization Headers

For API access, include the session cookie:

```http
Cookie: session=sess_12345...
```

## ⚙️ Base Configuration

### Request Headers

```http
Content-Type: application/json
Accept: application/json
X-Requested-With: XMLHttpRequest
```

### Response Format

All API responses follow this structure:

```json
{
  "success": boolean,
  "message": "Human readable message",
  "data": {}, // Response data (optional)
  "error": {}, // Error details (optional)
  "timestamp": "2024-01-01T00:00:00Z",
  "request_id": "req_12345..."
}
```

## 🚨 Error Handling

### HTTP Status Codes

| Code | Meaning | Description |
|------|---------|-------------|
| 200 | OK | Request successful |
| 201 | Created | Resource created successfully |
| 400 | Bad Request | Invalid request data |
| 401 | Unauthorized | Authentication required |
| 403 | Forbidden | Insufficient permissions |
| 404 | Not Found | Resource not found |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Internal Server Error | Server error |

### Error Response Format

```json
{
  "success": false,
  "message": "Validation failed",
  "error": {
    "code": "VALIDATION_ERROR",
    "details": [
      {
        "field": "username",
        "message": "Username is required"
      }
    ]
  },
  "timestamp": "2024-01-01T00:00:00Z",
  "request_id": "req_12345..."
}
```

## 🛡️ Rate Limiting

### Limits

- **General API**: 100 requests per minute per IP
- **Authentication**: 10 requests per minute per IP
- **File Upload**: 5 requests per minute per user
- **Report Generation**: 3 requests per minute per user

### Rate Limit Headers

```http
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1641024000
```

## 🔗 API Endpoints

### Authentication Endpoints

#### Login
```http
POST /api/auth/login
```

#### Logout
```http
POST /api/auth/logout
```

#### Session Check
```http
GET /api/auth/session
```

### Admin Endpoints

#### Dashboard Statistics
```http
GET /api/admin/dashboard-stats
```

#### User Management
```http
GET /api/admin/users
POST /api/admin/users
PUT /api/admin/users/{user_id}
DELETE /api/admin/users/{user_id}
```

#### Soldier Management
```http
GET /api/admin/soldiers
POST /api/admin/soldiers
PUT /api/admin/soldiers/{force_id}
DELETE /api/admin/soldiers/{force_id}
```

### Survey Endpoints

#### Questionnaires
```http
GET /api/survey/questionnaires
POST /api/survey/questionnaires
PUT /api/survey/questionnaires/{id}
DELETE /api/survey/questionnaires/{id}
```

#### Survey Responses
```http
GET /api/survey/responses
POST /api/survey/submit
GET /api/survey/responses/{response_id}
```

### Image Processing Endpoints

#### Face Recognition
```http
POST /api/image/face-recognition
```

#### Emotion Detection
```http
POST /api/image/emotion-detection
```

## 💡 Examples

See the [API Examples](examples.md) document for detailed request/response examples in multiple programming languages.

---

For detailed endpoint documentation, see:
- [Endpoint Reference](endpoints.md)
- [Authentication Guide](authentication.md)
- [Request/Response Examples](examples.md)
- [Error Handling Guide](error-handling.md)