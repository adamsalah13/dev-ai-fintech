# Review and Validation Prompts

## 🎯 Overview

This collection provides AI prompts for review and validation processes across different team roles in the PayFlow fintech application development. These prompts help ensure quality, consistency, and compliance across all deliverables.

## 📋 Requirements Review Prompts

### Business Requirements Validation

#### Stakeholder Alignment Review
```
Review these business requirements for completeness and stakeholder alignment:

**Requirements:**
[INSERT REQUIREMENTS DOCUMENT]

**Stakeholders:**
[INSERT STAKEHOLDER LIST]

**Business Context:**
[INSERT BUSINESS CONTEXT]

Please evaluate:
1. Completeness and clarity of requirements
2. Potential conflicts or contradictions
3. Missing stakeholder perspectives
4. Regulatory compliance considerations (fintech)
5. Implementation feasibility concerns
6. Success criteria and acceptance criteria
7. Dependencies and assumptions
8. Risk factors and mitigation needs

Provide feedback formatted as actionable recommendations with priority levels.
```

#### User Story Quality Review
```
Review these user stories for quality and implementability:

**User Stories:**
[INSERT USER STORIES]

**Acceptance Criteria:**
[INSERT ACCEPTANCE CRITERIA]

Please assess:
1. Story clarity and completeness
2. INVEST criteria compliance (Independent, Negotiable, Valuable, Estimable, Small, Testable)
3. Acceptance criteria coverage
4. Edge cases and error scenarios
5. Security and compliance considerations
6. Dependencies between stories
7. Testability and QA requirements
8. Business value alignment

Format feedback as specific improvement suggestions for each story.
```

## 💻 Code Review Prompts

### Comprehensive Code Review

#### Security and Compliance Review
```
Perform a comprehensive security review of this fintech application code:

**Code:**
[INSERT CODE]

**Context:**
- Application: PayFlow payment processing
- Compliance Requirements: PCI DSS, GDPR, SOX
- Security Standards: OWASP Top 10

Please review for:
1. **Security Vulnerabilities:**
   - SQL injection prevention
   - XSS protection
   - Authentication and authorization
   - Input validation and sanitization
   - Encryption of sensitive data

2. **Fintech Compliance:**
   - PCI DSS data handling requirements
   - GDPR privacy considerations
   - Audit trail logging
   - Data retention policies

3. **Code Quality:**
   - Error handling and edge cases
   - Performance considerations
   - Memory management
   - Resource cleanup

4. **Architecture Compliance:**
   - Follows established patterns
   - Proper separation of concerns
   - API design consistency

Provide specific findings with severity levels and remediation recommendations.
```

#### Performance and Scalability Review
```
Review this code for performance and scalability in a fintech context:

**Code:**
[INSERT CODE]

**Expected Load:**
- Concurrent users: [NUMBER]
- Transactions per second: [NUMBER]
- Data volume: [SIZE]

**Performance Requirements:**
- API response time: <200ms
- Database query time: <50ms
- Memory usage: <512MB per instance

Please analyze:
1. **Performance Issues:**
   - Algorithmic complexity
   - Database query efficiency
   - Memory usage patterns
   - Network call optimization

2. **Scalability Concerns:**
   - Resource utilization
   - Bottleneck identification
   - Horizontal scaling readiness
   - State management

3. **Fintech-Specific Considerations:**
   - Transaction consistency
   - Data integrity under load
   - Audit logging performance
   - Regulatory reporting efficiency

4. **Optimization Recommendations:**
   - Code improvements
   - Architecture changes
   - Caching strategies
   - Database optimizations

Format as performance analysis report with actionable recommendations.
```

### AI-Generated Code Review

