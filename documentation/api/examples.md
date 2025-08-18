# API Examples - SATHI

Comprehensive examples of SATHI API usage with code samples in multiple programming languages.

## 📋 Table of Contents

1. [Authentication Examples](#authentication-examples)
2. [Survey Management Examples](#survey-management-examples)
3. [Admin Operations Examples](#admin-operations-examples)
4. [Image Processing Examples](#image-processing-examples)
5. [Error Handling Examples](#error-handling-examples)
6. [JavaScript/TypeScript Examples](#javascripttypescript-examples)
7. [Python Examples](#python-examples)
8. [cURL Examples](#curl-examples)

## 🔐 Authentication Examples

### Login Authentication

#### JavaScript/TypeScript
```typescript
// Login with async/await
const login = async (forceId: string, password: string) => {
  try {
    const response = await fetch('/api/auth/login', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        username: forceId,
        password: password
      })
    });

    const data = await response.json();
    
    if (data.success) {
      console.log('Login successful:', data.user);
      localStorage.setItem('sessionId', data.session_id);
      return data.user;
    } else {
      throw new Error(data.message || 'Login failed');
    }
  } catch (error) {
    console.error('Login error:', error);
    throw error;
  }
};

// Usage
try {
  const user = await login('CRPF001234', 'mySecurePassword123!');
  // Redirect to dashboard or update UI
} catch (error) {
  // Display error message to user
  showErrorMessage(error.message);
}
```

#### Python
```python
import requests
import json

def login(force_id, password):
    """Authenticate user and return session information"""
    url = "http://localhost:5000/api/auth/login"
    
    payload = {
        "username": force_id,
        "password": password
    }
    
    headers = {
        "Content-Type": "application/json"
    }
    
    try:
        response = requests.post(url, data=json.dumps(payload), headers=headers)
        response.raise_for_status()  # Raise exception for HTTP errors
        
        data = response.json()
        
        if data.get('success'):
            print(f"Login successful for user: {data['user']['username']}")
            return data['user'], response.cookies
        else:
            raise Exception(data.get('message', 'Login failed'))
            
    except requests.exceptions.RequestException as e:
        print(f"Network error during login: {e}")
        raise
    except Exception as e:
        print(f"Login error: {e}")
        raise

# Usage
try:
    user, session_cookies = login('CRPF001234', 'mySecurePassword123!')
    print(f"Welcome {user['name']}")
except Exception as e:
    print(f"Login failed: {e}")
```

#### cURL
```bash
# Login request
curl -X POST "http://localhost:5000/api/auth/login" \
  -H "Content-Type: application/json" \
  -d '{
    "username": "CRPF001234",
    "password": "mySecurePassword123!"
  }' \
  -c cookies.txt \
  -v

# Response
# {
#   "success": true,
#   "message": "Login successful",
#   "user": {
#     "username": "CRPF001234",
#     "name": "John Doe",
#     "role": "personnel",
#     "permissions": ["read", "survey"],
#     "last_login": "2024-01-01T10:30:00Z"
#   }
# }
```

### Session Management

#### JavaScript/TypeScript
```typescript
// Check session status
const checkSession = async (): Promise<boolean> => {
  try {
    const response = await fetch('/api/auth/session', {
      method: 'GET',
      credentials: 'include' // Include cookies
    });

    const data = await response.json();
    return data.success && data.authenticated;
  } catch (error) {
    console.error('Session check failed:', error);
    return false;
  }
};

// Logout
const logout = async (): Promise<void> => {
  try {
    await fetch('/api/auth/logout', {
      method: 'POST',
      credentials: 'include'
    });
    
    localStorage.removeItem('sessionId');
    // Redirect to login page
    window.location.href = '/login';
  } catch (error) {
    console.error('Logout error:', error);
    // Force redirect anyway
    window.location.href = '/login';
  }
};
```

## 📝 Survey Management Examples

### Get Available Surveys

#### JavaScript/TypeScript
```typescript
interface Survey {
  id: number;
  title: string;
  description: string;
  questionCount: number;
  estimatedTime: string;
  status: 'active' | 'inactive';
}

const getSurveys = async (): Promise<Survey[]> => {
  try {
    const response = await fetch('/api/survey/questionnaires', {
      method: 'GET',
      credentials: 'include',
      headers: {
        'Accept': 'application/json'
      }
    });

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    const data = await response.json();
    
    if (data.success) {
      return data.data.questionnaires;
    } else {
      throw new Error(data.message || 'Failed to fetch surveys');
    }
  } catch (error) {
    console.error('Error fetching surveys:', error);
    throw error;
  }
};

// Usage with error handling
const loadSurveys = async () => {
  try {
    const surveys = await getSurveys();
    console.log(`Found ${surveys.length} surveys`);
    
    surveys.forEach(survey => {
      console.log(`- ${survey.title} (${survey.questionCount} questions)`);
    });
    
    return surveys;
  } catch (error) {
    console.error('Failed to load surveys:', error);
    return [];
  }
};
```

#### Python
```python
import requests

def get_surveys(session_cookies):
    """Fetch available surveys for the user"""
    url = "http://localhost:5000/api/survey/questionnaires"
    
    headers = {
        "Accept": "application/json"
    }
    
    try:
        response = requests.get(url, headers=headers, cookies=session_cookies)
        response.raise_for_status()
        
        data = response.json()
        
        if data.get('success'):
            questionnaires = data['data']['questionnaires']
            print(f"Found {len(questionnaires)} surveys")
            
            for survey in questionnaires:
                print(f"- {survey['title']}: {survey['question_count']} questions")
            
            return questionnaires
        else:
            raise Exception(data.get('message', 'Failed to fetch surveys'))
            
    except requests.exceptions.RequestException as e:
        print(f"Network error fetching surveys: {e}")
        raise

# Usage
try:
    surveys = get_surveys(session_cookies)
    for survey in surveys:
        if survey['is_active']:
            print(f"Available: {survey['title']}")
except Exception as e:
    print(f"Error: {e}")
```

### Submit Survey Response

#### JavaScript/TypeScript
```typescript
interface SurveyResponse {
  questionId: number;
  answer: string | number;
  responseTime: number;
}

interface SurveySubmission {
  forceId: string;
  questionnaireId: number;
  responses: SurveyResponse[];
  surveyMetadata: {
    startTime: string;
    endTime: string;
    deviceInfo: string;
    ipAddress?: string;
  };
}

const submitSurvey = async (submission: SurveySubmission) => {
  try {
    const response = await fetch('/api/survey/submit', {
      method: 'POST',
      credentials: 'include',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(submission)
    });

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    const data = await response.json();
    
    if (data.success) {
      console.log('Survey submitted successfully');
      console.log('Risk Assessment:', data.data.risk_assessment);
      
      return {
        responseId: data.data.response_id,
        riskLevel: data.data.risk_assessment.risk_level,
        score: data.data.risk_assessment.combined_score,
        recommendations: data.data.risk_assessment.recommendations
      };
    } else {
      throw new Error(data.message || 'Survey submission failed');
    }
  } catch (error) {
    console.error('Survey submission error:', error);
    throw error;
  }
};

// Example survey submission
const exampleSubmission: SurveySubmission = {
  forceId: 'CRPF001234',
  questionnaireId: 15,
  responses: [
    {
      questionId: 101,
      answer: '3',
      responseTime: 15.5
    },
    {
      questionId: 102,
      answer: 'Good',
      responseTime: 8.2
    },
    {
      questionId: 103,
      answer: '4',
      responseTime: 12.1
    }
  ],
  surveyMetadata: {
    startTime: '2024-01-01T10:00:00Z',
    endTime: '2024-01-01T10:08:30Z',
    deviceInfo: 'Chrome 96.0 on Windows 10'
  }
};

// Submit the survey
try {
  const result = await submitSurvey(exampleSubmission);
  
  if (result.riskLevel === 'HIGH' || result.riskLevel === 'CRITICAL') {
    // Show alert or redirect to support
    showRiskAlert(result);
  } else {
    // Show success message
    showSuccessMessage('Survey completed successfully');
  }
} catch (error) {
  showErrorMessage(`Failed to submit survey: ${error.message}`);
}
```

#### Python
```python
import requests
import json
from datetime import datetime, timezone

def submit_survey(force_id, questionnaire_id, responses, session_cookies):
    """Submit completed survey responses"""
    url = "http://localhost:5000/api/survey/submit"
    
    # Prepare submission data
    submission_data = {
        "force_id": force_id,
        "questionnaire_id": questionnaire_id,
        "responses": responses,
        "survey_metadata": {
            "start_time": datetime.now(timezone.utc).isoformat(),
            "end_time": datetime.now(timezone.utc).isoformat(),
            "device_info": "Python Script",
            "ip_address": "127.0.0.1"
        }
    }
    
    headers = {
        "Content-Type": "application/json"
    }
    
    try:
        response = requests.post(
            url, 
            data=json.dumps(submission_data), 
            headers=headers, 
            cookies=session_cookies
        )
        response.raise_for_status()
        
        data = response.json()
        
        if data.get('success'):
            risk_assessment = data['data']['risk_assessment']
            
            print(f"Survey submitted successfully!")
            print(f"Response ID: {data['data']['response_id']}")
            print(f"Risk Level: {risk_assessment['risk_level']}")
            print(f"Combined Score: {risk_assessment['combined_score']}")
            
            if risk_assessment.get('recommendations'):
                print("Recommendations:")
                for rec in risk_assessment['recommendations']:
                    print(f"- {rec}")
            
            return data['data']
        else:
            raise Exception(data.get('message', 'Survey submission failed'))
            
    except requests.exceptions.RequestException as e:
        print(f"Network error during survey submission: {e}")
        raise

# Example responses
responses = [
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
]

# Submit survey
try:
    result = submit_survey('CRPF001234', 15, responses, session_cookies)
    
    if result['risk_assessment']['risk_level'] in ['HIGH', 'CRITICAL']:
        print("⚠️  High risk detected - recommend immediate follow-up")
    else:
        print("✅ Survey completed successfully")
        
except Exception as e:
    print(f"Survey submission failed: {e}")
```

## 👨‍💼 Admin Operations Examples

### Get Dashboard Statistics

#### JavaScript/TypeScript
```typescript
interface DashboardStats {
  totalSoldiers: number;
  surveyResponses: number;
  highRiskSoldiers: number;
  criticalAlerts: number;
  surveysToday: number;
  averageResponseTime: number;
  systemHealth: string;
}

const getDashboardStats = async (): Promise<DashboardStats> => {
  try {
    const response = await fetch('/api/admin/dashboard-stats', {
      method: 'GET',
      credentials: 'include',
      headers: {
        'Accept': 'application/json'
      }
    });

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    const data = await response.json();
    
    if (data.success) {
      return data.data;
    } else {
      throw new Error(data.message || 'Failed to fetch dashboard stats');
    }
  } catch (error) {
    console.error('Error fetching dashboard stats:', error);
    throw error;
  }
};

// Usage in React component
const AdminDashboard: React.FC = () => {
  const [stats, setStats] = useState<DashboardStats | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const loadStats = async () => {
      try {
        setLoading(true);
        const dashboardStats = await getDashboardStats();
        setStats(dashboardStats);
        setError(null);
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Unknown error');
      } finally {
        setLoading(false);
      }
    };

    loadStats();
    
    // Refresh every 30 seconds
    const interval = setInterval(loadStats, 30000);
    return () => clearInterval(interval);
  }, []);

  if (loading) return <LoadingSpinner />;
  if (error) return <ErrorMessage message={error} />;
  if (!stats) return <EmptyState />;

  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
      <StatCard 
        title="Total Personnel" 
        value={stats.totalSoldiers} 
        icon="users"
      />
      <StatCard 
        title="Survey Responses" 
        value={stats.surveyResponses} 
        icon="clipboard"
      />
      <StatCard 
        title="High Risk Personnel" 
        value={stats.highRiskSoldiers} 
        icon="exclamation-triangle"
        variant="warning"
      />
      <StatCard 
        title="Critical Alerts" 
        value={stats.criticalAlerts} 
        icon="bell"
        variant="danger"
      />
    </div>
  );
};
```

### Get Soldiers Report with Filters

#### JavaScript/TypeScript
```typescript
interface SoldierReportFilters {
  page?: number;
  limit?: number;
  riskLevel?: 'LOW' | 'MEDIUM' | 'HIGH' | 'CRITICAL';
  unit?: string;
  dateFrom?: string;
  dateTo?: string;
}

interface SoldierInfo {
  forceId: string;
  name: string;
  unit: string;
  riskLevel: string;
  combinedScore: number;
  lastSurveyDate: string;
  surveysCompleted: number;
  emotionAnalysis: {
    dominantEmotion: string;
    confidence: number;
  };
}

const getSoldiersReport = async (filters: SoldierReportFilters = {}) => {
  const queryParams = new URLSearchParams();
  
  // Add filters to query parameters
  Object.entries(filters).forEach(([key, value]) => {
    if (value !== undefined && value !== null) {
      queryParams.append(key, value.toString());
    }
  });

  try {
    const response = await fetch(
      `/api/admin/soldiers-report?${queryParams.toString()}`,
      {
        method: 'GET',
        credentials: 'include',
        headers: {
          'Accept': 'application/json'
        }
      }
    );

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    const data = await response.json();
    
    if (data.success) {
      return {
        soldiers: data.data.soldiers,
        pagination: data.data.pagination,
        summary: data.data.summary
      };
    } else {
      throw new Error(data.message || 'Failed to fetch soldiers report');
    }
  } catch (error) {
    console.error('Error fetching soldiers report:', error);
    throw error;
  }
};

// Usage with filters
const loadHighRiskSoldiers = async () => {
  try {
    const filters: SoldierReportFilters = {
      riskLevel: 'HIGH',
      limit: 50,
      dateFrom: '2024-01-01',
      dateTo: '2024-01-31'
    };

    const report = await getSoldiersReport(filters);
    
    console.log(`Found ${report.soldiers.length} high-risk soldiers`);
    console.log('Risk Distribution:', report.summary.riskDistribution);
    
    // Process high-risk soldiers
    report.soldiers.forEach((soldier: SoldierInfo) => {
      console.log(
        `${soldier.forceId} - ${soldier.name}: Score ${soldier.combinedScore}`
      );
    });
    
    return report;
  } catch (error) {
    console.error('Failed to load high-risk soldiers:', error);
    throw error;
  }
};
```

## 🖼️ Image Processing Examples

### Emotion Detection

#### JavaScript/TypeScript
```typescript
interface EmotionAnalysis {
  emotions: {
    happy: number;
    sad: number;
    angry: number;
    fear: number;
    surprise: number;
    disgust: number;
    neutral: number;
  };
  dominantEmotion: string;
  confidence: number;
  analysisMetadata: {
    modelVersion: string;
    processingTime: string;
    imageQuality: string;
  };
}

const analyzeEmotion = async (imageFile: File, forceId: string): Promise<EmotionAnalysis> => {
  const formData = new FormData();
  formData.append('image', imageFile);
  formData.append('force_id', forceId);

  try {
    const response = await fetch('/api/image/emotion-detection', {
      method: 'POST',
      credentials: 'include',
      body: formData // Don't set Content-Type, let browser set it with boundary
    });

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`);
    }

    const data = await response.json();
    
    if (data.success) {
      return data.data;
    } else {
      throw new Error(data.message || 'Emotion analysis failed');
    }
  } catch (error) {
    console.error('Emotion analysis error:', error);
    throw error;
  }
};

// Usage with file input
const handleImageUpload = async (event: React.ChangeEvent<HTMLInputElement>) => {
  const file = event.target.files?.[0];
  if (!file) return;

  // Validate file type
  if (!file.type.startsWith('image/')) {
    alert('Please select an image file');
    return;
  }

  // Validate file size (max 10MB)
  if (file.size > 10 * 1024 * 1024) {
    alert('File size must be less than 10MB');
    return;
  }

  try {
    setAnalyzing(true);
    const analysis = await analyzeEmotion(file, 'CRPF001234');
    
    console.log('Emotion Analysis Results:');
    console.log(`Dominant Emotion: ${analysis.dominantEmotion}`);
    console.log(`Confidence: ${(analysis.confidence * 100).toFixed(1)}%`);
    
    // Display emotion breakdown
    Object.entries(analysis.emotions).forEach(([emotion, score]) => {
      console.log(`${emotion}: ${(score * 100).toFixed(1)}%`);
    });

    setEmotionData(analysis);
  } catch (error) {
    console.error('Failed to analyze emotion:', error);
    alert(`Emotion analysis failed: ${error.message}`);
  } finally {
    setAnalyzing(false);
  }
};
```

#### Python
```python
import requests

def analyze_emotion(image_path, force_id, session_cookies):
    """Analyze emotions from facial image"""
    url = "http://localhost:5000/api/image/emotion-detection"
    
    try:
        with open(image_path, 'rb') as image_file:
            files = {
                'image': ('image.jpg', image_file, 'image/jpeg')
            }
            data = {
                'force_id': force_id
            }
            
            response = requests.post(
                url, 
                files=files, 
                data=data, 
                cookies=session_cookies
            )
            response.raise_for_status()
            
            result = response.json()
            
            if result.get('success'):
                analysis = result['data']
                
                print("Emotion Analysis Results:")
                print(f"Dominant Emotion: {analysis['dominant_emotion']}")
                print(f"Confidence: {analysis['confidence']:.2%}")
                print("\nEmotion Breakdown:")
                
                for emotion, score in analysis['emotions'].items():
                    print(f"  {emotion.capitalize()}: {score:.2%}")
                
                print(f"\nProcessing Time: {analysis['analysis_metadata']['processing_time']}")
                print(f"Image Quality: {analysis['analysis_metadata']['image_quality']}")
                
                return analysis
            else:
                raise Exception(result.get('message', 'Emotion analysis failed'))
                
    except FileNotFoundError:
        print(f"Image file not found: {image_path}")
        raise
    except requests.exceptions.RequestException as e:
        print(f"Network error during emotion analysis: {e}")
        raise

# Usage
try:
    emotion_data = analyze_emotion('selfie.jpg', 'CRPF001234', session_cookies)
    
    # Check for concerning emotions
    if emotion_data['emotions']['sad'] > 0.6 or emotion_data['emotions']['angry'] > 0.5:
        print("⚠️  Detected concerning emotional state")
    elif emotion_data['emotions']['happy'] > 0.5:
        print("😊 Positive emotional state detected")
    else:
        print("😐 Neutral emotional state")
        
except Exception as e:
    print(f"Emotion analysis failed: {e}")
```

### Face Recognition

#### cURL
```bash
# Face recognition with image upload
curl -X POST "http://localhost:5000/api/image/face-recognition" \
  -H "Accept: application/json" \
  -b cookies.txt \
  -F "image=@selfie.jpg" \
  -F "force_id=CRPF001234"

# Response
# {
#   "success": true,
#   "data": {
#     "face_detected": true,
#     "confidence": 0.92,
#     "soldier_matched": {
#       "force_id": "CRPF001234",
#       "name": "John Doe",
#       "match_confidence": 0.95
#     },
#     "face_quality": {
#       "brightness": "good",
#       "sharpness": "excellent",
#       "angle": "optimal"
#     }
#   }
# }
```

## 🚨 Error Handling Examples

### Comprehensive Error Handling

#### JavaScript/TypeScript
```typescript
// Custom error types
class ApiError extends Error {
  constructor(
    message: string,
    public status: number,
    public code?: string
  ) {
    super(message);
    this.name = 'ApiError';
  }
}

class NetworkError extends Error {
  constructor(message: string) {
    super(message);
    this.name = 'NetworkError';
  }
}

// Generic API request function with error handling
const apiRequest = async <T>(
  endpoint: string,
  options: RequestInit = {}
): Promise<T> => {
  const defaultOptions: RequestInit = {
    credentials: 'include',
    headers: {
      'Content-Type': 'application/json',
      'Accept': 'application/json'
    }
  };

  const requestOptions = { ...defaultOptions, ...options };

  try {
    const response = await fetch(`/api${endpoint}`, requestOptions);

    // Handle different HTTP status codes
    if (response.status === 401) {
      // Unauthorized - redirect to login
      window.location.href = '/login';
      throw new ApiError('Authentication required', 401, 'UNAUTHORIZED');
    }

    if (response.status === 403) {
      throw new ApiError('Insufficient permissions', 403, 'FORBIDDEN');
    }

    if (response.status === 404) {
      throw new ApiError('Resource not found', 404, 'NOT_FOUND');
    }

    if (response.status === 429) {
      throw new ApiError('Too many requests', 429, 'RATE_LIMITED');
    }

    if (!response.ok) {
      const errorText = await response.text();
      throw new ApiError(
        `HTTP ${response.status}: ${response.statusText}`,
        response.status
      );
    }

    const data = await response.json();

    if (!data.success) {
      throw new ApiError(
        data.message || 'API request failed',
        response.status,
        data.error?.code
      );
    }

    return data.data;
  } catch (error) {
    if (error instanceof TypeError && error.message.includes('fetch')) {
      throw new NetworkError('Network connection failed');
    }
    
    throw error;
  }
};

// Usage with comprehensive error handling
const handleSurveySubmission = async (surveyData: SurveySubmission) => {
  try {
    const result = await apiRequest<SurveyResult>('/survey/submit', {
      method: 'POST',
      body: JSON.stringify(surveyData)
    });

    // Success handling
    showSuccessMessage('Survey submitted successfully');
    return result;

  } catch (error) {
    if (error instanceof NetworkError) {
      showErrorMessage('Network connection failed. Please check your internet connection.');
    } else if (error instanceof ApiError) {
      switch (error.code) {
        case 'VALIDATION_ERROR':
          showErrorMessage('Please check your survey responses and try again.');
          break;
        case 'RATE_LIMITED':
          showErrorMessage('Too many requests. Please wait a moment and try again.');
          break;
        case 'UNAUTHORIZED':
          // Handled automatically by redirect
          break;
        default:
          showErrorMessage(`Survey submission failed: ${error.message}`);
      }
    } else {
      showErrorMessage('An unexpected error occurred. Please try again.');
      console.error('Unexpected error:', error);
    }
    
    throw error;
  }
};
```

#### Python Error Handling
```python
import requests
import json
from typing import Dict, Any, Optional

class SATHIApiError(Exception):
    """Custom exception for SATHI API errors"""
    def __init__(self, message: str, status_code: int = None, error_code: str = None):
        super().__init__(message)
        self.status_code = status_code
        self.error_code = error_code

class SATHIClient:
    def __init__(self, base_url: str = "http://localhost:5000/api"):
        self.base_url = base_url
        self.session = requests.Session()
        self.session.headers.update({
            'Content-Type': 'application/json',
            'Accept': 'application/json'
        })

    def _handle_response(self, response: requests.Response) -> Dict[str, Any]:
        """Handle API response with comprehensive error checking"""
        try:
            data = response.json()
        except json.JSONDecodeError:
            raise SATHIApiError(
                f"Invalid JSON response: {response.text}",
                response.status_code
            )

        if response.status_code == 401:
            raise SATHIApiError(
                "Authentication required", 
                401, 
                "UNAUTHORIZED"
            )
        elif response.status_code == 403:
            raise SATHIApiError(
                "Insufficient permissions", 
                403, 
                "FORBIDDEN"
            )
        elif response.status_code == 404:
            raise SATHIApiError(
                "Resource not found", 
                404, 
                "NOT_FOUND"
            )
        elif response.status_code == 429:
            raise SATHIApiError(
                "Rate limit exceeded", 
                429, 
                "RATE_LIMITED"
            )
        elif not response.ok:
            error_message = data.get('message', f'HTTP {response.status_code}')
            error_code = data.get('error', {}).get('code')
            raise SATHIApiError(error_message, response.status_code, error_code)

        if not data.get('success', False):
            error_message = data.get('message', 'API request failed')
            error_code = data.get('error', {}).get('code')
            raise SATHIApiError(error_message, response.status_code, error_code)

        return data.get('data')

    def login(self, force_id: str, password: str) -> Dict[str, Any]:
        """Login with error handling"""
        try:
            response = self.session.post(
                f"{self.base_url}/auth/login",
                json={"username": force_id, "password": password},
                timeout=10
            )
            
            return self._handle_response(response)
            
        except requests.exceptions.ConnectionError:
            raise SATHIApiError("Failed to connect to SATHI server")
        except requests.exceptions.Timeout:
            raise SATHIApiError("Request timed out")
        except requests.exceptions.RequestException as e:
            raise SATHIApiError(f"Network error: {str(e)}")

    def submit_survey(self, survey_data: Dict[str, Any]) -> Dict[str, Any]:
        """Submit survey with comprehensive error handling"""
        try:
            response = self.session.post(
                f"{self.base_url}/survey/submit",
                json=survey_data,
                timeout=30  # Longer timeout for survey processing
            )
            
            return self._handle_response(response)
            
        except requests.exceptions.ConnectionError:
            raise SATHIApiError("Failed to connect to SATHI server")
        except requests.exceptions.Timeout:
            raise SATHIApiError("Survey submission timed out")
        except requests.exceptions.RequestException as e:
            raise SATHIApiError(f"Network error: {str(e)}")

# Usage with error handling
def main():
    client = SATHIClient()
    
    try:
        # Login
        user_data = client.login('CRPF001234', 'password123')
        print(f"Login successful: {user_data['name']}")
        
        # Submit survey
        survey_data = {
            "force_id": "CRPF001234",
            "questionnaire_id": 15,
            "responses": [
                {"question_id": 101, "answer": "3", "response_time": 15.5}
            ]
        }
        
        result = client.submit_survey(survey_data)
        print(f"Survey submitted: {result['response_id']}")
        
    except SATHIApiError as e:
        if e.error_code == "UNAUTHORIZED":
            print("Please check your credentials and try again")
        elif e.error_code == "VALIDATION_ERROR":
            print("Please check your survey responses")
        elif e.error_code == "RATE_LIMITED":
            print("Too many requests. Please wait and try again")
        else:
            print(f"API Error: {e}")
    except Exception as e:
        print(f"Unexpected error: {e}")

if __name__ == "__main__":
    main()
```

This comprehensive API examples documentation provides practical, real-world examples of how to interact with the SATHI API across different programming languages and scenarios. Each example includes proper error handling, type safety (where applicable), and best practices for production use.