# SATHI Troubleshooting Guide

Common issues and their solutions for SATHI (CRPF Mental Health Monitoring System).

## 📋 Table of Contents

1. [Quick Diagnostics](#quick-diagnostics)
2. [Login & Authentication Issues](#login--authentication-issues)
3. [Survey & Assessment Problems](#survey--assessment-problems)
4. [Camera & Media Issues](#camera--media-issues)
5. [Performance Issues](#performance-issues)
6. [Data & Results Problems](#data--results-problems)
7. [Browser Compatibility](#browser-compatibility)
8. [Network & Connectivity](#network--connectivity)
9. [Mobile Device Issues](#mobile-device-issues)
10. [When to Contact Support](#when-to-contact-support)

## 🔍 Quick Diagnostics

Before diving into specific issues, try these quick fixes:

### Universal Solutions (Try First)

1. **Refresh the Page**: Press `Ctrl+F5` (Windows) or `Cmd+Shift+R` (Mac)
2. **Clear Browser Cache**: 
   - Chrome: `Ctrl+Shift+Delete` → Clear browsing data
   - Firefox: `Ctrl+Shift+Delete` → Clear recent history
   - Safari: `Cmd+Option+E` → Empty caches
3. **Check Internet Connection**: Ensure stable internet connectivity
4. **Try Different Browser**: Test with Chrome, Firefox, or Edge
5. **Restart Browser**: Close completely and reopen

### System Status Check

✅ **Check if the issue affects everyone or just you**
- Try accessing from different device/network
- Ask colleagues if they're experiencing similar issues
- Check system status page (if available)

## 🔐 Login & Authentication Issues

### Cannot Login - Invalid Credentials

**Symptoms**: "Invalid username or password" error

**Solutions**:

1. **Verify Force ID Format**
   ```
   Correct: CRPF001234
   Incorrect: crpf001234, CRPF-001234, 001234
   ```

2. **Check Password Requirements**
   - Minimum 8 characters
   - At least one uppercase letter
   - At least one lowercase letter  
   - At least one number
   - At least one special character

3. **Password Reset**
   - Click "Forgot Password" on login page
   - Enter your Force ID
   - Follow email instructions (check spam folder)
   - Contact administrator if email not received

### Session Expires Too Quickly

**Symptoms**: Logged out frequently, session timeout errors

**Solutions**:

1. **Check Session Settings**
   - Default session: 30 minutes of inactivity
   - Keep browser tab active
   - Avoid long breaks during surveys

2. **Browser Settings**
   - Enable cookies for the site
   - Disable "Clear cookies on exit"
   - Check if ad blockers are interfering

3. **Security Software**
   - Whitelist SATHI domain in antivirus
   - Disable VPN if causing issues
   - Check firewall settings

### Login Page Won't Load

**Symptoms**: Blank page, loading forever, connection errors

**Solutions**:

1. **Network Issues**
   ```bash
   # Test connection
   ping sathi.crpf.gov.in
   
   # Check DNS resolution
   nslookup sathi.crpf.gov.in
   ```

2. **Browser Issues**
   - Clear DNS cache: `ipconfig /flushdns` (Windows)
   - Disable browser extensions temporarily
   - Try incognito/private browsing mode

3. **Firewall/Proxy**
   - Contact IT department about domain access
   - Check proxy settings
   - Try different network (mobile hotspot)

## 📝 Survey & Assessment Problems

### Survey Won't Start

**Symptoms**: "Start Survey" button doesn't work, error messages

**Solutions**:

1. **Prerequisites Check**
   - Ensure camera permission granted
   - Check if active questionnaire available
   - Verify you haven't already completed today's survey

2. **Browser Permissions**
   ```
   Chrome: Settings → Privacy → Site Settings → Camera
   Firefox: about:preferences#privacy → Permissions → Camera
   Safari: Preferences → Websites → Camera
   ```

3. **JavaScript Issues**
   - Enable JavaScript in browser
   - Disable ad blockers temporarily
   - Check browser console for errors (F12)

### Survey Progress Not Saving

**Symptoms**: Lost progress when returning, starting over repeatedly

**Solutions**:

1. **Browser Storage**
   - Enable local storage/cookies
   - Check available disk space
   - Don't use private/incognito mode

2. **Session Management**
   - Don't close browser during survey
   - Avoid switching tabs frequently
   - Complete survey in one session when possible

3. **Auto-Save Settings**
   - Progress saves every 30 seconds automatically
   - Manual save: Look for "Save Progress" button
   - Check network stability during survey

### Questions Not Loading Properly

**Symptoms**: Blank questions, missing options, formatting issues

**Solutions**:

1. **Questionnaire Issues**
   - Refresh the page
   - Try different questionnaire
   - Report specific question number to admin

2. **Language Settings**
   - Check selected language (English/Hindi)
   - Clear language cache
   - Reset language preference

3. **Data Loading**
   - Wait for complete page load
   - Check network speed
   - Try during off-peak hours

## 📹 Camera & Media Issues

### Camera Not Working

**Symptoms**: Black screen, "Camera not found", permission denied

**Solutions**:

1. **Permission Issues**
   ```
   Browser Permissions:
   1. Click lock icon in address bar
   2. Set Camera to "Allow"
   3. Refresh page
   ```

2. **Hardware Issues**
   - Check if camera works in other apps
   - Ensure camera isn't used by other applications
   - Try external camera if built-in fails

3. **Driver Issues**
   - Update camera drivers
   - Restart computer
   - Check Device Manager (Windows)

### Poor Image Quality

**Symptoms**: Blurry image, dark picture, emotion detection fails

**Solutions**:

1. **Lighting Optimization**
   - Face light source (window/lamp)
   - Avoid backlighting
   - Use even, diffused lighting

2. **Positioning**
   - Center face in frame
   - Maintain 18-24 inches distance
   - Ensure camera is at eye level

3. **Camera Settings**
   - Clean camera lens
   - Adjust browser zoom to 100%
   - Check camera resolution settings

### Emotion Detection Not Working

**Symptoms**: "No face detected", "Analysis failed", poor confidence scores

**Solutions**:

1. **Face Detection Requirements**
   - Clear view of full face
   - Remove glasses/masks temporarily
   - Look directly at camera
   - Keep face still during analysis

2. **Technical Requirements**
   - Good lighting conditions
   - Stable internet connection
   - Updated browser version
   - Allow sufficient processing time

3. **Alternative Options**
   - Complete survey without emotion detection
   - Contact admin about technical requirements
   - Try different device/camera

## 🐌 Performance Issues

### Slow Loading Times

**Symptoms**: Pages take long to load, timeouts, unresponsive interface

**Solutions**:

1. **Network Optimization**
   - Test internet speed: minimum 1 Mbps recommended
   - Use wired connection if possible
   - Close bandwidth-heavy applications

2. **Browser Optimization**
   ```
   Performance Tips:
   - Close unnecessary tabs
   - Clear browser cache regularly
   - Disable heavy extensions
   - Update to latest browser version
   ```

3. **System Resources**
   - Close other applications
   - Check available RAM (minimum 4GB recommended)
   - Restart computer if running slowly

### High Memory Usage

**Symptoms**: Browser crashes, system freezes, out of memory errors

**Solutions**:

1. **Memory Management**
   - Close unused browser tabs
   - Restart browser periodically
   - Check Task Manager for memory usage

2. **Browser Settings**
   - Disable hardware acceleration
   - Reduce browser cache size
   - Clear browsing data regularly

3. **System Optimization**
   - Update operating system
   - Check for malware
   - Consider adding more RAM

## 📊 Data & Results Problems

### Results Not Displaying

**Symptoms**: Blank results page, "No data found", loading errors

**Solutions**:

1. **Data Availability**
   - Ensure surveys have been completed
   - Check date range filters
   - Verify data processing completion

2. **Permission Issues**
   - Confirm account has result viewing permissions
   - Contact admin about access levels
   - Check user role settings

3. **Technical Issues**
   - Clear browser cache
   - Try different browser
   - Check network connectivity

### Incorrect Results/Scores

**Symptoms**: Unexpected scores, missing data, calculation errors

**Solutions**:

1. **Data Verification**
   - Review survey responses for accuracy
   - Check completion status of assessments
   - Verify emotion detection data quality

2. **Calculation Issues**
   - Contact technical support immediately
   - Document specific discrepancies
   - Provide screenshot evidence

3. **Temporary Workarounds**
   - Note discrepancies for support review
   - Use alternative assessment methods
   - Manual calculation verification

## 🌐 Browser Compatibility

### Supported Browsers

✅ **Fully Supported**:
- Chrome 80+ (Recommended)
- Firefox 75+
- Safari 13+ (macOS/iOS)
- Edge 80+

⚠️ **Limited Support**:
- Internet Explorer 11 (deprecated)
- Older mobile browsers

❌ **Not Supported**:
- Internet Explorer 10 and below

### Browser-Specific Issues

**Chrome Issues**:
```
Common fixes:
- Update to latest version
- Reset Chrome settings
- Disable hardware acceleration
- Clear site data
```

**Firefox Issues**:
```
Common fixes:
- Refresh Firefox
- Disable tracking protection for site
- Clear cookies and cache
- Check add-ons compatibility
```

**Safari Issues**:
```
Common fixes:
- Enable JavaScript
- Allow cross-site tracking
- Clear website data
- Update macOS/iOS
```

## 🌐 Network & Connectivity

### Connection Timeouts

**Symptoms**: "Connection timeout", "Server not responding"

**Solutions**:

1. **Network Diagnostics**
   ```bash
   # Test basic connectivity
   ping sathi.crpf.gov.in
   
   # Check specific ports
   telnet sathi.crpf.gov.in 443
   ```

2. **DNS Issues**
   - Try different DNS servers (8.8.8.8, 1.1.1.1)
   - Flush DNS cache
   - Contact network administrator

3. **Proxy/Firewall**
   - Check corporate firewall settings
   - Configure proxy exceptions
   - Try mobile hotspot as alternative

### Intermittent Connectivity

**Symptoms**: Works sometimes, random disconnections

**Solutions**:

1. **Network Stability**
   - Check WiFi signal strength
   - Use wired connection if possible
   - Restart router/modem

2. **ISP Issues**
   - Contact internet service provider
   - Check for service outages
   - Try different network

## 📱 Mobile Device Issues

### Mobile Browser Problems

**Symptoms**: Layout issues, buttons not working, slow performance

**Solutions**:

1. **Browser Selection**
   ```
   Recommended Mobile Browsers:
   - Chrome (Android/iOS)
   - Safari (iOS)
   - Firefox (Android/iOS)
   - Edge (Android/iOS)
   ```

2. **Mobile Optimization**
   - Use latest browser version
   - Enable desktop site if needed
   - Check available storage space

3. **Touch Interface**
   - Ensure accurate touch calibration
   - Use stylus for precision if needed
   - Zoom appropriately for buttons

### Camera Issues on Mobile

**Symptoms**: Camera not accessible, poor quality, orientation problems

**Solutions**:

1. **Permissions**
   - Check app permissions in device settings
   - Allow camera access for browser
   - Restart browser after permission change

2. **Hardware**
   - Clean camera lens
   - Ensure adequate lighting
   - Hold device steady

3. **Orientation**
   - Use portrait mode for surveys
   - Lock screen orientation
   - Ensure face fits properly in frame

## 📞 When to Contact Support

### Contact Support For:

🚨 **Immediate Issues**:
- Security concerns or data breaches
- System-wide outages
- Critical errors preventing survey completion

⚡ **High Priority**:
- Login issues for multiple users
- Data accuracy problems
- Performance degradation

📋 **Standard Issues**:
- Individual account problems
- Feature requests
- Training needs

### Support Information to Provide:

1. **User Information**
   - Force ID or username
   - User role (personnel/admin/provider)
   - Contact information

2. **Technical Details**
   - Operating system and version
   - Browser name and version
   - Error messages (exact text)
   - Screenshots of issues

3. **Problem Description**
   - What were you trying to do?
   - What actually happened?
   - When did the problem start?
   - Steps to reproduce

### Contact Methods:

📧 **Email**: support@sathi.crpf.gov.in  
📞 **Phone**: 1800-XXX-XXXX (24/7)  
💬 **Chat**: Available in system (bottom-right widget)  
🎫 **Ticket System**: Create ticket through admin panel  

### Response Times:

- **Critical Issues**: 2 hours
- **High Priority**: 8 hours  
- **Standard Issues**: 24 hours
- **Feature Requests**: 5 business days

---

**Remember**: Many issues can be resolved quickly with basic troubleshooting. Try the simple solutions first before contacting support. When in doubt, document the issue and reach out to our support team for assistance.