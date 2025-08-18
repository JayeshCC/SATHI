# Contributing to SATHI
*Part of SATHI Documentation Suite*

**📚 Quick Navigation:**
[🏠 README](README.md) | [🤝 Contributing](CONTRIBUTING.md) | [🔌 API](API_DOCUMENTATION.md) | [👥 Users](USER_GUIDE.md) | [🚀 Deploy](DEPLOYMENT_GUIDE.md) | [🔧 Troubleshoot](TROUBLESHOOTING.md)

---

## Table of Contents

1. [Welcome to SATHI Development](#welcome-to-sathi-development)
2. [Development Environment Setup](#development-environment-setup)
3. [Code Standards and Conventions](#code-standards-and-conventions)
4. [Git Workflow and Branching Strategy](#git-workflow-and-branching-strategy)
5. [Pull Request Process](#pull-request-process)
6. [Issue Reporting Guidelines](#issue-reporting-guidelines)
7. [Code Review Checklist](#code-review-checklist)
8. [Testing Requirements](#testing-requirements)
9. [Documentation Contribution Guidelines](#documentation-contribution-guidelines)
10. [Security Guidelines](#security-guidelines)

## Welcome to SATHI Development

Thank you for your interest in contributing to **SATHI** (System for Analyzing and Tracking Human Intelligence)! This guide will help you get started with contributing to our mental health monitoring system for CRPF personnel.

### 🎯 What We're Building

SATHI is a critical healthcare platform that requires:
- **Medical-grade reliability** and accuracy
- **Strict security standards** for sensitive health data
- **High-performance AI/ML** capabilities
- **User-friendly interfaces** for healthcare professionals
- **Comprehensive documentation** for maintainability

### 👥 Types of Contributors

- **🏥 Healthcare Professionals**: Providing domain expertise and user requirements
- **💻 Software Developers**: Backend, frontend, and AI/ML development
- **🎨 UI/UX Designers**: Creating intuitive interfaces for medical professionals
- **📚 Technical Writers**: Improving documentation and user guides
- **🔧 DevOps Engineers**: Infrastructure, deployment, and monitoring
- **🧪 QA Engineers**: Testing, validation, and quality assurance

## Development Environment Setup

### Prerequisites

**Required Software:**
- **Python 3.8+** with pip package manager
- **Node.js 16+** with npm package manager  
- **MySQL 8.0+** database server
- **Git** for version control
- **VS Code** (recommended IDE with extensions)

**Optional but Recommended:**
- **Docker** for containerized development
- **Postman** for API testing
- **MySQL Workbench** for database management

### 🚀 Quick Setup (5-minute start)

```bash
# 1. Clone and navigate to repository
git clone https://github.com/JayeshCC/SATHI.git
cd SATHI

# 2. Backend setup
cd backend
python -m venv venv

# Activate virtual environment
# Windows:
venv\Scripts\activate
# macOS/Linux:
source venv/bin/activate

# Install Python dependencies
pip install -r requirements.txt

# 3. Frontend setup (new terminal)
cd frontend
npm install

# 4. Start development servers
# Backend (port 5000)
python app.py

# Frontend (port 3000) - new terminal
npm start
```

### 📋 Detailed Environment Setup

#### Backend Development Environment

```bash
# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # Linux/Mac
# OR
venv\Scripts\activate     # Windows

# Install dependencies
pip install -r requirements.txt

# Install development tools
pip install black flake8 isort pytest pytest-cov

# Set up environment variables
cp .env.example .env
# Edit .env with your database credentials
```

#### Frontend Development Environment

```bash
cd frontend

# Install dependencies
npm install

# Install development tools
npm install -D eslint prettier @typescript-eslint/parser

# Set up environment variables
cp .env.example .env
# Edit .env with your API endpoints
```

#### Database Setup

```sql
-- Create database
CREATE DATABASE crpf_mental_health;

-- Import schema
mysql -u root -p crpf_mental_health < backend/db/schema.sql

-- Import initial data
mysql -u root -p crpf_mental_health < backend/db/initial_data.sql
```

### 🛠️ Development Tools Configuration

#### VS Code Extensions (recommended)

```json
{
  "recommendations": [
    "ms-python.python",
    "ms-python.flake8", 
    "bradlc.vscode-tailwindcss",
    "esbenp.prettier-vscode",
    "ms-vscode.vscode-typescript-next",
    "ms-toolsai.jupyter",
    "ms-vscode.vscode-json"
  ]
}
```

#### Git Hooks Setup

```bash
# Install pre-commit hooks
pip install pre-commit
pre-commit install

# This will run linting and formatting on each commit
```

## Code Standards and Conventions

### 🐍 Python (Backend) Standards

**Code Style:**
- Follow **PEP 8** style guide strictly
- Use **type hints** for all function parameters and return values
- Maximum line length: **88 characters** (Black formatter default)
- Use **docstrings** for all functions, classes, and modules

**Example:**
```python
from typing import Dict, List, Optional, Tuple
import logging

logger = logging.getLogger(__name__)

def calculate_depression_score(
    text: str, 
    emotion_data: Dict[str, float]
) -> Tuple[float, str]:
    """
    Calculate depression score from text and emotion analysis.
    
    Args:
        text: Survey response text to analyze
        emotion_data: Dictionary containing emotion detection results
        
    Returns:
        Tuple of (depression_score, risk_level)
        
    Raises:
        ValueError: If text is empty or emotion_data is invalid
        
    Example:
        >>> score, level = calculate_depression_score(
        ...     "I feel very sad today", 
        ...     {"sad": 0.8, "happy": 0.1}
        ... )
        >>> print(f"Score: {score}, Level: {level}")
        Score: 0.75, Level: HIGH
    """
    if not text or not text.strip():
        raise ValueError("Text cannot be empty")
        
    if not emotion_data:
        raise ValueError("Emotion data cannot be empty")
        
    try:
        # Your implementation here
        score = process_analysis(text, emotion_data)
        risk_level = determine_risk_level(score)
        
        logger.info(f"Calculated depression score: {score}")
        return score, risk_level
        
    except Exception as e:
        logger.error(f"Error calculating depression score: {e}")
        raise
```

**Security Requirements:**
```python
# Always validate input
def validate_force_id(force_id: str) -> bool:
    """Validate CRPF force ID format."""
    if not force_id or len(force_id) != 9:
        return False
    return force_id.isdigit()

# Use parameterized queries
def get_soldier_data(force_id: str) -> Optional[Dict]:
    """Safely retrieve soldier data."""
    query = "SELECT * FROM soldiers WHERE force_id = %s"
    return db.execute(query, (force_id,))

# Log security events
def log_security_event(event_type: str, user_id: str, details: str):
    """Log security-related events."""
    security_logger.warning(f"{event_type}: User {user_id} - {details}")
```

### ⚛️ TypeScript (Frontend) Standards

**Code Style:**
- Use **strict TypeScript** configuration
- Follow **React functional components** with hooks
- Use **Tailwind CSS** for styling
- Maximum line length: **100 characters**

**Example:**
```typescript
import React, { useState, useEffect, useCallback } from 'react';
import { SoldierData, RiskLevel, ApiResponse } from '../types/api';
import { useAuth } from '../hooks/useAuth';
import { soldierService } from '../services/soldier.service';

interface SoldierDashboardProps {
  onRiskLevelChange?: (level: RiskLevel) => void;
  refreshInterval?: number;
}

/**
 * Soldier Dashboard Component
 * 
 * Displays real-time mental health metrics and risk assessments
 * for CRPF personnel with automatic refresh capabilities.
 */
export const SoldierDashboard: React.FC<SoldierDashboardProps> = ({
  onRiskLevelChange,
  refreshInterval = 30000
}) => {
  const { user, isAuthenticated } = useAuth();
  const [soldiers, setSoldiers] = useState<SoldierData[]>([]);
  const [loading, setLoading] = useState<boolean>(true);
  const [error, setError] = useState<string | null>(null);

  const fetchSoldiers = useCallback(async (): Promise<void> => {
    try {
      setLoading(true);
      const response: ApiResponse<SoldierData[]> = await soldierService.getAll();
      
      if (response.success) {
        setSoldiers(response.data);
        setError(null);
      } else {
        setError(response.message || 'Failed to fetch soldier data');
      }
    } catch (err) {
      console.error('Error fetching soldiers:', err);
      setError('Network error occurred');
    } finally {
      setLoading(false);
    }
  }, []);

  useEffect(() => {
    if (isAuthenticated) {
      fetchSoldiers();
      
      const interval = setInterval(fetchSoldiers, refreshInterval);
      return () => clearInterval(interval);
    }
  }, [isAuthenticated, fetchSoldiers, refreshInterval]);

  if (!isAuthenticated) {
    return <div className="text-center py-8">Please log in to view dashboard</div>;
  }

  return (
    <div className="min-h-screen bg-gray-50 p-6">
      <div className="max-w-7xl mx-auto">
        <h1 className="text-3xl font-bold text-gray-900 mb-8">
          CRPF Mental Health Dashboard
        </h1>
        
        {error && (
          <div className="bg-red-50 border border-red-200 rounded-md p-4 mb-6">
            <p className="text-red-800">{error}</p>
          </div>
        )}
        
        {loading ? (
          <div className="flex justify-center items-center h-64">
            <div className="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-600" />
          </div>
        ) : (
          <SoldierGrid soldiers={soldiers} onRiskLevelChange={onRiskLevelChange} />
        )}
      </div>
    </div>
  );
};
```

### 🏗️ Architecture Patterns

**Backend Architecture:**
- **Blueprint-based modules** for API organization
- **Service layer** for business logic separation
- **Repository pattern** for data access
- **Dependency injection** for testability

**Frontend Architecture:**
- **Component-based architecture** with React
- **Custom hooks** for reusable logic
- **Context API** for global state management
- **Service layer** for API communication

## Git Workflow and Branching Strategy

### 🌳 Branch Naming Convention

```
main                    # Production-ready code
develop                 # Integration branch for features
feature/SATHI-123-auth  # New features
bugfix/SATHI-456-login  # Bug fixes
hotfix/SATHI-789-crit   # Critical production fixes
release/v1.2.0          # Release preparation
```

### 📋 Commit Message Format

```
type(scope): brief description

Detailed explanation of what was changed and why.

Fixes: #123
Related: #456
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples:**
```bash
feat(auth): implement multi-factor authentication for admin users

- Add SMS and email verification options
- Integrate with CRPF authentication system
- Update security documentation

Fixes: #234
Related: #235, #236

fix(api): resolve depression score calculation edge case

- Handle empty emotion data gracefully
- Add proper error logging
- Update unit tests for edge cases

Fixes: #567
```

### 🔄 Development Workflow

#### 1. Starting New Work

```bash
# Update your local repository
git checkout main
git pull origin main

# Create feature branch
git checkout -b feature/SATHI-123-mental-health-dashboard

# Make your changes...
git add .
git commit -m "feat(dashboard): add mental health risk visualization"

# Push to remote
git push origin feature/SATHI-123-mental-health-dashboard
```

#### 2. Keeping Branch Updated

```bash
# Regularly sync with main branch
git checkout main
git pull origin main
git checkout feature/SATHI-123-mental-health-dashboard
git merge main

# Resolve any conflicts and test thoroughly
```

#### 3. Ready for Review

```bash
# Final checks before PR
npm run test          # Run all tests
npm run lint          # Check code style
npm run build         # Verify build works

# Push final changes
git push origin feature/SATHI-123-mental-health-dashboard
```

## Pull Request Process

### 📝 PR Template

When creating a pull request, use this template:

```markdown
## Description
Brief description of changes and motivation.

## Type of Change
- [ ] 🐛 Bug fix (non-breaking change which fixes an issue)
- [ ] ✨ New feature (non-breaking change which adds functionality)
- [ ] 💥 Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] 📚 Documentation update

## Testing
- [ ] Tests pass locally
- [ ] New tests added for new functionality
- [ ] Manual testing completed

## Screenshots (if applicable)
Include before/after screenshots for UI changes.

## Checklist
- [ ] My code follows the style guidelines of this project
- [ ] I have performed a self-review of my own code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] New and existing unit tests pass locally with my changes

## Related Issues
Closes #123
Related to #456
```

### 🔍 PR Review Process

**Before Requesting Review:**
1. ✅ **Self-review**: Review your own changes thoroughly
2. ✅ **Testing**: All tests pass locally
3. ✅ **Documentation**: Update relevant documentation
4. ✅ **Security**: No sensitive data exposed
5. ✅ **Performance**: No significant performance regressions

**Review Criteria:**
- **Code Quality**: Follows style guidelines and best practices
- **Security**: No security vulnerabilities introduced
- **Testing**: Adequate test coverage for new functionality
- **Documentation**: Clear comments and updated docs
- **Healthcare Compliance**: Meets medical software standards

**Approval Requirements:**
- **2 approvals** for main branch merges
- **1 approval** for develop branch merges
- **Code owner approval** for security-sensitive changes
- **Healthcare domain expert approval** for medical logic changes

## Issue Reporting Guidelines

### 🐛 Bug Reports

Use this template for bug reports:

```markdown
## Bug Description
A clear and concise description of what the bug is.

## Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. Scroll down to '...'
4. See error

## Expected Behavior
A clear description of what you expected to happen.

## Actual Behavior
A clear description of what actually happened.

## Screenshots
If applicable, add screenshots to help explain your problem.

## Environment
- OS: [e.g. Windows 10, Ubuntu 20.04]
- Browser: [e.g. Chrome 96, Firefox 94]
- SATHI Version: [e.g. v1.2.3]
- Database: [e.g. MySQL 8.0.27]

## Additional Context
Add any other context about the problem here.

## Severity
- [ ] 🔴 Critical (system down, data loss)
- [ ] 🟠 High (major feature broken)
- [ ] 🟡 Medium (minor feature issue)
- [ ] 🟢 Low (cosmetic issue)
```

### ✨ Feature Requests

```markdown
## Feature Description
A clear and concise description of what you want to happen.

## Use Case
Describe the healthcare scenario where this feature would be valuable.

## Proposed Solution
A clear description of what you want to happen.

## Alternative Solutions
A clear description of any alternative solutions you've considered.

## Healthcare Impact
How will this feature improve mental health monitoring for CRPF personnel?

## Priority
- [ ] 🔴 Critical (urgent healthcare need)
- [ ] 🟠 High (important for user experience)
- [ ] 🟡 Medium (nice to have improvement)
- [ ] 🟢 Low (future enhancement)
```

## Code Review Checklist

### 👀 Reviewer Checklist

**Security Review:**
- [ ] No hardcoded secrets or credentials
- [ ] Input validation implemented
- [ ] SQL injection prevention (parameterized queries)
- [ ] XSS prevention (proper output encoding)
- [ ] Authentication and authorization checks
- [ ] Sensitive data handling follows HIPAA guidelines

**Code Quality Review:**
- [ ] Code follows established style guidelines
- [ ] Functions are single-purpose and well-named
- [ ] Complex logic is commented and documented
- [ ] Error handling is comprehensive
- [ ] Performance considerations addressed
- [ ] No code duplication without justification

**Healthcare Compliance Review:**
- [ ] Medical data handling follows privacy standards
- [ ] Audit logging implemented for sensitive operations
- [ ] Data validation meets healthcare requirements
- [ ] User consent and privacy considerations addressed

**Testing Review:**
- [ ] Unit tests cover new functionality
- [ ] Integration tests verify end-to-end workflows
- [ ] Edge cases and error conditions tested
- [ ] Performance tests for critical paths
- [ ] Manual testing results documented

## Testing Requirements

### 🧪 Test Categories

**Unit Tests (Required):**
```python
# Example: Backend unit test
import pytest
from services.depression_analyzer import DepressionAnalyzer

def test_depression_score_calculation():
    """Test depression score calculation with various inputs."""
    analyzer = DepressionAnalyzer()
    
    # Test normal case
    result = analyzer.calculate_score("I feel sad today", {"sad": 0.8})
    assert 0.0 <= result <= 1.0
    assert result > 0.5  # Sad text should score high
    
    # Test edge case
    with pytest.raises(ValueError):
        analyzer.calculate_score("", {})
```

```typescript
// Example: Frontend unit test
import { render, screen, fireEvent } from '@testing-library/react';
import { SoldierCard } from '../components/SoldierCard';

describe('SoldierCard', () => {
  it('displays soldier information correctly', () => {
    const mockSoldier = {
      forceId: '123456789',
      name: 'Test Soldier',
      riskLevel: 'HIGH'
    };
    
    render(<SoldierCard soldier={mockSoldier} />);
    
    expect(screen.getByText('Test Soldier')).toBeInTheDocument();
    expect(screen.getByText('HIGH')).toBeInTheDocument();
    expect(screen.getByText('123456789')).toBeInTheDocument();
  });
});
```

**Integration Tests:**
```python
def test_complete_survey_workflow():
    """Test complete survey submission and analysis workflow."""
    # Submit survey response
    response = client.post('/api/survey/submit', json={
        'force_id': '123456789',
        'responses': ['I feel great today'],
        'image_data': 'base64_image_data'
    })
    
    # Verify response stored
    assert response.status_code == 200
    
    # Verify analysis triggered
    analysis = get_latest_analysis('123456789')
    assert analysis is not None
    assert 'depression_score' in analysis
```

**Performance Tests:**
```python
def test_analysis_performance():
    """Ensure analysis completes within acceptable time limits."""
    import time
    
    start_time = time.time()
    result = analyzer.analyze_large_dataset(sample_data)
    duration = time.time() - start_time
    
    assert duration < 5.0  # Must complete within 5 seconds
    assert result['accuracy'] > 0.95  # Must maintain high accuracy
```

### 🎯 Coverage Requirements

- **Minimum coverage**: 80% for new code
- **Critical paths**: 95% coverage required
- **Healthcare algorithms**: 100% coverage required

```bash
# Run coverage analysis
pytest --cov=backend --cov-report=html
npm run test -- --coverage
```

## Documentation Contribution Guidelines

### 📚 Documentation Standards

**Markdown Formatting:**
- Use **consistent heading hierarchy** (H1 → H2 → H3)
- Include **table of contents** for documents >500 words
- Use **code syntax highlighting** with language specification
- Include **cross-references** to related documents

**Content Requirements:**
- **Clear, actionable instructions** for all procedures
- **Code examples** for all technical concepts
- **Screenshots** for UI-related documentation
- **Healthcare context** for medical features

**Example Documentation:**
```markdown
### Depression Score Calculation

The depression score algorithm combines multiple data sources to provide 
accurate mental health assessment for CRPF personnel.

#### Algorithm Overview

```python
def calculate_depression_score(text_analysis, emotion_data, historical_data):
    """
    Calculate comprehensive depression score.
    
    The algorithm weighs three key factors:
    1. Text sentiment analysis (40% weight)
    2. Facial emotion detection (35% weight) 
    3. Historical trend analysis (25% weight)
    """
    pass
```

#### Healthcare Rationale

This multi-modal approach aligns with clinical best practices for mental 
health assessment, providing healthcare professionals with reliable data 
for early intervention decisions.

💡 **Related**: See [API Documentation - Depression Analysis](API_DOCUMENTATION.md#depression-analysis) 
for implementation details.
```

### 🔗 Cross-Linking Guidelines

Always include contextual links:
```markdown
For API authentication details, see [API Documentation - Authentication](API_DOCUMENTATION.md#authentication)

💡 **Related**: See [User Guide - Patient Portal](USER_GUIDE.md#patient-portal) for end-user perspective

🔧 **Troubleshooting**: If you encounter issues, check [Troubleshooting - Authentication Errors](TROUBLESHOOTING.md#authentication-errors)
```

## Security Guidelines

### 🔒 Healthcare Data Security

**Data Handling Requirements:**
- **Encryption at rest**: All mental health data encrypted
- **Encryption in transit**: HTTPS/TLS for all communications
- **Access logging**: All data access logged and auditable
- **Data minimization**: Collect only necessary information
- **Consent management**: Explicit consent for data collection

**Code Security Practices:**
```python
# ✅ Good: Secure password handling
from werkzeug.security import generate_password_hash, check_password_hash

def create_user(username: str, password: str) -> bool:
    """Create user with secure password hashing."""
    password_hash = generate_password_hash(password)
    # Store password_hash, never store plain password
    
# ✅ Good: Input validation
def validate_survey_input(responses: List[str]) -> bool:
    """Validate survey responses for security."""
    for response in responses:
        if len(response) > 1000:  # Prevent large inputs
            return False
        if contains_malicious_content(response):
            return False
    return True

# ❌ Bad: SQL injection vulnerability
def get_soldier_bad(force_id: str):
    query = f"SELECT * FROM soldiers WHERE force_id = '{force_id}'"
    return db.execute(query)  # Never do this!

# ✅ Good: Parameterized query
def get_soldier_good(force_id: str):
    query = "SELECT * FROM soldiers WHERE force_id = %s"
    return db.execute(query, (force_id,))
```

### 🛡️ Authentication & Authorization

```python
# Role-based access control
@require_role(['admin', 'healthcare_provider'])
def view_sensitive_data():
    """Only authorized roles can access sensitive mental health data."""
    pass

# Session security
@login_required
def protected_endpoint():
    """Ensure user is authenticated before accessing protected resources."""
    pass
```

---

## 💡 Quick Tips for New Contributors

1. **🏥 Healthcare First**: Always consider the medical impact of your changes
2. **🔒 Security Always**: Mental health data requires highest security standards  
3. **📖 Document Everything**: Healthcare software needs comprehensive documentation
4. **🧪 Test Thoroughly**: Lives depend on the reliability of our code
5. **💬 Ask Questions**: When in doubt, ask the healthcare domain experts

## 🆘 Getting Help

- **💬 Technical Questions**: Create GitHub issues with `question` label
- **🏥 Healthcare Domain**: Contact healthcare team leads
- **🔒 Security Concerns**: Email security team directly (private channel)
- **📚 Documentation**: Check existing docs first, then create improvement issues

💡 **Related Documentation:**
- For API integration, see [API Documentation](API_DOCUMENTATION.md)
- For deployment setup, see [Deployment Guide](DEPLOYMENT_GUIDE.md)  
- For user-facing features, see [User Guide](USER_GUIDE.md)
- For troubleshooting, see [Troubleshooting Guide](TROUBLESHOOTING.md)

---

**Thank you for contributing to SATHI!** Your work helps improve mental health support for CRPF personnel who serve our nation. Every contribution, no matter how small, makes a meaningful difference in the lives of our brave service members.