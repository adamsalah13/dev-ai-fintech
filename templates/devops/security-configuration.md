# Security Configuration Templates

## 🔒 Overview

This template provides comprehensive security configuration approaches for DevOps environments in fintech applications, covering infrastructure security, compliance automation, and security monitoring specific to financial technology deployments.

## 🛡️ AI Prompt Templates

### Infrastructure Security Hardening

#### Cloud Security Configuration

```text
Develop comprehensive cloud security configuration for fintech infrastructure:

**Cloud Environment:**
- Provider: AWS/Azure/GCP
- Services: EC2/ECS/EKS, S3, RDS, Lambda, API Gateway
- Compliance: PCI DSS, SOC 2, ISO 27001, GDPR
- Region: Multi-region deployment with disaster recovery
- Network: VPC with public/private subnets, security groups, NACLs

**Security Configuration Categories:**

1. **Identity and Access Management (IAM)**
   - Least privilege principle implementation
   - Multi-factor authentication (MFA) enforcement
   - Role-based access control (RBAC) setup
   - Service account management and rotation
   - Access key rotation policies
   - Session timeout and idle timeout settings
   - Audit logging for all IAM activities

2. **Network Security**
   - VPC design with proper segmentation
   - Security group configurations (least privilege)
   - Network ACLs for subnet protection
   - VPN and Direct Connect security
   - Web Application Firewall (WAF) setup
   - DDoS protection (AWS Shield, Azure DDoS Protection)
   - Network traffic encryption (TLS 1.3)

3. **Data Protection**
   - Encryption at rest for all data stores
   - Encryption in transit (TLS everywhere)
   - Key management service (KMS) configuration
   - Database security (RDS security groups, encryption)
   - S3 bucket policies and encryption
   - Backup encryption and secure storage
   - Data classification and labeling

4. **Compute Security**
   - EC2 instance hardening (CIS benchmarks)
   - Container security (image scanning, runtime protection)
   - Serverless function security (Lambda layers, VPC config)
   - Auto-scaling group security configurations
   - Instance metadata service protection
   - SSH/RDP access restrictions
   - Patch management automation

5. **Monitoring and Logging**
   - CloudTrail configuration for API activity logging
   - VPC Flow Logs for network traffic analysis
   - CloudWatch alarms for security events
   - GuardDuty for threat detection
   - Config Rules for compliance monitoring
   - Security Hub for centralized security findings
   - Log aggregation and retention policies

6. **Compliance Automation**
   - Automated compliance checks
   - Security group drift detection
   - Encryption compliance validation
   - Access control policy validation
   - Vulnerability scanning automation
   - Incident response automation
   - Audit report generation

**Fintech-Specific Security Requirements:**
- PCI DSS Level 1 compliance for payment processing
- Financial data encryption standards (AES-256)
- Audit trail requirements for financial transactions
- Geographic data residency requirements
- Regulatory reporting automation
- Customer data protection (PII, PHI)
- Business continuity and disaster recovery security

Create:
- Infrastructure as Code security templates
- Automated security validation scripts
- Compliance monitoring dashboards
- Incident response playbooks
- Security configuration documentation
- Audit and compliance reporting framework
```

#### Container Security Configuration

```text
Design comprehensive container security configuration for fintech microservices:

**Container Platform:**
- Kubernetes (EKS/GKE/AKS) or ECS Fargate
- Container registry: ECR/GCR/ACR
- Image scanning: Trivy, Clair, or native cloud scanning
- Runtime security: Falco, Sysdig, or Aqua Security
- Service mesh: Istio with mTLS
- CI/CD integration: Security scanning in pipelines

**Container Security Layers:**

1. **Image Security**
   - Base image selection (distroless, minimal images)
   - Vulnerability scanning before deployment
   - Image signing and verification
   - SBOM (Software Bill of Materials) generation
   - License compliance checking
   - Image hardening and optimization

2. **Build Security**
   - Secure CI/CD pipeline configuration
   - Dependency vulnerability scanning
   - Code signing and integrity checks
   - Build environment security
   - Artifact repository security
   - Supply chain security validation

3. **Runtime Security**
   - Pod security policies implementation
   - Network policies for pod communication
   - Service account token management
   - Secret management (Vault, AWS Secrets Manager)
   - Runtime vulnerability scanning
   - Behavioral anomaly detection
   - Container drift detection

4. **Orchestration Security**
   - Kubernetes RBAC configuration
   - API server security hardening
   - etcd encryption and access control
   - Node security configuration
   - Cluster network security
   - Ingress controller security
   - Certificate management automation

5. **Service Mesh Security**
   - Mutual TLS (mTLS) implementation
   - Traffic encryption between services
   - Identity and access management
   - Policy enforcement (authorization, rate limiting)
   - Observability and monitoring
   - Zero-trust networking
   - Service-to-service authentication

**Fintech Container Security Requirements:**
- Payment data isolation and encryption
- Regulatory compliance (PCI DSS container requirements)
- Financial transaction audit trails
- High availability and failover security
- Multi-tenant isolation
- Sensitive data handling in containers
- Container lifecycle security

Create:
- Kubernetes security policies
- Container image security scanning
- Service mesh configuration templates
- Runtime security monitoring setup
- Compliance validation scripts
- Incident response procedures for containers
```

