# SATHI - Architectural Overview

## Table of Contents

1. [System Architecture Overview](#system-architecture-overview)
2. [High-Level System Design](#high-level-system-design)
3. [Microservices Architecture](#microservices-architecture)
4. [Data Architecture](#data-architecture)
5. [AI/ML Architecture](#aiml-architecture)
6. [Security Architecture](#security-architecture)
7. [Deployment Architecture](#deployment-architecture)
8. [Scalability & Performance](#scalability--performance)
9. [Integration Architecture](#integration-architecture)
10. [Monitoring & Observability](#monitoring--observability)

## System Architecture Overview

SATHI (System for Analyzing and Tracking Human Intelligence) is designed as a modern, scalable mental health monitoring platform for CRPF personnel. The architecture follows microservices principles with clear separation of concerns, ensuring maintainability, scalability, and reliability.

### Core Architecture Principles

1. **Modularity**: Clean separation between frontend, backend, AI/ML services, and data layer
2. **Scalability**: Horizontal scaling capabilities for high-load scenarios
3. **Security**: Multi-layered security approach with encryption and access controls
4. **Resilience**: Fault-tolerant design with graceful degradation
5. **Maintainability**: Well-structured codebase with clear interfaces and documentation

## High-Level System Design

```mermaid
graph TB
    subgraph "Presentation Layer"
        A[React Frontend]
        B[Mobile Web Interface]
    end
    
    subgraph "Application Layer"
        C[Flask API Gateway]
        D[Authentication Service]
        E[Admin Service]
        F[Survey Service]
        G[Image Processing Service]
        H[Monitoring Service]
    end
    
    subgraph "AI/ML Layer"
        I[Face Recognition Engine]
        J[Emotion Detection Engine]
        K[NLP Sentiment Analysis]
        L[Risk Assessment Engine]
    end
    
    subgraph "Data Layer"
        M[MySQL Database]
        N[File Storage System]
        O[Model Storage]
        P[Cache Layer]
    end
    
    subgraph "Infrastructure Layer"
        Q[Load Balancer]
        R[Application Servers]
        S[Database Servers]
        T[Storage Servers]
    end
    
    A --> C
    B --> C
    C --> D
    C --> E
    C --> F
    C --> G
    C --> H
    
    E --> I
    F --> J
    F --> K
    G --> I
    G --> J
    H --> L
    
    D --> M
    E --> M
    F --> M
    G --> M
    H --> M
    
    I --> O
    J --> O
    K --> O
    L --> O
    
    G --> N
    
    Q --> R
    R --> S
    R --> T
```

### System Components

| Component | Technology | Purpose |
|-----------|------------|---------|
| Frontend | React + TypeScript | User interface and interaction |
| API Gateway | Flask | Request routing and authentication |
| Database | MySQL 8.0 | Primary data storage |
| AI/ML Services | TensorFlow, OpenCV | Machine learning processing |
| File Storage | Local/Cloud Storage | Image and model file storage |
| Cache | Redis (Optional) | Performance optimization |

## Microservices Architecture

### Service Decomposition

The SATHI system is organized into focused microservices, each handling specific business capabilities:

```mermaid
graph LR
    subgraph "Client Applications"
        WEB[Web Frontend]
        MOB[Mobile Web]
    end
    
    subgraph "API Gateway Layer"
        GW[Flask API Gateway]
    end
    
    subgraph "Core Services"
        AUTH[Authentication Service]
        ADMIN[Admin Management Service]
        SURVEY[Survey Service]
        IMAGE[Image Processing Service]
        MONITOR[System Monitoring Service]
    end
    
    subgraph "AI/ML Services"
        FACE[Face Recognition Service]
        EMOTION[Emotion Detection Service]
        NLP[NLP Analysis Service]
        RISK[Risk Assessment Service]
    end
    
    subgraph "Data Services"
        DB[Database Service]
        STORAGE[File Storage Service]
        CACHE[Caching Service]
    end
    
    WEB --> GW
    MOB --> GW
    
    GW --> AUTH
    GW --> ADMIN
    GW --> SURVEY
    GW --> IMAGE
    GW --> MONITOR
    
    ADMIN --> FACE
    SURVEY --> EMOTION
    SURVEY --> NLP
    IMAGE --> FACE
    IMAGE --> EMOTION
    MONITOR --> RISK
    
    AUTH --> DB
    ADMIN --> DB
    SURVEY --> DB
    IMAGE --> DB
    MONITOR --> DB
    
    IMAGE --> STORAGE
    FACE --> STORAGE
    EMOTION --> STORAGE
```

### Service Responsibilities

#### Authentication Service
- User authentication and authorization
- Session management
- Role-based access control
- Security token validation

#### Admin Management Service
- Dashboard statistics and analytics
- User management (soldiers/admins)
- System configuration
- Report generation

#### Survey Service
- Questionnaire management
- Survey response collection
- Sentiment analysis integration
- Mental health scoring

#### Image Processing Service
- Face detection and recognition
- Image collection and training
- Model management
- Batch processing capabilities

#### System Monitoring Service
- Health checks and system status
- Performance monitoring
- Alert management
- Log aggregation

### Inter-Service Communication

```mermaid
sequenceDiagram
    participant Client
    participant Gateway
    participant AuthSvc
    participant SurveySvc
    participant MLSvc
    participant Database

    Client->>Gateway: Submit Survey Request
    Gateway->>AuthSvc: Validate Session
    AuthSvc-->>Gateway: Session Valid
    
    Gateway->>SurveySvc: Process Survey
    SurveySvc->>MLSvc: Analyze Responses
    MLSvc-->>SurveySvc: Analysis Results
    
    SurveySvc->>Database: Store Results
    Database-->>SurveySvc: Confirmation
    
    SurveySvc-->>Gateway: Processing Complete
    Gateway-->>Client: Survey Submitted
```

## Data Architecture

### Database Design

The system uses a normalized relational database design optimized for mental health data management:

```mermaid
erDiagram
    USERS {
        int user_id PK
        string force_id UK
        string password_hash
        enum user_type
        timestamp created_at
        timestamp last_login
    }
    
    QUESTIONNAIRES {
        int questionnaire_id PK
        string title
        text description
        enum status
        int total_questions
        timestamp created_at
    }
    
    QUESTIONS {
        int question_id PK
        int questionnaire_id FK
        text question_text
        text question_text_hindi
        timestamp created_at
    }
    
    WEEKLY_SESSIONS {
        int session_id PK
        string force_id FK
        int questionnaire_id FK
        int year
        timestamp start_timestamp
        timestamp completion_timestamp
        enum status
        float nlp_avg_score
        float image_avg_score
        float combined_avg_score
        float mental_state_score
    }
    
    QUESTION_RESPONSES {
        int response_id PK
        int session_id FK
        int question_id FK
        text answer_text
        float nlp_depression_score
        float image_depression_score
        float combined_depression_score
        timestamp created_at
    }
    
    CCTV_DETECTIONS {
        int detection_id PK
        int monitoring_id
        string force_id FK
        float depression_score
        string emotion
        text face_image
        timestamp detection_time
    }
    
    SYSTEM_SETTINGS {
        string setting_name PK
        text setting_value
        string category
        enum data_type
        text description
        timestamp updated_at
    }
    
    TRAINED_SOLDIERS {
        string force_id PK
        string model_version
        timestamp trained_at
    }
    
    USERS ||--o{ WEEKLY_SESSIONS : "has"
    QUESTIONNAIRES ||--o{ QUESTIONS : "contains"
    QUESTIONNAIRES ||--o{ WEEKLY_SESSIONS : "used_in"
    WEEKLY_SESSIONS ||--o{ QUESTION_RESPONSES : "contains"
    QUESTIONS ||--o{ QUESTION_RESPONSES : "answered_in"
    USERS ||--o{ CCTV_DETECTIONS : "detected"
    USERS ||--o{ TRAINED_SOLDIERS : "trained"
```

### Data Flow Architecture

```mermaid
flowchart TD
    A[Data Input Sources] --> B[Data Validation Layer]
    B --> C[Data Processing Layer]
    C --> D[Data Storage Layer]
    D --> E[Data Analysis Layer]
    E --> F[Data Presentation Layer]

    subgraph "Input Sources"
        A1[Survey Responses]
        A2[Webcam Images]
        A3[CCTV Feeds]
        A4[System Metrics]
    end

    subgraph "Validation"
        B1[Input Sanitization]
        B2[Schema Validation]
        B3[Business Rules Check]
    end

    subgraph "Processing"
        C1[NLP Processing]
        C2[Image Processing]
        C3[Score Calculation]
        C4[Risk Assessment]
    end

    subgraph "Storage"
        D1[Transactional Data]
        D2[Analytical Data]
        D3[File Storage]
        D4[Model Storage]
    end

    subgraph "Analysis"
        E1[Statistical Analysis]
        E2[Trend Analysis]
        E3[Predictive Modeling]
        E4[Alert Generation]
    end

    subgraph "Presentation"
        F1[Dashboard Views]
        F2[Reports]
        F3[Alerts]
        F4[APIs]
    end

    A1 --> B1
    A2 --> B1
    A3 --> B1
    A4 --> B2

    B1 --> C1
    B1 --> C2
    B2 --> C3
    B3 --> C4

    C1 --> D1
    C2 --> D3
    C3 --> D2
    C4 --> D1

    D1 --> E1
    D2 --> E2
    D3 --> E3
    D1 --> E4

    E1 --> F1
    E2 --> F2
    E3 --> F1
    E4 --> F3
```

### Data Consistency and Integrity

#### ACID Compliance
- **Atomicity**: All database operations are atomic
- **Consistency**: Data integrity constraints are enforced
- **Isolation**: Concurrent transactions are properly isolated
- **Durability**: Committed data is permanently stored

#### Data Validation Layers
1. **Frontend Validation**: Client-side input validation
2. **API Validation**: Server-side request validation
3. **Database Constraints**: Schema-level data integrity
4. **Business Logic Validation**: Domain-specific rules

## AI/ML Architecture

### Machine Learning Pipeline

```mermaid
flowchart TB
    subgraph "Data Collection"
        A1[Survey Text Data]
        A2[Webcam Images]
        A3[Historical Data]
    end

    subgraph "Preprocessing"
        B1[Text Cleaning & NLP]
        B2[Image Processing]
        B3[Feature Extraction]
    end

    subgraph "Model Training"
        C1[Sentiment Analysis Model]
        C2[Face Recognition Model]
        C3[Emotion Detection Model]
        C4[Risk Assessment Model]
    end

    subgraph "Model Deployment"
        D1[Model Serving]
        D2[Real-time Inference]
        D3[Batch Processing]
    end

    subgraph "Feedback Loop"
        E1[Performance Monitoring]
        E2[Model Retraining]
        E3[A/B Testing]
    end

    A1 --> B1
    A2 --> B2
    A3 --> B3

    B1 --> C1
    B2 --> C2
    B2 --> C3
    B1 --> C4
    B3 --> C4

    C1 --> D1
    C2 --> D1
    C3 --> D1
    C4 --> D2

    D1 --> E1
    D2 --> E1
    D3 --> E1

    E1 --> E2
    E2 --> C1
    E2 --> C2
    E2 --> C3
    E2 --> C4
```

### AI/ML Components Detail

#### 1. Natural Language Processing Engine
```python
# Architecture Pattern
class NLPAnalysisEngine:
    def __init__(self):
        self.preprocessor = TextPreprocessor()
        self.sentiment_analyzer = SentimentAnalyzer()
        self.depression_scorer = DepressionScorer()
    
    def analyze_text(self, text: str) -> AnalysisResult:
        # Text preprocessing
        cleaned_text = self.preprocessor.clean(text)
        
        # Sentiment analysis
        sentiment = self.sentiment_analyzer.analyze(cleaned_text)
        
        # Depression scoring
        depression_score = self.depression_scorer.calculate(sentiment)
        
        return AnalysisResult(
            sentiment=sentiment,
            depression_score=depression_score,
            confidence=sentiment.confidence
        )
```

#### 2. Face Recognition Engine
```python
# Architecture Pattern
class FaceRecognitionEngine:
    def __init__(self):
        self.face_detector = HaarCascadeDetector()
        self.feature_extractor = FaceFeatureExtractor()
        self.model_manager = FaceModelManager()
    
    def process_image(self, image: np.ndarray) -> RecognitionResult:
        # Face detection
        faces = self.face_detector.detect(image)
        
        # Feature extraction
        encodings = self.feature_extractor.extract(image, faces)
        
        # Face matching
        matches = self.model_manager.match_faces(encodings)
        
        return RecognitionResult(
            faces_detected=len(faces),
            matches=matches,
            confidence_scores=[m.confidence for m in matches]
        )
```

#### 3. Emotion Detection Engine
```python
# Architecture Pattern
class EmotionDetectionEngine:
    def __init__(self):
        self.face_preprocessor = FacePreprocessor()
        self.emotion_model = EmotionClassificationModel()
        self.depression_mapper = EmotionDepressionMapper()
    
    def detect_emotion(self, face_image: np.ndarray) -> EmotionResult:
        # Face preprocessing
        processed_face = self.face_preprocessor.preprocess(face_image)
        
        # Emotion classification
        emotions = self.emotion_model.predict(processed_face)
        
        # Depression score mapping
        depression_score = self.depression_mapper.map_to_depression(emotions)
        
        return EmotionResult(
            dominant_emotion=emotions.dominant,
            all_emotions=emotions.all,
            depression_score=depression_score
        )
```

### Model Management Architecture

```mermaid
graph TB
    subgraph "Model Development"
        A1[Data Scientists]
        A2[Model Training]
        A3[Model Validation]
        A4[Model Testing]
    end
    
    subgraph "Model Registry"
        B1[Model Versioning]
        B2[Model Metadata]
        B3[Model Artifacts]
        B4[Performance Metrics]
    end
    
    subgraph "Model Deployment"
        C1[Staging Environment]
        C2[A/B Testing]
        C3[Production Deployment]
        C4[Rollback Capability]
    end
    
    subgraph "Model Monitoring"
        D1[Performance Tracking]
        D2[Data Drift Detection]
        D3[Model Degradation]
        D4[Retraining Triggers]
    end
    
    A1 --> A2
    A2 --> A3
    A3 --> A4
    A4 --> B1
    
    B1 --> B2
    B2 --> B3
    B3 --> B4
    
    B4 --> C1
    C1 --> C2
    C2 --> C3
    C3 --> C4
    
    C3 --> D1
    D1 --> D2
    D2 --> D3
    D3 --> D4
    
    D4 --> A2
```

## Security Architecture

### Multi-Layered Security Approach

```mermaid
graph TB
    subgraph "Application Security"
        A1[Authentication Layer]
        A2[Authorization Layer]
        A3[Session Management]
        A4[Input Validation]
    end
    
    subgraph "Data Security"
        B1[Data Encryption]
        B2[Database Security]
        B3[File System Security]
        B4[Backup Security]
    end
    
    subgraph "Network Security"
        C1[HTTPS/TLS]
        C2[Firewall Rules]
        C3[VPN Access]
        C4[API Rate Limiting]
    end
    
    subgraph "Infrastructure Security"
        D1[Server Hardening]
        D2[Access Controls]
        D3[Monitoring & Logging]
        D4[Intrusion Detection]
    end
    
    A1 --> A2
    A2 --> A3
    A3 --> A4
    
    B1 --> B2
    B2 --> B3
    B3 --> B4
    
    C1 --> C2
    C2 --> C3
    C3 --> C4
    
    D1 --> D2
    D2 --> D3
    D3 --> D4
```

### Authentication & Authorization Flow

```mermaid
sequenceDiagram
    participant User
    participant Frontend
    participant AuthService
    participant Database
    participant Application

    User->>Frontend: Login Request
    Frontend->>AuthService: Credentials
    AuthService->>Database: Validate User
    Database-->>AuthService: User Data
    
    alt Valid Credentials
        AuthService->>AuthService: Generate Session
        AuthService->>Database: Store Session
        AuthService-->>Frontend: Session Token
        Frontend-->>User: Login Success
        
        User->>Frontend: API Request
        Frontend->>Application: Request + Token
        Application->>AuthService: Validate Token
        AuthService-->>Application: User Context
        Application-->>Frontend: Authorized Response
    else Invalid Credentials
        AuthService-->>Frontend: Authentication Error
        Frontend-->>User: Login Failed
    end
```

### Data Protection Strategies

#### 1. Data Classification
| Classification | Description | Protection Level |
|----------------|-------------|------------------|
| Public | General system information | Basic encryption |
| Internal | Operational data | Standard encryption |
| Confidential | Personal information | Advanced encryption |
| Restricted | Mental health data | Maximum encryption |

#### 2. Encryption Implementation
```python
# Data Encryption Architecture
class DataEncryption:
    def __init__(self):
        self.key_manager = KeyManager()
        self.cipher = AESCipher()
    
    def encrypt_sensitive_data(self, data: dict) -> str:
        """Encrypt sensitive mental health data"""
        key = self.key_manager.get_encryption_key()
        encrypted_data = self.cipher.encrypt(data, key)
        return encrypted_data
    
    def decrypt_sensitive_data(self, encrypted_data: str) -> dict:
        """Decrypt sensitive mental health data"""
        key = self.key_manager.get_encryption_key()
        decrypted_data = self.cipher.decrypt(encrypted_data, key)
        return decrypted_data
```

#### 3. Access Control Matrix
| Role | Survey Data | User Management | System Config | Reports |
|------|-------------|-----------------|---------------|---------|
| Admin | Full Access | Full Access | Full Access | Full Access |
| Soldier | Own Data Only | No Access | No Access | No Access |
| Viewer | Read Only | No Access | No Access | Read Only |

## Deployment Architecture

### Container-Based Deployment

```mermaid
graph TB
    subgraph "Load Balancer Tier"
        LB[Nginx Load Balancer]
    end
    
    subgraph "Application Tier"
        subgraph "Frontend Containers"
            FE1[React App 1]
            FE2[React App 2]
            FE3[React App 3]
        end
        
        subgraph "Backend Containers"
            BE1[Flask API 1]
            BE2[Flask API 2]
            BE3[Flask API 3]
        end
        
        subgraph "ML Containers"
            ML1[Face Recognition Service]
            ML2[Emotion Detection Service]
            ML3[NLP Analysis Service]
        end
    end
    
    subgraph "Data Tier"
        subgraph "Database Cluster"
            DB1[(MySQL Primary)]
            DB2[(MySQL Replica 1)]
            DB3[(MySQL Replica 2)]
        end
        
        subgraph "Storage"
            FS[File Storage]
            MS[Model Storage]
            BS[Backup Storage]
        end
    end
    
    LB --> FE1
    LB --> FE2
    LB --> FE3
    
    FE1 --> BE1
    FE2 --> BE2
    FE3 --> BE3
    
    BE1 --> ML1
    BE2 --> ML2
    BE3 --> ML3
    
    BE1 --> DB1
    BE2 --> DB1
    BE3 --> DB1
    
    DB1 --> DB2
    DB1 --> DB3
    
    ML1 --> MS
    ML2 --> MS
    ML3 --> MS
    
    BE1 --> FS
    BE2 --> FS
    BE3 --> FS
```

### Cloud Deployment Options

#### AWS Architecture
```mermaid
graph TB
    subgraph "AWS Cloud"
        subgraph "Public Subnet"
            ALB[Application Load Balancer]
            NAT[NAT Gateway]
        end
        
        subgraph "Private Subnet 1"
            ECS1[ECS Cluster 1]
            RDS1[(RDS Primary)]
        end
        
        subgraph "Private Subnet 2"
            ECS2[ECS Cluster 2]
            RDS2[(RDS Standby)]
        end
        
        subgraph "Storage"
            S3[S3 Bucket]
            EFS[EFS Storage]
        end
        
        subgraph "Monitoring"
            CW[CloudWatch]
            XR[X-Ray]
        end
    end
    
    ALB --> ECS1
    ALB --> ECS2
    
    ECS1 --> RDS1
    ECS2 --> RDS1
    
    RDS1 --> RDS2
    
    ECS1 --> S3
    ECS2 --> S3
    ECS1 --> EFS
    ECS2 --> EFS
    
    ECS1 --> CW
    ECS2 --> CW
    ECS1 --> XR
    ECS2 --> XR
```

### Deployment Environments

#### Environment Configuration
| Environment | Purpose | Resources | Deployment |
|-------------|---------|-----------|------------|
| Development | Local development | Minimal | Docker Compose |
| Testing | Automated testing | Medium | Kubernetes |
| Staging | Pre-production testing | Production-like | Kubernetes |
| Production | Live system | High availability | Kubernetes/Cloud |

#### CI/CD Pipeline
```mermaid
graph LR
    A[Code Commit] --> B[Build Trigger]
    B --> C[Unit Tests]
    C --> D[Integration Tests]
    D --> E[Security Scan]
    E --> F[Build Images]
    F --> G[Deploy to Staging]
    G --> H[E2E Tests]
    H --> I[Deploy to Production]
    I --> J[Health Checks]
    J --> K[Monitoring]
```

## Scalability & Performance

### Horizontal Scaling Strategy

```mermaid
graph TB
    subgraph "Traffic Distribution"
        LB[Load Balancer]
        HPA[Horizontal Pod Autoscaler]
    end
    
    subgraph "Application Scaling"
        subgraph "Frontend Pods"
            FE1[Frontend 1]
            FE2[Frontend 2]
            FEN[Frontend N]
        end
        
        subgraph "Backend Pods"
            BE1[Backend 1]
            BE2[Backend 2]
            BEN[Backend N]
        end
        
        subgraph "ML Service Pods"
            ML1[ML Service 1]
            ML2[ML Service 2]
            MLN[ML Service N]
        end
    end
    
    subgraph "Data Scaling"
        subgraph "Database"
            Master[(Master DB)]
            Slave1[(Read Replica 1)]
            SlaveN[(Read Replica N)]
        end
        
        subgraph "Caching"
            Redis1[Redis 1]
            RedisN[Redis N]
        end
    end
    
    LB --> FE1
    LB --> FE2
    LB --> FEN
    
    HPA -.-> FE1
    HPA -.-> BE1
    HPA -.-> ML1
    
    FE1 --> BE1
    FE2 --> BE2
    FEN --> BEN
    
    BE1 --> ML1
    BE2 --> ML2
    BEN --> MLN
    
    BE1 --> Master
    BE2 --> Slave1
    BEN --> SlaveN
    
    Master --> Slave1
    Master --> SlaveN
    
    BE1 --> Redis1
    BE2 --> RedisN
```

### Performance Optimization Strategies

#### 1. Database Optimization
```sql
-- Index optimization for frequent queries
CREATE INDEX idx_weekly_sessions_force_completion 
ON weekly_sessions(force_id, completion_timestamp);

CREATE INDEX idx_question_responses_session 
ON question_responses(session_id, question_id);

CREATE INDEX idx_cctv_detections_force_time 
ON cctv_detections(force_id, detection_time);

-- Partitioning for large tables
CREATE TABLE weekly_sessions_2024 PARTITION OF weekly_sessions
FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');
```

#### 2. Caching Strategy
```python
# Multi-level caching architecture
class CacheManager:
    def __init__(self):
        self.memory_cache = MemoryCache()  # L1 Cache
        self.redis_cache = RedisCache()    # L2 Cache
        self.database = DatabaseLayer()    # L3 Storage
    
    def get_dashboard_stats(self, timeframe: str) -> dict:
        # Check L1 cache first
        cache_key = f"dashboard_stats_{timeframe}"
        
        # L1 Cache check
        result = self.memory_cache.get(cache_key)
        if result:
            return result
        
        # L2 Cache check
        result = self.redis_cache.get(cache_key)
        if result:
            self.memory_cache.set(cache_key, result, ttl=300)
            return result
        
        # Database query
        result = self.database.get_dashboard_stats(timeframe)
        
        # Cache at both levels
        self.redis_cache.set(cache_key, result, ttl=1800)
        self.memory_cache.set(cache_key, result, ttl=300)
        
        return result
```

#### 3. ML Model Optimization
```python
# Model serving optimization
class ModelServingOptimizer:
    def __init__(self):
        self.model_cache = ModelCache()
        self.batch_processor = BatchProcessor()
        self.async_processor = AsyncProcessor()
    
    def optimize_inference(self, requests: List[InferenceRequest]) -> List[Result]:
        # Batch similar requests
        batched_requests = self.batch_processor.group_requests(requests)
        
        # Process batches asynchronously
        tasks = []
        for batch in batched_requests:
            task = self.async_processor.submit(self.process_batch, batch)
            tasks.append(task)
        
        # Collect results
        results = []
        for task in tasks:
            batch_results = task.result()
            results.extend(batch_results)
        
        return results
```

## Integration Architecture

### External System Integration

```mermaid
graph TB
    subgraph "SATHI System"
        API[SATHI API Gateway]
        AUTH[Authentication Service]
        DATA[Data Layer]
    end
    
    subgraph "External Systems"
        CRPF[CRPF HR System]
        MEDICAL[Medical Records System]
        ALERT[Alert Management System]
        BACKUP[Backup System]
    end
    
    subgraph "Integration Layer"
        ESB[Enterprise Service Bus]
        QUEUE[Message Queue]
        WEBHOOK[Webhook Service]
    end
    
    API --> ESB
    ESB --> CRPF
    ESB --> MEDICAL
    ESB --> ALERT
    
    DATA --> QUEUE
    QUEUE --> BACKUP
    
    ALERT --> WEBHOOK
    WEBHOOK --> API
```

### API Integration Patterns

#### 1. Synchronous Integration
```python
# REST API integration
class ExternalAPIIntegration:
    def __init__(self, base_url: str, api_key: str):
        self.base_url = base_url
        self.api_key = api_key
        self.session = requests.Session()
        
    def sync_user_data(self, force_id: str) -> dict:
        """Synchronously fetch user data from HR system"""
        endpoint = f"{self.base_url}/personnel/{force_id}"
        headers = {"Authorization": f"Bearer {self.api_key}"}
        
        response = self.session.get(endpoint, headers=headers)
        response.raise_for_status()
        
        return response.json()
```

#### 2. Asynchronous Integration
```python
# Message queue integration
class MessageQueueIntegration:
    def __init__(self, queue_config: dict):
        self.queue = QueueManager(queue_config)
        
    async def send_alert(self, alert_data: dict):
        """Asynchronously send alert to external system"""
        message = {
            "type": "mental_health_alert",
            "data": alert_data,
            "timestamp": datetime.utcnow().isoformat()
        }
        
        await self.queue.publish("alerts", message)
        
    async def process_alerts(self):
        """Process incoming alerts from queue"""
        async for message in self.queue.subscribe("alerts"):
            await self.handle_alert(message)
```

### Data Synchronization

```mermaid
sequenceDiagram
    participant SATHI
    participant Queue
    participant ExternalSys
    participant Database

    SATHI->>Queue: Publish Data Change Event
    Queue->>ExternalSys: Deliver Event
    ExternalSys->>ExternalSys: Process Update
    ExternalSys->>Queue: Acknowledgment
    Queue->>SATHI: Delivery Confirmed
    
    Note over ExternalSys: External system processes data
    
    ExternalSys->>Queue: Response Data
    Queue->>SATHI: Deliver Response
    SATHI->>Database: Update Local Data
```

## Monitoring & Observability

### Comprehensive Monitoring Stack

```mermaid
graph TB
    subgraph "Application Layer"
        APP1[Frontend Apps]
        APP2[Backend APIs]
        APP3[ML Services]
    end
    
    subgraph "Monitoring Layer"
        METRICS[Metrics Collection]
        LOGS[Log Aggregation]
        TRACES[Distributed Tracing]
        HEALTH[Health Checks]
    end
    
    subgraph "Storage Layer"
        TSDB[Time Series DB]
        LOGDB[Log Database]
        TRACEDB[Trace Database]
    end
    
    subgraph "Visualization Layer"
        DASH[Dashboards]
        ALERTS[Alert Manager]
        REPORTS[Reports]
    end
    
    APP1 --> METRICS
    APP1 --> LOGS
    APP1 --> TRACES
    APP1 --> HEALTH
    
    APP2 --> METRICS
    APP2 --> LOGS
    APP2 --> TRACES
    APP2 --> HEALTH
    
    APP3 --> METRICS
    APP3 --> LOGS
    APP3 --> TRACES
    APP3 --> HEALTH
    
    METRICS --> TSDB
    LOGS --> LOGDB
    TRACES --> TRACEDB
    
    TSDB --> DASH
    LOGDB --> DASH
    TRACEDB --> DASH
    
    DASH --> ALERTS
    DASH --> REPORTS
```

### Observability Implementation

#### 1. Application Metrics
```python
# Metrics collection
from prometheus_client import Counter, Histogram, Gauge

class ApplicationMetrics:
    def __init__(self):
        self.request_count = Counter('http_requests_total', 'Total HTTP requests', ['method', 'endpoint'])
        self.request_duration = Histogram('http_request_duration_seconds', 'HTTP request duration')
        self.active_sessions = Gauge('active_sessions_total', 'Number of active sessions')
        self.ml_inference_time = Histogram('ml_inference_duration_seconds', 'ML inference time')
    
    def record_request(self, method: str, endpoint: str, duration: float):
        self.request_count.labels(method=method, endpoint=endpoint).inc()
        self.request_duration.observe(duration)
    
    def record_ml_inference(self, model_type: str, duration: float):
        self.ml_inference_time.labels(model=model_type).observe(duration)
```

#### 2. Distributed Tracing
```python
# OpenTelemetry integration
from opentelemetry import trace
from opentelemetry.exporter.jaeger.thrift import JaegerExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

class TracingSetup:
    def __init__(self):
        trace.set_tracer_provider(TracerProvider())
        tracer = trace.get_tracer(__name__)
        
        jaeger_exporter = JaegerExporter(
            agent_host_name="localhost",
            agent_port=6831,
        )
        
        span_processor = BatchSpanProcessor(jaeger_exporter)
        trace.get_tracer_provider().add_span_processor(span_processor)
    
    def trace_survey_submission(self, session_id: str):
        tracer = trace.get_tracer(__name__)
        
        with tracer.start_as_current_span("survey_submission") as span:
            span.set_attribute("session_id", session_id)
            
            with tracer.start_as_current_span("nlp_analysis"):
                # NLP processing
                pass
            
            with tracer.start_as_current_span("emotion_detection"):
                # Emotion detection
                pass
            
            with tracer.start_as_current_span("score_calculation"):
                # Score calculation
                pass
```

#### 3. Alert Configuration
```yaml
# Alert rules configuration
alerts:
  - name: HighErrorRate
    condition: error_rate > 0.05
    duration: 5m
    severity: warning
    message: "High error rate detected: {{ $value }}"
    
  - name: DatabaseConnectionFailure
    condition: db_connections_failed > 0
    duration: 1m
    severity: critical
    message: "Database connection failures detected"
    
  - name: MLModelPerformanceDegradation
    condition: ml_model_accuracy < 0.85
    duration: 10m
    severity: warning
    message: "ML model performance degraded: {{ $value }}"
    
  - name: HighMemoryUsage
    condition: memory_usage > 0.9
    duration: 5m
    severity: warning
    message: "High memory usage: {{ $value }}"
```

### Health Check Architecture

```mermaid
graph TB
    subgraph "Health Check Hierarchy"
        APP[Application Health]
        DEP[Dependency Health]
        SYS[System Health]
    end
    
    subgraph "Check Types"
        LIVE[Liveness Checks]
        READY[Readiness Checks]
        STARTUP[Startup Checks]
    end
    
    subgraph "Dependencies"
        DB[(Database)]
        CACHE[Cache]
        EXT[External APIs]
        FS[File System]
    end
    
    APP --> LIVE
    APP --> READY
    APP --> STARTUP
    
    DEP --> DB
    DEP --> CACHE
    DEP --> EXT
    DEP --> FS
    
    SYS --> APP
    SYS --> DEP
```

---

This architectural overview provides a comprehensive view of the SATHI system's design, covering all major architectural aspects from high-level system design to detailed implementation patterns. The architecture ensures scalability, security, and maintainability while supporting the complex requirements of mental health monitoring for CRPF personnel.