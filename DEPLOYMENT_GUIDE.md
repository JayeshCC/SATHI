# SATHI Deployment Guide

## 🎯 Overview

This guide provides comprehensive instructions for deploying SATHI (System for Analyzing and Tracking Human Intelligence) in production environments.

## 📋 Pre-Deployment Requirements

### System Requirements

#### Minimum Hardware Requirements
- **CPU**: 4 cores, 2.4 GHz
- **RAM**: 8 GB
- **Storage**: 100 GB SSD
- **Network**: 100 Mbps bandwidth

#### Recommended Hardware Requirements
- **CPU**: 8 cores, 3.0 GHz
- **RAM**: 16 GB
- **Storage**: 500 GB SSD
- **Network**: 1 Gbps bandwidth
- **GPU**: NVIDIA GTX 1060 or better (optional, for faster emotion detection)

#### Operating System
- **Ubuntu 20.04 LTS** (recommended)
- **CentOS 8**
- **Windows Server 2019** (alternative)

### Software Dependencies

- **Python**: 3.8 or higher
- **Node.js**: 16.x LTS or higher
- **MySQL**: 8.0 or higher
- **Nginx**: 1.18 or higher (web server/reverse proxy)
- **PM2**: Process manager for Node.js applications
- **Supervisor**: Process manager for Python applications

## 🔧 Server Setup and Security

### Initial Server Configuration

```bash
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install essential packages
sudo apt install -y curl wget git build-essential software-properties-common

# Configure firewall
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow 80
sudo ufw allow 443
sudo ufw allow 5000  # Backend API (temporary, will be proxied)
```

### Create Application User

```bash
# Create dedicated user for the application
sudo adduser crpf_monitor
sudo usermod -aG sudo crpf_monitor
sudo su - crpf_monitor
```

### SSL Certificate Setup

```bash
# Install Certbot for Let's Encrypt SSL
sudo apt install certbot python3-certbot-nginx

# Generate SSL certificate (replace your-domain.com)
sudo certbot --nginx -d your-domain.com

# Auto-renewal setup
sudo crontab -e
# Add: 0 12 * * * /usr/bin/certbot renew --quiet
```

## 🗄️ Database Setup

### Install and Configure MySQL

```bash
# Install MySQL Server
sudo apt install mysql-server

# Secure MySQL installation
sudo mysql_secure_installation

# Create production database and user
sudo mysql -u root -p
```

```sql
-- Create production database
CREATE DATABASE crpf_mental_health_prod CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- Create dedicated database user
CREATE USER 'crpf_prod_user'@'localhost' IDENTIFIED BY 'STRONG_SECURE_PASSWORD';
GRANT ALL PRIVILEGES ON crpf_mental_health_prod.* TO 'crpf_prod_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### Configure MySQL for Production

```bash
# Edit MySQL configuration
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Add these optimizations:

```ini
[mysqld]
# Performance tuning
innodb_buffer_pool_size = 2G
innodb_log_file_size = 256M
max_connections = 200
query_cache_type = 1
query_cache_size = 128M

# Security
bind-address = 127.0.0.1
local-infile = 0
```

```bash
# Restart MySQL
sudo systemctl restart mysql
```

## 🐍 Backend Deployment

### Clone and Setup Application

```bash
# Clone repository to production location
cd /opt
sudo git clone https://github.com/JayeshCC/SATHI.git crpf_monitor
sudo chown -R crpf_monitor:crpf_monitor /opt/crpf_monitor
cd /opt/crpf_monitor

# Setup Python virtual environment
cd backend
python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```

### Production Environment Configuration

```bash
# Create production environment file
cp .env.example .env.prod
nano .env.prod
```

Configure production environment:

```bash
# Database Configuration
DB_NAME=crpf_mental_health_prod
DB_USER=crpf_prod_user
DB_PASSWORD=STRONG_SECURE_PASSWORD
DB_HOST=localhost
DB_PORT=3306

# Server Configuration
BACKEND_PORT=5000
FRONTEND_URL=https://your-domain.com
DEBUG_MODE=false

# Security Configuration
SECRET_KEY=GENERATE_STRONG_SECRET_KEY_HERE
SESSION_TIMEOUT=1800
MAX_LOGIN_ATTEMPTS=3
PASSWORD_MIN_LENGTH=8

# Mental Health Scoring Configuration
NLP_WEIGHT=0.7
EMOTION_WEIGHT=0.3

# Risk Level Thresholds
RISK_LOW_THRESHOLD=0.3
RISK_MEDIUM_THRESHOLD=0.5
RISK_HIGH_THRESHOLD=0.7
RISK_CRITICAL_THRESHOLD=0.85

# Camera and Emotion Detection Configuration
CAMERA_WIDTH=640
CAMERA_HEIGHT=480
DETECTION_INTERVAL=30

# Performance Configuration
PAGE_SIZE=20
MAX_PAGE_SIZE=100

# Notification Configuration
EMAIL_ENABLED=true

# Logging Configuration
LOG_LEVEL=INFO
LOG_FILE_PATH=/opt/crpf_monitor/logs/app.log
```

