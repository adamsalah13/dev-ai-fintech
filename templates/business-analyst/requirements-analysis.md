# Requirements Analysis Templates

## 🎯 Overview

This template provides comprehensive AI-driven approaches for requirements gathering, analysis, and documentation in fintech environments. It focuses on eliciting stakeholder needs, documenting functional and non-functional requirements, and ensuring regulatory compliance.

## 🔍 AI Prompt Templates

### Stakeholder Requirements Elicitation

#### Executive Stakeholder Requirements

```text
Analyze and document requirements from executive stakeholders for a fintech payment platform:

**Executive Context:**
- Company: Digital payment startup targeting SMB market
- Stage: Series A funding, 50+ employees
- Timeline: 12-month product roadmap
- Competition: Square, Stripe, PayPal
- Revenue model: Transaction fees + SaaS subscriptions

**Key Stakeholder Roles:**
- CEO: Overall vision and business outcomes
- CFO: Financial controls and revenue optimization
- CPO: Product strategy and user experience
- CTO: Technical feasibility and scalability
- Head of Compliance: Regulatory requirements

**Requirements Gathering Focus:**
1. **Business Objectives**
   - Revenue targets and growth metrics
   - Market positioning and differentiation
   - Customer acquisition and retention goals
   - Competitive advantages to develop

2. **Strategic Initiatives**
   - New market segments to enter
   - Partnership opportunities
   - Technology investments priorities
   - Risk tolerance and mitigation

3. **Success Metrics**
   - KPIs and OKRs definition
   - Performance benchmarks
   - ROI expectations
   - Timeline milestones

Please create:
- Executive interview questions
- Requirements prioritization matrix
- Business case templates
- Success criteria documentation
- Risk assessment framework
```

#### End-User Requirements Discovery

```text
Design comprehensive user requirements gathering for fintech payment application users:

**User Segments:**
- Small Business Owners (primary)
- Freelancers and Contractors
- E-commerce Merchants
- Service Providers
- Non-profit Organizations

**User Journey Focus Areas:**
1. **Onboarding Experience**
   - Account registration and verification
   - KYC/AML compliance workflows
   - Initial payment setup
   - Learning curve and training needs

2. **Core Payment Functions**
   - Transaction processing
   - Payment method management
   - Reporting and analytics
   - Integration requirements

3. **Account Management**
   - Profile and settings management
   - Security and privacy controls
   - Support and help resources
   - Mobile vs desktop preferences

**Requirements Discovery Methods:**
- User interviews and surveys
- Journey mapping workshops
- Usability testing sessions
- Competitive analysis
- Analytics and data analysis

Create detailed:
- User persona templates
- Interview guides and scripts
- Survey questionnaires
- Journey mapping templates
- Pain point analysis frameworks
```

### Functional Requirements Documentation

#### Payment Processing Requirements

```text
Document comprehensive functional requirements for payment processing system:

**Core Payment Functions:**
1. **Transaction Processing**
   - Accept credit/debit cards (Visa, MC, Amex, Discover)
   - ACH bank transfers (same-day and standard)
   - Digital wallets (Apple Pay, Google Pay, PayPal)
   - Buy-now-pay-later options
   - Cryptocurrency payments (Bitcoin, Ethereum)

2. **Payment Flow Requirements**
   - One-time payments
   - Recurring billing and subscriptions
   - Split payments and marketplace functionality
   - Refunds and partial refunds
   - Payment scheduling and future dating

3. **Security and Compliance**
   - PCI DSS Level 1 compliance
   - 3D Secure authentication
   - Fraud detection and prevention
   - Anti-money laundering (AML) checks
   - Know Your Customer (KYC) verification

**Integration Requirements:**
- RESTful API with comprehensive documentation
- Webhook notifications for real-time updates
- SDKs for popular programming languages
- Pre-built plugins for e-commerce platforms
- Mobile SDK for iOS and Android

**Performance Requirements:**
- 99.9% uptime SLA
- <200ms average response time
- 10,000+ concurrent transactions
- Global payment processing capability
- Real-time transaction status updates

Document with:
- Detailed user stories with acceptance criteria
- API specifications and data models
- Integration guides and examples
- Error handling and edge cases
- Testing scenarios and validation rules
```