### Security Monitoring and Compliance

#### Security Information and Event Management (SIEM)

```text
Implement comprehensive SIEM solution for fintech security monitoring:

**SIEM Components:**
- Log aggregation: ELK Stack, Splunk, or cloud-native SIEM
- Threat detection: Correlation rules and machine learning
- Alert management: Automated alerting and escalation
- Compliance reporting: Automated report generation
- Forensic analysis: Incident investigation capabilities
- Integration: API integrations with security tools

**Security Monitoring Domains:**

1. **Network Security Monitoring**
   - Firewall log analysis and correlation
   - IDS/IPS alert processing
   - DDoS attack detection and mitigation
   - VPN and remote access monitoring
   - DNS security monitoring
   - SSL/TLS handshake monitoring

2. **Host and Endpoint Security**
   - Server log aggregation and analysis
   - Endpoint detection and response (EDR)
   - File integrity monitoring
   - Process and user behavior analytics
   - Malware detection and response
   - Patch management monitoring

3. **Application Security Monitoring**
   - Web application firewall (WAF) logs
   - API security monitoring
   - Database activity monitoring
   - Authentication and authorization logs
   - Application error and exception monitoring
   - Performance and availability monitoring

4. **Cloud Security Monitoring**
   - CloudTrail/Cloud Audit Logs analysis
   - Configuration compliance monitoring
   - Resource access and permission monitoring
   - Storage and data access monitoring
   - Serverless function monitoring
   - Container orchestration security

5. **Financial Transaction Monitoring**
   - Payment processing security monitoring
   - Fraud detection alert correlation
   - Transaction anomaly detection
   - Regulatory reporting compliance
   - AML/KYC monitoring integration
   - PCI DSS compliance monitoring

**SIEM Implementation:**
- Data sources and log collection
- Parsing and normalization rules
- Correlation and alerting rules
- Dashboard and visualization setup
- Report generation automation
- Integration with incident response
- Performance and scalability tuning

Create:
- SIEM configuration templates
- Security monitoring dashboards
- Alert and response procedures
- Compliance reporting automation
- Forensic investigation playbooks
- SIEM maintenance and tuning procedures
```

## 📋 Security Configuration Templates

### AWS Security Configuration Template

