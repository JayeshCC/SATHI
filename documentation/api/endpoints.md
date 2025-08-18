# API Endpoints Reference

Complete reference for all SATHI API endpoints with detailed parameters, request/response formats, and examples.

## 📋 Table of Contents

1. [Authentication Endpoints](#authentication-endpoints)
2. [Admin Endpoints](#admin-endpoints)
3. [Survey Endpoints](#survey-endpoints)
4. [Image Processing Endpoints](#image-processing-endpoints)
5. [Monitor Endpoints](#monitor-endpoints)
6. [Settings Endpoints](#settings-endpoints)

---

## 🔐 Authentication Endpoints

### Login

**Endpoint:** `POST /api/auth/login`

**Description:** Authenticate user and create session

**Request Body:**
```json
{
  "username": "string (required)",
  "password": "string (required)"
}
```

**Response (Success):**
```json
{
  "success": true,
  "message": "Login successful",
  "user": {
    "username": "admin_user",
    "role": "admin",
    "permissions": ["read", "write", "admin"],
    "last_login": "2024-01-01T00:00:00Z"
  }
}
```

**Response (Error):**
```json
{
  "success": false,
  "message": "Invalid credentials",
  "error": {
    "code": "INVALID_CREDENTIALS"
  }
}
```

### Logout

**Endpoint:** `POST /api/auth/logout`

**Description:** End user session

**Headers:** `Cookie: session=...`

**Response:**
```json
{
  "success": true,
  "message": "Logout successful"
}
```

### Session Check

**Endpoint:** `GET /api/auth/session`

**Description:** Verify current session status

**Headers:** `Cookie: session=...`

**Response:**
```json
{
  "success": true,
  "authenticated": true,
  "user": {
    "username": "admin_user",
    "role": "admin",
    "session_expires": "2024-01-01T00:30:00Z"
  }
}
```

---

## 👨‍💼 Admin Endpoints

### Dashboard Statistics

**Endpoint:** `GET /api/admin/dashboard-stats`

**Description:** Get overall system statistics for admin dashboard

**Headers:** `Cookie: session=...`

**Response:**
```json
{
  "success": true,
  "data": {
    "total_soldiers": 1250,
    "survey_responses": 945,
    "high_risk_soldiers": 23,
    "critical_alerts": 5,
    "surveys_today": 45,
    "average_response_time": 125.5,
    "system_health": "excellent"
  }
}
```

### Soldiers Report

**Endpoint:** `GET /api/admin/soldiers-report`

**Description:** Get detailed soldiers report with filtering

**Query Parameters:**
- `page` (integer, optional): Page number (default: 1)
- `limit` (integer, optional): Results per page (default: 50, max: 100)
- `risk_level` (string, optional): Filter by risk level (LOW, MEDIUM, HIGH, CRITICAL)
- `unit` (string, optional): Filter by unit
- `date_from` (string, optional): Start date (YYYY-MM-DD)
- `date_to` (string, optional): End date (YYYY-MM-DD)

**Response:**
```json
{
  "success": true,
  "data": {
    "soldiers": [
      {
        "force_id": "CRPF001234",
        "name": "John Doe",
        "unit": "Alpha Company",
        "risk_level": "MEDIUM",
        "combined_score": 65.5,
        "last_survey_date": "2024-01-01",
        "surveys_completed": 12,
        "emotion_analysis": {
          "dominant_emotion": "neutral",
          "confidence": 0.85
        }
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 50,
      "total": 1250,
      "total_pages": 25
    },
    "summary": {
      "risk_distribution": {
        "LOW": 850,
        "MEDIUM": 300,
        "HIGH": 75,
        "CRITICAL": 25
      }
    }
  }
}
```

### Create Questionnaire

**Endpoint:** `POST /api/admin/create-questionnaire`

**Description:** Create a new questionnaire

**Request Body:**
```json
{
  "title": "Weekly Mental Health Check",
  "description": "Regular assessment for CRPF personnel",
  "questions": [
    {
      "question_text": "How would you rate your stress level?",
      "question_type": "scale",
      "options": ["1", "2", "3", "4", "5"],
      "required": true,
      "order": 1
    },
    {
      "question_text": "Describe your sleep quality",
      "question_type": "multiple_choice",
      "options": ["Excellent", "Good", "Fair", "Poor"],
      "required": true,
      "order": 2
    }
  ],
  "is_active": true
}
```

**Response:**
```json
{
  "success": true,
  "message": "Questionnaire created successfully",
  "data": {
    "questionnaire_id": 15,
    "title": "Weekly Mental Health Check",
    "total_questions": 2,
    "created_at": "2024-01-01T00:00:00Z"
  }
}
```

### Get All Questionnaires

**Endpoint:** `GET /api/admin/questionnaires`

**Description:** Retrieve all questionnaires with filtering options

**Query Parameters:**
- `active_only` (boolean, optional): Only show active questionnaires
- `page` (integer, optional): Page number
- `limit` (integer, optional): Results per page

**Response:**
```json
{
  "success": true,
  "data": {
    "questionnaires": [
      {
        "questionnaire_id": 15,
        "title": "Weekly Mental Health Check",
        "description": "Regular assessment for CRPF personnel",
        "question_count": 10,
        "is_active": true,
        "created_at": "2024-01-01T00:00:00Z",
        "last_updated": "2024-01-01T12:00:00Z",
        "response_count": 245
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 5
    }
  }
}
```

### Advanced Search

**Endpoint:** `POST /api/admin/advanced-search`

**Description:** Perform advanced search with multiple criteria

**Request Body:**
```json
{
  "criteria": {
    "force_id": "CRPF001*",
    "risk_level": ["HIGH", "CRITICAL"],
    "unit": ["Alpha Company", "Bravo Company"],
    "date_range": {
      "start": "2024-01-01",
      "end": "2024-01-31"
    },
    "survey_completion": {
      "min": 5,
      "max": 20
    }
  },
  "sort": {
    "field": "combined_score",
    "order": "desc"
  },
  "page": 1,
  "limit": 50
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "results": [
      {
        "force_id": "CRPF001234",
        "name": "John Doe",
        "unit": "Alpha Company",
        "risk_level": "HIGH",
        "combined_score": 85.5,
        "last_survey_date": "2024-01-15",
        "match_criteria": ["risk_level", "unit", "date_range"]
      }
    ],
    "total_matches": 23,
    "search_time": "0.125s"
  }
}
```

---

## 📋 Survey Endpoints

### Get Survey Initialization Data

**Endpoint:** `GET /api/survey/init`

**Description:** Get initialization data for survey interface

**Query Parameters:**
- `force_id` (string, required): Force ID of the soldier

**Response:**
```json
{
  "success": true,
  "data": {
    "soldier_info": {
      "force_id": "CRPF001234",
      "name": "John Doe",
      "unit": "Alpha Company",
      "last_survey": "2024-01-01"
    },
    "available_questionnaires": [
      {
        "questionnaire_id": 15,
        "title": "Weekly Mental Health Check",
        "estimated_time": "5-10 minutes"
      }
    ],
    "system_settings": {
      "allow_anonymous": false,
      "require_camera": true,
      "enable_emotion_detection": true
    }
  }
}
```

### Get Active Questionnaire

**Endpoint:** `GET /api/survey/active-questionnaire`

**Description:** Get the currently active questionnaire

**Response:**
```json
{
  "success": true,
  "data": {
    "questionnaire": {
      "questionnaire_id": 15,
      "title": "Weekly Mental Health Check",
      "description": "Regular assessment for CRPF personnel",
      "questions": [
        {
          "question_id": 101,
          "question_text": "How would you rate your stress level?",
          "question_type": "scale",
          "options": ["1", "2", "3", "4", "5"],
          "required": true,
          "order": 1,
          "max_time": 30
        }
      ],
      "total_questions": 10,
      "estimated_time": "5-10 minutes"
    }
  }
}
```

### Submit Survey

**Endpoint:** `POST /api/survey/submit`

**Description:** Submit completed survey responses

**Request Body:**
```json
{
  "force_id": "CRPF001234",
  "questionnaire_id": 15,
  "responses": [
    {
      "question_id": 101,
      "answer": "3",
      "response_time": 15.5
    },
    {
      "question_id": 102,
      "answer": "Good",
      "response_time": 8.2
    }
  ],
  "survey_metadata": {
    "start_time": "2024-01-01T10:00:00Z",
    "end_time": "2024-01-01T10:08:30Z",
    "device_info": "Chrome 96.0",
    "ip_address": "192.168.1.100"
  }
}
```

**Response:**
```json
{
  "success": true,
  "message": "Survey submitted successfully",
  "data": {
    "response_id": "resp_12345",
    "risk_assessment": {
      "risk_level": "MEDIUM",
      "combined_score": 65.5,
      "confidence": 0.85,
      "recommendations": [
        "Consider stress management techniques",
        "Monitor sleep patterns"
      ]
    },
    "next_survey_date": "2024-01-08"
  }
}
```

---

## 🖼️ Image Processing Endpoints

### Face Recognition

**Endpoint:** `POST /api/image/face-recognition`

**Description:** Process face recognition for soldier identification

**Request:**
```http
POST /api/image/face-recognition
Content-Type: multipart/form-data

image: [binary image data]
force_id: CRPF001234 (optional)
```

**Response:**
```json
{
  "success": true,
  "data": {
    "face_detected": true,
    "confidence": 0.92,
    "soldier_matched": {
      "force_id": "CRPF001234",
      "name": "John Doe",
      "match_confidence": 0.95
    },
    "face_quality": {
      "brightness": "good",
      "sharpness": "excellent",
      "angle": "optimal"
    }
  }
}
```

### Emotion Detection

**Endpoint:** `POST /api/image/emotion-detection`

**Description:** Analyze emotions from facial image

**Request:**
```http
POST /api/image/emotion-detection
Content-Type: multipart/form-data

image: [binary image data]
force_id: CRPF001234
```

**Response:**
```json
{
  "success": true,
  "data": {
    "emotions": {
      "happy": 0.15,
      "sad": 0.25,
      "angry": 0.05,
      "fear": 0.10,
      "surprise": 0.05,
      "disgust": 0.02,
      "neutral": 0.38
    },
    "dominant_emotion": "neutral",
    "confidence": 0.88,
    "analysis_metadata": {
      "model_version": "v2.1.0",
      "processing_time": "0.45s",
      "image_quality": "high"
    }
  }
}
```

---

## 📊 Monitor Endpoints

### System Health

**Endpoint:** `GET /api/monitor/health`

**Description:** Get system health status

**Response:**
```json
{
  "success": true,
  "data": {
    "status": "healthy",
    "services": {
      "database": "online",
      "ml_models": "loaded",
      "file_storage": "accessible",
      "cache": "operational"
    },
    "metrics": {
      "cpu_usage": 45.2,
      "memory_usage": 62.1,
      "disk_usage": 78.5,
      "active_sessions": 23
    },
    "uptime": "72h 15m 30s"
  }
}
```

### Performance Metrics

**Endpoint:** `GET /api/monitor/metrics`

**Description:** Get detailed performance metrics

**Query Parameters:**
- `timeframe` (string, optional): Time range (1h, 6h, 24h, 7d, 30d)

**Response:**
```json
{
  "success": true,
  "data": {
    "timeframe": "24h",
    "api_requests": {
      "total": 15420,
      "success_rate": 99.2,
      "average_response_time": 125.5
    },
    "database": {
      "queries_per_second": 45.2,
      "average_query_time": 15.8,
      "connection_pool_usage": 65.0
    },
    "ml_processing": {
      "face_recognition_requests": 234,
      "emotion_analysis_requests": 189,
      "average_processing_time": 450.2
    }
  }
}
```

---

## ⚙️ Settings Endpoints

### Get System Settings

**Endpoint:** `GET /api/admin/settings`

**Description:** Retrieve all system settings

**Query Parameters:**
- `category` (string, optional): Filter by category (system, security, survey, etc.)

**Response:**
```json
{
  "success": true,
  "data": {
    "settings": {
      "system": {
        "max_file_size": "10MB",
        "session_timeout": 1800,
        "max_concurrent_users": 100
      },
      "survey": {
        "max_question_time": 120,
        "require_camera": true,
        "enable_emotion_detection": true
      },
      "security": {
        "password_min_length": 8,
        "require_2fa": false,
        "session_security": "high"
      }
    }
  }
}
```

### Update System Settings

**Endpoint:** `PUT /api/admin/settings`

**Description:** Update system settings

**Request Body:**
```json
{
  "settings": {
    "max_file_size": "15MB",
    "session_timeout": 3600,
    "require_camera": false
  }
}
```

**Response:**
```json
{
  "success": true,
  "message": "Settings updated successfully",
  "data": {
    "updated_settings": 3,
    "requires_restart": false,
    "changes": [
      {
        "setting": "max_file_size",
        "old_value": "10MB",
        "new_value": "15MB"
      }
    ]
  }
}
```

---

## 📝 Notes

### Common Response Patterns

All endpoints follow consistent patterns:
- Success responses always include `success: true`
- Error responses always include `success: false` and detailed error information
- Pagination follows the same structure across all list endpoints
- Timestamps are in ISO 8601 format (UTC)

### Authentication Requirements

- All endpoints except `/api/auth/login` require authentication
- Session cookies are automatically handled by the browser
- For API clients, include the session cookie in requests

### File Upload Limits

- Maximum file size: 10MB (configurable)
- Supported image formats: JPEG, PNG, WebP
- Images are automatically resized if larger than 2048x2048px

For detailed examples and code samples, see [API Examples](examples.md).