# SATHI Frontend - React TypeScript Application

This is the frontend application for SATHI (CRPF Mental Health Monitoring System), built with React 18, TypeScript, and Tailwind CSS. The application provides a modern, responsive interface for mental health monitoring and assessment.

## 🏗️ Architecture Overview

The frontend is built with a component-based architecture using React TypeScript:

```
src/
├── components/          # Reusable UI components
│   ├── common/         # Common components (buttons, inputs, etc.)
│   ├── layout/         # Layout components (header, sidebar, etc.)
│   └── charts/         # Chart components for data visualization
├── pages/              # Application pages/routes
│   ├── admin/          # Admin dashboard and management
│   ├── survey/         # Survey forms and results
│   └── auth/           # Authentication pages
├── services/           # API service layer
│   ├── api.ts          # Main API client
│   ├── auth.service.ts # Authentication services
│   └── *.service.ts    # Feature-specific services
├── context/            # React context providers
│   ├── AuthContext.tsx # Authentication state management
│   └── ThemeContext.tsx# Theme and UI state
├── utils/              # Utility functions and helpers
│   ├── constants.ts    # Application constants
│   ├── helpers.ts      # Helper functions
│   └── types.ts        # TypeScript type definitions
├── styles/             # CSS and styling files
└── router.tsx          # Application routing configuration
```

## 🚀 Quick Start

### Prerequisites

- **Node.js 16+** (18+ recommended)
- **npm 8+** or **yarn 1.22+**
- **Backend API** running on `http://localhost:5000`

### Installation

1. **Install dependencies**:
   ```bash
   npm install
   ```

2. **Configure environment**:
   ```bash
   # Create environment file
   cp .env.example .env.local
   
   # Edit with your configuration
   REACT_APP_API_URL=http://localhost:5000/api
   REACT_APP_ENVIRONMENT=development
   ```

