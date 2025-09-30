# Security Testing Templates

## 🔒 Overview

This template provides comprehensive security testing methodologies for fintech applications, covering authentication, authorization, data protection, and compliance testing specific to financial technology environments.

## 🛡️ AI Prompt Templates

### Fintech Security Testing Framework

#### Comprehensive Security Test Plan

```text
Develop comprehensive security testing strategy for fintech payment platform:

**Application Architecture:**
- Frontend: React-based web application
- Backend: Node.js microservices architecture
- Database: PostgreSQL with encrypted data storage
- Payment Processing: Integration with Stripe, PayPal, and ACH networks
- Authentication: OAuth 2.0 with JWT tokens
- Infrastructure: AWS cloud hosting with load balancers
- Mobile: iOS and Android native applications

**Security Testing Scope:**

1. **Authentication and Authorization Testing**
   - Multi-factor authentication validation
   - Password policy enforcement
   - Session management security
   - Role-based access control (RBAC)
   - OAuth 2.0 implementation testing
   - JWT token security validation
   - Single sign-on (SSO) integration
   - Account lockout mechanisms

2. **Input Validation and Data Security**
   - SQL injection vulnerability testing
   - Cross-site scripting (XSS) prevention
   - Cross-site request forgery (CSRF) protection
   - Command injection testing
   - File upload security validation
   - Data sanitization verification
   - Parameter tampering detection
   - Buffer overflow testing

3. **Payment Security Testing**
   - PCI DSS compliance validation
   - Card data protection testing
   - Payment tokenization security
   - Encryption in transit validation
   - Encryption at rest verification
   - Key management security
   - Payment fraud detection testing
   - Transaction integrity validation

4. **API Security Testing**
   - API authentication mechanisms
   - Rate limiting and throttling
   - API versioning security
   - Input parameter validation
   - Response data sanitization
   - Error handling security
   - API documentation security
   - Third-party API integration security

5. **Infrastructure Security Testing**
   - Network security configuration
   - SSL/TLS implementation testing
   - Firewall configuration validation
   - Load balancer security settings
   - Database security configuration
   - Server hardening validation
   - Container security testing
   - Cloud security configuration

**Security Testing Methods:**
- Static Application Security Testing (SAST)
- Dynamic Application Security Testing (DAST)
- Interactive Application Security Testing (IAST)
- Penetration testing and ethical hacking
- Code review and manual testing
- Automated security scanning
- Compliance validation testing
- Threat modeling and risk assessment

**Compliance Requirements:**
- PCI DSS Level 1 compliance
- SOC 2 Type II requirements
- GDPR data protection validation
- CCPA privacy compliance
- GLBA safeguards rule compliance
- State data breach notification laws
- FFIEC IT examination handbook
- NIST Cybersecurity Framework

Create:
- Detailed security test plan
- Test case specifications
- Automated testing scripts
- Compliance validation checklists
- Vulnerability assessment reports
- Security testing tools and frameworks
- Risk assessment and mitigation strategies
```

#### Payment Security Testing