### Initialize Production Database

```bash
# Load database schema
mysql -u crpf_prod_user -p crpf_mental_health_prod < db/schema.sql

# Run any migration scripts
python db/init_db.py
```

### Setup Process Manager for Backend

```bash
# Install Supervisor
sudo apt install supervisor

# Create Supervisor configuration
sudo nano /etc/supervisor/conf.d/crpf_backend.conf
```

```ini
[program:crpf_backend]
command=/opt/crpf_monitor/backend/venv/bin/python app.py
directory=/opt/crpf_monitor/backend
user=crpf_monitor
environment=FLASK_ENV=production
autostart=true
autorestart=true
redirect_stderr=true
stdout_logfile=/opt/crpf_monitor/logs/backend.log
stderr_logfile=/opt/crpf_monitor/logs/backend_error.log
```

```bash
# Update Supervisor and start service
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start crpf_backend
```

## 🌐 Frontend Deployment

### Build Frontend for Production

```bash
cd /opt/crpf_monitor/frontend

# Install dependencies
npm install

# Build for production
npm run build

# Create production directory
sudo mkdir -p /var/www/crpf_monitor
sudo cp -r build/* /var/www/crpf_monitor/
sudo chown -R www-data:www-data /var/www/crpf_monitor
```

### Configure Nginx

```bash
# Create Nginx configuration
sudo nano /etc/nginx/sites-available/crpf_monitor
```

```nginx
server {
    listen 80;
    server_name your-domain.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name your-domain.com;

    # SSL Configuration
    ssl_certificate /etc/letsencrypt/live/your-domain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/your-domain.com/privkey.pem;
    
    # Security headers
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Frame-Options DENY always;
    add_header X-Content-Type-Options nosniff always;
    add_header X-XSS-Protection "1; mode=block" always;

    # Serve static files
    location / {
        root /var/www/crpf_monitor;
        try_files $uri $uri/ /index.html;
        
        # Cache static assets
        location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
            expires 1y;
            add_header Cache-Control "public, immutable";
        }
    }

    # Proxy API requests to backend
    location /api/ {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # Timeouts
        proxy_connect_timeout 30s;
        proxy_send_timeout 30s;
        proxy_read_timeout 30s;
    }

    # Logging
    access_log /var/log/nginx/crpf_monitor_access.log;
    error_log /var/log/nginx/crpf_monitor_error.log;
}
```

```bash
# Enable site and restart Nginx
sudo ln -s /etc/nginx/sites-available/crpf_monitor /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

## 🐳 Docker Deployment (Alternative)

### Docker Compose Configuration

Create `docker-compose.prod.yml`:

```yaml
version: "3.8"

services:
  backend:
    build: 
      context: ./backend
      dockerfile: Dockerfile.prod
    environment:
      - DB_HOST=db
      - DB_NAME=crpf_mental_health
      - DB_USER=crpf_user
      - DB_PASSWORD=secure_password
      - DEBUG_MODE=false
    depends_on:
      - db
    volumes:
      - ./backend/storage:/app/storage
      - ./logs:/app/logs
    restart: unless-stopped

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile.prod
    ports:
      - "80:80"
      - "443:443"
    depends_on:
      - backend
    volumes:
      - ./ssl:/etc/nginx/ssl
    restart: unless-stopped

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secure_root_password
      MYSQL_DATABASE: crpf_mental_health
      MYSQL_USER: crpf_user
      MYSQL_PASSWORD: secure_password
    volumes:
      - db_data:/var/lib/mysql
      - ./backend/db/schema.sql:/docker-entrypoint-initdb.d/schema.sql
    restart: unless-stopped

  redis:
    image: redis:6-alpine
    restart: unless-stopped

volumes:
  db_data:
```

### Deploy with Docker

```bash
# Build and start services
docker-compose -f docker-compose.prod.yml up -d

