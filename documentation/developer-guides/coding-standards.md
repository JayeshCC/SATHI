# SATHI Coding Standards

Comprehensive coding standards and conventions for the SATHI (CRPF Mental Health Monitoring System) project.

## 📋 Table of Contents

1. [General Principles](#general-principles)
2. [Python Backend Standards](#python-backend-standards)
3. [TypeScript Frontend Standards](#typescript-frontend-standards)
4. [Database Standards](#database-standards)
5. [API Design Standards](#api-design-standards)
6. [Documentation Standards](#documentation-standards)
7. [Security Standards](#security-standards)
8. [Testing Standards](#testing-standards)
9. [Performance Standards](#performance-standards)
10. [Code Review Guidelines](#code-review-guidelines)

## 🎯 General Principles

### Core Values

1. **Security First**: Healthcare data protection is paramount
2. **Clarity over Cleverness**: Code should be readable and maintainable
3. **Consistency**: Follow established patterns and conventions
4. **Performance**: Optimize for efficiency without sacrificing readability
5. **Accessibility**: Ensure the system is accessible to all users

### Universal Standards

- **English Comments and Documentation**: All code comments and documentation in English
- **UTF-8 Encoding**: Use UTF-8 encoding for all files
- **Line Endings**: Use LF (Unix-style) line endings
- **Indentation**: Use spaces, not tabs (2 spaces for JS/TS, 4 spaces for Python)
- **File Naming**: Use kebab-case for files, PascalCase for components

## 🐍 Python Backend Standards

### Code Style

Follow PEP 8 with these specific guidelines:

```python
# File: user_service.py
"""
User management service for SATHI system.

This module provides user authentication, authorization, and management
functionality for the CRPF mental health monitoring system.
"""

import logging
from typing import Dict, List, Optional, Union
from datetime import datetime, timedelta

from flask import request, session
from werkzeug.security import check_password_hash, generate_password_hash

from models.user import User
from utils.validators import validate_force_id, validate_password
from utils.security import requires_auth, audit_log


logger = logging.getLogger(__name__)


class UserService:
    """
    Service class for user management operations.
    
    Handles user authentication, session management, and user data operations
    with proper security controls and audit logging.
    """
    
    def __init__(self, db_session):
        """
        Initialize the UserService.
        
        Args:
            db_session: Database session instance
        """
        self.db_session = db_session
    
    @audit_log('user_login_attempt')
    def authenticate_user(self, force_id: str, password: str) -> Optional[Dict]:
        """
        Authenticate a user with force ID and password.
        
        Args:
            force_id: CRPF Force ID (e.g., 'CRPF001234')
            password: User password
            
        Returns:
            dict: User information if authentication successful, None otherwise
            
        Raises:
            ValidationError: If force_id format is invalid
            AuthenticationError: If authentication fails
            
        Example:
            >>> user_service = UserService(db_session)
            >>> user = user_service.authenticate_user('CRPF001234', 'password123')
            >>> if user:
            ...     print(f"Welcome {user['name']}")
        """
        # Input validation
        if not validate_force_id(force_id):
            logger.warning(f"Invalid force ID format attempted: {force_id}")
            raise ValidationError("Invalid Force ID format")
        
        if not validate_password(password):
            logger.warning(f"Password validation failed for: {force_id}")
            raise ValidationError("Invalid password format")
        
        try:
            # Database query with proper error handling
            user = self.db_session.query(User).filter_by(
                force_id=force_id.upper(),
                is_active=True
            ).first()
            
            if not user:
                logger.warning(f"User not found: {force_id}")
                return None
            
            if not check_password_hash(user.password_hash, password):
                logger.warning(f"Invalid password for user: {force_id}")
                return None
            
            # Update last login
            user.last_login = datetime.utcnow()
            self.db_session.commit()
            
            logger.info(f"User authenticated successfully: {force_id}")
            
            return {
                'user_id': user.id,
                'force_id': user.force_id,
                'name': user.name,
                'role': user.role,
                'permissions': user.get_permissions(),
                'last_login': user.last_login.isoformat()
            }
            
        except Exception as e:
            logger.error(f"Authentication error for {force_id}: {str(e)}")
            self.db_session.rollback()
            raise AuthenticationError("Authentication failed")
    
    @requires_auth('admin')
    def create_user(self, user_data: Dict) -> Dict:
        """
        Create a new user account.
        
        Args:
            user_data: Dictionary containing user information
            
        Returns:
            dict: Created user information
            
        Raises:
            ValidationError: If user data is invalid
            DuplicateUserError: If user already exists
        """
        # Implementation with proper validation and error handling
        pass


# Constants - Use UPPER_CASE for constants
MAX_LOGIN_ATTEMPTS = 3
SESSION_TIMEOUT_MINUTES = 30
PASSWORD_MIN_LENGTH = 8

# Configuration - Use descriptive names
AUTH_CONFIG = {
    'session_timeout': timedelta(minutes=SESSION_TIMEOUT_MINUTES),
    'max_login_attempts': MAX_LOGIN_ATTEMPTS,
    'password_requirements': {
        'min_length': PASSWORD_MIN_LENGTH,
        'require_uppercase': True,
        'require_lowercase': True,
        'require_digit': True,
        'require_special': True
    }
}
```

### Key Python Standards

**1. Naming Conventions**
```python
# Variables and functions: snake_case
user_name = "john_doe"
def get_user_data():
    pass

# Classes: PascalCase
class UserService:
    pass

# Constants: UPPER_CASE
MAX_FILE_SIZE = 10485760

# Private methods: leading underscore
def _validate_input(self, data):
    pass
```

**2. Type Hints**
```python
# Always use type hints for function signatures
def process_survey_data(
    survey_id: int,
    responses: List[Dict[str, Any]],
    user_id: Optional[str] = None
) -> Tuple[bool, Dict[str, Any]]:
    """Process survey responses and return results."""
    pass

# Use Union for multiple types
def get_user(identifier: Union[int, str]) -> Optional[User]:
    pass
```

**3. Error Handling**
```python
# Use specific exceptions
class ValidationError(Exception):
    """Raised when input validation fails."""
    pass

class AuthenticationError(Exception):
    """Raised when authentication fails."""
    pass

# Proper exception handling
try:
    result = process_data(input_data)
except ValidationError as e:
    logger.error(f"Validation failed: {e}")
    return {"success": False, "error": str(e)}
except Exception as e:
    logger.error(f"Unexpected error: {e}")
    return {"success": False, "error": "Internal server error"}
```

**4. Logging**
```python
import logging

# Configure logger at module level
logger = logging.getLogger(__name__)

# Use appropriate log levels
logger.debug("Debugging information")
logger.info("User logged in successfully")
logger.warning("Invalid input detected")
logger.error("Database connection failed")
logger.critical("System security breach detected")
```

## 📱 TypeScript Frontend Standards

### Code Style

```typescript
// File: UserService.ts
/**
 * User management service for SATHI frontend.
 * 
 * Provides user authentication, session management, and user data
 * operations with proper error handling and type safety.
 */

import { useState, useCallback, useEffect } from 'react';
import axios, { AxiosResponse } from 'axios';

import { User, LoginCredentials, ApiResponse } from '../types/auth.types';
import { handleApiError, validateForceId } from '../utils/helpers';
import { API_ENDPOINTS } from '../utils/constants';

// Interfaces - Use PascalCase
interface UserServiceConfig {
  baseUrl: string;
  timeout: number;
  retryAttempts: number;
}

interface LoginResult {
  success: boolean;
  user?: User;
  error?: string;
}

// Types - Use PascalCase with descriptive names
type UserRole = 'personnel' | 'healthcare_provider' | 'admin';
type AuthStatus = 'authenticated' | 'unauthenticated' | 'loading' | 'error';

// Constants - Use UPPER_CASE
const DEFAULT_TIMEOUT = 10000;
const MAX_RETRY_ATTEMPTS = 3;
const SESSION_STORAGE_KEY = 'sathi_session';

/**
 * User service class for authentication and user management.
 * 
 * Provides methods for login, logout, session management, and user data
 * operations with proper error handling and TypeScript type safety.
 */
class UserService {
  private config: UserServiceConfig;
  private authStatus: AuthStatus = 'loading';

  constructor(config: Partial<UserServiceConfig> = {}) {
    this.config = {
      baseUrl: process.env.REACT_APP_API_URL || 'http://localhost:5000/api',
      timeout: DEFAULT_TIMEOUT,
      retryAttempts: MAX_RETRY_ATTEMPTS,
      ...config
    };
  }

  /**
   * Authenticate user with force ID and password.
   * 
   * @param credentials - User login credentials
   * @returns Promise resolving to login result
   * 
   * @example
   * ```typescript
   * const userService = new UserService();
   * const result = await userService.login({
   *   forceId: 'CRPF001234',
   *   password: 'securePassword123'
   * });
   * 
   * if (result.success) {
   *   console.log('Login successful:', result.user);
   * } else {
   *   console.error('Login failed:', result.error);
   * }
   * ```
   */
  public async login(credentials: LoginCredentials): Promise<LoginResult> {
    try {
      // Input validation
      if (!validateForceId(credentials.forceId)) {
        return {
          success: false,
          error: 'Invalid Force ID format'
        };
      }

      if (!credentials.password || credentials.password.length < 8) {
        return {
          success: false,
          error: 'Password must be at least 8 characters'
        };
      }

      // API call with proper error handling
      const response: AxiosResponse<ApiResponse<User>> = await axios.post(
        `${this.config.baseUrl}/auth/login`,
        {
          username: credentials.forceId,
          password: credentials.password
        },
        {
          timeout: this.config.timeout,
          headers: {
            'Content-Type': 'application/json'
          }
        }
      );

      if (response.data.success && response.data.data) {
        // Store session information securely
        sessionStorage.setItem(SESSION_STORAGE_KEY, JSON.stringify({
          user: response.data.data,
          timestamp: Date.now()
        }));

        this.authStatus = 'authenticated';

        return {
          success: true,
          user: response.data.data
        };
      } else {
        return {
          success: false,
          error: response.data.message || 'Login failed'
        };
      }

    } catch (error) {
      const errorMessage = handleApiError(error);
      console.error('Login error:', errorMessage);
      
      return {
        success: false,
        error: errorMessage
      };
    }
  }

  /**
   * Logout current user and clear session.
   * 
   * @returns Promise resolving to logout success status
   */
  public async logout(): Promise<boolean> {
    try {
      await axios.post(`${this.config.baseUrl}/auth/logout`);
      
      // Clear session data
      sessionStorage.removeItem(SESSION_STORAGE_KEY);
      this.authStatus = 'unauthenticated';
      
      return true;
    } catch (error) {
      console.error('Logout error:', handleApiError(error));
      return false;
    }
  }

  /**
   * Get current authentication status.
   * 
   * @returns Current authentication status
   */
  public getAuthStatus(): AuthStatus {
    return this.authStatus;
  }

  /**
   * Check if user has specific permission.
   * 
   * @param permission - Permission to check
   * @returns True if user has permission
   */
  public hasPermission(permission: string): boolean {
    const session = this.getCurrentSession();
    return session?.user?.permissions?.includes(permission) ?? false;
  }

  /**
   * Get current user session from storage.
   * 
   * @returns Current session data or null
   */
  private getCurrentSession(): { user: User; timestamp: number } | null {
    try {
      const sessionData = sessionStorage.getItem(SESSION_STORAGE_KEY);
      return sessionData ? JSON.parse(sessionData) : null;
    } catch {
      return null;
    }
  }
}

// React Hook for user authentication
export const useAuth = () => {
  const [authStatus, setAuthStatus] = useState<AuthStatus>('loading');
  const [user, setUser] = useState<User | null>(null);

  const login = useCallback(async (credentials: LoginCredentials): Promise<LoginResult> => {
    setAuthStatus('loading');
    
    const userService = new UserService();
    const result = await userService.login(credentials);
    
    if (result.success && result.user) {
      setUser(result.user);
      setAuthStatus('authenticated');
    } else {
      setAuthStatus('unauthenticated');
    }
    
    return result;
  }, []);

  const logout = useCallback(async (): Promise<boolean> => {
    const userService = new UserService();
    const success = await userService.logout();
    
    if (success) {
      setUser(null);
      setAuthStatus('unauthenticated');
    }
    
    return success;
  }, []);

  return {
    authStatus,
    user,
    login,
    logout,
    isAuthenticated: authStatus === 'authenticated',
    isLoading: authStatus === 'loading'
  };
};

export { UserService, type UserRole, type AuthStatus };
```

### Key TypeScript Standards

**1. Naming Conventions**
```typescript
// Variables and functions: camelCase
const userName = 'john_doe';
const getUserData = () => {};

// Types and Interfaces: PascalCase
interface UserData {
  forceId: string;
  name: string;
}

type UserRole = 'admin' | 'user';

// Components: PascalCase
const UserDashboard: React.FC = () => {};

// Constants: UPPER_CASE
const MAX_FILE_SIZE = 10485760;
const API_ENDPOINTS = {
  LOGIN: '/auth/login',
  LOGOUT: '/auth/logout'
};
```

**2. Type Definitions**
```typescript
// Use strict typing
interface SurveyResponse {
  questionId: number;
  answer: string | number;
  responseTime: number;
}

// Use generic types when appropriate
interface ApiResponse<T> {
  success: boolean;
  data?: T;
  message: string;
  error?: string;
}

// Use utility types
type PartialUser = Partial<User>;
type RequiredLogin = Required<Pick<User, 'forceId' | 'password'>>;
```

**3. Component Standards**
```typescript
// Functional components with proper typing
interface ComponentProps {
  title: string;
  data: SurveyData[];
  onSubmit: (data: SurveyResponse) => Promise<void>;
  className?: string;
}

const SurveyComponent: React.FC<ComponentProps> = ({
  title,
  data,
  onSubmit,
  className = ''
}) => {
  // Component implementation
  return (
    <div className={`survey-container ${className}`}>
      <h2>{title}</h2>
      {/* Component content */}
    </div>
  );
};

export default SurveyComponent;
```

## 🗄️ Database Standards

### Schema Design

```sql
-- Table naming: snake_case, plural nouns
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    force_id CHAR(10) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255),
    password_hash VARCHAR(255) NOT NULL,
    role ENUM('personnel', 'healthcare_provider', 'admin') NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    -- Indexes for performance
    INDEX idx_force_id (force_id),
    INDEX idx_role (role),
    INDEX idx_active (is_active),
    INDEX idx_created_at (created_at)
);

-- Survey responses with proper relationships
CREATE TABLE survey_responses (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    response_id VARCHAR(50) UNIQUE NOT NULL,
    force_id CHAR(10) NOT NULL,
    questionnaire_id INT NOT NULL,
    submitted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    risk_level ENUM('LOW', 'MEDIUM', 'HIGH', 'CRITICAL'),
    combined_score DECIMAL(5,2),
    
    -- Foreign key constraints
    FOREIGN KEY (force_id) REFERENCES users(force_id) ON DELETE CASCADE,
    FOREIGN KEY (questionnaire_id) REFERENCES questionnaires(id) ON DELETE CASCADE,
    
    -- Indexes
    INDEX idx_force_id (force_id),
    INDEX idx_questionnaire (questionnaire_id),
    INDEX idx_submitted_at (submitted_at),
    INDEX idx_risk_level (risk_level)
);
```

### Query Standards

```python
# Use parameterized queries to prevent SQL injection
def get_user_surveys(force_id: str, limit: int = 10) -> List[Dict]:
    """Get recent surveys for a user with proper pagination."""
    query = """
        SELECT 
            sr.response_id,
            sr.submitted_at,
            sr.risk_level,
            sr.combined_score,
            q.title as questionnaire_title
        FROM survey_responses sr
        JOIN questionnaires q ON sr.questionnaire_id = q.id
        WHERE sr.force_id = %s
        ORDER BY sr.submitted_at DESC
        LIMIT %s
    """
    
    # Use parameterized query
    results = db_session.execute(query, (force_id, limit)).fetchall()
    
    return [dict(row) for row in results]

# Use transactions for data consistency
def submit_survey_response(response_data: Dict) -> bool:
    """Submit survey response with transaction handling."""
    try:
        with db_session.begin():
            # Insert response
            response = SurveyResponse(**response_data)
            db_session.add(response)
            
            # Update user statistics
            user = db_session.query(User).filter_by(
                force_id=response_data['force_id']
            ).first()
            user.last_survey_date = datetime.utcnow()
            
            # Commit transaction
            db_session.commit()
            return True
            
    except Exception as e:
        db_session.rollback()
        logger.error(f"Survey submission failed: {e}")
        return False
```

## 🔗 API Design Standards

### RESTful Endpoints

```python
# Use consistent URL patterns
@api.route('/api/admin/users', methods=['GET'])
@api.route('/api/admin/users/<int:user_id>', methods=['GET', 'PUT', 'DELETE'])
@api.route('/api/survey/responses', methods=['GET', 'POST'])
@api.route('/api/survey/responses/<string:response_id>', methods=['GET'])

# Consistent response format
def create_response(success: bool, data: Any = None, message: str = '', 
                   error: Any = None) -> Dict:
    """Create standardized API response."""
    return {
        'success': success,
        'data': data,
        'message': message,
        'error': error,
        'timestamp': datetime.utcnow().isoformat(),
        'request_id': generate_request_id()
    }

# HTTP status codes
@api.route('/api/users', methods=['POST'])
def create_user():
    try:
        # Create user logic
        user = create_new_user(request.json)
        return create_response(
            success=True,
            data=user,
            message='User created successfully'
        ), 201  # Created
        
    except ValidationError as e:
        return create_response(
            success=False,
            error=str(e),
            message='Validation failed'
        ), 400  # Bad Request
        
    except DuplicateUserError:
        return create_response(
            success=False,
            message='User already exists'
        ), 409  # Conflict
        
    except Exception as e:
        logger.error(f"User creation failed: {e}")
        return create_response(
            success=False,
            message='Internal server error'
        ), 500  # Internal Server Error
```

## 📝 Documentation Standards

### Code Documentation

```python
def analyze_mental_health_score(
    survey_data: Dict[str, Any],
    user_profile: Dict[str, Any],
    historical_data: Optional[List[Dict]] = None
) -> Dict[str, Any]:
    """
    Analyze mental health score based on survey responses and user profile.
    
    This function processes survey responses using machine learning models
    to determine risk levels and provide recommendations for CRPF personnel.
    
    Args:
        survey_data: Dictionary containing survey responses with keys:
            - 'responses': List of question-answer pairs
            - 'completion_time': Total time taken for survey
            - 'emotion_data': Optional emotion detection results
        user_profile: User information including:
            - 'force_id': CRPF Force ID
            - 'age': User age
            - 'unit': CRPF unit assignment
            - 'service_years': Years of service
        historical_data: Optional list of previous survey results for trend analysis
    
    Returns:
        Dictionary containing analysis results:
            {
                'risk_level': str,  # 'LOW', 'MEDIUM', 'HIGH', 'CRITICAL'
                'combined_score': float,  # 0-100 scale
                'confidence': float,  # 0-1 confidence level
                'recommendations': List[str],  # Suggested actions
                'trend_analysis': Dict,  # Historical trend data
                'next_assessment_date': str  # ISO format date
            }
    
    Raises:
        ValidationError: If survey_data or user_profile is invalid
        ModelError: If ML model processing fails
        DataError: If historical_data format is incorrect
    
    Example:
        >>> survey_data = {
        ...     'responses': [{'question_id': 1, 'answer': 3}],
        ...     'completion_time': 300
        ... }
        >>> user_profile = {
        ...     'force_id': 'CRPF001234',
        ...     'age': 35,
        ...     'unit': 'Alpha Company'
        ... }
        >>> result = analyze_mental_health_score(survey_data, user_profile)
        >>> print(f"Risk Level: {result['risk_level']}")
        Risk Level: MEDIUM
    
    Note:
        This function uses sensitive health data and should only be called
        by authorized healthcare providers or system administrators.
    """
    # Implementation details...
```

### API Documentation

```python
@api.route('/api/survey/submit', methods=['POST'])
@swag_from({
    'tags': ['Survey'],
    'summary': 'Submit completed survey responses',
    'description': 'Process and store survey responses with risk analysis',
    'parameters': [
        {
            'name': 'body',
            'in': 'body',
            'required': True,
            'schema': {
                'type': 'object',
                'properties': {
                    'force_id': {'type': 'string', 'example': 'CRPF001234'},
                    'questionnaire_id': {'type': 'integer', 'example': 15},
                    'responses': {
                        'type': 'array',
                        'items': {
                            'type': 'object',
                            'properties': {
                                'question_id': {'type': 'integer'},
                                'answer': {'type': 'string'},
                                'response_time': {'type': 'number'}
                            }
                        }
                    }
                }
            }
        }
    ],
    'responses': {
        '201': {
            'description': 'Survey submitted successfully',
            'schema': {
                'type': 'object',
                'properties': {
                    'success': {'type': 'boolean'},
                    'data': {
                        'type': 'object',
                        'properties': {
                            'response_id': {'type': 'string'},
                            'risk_level': {'type': 'string'},
                            'combined_score': {'type': 'number'}
                        }
                    }
                }
            }
        }
    }
})
def submit_survey():
    """Submit survey endpoint with comprehensive documentation."""
    # Implementation...
```

## 🔒 Security Standards

### Input Validation

```python
from marshmallow import Schema, fields, validate, ValidationError

class SurveySubmissionSchema(Schema):
    """Schema for validating survey submission data."""
    
    force_id = fields.Str(
        required=True,
        validate=validate.Regexp(
            r'^CRPF\d{6}$',
            error='Force ID must be in format CRPF######'
        )
    )
    questionnaire_id = fields.Int(
        required=True,
        validate=validate.Range(min=1)
    )
    responses = fields.List(
        fields.Dict(),
        required=True,
        validate=validate.Length(min=1)
    )

def validate_survey_submission(data: Dict) -> Dict:
    """Validate survey submission data."""
    schema = SurveySubmissionSchema()
    try:
        return schema.load(data)
    except ValidationError as e:
        raise ValidationError(f"Invalid survey data: {e.messages}")
```

### Authentication & Authorization

```python
from functools import wraps
from flask import session, request, jsonify

def requires_auth(permission: str = None):
    """Decorator for requiring authentication and optional permission."""
    def decorator(f):
        @wraps(f)
        def decorated_function(*args, **kwargs):
            # Check session
            if 'user_id' not in session:
                return jsonify({
                    'success': False,
                    'message': 'Authentication required'
                }), 401
            
            # Check permission if specified
            if permission and not user_has_permission(session['user_id'], permission):
                return jsonify({
                    'success': False,
                    'message': 'Insufficient permissions'
                }), 403
            
            return f(*args, **kwargs)
        return decorated_function
    return decorator

@requires_auth('admin')
def admin_function():
    """Function requiring admin permission."""
    pass
```

## ✅ Testing Standards

### Unit Testing

```python
# test_user_service.py
import pytest
from unittest.mock import Mock, patch
from datetime import datetime

from services.user_service import UserService
from models.user import User
from exceptions import ValidationError, AuthenticationError


class TestUserService:
    """Test cases for UserService class."""
    
    @pytest.fixture
    def mock_db_session(self):
        """Mock database session for testing."""
        return Mock()
    
    @pytest.fixture
    def user_service(self, mock_db_session):
        """User service instance for testing."""
        return UserService(mock_db_session)
    
    @pytest.fixture
    def sample_user(self):
        """Sample user data for testing."""
        return User(
            id=1,
            force_id='CRPF001234',
            name='John Doe',
            role='personnel',
            password_hash='hashed_password',
            is_active=True,
            last_login=datetime.utcnow()
        )
    
    def test_authenticate_user_success(self, user_service, mock_db_session, sample_user):
        """Test successful user authentication."""
        # Arrange
        mock_db_session.query.return_value.filter_by.return_value.first.return_value = sample_user
        
        with patch('werkzeug.security.check_password_hash', return_value=True):
            # Act
            result = user_service.authenticate_user('CRPF001234', 'correct_password')
            
            # Assert
            assert result is not None
            assert result['force_id'] == 'CRPF001234'
            assert result['name'] == 'John Doe'
            assert result['role'] == 'personnel'
    
    def test_authenticate_user_invalid_force_id(self, user_service):
        """Test authentication with invalid force ID format."""
        # Act & Assert
        with pytest.raises(ValidationError, match="Invalid Force ID format"):
            user_service.authenticate_user('invalid_id', 'password')
    
    def test_authenticate_user_wrong_password(self, user_service, mock_db_session, sample_user):
        """Test authentication with wrong password."""
        # Arrange
        mock_db_session.query.return_value.filter_by.return_value.first.return_value = sample_user
        
        with patch('werkzeug.security.check_password_hash', return_value=False):
            # Act
            result = user_service.authenticate_user('CRPF001234', 'wrong_password')
            
            # Assert
            assert result is None
    
    @patch('services.user_service.logger')
    def test_authenticate_user_database_error(self, mock_logger, user_service, mock_db_session):
        """Test authentication with database error."""
        # Arrange
        mock_db_session.query.side_effect = Exception("Database connection failed")
        
        # Act & Assert
        with pytest.raises(AuthenticationError):
            user_service.authenticate_user('CRPF001234', 'password')
        
        mock_logger.error.assert_called_once()
```

### Frontend Testing

```typescript
// UserService.test.ts
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { rest } from 'msw';
import { setupServer } from 'msw/node';

import { UserService, useAuth } from '../UserService';
import { LoginCredentials } from '../types/auth.types';

// Mock server for API calls
const server = setupServer(
  rest.post('/api/auth/login', (req, res, ctx) => {
    const { username, password } = req.body as LoginCredentials;
    
    if (username === 'CRPF001234' && password === 'correct_password') {
      return res(
        ctx.json({
          success: true,
          data: {
            force_id: 'CRPF001234',
            name: 'John Doe',
            role: 'personnel',
            permissions: ['read']
          }
        })
      );
    }
    
    return res(
      ctx.status(401),
      ctx.json({
        success: false,
        message: 'Invalid credentials'
      })
    );
  })
);

beforeAll(() => server.listen());
afterEach(() => server.resetHandlers());
afterAll(() => server.close());

describe('UserService', () => {
  let userService: UserService;
  
  beforeEach(() => {
    userService = new UserService();
  });
  
  describe('login', () => {
    it('should login successfully with valid credentials', async () => {
      // Arrange
      const credentials: LoginCredentials = {
        forceId: 'CRPF001234',
        password: 'correct_password'
      };
      
      // Act
      const result = await userService.login(credentials);
      
      // Assert
      expect(result.success).toBe(true);
      expect(result.user).toBeDefined();
      expect(result.user?.force_id).toBe('CRPF001234');
    });
    
    it('should fail login with invalid credentials', async () => {
      // Arrange
      const credentials: LoginCredentials = {
        forceId: 'CRPF001234',
        password: 'wrong_password'
      };
      
      // Act
      const result = await userService.login(credentials);
      
      // Assert
      expect(result.success).toBe(false);
      expect(result.error).toBe('Invalid credentials');
    });
    
    it('should validate force ID format', async () => {
      // Arrange
      const credentials: LoginCredentials = {
        forceId: 'invalid_format',
        password: 'password123'
      };
      
      // Act
      const result = await userService.login(credentials);
      
      // Assert
      expect(result.success).toBe(false);
      expect(result.error).toBe('Invalid Force ID format');
    });
  });
});

describe('useAuth hook', () => {
  const TestComponent = () => {
    const { authStatus, login, isAuthenticated } = useAuth();
    
    return (
      <div>
        <div data-testid="auth-status">{authStatus}</div>
        <div data-testid="is-authenticated">{isAuthenticated.toString()}</div>
        <button
          onClick={() => login({
            forceId: 'CRPF001234',
            password: 'correct_password'
          })}
        >
          Login
        </button>
      </div>
    );
  };
  
  it('should handle login flow correctly', async () => {
    // Arrange
    render(<TestComponent />);
    
    // Initial state
    expect(screen.getByTestId('auth-status')).toHaveTextContent('loading');
    expect(screen.getByTestId('is-authenticated')).toHaveTextContent('false');
    
    // Act
    fireEvent.click(screen.getByText('Login'));
    
    // Assert
    await waitFor(() => {
      expect(screen.getByTestId('auth-status')).toHaveTextContent('authenticated');
      expect(screen.getByTestId('is-authenticated')).toHaveTextContent('true');
    });
  });
});
```

## 📊 Performance Standards

### Backend Performance

```python
# Use caching for expensive operations
from functools import lru_cache
import redis

redis_client = redis.Redis(host='localhost', port=6379, db=0)

@lru_cache(maxsize=128)
def get_questionnaire_config(questionnaire_id: int) -> Dict:
    """Get questionnaire configuration with caching."""
    # Expensive database operation
    return fetch_questionnaire_from_db(questionnaire_id)

def get_user_statistics(force_id: str) -> Dict:
    """Get user statistics with Redis caching."""
    cache_key = f"user_stats:{force_id}"
    
    # Try cache first
    cached_data = redis_client.get(cache_key)
    if cached_data:
        return json.loads(cached_data)
    
    # Calculate statistics
    stats = calculate_user_statistics(force_id)
    
    # Cache for 5 minutes
    redis_client.setex(cache_key, 300, json.dumps(stats))
    
    return stats

# Database query optimization
def get_survey_responses_paginated(
    force_id: str,
    page: int = 1,
    limit: int = 20
) -> Dict:
    """Get paginated survey responses with optimized query."""
    offset = (page - 1) * limit
    
    # Use efficient query with proper indexing
    query = """
        SELECT 
            sr.response_id,
            sr.submitted_at,
            sr.risk_level,
            q.title
        FROM survey_responses sr
        JOIN questionnaires q ON sr.questionnaire_id = q.id
        WHERE sr.force_id = %s
        ORDER BY sr.submitted_at DESC
        LIMIT %s OFFSET %s
    """
    
    responses = db_session.execute(query, (force_id, limit, offset)).fetchall()
    
    # Get total count for pagination
    count_query = "SELECT COUNT(*) FROM survey_responses WHERE force_id = %s"
    total = db_session.execute(count_query, (force_id,)).scalar()
    
    return {
        'responses': [dict(row) for row in responses],
        'pagination': {
            'page': page,
            'limit': limit,
            'total': total,
            'total_pages': math.ceil(total / limit)
        }
    }
```

### Frontend Performance

```typescript
// Use React.memo for expensive components
import React, { memo, useMemo, useCallback } from 'react';

interface ExpensiveComponentProps {
  data: LargeDataSet[];
  onItemClick: (id: string) => void;
}

const ExpensiveComponent = memo<ExpensiveComponentProps>(({ data, onItemClick }) => {
  // Memoize expensive calculations
  const processedData = useMemo(() => {
    return data.map(item => ({
      ...item,
      calculatedValue: expensiveCalculation(item)
    }));
  }, [data]);

  // Memoize callback functions
  const handleItemClick = useCallback((id: string) => {
    onItemClick(id);
  }, [onItemClick]);

  return (
    <div>
      {processedData.map(item => (
        <div key={item.id} onClick={() => handleItemClick(item.id)}>
          {item.calculatedValue}
        </div>
      ))}
    </div>
  );
});

// Code splitting for route components
import { lazy, Suspense } from 'react';

const AdminDashboard = lazy(() => import('./pages/AdminDashboard'));
const SurveyForm = lazy(() => import('./pages/SurveyForm'));

const App = () => {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Routes>
        <Route path="/admin" element={<AdminDashboard />} />
        <Route path="/survey" element={<SurveyForm />} />
      </Routes>
    </Suspense>
  );
};
```

## 🔍 Code Review Guidelines

### Review Checklist

**Security Review**:
- [ ] Input validation implemented
- [ ] SQL injection prevention
- [ ] XSS protection in place
- [ ] Authentication/authorization checks
- [ ] Sensitive data handling

**Code Quality**:
- [ ] Follows naming conventions
- [ ] Proper error handling
- [ ] Adequate test coverage
- [ ] Performance considerations
- [ ] Documentation completeness

**Functionality**:
- [ ] Meets requirements
- [ ] Edge cases handled
- [ ] Backward compatibility maintained
- [ ] No breaking changes

### Review Process

1. **Automated Checks**: All CI/CD checks must pass
2. **Self Review**: Author reviews their own code first
3. **Peer Review**: At least one team member review
4. **Security Review**: For security-sensitive changes
5. **Final Approval**: Lead developer or architect approval

---

These coding standards ensure consistency, maintainability, and security across the SATHI project. All contributors should follow these guidelines and participate in code reviews to maintain high code quality.