```text
Design comprehensive payment security testing for fintech platform:

**Payment Processing Components:**
- Credit/debit card processing
- ACH bank transfers
- Digital wallet integrations (Apple Pay, Google Pay)
- Cryptocurrency transactions
- International wire transfers
- Recurring billing systems
- Marketplace split payments

**PCI DSS Testing Requirements:**

1. **Cardholder Data Protection**
   - Primary Account Number (PAN) protection
   - Sensitive authentication data handling
   - Cardholder data storage validation
   - Data retention policy compliance
   - Secure deletion verification
   - Data masking and truncation
   - Database encryption validation

2. **Secure Network Architecture**
   - Network segmentation testing
   - Firewall configuration validation
   - DMZ security implementation
   - Network access control testing
   - Wireless network security
   - VPN security configuration
   - Network monitoring and logging

3. **Vulnerability Management**
   - Security patch management
   - Vulnerability scanning automation
   - Penetration testing procedures
   - Code review processes
   - Third-party security assessments
   - Risk assessment methodologies
   - Incident response procedures

4. **Access Control Testing**
   - User authentication mechanisms
   - Multi-factor authentication
   - Privileged access management
   - Role-based access controls
   - Password policy enforcement
   - Account provisioning/deprovisioning
   - Access review procedures

5. **Monitoring and Testing**
   - Security event logging
   - Log monitoring and analysis
   - File integrity monitoring
   - Intrusion detection systems
   - Security testing procedures
   - Compliance reporting
   - Incident response testing

**Payment Fraud Testing:**
- Card testing attack simulation
- Account takeover scenarios
- Synthetic identity fraud detection
- Velocity checking validation
- Geolocation fraud detection
- Device fingerprinting testing
- Machine learning fraud model validation

**Tokenization Testing:**
- Token generation security
- Token storage validation
- Detokenization security
- Token lifecycle management
- Token format compliance
- Cross-system token usage
- Token security standards compliance

Deliver:
- PCI DSS compliance test suite
- Payment security test cases
- Fraud detection test scenarios
- Automated security testing framework
- Compliance validation reports
```

### Authentication and Authorization Testing

#### Identity and Access Management Testing

```text
Comprehensive IAM security testing for fintech applications:

**Authentication Mechanisms:**
- Username/password authentication
- Multi-factor authentication (MFA)
- Biometric authentication (fingerprint, facial recognition)
- Hardware security keys (FIDO2/WebAuthn)
- Social login integration (OAuth providers)
- Certificate-based authentication
- Single sign-on (SSO) integration

**Authentication Testing Scenarios:**

1. **Password Security Testing**
   - Password complexity validation
   - Password history enforcement  
   - Account lockout mechanisms
   - Password reset security
   - Brute force attack protection
   - Dictionary attack prevention
   - Password encryption storage
   - Password transmission security

2. **Multi-Factor Authentication Testing**
   - SMS-based OTP validation
   - Email-based OTP testing
   - Authenticator app integration (TOTP)
   - Hardware token compatibility
   - Backup code functionality
   - MFA bypass prevention
   - MFA recovery procedures
   - MFA device management

3. **Session Management Testing**
   - Session token generation
   - Session timeout enforcement
   - Concurrent session handling
   - Session fixation prevention
   - Session hijacking protection
   - Secure cookie configuration
   - Session invalidation
   - Cross-domain session security

4. **Authorization Testing**
   - Role-based access control (RBAC)
   - Attribute-based access control (ABAC)
   - Privilege escalation prevention
   - Horizontal access control
   - Vertical access control
   - Resource-based permissions
   - Dynamic authorization decisions
   - Policy enforcement testing

**OAuth 2.0 and OpenID Connect Testing:**
- Authorization code flow security
- Implicit flow vulnerabilities
- Client authentication validation
- Scope-based access control
- Token endpoint security
- Refresh token management
- JWT token validation
- PKCE implementation testing

**API Security Testing:**
- API key authentication
- Bearer token validation
- Rate limiting enforcement
- API versioning security
- Endpoint access control
- Request/response validation
- Error handling security
- API documentation security

Create:
- Authentication test suite
- Authorization test cases
- Session management tests
- OAuth/OpenID Connect tests
- API security test scenarios
- Automated IAM testing framework
```

## 📋 Security Test Case Templates

### Authentication Test Cases