# Monitor logs
docker-compose -f docker-compose.prod.yml logs -f
```

## ☁️ Cloud Deployment

### AWS Deployment

#### Using AWS ECS

```yaml
# ecs-task-definition.json
{
  "family": "crpf-monitor",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "1024",
  "memory": "2048",
  "executionRoleArn": "arn:aws:iam::account:role/ecsTaskExecutionRole",
  "containerDefinitions": [
    {
      "name": "backend",
      "image": "your-account.dkr.ecr.region.amazonaws.com/crpf-backend:latest",
      "portMappings": [
        {
          "containerPort": 5000,
          "protocol": "tcp"
        }
      ],
      "environment": [
        {
          "name": "DB_HOST",
          "value": "crpf-db.cluster-xyz.region.rds.amazonaws.com"
        }
      ],
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/crpf-monitor",
          "awslogs-region": "us-west-2",
          "awslogs-stream-prefix": "ecs"
        }
      }
    }
  ]
}
```

#### Using AWS RDS for Database

```bash
# Create RDS instance
aws rds create-db-instance \
    --db-instance-identifier crpf-monitor-db \
    --db-instance-class db.t3.medium \
    --engine mysql \
    --master-username admin \
    --master-user-password SecurePassword123 \
    --allocated-storage 100 \
    --vpc-security-group-ids sg-xxxxxx \
    --backup-retention-period 7 \
    --multi-az
```

### Azure Deployment

#### Using Azure Container Instances

```yaml
# azure-deploy.yml
apiVersion: 2018-10-01
location: eastus
name: crpf-monitor
properties:
  containers:
  - name: backend
    properties:
      image: your-registry.azurecr.io/crpf-backend:latest
      ports:
      - port: 5000
      environmentVariables:
      - name: DB_HOST
        value: crpf-db.mysql.database.azure.com
      resources:
        requests:
          cpu: 1
          memoryInGb: 2
  - name: frontend
    properties:
      image: your-registry.azurecr.io/crpf-frontend:latest
      ports:
      - port: 80
      resources:
        requests:
          cpu: 0.5
          memoryInGb: 1
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: 80
    - protocol: tcp
      port: 5000
```

## 📊 Monitoring and Logging

### Application Monitoring

#### Health Check Endpoint

```bash
# Test application health
curl -f https://your-domain.com/api/monitor/health
```

#### Log Monitoring

```bash
# Backend logs
tail -f /opt/crpf_monitor/logs/backend.log

# Nginx logs
tail -f /var/log/nginx/crpf_monitor_access.log
tail -f /var/log/nginx/crpf_monitor_error.log

# System logs
journalctl -u supervisor -f
```

### Performance Monitoring

#### Using Prometheus and Grafana

```yaml
# monitoring/docker-compose.yml
version: "3.8"
services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana
    ports:
      - "3001:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    volumes:
      - grafana_data:/var/lib/grafana

volumes:
  grafana_data:
```

## 🔒 Security Configuration

### SSL/TLS Configuration

```nginx
# Strong SSL configuration
ssl_protocols TLSv1.2 TLSv1.3;
ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384;
ssl_prefer_server_ciphers off;
ssl_session_cache shared:SSL:10m;
```

### Firewall Configuration

```bash
# Configure UFW
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

### Regular Security Updates

```bash
# Create update script
nano /opt/crpf_monitor/scripts/security_update.sh
```

```bash
#!/bin/bash
# Automated security updates
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y

# Update SSL certificates
sudo certbot renew --quiet

# Restart services if needed
sudo systemctl reload nginx
sudo supervisorctl restart crpf_backend
```

```bash
# Make executable and schedule
chmod +x /opt/crpf_monitor/scripts/security_update.sh
# Add to crontab: 0 2 * * 0 /opt/crpf_monitor/scripts/security_update.sh
```

## 🔄 Backup and Recovery

### Database Backup

```bash
# Create backup script
nano /opt/crpf_monitor/scripts/backup_db.sh
```

```bash
#!/bin/bash
BACKUP_DIR="/opt/crpf_monitor/backups"
DATE=$(date +%Y%m%d_%H%M%S)
DB_NAME="crpf_mental_health_prod"

# Create backup directory
mkdir -p $BACKUP_DIR

# Backup database
mysqldump -u crpf_prod_user -p$DB_PASSWORD $DB_NAME > $BACKUP_DIR/backup_$DATE.sql

# Compress backup
gzip $BACKUP_DIR/backup_$DATE.sql

# Remove backups older than 30 days
find $BACKUP_DIR -name "backup_*.sql.gz" -mtime +30 -delete

echo "Backup completed: backup_$DATE.sql.gz"
```

