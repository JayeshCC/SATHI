# Frontend Architecture - SATHI

Comprehensive guide to the SATHI frontend architecture, built with React TypeScript and modern web technologies.

## 📋 Table of Contents

1. [Architecture Overview](#architecture-overview)
2. [Technology Stack](#technology-stack)
3. [Project Structure](#project-structure)
4. [Component Architecture](#component-architecture)
5. [State Management](#state-management)
6. [Routing & Navigation](#routing--navigation)
7. [API Integration](#api-integration)
8. [Styling & Design System](#styling--design-system)
9. [Performance Optimization](#performance-optimization)
10. [Security Implementation](#security-implementation)

## 🏗️ Architecture Overview

### High-Level Architecture

The SATHI frontend follows a modern, component-based architecture using React with TypeScript:

```
┌─────────────────────────────────────────────────────────────┐
│                    Browser Environment                       │
├─────────────────────────────────────────────────────────────┤
│  React Application (Single Page Application)                │
│  ┌─────────────────┐  ┌─────────────────┐  ┌──────────────┐ │
│  │   Presentation  │  │   Business      │  │   Data       │ │
│  │     Layer       │  │     Logic       │  │    Layer     │ │
│  │                 │  │                 │  │              │ │
│  │ • Components    │  │ • State Mgmt    │  │ • API Client │ │
│  │ • Pages         │  │ • Hooks         │  │ • Services   │ │
│  │ • UI Elements   │  │ • Context       │  │ • Cache      │ │
│  └─────────────────┘  └─────────────────┘  └──────────────┘ │
├─────────────────────────────────────────────────────────────┤
│                     Development Tools                       │
│  TypeScript • ESLint • Prettier • Jest • Cypress          │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    Backend API Services                     │
│        Flask REST API • WebSocket • File Upload            │
└─────────────────────────────────────────────────────────────┘
```

### Design Principles

**1. Component-Driven Development**
- Reusable, composable components
- Clear separation of concerns
- Props-based data flow
- Testable component interfaces

**2. Type Safety**
- Strict TypeScript configuration
- Comprehensive type definitions
- Runtime type validation
- IDE support and autocomplete

**3. Performance First**
- Code splitting and lazy loading
- Optimized bundle sizes
- Efficient re-rendering
- Resource caching strategies

**4. Security by Design**
- Input validation and sanitization
- XSS protection
- Secure API communication
- Content Security Policy

## 🔧 Technology Stack

### Core Technologies

```typescript
// Primary Stack
const techStack = {
  framework: "React 18.2+",
  language: "TypeScript 4.9+",
  bundler: "Create React App (Webpack 5)",
  styling: "Tailwind CSS 3.0+",
  stateManagement: "React Context + useReducer",
  routing: "React Router v6",
  testing: {
    unit: "Jest + React Testing Library",
    e2e: "Cypress",
    coverage: "Jest Coverage Reports"
  },
  linting: {
    code: "ESLint",
    formatting: "Prettier",
    types: "TypeScript"
  }
};
```

### Supporting Libraries

```typescript
// Additional Dependencies
const dependencies = {
  ui: [
    "@headlessui/react",     // Accessible UI components
    "@heroicons/react",      // Icon library
    "react-hot-toast",       // Notifications
    "framer-motion"          // Animations
  ],
  data: [
    "axios",                 // HTTP client
    "react-query",           // Server state management
    "formik",                // Form handling
    "yup"                    // Validation
  ],
  utilities: [
    "date-fns",              // Date manipulation
    "lodash",                // Utility functions
    "classnames",            // CSS class management
    "react-helmet"           // Document head management
  ],
  development: [
    "@types/react",          // TypeScript definitions
    "@testing-library/react", // Testing utilities
    "msw",                   // API mocking
    "storybook"              // Component documentation
  ]
};
```

## 📁 Project Structure

### Directory Organization

```
src/
├── components/              # Reusable UI components
│   ├── common/             # Generic components
│   │   ├── Button/
│   │   │   ├── Button.tsx
│   │   │   ├── Button.test.tsx
│   │   │   ├── Button.stories.tsx
│   │   │   └── index.ts
│   │   ├── Input/
│   │   ├── Modal/
│   │   └── LoadingSpinner/
│   ├── layout/             # Layout components
│   │   ├── Header/
│   │   ├── Sidebar/
│   │   ├── Footer/
│   │   └── Navigation/
│   ├── charts/             # Data visualization
│   │   ├── LineChart/
│   │   ├── BarChart/
│   │   └── PieChart/
│   └── forms/              # Form components
│       ├── SurveyForm/
│       ├── LoginForm/
│       └── ProfileForm/
├── pages/                  # Page components (routes)
│   ├── auth/
│   │   ├── LoginPage.tsx
│   │   └── LogoutPage.tsx
│   ├── admin/
│   │   ├── Dashboard.tsx
│   │   ├── UserManagement.tsx
│   │   └── Settings.tsx
│   ├── survey/
│   │   ├── SurveyForm.tsx
│   │   └── Results.tsx
│   └── patient/
│       ├── Dashboard.tsx
│       └── History.tsx
├── services/               # API and business logic
│   ├── api/
│   │   ├── client.ts       # Axios configuration
│   │   ├── endpoints.ts    # API endpoints
│   │   └── types.ts        # API type definitions
│   ├── auth.service.ts     # Authentication service
│   ├── survey.service.ts   # Survey operations
│   ├── admin.service.ts    # Admin operations
│   └── user.service.ts     # User management
├── context/                # React Context providers
│   ├── AuthContext.tsx     # Authentication state
│   ├── ThemeContext.tsx    # UI theme state
│   └── NotificationContext.tsx # Notifications
├── hooks/                  # Custom React hooks
│   ├── useAuth.ts         # Authentication hook
│   ├── useApi.ts          # API operations hook
│   ├── useLocalStorage.ts # Local storage hook
│   └── useDebounce.ts     # Debounce hook
├── utils/                  # Utility functions
│   ├── constants.ts        # Application constants
│   ├── helpers.ts          # Helper functions
│   ├── validators.ts       # Validation functions
│   ├── formatters.ts       # Data formatters
│   └── types.ts            # Global type definitions
├── styles/                 # Styling files
│   ├── globals.css         # Global styles
│   ├── components.css      # Component styles
│   └── utilities.css       # Utility classes
├── assets/                 # Static assets
│   ├── images/
│   ├── icons/
│   └── fonts/
├── __tests__/              # Test utilities
│   ├── setupTests.ts
│   ├── testUtils.tsx
│   └── mocks/
└── router.tsx              # Application routing
```

### File Naming Conventions

```typescript
// Component files: PascalCase
Button.tsx              // Component implementation
Button.test.tsx         // Unit tests
Button.stories.tsx      // Storybook stories
Button.module.css       // Component-specific styles
index.ts               // Barrel export

// Service files: camelCase
authService.ts         // Service implementation
authService.test.ts    // Service tests

// Hook files: camelCase starting with 'use'
useAuth.ts            // Custom hook
useAuth.test.ts       // Hook tests

// Utility files: camelCase
dateHelpers.ts        // Utility functions
constants.ts          // Constants
types.ts             // Type definitions
```

## 🧩 Component Architecture

### Component Hierarchy

```typescript
// Component Architecture Pattern
App
├── AuthProvider           // Authentication context
├── ThemeProvider         // Theme context
├── NotificationProvider  // Notification context
└── Router
    ├── PublicRoutes
    │   ├── LoginPage
    │   └── ForgotPasswordPage
    └── PrivateRoutes
        ├── Layout
        │   ├── Header
        │   ├── Sidebar
        │   └── Main
        │       ├── Dashboard
        │       ├── SurveyPages
        │       ├── AdminPages
        │       └── ProfilePages
        └── Footer
```

### Component Design Patterns

#### 1. Compound Components

```typescript
// Compound component pattern for complex UI
const Card = ({ children, className = "" }) => (
  <div className={`bg-white rounded-lg shadow ${className}`}>
    {children}
  </div>
);

const CardHeader = ({ children, className = "" }) => (
  <div className={`px-6 py-4 border-b ${className}`}>
    {children}
  </div>
);

const CardBody = ({ children, className = "" }) => (
  <div className={`px-6 py-4 ${className}`}>
    {children}
  </div>
);

const CardFooter = ({ children, className = "" }) => (
  <div className={`px-6 py-4 border-t bg-gray-50 ${className}`}>
    {children}
  </div>
);

// Usage
<Card>
  <Card.Header>
    <h2>Assessment Results</h2>
  </Card.Header>
  <Card.Body>
    <AssessmentChart data={results} />
  </Card.Body>
  <Card.Footer>
    <Button>View Details</Button>
  </Card.Footer>
</Card>
```

#### 2. Render Props Pattern

```typescript
// DataFetcher component using render props
interface DataFetcherProps<T> {
  url: string;
  children: (data: {
    data: T | null;
    loading: boolean;
    error: string | null;
    refetch: () => void;
  }) => React.ReactNode;
}

const DataFetcher = <T,>({ url, children }: DataFetcherProps<T>) => {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<string | null>(null);

  const fetchData = useCallback(async () => {
    setLoading(true);
    setError(null);
    
    try {
      const response = await api.get<T>(url);
      setData(response.data);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'Unknown error');
    } finally {
      setLoading(false);
    }
  }, [url]);

  useEffect(() => {
    fetchData();
  }, [fetchData]);

  return <>{children({ data, loading, error, refetch: fetchData })}</>;
};

// Usage
<DataFetcher<SurveyResult[]> url="/api/surveys/results">
  {({ data, loading, error, refetch }) => {
    if (loading) return <LoadingSpinner />;
    if (error) return <ErrorMessage message={error} onRetry={refetch} />;
    if (!data) return <EmptyState />;
    
    return <SurveyResultsList results={data} />;
  }}
</DataFetcher>
```

#### 3. Higher-Order Components (HOCs)

```typescript
// HOC for authentication protection
const withAuth = <P extends object>(
  Component: React.ComponentType<P>,
  requiredPermissions: string[] = []
) => {
  return (props: P) => {
    const { user, isAuthenticated, hasPermissions } = useAuth();

    if (!isAuthenticated) {
      return <Navigate to="/login" replace />;
    }

    if (requiredPermissions.length > 0 && !hasPermissions(requiredPermissions)) {
      return <UnauthorizedPage />;
    }

    return <Component {...props} />;
  };
};

// Usage
const ProtectedAdminDashboard = withAuth(AdminDashboard, ['admin']);
```

### Component Best Practices

```typescript
// Well-structured component example
interface SurveyCardProps {
  survey: Survey;
  onTake: (surveyId: string) => void;
  onViewResults: (surveyId: string) => void;
  disabled?: boolean;
  className?: string;
}

const SurveyCard: React.FC<SurveyCardProps> = ({
  survey,
  onTake,
  onViewResults,
  disabled = false,
  className = ""
}) => {
  // Memoized calculations
  const isCompleted = useMemo(() => 
    survey.status === 'completed', [survey.status]
  );

  const estimatedTime = useMemo(() => 
    `${survey.questionCount * 0.5} - ${survey.questionCount * 1} min`,
    [survey.questionCount]
  );

  // Event handlers
  const handleTakeClick = useCallback(() => {
    if (!disabled && !isCompleted) {
      onTake(survey.id);
    }
  }, [survey.id, onTake, disabled, isCompleted]);

  const handleViewResultsClick = useCallback(() => {
    if (isCompleted) {
      onViewResults(survey.id);
    }
  }, [survey.id, onViewResults, isCompleted]);

  return (
    <Card className={className}>
      <Card.Header>
        <div className="flex justify-between items-center">
          <h3 className="text-lg font-semibold">{survey.title}</h3>
          <StatusBadge status={survey.status} />
        </div>
      </Card.Header>
      
      <Card.Body>
        <p className="text-gray-600 mb-4">{survey.description}</p>
        
        <div className="space-y-2">
          <InfoItem label="Questions" value={survey.questionCount} />
          <InfoItem label="Estimated Time" value={estimatedTime} />
          <InfoItem label="Last Taken" value={survey.lastTaken} />
        </div>
      </Card.Body>
      
      <Card.Footer>
        <div className="flex space-x-2">
          {!isCompleted && (
            <Button
              onClick={handleTakeClick}
              disabled={disabled}
              variant="primary"
            >
              Take Survey
            </Button>
          )}
          
          {isCompleted && (
            <Button
              onClick={handleViewResultsClick}
              variant="secondary"
            >
              View Results
            </Button>
          )}
        </div>
      </Card.Footer>
    </Card>
  );
};

export default memo(SurveyCard);
```

## 🔄 State Management

### Context-Based State Management

SATHI uses React Context for global state management, avoiding the complexity of external state management libraries:

#### Authentication Context

```typescript
// AuthContext.tsx
interface AuthState {
  user: User | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  permissions: string[];
}

interface AuthContextType extends AuthState {
  login: (credentials: LoginCredentials) => Promise<boolean>;
  logout: () => Promise<void>;
  checkPermission: (permission: string) => boolean;
  refreshToken: () => Promise<void>;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider: React.FC<{ children: React.ReactNode }> = ({ 
  children 
}) => {
  const [state, dispatch] = useReducer(authReducer, initialAuthState);

  const login = useCallback(async (credentials: LoginCredentials) => {
    dispatch({ type: 'LOGIN_START' });
    
    try {
      const response = await authService.login(credentials);
      
      if (response.success) {
        dispatch({ 
          type: 'LOGIN_SUCCESS', 
          payload: response.user 
        });
        return true;
      } else {
        dispatch({ 
          type: 'LOGIN_FAILURE', 
          payload: response.error 
        });
        return false;
      }
    } catch (error) {
      dispatch({ 
        type: 'LOGIN_FAILURE', 
        payload: 'Login failed' 
      });
      return false;
    }
  }, []);

  const logout = useCallback(async () => {
    try {
      await authService.logout();
    } finally {
      dispatch({ type: 'LOGOUT' });
    }
  }, []);

  const checkPermission = useCallback((permission: string) => {
    return state.permissions.includes(permission);
  }, [state.permissions]);

  const value = {
    ...state,
    login,
    logout,
    checkPermission,
    refreshToken: authService.refreshToken
  };

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
};
```

#### Survey Context

```typescript
// SurveyContext.tsx
interface SurveyState {
  currentSurvey: Survey | null;
  responses: SurveyResponse[];
  progress: number;
  isSubmitting: boolean;
  emotionData: EmotionAnalysis | null;
}

const SurveyContext = createContext<{
  state: SurveyState;
  actions: {
    startSurvey: (surveyId: string) => void;
    updateResponse: (questionId: string, answer: any) => void;
    submitSurvey: () => Promise<void>;
    saveProgress: () => Promise<void>;
    setEmotionData: (data: EmotionAnalysis) => void;
  };
} | undefined>(undefined);

export const SurveyProvider: React.FC<{ children: React.ReactNode }> = ({
  children
}) => {
  const [state, setState] = useState<SurveyState>(initialSurveyState);

  const actions = useMemo(() => ({
    startSurvey: (surveyId: string) => {
      // Implementation
    },
    updateResponse: (questionId: string, answer: any) => {
      setState(prev => ({
        ...prev,
        responses: prev.responses.map(r =>
          r.questionId === questionId ? { ...r, answer } : r
        )
      }));
    },
    submitSurvey: async () => {
      // Implementation
    },
    saveProgress: async () => {
      // Implementation
    },
    setEmotionData: (data: EmotionAnalysis) => {
      setState(prev => ({ ...prev, emotionData: data }));
    }
  }), []);

  return (
    <SurveyContext.Provider value={{ state, actions }}>
      {children}
    </SurveyContext.Provider>
  );
};
```

### Local State Management

For component-specific state, use React hooks effectively:

```typescript
// Custom hook for form state management
const useFormState = <T>(initialValues: T, validationSchema?: any) => {
  const [values, setValues] = useState<T>(initialValues);
  const [errors, setErrors] = useState<Partial<Record<keyof T, string>>>({});
  const [touched, setTouched] = useState<Partial<Record<keyof T, boolean>>>({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const setValue = useCallback((field: keyof T, value: any) => {
    setValues(prev => ({ ...prev, [field]: value }));
    
    // Clear error when user starts typing
    if (errors[field]) {
      setErrors(prev => ({ ...prev, [field]: undefined }));
    }
  }, [errors]);

  const setFieldTouched = useCallback((field: keyof T) => {
    setTouched(prev => ({ ...prev, [field]: true }));
  }, []);

  const validate = useCallback(async () => {
    if (!validationSchema) return true;

    try {
      await validationSchema.validate(values, { abortEarly: false });
      setErrors({});
      return true;
    } catch (validationErrors) {
      const errorMap: Partial<Record<keyof T, string>> = {};
      validationErrors.inner.forEach((error: any) => {
        errorMap[error.path as keyof T] = error.message;
      });
      setErrors(errorMap);
      return false;
    }
  }, [values, validationSchema]);

  const handleSubmit = useCallback(async (onSubmit: (values: T) => Promise<void>) => {
    setIsSubmitting(true);
    
    const isValid = await validate();
    if (!isValid) {
      setIsSubmitting(false);
      return;
    }

    try {
      await onSubmit(values);
    } finally {
      setIsSubmitting(false);
    }
  }, [values, validate]);

  return {
    values,
    errors,
    touched,
    isSubmitting,
    setValue,
    setFieldTouched,
    validate,
    handleSubmit,
    reset: () => setValues(initialValues)
  };
};
```

## 🚦 Routing & Navigation

### Route Configuration

```typescript
// router.tsx
const AppRouter: React.FC = () => {
  return (
    <BrowserRouter>
      <Routes>
        {/* Public Routes */}
        <Route path="/login" element={<LoginPage />} />
        <Route path="/forgot-password" element={<ForgotPasswordPage />} />
        
        {/* Protected Routes */}
        <Route element={<ProtectedRoute />}>
          <Route path="/" element={<DashboardLayout />}>
            <Route index element={<Dashboard />} />
            
            {/* Survey Routes */}
            <Route path="survey">
              <Route index element={<SurveyList />} />
              <Route path=":id" element={<SurveyForm />} />
              <Route path="results" element={<SurveyResults />} />
            </Route>
            
            {/* Admin Routes */}
            <Route element={<AdminRoute />}>
              <Route path="admin">
                <Route index element={<AdminDashboard />} />
                <Route path="users" element={<UserManagement />} />
                <Route path="settings" element={<SystemSettings />} />
                <Route path="reports" element={<Reports />} />
              </Route>
            </Route>
            
            {/* Profile Routes */}
            <Route path="profile" element={<ProfilePage />} />
            <Route path="settings" element={<UserSettings />} />
          </Route>
        </Route>
        
        {/* Error Routes */}
        <Route path="/404" element={<NotFoundPage />} />
        <Route path="/unauthorized" element={<UnauthorizedPage />} />
        <Route path="*" element={<Navigate to="/404" replace />} />
      </Routes>
    </BrowserRouter>
  );
};
```

### Route Protection

```typescript
// ProtectedRoute component
const ProtectedRoute: React.FC = () => {
  const { isAuthenticated, isLoading } = useAuth();

  if (isLoading) {
    return <LoadingPage />;
  }

  if (!isAuthenticated) {
    return <Navigate to="/login" replace />;
  }

  return <Outlet />;
};

// Admin route protection
const AdminRoute: React.FC = () => {
  const { checkPermission } = useAuth();

  if (!checkPermission('admin')) {
    return <Navigate to="/unauthorized" replace />;
  }

  return <Outlet />;
};
```

## 🌐 API Integration

### API Client Configuration

```typescript
// services/api/client.ts
const createApiClient = () => {
  const client = axios.create({
    baseURL: process.env.REACT_APP_API_URL || 'http://localhost:5000/api',
    timeout: 10000,
    headers: {
      'Content-Type': 'application/json'
    }
  });

  // Request interceptor for authentication
  client.interceptors.request.use(
    (config) => {
      const token = getAuthToken();
      if (token) {
        config.headers.Authorization = `Bearer ${token}`;
      }
      return config;
    },
    (error) => Promise.reject(error)
  );

  // Response interceptor for error handling
  client.interceptors.response.use(
    (response) => response,
    async (error) => {
      if (error.response?.status === 401) {
        // Token expired, try refresh
        try {
          await refreshAuthToken();
          return client.request(error.config);
        } catch (refreshError) {
          // Refresh failed, redirect to login
          window.location.href = '/login';
        }
      }
      return Promise.reject(error);
    }
  );

  return client;
};

export const apiClient = createApiClient();
```

### Service Layer Implementation

```typescript
// services/survey.service.ts
export class SurveyService {
  private static instance: SurveyService;
  
  public static getInstance(): SurveyService {
    if (!SurveyService.instance) {
      SurveyService.instance = new SurveyService();
    }
    return SurveyService.instance;
  }

  async getSurveys(): Promise<Survey[]> {
    try {
      const response = await apiClient.get<ApiResponse<Survey[]>>('/surveys');
      return response.data.data || [];
    } catch (error) {
      throw new Error(`Failed to fetch surveys: ${error.message}`);
    }
  }

  async submitSurvey(surveyData: SurveySubmission): Promise<SurveyResult> {
    try {
      const response = await apiClient.post<ApiResponse<SurveyResult>>(
        '/surveys/submit',
        surveyData
      );
      
      if (!response.data.success) {
        throw new Error(response.data.message || 'Survey submission failed');
      }
      
      return response.data.data!;
    } catch (error) {
      throw new Error(`Failed to submit survey: ${error.message}`);
    }
  }

  async uploadImage(file: File, surveyId: string): Promise<EmotionAnalysis> {
    try {
      const formData = new FormData();
      formData.append('image', file);
      formData.append('survey_id', surveyId);

      const response = await apiClient.post<ApiResponse<EmotionAnalysis>>(
        '/image/emotion-detection',
        formData,
        {
          headers: { 'Content-Type': 'multipart/form-data' }
        }
      );

      return response.data.data!;
    } catch (error) {
      throw new Error(`Failed to analyze emotion: ${error.message}`);
    }
  }
}

export const surveyService = SurveyService.getInstance();
```

## 🎨 Styling & Design System

### Tailwind CSS Configuration

```javascript
// tailwind.config.js
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
    "./public/index.html"
  ],
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          100: '#dbeafe',
          500: '#3b82f6',
          600: '#2563eb',
          700: '#1d4ed8',
          900: '#1e3a8a'
        },
        secondary: {
          50: '#f8fafc',
          100: '#f1f5f9',
          500: '#64748b',
          600: '#475569',
          700: '#334155',
          900: '#0f172a'
        },
        success: {
          50: '#ecfdf5',
          500: '#10b981',
          600: '#059669'
        },
        warning: {
          50: '#fffbeb',
          500: '#f59e0b',
          600: '#d97706'
        },
        error: {
          50: '#fef2f2',
          500: '#ef4444',
          600: '#dc2626'
        }
      },
      fontFamily: {
        sans: ['Inter', 'system-ui', 'sans-serif'],
        mono: ['JetBrains Mono', 'monospace']
      },
      spacing: {
        '18': '4.5rem',
        '88': '22rem'
      },
      animation: {
        'fade-in': 'fadeIn 0.5s ease-in-out',
        'slide-up': 'slideUp 0.3s ease-out'
      }
    }
  },
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography')
  ]
};
```

### Component Styling System

```typescript
// utils/classNames.ts
export const cn = (...classes: (string | undefined | null | false)[]) => {
  return classes.filter(Boolean).join(' ');
};

// Component variants system
const buttonVariants = {
  variant: {
    primary: 'bg-primary-600 text-white hover:bg-primary-700',
    secondary: 'bg-secondary-200 text-secondary-900 hover:bg-secondary-300',
    danger: 'bg-error-600 text-white hover:bg-error-700',
    ghost: 'bg-transparent text-secondary-700 hover:bg-secondary-100'
  },
  size: {
    sm: 'px-3 py-1.5 text-sm',
    md: 'px-4 py-2',
    lg: 'px-6 py-3 text-lg'
  }
};

interface ButtonProps {
  variant?: keyof typeof buttonVariants.variant;
  size?: keyof typeof buttonVariants.size;
  className?: string;
  children: React.ReactNode;
  // ... other props
}

const Button: React.FC<ButtonProps> = ({
  variant = 'primary',
  size = 'md',
  className,
  children,
  ...props
}) => {
  return (
    <button
      className={cn(
        'inline-flex items-center justify-center rounded-md font-medium transition-colors',
        'focus:outline-none focus:ring-2 focus:ring-primary-500 focus:ring-offset-2',
        'disabled:opacity-50 disabled:pointer-events-none',
        buttonVariants.variant[variant],
        buttonVariants.size[size],
        className
      )}
      {...props}
    >
      {children}
    </button>
  );
};
```

This comprehensive frontend architecture guide provides the foundation for building scalable, maintainable, and performant React applications for the SATHI system. The architecture emphasizes type safety, component reusability, performance optimization, and security best practices.