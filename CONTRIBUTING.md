# Contributing to SATHI

## 🎯 Overview

Welcome to the SATHI (System for Analyzing and Tracking Human Intelligence) development team! This guide will help you set up your development environment and understand our contribution workflow.

## 📋 Prerequisites

### Required Software

- **Python 3.8+** with pip
- **Node.js 16+** with npm
- **MySQL 8.0+**
- **Git**
- **VS Code** (recommended IDE)

### Hardware Requirements

- **Minimum RAM**: 8GB (16GB recommended)
- **Storage**: 10GB free space
- **Camera**: Optional (for emotion detection testing)
- **Network**: Stable internet connection

## 🚀 Development Environment Setup

### 1. Repository Setup

```bash
git clone https://github.com/JayeshCC/SATHI.git
cd SATHI
```

### 2. Backend Setup

```bash
cd backend

# Create virtual environment
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Setup environment variables
cp .env.example .env
# Edit .env with your database configuration
```

### 3. Database Setup

```sql
CREATE DATABASE crpf_mental_health;
CREATE USER 'crpf_user'@'localhost' IDENTIFIED BY 'secure_password';
GRANT ALL PRIVILEGES ON crpf_mental_health.* TO 'crpf_user'@'localhost';
FLUSH PRIVILEGES;
```

```bash
# Initialize database schema
python db/init_db.py

# Start backend server
python app.py
```

### 4. Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Start development server
npm start
```

### 5. Access the Application

- **Frontend**: http://localhost:3000
- **Backend API**: http://localhost:5000
- **Admin Login**: Force ID: 100000001, Password: admin123

## 🏗️ Codebase Architecture

### Backend Architecture (Flask)

```
backend/
├── api/                    # API modules
│   ├── auth/              # Authentication endpoints
│   ├── admin/             # Administrative functions
│   ├── survey/            # Survey management
│   ├── image/             # Image processing & ML
│   └── monitor/           # System monitoring
├── config/                # Configuration management
├── db/                    # Database schema and utilities
├── services/              # Business logic services
├── utils/                 # Utility functions
└── app.py                 # Main application entry point
```

### Frontend Architecture (React TypeScript)

```
frontend/src/
├── components/            # Reusable UI components
├── pages/                 # Application pages
│   ├── admin/            # Admin interface
│   └── soldier/          # Soldier interface
├── services/             # API service layer
├── context/              # React context providers
└── utils/                # Utility functions
```

## 🧪 Testing & Quality Assurance

### Backend Testing

#### Unit Tests

```bash
cd backend
python -m pytest tests/unit/ -v
```

#### Integration Tests

```bash
python -m pytest tests/integration/ -v
```

### Frontend Testing

#### Component Tests

```bash
cd frontend
npm test
```

#### E2E Tests

```bash
npm run test:e2e
```

## 📝 Best Practices & Standards

### Code Standards

#### Python (Backend)
- Follow PEP 8 style guide
- Use type hints for function parameters and return values
- Write comprehensive docstrings for functions and classes
- Maximum line length: 100 characters

```python
def calculate_depression_score(nlp_score: float, emotion_score: float) -> float:
    """
    Calculate combined depression score from NLP and emotion analysis.
    
    Args:
        nlp_score: Sentiment analysis score (0.0-1.0)
        emotion_score: Emotion detection score (0.0-1.0)
    
    Returns:
        Combined depression score (0.0-1.0)
    """
    pass
```

#### TypeScript (Frontend)
- Use strict TypeScript configuration
- Define interfaces for all data structures
- Use functional components with hooks
- Follow ESLint configuration

```typescript
interface SoldierData {
  forceId: string;
  combinedScore: number;
  riskLevel: 'LOW' | 'MEDIUM' | 'HIGH' | 'CRITICAL';
  lastSurveyDate: string;
}

const SoldierCard: React.FC<{ soldier: SoldierData }> = ({ soldier }) => {
  // Component implementation
};
```

### Security Best Practices

1. **Input Validation**: Validate all user inputs on both client and server
2. **SQL Injection Prevention**: Use parameterized queries
3. **XSS Protection**: Sanitize all user-generated content
4. **Authentication**: Implement proper session management
5. **Data Encryption**: Encrypt sensitive data at rest and in transit

### Performance Optimization

1. **Database**: Use proper indexing and query optimization
2. **API**: Implement pagination for large datasets
3. **Frontend**: Use lazy loading and code splitting
4. **Caching**: Implement caching strategies for frequently accessed data

## 🔧 Development Tools

### Recommended IDE Setup (VS Code)

Install these extensions:
- Python
- TypeScript
- ESLint
- Prettier
- GitLens
- MySQL Workbench

### Debugging

#### Backend Debugging
```python
# Use Python debugger
import pdb; pdb.set_trace()
```

#### Frontend Debugging
- Use browser developer tools
- React Developer Tools extension
- Chrome DevTools for performance profiling

## 📊 Development Workflow

### 1. Feature Development

1. Create a feature branch: `git checkout -b feature/feature-name`
2. Implement the feature with tests
3. Run all tests to ensure nothing breaks
4. Update documentation if needed
5. Submit a pull request

### 2. Code Review Process

1. All code must be reviewed by at least one team member
2. Ensure all tests pass
3. Verify documentation is updated
4. Check for security vulnerabilities
5. Approve and merge

### 3. Testing Strategy

- **Unit Tests**: Test individual functions/components
- **Integration Tests**: Test API endpoints and database interactions
- **E2E Tests**: Test complete user workflows
- **Performance Tests**: Test system under load

## 🚨 Troubleshooting Development Issues

### Common Issues

#### Database Connection Issues
```bash
# Check MySQL service status
sudo systemctl status mysql

# Reset database permissions
mysql -u root -p
GRANT ALL PRIVILEGES ON crpf_mental_health.* TO 'crpf_user'@'localhost';
FLUSH PRIVILEGES;
```

#### Model Loading Errors
```python
# Verify model files exist
import os
model_files = [
    'model/emotion_model.json',
    'model/emotion_model.h5'
]
for file in model_files:
    if not os.path.exists(file):
        print(f"Missing: {file}")
```

#### Camera Access Issues
```python
# Test camera availability
import cv2
cap = cv2.VideoCapture(0)
if cap.isOpened():
    print("Camera working")
cap.release()
```

## 📚 Additional Resources

- [API Documentation](API_DOCUMENTATION.md)
- [User Guide](USER_GUIDE.md)
- [Deployment Guide](DEPLOYMENT_GUIDE.md)
- [Troubleshooting Guide](TROUBLESHOOTING.md)

## 🤝 Getting Help

- **Technical Issues**: Create an issue in GitHub
- **Questions**: Use GitHub Discussions
- **Security Issues**: Contact maintainers directly

## 📄 License

This project is proprietary software developed for the Central Reserve Police Force (CRPF). All rights reserved.

---

**Note**: This system handles sensitive mental health data. Please ensure all development and testing complies with applicable privacy and security regulations.