```markdown
# Authentication Security Test Cases

## Test Case: Password Policy Validation
**Test ID:** AUTH-001
**Priority:** High
**Category:** Authentication
**Compliance:** PCI DSS Requirement 8.2.3

### Test Objective
Validate that password policy requirements are properly enforced during user registration and password changes.

### Prerequisites
- Test user accounts with various privilege levels
- Access to user registration and password change functionality
- Understanding of organization's password policy requirements

### Test Data
**Valid Passwords:**
- "SecureP@ssw0rd123!" (meets all requirements)
- "MyStr0ng#Pass2024" (meets all requirements)

**Invalid Passwords:**
- "password" (too simple, no special characters)
- "12345678" (all numeric, too simple)
- "PASSWORD" (all uppercase, no special characters)
- "Pass1!" (too short)

### Test Steps
1. **Navigate to user registration page**
   - Access user registration form
   - Verify password field is present
   - Verify password confirmation field is present

2. **Test minimum length requirement**
   - Enter password with fewer characters than minimum (e.g., "Pass1!")
   - Attempt to submit form
   - **Expected Result:** Error message displayed indicating minimum length requirement

3. **Test complexity requirements**
   - Enter password without uppercase letters (e.g., "password123!")
   - Attempt to submit form
   - **Expected Result:** Error message for missing uppercase requirement
   
   - Enter password without lowercase letters (e.g., "PASSWORD123!")
   - Attempt to submit form
   - **Expected Result:** Error message for missing lowercase requirement
   
   - Enter password without numbers (e.g., "Password!")
   - Attempt to submit form
   - **Expected Result:** Error message for missing numeric requirement
   
   - Enter password without special characters (e.g., "Password123")
   - Attempt to submit form
   - **Expected Result:** Error message for missing special character requirement

4. **Test dictionary word prevention**
   - Enter common dictionary words (e.g., "Password123!")
   - Attempt to submit form
   - **Expected Result:** Error message preventing common dictionary words

5. **Test password confirmation**
   - Enter valid password in password field
   - Enter different password in confirmation field
   - Attempt to submit form
   - **Expected Result:** Error message indicating password mismatch

6. **Test successful password creation**
   - Enter valid password meeting all requirements
   - Enter same password in confirmation field
   - Submit form
   - **Expected Result:** User account created successfully

### Pass/Fail Criteria
- **Pass:** All invalid passwords are rejected with appropriate error messages
- **Pass:** Valid passwords are accepted and account creation succeeds
- **Fail:** Any invalid password is accepted or incorrect error messages are displayed

### Risk Level
**High** - Weak password policies can lead to account compromise and unauthorized access

### Remediation
If test fails:
1. Review password validation logic in application code
2. Implement proper client-side and server-side validation
3. Update error messages to clearly indicate requirements
4. Consider implementing password strength meter
5. Review and update password policy documentation

---

## Test Case: Multi-Factor Authentication Bypass
**Test ID:** AUTH-002
**Priority:** Critical
**Category:** Authentication
**Compliance:** PCI DSS Requirement 8.3

### Test Objective
Verify that multi-factor authentication cannot be bypassed through direct URL access or session manipulation.

### Prerequisites
- User account with MFA enabled
- Understanding of application URL structure
- Web proxy tool (e.g., Burp Suite, OWASP ZAP)

### Test Steps
1. **Attempt direct URL access after first factor**
   - Complete username/password authentication
   - Before completing MFA, attempt to access protected resources directly
   - Try various protected URLs (dashboard, account settings, transaction pages)
   - **Expected Result:** Access denied, redirected to MFA completion page

2. **Test session manipulation**
   - Complete first factor authentication
   - Use web proxy to capture and modify session tokens
   - Attempt to set session as fully authenticated
   - Try to access protected resources
   - **Expected Result:** Access denied, MFA completion still required

3. **Test MFA step skipping**
   - Complete first factor authentication
   - Attempt to skip MFA by manipulating form parameters
   - Try submitting empty or invalid MFA codes
   - **Expected Result:** MFA validation enforced, invalid codes rejected

4. **Test parallel session attack**
   - Start authentication process in multiple browser sessions
   - Complete first factor in session 1
   - Complete MFA in session 2 with different account
   - Attempt to use session 1 after session 2 MFA completion
   - **Expected Result:** Each session maintains independent MFA state

### Pass/Fail Criteria
- **Pass:** All attempts to bypass MFA are prevented
- **Pass:** Users cannot access protected resources without completing MFA
- **Fail:** Any method successfully bypasses MFA requirement

### Risk Level
**Critical** - MFA bypass can lead to complete account compromise

---

## Test Case: SQL Injection in Payment Forms
**Test ID:** SEC-001
**Priority:** Critical
**Category:** Input Validation
**Compliance:** PCI DSS Requirement 6.5.1

### Test Objective
Validate that payment-related forms properly sanitize input to prevent SQL injection attacks.

### Prerequisites
- Access to payment forms (amount, description, recipient fields)
- Understanding of SQL injection techniques
- Database error monitoring capabilities

### Test Payloads
**Basic SQL Injection:**
- `' OR '1'='1`
- `'; DROP TABLE payments; --`
- `' UNION SELECT * FROM users --`

