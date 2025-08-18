# SATHI API Documentation

## 🎯 Overview

This document provides comprehensive documentation for all API endpoints in the SATHI (System for Analyzing and Tracking Human Intelligence) mental health monitoring system.

## 🔗 Base URL

- **Development**: `http://localhost:5000`
- **Production**: `https://your-domain.com`

## 🔐 Authentication

The system currently uses session-based authentication. All admin endpoints require authentication.

### Admin Login

#### POST /api/auth/login

**Description**: Authenticate admin user and create session

**Request Body**:
```json
{
  "force_id": "100000001",
  "password": "admin123"
}
```

**Response (Success)**:
```json
{
  "message": "Admin login successful",
  "user": {
    "force_id": "100000001",
    "role": "admin"
  }
}
```

**Response (Error)**:
```json
{
  "error": "Invalid credentials"
}
```

## 📊 Admin Dashboard APIs

### Get Dashboard Statistics

#### GET /api/admin/dashboard-stats

**Description**: Retrieve real-time dashboard statistics

**Query Parameters**:
- `timeframe` (optional): `7d`, `30d`, `90d` (default: `7d`)

**Response**:
```json
{
  "totalSoldiers": 150,
  "activeSurveys": 1,
  "highRiskSoldiers": 5,
  "criticalAlerts": 1,
  "surveyCompletionRate": 85.5,
  "averageMentalHealthScore": 0.35,
  "trendsData": {
    "labels": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
    "riskLevels": {
      "low": [120, 118, 125, 122, 119, 121, 123],
      "medium": [20, 22, 18, 21, 23, 19, 20],
      "high": [8, 7, 5, 6, 7, 8, 6],
      "critical": [2, 3, 2, 1, 1, 2, 1]
    }
  }
}
```

### Get Soldiers Report

#### GET /api/admin/soldiers-report

**Description**: Get comprehensive soldiers mental health report

**Query Parameters**:
- `risk_level`: `all`, `low`, `mid`, `high`, `critical`
- `days`: `3`, `7`, `30`, `180`
- `force_id`: Filter by specific Force ID
- `page`: Page number for pagination
- `per_page`: Items per page (max 100)

**Response**:
```json
{
  "soldiers": [
    {
      "force_id": "100000001",
      "name": "Soldier 100000001",
      "latest_session_id": 123,
      "combined_score": 0.45,
      "nlp_score": 0.5,
      "image_score": 0.35,
      "last_survey_date": "2024-01-15 10:30",
      "questionnaire_title": "Weekly Assessment",
      "risk_level": "MEDIUM",
      "total_cctv_detections": 15,
      "avg_cctv_score": 0.3,
      "mental_state": "MILD CONCERN",
      "alert_level": "YELLOW",
      "recommendation": "Weekly check-ins, monitor closely"
    }
  ],
  "pagination": {
    "current_page": 1,
    "per_page": 20,
    "total_count": 150,
    "total_pages": 8,
    "has_next": true,
    "has_prev": false
  }
}
```

## 📝 Survey Management APIs

### Get All Questionnaires

#### GET /api/admin/questionnaires

**Description**: Retrieve all available questionnaires

**Response**:
```json
{
  "questionnaires": [
    {
      "id": 1,
      "title": "Weekly Mental Health Assessment",
      "description": "Standard weekly questionnaire",
      "status": "Active",
      "total_questions": 10,
      "created_at": "2024-01-01T00:00:00Z"
    }
  ]
}
```

### Create Questionnaire

#### POST /api/admin/create-questionnaire

**Description**: Create a new questionnaire

**Request Body**:
```json
{
  "title": "Emergency Assessment",
  "description": "Emergency mental health check",
  "isActive": true,
  "numberOfQuestions": 5
}
```

**Response**:
```json
{
  "message": "Questionnaire created successfully",
  "questionnaire_id": 2
}
```

### Add Question to Questionnaire

#### POST /api/admin/add-question

**Description**: Add a question to an existing questionnaire

**Request Body**:
```json
{
  "questionnaire_id": 1,
  "question_text": "How are you feeling today?",
  "question_text_hindi": "आज आप कैसा महसूस कर रहे हैं?"
}
```