```yaml
# AWS Security Configuration Template
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Fintech Security Configuration Template'

Parameters:
  Environment:
    Type: String
    AllowedValues: [dev, staging, prod]
    Default: dev

Resources:
  # VPC Configuration
  VPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsHostnames: true
      EnableDnsSupport: true
      Tags:
        - Key: Name
          Value: !Sub '${Environment}-vpc'
        - Key: Environment
          Value: !Ref Environment

  # Security Groups
  ApplicationSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Security group for application servers
      VpcId: !Ref VPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 443
          ToPort: 443
          CidrIp: 0.0.0.0/0
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0
      SecurityGroupEgress:
        - IpProtocol: -1
          CidrIp: 0.0.0.0/0
      Tags:
        - Key: Name
          Value: !Sub '${Environment}-app-sg'

  DatabaseSecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Security group for database servers
      VpcId: !Ref VPC
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 5432
          ToPort: 5432
          SourceSecurityGroupId: !Ref ApplicationSecurityGroup
      Tags:
        - Key: Name
          Value: !Sub '${Environment}-db-sg'

  # KMS Key for Encryption
  EncryptionKey:
    Type: AWS::KMS::Key
    Properties:
      Description: KMS key for data encryption
      KeyPolicy:
        Version: '2012-10-17'
        Statement:
          - Sid: Enable IAM User Permissions
            Effect: Allow
            Principal:
              AWS: !Sub 'arn:aws:iam::${AWS::AccountId}:root'
            Action: kms:*
            Resource: '*'
          - Sid: Allow use of the key
            Effect: Allow
            Principal:
              AWS: !Sub 'arn:aws:iam::${AWS::AccountId}:role/${Environment}-app-role'
            Action:
              - kms:Decrypt
              - kms:DescribeKey
            Resource: '*'
      Tags:
        - Key: Name
          Value: !Sub '${Environment}-encryption-key'

  # CloudTrail Configuration
  CloudTrail:
    Type: AWS::CloudTrail::Trail
    Properties:
      TrailName: !Sub '${Environment}-cloudtrail'
      S3BucketName: !Ref CloudTrailBucket
      S3KeyPrefix: cloudtrail
      IsLogging: true
      IsMultiRegionTrail: true
      IncludeGlobalServiceEvents: true
      EnableLogFileValidation: true
      KMSKeyId: !Ref EncryptionKey
      Tags:
        - Key: Name
          Value: !Sub '${Environment}-cloudtrail'

  CloudTrailBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: !Sub '${Environment}-cloudtrail-${AWS::AccountId}'
      VersioningConfiguration:
        Status: Enabled
      ServerSideEncryptionConfiguration:
        - ServerSideEncryptionByDefault:
            SSEAlgorithm: AES256
      PublicAccessBlockConfiguration:
        BlockPublicAcls: true
        BlockPublicPolicy: true
        IgnorePublicAcls: true
        RestrictPublicBuckets: true

  # GuardDuty Configuration
  GuardDutyDetector:
    Type: AWS::GuardDuty::Detector
    Properties:
      Enable: true
      FindingPublishingFrequency: FIFTEEN_MINUTES

  # Security Hub Configuration
  SecurityHub:
    Type: AWS::SecurityHub::Hub
    Properties: {}

  # Config Rules
  S3BucketServerSideEncryptionEnabled:
    Type: AWS::Config::ConfigRule
    Properties:
      ConfigRuleName: s3-bucket-server-side-encryption-enabled
      Description: Checks that your S3 buckets have server-side encryption enabled
      Source:
        Owner: AWS
        SourceIdentifier: S3_BUCKET_SERVER_SIDE_ENCRYPTION_ENABLED
      Scope:
        ComplianceResourceTypes:
          - AWS::S3::Bucket

  # WAF Configuration
  WAFWebACL:
    Type: AWS::WAFv2::WebACL
    Properties:
      Name: !Sub '${Environment}-web-acl'
      Scope: REGIONAL
      DefaultAction:
        Allow: {}
      Rules:
        - Name: AWSManagedRulesCommonRuleSet
          Priority: 1
          OverrideAction:
            None: {}
          Statement:
            ManagedRuleGroupStatement:
              VendorName: AWS
              Name: AWSManagedRulesCommonRuleSet
          VisibilityConfig:
            SampledRequestsEnabled: true
            CloudWatchMetricsEnabled: true
            MetricName: AWSManagedRulesCommonRuleSet
        - Name: AWSManagedRulesKnownBadInputsRuleSet
          Priority: 2
          OverrideAction:
            None: {}
          Statement:
            ManagedRuleGroupStatement:
              VendorName: AWS
              Name: AWSManagedRulesKnownBadInputsRuleSet
          VisibilityConfig:
            SampledRequestsEnabled: true
            CloudWatchMetricsEnabled: true
            MetricName: AWSManagedRulesKnownBadInputsRuleSet
      VisibilityConfig:
        SampledRequestsEnabled: true
        CloudWatchMetricsEnabled: true
        MetricName: !Sub '${Environment}-web-acl'

Outputs:
  VPCId:
    Description: VPC ID
    Value: !Ref VPC
    Export:
      Name: !Sub '${Environment}-vpc-id'

  ApplicationSecurityGroup:
    Description: Application Security Group ID
    Value: !Ref ApplicationSecurityGroup
    Export:
      Name: !Sub '${Environment}-app-sg'

  EncryptionKey:
    Description: KMS Encryption Key ID
    Value: !Ref EncryptionKey
    Export:
      Name: !Sub '${Environment}-encryption-key'
```

### Kubernetes Security Policies