**Time-based Blind SQL Injection:**
- `'; WAITFOR DELAY '00:00:05' --`
- `' AND (SELECT SLEEP(5)) --`

**Boolean-based Blind SQL Injection:**
- `' AND 1=1 --`
- `' AND 1=2 --`

### Test Steps
1. **Test payment amount field**
   - Enter SQL injection payloads in amount field
   - Submit payment form
   - Monitor for database errors or unexpected behavior
   - **Expected Result:** Input sanitized, no SQL execution

2. **Test payment description field**
   - Enter SQL injection payloads in description field
   - Submit payment form
   - Check if malicious SQL is executed
   - **Expected Result:** Input properly escaped and sanitized

3. **Test recipient information fields**
   - Enter SQL injection payloads in recipient name/email fields
   - Submit payment form
   - Monitor database logs for injection attempts
   - **Expected Result:** All input properly validated and sanitized

4. **Test search and filter functions**
   - Use SQL injection payloads in transaction search
   - Test date range filters with malicious input
   - Test sorting parameters with injection attempts
   - **Expected Result:** All search functionality resistant to injection

### Pass/Fail Criteria
- **Pass:** All SQL injection attempts are prevented
- **Pass:** No database errors or unexpected behavior observed
- **Fail:** Any successful SQL injection or database error messages

### Risk Level
**Critical** - SQL injection can lead to data breach and financial fraud
```

### API Security Test Cases

```markdown
# API Security Test Cases

## Test Case: API Authentication Bypass
**Test ID:** API-001
**Priority:** Critical
**Category:** API Security
**Compliance:** PCI DSS Requirement 7.1

### Test Objective
Verify that API endpoints properly validate authentication tokens and prevent unauthorized access.

### Prerequisites
- API documentation and endpoint list
- Valid API authentication tokens
- HTTP testing tool (Postman, curl, etc.)

### Test Steps
1. **Test unauthenticated requests**
   - Send requests to protected endpoints without authentication headers
   - Try various HTTP methods (GET, POST, PUT, DELETE)
   - **Expected Result:** 401 Unauthorized response

2. **Test invalid token formats**
   - Send requests with malformed JWT tokens
   - Use expired authentication tokens
   - Try random string values as tokens
   - **Expected Result:** 401 Unauthorized response with proper error messages

3. **Test token manipulation**
   - Modify JWT payload data (user ID, permissions)
   - Alter token signature
   - Change token expiration time
   - **Expected Result:** Token validation fails, access denied

4. **Test privilege escalation**
   - Use regular user token to access admin endpoints
   - Attempt to modify other users' data
   - Try to access higher privilege resources
   - **Expected Result:** 403 Forbidden response

### Pass/Fail Criteria
- **Pass:** All unauthorized requests are properly rejected
- **Pass:** Appropriate HTTP status codes and error messages
- **Fail:** Any unauthorized access succeeds

---

## Test Case: API Rate Limiting
**Test ID:** API-002
**Priority:** High
**Category:** API Security
**Compliance:** Security Best Practices

### Test Objective
Validate that API endpoints implement proper rate limiting to prevent abuse and DoS attacks.

### Prerequisites
- API rate limiting configuration details
- Automated testing tool capable of high-volume requests
- Valid authentication credentials

### Test Steps
1. **Identify rate limit thresholds**
   - Review API documentation for rate limits
   - Send normal volume of requests to establish baseline
   - Monitor response headers for rate limit information

2. **Test rate limit enforcement**
   - Send requests at the maximum allowed rate
   - Gradually increase request frequency
   - Monitor for rate limit responses (429 Too Many Requests)
   - **Expected Result:** Rate limiting activated at defined threshold