#### Reporting and Analytics Requirements

```text
Define requirements for comprehensive reporting and analytics system:

**Financial Reporting:**
1. **Transaction Reports**
   - Daily, weekly, monthly transaction summaries
   - Payment method breakdown and trends
   - Geographic distribution analysis
   - Failed transaction analysis
   - Chargeback and dispute tracking

2. **Revenue Analytics**
   - Revenue forecasting and projections
   - Customer lifetime value analysis
   - Fee analysis and optimization
   - Profitability by customer segment
   - Seasonal trend analysis

3. **Compliance Reporting**
   - Regulatory filing requirements
   - Audit trail documentation
   - Risk assessment reports
   - KYC/AML compliance status
   - Data privacy compliance tracking

**Business Intelligence Features:**
- Interactive dashboards and visualizations
- Custom report builder
- Automated report scheduling
- Data export capabilities (CSV, PDF, Excel)
- API access for third-party tools

**User Experience Requirements:**
- Role-based access controls
- Mobile-responsive design
- Real-time data updates
- Drill-down capabilities
- Customizable date ranges and filters

Include:
- Report mockups and wireframes
- Data model specifications
- User permission matrices
- Performance requirements
- Integration specifications
```

### Non-Functional Requirements

#### Security and Compliance Requirements

```text
Document comprehensive security and compliance requirements for fintech platform:

**Data Security Requirements:**
1. **Encryption Standards**
   - AES-256 encryption for data at rest
   - TLS 1.3 for data in transit
   - End-to-end encryption for sensitive data
   - Key management and rotation policies
   - Hardware security module (HSM) integration

2. **Access Controls**
   - Multi-factor authentication (MFA)
   - Role-based access control (RBAC)
   - Single sign-on (SSO) integration
   - Session management and timeout
   - Privileged access management (PAM)

3. **Monitoring and Logging**
   - Security event logging (SIEM)
   - Real-time threat detection
   - Intrusion detection and prevention
   - Vulnerability scanning and assessment
   - Incident response procedures

**Regulatory Compliance:**
1. **Financial Regulations**
   - PCI DSS Level 1 certification
   - SOX compliance for public companies
   - FFIEC guidelines adherence
   - State money transmitter licenses
   - Anti-money laundering (AML) compliance

2. **Data Privacy Regulations**
   - GDPR compliance for EU customers
   - CCPA compliance for California residents
   - Data retention and deletion policies
   - Consent management systems
   - Privacy impact assessments

3. **Industry Standards**
   - ISO 27001 information security
   - SOC 2 Type II attestation
   - NIST Cybersecurity Framework
   - Open Banking standards
   - API security best practices

Provide:
- Compliance checklists and matrices
- Security control documentation
- Audit preparation guidelines
- Risk assessment templates
- Policy and procedure frameworks
```

#### Performance and Scalability Requirements

```text
Define comprehensive performance and scalability requirements:

**System Performance:**
1. **Response Time Requirements**
   - API endpoints: <200ms (95th percentile)
   - Payment processing: <3 seconds end-to-end
   - Database queries: <50ms average
   - Page load times: <2 seconds
   - Mobile app launch: <3 seconds

2. **Throughput Requirements**
   - 10,000+ transactions per second peak
   - 1 million+ daily active users
   - 50GB+ daily data processing
   - 99.9% system availability
   - <4 hours annual downtime

3. **Scalability Requirements**
   - Horizontal scaling capability
   - Auto-scaling based on demand
   - Multi-region deployment support
   - CDN integration for global performance
   - Microservices architecture support

**Resource Requirements:**
- Database: 99.9% availability, <1s query time
- Storage: Unlimited with tiered archiving
- Bandwidth: 10Gbps+ with burst capability
- Compute: Auto-scaling based on CPU/memory
- Backup: 15-minute RPO, 1-hour RTO

**Monitoring Requirements:**
- Real-time performance dashboards
- Automated alerting and notifications
- Capacity planning and forecasting
- Performance trend analysis
- SLA monitoring and reporting

Include:
- Performance testing scenarios
- Load testing requirements
- Capacity planning models
- Monitoring and alerting setup
- Performance benchmarking criteria
```

