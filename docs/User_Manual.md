# SATHI - User Manual

## Table of Contents

1. [Introduction](#introduction)
2. [Getting Started](#getting-started)
3. [System Login](#system-login)
4. [Admin Dashboard](#admin-dashboard)
5. [Soldier Management](#soldier-management)
6. [Survey Management](#survey-management)
7. [Mental Health Assessment](#mental-health-assessment)
8. [Reports and Analytics](#reports-and-analytics)
9. [System Configuration](#system-configuration)
10. [Troubleshooting](#troubleshooting)
11. [Frequently Asked Questions](#frequently-asked-questions)

## Introduction

Welcome to **SATHI** (System for Analyzing and Tracking Human Intelligence), a comprehensive mental health monitoring system designed specifically for CRPF personnel. This user manual will guide you through all the features and functions of the system in a simple, step-by-step manner.

### What is SATHI?

SATHI is an advanced digital platform that helps monitor and assess the mental health of CRPF soldiers using:

- **Smart Surveys**: Interactive questionnaires in both English and Hindi
- **AI Technology**: Automatic analysis of responses using artificial intelligence
- **Real-time Monitoring**: Live emotion detection during surveys
- **Comprehensive Reports**: Detailed analytics and health assessments
- **Early Warning System**: Alerts for personnel who may need support

### Who Can Use SATHI?

**For Administrators:**
- View overall mental health statistics
- Manage soldier profiles and surveys
- Generate detailed reports
- Configure system settings
- Monitor system performance

**For Soldiers:**
- Complete weekly mental health surveys
- View personal assessment results
- Access support resources

### Key Benefits

✅ **Easy to Use**: Simple, user-friendly interface  
✅ **Secure**: All data is protected and confidential  
✅ **Multilingual**: Available in English and Hindi  
✅ **Mobile-Friendly**: Works on computers, tablets, and phones  
✅ **Real-time Results**: Instant analysis and feedback  
✅ **Comprehensive Reports**: Detailed insights for decision-making  

## Getting Started

### System Requirements

**For Computers:**
- Windows 10 or later / macOS / Linux
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Internet connection
- Screen resolution: 1024x768 or higher

**For Mobile Devices:**
- Android 7.0+ or iOS 12+
- Mobile browser
- Internet connection

### Accessing the System

1. **Open your web browser**
2. **Navigate to the SATHI website**
   - URL: `http://your-sathi-domain.com`
   - Contact your IT administrator for the exact web address
3. **Bookmark the page** for easy future access

### Important Security Notes

🔒 **Keep Your Credentials Safe**
- Never share your login details with anyone
- Use a strong, unique password
- Log out when finished using the system

🔒 **Data Privacy**
- All information is strictly confidential
- Data is used only for mental health support
- No personal information is shared without permission

## System Login

### Admin Login Process

**Step 1: Access the Login Page**

![Login Page](screenshots/login_page.jpg)

1. Open your web browser and go to the SATHI website
2. You will see the login screen with the SATHI logo
3. The page displays two input fields: Force ID and Password

**Step 2: Enter Your Credentials**

1. **Force ID Field**: Enter your 9-digit Force ID
   - Example: `100000001`
   - Make sure all 9 digits are correct

2. **Password Field**: Enter your password
   - Passwords are case-sensitive
   - Type carefully to avoid errors

**Step 3: Sign In**

1. Click the **"Sign In"** button
2. The system will verify your credentials
3. If successful, you'll be redirected to the main dashboard

### Troubleshooting Login Issues

**Problem: "Invalid credentials" error**
- ✅ Check that your Force ID is exactly 9 digits
- ✅ Verify your password is typed correctly
- ✅ Ensure Caps Lock is off
- ✅ Contact your administrator if the problem persists

**Problem: Page won't load**
- ✅ Check your internet connection
- ✅ Try refreshing the page
- ✅ Clear your browser cache
- ✅ Try a different browser

**Problem: Forgot password**
- ✅ Contact your system administrator
- ✅ They will reset your password securely
- ✅ Never share your new password with others

### First-Time Login

If this is your first time logging in:

1. **Use the temporary credentials** provided by your administrator
2. **Change your password** immediately after login
3. **Familiarize yourself** with the dashboard layout
4. **Review the user manual** to understand all features

## Admin Dashboard

Once you successfully log in, you'll see the main Admin Dashboard - your central hub for monitoring and managing the mental health system.

### Dashboard Overview

![Admin Dashboard](screenshots/admin_dashboard.jpg)

The dashboard is organized into several key sections:

**Top Navigation Bar:**
- 🏠 Dashboard (current page)
- 👥 Soldiers management
- 📋 Survey management
- ⚙️ System settings
- 🔓 Logout option

**Main Statistics Cards:**
- **Total Soldiers**: Shows the number of registered personnel
- **Survey Responses**: Current survey completion statistics
- **High Risk Soldiers**: Number of personnel requiring attention
- **Critical Alerts**: Urgent cases needing immediate action

### Understanding the Statistics

**1. Total Soldiers Card**
- Displays the total number of soldiers registered in the system
- Includes both active and inactive personnel
- Updates automatically as new soldiers are added

**2. Current Survey Responses**
- Shows completion rate for the active survey
- Displays number of completed vs. pending responses
- Helps track survey participation

**3. High Risk Soldiers**
- Indicates personnel with elevated mental health scores
- Includes both "High" and "Critical" risk categories
- Requires administrative attention and follow-up

**4. Critical Alerts**
- Shows soldiers needing immediate support
- These cases should be prioritized for counseling
- System generates automatic notifications

### Dashboard Analytics

![Dashboard Analytics](screenshots/dashboard_analytics.jpg)

**Risk Distribution Chart:**
- **Green (Low Risk)**: Soldiers with good mental health indicators
- **Yellow (Medium Risk)**: Moderate concern, monitor regularly
- **Orange (High Risk)**: Significant concern, needs intervention
- **Red (Critical Risk)**: Urgent attention required

**Trends Chart:**
- Shows mental health trends over the past 7 days
- Helps identify patterns and seasonal changes
- Useful for planning interventions and resources

### How to Navigate the Dashboard

**Step 1: Review Overall Statistics**
1. Start by checking the main statistic cards
2. Note any high numbers in risk categories
3. Pay special attention to critical alerts

**Step 2: Analyze Trends**
1. Look at the risk distribution chart
2. Check if high-risk numbers are increasing
3. Review the weekly trends for patterns

**Step 3: Take Action**
1. Click on high numbers to see detailed lists
2. Prioritize critical and high-risk cases
3. Plan follow-up actions and interventions

### Dashboard Refresh

The dashboard updates automatically every few minutes, but you can also:

1. **Manual Refresh**: Click the refresh button or press F5
2. **Real-time Updates**: Critical alerts appear immediately
3. **Time Frame Selection**: Choose different time periods for analysis

## Soldier Management

The Soldier Management section allows administrators to view, search, and manage all personnel in the system.

### Accessing Soldier Reports

**Step 1: Navigate to Soldiers Section**
1. Click **"Soldiers"** in the top navigation menu
2. You'll see the Soldiers Report page

![Soldiers Report](screenshots/soldiers_report.jpg)

**Step 2: Understanding the Soldiers List**

Each soldier entry shows:
- **Force ID**: Unique 9-digit identifier
- **Risk Level**: Current mental health risk category
- **Combined Score**: Overall mental health score (0-1 scale)
- **NLP Score**: Text analysis score from survey responses
- **Image Score**: Emotion detection score from webcam monitoring
- **Last Survey**: Date of most recent survey completion
- **Questionnaire**: Name of the survey completed

### Risk Level Categories

**🟢 LOW RISK (Green)**
- Score: 0.0 - 0.3
- Indication: Good mental health
- Action: Regular monitoring

**🟡 MEDIUM RISK (Yellow)**
- Score: 0.3 - 0.5
- Indication: Some stress indicators
- Action: Weekly check-ins

**🟠 HIGH RISK (Orange)**
- Score: 0.5 - 0.7
- Indication: Significant concern
- Action: Counseling recommended

**🔴 CRITICAL RISK (Red)**
- Score: 0.7 - 1.0
- Indication: Urgent attention needed
- Action: Immediate intervention required

### Filtering and Searching

**Using the Filter Options:**

1. **Risk Level Filter**:
   - Select "All", "Low", "Mid", "High", or "Critical"
   - Filter shows only soldiers in selected risk category

2. **Time Period Filter**:
   - Choose from last 3, 7, 30, or 180 days
   - Shows data from selected time frame

3. **Force ID Search**:
   - Enter specific Force ID to find individual soldiers
   - Useful for quick lookups

**Step-by-Step Filtering:**

1. **Select Risk Level**: Choose the risk category you want to view
2. **Choose Time Period**: Select the relevant time frame
3. **Apply Filters**: Click "Apply" or the filter automatically updates
4. **Review Results**: Examine the filtered list of soldiers

### Advanced Search

![Advanced Search](screenshots/advanced_search.jpg)

For more detailed searches:

**Step 1: Access Advanced Search**
1. Click the **"Advanced Search"** button
2. A detailed search form will appear

**Step 2: Configure Search Criteria**
- **Search Term**: Enter Force ID, name, or unit
- **Risk Levels**: Select multiple risk categories
- **Units**: Choose specific military units
- **Date Range**: Set custom start and end dates
- **Score Range**: Define minimum and maximum scores
- **Sorting Options**: Choose how to order results

**Step 3: Execute Search**
1. Fill in your desired criteria
2. Click **"Search"** to see results
3. Use **"Clear"** to reset all filters

### Viewing Individual Soldier Details

**To see detailed information about a specific soldier:**

1. **Find the soldier** in the list using filters or search
2. **Click on their Force ID** or name
3. **Review their profile** which includes:
   - Complete survey history
   - Risk score trends over time
   - Detailed mental health assessments
   - Recommended actions

### Managing Soldier Information

**Adding New Soldiers:**

1. Click **"Add Soldier"** button
2. Enter the new soldier's Force ID
3. Set a temporary password
4. Click **"Save"** to add them to the system

**Updating Soldier Information:**

1. Select the soldier from the list
2. Click **"Edit"** button
3. Update necessary information
4. Save changes

**Removing Soldiers:**

1. Select the soldier to remove
2. Click **"Delete"** (use with caution)
3. Confirm the deletion
4. This permanently removes all their data

### Exporting Reports

**To export soldier data:**

![Export Options](screenshots/soldiers_report.jpg)

1. **Apply desired filters** to select the soldiers you want to export
2. **Choose export format**:
   - **PDF**: Professional report format for printing
   - **CSV**: Data format for spreadsheet analysis
3. **Click the export button**
4. **Download the file** to your computer

**Export Uses:**
- Monthly reports for senior management
- Data analysis in external tools
- Backup of critical information
- Compliance documentation

## Survey Management

The Survey Management section allows administrators to create, modify, and manage questionnaires used for mental health assessment.

### Understanding Questionnaires

**What are Questionnaires?**
- Collections of questions designed to assess mental health
- Can be customized for different purposes
- Support both English and Hindi languages
- Only one questionnaire can be active at a time

### Accessing Survey Management

![Questionnaire Management](screenshots/questionnaire_management.jpg)

**Step 1: Navigate to Surveys**
1. Click **"Surveys"** in the main navigation
2. You'll see the Questionnaire Management page

**Step 2: View Existing Questionnaires**

The page shows all questionnaires with:
- **Title**: Name of the questionnaire
- **Description**: Purpose and details
- **Status**: Active or Inactive
- **Total Questions**: Number of questions in the survey
- **Created Date**: When the questionnaire was made
- **Actions**: Edit, View, Activate, or Delete options

### Creating a New Questionnaire

**Step 1: Start Creation Process**
1. Click the **"Create New Questionnaire"** button
2. A form will appear for questionnaire details

**Step 2: Fill in Basic Information**
- **Title**: Give your questionnaire a clear, descriptive name
  - Example: "Weekly Mental Health Assessment"
- **Description**: Explain the purpose and when it should be used
  - Example: "Standard weekly survey for all CRPF personnel"
- **Number of Questions**: Specify how many questions you plan to add
- **Status**: Choose whether to make it active immediately

**Step 3: Save the Questionnaire**
1. Review all information for accuracy
2. Click **"Create Questionnaire"**
3. The system will create the questionnaire and assign it an ID

### Adding Questions to Questionnaires

**Step 1: Select Questionnaire**
1. Find the questionnaire you want to edit
2. Click the **"View/Edit"** button

**Step 2: Add New Questions**
1. Click **"Add Question"** button
2. Enter the question text in English
3. The system can automatically translate to Hindi, or you can enter manual translation
4. Click **"Save Question"**

**Question Writing Best Practices:**
- ✅ Keep questions clear and simple
- ✅ Avoid technical or complex language
- ✅ Focus on feelings and mental state
- ✅ Be sensitive to cultural considerations
- ✅ Ensure questions are relevant to CRPF personnel

**Example Good Questions:**
- "How are you feeling today?"
- "How would you rate your stress level this week?"
- "Do you feel supported by your colleagues?"
- "How well did you sleep last night?"

### Managing Question Translations

**Automatic Translation:**
1. Enter question in English
2. System automatically generates Hindi translation
3. Review the translation for accuracy
4. Edit if necessary for cultural appropriateness

**Manual Translation:**
1. Enter English question
2. Clear the Hindi field
3. Type your own Hindi translation
4. This ensures cultural sensitivity and accuracy

### Activating Questionnaires

**Important**: Only one questionnaire can be active at a time.

**To activate a questionnaire:**
1. Find the questionnaire you want to activate
2. Click **"Activate"** button
3. Confirm your choice
4. The system will:
   - Deactivate any currently active questionnaire
   - Activate your selected questionnaire
   - Make it available for soldiers to complete

### Survey Form Experience

![Survey Form](screenshots/survey_form.jpg)

When soldiers complete surveys, they experience:

**1. Welcome Screen**
- Introduction to the survey
- Instructions in their preferred language
- Privacy and confidentiality information

**2. Question Presentation**
- One question at a time for focus
- Clear, readable text
- Language toggle (English/Hindi)
- Progress indicator

**3. Webcam Monitoring** (if enabled)
- Automatic emotion detection during survey
- No recording of video or images
- Real-time analysis for additional insights

**4. Completion Screen**
- Thank you message
- Summary of submission
- Next steps or recommendations

### Survey Completion Tracking

**Monitoring Survey Progress:**
1. Return to the main dashboard
2. Check the "Survey Responses" statistics
3. See completion rates and pending responses
4. Identify soldiers who haven't completed surveys

**Following Up on Incomplete Surveys:**
1. Use the soldiers report to find incomplete surveys
2. Send reminders to personnel
3. Offer assistance for technical difficulties
4. Ensure accessibility for all soldiers

### Survey Analytics

**Understanding Survey Results:**
- Results are automatically analyzed using AI
- Sentiment analysis processes text responses
- Emotion detection adds visual cues
- Combined scores provide comprehensive assessment

**Result Categories:**
- **Text Analysis (NLP)**: 70% weight in final score
- **Emotion Detection**: 30% weight in final score
- **Combined Score**: Overall mental health indicator

## Mental Health Assessment

The system uses advanced AI technology to analyze survey responses and provide comprehensive mental health assessments.

### How Assessment Works

**1. Data Collection**
- Text responses from survey questions
- Emotional expressions captured during survey (if webcam enabled)
- Historical data and trends
- Self-reported mental state ratings

**2. AI Analysis**
- Natural Language Processing analyzes text sentiment
- Computer vision analyzes facial expressions for emotions
- Machine learning combines multiple data sources
- Risk algorithms calculate final scores

**3. Scoring System**
- Scores range from 0.0 (excellent mental health) to 1.0 (critical concern)
- Weighted combination: 70% text analysis + 30% emotion detection
- Dynamic thresholds adjust based on population data
- Continuous learning improves accuracy over time

### Understanding Assessment Results

![Survey Completion](screenshots/survey_completion.jpg)

**Mental Health Categories:**

**🟢 EXCELLENT MENTAL HEALTH (0.0 - 0.2)**
- **Indicator**: Positive emotional state, no concerns
- **Recommendation**: Continue normal duties
- **Action Required**: Regular monitoring only

**🟢 GOOD MENTAL HEALTH (0.2 - 0.3)**
- **Indicator**: Stable emotional state with minor stress
- **Recommendation**: Continue normal duties, light monitoring
- **Action Required**: Monthly check-ins

**🟡 MILD CONCERN (0.3 - 0.5)**
- **Indicator**: Moderate stress or negative mood detected
- **Recommendation**: Weekly check-ins, monitor closely
- **Action Required**: Increased supervision and support

**🟠 MODERATE DEPRESSION (0.5 - 0.7)**
- **Indicator**: Significant negative emotional indicators
- **Recommendation**: Counseling recommended, bi-weekly assessments
- **Action Required**: Professional intervention

**🔴 CRITICAL MENTAL HEALTH (0.7 - 1.0)**
- **Indicator**: Severe depression or distress indicators
- **Recommendation**: URGENT - Immediate professional intervention required
- **Action Required**: Immediate medical/psychological support

### Assessment Accuracy

**Factors That Improve Accuracy:**
- ✅ Regular survey completion
- ✅ Honest and detailed responses
- ✅ Consistent participation over time
- ✅ Proper webcam positioning (if used)
- ✅ Quiet environment during surveys

**Factors That May Affect Accuracy:**
- ❌ Incomplete or rushed responses
- ❌ Technical issues during survey
- ❌ Inconsistent participation
- ❌ External distractions during assessment

### Acting on Assessment Results

**For Low Risk Results:**
1. **Continue monitoring** with regular surveys
2. **Maintain positive environment** and support systems
3. **Document results** for trend analysis
4. **Celebrate positive mental health** achievements

**For Medium Risk Results:**
1. **Increase check-in frequency** to weekly
2. **Provide additional support** resources
3. **Monitor for changes** in subsequent surveys
4. **Consider stress reduction** activities

**For High Risk Results:**
1. **Schedule counseling sessions** within 1-2 weeks
2. **Assign buddy system** for peer support
3. **Review workload and stress factors**
4. **Implement stress management** programs

**For Critical Risk Results:**
1. **Immediate intervention** required (same day)
2. **Contact mental health professionals**
3. **Remove from high-stress duties** temporarily
4. **Provide continuous monitoring** and support
5. **Follow emergency protocols** if necessary

### Privacy and Confidentiality

**Data Protection:**
- All assessment data is encrypted and secure
- Access limited to authorized personnel only
- Results used solely for support and intervention
- No data shared without explicit consent

**Confidentiality Guidelines:**
- Individual results should be discussed privately
- General trends can be shared with leadership
- Personal details must remain confidential
- Follow organizational privacy policies

## Reports and Analytics

The Reports and Analytics section provides comprehensive insights into mental health trends and patterns across your organization.

### Types of Reports Available

**1. Individual Soldier Reports**
- Personal mental health history
- Score trends over time
- Survey completion history
- Detailed assessment results

**2. Unit/Group Reports**
- Aggregate statistics for specific units
- Comparative analysis between groups
- Risk distribution by unit
- Performance metrics

**3. Organizational Reports**
- Overall mental health statistics
- System-wide trends and patterns
- Monthly and quarterly summaries
- Executive-level dashboards

**4. Custom Reports**
- User-defined criteria and filters
- Specific time period analysis
- Targeted demographic studies
- Special situation assessments

### Generating Standard Reports

**Step 1: Access Reports Section**
1. Navigate to **"Reports"** in the main menu
2. Choose the type of report you need

**Step 2: Configure Report Parameters**
- **Time Period**: Select date range (daily, weekly, monthly, quarterly)
- **Population**: Choose specific units, groups, or all soldiers
- **Risk Levels**: Include all or filter by specific risk categories
- **Data Points**: Select which metrics to include

**Step 3: Generate and Download**
1. Click **"Generate Report"**
2. Wait for processing (may take a few minutes for large datasets)
3. Download in preferred format (PDF for presentation, CSV for analysis)

### Understanding Report Metrics

**Key Performance Indicators (KPIs):**

**1. Survey Participation Rate**
- Percentage of soldiers completing surveys on time
- Target: >85% participation rate
- Indicates engagement with mental health monitoring

**2. Average Mental Health Score**
- Mean score across all assessed personnel
- Lower scores indicate better overall mental health
- Tracks organizational mental health trends

**3. Risk Distribution**
- Percentage breakdown by risk categories
- Shows proportion of soldiers in each risk level
- Helps resource allocation and planning

**4. Trend Analysis**
- Week-over-week or month-over-month changes
- Identifies improving or deteriorating patterns
- Enables proactive intervention planning

### Advanced Analytics

**Trend Analysis Features:**
- **Seasonal Patterns**: Identify times of year with higher stress
- **Correlation Analysis**: Find relationships between factors
- **Predictive Indicators**: Early warning signs of mental health issues
- **Intervention Effectiveness**: Measure success of support programs

**Data Visualization:**
- **Charts and Graphs**: Visual representation of trends
- **Heat Maps**: Show risk levels across different units
- **Time Series**: Track changes over extended periods
- **Comparative Analysis**: Compare different groups or time periods

### Using Reports for Decision Making

**Strategic Planning:**
1. **Resource Allocation**: Deploy counselors where most needed
2. **Training Programs**: Focus on units with higher risk scores
3. **Policy Development**: Create policies based on data insights
4. **Performance Monitoring**: Track effectiveness of interventions

**Operational Decisions:**
1. **Duty Assignments**: Consider mental health in task allocation
2. **Leave Planning**: Identify personnel needing time off
3. **Team Composition**: Balance high and low risk individuals
4. **Support Services**: Adjust counseling and support availability

### Report Security and Sharing

**Access Controls:**
- Reports contain sensitive mental health information
- Access restricted to authorized personnel only
- Different permission levels for different report types
- Audit trail tracks who accessed what reports

**Sharing Guidelines:**
- **Internal Use Only**: Reports should not leave the organization
- **Anonymized Data**: Remove personal identifiers when possible
- **Need-to-Know Basis**: Share only with those who require the information
- **Secure Transmission**: Use encrypted channels for electronic sharing

### Scheduling Automated Reports

**Setting Up Regular Reports:**
1. Configure report parameters once
2. Set delivery schedule (daily, weekly, monthly)
3. Specify recipients and delivery method
4. System automatically generates and sends reports

**Benefits of Automation:**
- Ensures consistent monitoring
- Reduces manual workload
- Provides timely insights
- Maintains regular oversight

## System Configuration

The System Configuration section allows administrators to customize SATHI settings to meet specific organizational needs.

### Accessing System Settings

![System Settings](screenshots/system_settings.jpg)

**Step 1: Navigate to Settings**
1. Click **"Settings"** in the main navigation menu
2. You'll see the System Configuration page
3. Settings are organized by category for easy navigation

### Settings Categories

**1. AI/ML Settings**
- **NLP Weight**: Influence of text analysis in final scores (default: 70%)
- **Emotion Weight**: Influence of emotion detection in final scores (default: 30%)
- **Risk Thresholds**: Score boundaries for different risk levels
- **Model Parameters**: Advanced AI model configurations

**2. System Settings**
- **Session Timeout**: How long users stay logged in (default: 60 minutes)
- **Auto-Refresh**: Dashboard update frequency
- **Database Backup**: Backup schedule and retention
- **Performance Settings**: System optimization parameters

**3. Security Settings**
- **Password Policy**: Requirements for user passwords
- **Login Attempts**: Failed login attempt limits
- **Data Encryption**: Encryption strength settings
- **Audit Logging**: Activity tracking configuration

**4. UI Settings**
- **Language Options**: Available interface languages
- **Theme Settings**: Color schemes and layouts
- **Mobile Optimization**: Mobile device specific settings
- **Accessibility**: Features for users with disabilities

### Configuring AI/ML Parameters

**Understanding Score Weights:**

The system combines two types of analysis:
- **Text Analysis (NLP)**: Examines what soldiers write in survey responses
- **Emotion Detection**: Analyzes facial expressions during surveys

**Adjusting Weights:**
1. **Increase NLP Weight** if text responses are more reliable in your context
2. **Increase Emotion Weight** if facial expression analysis provides better insights
3. **Total weight** must always equal 100%
4. **Test changes** with small groups before full implementation

**Risk Threshold Configuration:**

Default thresholds:
- **Low Risk**: 0.0 - 0.3
- **Medium Risk**: 0.3 - 0.5
- **High Risk**: 0.5 - 0.7
- **Critical Risk**: 0.7 - 1.0

**Adjusting Thresholds:**
1. **Lower thresholds** = More sensitive (identifies more people as at-risk)
2. **Higher thresholds** = Less sensitive (identifies fewer people as at-risk)
3. **Consider your organization's** risk tolerance and resources
4. **Monitor results** after changes to ensure effectiveness

### System Performance Settings

**Session Management:**
- **Timeout Duration**: Balance security with user convenience
- **Auto-Save**: How frequently data is automatically saved
- **Concurrent Sessions**: Number of simultaneous logins allowed

**Database Optimization:**
- **Backup Schedule**: Daily, weekly, or monthly backups
- **Data Retention**: How long to keep historical data
- **Archive Settings**: Moving old data to long-term storage

**Performance Monitoring:**
- **Alert Thresholds**: When to notify about system performance
- **Resource Limits**: CPU and memory usage boundaries
- **Maintenance Windows**: Scheduled downtime for updates

### Security Configuration

**Password Requirements:**
- **Minimum Length**: Number of characters required
- **Complexity**: Requirements for numbers, symbols, uppercase/lowercase
- **Expiration**: How often passwords must be changed
- **History**: Prevention of password reuse

**Access Controls:**
- **Role Definitions**: Different permission levels for different users
- **IP Restrictions**: Limit access to specific network locations
- **Two-Factor Authentication**: Additional security layer options
- **Session Security**: Encryption and protection measures

### User Interface Customization

**Language Settings:**
- **Default Language**: Primary language for the interface
- **Available Languages**: Languages offered to users
- **Translation Quality**: Accuracy settings for automatic translations
- **Cultural Adaptation**: Region-specific customizations

**Visual Customization:**
- **Color Schemes**: Organizational branding colors
- **Logo Upload**: Custom organizational logos
- **Layout Options**: Different dashboard arrangements
- **Font Settings**: Readability and accessibility options

### Webcam and Monitoring Settings

**Emotion Detection Configuration:**
- **Enable/Disable**: Turn webcam monitoring on or off
- **Detection Interval**: How often to analyze expressions (default: 30 seconds)
- **Camera Resolution**: Quality settings for video capture
- **Privacy Settings**: Data retention and usage policies

**Privacy and Consent:**
- **Consent Management**: How to obtain user permission
- **Data Usage**: What data is collected and how it's used
- **Retention Policies**: How long to keep monitoring data
- **Opt-out Options**: Allowing users to disable monitoring

### Backup and Restore Settings

**Backup Configuration:**
1. **Set backup schedule** (daily recommended for critical systems)
2. **Choose backup location** (local or cloud storage)
3. **Configure retention policy** (how many backups to keep)
4. **Test restore procedures** regularly

**Settings Backup:**
1. **Export current settings** before making changes
2. **Save configuration snapshots** for quick restore
3. **Document changes** for audit purposes
4. **Maintain change log** for troubleshooting

### Applying Configuration Changes

**Step 1: Review Changes**
1. Double-check all modified settings
2. Understand the impact of each change
3. Consider testing with a small group first

**Step 2: Apply Changes**
1. Click **"Save Settings"**
2. System will validate the configuration
3. Some changes may require system restart

**Step 3: Monitor Results**
1. Watch for immediate effects on system performance
2. Monitor user feedback and system behavior
3. Be prepared to revert changes if necessary

**Step 4: Document Changes**
1. Record what was changed and why
2. Note the date and time of changes
3. Document any observed effects
4. Update procedures and training materials

## Troubleshooting

This section helps you resolve common issues that may occur while using SATHI.

### Common Login Issues

**Problem: Cannot access the login page**

**Symptoms:**
- Browser shows "page cannot be displayed"
- Connection timeout errors
- Blank page or error messages

**Solutions:**
1. **Check Internet Connection**
   - Verify your device is connected to the internet
   - Try accessing other websites to confirm connectivity
   - Restart your router/modem if necessary

2. **Verify Website Address**
   - Double-check the URL is correct
   - Ask your IT administrator for the correct address
   - Try typing the address manually instead of using bookmarks

3. **Browser Issues**
   - Clear browser cache and cookies
   - Try a different browser (Chrome, Firefox, Safari, Edge)
   - Disable browser extensions temporarily
   - Update your browser to the latest version

**Problem: "Invalid credentials" error**

**Symptoms:**
- Login form shows error message
- Credentials are rejected
- Unable to access system despite correct information

**Solutions:**
1. **Verify Credentials**
   - Check Force ID is exactly 9 digits
   - Ensure password is typed correctly (case-sensitive)
   - Turn off Caps Lock
   - Try typing in a text editor first, then copy-paste

2. **Account Issues**
   - Contact system administrator to verify account status
   - Request password reset if necessary
   - Confirm account hasn't been disabled or expired

**Problem: Login page loads but won't respond**

**Symptoms:**
- Login button doesn't work
- Form fields don't accept input
- Page appears frozen

**Solutions:**
1. **Browser Compatibility**
   - Use a supported browser (Chrome, Firefox, Safari, Edge)
   - Update browser to latest version
   - Enable JavaScript in browser settings
   - Disable ad blockers or privacy extensions temporarily

2. **Device Issues**
   - Restart your computer or mobile device
   - Close other applications to free up memory
   - Check if device meets minimum requirements

### Dashboard and Display Issues

**Problem: Dashboard shows "No Data" or blank sections**

**Symptoms:**
- Empty statistics cards
- Missing charts or graphs
- Incomplete information display

**Solutions:**
1. **Data Loading Issues**
   - Wait a few moments for data to load
   - Refresh the page (F5 or refresh button)
   - Check internet connection stability
   - Try accessing at a different time

2. **Permission Issues**
   - Verify you have administrator access
   - Contact system administrator to check permissions
   - Ensure your account has proper access rights

**Problem: Statistics appear incorrect or outdated**

**Symptoms:**
- Numbers don't match expected values
- Data appears to be from wrong time period
- Inconsistent information across sections

**Solutions:**
1. **Cache Issues**
   - Clear browser cache and reload page
   - Use hard refresh (Ctrl+F5 on Windows, Cmd+Shift+R on Mac)
   - Try private/incognito browsing mode

2. **System Updates**
   - Wait for automatic data refresh
   - Check if system maintenance is in progress
   - Contact administrator about data synchronization

### Survey and Assessment Issues

**Problem: Surveys won't load or complete**

**Symptoms:**
- Survey page is blank
- Questions don't appear
- Submit button doesn't work

**Solutions:**
1. **Browser Issues**
   - Enable JavaScript and cookies
   - Disable browser extensions
   - Try different browser
   - Clear browser data

2. **Network Issues**
   - Check internet connection stability
   - Try different network connection
   - Wait and retry during off-peak hours

**Problem: Webcam not working during surveys**

**Symptoms:**
- "Camera access denied" messages
- Black screen where webcam should appear
- Emotion detection not functioning

**Solutions:**
1. **Permission Issues**
   - Allow camera access when prompted
   - Check browser camera permissions settings
   - Restart browser after granting permissions

2. **Hardware Issues**
   - Verify webcam is connected and working
   - Check if other applications are using camera
   - Test camera with other software
   - Try different USB port for external cameras

3. **Browser Settings**
   - Check browser camera settings
   - Ensure site has camera permissions
   - Try different browser
   - Update browser to latest version

### Report and Export Issues

**Problem: Reports won't generate or download**

**Symptoms:**
- "Report generation failed" errors
- Download doesn't start
- Empty or corrupt files

**Solutions:**
1. **Data Size Issues**
   - Reduce date range for large reports
   - Filter data to smaller subsets
   - Try generating during off-peak hours

2. **Browser Settings**
   - Check download permissions
   - Verify popup blockers aren't interfering
   - Check available disk space
   - Try different browser

**Problem: PDF reports appear blank or formatting is wrong**

**Symptoms:**
- PDF files open but show no content
- Text is cut off or overlapping
- Images missing in reports

**Solutions:**
1. **PDF Viewer Issues**
   - Update PDF reader software
   - Try opening in different PDF viewer
   - Download file instead of viewing in browser

2. **Browser Issues**
   - Update browser to latest version
   - Clear browser cache
   - Disable browser PDF plugins temporarily

### Performance Issues

**Problem: System is slow or unresponsive**

**Symptoms:**
- Pages take long time to load
- Buttons and links are slow to respond
- Timeouts and error messages

**Solutions:**
1. **Network Issues**
   - Check internet connection speed
   - Try different network connection
   - Contact IT about network performance

2. **Server Load**
   - Try accessing during off-peak hours
   - Contact administrator about server capacity
   - Wait and retry later

3. **Device Performance**
   - Close unnecessary applications
   - Restart your computer
   - Check available memory and disk space
   - Update device drivers

### Mobile Device Issues

**Problem: System doesn't work properly on mobile**

**Symptoms:**
- Layout appears broken
- Buttons are too small to tap
- Text is difficult to read

**Solutions:**
1. **Browser Issues**
   - Use mobile browser that supports modern features
   - Update mobile browser app
   - Try different mobile browser

2. **Display Settings**
   - Rotate device to landscape mode
   - Adjust text size in device settings
   - Zoom in on specific sections as needed

### Getting Additional Help

**When to Contact Support:**

1. **System-wide issues** affecting multiple users
2. **Data corruption** or loss of information
3. **Security concerns** or suspected breaches
4. **Account access** problems that persist
5. **Feature requests** or system improvements

**Information to Provide When Seeking Help:**

1. **Detailed description** of the problem
2. **Steps you were taking** when the issue occurred
3. **Error messages** (exact text or screenshots)
4. **Browser and device** information
5. **Date and time** when problem occurred
6. **Your Force ID** and role in the system

**Support Contact Methods:**

- **System Administrator**: [Contact details provided by organization]
- **IT Help Desk**: [Internal support contact]
- **Emergency Contact**: [For critical issues affecting mental health monitoring]

## Frequently Asked Questions

### General System Questions

**Q: What is SATHI and how does it work?**

A: SATHI (System for Analyzing and Tracking Human Intelligence) is a comprehensive mental health monitoring platform designed for CRPF personnel. It works by:
- Collecting survey responses from soldiers about their mental state
- Using AI to analyze text responses for emotional indicators
- Optionally monitoring facial expressions during surveys for emotion detection
- Combining all data to create comprehensive mental health assessments
- Providing early warning systems for personnel who may need support

**Q: Is my personal information safe and confidential?**

A: Yes, absolutely. SATHI implements multiple layers of security:
- All data is encrypted both in storage and transmission
- Access is strictly limited to authorized personnel only
- Individual results are kept confidential and used only for support purposes
- The system complies with privacy regulations and organizational policies
- No personal data is shared with external parties without explicit consent

**Q: Who can see my survey results?**

A: Access to individual results is strictly controlled:
- **Your immediate supervisors** may see aggregate trends but not detailed individual responses
- **Mental health professionals** have access when providing support or counseling
- **System administrators** can access data for technical support only
- **Senior leadership** sees only anonymized, aggregate statistics
- Your **personal responses** are never shared in identifiable form without your consent

### Survey and Assessment Questions

**Q: How often do I need to complete surveys?**

A: Survey frequency depends on your organization's policy, but typically:
- **Weekly surveys** are standard for regular monitoring
- **Daily surveys** may be used during high-stress periods
- **Monthly surveys** might be sufficient for low-risk populations
- **Special surveys** may be conducted after significant events
- You'll receive notifications when surveys are due

**Q: What happens if I don't complete a survey on time?**

A: Missing surveys occasionally is not a problem, but consistent non-participation may:
- Result in follow-up from supervisors to ensure you're okay
- Lead to being classified as "no data" in risk assessments
- Prevent the system from providing helpful early warning indicators
- Be noted in compliance reports if surveys are mandatory

**Q: Can I change my answers after submitting a survey?**

A: No, once submitted, survey responses cannot be modified. This ensures:
- Data integrity and consistency
- Accurate trend analysis over time
- Prevention of retroactive changes that might skew results
- Authentic capture of your mental state at the time of completion

**Q: What if I'm having technical difficulties with webcam monitoring?**

A: Webcam monitoring is optional and surveys can be completed without it:
- Grant camera permissions when prompted by your browser
- Ensure good lighting and proper camera positioning
- If technical issues persist, contact IT support
- The survey will still work without webcam functionality
- Your responses are still valuable even without emotion detection

### Scoring and Assessment Questions

**Q: How are mental health scores calculated?**

A: The system uses a sophisticated multi-factor approach:
- **Text Analysis (70% weight)**: AI analyzes your written responses for emotional indicators
- **Emotion Detection (30% weight)**: Optional facial expression analysis during surveys
- **Combined Scoring**: Both factors are weighted and combined into a single score (0-1 scale)
- **Risk Categorization**: Scores are mapped to risk levels (Low, Medium, High, Critical)
- **Trend Analysis**: Changes over time are considered for more accurate assessment

**Q: What does my mental health score mean?**

A: Scores are interpreted as follows:
- **0.0 - 0.3 (Low Risk)**: Good mental health, continue normal activities
- **0.3 - 0.5 (Medium Risk)**: Some stress indicators, increased monitoring recommended
- **0.5 - 0.7 (High Risk)**: Significant concern, counseling recommended
- **0.7 - 1.0 (Critical Risk)**: Urgent attention needed, immediate intervention required

Remember: These are indicators, not diagnoses. Professional assessment is always recommended for concerning scores.

**Q: Can the system make mistakes in assessment?**

A: Like any AI system, SATHI is highly accurate but not perfect:
- **False positives** may occasionally identify risk where none exists
- **False negatives** might miss some indicators in rare cases
- **Human oversight** is always required for important decisions
- **Professional assessment** should always accompany concerning scores
- **Multiple data points** over time provide more accurate pictures than single assessments

### Administrative Questions

**Q: How do I add new soldiers to the system?**

A: Only administrators can add new personnel:
1. Go to the Soldiers section
2. Click "Add Soldier"
3. Enter the new soldier's 9-digit Force ID
4. Set a temporary password
5. Save the new account
6. Provide login credentials to the soldier securely
7. Ensure they change their password on first login

**Q: Can I customize the survey questions?**

A: Yes, administrators have full control over questionnaires:
- Create new questionnaires with custom questions
- Edit existing questions for clarity or relevance
- Add questions in both English and Hindi
- Activate different questionnaires for different time periods
- Archive old questionnaires while preserving historical data

**Q: How do I generate reports for leadership?**

A: The system provides comprehensive reporting tools:
1. Navigate to the Reports section
2. Choose report type (individual, unit, or organizational)
3. Set date ranges and filters as needed
4. Select output format (PDF for presentations, CSV for analysis)
5. Generate and download reports
6. Schedule automatic report generation for regular delivery

### Technical Questions

**Q: What browsers are supported?**

A: SATHI works best with modern browsers:
- **Google Chrome** (recommended, version 90+)
- **Mozilla Firefox** (version 88+)
- **Safari** (version 14+)
- **Microsoft Edge** (version 90+)
- **Mobile browsers** on Android 7+ and iOS 12+

Older browsers may experience compatibility issues.

**Q: What are the minimum system requirements?**

A: SATHI has modest system requirements:
- **Computer**: Windows 10, macOS 10.14, or Linux Ubuntu 18+
- **Mobile**: Android 7.0+ or iOS 12+
- **Memory**: 4GB RAM minimum, 8GB recommended
- **Internet**: Stable broadband connection
- **Screen**: 1024x768 minimum resolution
- **Webcam**: Optional, for emotion detection features

**Q: What should I do if I encounter an error?**

A: Follow these troubleshooting steps:
1. **Note the exact error message** and when it occurred
2. **Try refreshing the page** or logging out and back in
3. **Clear your browser cache** and try again
4. **Try a different browser** to isolate the issue
5. **Contact your system administrator** with details if problems persist
6. **Provide screenshots** of any error messages to help with diagnosis

### Privacy and Security Questions

**Q: Who has access to the SATHI system?**

A: Access is strictly controlled based on role and need:
- **Administrators**: Full system access for management and configuration
- **Mental Health Professionals**: Access to assessment data for providing support
- **Unit Commanders**: Aggregate statistics for their units only
- **Soldiers**: Access to complete surveys and view their own results
- **IT Support**: Technical access for system maintenance only

**Q: How long is my data kept in the system?**

A: Data retention follows organizational and legal requirements:
- **Active data**: Kept while you're serving and for trend analysis
- **Historical trends**: Maintained for research and system improvement
- **Personal details**: Removed when you leave the organization
- **Anonymized statistics**: May be retained for long-term research
- **Backup data**: Follows the same retention policies as primary data

**Q: Can I request my data to be deleted?**

A: Yes, you have rights regarding your personal data:
- **Request to see** your data and how it's being used
- **Request corrections** to any inaccurate information
- **Request deletion** in certain circumstances
- **Opt out** of optional features like webcam monitoring
- Contact your system administrator to exercise these rights

**Q: Is the system compliant with privacy regulations?**

A: Yes, SATHI is designed to comply with applicable privacy laws:
- Data protection measures meet or exceed regulatory requirements
- Privacy by design principles are built into the system
- Regular security audits ensure ongoing compliance
- Staff training covers privacy and data protection requirements
- Incident response procedures are in place for any security issues

### Getting Help and Support

**Q: Where can I get additional training on using SATHI?**

A: Training resources are available through multiple channels:
- **This user manual** provides comprehensive guidance
- **Online training modules** may be available through your organization
- **Administrator training** for those managing the system
- **Help desk support** for technical questions
- **Mental health professionals** can provide guidance on interpreting results

**Q: Who should I contact for different types of issues?**

A: Contact the appropriate support channel based on your issue:
- **Technical problems**: IT Help Desk or System Administrator
- **Account access issues**: System Administrator
- **Survey or assessment questions**: Your supervisor or HR department
- **Mental health concerns**: Mental health professionals or counselors
- **Privacy or security concerns**: System Administrator or Privacy Officer
- **Emergency mental health situations**: Follow your organization's emergency procedures

Remember: SATHI is a tool to support mental health monitoring and early intervention. It does not replace professional mental health services, and anyone with serious concerns should seek appropriate professional help immediately.

---

## Summary

SATHI is a powerful tool designed to support the mental health and wellbeing of CRPF personnel. By following this user manual, you can effectively use all features of the system to monitor, assess, and support mental health across your organization.

**Key Takeaways:**
- SATHI provides comprehensive mental health monitoring using advanced AI technology
- The system is secure, confidential, and designed specifically for CRPF needs
- Regular use and honest participation improve the accuracy and effectiveness of assessments
- Early identification of mental health concerns enables timely intervention and support
- Professional oversight and human judgment remain essential components of mental health care

**Remember:**
- Complete surveys honestly and regularly for best results
- Seek professional help for any serious mental health concerns
- Contact support when you encounter technical difficulties
- Maintain confidentiality and respect privacy of all personnel
- Use the system as intended to support the wellbeing of all CRPF personnel

For additional support or questions not covered in this manual, contact your system administrator or designated support personnel.