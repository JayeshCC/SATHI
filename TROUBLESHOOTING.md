# SATHI Troubleshooting Guide

## 🎯 Overview

This guide provides solutions to common issues encountered when using SATHI (System for Analyzing and Tracking Human Intelligence). Use this guide to quickly diagnose and resolve problems.

## 📋 Table of Contents

1. [Common System Issues](#common-system-issues)
2. [Login and Authentication Problems](#login-and-authentication-problems)
3. [Database Connection Issues](#database-connection-issues)
4. [Model Loading Errors](#model-loading-errors)
5. [Camera and CCTV Issues](#camera-and-cctv-issues)
6. [Performance Problems](#performance-problems)
7. [Frontend/UI Issues](#frontendui-issues)
8. [API and Network Issues](#api-and-network-issues)
9. [Data and Backup Issues](#data-and-backup-issues)
10. [Security and Access Issues](#security-and-access-issues)

## 🚨 Common System Issues

### System Won't Start

**Problem**: Application fails to start or crashes immediately

**Symptoms**:
- Error messages during startup
- Service fails to start
- Application exits unexpectedly

**Solutions**:

1. **Check System Logs**:
```bash
# Check application logs
tail -f /opt/crpf_monitor/logs/backend.log

# Check supervisor logs
sudo journalctl -u supervisor -n 50

# Check system logs
sudo journalctl -xe
```

2. **Verify Dependencies**:
```bash
# Check Python environment
cd /opt/crpf_monitor/backend
source venv/bin/activate
python -c "import flask, mysql.connector, cv2, tensorflow"

# Check MySQL service
sudo systemctl status mysql

# Check Nginx service
sudo systemctl status nginx
```

3. **Check Environment Configuration**:
```bash
# Verify environment file exists
ls -la /opt/crpf_monitor/backend/.env.prod

# Check environment variables
cat /opt/crpf_monitor/backend/.env.prod | grep -v PASSWORD
```

### Database Connection Refused

**Problem**: "Connection refused" or "Access denied" errors

**Symptoms**:
- Cannot connect to database
- Login fails with database error
- API requests return database errors

**Solutions**:

1. **Check MySQL Service**:
```bash
# Check service status
sudo systemctl status mysql

# Start MySQL if stopped
sudo systemctl start mysql

# Enable auto-start
sudo systemctl enable mysql
```

2. **Verify Database Credentials**:
```bash
# Test database connection
mysql -u crpf_prod_user -p crpf_mental_health_prod

# If connection fails, reset permissions
mysql -u root -p
```

```sql
-- Reset user permissions
GRANT ALL PRIVILEGES ON crpf_mental_health_prod.* TO 'crpf_prod_user'@'localhost';
FLUSH PRIVILEGES;
```

3. **Check Database Configuration**:
```bash
# Verify MySQL is listening on correct port
sudo netstat -tlnp | grep 3306

# Check MySQL configuration
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

## 🔐 Login and Authentication Problems

### Admin Can't Login

**Problem**: Admin credentials not working

**Symptoms**:
- "Invalid credentials" error
- Login form doesn't respond
- Session immediately expires

**Solutions**:

1. **Verify Admin Account**:
```bash
mysql -u crpf_prod_user -p crpf_mental_health_prod
```

```sql
-- Check if admin user exists
SELECT force_id, user_type FROM users WHERE user_type = 'admin';

-- Reset admin password if needed
UPDATE users 
SET password_hash = '$2b$12$LQv3c1yqBwlVHpPiGZ99FOOK5QpyJDMpM7VIZRqeOZnKHEWGY.VKO' 
WHERE force_id = '100000001';
-- This sets password to 'admin123'
```

2. **Check Session Configuration**:
```bash
# Verify session settings in environment
grep SESSION /opt/crpf_monitor/backend/.env.prod

# Check if session files exist
ls -la /tmp/flask_session/
```

3. **Clear Browser Data**:
- Clear browser cookies and cache
- Try incognito/private browsing mode
- Try different browser

### Session Keeps Expiring

**Problem**: User gets logged out frequently

**Solutions**:

1. **Adjust Session Timeout**:
```bash
# Edit environment configuration
nano /opt/crpf_monitor/backend/.env.prod

# Increase session timeout (in seconds)
SESSION_TIMEOUT=3600  # 1 hour instead of 30 minutes
```

2. **Check Server Time**:
```bash
# Verify server time is correct
timedatectl status

# Sync time if needed
sudo ntpdate -s time.nist.gov
```

## 🗄️ Database Connection Issues

### Database Performance Problems

**Problem**: Slow database queries

**Symptoms**:
- Long loading times
- Timeout errors
- High CPU usage

**Solutions**:

1. **Optimize Database**:
```sql
-- Check table status
SHOW TABLE STATUS;

-- Optimize tables
OPTIMIZE TABLE users, weekly_sessions, cctv_detections, question_responses;

-- Analyze tables
ANALYZE TABLE users, weekly_sessions, cctv_detections, question_responses;
```

2. **Check Indexes**:
```sql
-- Verify important indexes exist
SHOW INDEX FROM weekly_sessions;
SHOW INDEX FROM users;

-- Create missing indexes if needed
CREATE INDEX idx_weekly_sessions_force_timestamp ON weekly_sessions(force_id, completion_timestamp DESC);
CREATE INDEX idx_users_type_force ON users(user_type, force_id);
```

3. **Monitor Query Performance**:
```sql
-- Enable slow query log
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 2;

-- Check slow queries
SELECT * FROM mysql.slow_log ORDER BY start_time DESC LIMIT 10;
```

### Database Corruption

**Problem**: Database tables corrupted

**Symptoms**:
- Error reading from tables
- Inconsistent data
- Application crashes on database access

**Solutions**:

1. **Check Table Integrity**:
```sql
-- Check all tables
CHECK TABLE users, weekly_sessions, cctv_detections, question_responses;

-- Repair if needed
REPAIR TABLE table_name;
```

2. **Restore from Backup**:
```bash
# Stop application
sudo supervisorctl stop crpf_backend

# Restore from backup
gunzip < /opt/crpf_monitor/backups/backup_YYYYMMDD_HHMMSS.sql.gz | \
mysql -u crpf_prod_user -p crpf_mental_health_prod

# Restart application
sudo supervisorctl start crpf_backend
```

## 🤖 Model Loading Errors

### AI Models Not Loading

**Problem**: "Model file not found" or "Invalid model format"

**Symptoms**:
- Emotion detection not working
- Face recognition failing
- Error messages about missing models

**Solutions**:

1. **Verify Model Files**:
```bash
# Check if model files exist
ls -la /opt/crpf_monitor/backend/model/
ls -la /opt/crpf_monitor/backend/storage/models/

# Expected files:
# - emotion_model.json
# - emotion_model.h5
# - face_recognition_model.pkl
```

2. **Download Missing Models**:
```bash
cd /opt/crpf_monitor/backend

# Download emotion detection model (if available)
# Or retrain the model
python -c "
from services.emotion_detection_service import EmotionDetectionService
service = EmotionDetectionService()
service.train_emotion_model()
"
```

3. **Check Model Permissions**:
```bash
# Fix file permissions
sudo chown -R crpf_monitor:crpf_monitor /opt/crpf_monitor/backend/model/
sudo chown -R crpf_monitor:crpf_monitor /opt/crpf_monitor/backend/storage/

# Set correct permissions
chmod 644 /opt/crpf_monitor/backend/model/*
chmod 644 /opt/crpf_monitor/backend/storage/models/*
```

### Face Recognition Not Working

**Problem**: Cannot recognize faces or train face recognition model

**Solutions**:

1. **Check Training Data**:
```bash
# Verify profile images exist
ls -la /opt/crpf_monitor/backend/storage/face_encodings/

# Check if any soldiers have profile images
mysql -u crpf_prod_user -p crpf_mental_health_prod
```

```sql
SELECT COUNT(*) FROM users WHERE user_type = 'soldier';
-- Should show number of soldiers with profile images
```

2. **Retrain Face Recognition Model**:
```bash
cd /opt/crpf_monitor/backend
source venv/bin/activate

python -c "
from services.face_recognition_service import FaceRecognitionService
service = FaceRecognitionService()
service.train_model()
"
```

3. **Test Face Recognition**:
```python
# Test face recognition functionality
import cv2
from services.face_recognition_service import FaceRecognitionService

# Test camera access
cap = cv2.VideoCapture(0)
if cap.isOpened():
    print("Camera working")
    ret, frame = cap.read()
    if ret:
        print("Can capture frames")
cap.release()

# Test face recognition service
service = FaceRecognitionService()
print(f"Model loaded: {service.is_model_trained()}")
```

## 📹 Camera and CCTV Issues

### Camera Not Detected

**Problem**: Cannot access camera for emotion detection

**Symptoms**:
- "Camera not found" error
- Black screen instead of video feed
- Emotion detection not working

**Solutions**:

1. **Test Camera Access**:
```bash
# Check if camera device exists
ls -la /dev/video*

# Test camera with system tools
sudo apt install v4l-utils
v4l2-ctl --list-devices
```

2. **Test Camera in Python**:
```python
import cv2

# Test different camera indices
for i in range(5):
    cap = cv2.VideoCapture(i)
    if cap.isOpened():
        ret, frame = cap.read()
        if ret:
            print(f"Camera {i} working")
            print(f"Resolution: {frame.shape}")
        cap.release()
    else:
        print(f"Camera {i} not available")
```

3. **Fix Camera Permissions**:
```bash
# Add user to video group
sudo usermod -a -G video crpf_monitor

# Check camera permissions
ls -la /dev/video0

# Restart application
sudo supervisorctl restart crpf_backend
```

### CCTV Monitoring Stops

**Problem**: CCTV monitoring starts but stops unexpectedly

**Solutions**:

1. **Check Application Logs**:
```bash
# Check for camera-related errors
grep -i camera /opt/crpf_monitor/logs/backend.log
grep -i cctv /opt/crpf_monitor/logs/backend.log
grep -i emotion /opt/crpf_monitor/logs/backend.log
```

2. **Monitor System Resources**:
```bash
# Check CPU and memory usage
top -p $(pgrep -f "python app.py")

# Check disk space
df -h /opt/crpf_monitor/
```

3. **Restart Camera Service**:
```bash
# Through API
curl -X POST http://localhost:5000/api/image/stop-monitoring
curl -X POST http://localhost:5000/api/image/start-monitoring \
     -H "Content-Type: application/json" \
     -d '{"date": "2024-01-15"}'
```

## ⚡ Performance Problems

### System Running Slowly

**Problem**: Application response times are slow

**Symptoms**:
- Pages take long time to load
- API requests timeout
- High server resource usage

**Solutions**:

1. **Check System Resources**:
```bash
# Check CPU usage
top

# Check memory usage
free -h

# Check disk usage
df -h

# Check I/O wait
iostat 1 5
```

2. **Optimize Database**:
```sql
-- Find slow queries
SHOW PROCESSLIST;

-- Optimize tables
OPTIMIZE TABLE weekly_sessions, users, cctv_detections;

-- Update table statistics
ANALYZE TABLE weekly_sessions, users, cctv_detections;
```

3. **Restart Services**:
```bash
# Restart backend
sudo supervisorctl restart crpf_backend

# Restart database
sudo systemctl restart mysql

# Restart web server
sudo systemctl restart nginx
```

### High Memory Usage

**Problem**: Application consuming excessive memory

**Solutions**:

1. **Monitor Memory Usage**:
```bash
# Check memory usage by process
ps aux --sort=-%mem | head -10

# Check memory details
cat /proc/meminfo
```

2. **Optimize Python Application**:
```python
# Add to app.py or main module
import gc

# Force garbage collection periodically
gc.collect()

# Monitor memory usage
import psutil
process = psutil.Process()
print(f"Memory: {process.memory_info().rss / 1024 / 1024:.2f} MB")
```

3. **Optimize Image Processing**:
```python
# In emotion detection service
import cv2

# Release resources after processing
def process_frame(self, frame):
    # Process frame
    processed_frame = self.preprocess(frame)
    
    # Clear memory
    del frame
    del processed_frame
    cv2.destroyAllWindows()
```

## 🖥️ Frontend/UI Issues

### Page Not Loading

**Problem**: Frontend pages show errors or don't load

**Symptoms**:
- White screen
- JavaScript errors in console
- API calls failing

**Solutions**:

1. **Check Browser Console**:
   - Open browser developer tools (F12)
   - Check Console tab for JavaScript errors
   - Check Network tab for failed requests

2. **Verify Static Files**:
```bash
# Check if static files are served correctly
curl -I https://your-domain.com/static/css/main.css
curl -I https://your-domain.com/static/js/main.js

# Check Nginx static file configuration
sudo nginx -t
```

3. **Clear Browser Cache**:
   - Hard refresh (Ctrl+F5)
   - Clear browser cache and cookies
   - Try incognito/private mode

### API Calls Failing

**Problem**: Frontend cannot communicate with backend

**Solutions**:

1. **Test API Directly**:
```bash
# Test backend health
curl http://localhost:5000/api/monitor/health

# Test with authentication
curl -X POST http://localhost:5000/api/auth/login \
     -H "Content-Type: application/json" \
     -d '{"force_id":"100000001","password":"admin123"}'
```

2. **Check CORS Configuration**:
```python
# In backend app.py
from flask_cors import CORS

# Ensure CORS is configured
CORS(app, origins=['https://your-domain.com'])
```

3. **Verify Proxy Configuration**:
```nginx
# Check Nginx proxy configuration
location /api/ {
    proxy_pass http://127.0.0.1:5000;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
}
```

## 🌐 API and Network Issues

### API Response Timeouts

**Problem**: API requests timing out

**Solutions**:

1. **Increase Timeout Settings**:
```nginx
# In Nginx configuration
location /api/ {
    proxy_pass http://127.0.0.1:5000;
    proxy_connect_timeout 60s;
    proxy_send_timeout 60s;
    proxy_read_timeout 60s;
}
```

2. **Optimize Database Queries**:
```python
# Add query timeouts
cursor.execute("SET SESSION wait_timeout=30")

# Use pagination for large results
def get_soldiers_paginated(page=1, per_page=20):
    offset = (page - 1) * per_page
    query = f"SELECT * FROM users LIMIT {per_page} OFFSET {offset}"
    return execute_query(query)
```

3. **Add Request Monitoring**:
```python
import time
from functools import wraps

def monitor_performance(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        duration = time.time() - start_time
        
        if duration > 5.0:  # Log slow requests
            print(f"Slow request: {func.__name__} took {duration:.2f}s")
        
        return result
    return wrapper
```

### SSL Certificate Issues

**Problem**: SSL certificate errors

**Solutions**:

1. **Check Certificate Status**:
```bash
# Check certificate expiration
sudo certbot certificates

# Test SSL configuration
openssl s_client -connect your-domain.com:443
```

2. **Renew Certificate**:
```bash
# Manual renewal
sudo certbot renew --force-renewal

# Test renewal
sudo certbot renew --dry-run
```

3. **Fix Certificate Chain**:
```bash
# Check certificate chain
curl -I https://your-domain.com

# Update Nginx configuration if needed
sudo nano /etc/nginx/sites-available/crpf_monitor
```

## 💾 Data and Backup Issues

### Data Loss or Corruption

**Problem**: Important data missing or corrupted

**Solutions**:

1. **Check Recent Backups**:
```bash
# List available backups
ls -la /opt/crpf_monitor/backups/

# Check backup integrity
gunzip -t /opt/crpf_monitor/backups/backup_YYYYMMDD_HHMMSS.sql.gz
```

2. **Restore from Backup**:
```bash
# Stop application
sudo supervisorctl stop crpf_backend

# Restore database
gunzip < /opt/crpf_monitor/backups/backup_YYYYMMDD_HHMMSS.sql.gz | \
mysql -u crpf_prod_user -p crpf_mental_health_prod

# Verify restoration
mysql -u crpf_prod_user -p crpf_mental_health_prod -e "SELECT COUNT(*) FROM users;"

# Restart application
sudo supervisorctl start crpf_backend
```

### Backup Failures

**Problem**: Automated backups not working

**Solutions**:

1. **Check Backup Script**:
```bash
# Test backup script manually
sudo -u crpf_monitor /opt/crpf_monitor/scripts/backup_db.sh

# Check cron job
crontab -l -u crpf_monitor
```

2. **Verify Backup Directory**:
```bash
# Check backup directory permissions
ls -la /opt/crpf_monitor/backups/

# Ensure sufficient disk space
df -h /opt/crpf_monitor/
```

3. **Test Database Access**:
```bash
# Test backup process
mysqldump -u crpf_prod_user -p crpf_mental_health_prod > test_backup.sql

# Check if backup file was created
ls -la test_backup.sql
```

## 🔒 Security and Access Issues

### Unauthorized Access Attempts

**Problem**: Security alerts or suspicious access

**Solutions**:

1. **Check Access Logs**:
```bash
# Check Nginx access logs
sudo tail -f /var/log/nginx/crpf_monitor_access.log

# Check authentication logs
sudo grep "authentication" /opt/crpf_monitor/logs/backend.log

# Check system auth logs
sudo tail -f /var/log/auth.log
```

2. **Block Suspicious IPs**:
```bash
# Block IP with UFW
sudo ufw deny from <suspicious_ip>

# Or use fail2ban
sudo apt install fail2ban
```

3. **Review User Accounts**:
```sql
-- Check all admin users
SELECT force_id, user_type, last_login FROM users WHERE user_type = 'admin';

-- Check recent login attempts
SELECT * FROM login_attempts ORDER BY timestamp DESC LIMIT 20;
```

### Permission Denied Errors

**Problem**: File or directory permission errors

**Solutions**:

1. **Fix File Permissions**:
```bash
# Fix application file permissions
sudo chown -R crpf_monitor:crpf_monitor /opt/crpf_monitor/

# Fix specific directories
sudo chmod 755 /opt/crpf_monitor/backend/storage/
sudo chmod 644 /opt/crpf_monitor/backend/storage/models/*
```

2. **Check Service User**:
```bash
# Verify service is running as correct user
ps aux | grep "python app.py"

# Check supervisor configuration
cat /etc/supervisor/conf.d/crpf_backend.conf
```

## 📞 Getting Additional Help

### When to Contact Support

**Escalate to technical support for:**

1. **System-wide outages** affecting all users
2. **Data security incidents** or suspected breaches
3. **Critical functionality failures** affecting mental health monitoring
4. **Hardware failures** or infrastructure issues
5. **Performance issues** that cannot be resolved with standard troubleshooting

### Information to Provide

When contacting support, include:

1. **Detailed Problem Description**:
   - What were you trying to do?
   - What happened instead?
   - When did the problem start?

2. **Error Messages**:
   - Exact error text (copy/paste or screenshot)
   - Error codes if any
   - Browser console errors

3. **System Information**:
   - Operating system and version
   - Browser type and version
   - Server specifications
   - Application version

4. **Log Files**:
```bash
# Collect relevant logs
sudo tar -czf support_logs.tar.gz \
    /opt/crpf_monitor/logs/backend.log \
    /var/log/nginx/crpf_monitor_error.log \
    /var/log/mysql/error.log
```

5. **Reproduction Steps**:
   - Step-by-step instructions to reproduce the issue
   - Any workarounds you've tried
   - Whether the issue affects all users or specific ones

### Emergency Procedures

For **CRITICAL ISSUES** affecting mental health monitoring:

1. **Immediate Response** (if someone is at immediate risk):
   - Contact emergency services
   - Notify chain of command
   - Document the technical issue separately

2. **System Restoration**:
   - Switch to manual monitoring procedures
   - Restore from most recent backup
   - Implement temporary workarounds

3. **Communication**:
   - Notify all stakeholders
   - Provide regular updates on restoration progress
   - Document incident for review

## 📚 Additional Resources

- [User Guide](USER_GUIDE.md) - Complete user documentation
- [API Documentation](API_DOCUMENTATION.md) - Technical API reference
- [Deployment Guide](DEPLOYMENT_GUIDE.md) - Installation and deployment
- [Contributing Guide](CONTRIBUTING.md) - Development setup and guidelines

---

**Remember**: This system handles sensitive mental health data. Always prioritize data security and individual safety when troubleshooting issues. If in doubt about someone's immediate safety, contact emergency services or your organization's crisis intervention team.