### Application Backup

```bash
# Full application backup
nano /opt/crpf_monitor/scripts/backup_app.sh
```

```bash
#!/bin/bash
BACKUP_DIR="/opt/crpf_monitor/backups"
DATE=$(date +%Y%m%d_%H%M%S)

# Backup application files
tar -czf $BACKUP_DIR/app_backup_$DATE.tar.gz \
    --exclude='*.log' \
    --exclude='__pycache__' \
    --exclude='node_modules' \
    /opt/crpf_monitor/

# Backup configuration files
tar -czf $BACKUP_DIR/config_backup_$DATE.tar.gz \
    /etc/nginx/sites-available/crpf_monitor \
    /etc/supervisor/conf.d/crpf_backend.conf \
    /opt/crpf_monitor/backend/.env.prod
```

### Recovery Procedures

```bash
# Database recovery
gunzip < /opt/crpf_monitor/backups/backup_YYYYMMDD_HHMMSS.sql.gz | \
mysql -u crpf_prod_user -p crpf_mental_health_prod

# Application recovery
tar -xzf /opt/crpf_monitor/backups/app_backup_YYYYMMDD_HHMMSS.tar.gz -C /
sudo supervisorctl restart crpf_backend
sudo systemctl reload nginx
```

## 🚀 Performance Optimization

### Database Optimization

```sql
-- Add indexes for better performance
CREATE INDEX idx_weekly_sessions_force_timestamp ON weekly_sessions(force_id, completion_timestamp DESC);
CREATE INDEX idx_users_type_force ON users(user_type, force_id);
CREATE INDEX idx_weekly_sessions_completion ON weekly_sessions(completion_timestamp);
```

### Application Optimization

```bash
# Configure Python for production
export PYTHONOPTIMIZE=1
export PYTHONUNBUFFERED=1

# Use production WSGI server
pip install gunicorn
```

Create Gunicorn configuration:

```python
# gunicorn.conf.py
bind = "127.0.0.1:5000"
workers = 4
worker_class = "sync"
worker_connections = 1000
max_requests = 1000
max_requests_jitter = 100
timeout = 30
keepalive = 2
```

## 📋 Deployment Checklist

### Pre-Deployment
- [ ] Server provisioned with required specifications
- [ ] SSL certificates obtained and configured
- [ ] Database server installed and secured
- [ ] Application user created with proper permissions
- [ ] Firewall configured and tested

### Application Deployment
- [ ] Code deployed to production directory
- [ ] Dependencies installed in virtual environment
- [ ] Environment variables configured
- [ ] Database schema initialized
- [ ] Process managers configured (Supervisor/PM2)

### Web Server Configuration
- [ ] Nginx installed and configured
- [ ] SSL/TLS properly configured
- [ ] Static files served efficiently
- [ ] API proxying working correctly
- [ ] Security headers configured

### Testing and Validation
- [ ] Application accessible via HTTPS
- [ ] Admin login working
- [ ] Database connectivity verified
- [ ] API endpoints responding correctly
- [ ] File uploads working
- [ ] Email notifications configured (if enabled)

### Security and Monitoring
- [ ] Log files created and rotated
- [ ] Monitoring tools configured
- [ ] Backup procedures tested
- [ ] Security scanning completed
- [ ] Performance baseline established

### Documentation and Handover
- [ ] Deployment documentation updated
- [ ] Admin credentials shared securely
- [ ] Support contacts provided
- [ ] Maintenance procedures documented

## 📞 Support and Maintenance

### Regular Maintenance Tasks

**Daily:**
- Monitor system logs for errors
- Check disk space and system resources
- Verify backup completion

**Weekly:**
- Review security logs
- Update system packages
- Database optimization

**Monthly:**
- SSL certificate renewal check
- Performance review and optimization
- Security vulnerability assessment

### Getting Help

For deployment issues or questions:
- Review [Troubleshooting Guide](TROUBLESHOOTING.md)
- Check system logs for error details
- Contact technical support with specific error messages
- Provide deployment environment details

## 📚 Additional Resources

- [API Documentation](API_DOCUMENTATION.md) - Complete API reference
- [User Guide](USER_GUIDE.md) - End-user documentation
- [Contributing Guide](CONTRIBUTING.md) - Development setup
- [Troubleshooting Guide](TROUBLESHOOTING.md) - Common issues and solutions

---

**Note**: This deployment handles sensitive mental health data. Ensure all deployment procedures comply with applicable privacy, security, and regulatory requirements.