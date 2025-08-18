# Deployment Guide
*Part of SATHI Documentation Suite*

**📚 Quick Navigation:**
[🏠 README](README.md) | [🤝 Contributing](CONTRIBUTING.md) | [🔌 API](docs/API_Documentation.md) | [👥 Users](docs/USER_GUIDE.md) | [🚀 Deploy](DEPLOYMENT_GUIDE.md) | [🔧 Troubleshoot](TROUBLESHOOTING.md)

---

## Table of Contents

1. [Overview](#overview)
2. [Local Development Setup](#local-development-setup)
3. [Production Environment Requirements](#production-environment-requirements)
4. [Standard Web Deployment](#standard-web-deployment)
5. [Docker Containerization](#docker-containerization)
6. [Cloud Deployment](#cloud-deployment)
7. [Windows Executable Deployment](#windows-executable-deployment)
8. [Database Setup and Configuration](#database-setup-and-configuration)
9. [Environment Configuration](#environment-configuration)
10. [Security Configuration](#security-configuration)
11. [Performance Optimization](#performance-optimization)
12. [Backup and Recovery](#backup-and-recovery)
13. [Mobile Configuration](#mobile-configuration)

## Overview

This comprehensive deployment guide covers all aspects of deploying the SATHI mental health monitoring system across different environments, from local development to production-scale enterprise deployments.

### 🎯 Deployment Scenarios

**Development Deployments:**
- Local development environment
- Testing and staging environments
- Developer workstations

**Production Deployments:**
- Enterprise server deployment
- Cloud-based deployment (AWS, Azure, GCP)
- Hybrid cloud deployment
- High-availability clusters

**Specialized Deployments:**
- Windows executable for desktop deployment
- Mobile-optimized deployment
- Offline/air-gapped environments
- Edge computing deployments

💡 **Related**: For detailed development environment setup, see [Contributing Guide - Development Setup](CONTRIBUTING.md#development-environment-setup)

## Local Development Setup

### 🚀 Quick Start (5 minutes)

```bash
# 1. Clone repository
git clone https://github.com/JayeshCC/SATHI.git
cd SATHI

# 2. Backend setup
cd backend
python -m venv venv

# Activate virtual environment
source venv/bin/activate  # Linux/Mac
# OR
venv\Scripts\activate     # Windows

pip install -r requirements.txt

# 3. Database setup
mysql -u root -p
CREATE DATABASE crpf_mental_health;
mysql -u root -p crpf_mental_health < db/schema.sql

# 4. Environment configuration
cp .env.example .env
# Edit .env with your database credentials

# 5. Start backend
python app.py

# 6. Frontend setup (new terminal)
cd frontend
npm install
npm start
```

## Production Environment Requirements

### 🖥️ Hardware Requirements

**Minimum Requirements (Up to 100 users):**
- **CPU**: 4 cores, 2.5 GHz
- **Memory**: 8 GB RAM
- **Storage**: 100 GB SSD
- **Network**: 100 Mbps bandwidth

**Recommended Requirements (100-500 users):**
- **CPU**: 8 cores, 3.0 GHz
- **Memory**: 16 GB RAM
- **Storage**: 500 GB SSD
- **Network**: 1 Gbps bandwidth

**Enterprise Requirements (500+ users):**
- **CPU**: 16+ cores, 3.5 GHz
- **Memory**: 32+ GB RAM
- **Storage**: 1+ TB NVMe SSD
- **Network**: 10 Gbps bandwidth
- **Redundancy**: Multiple servers, load balancing

### 🐧 Operating System Requirements

**Supported Operating Systems:**
- **Ubuntu 20.04 LTS** (recommended)
- **CentOS 8** or **Rocky Linux 8**
- **Red Hat Enterprise Linux 8**
- **Debian 11**
- **Windows Server 2019/2022** (limited support)

**Required Services:**
- **Web Server**: Nginx 1.18+ or Apache 2.4+
- **Application Server**: Gunicorn, uWSGI, or mod_wsgi
- **Database**: MySQL 8.0+ or PostgreSQL 12+
- **Cache**: Redis 6.0+ (optional but recommended)
- **Process Manager**: systemd, PM2, or Supervisor

## Standard Web Deployment

### 🔧 Backend Deployment (Flask)

#### Production WSGI Setup

**Using Gunicorn (Recommended):**
```bash
# Install Gunicorn
pip install gunicorn

# Create Gunicorn configuration
cat > gunicorn.conf.py << EOG
bind = "127.0.0.1:5000"
workers = 4
worker_class = "gevent"
worker_connections = 1000
max_requests = 1000
max_requests_jitter = 50
timeout = 30
keepalive = 2
preload_app = True
EOG

# Start Gunicorn
gunicorn --config gunicorn.conf.py app:app
```

#### Systemd Service Configuration

```bash
# Create systemd service file
sudo tee /etc/systemd/system/sathi-backend.service << EOS
[Unit]
Description=SATHI Backend Application
After=network.target

[Service]
User=sathi
Group=sathi
WorkingDirectory=/opt/sathi/backend
Environment=PATH=/opt/sathi/backend/venv/bin
ExecStart=/opt/sathi/backend/venv/bin/gunicorn --config gunicorn.conf.py app:app
ExecReload=/bin/kill -s HUP \$MAINPID
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
EOS

# Enable and start service
sudo systemctl daemon-reload
sudo systemctl enable sathi-backend
sudo systemctl start sathi-backend
sudo systemctl status sathi-backend
```

### 🌐 Frontend Deployment (React)

#### Production Build

```bash
cd frontend

# Install dependencies
npm ci --only=production

# Create production build
npm run build

# Optimize build (optional)
npm install -g serve
serve -s build -l 3000
```

💡 **Related**: For security configuration details, see [Contributing Guide - Security Guidelines](CONTRIBUTING.md#security-guidelines)

## Docker Containerization

### 🐳 Docker Setup

#### Backend Dockerfile

```dockerfile
# backend/Dockerfile
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    gcc \
    default-libmysqlclient-dev \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Copy requirements and install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Create non-root user
RUN useradd --create-home --shell /bin/bash sathi
RUN chown -R sathi:sathi /app
USER sathi

# Expose port
EXPOSE 5000

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:5000/api/health || exit 1

# Start application
CMD ["gunicorn", "--config", "gunicorn.conf.py", "app:app"]
```

#### Docker Compose Configuration

```yaml
# docker-compose.yml
version: '3.8'

services:
  database:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: \${DB_ROOT_PASSWORD}
      MYSQL_DATABASE: \${DB_NAME}
      MYSQL_USER: \${DB_USER}
      MYSQL_PASSWORD: \${DB_PASSWORD}
    volumes:
      - mysql_data:/var/lib/mysql
      - ./backend/db:/docker-entrypoint-initdb.d
    ports:
      - "3306:3306"
    restart: unless-stopped

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    environment:
      - DATABASE_URL=mysql://\${DB_USER}:\${DB_PASSWORD}@database:3306/\${DB_NAME}
      - FLASK_ENV=production
    ports:
      - "5000:5000"
    depends_on:
      - database
    restart: unless-stopped

volumes:
  mysql_data:
```

## Cloud Deployment

### ☁️ AWS Deployment

#### EC2 Instance Setup

```bash
# Launch EC2 instance
aws ec2 run-instances \
    --image-id ami-0c55b159cbfafe1d0 \
    --count 1 \
    --instance-type t3.medium \
    --key-name your-key-pair \
    --security-group-ids sg-xxxxxxxxx \
    --subnet-id subnet-xxxxxxxxx
```

## Windows Executable Deployment

### 🖥️ Standalone Desktop Application

#### Backend as Executable

```bash
# Install PyInstaller
pip install pyinstaller

# Create executable
pyinstaller --onefile \
    --add-data "db;db" \
    --add-data "models;models" \
    --hidden-import=sklearn \
    --hidden-import=cv2 \
    --name SATHI_Backend \
    app.py
```

## Database Setup and Configuration

### 🗄️ Production Database Configuration

#### MySQL Optimization

```sql
-- MySQL production configuration
[mysqld]
# Basic settings
user = mysql
port = 3306
basedir = /usr
datadir = /var/lib/mysql

# Connection settings
max_connections = 200
connect_timeout = 60
wait_timeout = 600
max_allowed_packet = 64M

# InnoDB settings
innodb_buffer_pool_size = 1G
innodb_log_file_size = 256M
innodb_log_buffer_size = 8M

# Security settings
bind-address = 127.0.0.1
local-infile = 0
```

#### Database Security

```sql
-- Create dedicated database user
CREATE USER 'sathi_app'@'localhost' IDENTIFIED BY 'strong_password_here';
GRANT SELECT, INSERT, UPDATE, DELETE ON crpf_mental_health.* TO 'sathi_app'@'localhost';

-- Create read-only user for reporting
CREATE USER 'sathi_readonly'@'localhost' IDENTIFIED BY 'readonly_password';
GRANT SELECT ON crpf_mental_health.* TO 'sathi_readonly'@'localhost';

-- Remove test databases and users
DROP DATABASE IF EXISTS test;
DELETE FROM mysql.user WHERE User='';
FLUSH PRIVILEGES;
```

## Environment Configuration

### 🔧 Environment Variables

#### Backend Environment (.env)

```bash
# Database Configuration
DATABASE_URL=mysql://sathi_app:password@localhost:3306/crpf_mental_health
DB_POOL_SIZE=10
DB_POOL_TIMEOUT=30

# Flask Configuration
FLASK_ENV=production
FLASK_DEBUG=False
SECRET_KEY=your-super-secret-key-here-make-it-very-long-and-random
JWT_SECRET_KEY=another-secret-key-for-jwt-tokens

# Security Configuration
BCRYPT_LOG_ROUNDS=12
JWT_ACCESS_TOKEN_EXPIRES=3600
SESSION_TIMEOUT=3600

# File Upload Configuration
UPLOAD_FOLDER=/opt/sathi/uploads
MAX_CONTENT_LENGTH=16777216
ALLOWED_EXTENSIONS=jpg,jpeg,png,gif

# AI/ML Configuration
NLP_MODEL_PATH=/opt/sathi/models/nlp_model.pkl
EMOTION_MODEL_PATH=/opt/sathi/models/emotion_model.h5
AI_CONFIDENCE_THRESHOLD=0.7

# Logging Configuration
LOG_LEVEL=INFO
LOG_FILE=/var/log/sathi/application.log
LOG_MAX_SIZE=10485760
LOG_BACKUP_COUNT=5
```

## Security Configuration

### 🔐 SSL/TLS Setup

#### Let's Encrypt Certificate

```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Obtain certificate
sudo certbot --nginx -d your-domain.com -d www.your-domain.com

# Auto-renewal
sudo crontab -e
# Add: 0 12 * * * /usr/bin/certbot renew --quiet
```

### 🛡️ Firewall Configuration

```bash
# UFW (Ubuntu Firewall) setup
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow SSH
sudo ufw allow 22/tcp

# Allow HTTP and HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

# Enable firewall
sudo ufw enable
```

## Performance Optimization

### ⚡ Database Performance

```sql
-- Database indexing for performance
CREATE INDEX idx_soldiers_force_id ON soldiers(force_id);
CREATE INDEX idx_survey_responses_soldier_id ON survey_responses(soldier_id);
CREATE INDEX idx_survey_responses_created_at ON survey_responses(created_at);
CREATE INDEX idx_analysis_results_soldier_id ON analysis_results(soldier_id);
CREATE INDEX idx_analysis_results_risk_level ON analysis_results(risk_level);

-- Composite indexes for common queries
CREATE INDEX idx_soldiers_unit_status ON soldiers(unit, status);
CREATE INDEX idx_survey_responses_date_risk ON survey_responses(created_at, risk_level);
```

## Backup and Recovery

### 💾 Automated Backup Strategy

#### Database Backup Script

```bash
#!/bin/bash
# /opt/sathi/scripts/backup.sh

# Configuration
BACKUP_DIR="/opt/sathi/backups"
DB_NAME="crpf_mental_health"
DB_USER="sathi_app"
DB_PASSWORD="secure_password"
RETENTION_DAYS=30
DATE=\$(date +%Y%m%d_%H%M%S)

# Create backup directory
mkdir -p \$BACKUP_DIR

# Database backup
mysqldump -u \$DB_USER -p\$DB_PASSWORD \
    --single-transaction \
    --routines \
    --triggers \
    --events \
    \$DB_NAME | gzip > \$BACKUP_DIR/db_backup_\$DATE.sql.gz

# Clean old backups
find \$BACKUP_DIR -name "*backup_*.gz" -mtime +\$RETENTION_DAYS -delete

# Log backup completion
echo "\$(date): Backup completed - \$DATE" >> /var/log/sathi_backup.log
```

## Mobile Configuration

### 📱 Progressive Web App Setup

```json
// frontend/public/manifest.json
{
  "name": "SATHI Mental Health Monitor",
  "short_name": "SATHI",
  "description": "CRPF Mental Health Monitoring System",
  "start_url": "/",
  "display": "standalone",
  "theme_color": "#1f2937",
  "background_color": "#ffffff",
  "icons": [
    {
      "src": "icons/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "icons/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

---

## 🔗 Related Documentation

🤝 **For Development**: See [Contributing Guide](CONTRIBUTING.md) for development setup and guidelines

🔌 **For API Integration**: See [API Documentation](docs/API_Documentation.md) for endpoint reference

👥 **For Users**: See [User Guide](docs/USER_GUIDE.md) for user-facing documentation

🔧 **For Issues**: See [Troubleshooting Guide](TROUBLESHOOTING.md) for deployment problem resolution

---

This comprehensive deployment guide provides all the essential information needed to deploy SATHI across various environments. Each deployment scenario is optimized for healthcare-grade reliability, security, and performance required for mental health monitoring systems.