```yaml
# Kubernetes Security Policies for Fintech Applications
apiVersion: v1
kind: Namespace
metadata:
  name: fintech-app
  labels:
    name: fintech-app
    environment: production
---
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restricted-finance-psp
spec:
  privileged: false
  allowPrivilegeEscalation: false
  requiredDropCapabilities:
    - ALL
  allowedCapabilities:
    - NET_BIND_SERVICE
  volumes:
    - 'configMap'
    - 'emptyDir'
    - 'projected'
    - 'secret'
    - 'downwardAPI'
    - 'persistentVolumeClaim'
  hostNetwork: false
  hostIPC: false
  hostPID: false
  runAsUser:
    rule: 'MustRunAsNonRoot'
  runAsGroup:
    rule: 'MustRunAs'
    ranges:
    - min: 1000
      max: 65535
  seLinux:
    rule: 'RunAsAny'
  supplementalGroups:
    rule: 'MustRunAs'
    ranges:
    - min: 1000
      max: 65535
  fsGroup:
    rule: 'MustRunAs'
    ranges:
    - min: 1000
      max: 65535
  readOnlyRootFilesystem: true
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: finance-app-role
rules:
- apiGroups: [""]
  resources: ["pods", "services", "endpoints", "configmaps", "secrets"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
- apiGroups: ["apps"]
  resources: ["deployments", "replicasets", "statefulsets"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
- apiGroups: ["networking.k8s.io"]
  resources: ["networkpolicies"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: finance-app-binding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: finance-app-role
subjects:
- kind: ServiceAccount
  name: finance-app-sa
  namespace: fintech-app
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: finance-app-sa
  namespace: fintech-app
  annotations:
    eks.amazonaws.com/role-arn: arn:aws:iam::123456789012:role/finance-app-role
---
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: finance-app-network-policy
  namespace: fintech-app
spec:
  podSelector:
    matchLabels:
      app: finance-app
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: istio-system
    - podSelector:
        matchLabels:
          app: istio-ingressgateway
    ports:
    - protocol: TCP
      port: 8080
    - protocol: TCP
      port: 8443
  egress:
  - to:
    - podSelector:
        matchLabels:
          app: finance-db
    ports:
    - protocol: TCP
      port: 5432
  - to: []
    ports:
    - protocol: TCP
      port: 53
    - protocol: UDP
      port: 53
---
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: finance-app-mtls
  namespace: fintech-app
spec:
  selector:
    matchLabels:
      app: finance-app
  mtls:
    mode: STRICT
---
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: finance-app-authz
  namespace: fintech-app
spec:
  selector:
    matchLabels:
      app: finance-app
  action: ALLOW
  rules:
  - from:
    - source:
        principals: ["cluster.local/ns/istio-system/sa/istio-ingressgateway-service-account"]
    to:
    - operation:
        methods: ["GET", "POST"]
        paths: ["/api/v1/payments", "/api/v1/transactions"]
```

### Security Monitoring Dashboard

```json
{
  "dashboard": {
    "title": "Fintech Security Monitoring Dashboard",
    "tags": ["security", "fintech", "compliance"],
    "timezone": "UTC",
    "panels": [
      {
        "title": "Security Events Overview",
        "type": "stat",
        "targets": [
          {
            "expr": "sum(rate(security_events_total[5m]))",
            "legendFormat": "Total Security Events"
          }
        ],
        "fieldConfig": {
          "defaults": {
            "mappings": [
              {
                "options": {
                  "match": "null",
                  "result": {
                    "text": "N/A"
                  }
                },
                "type": "special"
              }
            ],
            "thresholds": {
              "mode": "absolute",
              "steps": [
                {
                  "color": "green",
                  "value": null
                },
                {
                  "color": "red",
                  "value": 80
                }
              ]
            },
            "unit": "events/min"
          }
        }
      },
      {
        "title": "Failed Authentication Attempts",
        "type": "graph",
        "targets": [
          {
            "expr": "sum(rate(authentication_failures_total[5m])) by (service)",
            "legendFormat": "{{service}}"
          }
        ]
      },
      {
        "title": "PCI DSS Compliance Status",
        "type": "table",
        "targets": [
          {
            "expr": "pci_dss_compliance_status",
            "legendFormat": "Compliance Status"
          }
        ],
        "fieldConfig": {
          "defaults": {
            "mappings": [
              {
                "options": {
                  "0": {
                    "text": "Non-Compliant",
                    "color": "red"
                  },
                  "1": {
                    "text": "Compliant",
                    "color": "green"
                  }
                },
                "type": "value"
              }
            ]
          }
        }
      },
      {
        "title": "Data Encryption Status",
        "type": "bargauge",
        "targets": [
          {
            "expr": "encryption_status{type=\"at_rest\"}",
            "legendFormat": "Data at Rest"
          },
          {
            "expr": "encryption_status{type=\"in_transit\"}",
            "legendFormat": "Data in Transit"
          }
        ]
      },
      {
        "title": "Vulnerability Scan Results",
        "type": "table",
        "targets": [
          {
            "expr": "vulnerability_scan_results",
            "legendFormat": "Vulnerabilities"
          }
        ],
        "transformations": [
          {
            "id": "organize",
            "options": {
              "excludeByName": {},
              "indexByName": {},
              "renameByName": {
                "severity": "Severity",
                "count": "Count",
                "service": "Service"
              }
            }
          }
        ]
      },
      {
        "title": "Network Security Events",
        "type": "logs",
        "targets": [
          {
            "expr": "{job=\"network-security\"} | json",
            "legendFormat": "Network Events"
          }
        ]
      }
    ],
    "time": {
      "from": "now-1h",
      "to": "now"
    },
    "refresh": "30s"
  }
}
```