## 📋 Requirements Documentation Templates

### Business Requirements Document (BRD)

```markdown
# Business Requirements Document
## [Project Name]

### 1. Executive Summary
**Project Overview:**
[Brief description of the project and its purpose]

**Business Justification:**
[Why this project is needed and its expected business value]

**Success Criteria:**
[How success will be measured]

### 2. Business Objectives
**Primary Objectives:**
- [Objective 1 with measurable outcome]
- [Objective 2 with measurable outcome]
- [Objective 3 with measurable outcome]

**Secondary Objectives:**
- [Supporting objective 1]
- [Supporting objective 2]

### 3. Stakeholder Analysis
| Stakeholder | Role | Influence | Interest | Communication Needs |
|-------------|------|-----------|----------|-------------------|
| [Name/Role] | [Primary/Secondary] | [High/Medium/Low] | [High/Medium/Low] | [Frequency/Method] |

### 4. Business Requirements
#### 4.1 Functional Requirements
**FR-001: [Requirement Title]**
- **Description:** [Detailed description]
- **Priority:** [High/Medium/Low]
- **Source:** [Stakeholder/Document]
- **Acceptance Criteria:** [Specific criteria]

#### 4.2 Non-Functional Requirements
**NFR-001: [Requirement Title]**
- **Category:** [Performance/Security/Usability/etc.]
- **Description:** [Detailed description]
- **Metric:** [Measurable criteria]
- **Priority:** [High/Medium/Low]

### 5. Business Rules
**BR-001: [Rule Title]**
- **Description:** [Business rule description]
- **Rationale:** [Why this rule exists]
- **Impact:** [What happens if violated]

### 6. Assumptions and Constraints
**Assumptions:**
- [Assumption 1]
- [Assumption 2]

**Constraints:**
- [Constraint 1]
- [Constraint 2]

### 7. Risk Analysis
| Risk | Impact | Probability | Mitigation Strategy | Owner |
|------|--------|-------------|-------------------|-------|
| [Risk description] | [High/Medium/Low] | [High/Medium/Low] | [Mitigation approach] | [Responsible party] |

### 8. Success Metrics
**Key Performance Indicators:**
- [KPI 1]: [Current baseline] → [Target value]
- [KPI 2]: [Current baseline] → [Target value]

**Return on Investment:**
- [Expected ROI calculation and timeline]

### 9. Timeline and Milestones
| Milestone | Target Date | Dependencies | Deliverables |
|-----------|-------------|--------------|--------------|
| [Milestone 1] | [Date] | [Dependencies] | [Deliverables] |

### 10. Approval and Sign-off
| Stakeholder | Role | Signature | Date |
|-------------|------|-----------|------|
| [Name] | [Title] | | |
```

### Functional Requirements Specification (FRS)