3. **Test rate limit recovery**
   - Trigger rate limiting
   - Wait for rate limit window to reset
   - Resume normal request patterns
   - **Expected Result:** Normal access restored after reset period

4. **Test distributed rate limiting**
   - Use multiple IP addresses or user accounts
   - Test if rate limits are per-user, per-IP, or global
   - Attempt to circumvent limits through distribution
   - **Expected Result:** Rate limits properly enforced per defined scope

### Pass/Fail Criteria
- **Pass:** Rate limiting properly prevents excessive requests
- **Pass:** Appropriate error messages and recovery mechanisms
- **Fail:** Rate limiting can be bypassed or is ineffective
```

## 📈 Security Testing Automation

### Automated Security Testing Framework

```javascript
// Example: Automated Authentication Testing with Jest and Puppeteer

const puppeteer = require('puppeteer');
const { expect } = require('@jest/globals');

describe('Authentication Security Tests', () => {
  let browser, page;
  
  beforeAll(async () => {
    browser = await puppeteer.launch();
    page = await browser.newPage();
  });
  
  afterAll(async () => {
    await browser.close();
  });
  
  test('Should enforce password complexity requirements', async () => {
    await page.goto('https://app.example.com/register');
    
    // Test weak password rejection
    await page.type('#password', 'weak');
    await page.type('#confirmPassword', 'weak');
    await page.click('#submitButton');
    
    const errorMessage = await page.$eval('.error-message', el => el.textContent);
    expect(errorMessage).toContain('Password must meet complexity requirements');
  });
  
  test('Should prevent SQL injection in login form', async () => {
    await page.goto('https://app.example.com/login');
    
    // Attempt SQL injection
    await page.type('#username', "admin'; DROP TABLE users; --");
    await page.type('#password', 'password');
    await page.click('#loginButton');
    
    // Should receive normal login error, not database error
    const response = await page.waitForResponse(response => 
      response.url().includes('/api/login') && response.status() !== 200
    );
    
    expect(response.status()).toBe(401);
    
    // Verify no database errors in response
    const responseText = await response.text();
    expect(responseText).not.toContain('SQL');
    expect(responseText).not.toContain('database');
  });
  
  test('Should enforce MFA for protected resources', async () => {
    // Complete first factor authentication
    await page.goto('https://app.example.com/login');
    await page.type('#username', 'testuser@example.com');
    await page.type('#password', 'ValidPassword123!');
    await page.click('#loginButton');
    
    // Wait for MFA prompt
    await page.waitForSelector('#mfaCode');
    
    // Attempt to access protected resource without completing MFA
    await page.goto('https://app.example.com/dashboard');
    
    // Should be redirected back to MFA page
    const currentUrl = page.url();
    expect(currentUrl).toContain('/mfa');
  });
});
```

### Security Testing Pipeline Integration

```yaml
# Example: CI/CD Security Testing Pipeline (GitHub Actions)
name: Security Testing Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  security-tests:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        
    - name: Install dependencies
      run: npm ci
      
    - name: Run SAST (Static Analysis)
      run: |
        npm install -g @microsoft/applicationinsights
        npm run security:sast
        
    - name: Run Dependency Security Scan
      run: |
        npm audit --audit-level=moderate
        npm install -g retire
        retire --path ./
        
    - name: Start Application for DAST
      run: |
        npm run build
        npm start &
        sleep 30
        
    - name: Run DAST (Dynamic Analysis)
      run: |
        docker run -t owasp/zap2docker-stable zap-baseline.py \
          -t http://localhost:3000 \
          -r security-report.html
          
    - name: Run Custom Security Tests
      run: npm run test:security
      
    - name: Upload Security Reports
      uses: actions/upload-artifact@v3
      if: always()
      with:
        name: security-reports
        path: |
          security-report.html
          test-results/
```

This comprehensive security testing template ensures thorough coverage of authentication, authorization, input validation, and payment security while providing practical test cases and automation frameworks for fintech applications.