### Compliance Automation Scripts

```bash
#!/bin/bash
# Fintech Security Compliance Automation Script

set -e

# Configuration
ENVIRONMENT="${1:-prod}"
REPORT_DATE=$(date +%Y-%m-%d)
REPORT_DIR="/var/log/compliance-reports"
LOG_FILE="${REPORT_DIR}/compliance-check-${REPORT_DATE}.log"

# Logging function
log() {
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"
}

# Initialize report
init_report() {
    mkdir -p "$REPORT_DIR"
    cat > "${REPORT_DIR}/compliance-report-${REPORT_DATE}.json" << EOF
{
  "report_date": "$REPORT_DATE",
  "environment": "$ENVIRONMENT",
  "checks": []
}
EOF
}

# Add check result to report
add_check_result() {
    local check_name="$1"
    local status="$2"
    local details="$3"
    local remediation="$4"

    jq --arg check_name "$check_name" \
       --arg status "$status" \
       --arg details "$details" \
       --arg remediation "$remediation" \
       --arg timestamp "$(date +%s)" \
       '.checks += [{
         "name": $check_name,
         "status": $status,
         "details": $details,
         "remediation": $remediation,
         "timestamp": $timestamp
       }]' "${REPORT_DIR}/compliance-report-${REPORT_DATE}.json" > "${REPORT_DIR}/temp.json" && \
    mv "${REPORT_DIR}/temp.json" "${REPORT_DIR}/compliance-report-${REPORT_DATE}.json"
}

# PCI DSS Encryption Check
check_pci_encryption() {
    log "Checking PCI DSS encryption compliance..."

    # Check S3 bucket encryption
    local s3_encrypted=$(aws s3api get-bucket-encryption --bucket fintech-data-$ENVIRONMENT 2>/dev/null | jq -r '.ServerSideEncryptionConfiguration[0].ServerSideEncryptionByDefault.SSEAlgorithm' || echo "NONE")

    if [ "$s3_encrypted" = "AES256" ] || [ "$s3_encrypted" = "aws:kms" ]; then
        add_check_result "PCI DSS S3 Encryption" "PASS" "S3 bucket encryption enabled with $s3_encrypted" ""
    else
        add_check_result "PCI DSS S3 Encryption" "FAIL" "S3 bucket encryption not properly configured" "Enable server-side encryption on S3 buckets"
    fi

    # Check RDS encryption
    local rds_encrypted=$(aws rds describe-db-instances --db-instance-identifier fintech-db-$ENVIRONMENT 2>/dev/null | jq -r '.DBInstances[0].StorageEncrypted' || echo "false")

    if [ "$rds_encrypted" = "true" ]; then
        add_check_result "PCI DSS RDS Encryption" "PASS" "RDS instance encryption enabled" ""
    else
        add_check_result "PCI DSS RDS Encryption" "FAIL" "RDS instance not encrypted" "Enable encryption for RDS instances"
    fi
}

# IAM Security Check
check_iam_security() {
    log "Checking IAM security configuration..."

    # Check MFA for root account
    local root_mfa=$(aws iam get-account-summary 2>/dev/null | jq -r '.SummaryMap.AccountMFAEnabled' || echo "0")

    if [ "$root_mfa" -eq 1 ]; then
        add_check_result "Root Account MFA" "PASS" "Root account MFA is enabled" ""
    else
        add_check_result "Root Account MFA" "FAIL" "Root account MFA is not enabled" "Enable MFA for root account"
    fi

    # Check for access keys older than 90 days
    local old_keys=$(aws iam list-users --query 'Users[*].UserName' --output text | xargs -I {} aws iam list-access-keys --user-name {} --query 'AccessKeyMetadata[?CreateDate<`'"$(date -d '90 days ago' +%Y-%m-%d)"'`].AccessKeyId' --output text 2>/dev/null | wc -l)

    if [ "$old_keys" -eq 0 ]; then
        add_check_result "Access Key Rotation" "PASS" "No access keys older than 90 days" ""
    else
        add_check_result "Access Key Rotation" "FAIL" "$old_keys access keys older than 90 days" "Rotate access keys older than 90 days"
    fi
}

# Network Security Check
check_network_security() {
    log "Checking network security configuration..."

    # Check security groups for overly permissive rules
    local open_sg=$(aws ec2 describe-security-groups --filters Name=group-name,Values=fintech-* --query 'SecurityGroups[*].GroupId' --output text 2>/dev/null | xargs -I {} aws ec2 describe-security-groups --group-ids {} --query 'SecurityGroups[*].IpPermissions[?IpRanges[?Cidr==`0.0.0.0/0`]]' --output text | wc -l)

    if [ "$open_sg" -eq 0 ]; then
        add_check_result "Security Group Configuration" "PASS" "No security groups allow unrestricted access" ""
    else
        add_check_result "Security Group Configuration" "FAIL" "$open_sg security groups allow unrestricted access" "Review and restrict security group rules"
    fi

    # Check VPC flow logs
    local flow_logs=$(aws ec2 describe-flow-logs --filter Name=resource-id,Values=$(aws ec2 describe-vpcs --filters Name=tag:Name,Values=fintech-vpc-$ENVIRONMENT --query 'Vpcs[*].VpcId' --output text) --query 'FlowLogs[0].FlowLogId' --output text 2>/dev/null || echo "NONE")

    if [ "$flow_logs" != "NONE" ]; then
        add_check_result "VPC Flow Logs" "PASS" "VPC flow logs are enabled" ""
    else
        add_check_result "VPC Flow Logs" "FAIL" "VPC flow logs are not enabled" "Enable VPC flow logs for network monitoring"
    fi
}

# Generate compliance summary
generate_summary() {
    local total_checks=$(jq '.checks | length' "${REPORT_DIR}/compliance-report-${REPORT_DATE}.json")
    local passed_checks=$(jq '.checks | map(select(.status == "PASS")) | length' "${REPORT_DIR}/compliance-report-${REPORT_DATE}.json")
    local failed_checks=$(jq '.checks | map(select(.status == "FAIL")) | length' "${REPORT_DIR}/compliance-report-${REPORT_DATE}.json")
    local compliance_rate=$((passed_checks * 100 / total_checks))

    jq --arg total "$total_checks" \
       --arg passed "$passed_checks" \
       --arg failed "$failed_checks" \
       --arg rate "$compliance_rate" \
       '. + {
         "summary": {
           "total_checks": $total,
           "passed_checks": $passed,
           "failed_checks": $failed,
           "compliance_rate": $rate
         }
       }' "${REPORT_DIR}/compliance-report-${REPORT_DATE}.json" > "${REPORT_DIR}/temp.json" && \
    mv "${REPORT_DIR}/temp.json" "${REPORT_DIR}/compliance-report-${REPORT_DATE}.json"

    log "Compliance check completed: $passed_checks/$total_checks checks passed ($compliance_rate% compliance rate)"
}

# Main execution
main() {
    log "Starting fintech security compliance check for $ENVIRONMENT environment"

    init_report
    check_pci_encryption
    check_iam_security
    check_network_security
    generate_summary

    log "Compliance check completed. Report saved to ${REPORT_DIR}/compliance-report-${REPORT_DATE}.json"

    # Send alert if compliance rate is below threshold
    local compliance_rate=$(jq -r '.summary.compliance_rate' "${REPORT_DIR}/compliance-report-${REPORT_DATE}.json")
    if [ "$compliance_rate" -lt 95 ]; then
        log "ALERT: Compliance rate is below 95% ($compliance_rate%)"
        # Add alerting logic here (email, Slack, etc.)
    fi
}

main "$@"
```

This comprehensive security configuration template ensures thorough security hardening, compliance automation, and monitoring for fintech DevOps environments while maintaining focus on financial industry regulatory requirements.