3. **Start development server**:
   ```bash
   npm start
   ```

   The application will open at [http://localhost:3000](http://localhost:3000)

## 📜 Available Scripts

### Development

- **`npm start`** - Start development server with hot reload
- **`npm run dev`** - Alias for `npm start`
- **`npm run build`** - Build production bundle
- **`npm run preview`** - Preview production build locally

### Testing

- **`npm test`** - Run tests in interactive watch mode
- **`npm run test:coverage`** - Run tests with coverage report
- **`npm run test:ci`** - Run tests in CI mode (single run)

### Code Quality

- **`npm run lint`** - Run ESLint for code quality checks
- **`npm run lint:fix`** - Fix auto-fixable linting issues
- **`npm run type-check`** - Run TypeScript type checking
- **`npm run format`** - Format code with Prettier

### Build & Deployment

- **`npm run build`** - Create production build
- **`npm run analyze`** - Analyze bundle size
- **`npm run serve`** - Serve production build locally

## 🎨 UI/UX Features

### Design System

- **Tailwind CSS** for utility-first styling
- **Responsive Design** for mobile and desktop
- **Dark/Light Mode** support
- **Accessibility** compliance (WCAG 2.1 AA)

### Key Components

1. **Dashboard Components**
   - Real-time statistics cards
   - Interactive charts and graphs
   - Data visualization for mental health metrics

2. **Survey Components**
   - Multi-step survey forms
   - Progress tracking
   - Dynamic question rendering

3. **Admin Components**
   - User management interfaces
   - System configuration panels
   - Reporting and analytics tools

4. **Authentication**
   - Secure login/logout flows
   - Session management
   - Role-based access control

## 🔒 Security Features

### Frontend Security

- **Content Security Policy** (CSP) headers
- **XSS Protection** with input sanitization
- **Secure HTTP Headers** configuration
- **JWT Token Management** with automatic refresh
- **Route Protection** based on user roles

### Data Handling

- **Input Validation** on all forms
- **Sanitization** of user inputs
- **Secure API Communication** over HTTPS
- **No Sensitive Data** stored in localStorage

## 🌐 API Integration

### Service Layer Architecture

```typescript
// Example API service usage
import { authService, surveyService } from '../services';

// Authentication
const user = await authService.login(credentials);

// Survey operations
const surveys = await surveyService.getAllSurveys();
const result = await surveyService.submitSurvey(surveyData);
```

### Error Handling

- **Global Error Boundary** for React error catching
- **API Error Handling** with user-friendly messages
- **Network Error Recovery** with retry mechanisms
- **Loading States** for better user experience

## 📱 Responsive Design

### Breakpoints

- **Mobile**: 320px - 768px
- **Tablet**: 768px - 1024px
- **Desktop**: 1024px+

### Mobile Features

- **Touch-friendly** interface elements
- **Optimized Performance** for mobile devices
- **Progressive Web App** (PWA) capabilities
- **Offline Support** for critical features

## 🧪 Testing Strategy

### Testing Framework

- **Jest** for unit testing
- **React Testing Library** for component testing
- **Cypress** for end-to-end testing
- **MSW** for API mocking

### Test Coverage

- **Components**: >90% coverage target
- **Services**: >95% coverage target
- **Utilities**: >95% coverage target
- **Critical Paths**: 100% coverage required

### Running Tests

```bash
# Unit tests
npm test

# E2E tests
npm run test:e2e

# Generate coverage report
npm run test:coverage
```

## 🚀 Deployment

### Build Configuration

```bash
# Production build
npm run build

# Build with environment
REACT_APP_ENVIRONMENT=production npm run build
```

### Environment Variables

```bash
# API Configuration
REACT_APP_API_URL=https://api.sathi.crpf.gov.in
REACT_APP_WS_URL=wss://api.sathi.crpf.gov.in/ws

# Feature Flags
REACT_APP_ENABLE_ANALYTICS=true
REACT_APP_ENABLE_PUSH_NOTIFICATIONS=true

# Security
REACT_APP_CSP_NONCE=auto-generated
```

### Deployment Targets

- **Development**: Netlify/Vercel for preview deployments
- **Staging**: Docker containers in Kubernetes
- **Production**: Load-balanced containers with CDN

## 🔧 Development Guidelines

### Code Style

- **TypeScript Strict Mode** enabled
- **ESLint** with React and TypeScript rules
- **Prettier** for code formatting
- **Husky** for pre-commit hooks

### Component Guidelines

```typescript
// Component template
interface ComponentProps {
  title: string;
  data: DataType[];
  onAction: (id: string) => void;
}

const Component: React.FC<ComponentProps> = ({ 
  title, 
  data, 
  onAction 
}) => {
  // Component implementation
  return (
    <div className="component-container">
      {/* JSX content */}
    </div>
  );
};

export default Component;
```

### State Management

- **React Context** for global state
- **useState/useReducer** for local state
- **React Query** for server state management
- **Custom Hooks** for reusable logic

## 📊 Performance Optimization

### Optimization Techniques

- **Code Splitting** with React.lazy()
- **Bundle Optimization** with Webpack analysis
- **Image Optimization** with WebP format
- **Caching Strategies** for static assets

### Performance Monitoring

- **Web Vitals** tracking
- **Bundle Size** monitoring
- **Runtime Performance** profiling
- **User Experience** metrics

## 🆘 Troubleshooting

### Common Issues

1. **Build Failures**
   ```bash
   # Clear cache and reinstall
   rm -rf node_modules package-lock.json
   npm install
   ```

2. **Type Errors**
   ```bash
   # Check TypeScript configuration
   npm run type-check
   ```

3. **Linting Issues**
   ```bash
   # Auto-fix linting problems
   npm run lint:fix
   ```

### Debug Mode

```bash
# Enable debug logging
REACT_APP_DEBUG=true npm start
```

## 📚 Additional Resources

- **[Frontend Architecture Guide](../documentation/frontend/architecture.md)**
- **[Component Documentation](../documentation/frontend/components.md)**
- **[State Management Guide](../documentation/frontend/state-management.md)**
- **[API Integration Guide](../documentation/api/README.md)**
- **[Contributing Guidelines](../CONTRIBUTING.md)**

## 🤝 Contributing

Please read our [Contributing Guidelines](../CONTRIBUTING.md) before submitting pull requests.

### Development Workflow

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Run tests and linting
5. Submit a pull request

## 📄 License

This project is proprietary software developed for the Central Reserve Police Force (CRPF). All rights reserved.

---

**Note**: This frontend application handles sensitive mental health data. Please ensure all development and deployment practices comply with applicable privacy and security regulations.