```markdown
# Functional Requirements Specification
## [System/Module Name]

### 1. Introduction
**Purpose:**
[Purpose of this FRS document]

**Scope:**
[What is included and excluded from this specification]

**References:**
- Business Requirements Document v[X.X]
- System Architecture Document v[X.X]
- User Experience Design v[X.X]

### 2. System Overview
**System Description:**
[High-level description of the system]

**System Context:**
[How this system fits into the larger ecosystem]

### 3. Functional Requirements

#### 3.1 User Management
**FR-UM-001: User Registration**
- **Description:** The system shall allow new users to register accounts
- **Inputs:** Email, password, personal information
- **Processing:** Validate inputs, check for existing accounts, create user record
- **Outputs:** Confirmation email, user account creation
- **Preconditions:** Valid email address provided
- **Postconditions:** User account created in database
- **Priority:** High
- **Acceptance Criteria:**
  - User can register with valid email and strong password
  - System prevents duplicate registrations
  - Confirmation email sent within 30 seconds
  - Account activated upon email confirmation

#### 3.2 Payment Processing
**FR-PP-001: Process Payment**
- **Description:** The system shall process various payment methods
- **Inputs:** Payment amount, payment method, merchant details
- **Processing:** Validate payment details, authorize transaction, process payment
- **Outputs:** Transaction confirmation, receipt, status updates
- **Preconditions:** Valid payment method and sufficient funds
- **Postconditions:** Payment processed and recorded
- **Priority:** High
- **Business Rules:**
  - BR-PP-001: Minimum transaction amount is $0.50
  - BR-PP-002: Maximum transaction amount is $25,000
  - BR-PP-003: Failed transactions must be logged for audit

### 4. Data Requirements
**Data Entities:**
- User: [attributes and relationships]
- Transaction: [attributes and relationships]
- Payment Method: [attributes and relationships]

**Data Validation Rules:**
- [Rule 1]: [Description]
- [Rule 2]: [Description]

### 5. Integration Requirements
**External Systems:**
- Payment Gateway: [Integration details]
- Banking APIs: [Integration details]
- Fraud Detection: [Integration details]

**Internal Systems:**
- User Management: [Integration details]
- Reporting System: [Integration details]
- Notification Service: [Integration details]

### 6. Error Handling
**Error Categories:**
- System Errors: [How to handle]
- Validation Errors: [How to handle]
- Business Rule Violations: [How to handle]

### 7. Security Requirements
**Authentication:**
- [Authentication requirements specific to functionality]

**Authorization:**
- [Authorization requirements specific to functionality]

**Data Protection:**
- [Data protection requirements specific to functionality]

### 8. Traceability Matrix
| Requirement ID | Business Requirement | Test Case ID | Design Element |
|----------------|---------------------|--------------|----------------|
| FR-UM-001 | BR-001 | TC-UM-001 | UserService.register() |
```

## 🎯 Requirements Analysis Best Practices

### Requirements Elicitation Techniques

1. **Stakeholder Interviews**
   - Structured questionnaires
   - Open-ended discussions
   - Follow-up sessions
   - Validation interviews

2. **Workshops and Sessions**
   - Requirements gathering workshops
   - Design thinking sessions
   - Priority ranking exercises
   - Consensus building meetings

3. **Documentation Analysis**
   - Existing system documentation
   - Business process documents
   - Regulatory requirements
   - Industry standards

4. **Observation and Analysis**
   - User behavior analysis
   - Process observation
   - System usage analytics
   - Competitive analysis

### Requirements Validation Methods

1. **Review and Inspection**
   - Peer reviews
   - Stakeholder walkthroughs
   - Expert reviews
   - Checklist-based inspection

2. **Prototyping**
   - Mockups and wireframes
   - Interactive prototypes
   - Proof of concepts
   - Pilot implementations

3. **Testing and Simulation**
   - Requirements-based testing
   - Scenario simulation
   - User acceptance testing
   - Performance modeling

## 📊 Requirements Traceability

### Traceability Matrix Template

```markdown
| Business Need | Business Requirement | Functional Requirement | Design Element | Test Case | Implementation |
|---------------|---------------------|----------------------|---------------|-----------|----------------|
| [Need ID] | [BR ID] | [FR ID] | [Design ID] | [TC ID] | [Code Module] |
```

### Change Management Process

1. **Change Request Initiation**
   - Change request form
   - Impact analysis
   - Stakeholder notification
   - Initial assessment

2. **Change Evaluation**
   - Technical feasibility
   - Business impact assessment
   - Cost-benefit analysis
   - Risk evaluation

3. **Change Approval**
   - Change control board review
   - Stakeholder sign-off
   - Implementation planning
   - Communication plan

4. **Change Implementation**
   - Requirements update
   - Documentation revision
   - Testing updates
   - Deployment planning

This comprehensive requirements analysis template ensures thorough documentation and analysis of all stakeholder needs while maintaining focus on fintech-specific regulatory and business requirements.