#### AI Code Validation
```
Review this AI-generated code for quality and correctness:

**AI Tool Used:** [INSERT TOOL NAME]
**Generation Context:** [INSERT CONTEXT/PROMPT USED]
**Generated Code:**
[INSERT CODE]

**Intended Functionality:**
[INSERT WHAT THE CODE SHOULD DO]

Please validate:
1. **Functional Correctness:**
   - Does the code meet requirements?
   - Are edge cases handled?
   - Is error handling appropriate?
   - Are business rules implemented correctly?

2. **Code Quality:**
   - Follows coding standards and conventions
   - Proper naming and documentation
   - Maintainability and readability
   - Test coverage adequacy

3. **Security Considerations:**
   - Input validation
   - Authentication/authorization
   - Data protection
   - Vulnerability prevention

4. **Integration Compatibility:**
   - Matches existing patterns
   - API consistency
   - Database schema compliance
   - Service integration correctness

5. **AI Generation Quality:**
   - Appropriateness of generated solution
   - Overcomplexity or oversimplification
   - Missing business context
   - Need for human refinement

Provide specific feedback on what to keep, modify, or replace.
```

## 🧪 Testing Review Prompts

### Test Strategy Review

#### Test Coverage Analysis
```
Review the test strategy and coverage for this feature:

**Feature:** [INSERT FEATURE DESCRIPTION]
**Test Strategy:** [INSERT TEST STRATEGY]
**Test Cases:** [INSERT TEST CASES]
**Coverage Report:** [INSERT COVERAGE DATA]

Please evaluate:
1. **Test Coverage Completeness:**
   - Unit test coverage (target: >80%)
   - Integration test scenarios
   - End-to-end test coverage
   - Edge cases and error conditions

2. **Fintech-Specific Testing:**
   - Payment processing scenarios
   - Fraud detection testing
   - Compliance validation tests
   - Security vulnerability tests

3. **Test Quality:**
   - Test case clarity and maintainability
   - Data setup and teardown
   - Mock usage and test isolation
   - Performance test inclusion

4. **Risk-Based Testing:**
   - High-risk scenario coverage
   - Business-critical path testing
   - Regulatory compliance testing
   - Data integrity validation

5. **Automation Strategy:**
   - Automated vs manual test balance
   - CI/CD integration
   - Test execution efficiency
   - Maintenance overhead

Provide recommendations for improving test coverage and quality.
```

#### Security Testing Review
```
Review the security testing approach for the PayFlow application:

**Security Test Plan:** [INSERT TEST PLAN]
**Test Cases:** [INSERT SECURITY TEST CASES]
**Tools Used:** [INSERT SECURITY TESTING TOOLS]

Please assess:
1. **OWASP Top 10 Coverage:**
   - Injection vulnerabilities
   - Broken authentication
   - Sensitive data exposure
   - XML external entities (XXE)
   - Broken access control
   - Security misconfigurations
   - Cross-site scripting (XSS)
   - Insecure deserialization
   - Known vulnerable components
   - Insufficient logging/monitoring

2. **Fintech-Specific Security Tests:**
   - Payment data protection
   - Transaction integrity
   - Fraud detection validation
   - Compliance control testing

3. **Test Implementation Quality:**
   - Test scenario realism
   - Attack vector coverage
   - False positive management
   - Result interpretation accuracy

4. **Integration with Development:**
   - Security testing in CI/CD
   - Developer security training validation
   - Security requirement traceability

Provide specific recommendations for enhancing security test coverage.
```

## 📚 Documentation Review Prompts

### Technical Documentation Review

#### API Documentation Review
```
Review this API documentation for completeness and usability:

**API Documentation:** [INSERT API DOCS]
**OpenAPI Specification:** [INSERT SPEC IF AVAILABLE]
**Target Audience:** [INSERT AUDIENCE]

Please evaluate:
1. **Completeness:**
   - All endpoints documented
   - Request/response examples
   - Error codes and messages
   - Authentication requirements
   - Rate limiting information

2. **Clarity and Usability:**
   - Clear descriptions and explanations
   - Code examples in multiple languages
   - Getting started guide
   - Common use case scenarios

3. **Technical Accuracy:**
   - Matches actual API behavior
   - Correct parameter types and formats
   - Valid example requests/responses
   - Up-to-date endpoint information

4. **Fintech Context:**
   - Compliance requirements explained
   - Security considerations highlighted
   - Data sensitivity warnings
   - Regulatory context provided

5. **Developer Experience:**
   - Easy to navigate structure
   - Searchable content
   - Interactive examples
   - Troubleshooting guidance

Provide specific improvements to enhance documentation quality and developer adoption.
```

