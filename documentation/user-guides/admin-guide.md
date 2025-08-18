# Administrator Guide - SATHI

Comprehensive guide for system administrators managing the SATHI mental health monitoring system.

## 📋 Table of Contents

1. [Administrator Overview](#administrator-overview)
2. [System Access & Permissions](#system-access--permissions)
3. [User Management](#user-management)
4. [System Configuration](#system-configuration)
5. [Monitoring & Analytics](#monitoring--analytics)
6. [Data Management](#data-management)
7. [Security Administration](#security-administration)
8. [Backup & Recovery](#backup--recovery)
9. [Troubleshooting](#troubleshooting)
10. [Best Practices](#best-practices)

## 🎯 Administrator Overview

### Administrative Responsibilities

As a SATHI system administrator, you are responsible for:

✅ **User Account Management** - Creating, modifying, and deactivating user accounts  
✅ **System Configuration** - Managing system settings and parameters  
✅ **Data Oversight** - Ensuring data integrity and compliance  
✅ **Security Management** - Maintaining system security and access controls  
✅ **Performance Monitoring** - Tracking system performance and health  
✅ **Backup Operations** - Ensuring data backup and recovery procedures  
✅ **Support Coordination** - Managing technical support and user assistance  

### Key Administrative Functions

**System Management:**
- Configure system-wide settings
- Manage questionnaires and assessments
- Monitor system performance
- Handle security incidents

**User Administration:**
- Create and manage user accounts
- Set user roles and permissions
- Monitor user activity
- Handle access requests

**Data Administration:**
- Generate comprehensive reports
- Manage data retention policies
- Ensure compliance with regulations
- Coordinate data backup operations

## 🔐 System Access & Permissions

### Administrator Account Types

**System Administrator (Full Access):**
- Complete system access and configuration
- User management across all units
- System-wide reporting and analytics
- Security and backup management

**Unit Administrator (Limited Scope):**
- User management within assigned units
- Unit-specific reporting and analytics
- Limited system configuration
- Local support coordination

**Technical Administrator (Infrastructure):**
- System maintenance and updates
- Database administration
- Server configuration
- Network and security management

### Access Management

**Administrator Login Process:**
1. Navigate to: `https://sathi.crpf.gov.in/admin`
2. Enter administrator credentials
3. Complete multi-factor authentication
4. Accept security acknowledgment

**Security Requirements:**
- Strong password policy enforcement
- Multi-factor authentication mandatory
- Regular password changes (90 days)
- Session timeouts (15 minutes)
- Access logging and monitoring

## 👥 User Management

### Creating User Accounts

#### CRPF Personnel Accounts

**Bulk User Creation:**
1. Access User Management → Bulk Import
2. Download user template file
3. Complete required fields:
   ```csv
   force_id,name,unit,rank,email,phone
   CRPF001234,John Doe,Alpha Company,Constable,john.doe@crpf.gov.in,9876543210
   CRPF001235,Jane Smith,Alpha Company,Head Constable,jane.smith@crpf.gov.in,9876543211
   ```
4. Upload completed file
5. Review validation results
6. Confirm account creation

**Individual Account Creation:**
```
User Details Form:
├── Force ID: CRPF######
├── Full Name: [First] [Last]
├── Unit: [Dropdown selection]
├── Rank: [Dropdown selection]
├── Email: [Optional]
├── Phone: [Optional]
├── Role: Personnel (default)
└── Status: Active (default)
```

#### Healthcare Provider Accounts

**Provider Account Setup:**
```
Provider Details:
├── Username: [Custom username]
├── Full Name: [Professional name]
├── Role: [Medical Officer/Counselor/Psychiatrist]
├── License Number: [Professional license]
├── Specialization: [Area of expertise]
├── Assigned Units: [Multi-select]
├── Permissions: [Role-based defaults]
└── Contact Information: [Email/Phone]
```

### User Role Management

**Role Definitions:**

**Personnel (Standard User):**
```
Permissions:
✅ Take mental health assessments
✅ View personal results and history
✅ Access support resources
✅ Update profile information
❌ View other users' data
❌ Administrative functions
```

**Healthcare Provider:**
```
Permissions:
✅ View assigned patients' data
✅ Generate patient reports
✅ Document interventions
✅ Access clinical tools
✅ Manage treatment plans
❌ System configuration
❌ User account management
```

**Unit Administrator:**
```
Permissions:
✅ Manage unit personnel accounts
✅ View unit-wide analytics
✅ Generate unit reports
✅ Configure unit settings
✅ Access aggregate data
❌ Individual patient details (without consent)
❌ System-wide configuration
```

**System Administrator:**
```
Permissions:
✅ Full system access
✅ User management across all units
✅ System configuration
✅ Security management
✅ Data administration
✅ Backup and recovery
```

### Account Lifecycle Management

**Account Activation:**
1. New accounts created in "Pending" status
2. Send activation email with temporary password
3. User completes first-time login setup
4. Account status changes to "Active"
5. Monitor initial system usage

**Account Modification:**
- Role changes require approval workflow
- Unit transfers update data access permissions
- Permission modifications logged for audit
- Email notifications sent for significant changes

**Account Deactivation:**
```
Deactivation Process:
1. Change status to "Inactive"
2. Revoke all system access immediately
3. Archive user data according to policy
4. Update unit rosters and assignments
5. Notify relevant supervisors
6. Document deactivation reason
```

## ⚙️ System Configuration

### Global System Settings

**Core Configuration:**

```yaml
System Settings:
  application:
    max_file_size: "10MB"
    session_timeout: 1800  # 30 minutes
    max_concurrent_users: 500
    maintenance_mode: false
  
  assessment:
    max_question_time: 120  # seconds
    auto_save_interval: 30  # seconds
    require_camera: true
    enable_emotion_detection: true
    anonymous_surveys: true
  
  security:
    password_min_length: 8
    password_complexity: true
    session_security: "high"
    rate_limiting: true
    audit_logging: true
  
  notifications:
    email_enabled: true
    sms_enabled: false
    push_notifications: true
    alert_thresholds:
      critical_risk: "immediate"
      high_risk: "24_hours"
      system_alerts: "immediate"
```

**Assessment Configuration:**

**Default Risk Thresholds:**
```json
{
  "risk_levels": {
    "critical": {"min": 0, "max": 19},
    "high": {"min": 20, "max": 39},
    "medium": {"min": 40, "max": 59},
    "low": {"min": 60, "max": 100}
  },
  "alert_triggers": {
    "critical_risk": "immediate",
    "score_decline": "20_percent",
    "missed_assessments": "3_consecutive"
  }
}
```

### Questionnaire Management

**Creating Custom Questionnaires:**

1. **Basic Information:**
   ```
   Questionnaire Details:
   ├── Title: [Descriptive name]
   ├── Description: [Purpose and scope]
   ├── Category: [Routine/Specialized/Crisis]
   ├── Estimated Time: [Minutes]
   ├── Target Population: [All/Specific units]
   └── Active Status: [Active/Inactive]
   ```

2. **Question Configuration:**
   ```
   Question Types:
   ├── Scale (1-5, 1-10)
   ├── Multiple Choice
   ├── Yes/No
   ├── Short Text
   ├── Rating Scale
   └── Conditional (based on previous answers)
   ```

3. **Scoring Setup:**
   ```
   Scoring Configuration:
   ├── Question Weights: [1-10 scale]
   ├── Category Mapping: [Stress/Sleep/Mood/etc.]
   ├── Risk Calculation: [Weighted average/Custom formula]
   └── Threshold Settings: [Risk level boundaries]
   ```

**Question Bank Management:**
- Maintain library of validated questions
- Version control for question modifications
- Import/export question sets
- Multi-language question support

### Unit-Specific Configuration

**Unit Settings Management:**
```
Unit Configuration:
├── Unit Information:
│   ├── Unit Name
│   ├── Commander Details
│   ├── Location
│   └── Contact Information
├── Assessment Schedule:
│   ├── Routine Assessment Frequency
│   ├── Mandatory Assessment Times
│   ├── Special Event Triggers
│   └── Compliance Requirements
├── Notification Settings:
│   ├── Alert Recipients
│   ├── Escalation Procedures
│   ├── Communication Preferences
│   └── Emergency Contacts
└── Reporting Preferences:
    ├── Report Frequency
    ├── Data Aggregation Level
    ├── Distribution Lists
    └── Format Preferences
```

## 📊 Monitoring & Analytics

### System Health Monitoring

**Real-time Dashboard:**

![Admin System Dashboard](../screenshots/admin-system-dashboard.png)

**Key Metrics Display:**
```
System Status:
├── Server Status: Online/Offline
├── Database Status: Connected/Error
├── Application Health: Healthy/Warning/Critical
├── Active Users: Current session count
├── Resource Usage: CPU/Memory/Disk
└── Response Times: Average API response times

Assessment Metrics:
├── Surveys Completed Today: Count
├── Completion Rate: Percentage
├── Average Completion Time: Minutes
├── Risk Distribution: Low/Medium/High/Critical counts
└── System Alerts: Active alert count
```

**Performance Monitoring:**
- API response time tracking
- Database query performance
- User session analytics
- Error rate monitoring
- Resource utilization trends

### Analytics & Reporting

**Unit Analytics Dashboard:**

```
Unit Performance Metrics:
├── Participation Rates:
│   ├── Overall completion rate
│   ├── On-time completion rate
│   └── Voluntary assessment rate
├── Mental Health Trends:
│   ├── Risk level distribution over time
│   ├── Category-specific trends (stress, sleep, etc.)
│   └── Seasonal patterns
├── Intervention Effectiveness:
│   ├── Risk level improvements post-intervention
│   ├── Treatment completion rates
│   └── Outcome measurements
└── Resource Utilization:
    ├── Counseling session usage
    ├── Support resource access
    └── System feature adoption
```

**Advanced Analytics:**

**Predictive Analytics:**
- Risk escalation prediction models
- Optimal intervention timing
- Resource demand forecasting
- Trend analysis and projections

**Population Health Analytics:**
- Cross-unit comparisons
- Demographic-based analysis
- Environmental factor correlations
- Operational stress indicators

### Custom Report Generation

**Report Builder Interface:**
```
Report Configuration:
├── Report Type:
│   ├── Individual Summary
│   ├── Unit Overview
│   ├── Trend Analysis
│   ├── Intervention Outcomes
│   └── Compliance Report
├── Data Filters:
│   ├── Date Range
│   ├── Units/Personnel
│   ├── Risk Levels
│   ├── Assessment Types
│   └── Intervention Categories
├── Output Format:
│   ├── PDF (formatted report)
│   ├── Excel (data analysis)
│   ├── CSV (raw data)
│   └── PowerPoint (presentation)
└── Distribution:
    ├── Email Recipients
    ├── Automated Schedule
    └── Access Permissions
```

**Scheduled Reporting:**
- Daily operational reports
- Weekly unit summaries
- Monthly trend analysis
- Quarterly outcome reviews
- Annual comprehensive reports

## 💾 Data Management

### Data Retention Policies

**Retention Schedule:**
```
Data Type | Retention Period | Archive Location
----------|------------------|------------------
Assessment Data | 7 years | Secure archive
User Activity Logs | 2 years | System logs
Audit Trails | 10 years | Compliance archive
System Backups | 1 year | Backup storage
Error Logs | 6 months | System logs
Performance Metrics | 3 years | Analytics database
```

**Data Lifecycle Management:**
1. **Active Data** (0-2 years): Full system access
2. **Archived Data** (2-7 years): Read-only access, compliance retrieval
3. **Long-term Archive** (7+ years): Compliance-only access
4. **Data Destruction**: Secure deletion after retention period

### Data Export & Import

**Data Export Capabilities:**
```
Export Options:
├── Individual Records:
│   ├── Complete user profile
│   ├── Assessment history
│   ├── Intervention records
│   └── Progress summaries
├── Unit Data:
│   ├── Unit roster exports
│   ├── Aggregate statistics
│   ├── Trend analysis data
│   └── Compliance reports
├── System Data:
│   ├── Configuration backups
│   ├── User management data
│   ├── Audit trail exports
│   └── Performance metrics
└── Compliance Exports:
    ├── Regulatory reporting
    ├── Audit preparation
    ├── Legal discovery
    └── Research datasets (anonymized)
```

**Data Import Procedures:**
- Bulk user imports from HR systems
- Assessment data migration
- Configuration restoration
- Legacy system data conversion

### Data Quality Management

**Data Validation Rules:**
```
Validation Checks:
├── Required Field Validation
├── Format Validation (Force ID, email, phone)
├── Range Validation (scores, dates)
├── Consistency Checks (cross-field validation)
├── Duplicate Detection
└── Completeness Verification
```

**Data Cleansing Procedures:**
- Automated data quality checks
- Duplicate record identification
- Inconsistency resolution
- Data standardization
- Regular data quality reports

## 🔒 Security Administration

### Access Control Management

**Role-Based Access Control (RBAC):**
```
Permission Matrix:
                    | Personnel | Provider | Unit Admin | Sys Admin
--------------------|-----------|----------|------------|----------
View Own Data       |     ✅     |    ✅     |     ✅      |    ✅
View Unit Data      |     ❌     |    ✅     |     ✅      |    ✅
Manage Users        |     ❌     |    ❌     |     ✅      |    ✅
System Config       |     ❌     |    ❌     |     ❌      |    ✅
Security Settings   |     ❌     |    ❌     |     ❌      |    ✅
```

**Security Monitoring:**
- Failed login attempt tracking
- Unusual access pattern detection
- Privileged action logging
- Session security monitoring
- Data access auditing

### Compliance Management

**Regulatory Compliance:**

**HIPAA Compliance:**
- Patient data protection
- Access control implementation
- Audit trail maintenance
- Breach notification procedures
- Business associate agreements

**Data Protection:**
- Encryption at rest and in transit
- Secure data transmission
- Access logging and monitoring
- Data minimization principles
- Privacy by design implementation

**Audit Requirements:**
```
Audit Trail Components:
├── User Authentication Events
├── Data Access Records
├── Configuration Changes
├── Administrative Actions
├── Security Incidents
├── System Modifications
└── Data Export/Import Activities
```

### Incident Response

**Security Incident Procedures:**
1. **Detection**: Automated monitoring and manual reporting
2. **Assessment**: Incident severity and impact evaluation
3. **Containment**: Immediate threat mitigation
4. **Investigation**: Root cause analysis
5. **Recovery**: System restoration and security enhancement
6. **Documentation**: Incident report and lessons learned

**Incident Classifications:**
- **Critical**: Data breach, system compromise
- **High**: Unauthorized access, service disruption
- **Medium**: Policy violations, configuration errors
- **Low**: Failed login attempts, minor violations

## 💽 Backup & Recovery

### Backup Strategy

**Backup Schedule:**
```
Backup Type | Frequency | Retention | Location
------------|-----------|-----------|----------
Full System | Weekly | 6 months | Offsite storage
Incremental | Daily | 30 days | Local storage
Database | Every 6 hours | 90 days | Cloud backup
Configuration | After changes | 1 year | Version control
User Data | Daily | 7 years | Encrypted archive
```

**Backup Verification:**
- Automated backup integrity checks
- Regular restore testing
- Recovery time objective (RTO): 4 hours
- Recovery point objective (RPO): 1 hour
- Monthly disaster recovery drills

### Disaster Recovery

**Recovery Procedures:**

**System Failure Recovery:**
1. Assess system failure scope and impact
2. Activate backup systems and data recovery
3. Restore services in priority order
4. Verify data integrity and system functionality
5. Resume normal operations with monitoring
6. Conduct post-incident review

**Data Recovery:**
- Point-in-time data restoration
- Selective data recovery options
- Cross-platform compatibility
- Automated recovery verification
- User notification procedures

## 🔧 Troubleshooting

### Common Administrative Issues

**User Account Issues:**
```
Issue: User Cannot Login
Troubleshooting Steps:
1. Verify account status (Active/Inactive/Locked)
2. Check password expiration
3. Review recent security events
4. Validate user permissions
5. Test with temporary password reset
6. Check system-wide authentication issues
```

**Performance Issues:**
```
Issue: Slow System Response
Troubleshooting Steps:
1. Check system resource utilization
2. Review database performance metrics
3. Analyze network connectivity
4. Identify high-usage periods
5. Review recent configuration changes
6. Check for concurrent user load
```

**Data Issues:**
```
Issue: Missing or Incorrect Data
Troubleshooting Steps:
1. Check data validation logs
2. Review import/export processes
3. Verify backup integrity
4. Analyze user input patterns
5. Check system integration points
6. Review data retention policies
```

### System Maintenance

**Regular Maintenance Tasks:**
```
Daily Tasks:
├── Monitor system health dashboards
├── Review security alerts
├── Check backup completion status
├── Analyze user activity reports
└── Address critical support tickets

Weekly Tasks:
├── Performance optimization review
├── Security patch assessment
├── User account audit
├── Data quality reports
└── Capacity planning analysis

Monthly Tasks:
├── Comprehensive security review
├── Disaster recovery testing
├── System configuration backup
├── Compliance audit preparation
└── User feedback analysis
```

## 🎯 Best Practices

### Administrative Excellence

**User Management Best Practices:**
1. **Principle of Least Privilege**: Grant minimum necessary permissions
2. **Regular Access Reviews**: Quarterly permission audits
3. **Prompt Deactivation**: Immediate access removal for departing users
4. **Role-Based Assignments**: Use standardized role templates
5. **Documentation**: Maintain detailed user management records

**Security Best Practices:**
1. **Multi-Factor Authentication**: Mandatory for all administrative accounts
2. **Regular Security Training**: Keep administrators updated on threats
3. **Incident Response**: Maintain updated response procedures
4. **Monitoring**: Continuous security event monitoring
5. **Updates**: Timely security patch application

**Data Management Best Practices:**
1. **Regular Backups**: Automated and verified backup procedures
2. **Data Quality**: Continuous data validation and cleansing
3. **Retention Compliance**: Strict adherence to retention policies
4. **Access Control**: Fine-grained data access permissions
5. **Audit Trails**: Comprehensive activity logging

### Performance Optimization

**System Performance:**
- Regular performance monitoring and tuning
- Proactive capacity planning
- Database optimization procedures
- Caching strategy implementation
- Load balancing configuration

**User Experience:**
- Response time monitoring
- User feedback collection
- Interface optimization
- Training and support programs
- Continuous improvement processes

---

**Remember**: As a SATHI administrator, you are the guardian of sensitive mental health data for CRPF personnel. Your diligent administration ensures the system operates securely, efficiently, and in compliance with all applicable regulations while supporting the mental health and wellbeing of our personnel.

For additional administrative support or escalation procedures, contact the SATHI technical team or your designated system administrator supervisor.