**Response**:
```json
{
  "message": "Question added successfully",
  "question_id": 15
}
```

## 🎭 Survey Response APIs

### Get Current Survey

#### GET /api/survey/current-survey

**Description**: Get the current active survey for soldier

**Headers**: 
- `Authorization: Bearer <token>` (for soldier authentication)

**Response**:
```json
{
  "questionnaire": {
    "id": 1,
    "title": "Weekly Mental Health Assessment",
    "description": "Please answer honestly",
    "questions": [
      {
        "id": 1,
        "question_text": "How are you feeling today?",
        "question_text_hindi": "आज आप कैसा महसूस कर रहे हैं?"
      }
    ]
  },
  "session_id": 123
}
```

### Submit Survey Response

#### POST /api/survey/submit-response

**Description**: Submit a response to a survey question

**Request Body**:
```json
{
  "session_id": 123,
  "question_id": 1,
  "response_text": "I am feeling okay today",
  "response_text_hindi": "मैं आज ठीक महसूस कर रहा हूं"
}
```

**Response**:
```json
{
  "message": "Response recorded successfully",
  "nlp_score": 0.45,
  "emotion_detected": "neutral"
}
```

### Complete Survey Session

#### POST /api/survey/complete-session

**Description**: Mark a survey session as completed

**Request Body**:
```json
{
  "session_id": 123
}
```

**Response**:
```json
{
  "message": "Survey completed successfully",
  "combined_score": 0.42,
  "risk_level": "MEDIUM",
  "recommendation": "Continue regular monitoring"
}
```

## 🖼️ Image Processing APIs

### Upload Profile Images

#### POST /api/image/upload-profile

**Description**: Upload soldier profile images for face recognition training

**Request Body**:
```json
{
  "force_id": "100000001",
  "images": ["base64_image_data1", "base64_image_data2"]
}
```

**Response**:
```json
{
  "message": "Profile images uploaded successfully",
  "images_count": 2
}
```

### Train Face Recognition Model

#### POST /api/image/train

**Description**: Train the face recognition model with uploaded images

**Response**:
```json
{
  "message": "Successfully trained model on 25 new soldiers",
  "trained_soldiers": ["100000001", "100000002", "100000003"],
  "model_accuracy": 0.95
}
```

### Start CCTV Monitoring

#### POST /api/image/start-monitoring

**Description**: Start real-time CCTV monitoring for emotion detection

**Request Body**:
```json
{
  "date": "2024-01-15",
  "duration_minutes": 60
}
```

**Response**:
```json
{
  "message": "CCTV monitoring started successfully",
  "monitoring_id": "mon_20240115_001"
}
```

### Stop CCTV Monitoring

#### POST /api/image/stop-monitoring

**Description**: Stop active CCTV monitoring

**Response**:
```json
{
  "message": "CCTV monitoring stopped",
  "detections_count": 45,
  "session_duration": "58 minutes"
}
```

## 🌐 Translation APIs

### Translate Question

#### POST /api/admin/translate-question

**Description**: Translate question text from English to Hindi

**Request Body**:
```json
{
  "question_text": "How are you feeling today?"
}
```

**Response**:
```json
{
  "hindi_text": "आज आप कैसा महसूस कर रहे हैं?"
}
```

### Translate Answer

#### POST /api/admin/translate-answer

**Description**: Translate answer text from Hindi to English

**Request Body**:
```json
{
  "answer_text": "मैं ठीक हूं"
}
```

**Response**:
```json
{
  "english_text": "I am fine"
}
```

## ⚙️ System Monitoring APIs

### Health Check

#### GET /api/monitor/health

**Description**: Get system health status

