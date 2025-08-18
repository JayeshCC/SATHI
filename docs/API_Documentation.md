# API Documentation
*Part of SATHI Documentation Suite*

**📚 Quick Navigation:**
[🏠 README](../README.md) | [🤝 Contributing](../CONTRIBUTING.md) | [🔌 API](API_Documentation.md) | [👥 Users](USER_GUIDE.md) | [🚀 Deploy](../DEPLOYMENT_GUIDE.md) | [🔧 Troubleshoot](../TROUBLESHOOTING.md)

---

## Table of Contents

1. [Overview](#overview)
2. [Authentication](#authentication)
3. [API Endpoints](#api-endpoints)
4. [Request/Response Formats](#requestresponse-formats)
5. [Error Handling](#error-handling)
6. [Rate Limiting](#rate-limiting)
7. [Examples](#examples)

## Overview

The SATHI API is a RESTful service built with Flask that provides comprehensive mental health monitoring capabilities for CRPF personnel. The API is organized into several modules:

- **Authentication**: User login, session management, and authorization
- **Admin**: Administrative functions, dashboard, and reporting
- **Survey**: Questionnaire management and survey responses
- **Image**: Face recognition, emotion detection, and image processing
- **Monitor**: System monitoring and health checks

### Base URL
```
http://localhost:5000/api
```

### Content Type
All requests should use `Content-Type: application/json` header.

## Authentication

### Login
**POST** `/api/auth/login`

Authenticate admin users and create a session.

**Request Body:**
```json
{
  "force_id": "100000001",
  "password": "admin123"
}
```

**Response:**
```json
{
  "message": "Admin login successful",
  "user": {
    "force_id": "100000001",
    "role": "admin"
  },
  "session_timeout": 3600
}
```

### Logout
**POST** `/api/auth/logout`

Clear user session.

**Response:**
```json
{
  "message": "Logout successful"
}
```

### Session Status
**GET** `/api/auth/session-status`

Check if current session is valid.

**Response:**
```json
{
  "valid": true,
  "user": {
    "force_id": "100000001",
    "role": "admin"
  },
  "expires_at": "2024-01-15T10:30:00"
}
```

### Soldier Registration
**POST** `/api/auth/register`

Register a new soldier in the system.

**Request Body:**
```json
{
  "force_id": "100000002",
  "password": "soldier123"
}
```

**Response:**
```json
{
  "message": "Soldier registered successfully",
  "user": {
    "force_id": "100000002",
    "role": "soldier"
  }
}
```

### Verify Soldier Credentials
**POST** `/api/auth/verify-soldier`

Verify soldier credentials without creating a session.

**Request Body:**
```json
{
  "force_id": "100000002",
  "password": "soldier123"
}
```

**Response:**
```json
{
  "message": "Soldier credentials verified",
  "verified": true,
  "force_id": "100000002"
}
```

## Admin API Endpoints

### Dashboard Statistics
**GET** `/api/admin/dashboard-stats?timeframe=7d`

Get comprehensive dashboard statistics.

**Query Parameters:**
- `timeframe`: `7d`, `30d`, `90d` (default: `7d`)

**Response:**
```json
{
  "totalSoldiers": 150,
  "currentSurveyResponses": 89,
  "currentSurveyTitle": "Weekly Mental Health Assessment",
  "currentSurveyCompletionRate": 59.3,
  "pendingResponses": 61,
  "highRiskSoldiers": 12,
  "criticalAlerts": 3,
  "surveyCompletionRate": 85.2,
  "averageMentalHealthScore": 0.234,
  "riskDistribution": {
    "low": 89,
    "medium": 35,
    "high": 9,
    "critical": 3,
    "noData": 14
  },
  "trendsData": {
    "labels": ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"],
    "riskLevels": {
      "low": [12, 15, 8, 14, 18, 16, 22],
      "medium": [5, 3, 7, 4, 6, 8, 5],
      "high": [2, 1, 3, 2, 1, 2, 3],
      "critical": [0, 1, 0, 1, 0, 0, 1]
    }
  }
}
```

### Soldiers Report
**GET** `/api/admin/soldiers-report`

Get detailed soldiers report with filtering and pagination.

**Query Parameters:**
- `risk_level`: `all`, `low`, `mid`, `high`, `critical`
- `days`: `3`, `7`, `30`, `180`
- `force_id`: Force ID filter
- `page`: Page number (default: 1)
- `per_page`: Items per page (default: 20)

**Response:**
```json
{
  "soldiers": [
    {
      "force_id": "100000002",
      "name": "Soldier 100000002",
      "combined_score": 0.456,
      "nlp_score": 0.523,
      "image_score": 0.234,
      "last_survey_date": "2024-01-15 09:30",
      "questionnaire_title": "Weekly Assessment",
      "risk_level": "MEDIUM",
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

### Questionnaire Management

#### Get All Questionnaires
**GET** `/api/admin/questionnaires`

**Response:**
```json
{
  "questionnaires": [
    {
      "id": 1,
      "title": "Weekly Mental Health Assessment",
      "description": "Standard weekly questionnaire",
      "status": "Active",
      "total_questions": 10,
      "created_at": "2024-01-01 10:00:00"
    }
  ]
}
```

#### Create Questionnaire
**POST** `/api/admin/create-questionnaire`

**Request Body:**
```json
{
  "title": "New Assessment",
  "description": "Description here",
  "isActive": true,
  "numberOfQuestions": 8
}
```

#### Get Questionnaire Details
**GET** `/api/admin/questionnaires/{id}`

**Response:**
```json
{
  "questionnaire": {
    "id": 1,
    "title": "Weekly Mental Health Assessment",
    "description": "Standard weekly questionnaire",
    "status": "Active",
    "total_questions": 10,
    "questions": [
      {
        "id": 1,
        "question_text": "How are you feeling today?",
        "question_text_hindi": "आज आप कैसा महसूस कर रहे हैं?"
      }
    ]
  }
}
```

#### Add Question
**POST** `/api/admin/add-question`

**Request Body:**
```json
{
  "questionnaire_id": 1,
  "question_text": "How is your stress level?",
  "question_text_hindi": "आपका तनाव का स्तर कैसा है?"
}
```

### Soldier Management

#### Add Soldier
**POST** `/api/admin/add-soldier`

**Request Body:**
```json
{
  "force_id": "100000003",
  "password": "password123"
}
```

#### Get All Soldiers
**GET** `/api/admin/soldiers`

**Response:**
```json
{
  "soldiers": [
    {
      "force_id": "100000002",
      "name": "N/A",
      "unit": "N/A",
      "rank": "N/A",
      "created_at": "2024-01-01T10:00:00"
    }
  ]
}
```

### Advanced Search
**POST** `/api/admin/search-soldiers`

**Request Body:**
```json
{
  "searchTerm": "100000",
  "filters": {
    "riskLevels": ["HIGH", "CRITICAL"],
    "units": ["Unit A", "Unit B"],
    "startDate": "2024-01-01",
    "endDate": "2024-01-31",
    "scoreMin": 0.5,
    "scoreMax": 1.0,
    "sortBy": "score",
    "sortOrder": "desc",
    "page": 1,
    "pageSize": 20
  }
}
```

### Export Reports

#### Download PDF Report
**POST** `/api/admin/download-soldiers-pdf`

**Request Body:**
```json
{
  "soldiers": [...],
  "filters": {...},
  "report_title": "Custom Report Title"
}
```

#### Download CSV Report
**POST** `/api/admin/download-soldiers-csv`

**Request Body:**
```json
{
  "soldiers": [...],
  "filters": {...}
}
```

### Translation Services

#### Translate Question (English to Hindi)
**POST** `/api/admin/translate-question`

**Request Body:**
```json
{
  "question_text": "How are you feeling today?"
}
```

**Response:**
```json
{
  "hindi_text": "आज आप कैसा महसूस कर रहे हैं?"
}
```

#### Translate Answer (Hindi to English)
**POST** `/api/admin/translate-answer`

**Request Body:**
```json
{
  "answer_text": "मैं ठीक महसूस कर रहा हूं"
}
```

**Response:**
```json
{
  "english_text": "I am feeling fine"
}
```

## Survey API Endpoints

### Get Survey Initialization Data
**GET** `/api/survey/survey-initialization`

Get all data needed for survey initialization in a single request.

**Response:**
```json
{
  "questionnaire": {
    "id": 1,
    "title": "Weekly Mental Health Assessment",
    "description": "Standard assessment",
    "total_questions": 10
  },
  "questions": [
    {
      "id": 1,
      "question_text": "How are you feeling today?",
      "question_text_hindi": "आज आप कैसा महसूस कर रहे हैं?"
    }
  ],
  "settings": {
    "webcam_enabled": true,
    "detection_interval": 30,
    "camera_width": 640,
    "camera_height": 480
  }
}
```

### Get Active Questionnaire
**GET** `/api/survey/active-questionnaire`

Get the currently active questionnaire and its questions.

### Submit Survey
**POST** `/api/survey/submit`

Submit completed survey responses.

**Request Body:**
```json
{
  "questionnaire_id": 1,
  "force_id": "100000002",
  "password": "soldier123",
  "responses": [
    {
      "question_id": 1,
      "answer_text": "I am feeling okay today"
    }
  ],
  "mental_state_rating": 5,
  "mental_state_emoji": "😐",
  "mental_state_text_en": "Neutral",
  "mental_state_text_hi": "तटस्थ"
}
```

**Response:**
```json
{
  "message": "Survey submitted successfully",
  "session_id": 123,
  "scores": {
    "nlp_avg_score": 0.456,
    "emotion_avg_score": 0.234,
    "combined_avg_score": 0.389,
    "weighting": "NLP: 70%, Emotion: 30%"
  },
  "mental_state": {
    "state": "MILD CONCERN",
    "level": "YELLOW",
    "description": "Moderate stress/negative mood detected",
    "recommendation": "Weekly check-ins, monitor closely"
  }
}
```

### Activate Questionnaire
**POST** `/api/survey/admin/questionnaires/{id}/activate`

Activate a specific questionnaire (deactivates all others).

## Image Processing API Endpoints

### Image Collection
**POST** `/api/image/collect`

Collect training images for face recognition.

**Request Body:**
```json
{
  "force_id": "100000002"
}
```

### Face Model Training

#### Train Single Soldier
**POST** `/api/image/train`

**Request Body:**
```json
{
  "force_id": "100000002"
}
```

#### Batch Training
**POST** `/api/image/train/batch`

**Request Body:**
```json
{
  "force_ids": ["100000002", "100000003", "100000004"]
}
```

### Survey Emotion Monitoring

#### Start Survey Monitoring
**POST** `/api/image/start-survey-monitoring`

**Request Body:**
```json
{
  "force_id": "100000002"
}
```

#### End Survey Monitoring
**POST** `/api/image/end-survey-monitoring`

**Request Body:**
```json
{
  "force_id": "100000002",
  "session_id": 123
}
```

### Model Management

#### Get Model Status
**GET** `/api/image/model-status`

**Response:**
```json
{
  "model_exists": true,
  "soldier_count": 25,
  "model_version": "v1.0",
  "last_trained": "2024-01-15T10:30:00",
  "model_operational": true
}
```

#### Get Soldier Training Status
**GET** `/api/image/soldier-training-status/{force_id}`

#### Delete Soldier Face Data
**DELETE** `/api/image/delete-soldier/{force_id}`

#### Refresh Model
**POST** `/api/image/refresh-model`

## Monitor API Endpoints

### System Health
**GET** `/api/monitor/system/health`

Get overall system health status.

**Response:**
```json
{
  "overall_health": "HEALTHY",
  "face_recognition": {
    "model_operational": true,
    "soldier_count": 25
  },
  "emotion_detection": {
    "system_operational": true
  },
  "database_connected": true,
  "timestamp": "2024-01-15T10:30:00"
}
```

### Face Model Monitoring

#### Get Face Model Status
**GET** `/api/monitor/face-model/status`

#### Refresh Face Model
**POST** `/api/monitor/face-model/refresh`

#### Check Model Integrity
**GET** `/api/monitor/face-model/integrity`

#### Check Database Sync
**GET** `/api/monitor/face-model/database-sync`

### Training History
**GET** `/api/monitor/training/history`

Get training history and statistics.

## Admin Settings API Endpoints

### Get All Settings
**GET** `/api/admin/settings`

**Response:**
```json
{
  "settings": [
    {
      "setting_name": "nlp_weight",
      "setting_value": "0.7",
      "category": "AI_ML",
      "data_type": "number",
      "description": "Weight for NLP analysis in combined scoring"
    }
  ],
  "categories": ["AI_ML", "SYSTEM", "SECURITY", "UI"]
}
```

### Update Setting
**PUT** `/api/admin/settings/{setting_name}`

**Request Body:**
```json
{
  "setting_value": "0.8",
  "description": "Updated NLP weight"
}
```

### Backup Settings
**POST** `/api/admin/settings/backup`

### Restore Settings
**POST** `/api/admin/settings/restore`

**Request Body:**
```json
{
  "backup_data": {...}
}
```

## Request/Response Formats

### Standard Success Response
```json
{
  "message": "Operation completed successfully",
  "data": {...}
}
```

### Standard Error Response
```json
{
  "error": "Detailed error message",
  "code": "ERROR_CODE",
  "details": {...}
}
```

## Error Handling

### HTTP Status Codes
- `200` - Success
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `500` - Internal Server Error

### Common Error Responses

#### Authentication Required
```json
{
  "error": "Authentication required",
  "code": "AUTH_REQUIRED"
}
```

#### Invalid Credentials
```json
{
  "error": "Invalid credentials",
  "code": "INVALID_CREDENTIALS"
}
```

#### Resource Not Found
```json
{
  "error": "Resource not found",
  "code": "NOT_FOUND"
}
```

#### Validation Error
```json
{
  "error": "Validation failed",
  "code": "VALIDATION_ERROR",
  "details": {
    "force_id": ["Must be 9 digits"],
    "password": ["Required field"]
  }
}
```

## Rate Limiting

The API implements rate limiting to prevent abuse:

- **General Endpoints**: 100 requests per minute
- **Authentication Endpoints**: 10 requests per minute
- **File Upload Endpoints**: 5 requests per minute

Rate limit headers are included in responses:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1642248000
```

## Examples

### Complete Survey Workflow

1. **Get Survey Data**
```bash
curl -X GET http://localhost:5000/api/survey/survey-initialization
```

2. **Start Emotion Monitoring**
```bash
curl -X POST http://localhost:5000/api/image/start-survey-monitoring \
  -H "Content-Type: application/json" \
  -d '{"force_id": "100000002"}'
```

3. **Submit Survey**
```bash
curl -X POST http://localhost:5000/api/survey/submit \
  -H "Content-Type: application/json" \
  -d '{
    "questionnaire_id": 1,
    "force_id": "100000002",
    "password": "soldier123",
    "responses": [{"question_id": 1, "answer_text": "I feel good"}]
  }'
```

### Admin Dashboard Access

1. **Login**
```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"force_id": "100000001", "password": "admin123"}'
```

2. **Get Dashboard Stats**
```bash
curl -X GET http://localhost:5000/api/admin/dashboard-stats?timeframe=7d \
  -H "Cookie: session=your_session_cookie"
```

3. **Get Soldiers Report**
```bash
curl -X GET "http://localhost:5000/api/admin/soldiers-report?risk_level=high&days=7" \
  -H "Cookie: session=your_session_cookie"
```

```

### Python SDK Example

```python
import requests
import json

class SATHIClient:
    """Python SDK for SATHI API integration."""
    
    def __init__(self, base_url: str = "http://localhost:5000/api"):
        self.base_url = base_url
        self.session = requests.Session()
    
    def login(self, force_id: str, password: str) -> bool:
        """Authenticate with SATHI system."""
        response = self.session.post(f"{self.base_url}/auth/login", json={
            "force_id": force_id,
            "password": password
        })
        return response.status_code == 200
    
    def get_dashboard_stats(self, timeframe: str = "7d") -> dict:
        """Get dashboard statistics."""
        response = self.session.get(
            f"{self.base_url}/admin/dashboard-stats",
            params={"timeframe": timeframe}
        )
        return response.json()
    
    def submit_survey(self, force_id: str, responses: list, image_data: str = None) -> dict:
        """Submit survey response with optional image."""
        payload = {
            "force_id": force_id,
            "responses": responses
        }
        if image_data:
            payload["image_data"] = image_data
            
        response = self.session.post(f"{self.base_url}/survey/submit", json=payload)
        return response.json()

# Usage example
client = SATHIClient()
if client.login("100000001", "admin123"):
    stats = client.get_dashboard_stats("30d")
    print(f"Total soldiers: {stats['total_soldiers']}")
```

### JavaScript SDK Example

```javascript
class SATHIClient {
    constructor(baseUrl = 'http://localhost:5000/api') {
        this.baseUrl = baseUrl;
        this.headers = {
            'Content-Type': 'application/json'
        };
    }

    async login(forceId, password) {
        const response = await fetch(`${this.baseUrl}/auth/login`, {
            method: 'POST',
            headers: this.headers,
            credentials: 'include',
            body: JSON.stringify({
                force_id: forceId,
                password: password
            })
        });
        return response.ok;
    }

    async getDashboardStats(timeframe = '7d') {
        const response = await fetch(
            `${this.baseUrl}/admin/dashboard-stats?timeframe=${timeframe}`,
            {
                method: 'GET',
                credentials: 'include'
            }
        );
        return await response.json();
    }

    async submitSurvey(forceId, responses, imageData = null) {
        const payload = {
            force_id: forceId,
            responses: responses
        };
        if (imageData) {
            payload.image_data = imageData;
        }

        const response = await fetch(`${this.baseUrl}/survey/submit`, {
            method: 'POST',
            headers: this.headers,
            credentials: 'include',
            body: JSON.stringify(payload)
        });
        return await response.json();
    }
}

// Usage example
const client = new SATHIClient();
const isLoggedIn = await client.login('100000001', 'admin123');
if (isLoggedIn) {
    const stats = await client.getDashboardStats('30d');
    console.log(`Total soldiers: ${stats.total_soldiers}`);
}
```

### Error Handling Examples

```python
# Python error handling
try:
    response = requests.post('/api/auth/login', json=login_data)
    response.raise_for_status()  # Raises exception for 4xx/5xx status codes
    
    data = response.json()
    if not data.get('success'):
        print(f"Login failed: {data.get('message')}")
    
except requests.exceptions.RequestException as e:
    print(f"Network error: {e}")
except ValueError as e:
    print(f"Invalid JSON response: {e}")
```

```javascript
// JavaScript error handling
try {
    const response = await fetch('/api/auth/login', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(loginData)
    });

    if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    if (!data.success) {
        throw new Error(data.message || 'Login failed');
    }

} catch (error) {
    console.error('Login error:', error.message);
    // Handle error appropriately in your application
}
```

## API Rate Limiting and Best Practices

### Rate Limits

| Endpoint Category | Rate Limit | Window |
|------------------|------------|---------|
| Authentication | 5 requests | 1 minute |
| Dashboard | 60 requests | 1 minute |
| Survey Submission | 10 requests | 1 minute |
| Image Processing | 20 requests | 1 minute |
| General API | 100 requests | 1 minute |

### Best Practices

1. **Authentication Management**
   ```python
   # Reuse sessions to avoid repeated authentication
   session = requests.Session()
   session.post('/api/auth/login', json=credentials)
   # Use same session for subsequent requests
   ```

2. **Error Retry Logic**
   ```python
   import time
   from functools import wraps

   def retry_on_failure(max_retries=3, delay=1):
       def decorator(func):
           @wraps(func)
           def wrapper(*args, **kwargs):
               for attempt in range(max_retries):
                   try:
                       return func(*args, **kwargs)
                   except requests.exceptions.RequestException as e:
                       if attempt == max_retries - 1:
                           raise
                       time.sleep(delay * (2 ** attempt))  # Exponential backoff
               return wrapper
       return decorator

   @retry_on_failure(max_retries=3)
   def call_api(endpoint, data):
       return requests.post(endpoint, json=data)
   ```

3. **Pagination Handling**
   ```python
   def get_all_soldiers():
       all_soldiers = []
       page = 1
       while True:
           response = requests.get(f'/api/admin/soldiers?page={page}&limit=50')
           data = response.json()
           
           if not data['soldiers']:
               break
               
           all_soldiers.extend(data['soldiers'])
           
           if len(data['soldiers']) < 50:  # Last page
               break
               
           page += 1
       
       return all_soldiers
   ```

## WebSocket API (Real-time Features)

### Connection

```javascript
const socket = new WebSocket('ws://localhost:5000/ws');

socket.onopen = function(event) {
    console.log('Connected to SATHI WebSocket');
    
    // Authenticate WebSocket connection
    socket.send(JSON.stringify({
        type: 'auth',
        token: 'your_session_token'
    }));
};

socket.onmessage = function(event) {
    const data = JSON.parse(event.data);
    
    switch(data.type) {
        case 'risk_alert':
            handleRiskAlert(data.payload);
            break;
        case 'analysis_complete':
            handleAnalysisComplete(data.payload);
            break;
        case 'system_status':
            updateSystemStatus(data.payload);
            break;
    }
};
```

### Real-time Risk Alerts

```javascript
function handleRiskAlert(alert) {
    // alert = {
    //   soldier_id: "123456789",
    //   risk_level: "HIGH",
    //   score: 0.85,
    //   timestamp: "2024-01-15T10:30:00Z",
    //   message: "Elevated depression indicators detected"
    // }
    
    if (alert.risk_level === 'CRITICAL') {
        showUrgentNotification(alert);
    } else {
        updateDashboard(alert);
    }
}
```

## SDK Integration Examples

### React Integration

```typescript
import React, { useState, useEffect } from 'react';
import { SATHIClient } from './services/sathi-client';

interface DashboardStats {
    total_soldiers: number;
    high_risk_soldiers: number;
    critical_alerts: number;
}

export const AdminDashboard: React.FC = () => {
    const [stats, setStats] = useState<DashboardStats | null>(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState<string | null>(null);

    useEffect(() => {
        const fetchStats = async () => {
            try {
                const client = new SATHIClient();
                const isAuthenticated = await client.login(
                    localStorage.getItem('force_id'),
                    localStorage.getItem('password')
                );

                if (isAuthenticated) {
                    const dashboardStats = await client.getDashboardStats('7d');
                    setStats(dashboardStats);
                } else {
                    setError('Authentication failed');
                }
            } catch (err) {
                setError('Failed to fetch dashboard stats');
            } finally {
                setLoading(false);
            }
        };

        fetchStats();
    }, []);

    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error}</div>;

    return (
        <div className="dashboard">
            <h1>SATHI Dashboard</h1>
            <div className="stats-grid">
                <div className="stat-card">
                    <h3>Total Soldiers</h3>
                    <p>{stats?.total_soldiers}</p>
                </div>
                <div className="stat-card">
                    <h3>High Risk</h3>
                    <p>{stats?.high_risk_soldiers}</p>
                </div>
                <div className="stat-card">
                    <h3>Critical Alerts</h3>
                    <p>{stats?.critical_alerts}</p>
                </div>
            </div>
        </div>
    );
};
```

---

## 🔗 Related Documentation

💡 **For Users**: See [User Guide - API Integration](USER_GUIDE.md#api-integration) for end-user API scenarios

🤝 **For Developers**: See [Contributing Guide - API Development](../CONTRIBUTING.md#api-development) for development guidelines

🚀 **For Deployment**: See [Deployment Guide - API Configuration](../DEPLOYMENT_GUIDE.md#api-configuration) for production setup

🔧 **For Issues**: See [Troubleshooting - API Errors](../TROUBLESHOOTING.md#api-errors) for common API problems and solutions

---

This comprehensive API documentation provides all the tools and examples needed to integrate with the SATHI mental health monitoring system. For additional support or questions, please refer to the [Contributing Guide](../CONTRIBUTING.md) or create an issue in the repository.