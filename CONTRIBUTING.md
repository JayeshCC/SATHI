# Contributing to SATHI

Welcome to the SATHI (CRPF Mental Health Monitoring System) project! We appreciate your interest in contributing to this important healthcare technology initiative.

## 📋 Table of Contents

1. [Code of Conduct](#code-of-conduct)
2. [Getting Started](#getting-started)
3. [Development Workflow](#development-workflow)
4. [Coding Standards](#coding-standards)
5. [Testing Requirements](#testing-requirements)
6. [Documentation Guidelines](#documentation-guidelines)
7. [Pull Request Process](#pull-request-process)
8. [Issue Reporting](#issue-reporting)
9. [Security Considerations](#security-considerations)
10. [Community Guidelines](#community-guidelines)

## 🤝 Code of Conduct

This project adheres to professional standards appropriate for a healthcare technology system:

- **Respectful Communication**: Maintain professional, respectful communication in all interactions
- **Privacy First**: Prioritize patient privacy and data security in all contributions
- **Quality Focus**: Maintain high code quality standards for this critical healthcare system
- **Collaborative Spirit**: Work together to improve mental health care for CRPF personnel
- **Compliance Awareness**: Ensure all contributions comply with healthcare data regulations

## 🚀 Getting Started

### Prerequisites

Before contributing, ensure you have:

- **Python 3.8+** for backend development
- **Node.js 16+** and **npm** for frontend development
- **MySQL 8.0+** for database
- **Git** for version control
- Understanding of healthcare data sensitivity

### Environment Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/JayeshCC/SATHI.git
   cd SATHI
   ```

2. **Backend Setup**:
   ```bash
   cd backend
   python -m venv venv
   source venv/bin/activate  # Linux/Mac
   # or
   venv\Scripts\activate     # Windows
   pip install -r requirements.txt
   ```

3. **Frontend Setup**:
   ```bash
   cd frontend
   npm install
   ```

4. **Database Setup**:
   ```bash
   # Follow instructions in backend/db/README.md
   ```

## 🔄 Development Workflow

### Branch Strategy

We use a feature branch workflow:

```
main
├── feature/user-authentication-enhancement
├── feature/survey-analytics-dashboard
├── bugfix/emotion-detection-accuracy
└── hotfix/security-patch-2024
```

### Workflow Steps

1. **Create Feature Branch**:
   ```bash
   git checkout main
   git pull origin main
   git checkout -b feature/your-feature-name
   ```

2. **Develop & Test**:
   - Write code following our [coding standards](#coding-standards)
   - Add comprehensive tests
   - Test locally with sample data (never use real patient data)

3. **Commit Changes**:
   ```bash
   git add .
   git commit -m "feat: add user authentication enhancement
   
   - Implement JWT token management
   - Add password complexity validation
   - Include audit logging for security events
   
   Fixes #123"
   ```

4. **Push and Create PR**:
   ```bash
   git push origin feature/your-feature-name
   ```

### Commit Message Format

Use conventional commit format:

```
type(scope): short description

Detailed description of changes made.
Include the impact and reasoning behind changes.

- Bullet point for specific change
- Another bullet point for another change

Fixes #issue-number
```

**Types**: feat, fix, docs, style, refactor, test, chore

## 📏 Coding Standards

### Python (Backend)

Follow PEP 8 with these specific guidelines:

```python
# Good: Clear function documentation
def analyze_mental_health_score(survey_data: dict, user_id: str) -> dict:
    """
    Analyze mental health score based on survey responses.
    
    Args:
        survey_data: Dictionary containing survey responses
        user_id: Unique identifier for the user
        
    Returns:
        dict: Analysis results with risk assessment
        
    Raises:
        ValidationError: If survey data is invalid
    """
    # Implementation with proper error handling
    if not survey_data:
        raise ValidationError("Survey data cannot be empty")
    
    # Process with security considerations
    sanitized_data = sanitize_survey_input(survey_data)
    return process_analysis(sanitized_data)
```

### TypeScript (Frontend)

Follow strict TypeScript configuration:

```typescript
// Good: Well-typed component with proper documentation
interface SurveyFormProps {
  userId: string;
  onSubmit: (data: SurveyResponse) => Promise<void>;
  initialData?: Partial<SurveyResponse>;
}

/**
 * Survey form component for mental health assessment
 * Handles secure data collection and validation
 */
const SurveyForm: React.FC<SurveyFormProps> = ({ 
  userId, 
  onSubmit, 
  initialData 
}) => {
  // Implementation with proper error boundaries
  const [formData, setFormData] = useState<SurveyResponse>({
    ...defaultSurveyData,
    ...initialData
  });

  const handleSubmit = useCallback(async (data: SurveyResponse) => {
    try {
      await onSubmit(data);
    } catch (error) {
      logger.error('Survey submission failed', { userId, error });
      // Handle error appropriately
    }
  }, [onSubmit, userId]);

  return (
    <form onSubmit={handleSubmit}>
      {/* Form implementation */}
    </form>
  );
};
```

### Key Standards

- **Security First**: All code must consider healthcare data security
- **Error Handling**: Comprehensive error handling with proper logging
- **Type Safety**: Use TypeScript strictly, avoid `any` types
- **Documentation**: Comment complex logic, especially ML algorithms
- **Testing**: Maintain >80% code coverage for new code

## 🧪 Testing Requirements

### Backend Testing

```python
# Unit test example
def test_mental_health_analysis_valid_data():
    """Test mental health analysis with valid survey data."""
    # Arrange
    survey_data = {
        "stress_level": 3,
        "sleep_quality": 2,
        "social_support": 4
    }
    user_id = "test_user_123"
    
    # Act
    result = analyze_mental_health_score(survey_data, user_id)
    
    # Assert
    assert result["risk_level"] in ["LOW", "MEDIUM", "HIGH", "CRITICAL"]
    assert "confidence_score" in result
    assert result["confidence_score"] >= 0 and result["confidence_score"] <= 1
```

### Frontend Testing

```typescript
// Component test example
describe('SurveyForm', () => {
  it('should validate required fields before submission', async () => {
    const mockOnSubmit = jest.fn();
    render(<SurveyForm userId="test123" onSubmit={mockOnSubmit} />);
    
    const submitButton = screen.getByRole('button', { name: /submit/i });
    fireEvent.click(submitButton);
    
    expect(mockOnSubmit).not.toHaveBeenCalled();
    expect(screen.getByText(/required field/i)).toBeInTheDocument();
  });
});
```

### Test Coverage Requirements

- **Unit Tests**: >80% coverage for new code
- **Integration Tests**: All API endpoints
- **E2E Tests**: Critical user workflows
- **Security Tests**: Authentication and authorization flows

## 📚 Documentation Guidelines

### Code Documentation

- **Functions**: Document purpose, parameters, return values, and exceptions
- **Classes**: Document purpose and key methods
- **Complex Logic**: Explain algorithms, especially ML models
- **API Endpoints**: Document request/response formats

### User Documentation

- **Screenshots**: Include for UI changes
- **Step-by-Step**: Provide clear instructions
- **Examples**: Show real usage examples (with anonymized data)
- **Troubleshooting**: Include common issues and solutions

## 🔄 Pull Request Process

### PR Requirements

1. **Description**: Clear description of changes and impact
2. **Testing**: Evidence of thorough testing
3. **Documentation**: Updated documentation for changes
4. **Security Review**: Security implications considered
5. **Performance**: Performance impact assessed

### PR Template

```markdown
## Description
Brief description of changes and their purpose.

## Type of Change
- [ ] Bug fix (non-breaking change that fixes an issue)
- [ ] New feature (non-breaking change that adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing completed
- [ ] Security testing completed

## Security Considerations
Describe any security implications and mitigations.

## Performance Impact
Describe any performance implications.

## Documentation
- [ ] Code comments updated
- [ ] User documentation updated
- [ ] API documentation updated

## Screenshots (if applicable)
Include screenshots for UI changes.

## Checklist
- [ ] My code follows the project's coding standards
- [ ] I have performed a self-review of my code
- [ ] I have commented my code, particularly in hard-to-understand areas
- [ ] I have made corresponding changes to the documentation
- [ ] My changes generate no new warnings
- [ ] I have added tests that prove my fix is effective or that my feature works
- [ ] New and existing unit tests pass locally with my changes
```

### Review Process

1. **Automated Checks**: All CI/CD checks must pass
2. **Code Review**: At least one approving review required
3. **Security Review**: Security-sensitive changes require security team review
4. **Documentation Review**: Documentation changes reviewed for clarity
5. **Testing Verification**: Test coverage and quality verified

## 🐛 Issue Reporting

### Bug Reports

Use the bug report template:

```markdown
## Bug Description
A clear and concise description of the bug.

## Steps to Reproduce
1. Go to '...'
2. Click on '....'
3. Scroll down to '....'
4. See error

## Expected Behavior
What you expected to happen.

## Actual Behavior
What actually happened.

## Screenshots
If applicable, add screenshots to help explain your problem.

## Environment
- OS: [e.g., Windows 10, Ubuntu 20.04]
- Browser: [e.g., Chrome 96, Firefox 95]
- Version: [e.g., v2.1.0]

## Additional Context
Any other context about the problem.

## Security Impact
If this is a security issue, describe the potential impact.
```

### Feature Requests

Use the feature request template:

```markdown
## Feature Description
A clear and concise description of the feature you'd like to see.

## Problem Statement
What problem does this solve for users?

## Proposed Solution
Describe your proposed solution.

## Alternative Solutions
Describe any alternative solutions you've considered.

## User Stories
- As a [user type], I want [goal] so that [reason]

## Acceptance Criteria
- [ ] Criteria 1
- [ ] Criteria 2

## Additional Context
Any other context, mockups, or examples.
```

## 🔒 Security Considerations

### Data Protection

- **No Real Data**: Never use real patient data in development/testing
- **Encryption**: Ensure sensitive data is encrypted at rest and in transit
- **Access Control**: Implement proper authentication and authorization
- **Audit Logging**: Log all access to sensitive operations

### Vulnerability Reporting

For security vulnerabilities:

1. **DO NOT** create a public issue
2. Email security concerns to: [security contact]
3. Include detailed description and reproduction steps
4. Wait for response before public disclosure

### Security Testing

- Run security scans on all code changes
- Test authentication and authorization flows
- Validate input sanitization
- Check for SQL injection vulnerabilities

## 🏛️ Community Guidelines

### Communication Channels

- **GitHub Issues**: Bug reports and feature requests
- **Pull Requests**: Code review and discussion
- **Documentation**: Questions about implementation

### Contribution Recognition

Contributors will be recognized in:
- Project README
- Release notes for significant contributions
- Annual contributor acknowledgments

### Support

For questions about contributing:
- Check existing documentation first
- Search closed issues for similar questions
- Create a new issue with the "question" label

## 📞 Getting Help

If you need assistance:

1. **Documentation**: Check this contributing guide and project documentation
2. **Search Issues**: Look for similar questions or problems
3. **Create Issue**: Create a new issue with detailed information
4. **Contact Maintainers**: For complex questions or security concerns

---

**Thank you for contributing to SATHI!** Your efforts help improve mental health care for CRPF personnel.

*This contributing guide follows healthcare software development best practices and emphasizes the critical nature of the system we're building together.*