#### User Guide Review
```
Review this user guide for clarity and effectiveness:

**User Guide:** [INSERT USER GUIDE]
**Target Users:** [INSERT USER TYPES]
**Use Cases:** [INSERT PRIMARY USE CASES]

Please assess:
1. **Content Quality:**
   - Step-by-step clarity
   - Screenshot relevance and quality
   - Comprehensive workflow coverage
   - Troubleshooting guidance

2. **User Experience:**
   - Logical information architecture
   - Easy navigation and search
   - Progressive disclosure of complexity
   - Multiple learning styles accommodation

3. **Completeness:**
   - All major features covered
   - Edge cases and exceptions
   - Error recovery procedures
   - Advanced usage scenarios

4. **Accessibility:**
   - Clear language and terminology
   - Visual aid effectiveness
   - Mobile-friendly formatting
   - Accessibility compliance

5. **Maintenance Considerations:**
   - Version control integration
   - Update process efficiency
   - Content freshness indicators
   - User feedback incorporation

Recommend specific improvements for user adoption and satisfaction.
```

## 🚀 Deployment Review Prompts

### Infrastructure Review

#### Deployment Readiness Review
```
Review the deployment readiness for this PayFlow feature:

**Feature:** [INSERT FEATURE]
**Deployment Plan:** [INSERT DEPLOYMENT PLAN]
**Infrastructure Changes:** [INSERT INFRASTRUCTURE CHANGES]
**Rollback Plan:** [INSERT ROLLBACK PLAN]

Please evaluate:
1. **Deployment Preparation:**
   - Environment configuration completeness
   - Database migration testing
   - Dependency management
   - Configuration management

2. **Risk Assessment:**
   - Potential failure points
   - Impact on existing functionality
   - Data integrity risks
   - Performance impact

3. **Monitoring and Observability:**
   - Logging implementation
   - Metrics collection
   - Alerting configuration
   - Dashboard readiness

4. **Fintech Compliance:**
   - Audit trail preservation
   - Regulatory requirement compliance
   - Data security during deployment
   - Change control documentation

5. **Rollback and Recovery:**
   - Rollback procedure testing
   - Data recovery capabilities
   - Service restoration time
   - Communication plan

6. **Post-Deployment Validation:**
   - Smoke test procedures
   - Performance validation
   - Security verification
   - User acceptance testing

Provide go/no-go recommendations with specific risk mitigation strategies.
```

### Security Deployment Review

#### Security Configuration Review
```
Review the security configuration for production deployment:

**Security Configuration:** [INSERT SECURITY CONFIG]
**Environment:** [INSERT ENVIRONMENT DETAILS]
**Compliance Requirements:** PCI DSS, GDPR, SOX

Please validate:
1. **Access Controls:**
   - Authentication mechanisms
   - Authorization policies
   - Role-based access control
   - Service-to-service authentication

2. **Data Protection:**
   - Encryption at rest configuration
   - Encryption in transit setup
   - Key management practices
   - Sensitive data handling

3. **Network Security:**
   - Firewall configurations
   - Network segmentation
   - VPN setup
   - Load balancer security

4. **Monitoring and Logging:**
   - Security event logging
   - Audit trail completeness
   - Anomaly detection setup
   - Incident response integration

5. **Compliance Validation:**
   - PCI DSS requirement coverage
   - GDPR privacy controls
   - SOX change control compliance
   - Regulatory reporting readiness

6. **Vulnerability Management:**
   - Security scanning integration
   - Patch management process
   - Dependency vulnerability tracking
   - Incident response procedures

Identify security risks and provide remediation recommendations before deployment.
```

## 📊 Cross-Functional Review Prompts

### End-to-End Feature Review