**Response**:
```json
{
  "status": "healthy",
  "checks": {
    "database": {"status": "healthy", "response_time": 0.05},
    "models": {"status": "healthy", "loaded": true},
    "disk_space": {"status": "healthy", "free_gb": 45.2},
    "memory": {"status": "healthy", "usage_percent": 65}
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### System Status

#### GET /api/monitor/status

**Description**: Get detailed system status and metrics

**Response**:
```json
{
  "system": {
    "uptime": "5 days, 12 hours",
    "version": "1.0.0",
    "environment": "production"
  },
  "performance": {
    "active_sessions": 25,
    "avg_response_time": 0.15,
    "total_requests": 15420,
    "error_rate": 0.02
  },
  "resources": {
    "cpu_usage": 25.5,
    "memory_usage": 65.2,
    "disk_usage": 45.8
  }
}
```

## 📈 Analytics APIs

### Export Report

#### GET /api/admin/export-report

**Description**: Export soldiers report in various formats

**Query Parameters**:
- `format`: `pdf`, `csv`, `excel`
- `risk_level`: Filter by risk level
- `date_range`: Date range for report

**Response**: File download (Content-Type based on format)

### Advanced Search

#### POST /api/admin/advanced-search

**Description**: Advanced search for soldiers with multiple filters

**Request Body**:
```json
{
  "filters": {
    "risk_levels": ["HIGH", "CRITICAL"],
    "date_range": {
      "start": "2024-01-01",
      "end": "2024-01-15"
    },
    "score_range": {
      "min": 0.5,
      "max": 1.0
    }
  },
  "sort": {
    "field": "combined_score",
    "order": "desc"
  },
  "pagination": {
    "page": 1,
    "per_page": 50
  }
}
```

**Response**: Same as soldiers-report endpoint with filtered results

## 📋 Request/Response Formats

### Standard Error Response

```json
{
  "error": "Error message",
  "code": "ERROR_CODE",
  "details": {
    "field": "Specific field error"
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

### Pagination Format

```json
{
  "pagination": {
    "current_page": 1,
    "per_page": 20,
    "total_count": 150,
    "total_pages": 8,
    "has_next": true,
    "has_prev": false
  }
}
```

## ⚠️ Error Handling

### HTTP Status Codes

- `200`: Success
- `201`: Created
- `400`: Bad Request
- `401`: Unauthorized
- `403`: Forbidden
- `404`: Not Found
- `500`: Internal Server Error

### Common Error Codes

- `INVALID_CREDENTIALS`: Authentication failed
- `MISSING_REQUIRED_FIELD`: Required field not provided
- `INVALID_FORMAT`: Data format is incorrect
- `RESOURCE_NOT_FOUND`: Requested resource doesn't exist
- `PERMISSION_DENIED`: Insufficient permissions
- `SYSTEM_ERROR`: Internal system error

## 🔄 Rate Limiting

Currently, no rate limiting is implemented, but it's planned for future versions:

- **Admin APIs**: 1000 requests per hour
- **Survey APIs**: 100 requests per hour
- **Upload APIs**: 10 requests per hour

## 📝 Examples

### Complete Survey Workflow

```bash
# 1. Get current survey
curl -H "Authorization: Bearer <token>" \
     http://localhost:5000/api/survey/current-survey

# 2. Submit responses
curl -X POST -H "Content-Type: application/json" \
     -d '{"session_id":123,"question_id":1,"response_text":"I feel good"}' \
     http://localhost:5000/api/survey/submit-response

# 3. Complete session
curl -X POST -H "Content-Type: application/json" \
     -d '{"session_id":123}' \
     http://localhost:5000/api/survey/complete-session
```

### Admin Dashboard Access

```bash
# 1. Login
curl -X POST -H "Content-Type: application/json" \
     -d '{"force_id":"100000001","password":"admin123"}' \
     http://localhost:5000/api/auth/login

# 2. Get dashboard stats
curl -b cookies.txt \
     http://localhost:5000/api/admin/dashboard-stats?timeframe=30d

# 3. Get soldiers report
curl -b cookies.txt \
     "http://localhost:5000/api/admin/soldiers-report?risk_level=high&days=7"
```

## 📚 Additional Resources

- [User Guide](USER_GUIDE.md) - End-user documentation
- [Contributing Guide](CONTRIBUTING.md) - Development setup
- [Deployment Guide](DEPLOYMENT_GUIDE.md) - Production deployment
- [Troubleshooting Guide](TROUBLESHOOTING.md) - Common issues and solutions

---

**Note**: This API handles sensitive mental health data. Ensure all requests comply with applicable privacy and security regulations.