#### Comprehensive Feature Validation
```
Conduct a comprehensive end-to-end review of this completed feature:

**Feature:** [INSERT FEATURE DESCRIPTION]
**Business Requirements:** [INSERT REQUIREMENTS]
**Implementation:** [INSERT IMPLEMENTATION SUMMARY]
**Test Results:** [INSERT TEST RESULTS]
**Documentation:** [INSERT DOCUMENTATION]

Please review from all perspectives:

1. **Business Alignment:**
   - Requirements fulfillment
   - Business value delivery
   - Stakeholder satisfaction
   - Success criteria achievement

2. **Technical Implementation:**
   - Code quality and maintainability
   - Architecture compliance
   - Performance and scalability
   - Security and compliance

3. **Quality Assurance:**
   - Test coverage and results
   - Bug resolution status
   - User acceptance testing
   - Performance validation

4. **Operational Readiness:**
   - Deployment preparation
   - Monitoring and alerting
   - Support documentation
   - Incident response procedures

5. **User Experience:**
   - Usability and accessibility
   - Documentation quality
   - Training material adequacy
   - Support process readiness

6. **Compliance and Risk:**
   - Regulatory requirement compliance
   - Security vulnerability assessment
   - Data privacy protection
   - Audit trail completeness

Provide an overall feature readiness assessment with recommendations for any remaining issues.
```

### Release Review

#### Production Release Review
```
Review this release package for production readiness:

**Release:** [INSERT RELEASE VERSION]
**Features Included:** [INSERT FEATURE LIST]
**Bug Fixes:** [INSERT BUG FIXES]
**Technical Changes:** [INSERT TECHNICAL CHANGES]

**Release Criteria:**
- All tests passing
- Security review completed
- Performance benchmarks met
- Documentation updated
- Stakeholder approval obtained

Please validate:
1. **Feature Completeness:**
   - All planned features implemented
   - Acceptance criteria met
   - Business stakeholder approval
   - User acceptance testing completed

2. **Quality Gates:**
   - Code review completion
   - Test coverage requirements met
   - Security vulnerability resolution
   - Performance benchmark achievement

3. **Documentation and Communication:**
   - Release notes accuracy
   - User documentation updates
   - Training material preparation
   - Support team readiness

4. **Operational Readiness:**
   - Deployment procedure validation
   - Monitoring configuration
   - Rollback plan testing
   - Support escalation procedures

5. **Compliance and Audit:**
   - Change control documentation
   - Audit trail completeness
   - Regulatory requirement validation
   - Risk assessment completion

6. **Post-Release Planning:**
   - Success metrics monitoring
   - User feedback collection
   - Issue triage procedures
   - Next release planning

Provide final go/no-go recommendation with any conditions that must be met before release.
```

## 🔄 Continuous Improvement Review

### Process Review Prompts

#### Workflow Effectiveness Review
```
Review the effectiveness of our AI-enhanced development workflows:

**Current Workflows:** [INSERT WORKFLOW DESCRIPTIONS]
**AI Tools Used:** [INSERT AI TOOLS]
**Team Feedback:** [INSERT TEAM FEEDBACK]
**Metrics:** [INSERT PERFORMANCE METRICS]

Please assess:
1. **Workflow Efficiency:**
   - Time savings achieved
   - Quality improvements
   - Bottleneck identification
   - Resource utilization

2. **AI Tool Effectiveness:**
   - Tool adoption rates
   - Quality of AI-generated content
   - Time savings per tool
   - User satisfaction levels

3. **Collaboration Quality:**
   - Cross-team communication
   - Handoff effectiveness
   - Shared understanding
   - Conflict resolution

4. **Quality Outcomes:**
   - Defect rates
   - Customer satisfaction
   - Performance improvements
   - Compliance adherence

5. **Team Satisfaction:**
   - Job satisfaction levels
   - Skill development
   - Work-life balance
   - Innovation opportunities

6. **Continuous Improvement:**
   - Learning and adaptation
   - Process evolution
   - Tool optimization
   - Knowledge sharing

Recommend specific improvements to enhance workflow effectiveness and team satisfaction.
```

---

These review prompts should be customized for your specific project context and team needs. Regular refinement based on review outcomes will improve their effectiveness over time.