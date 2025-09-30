# Infrastructure as Code Templates

## 🎯 Overview

This template provides comprehensive Infrastructure as Code (IaC) patterns and AI prompts for managing fintech infrastructure. It covers Terraform, AWS CloudFormation, Azure Resource Manager, and Kubernetes manifests with security and compliance focus.

## 🏗️ AI Prompt Templates

### Terraform Infrastructure Generation

#### Multi-Environment AWS Infrastructure

```
Create a comprehensive Terraform configuration for a fintech payment application with the following requirements:

**Infrastructure Components:**
- VPC with public/private subnets across 3 AZs
- Application Load Balancer with SSL termination
- ECS Fargate cluster for containerized applications
- RDS PostgreSQL with Multi-AZ deployment
- ElastiCache Redis cluster for session storage
- S3 buckets for document storage with encryption
- CloudFront CDN for static asset distribution
- Route53 for DNS management
- WAF for application protection

**Security Requirements:**
- All traffic encrypted in transit and at rest
- Private subnets for databases and application servers
- Security groups with principle of least privilege
- IAM roles and policies following best practices
- VPC Flow Logs enabled
- CloudTrail for audit logging
- KMS keys for encryption

**Compliance Requirements:**
- PCI DSS Level 1 compliance
- SOC 2 Type II controls
- GDPR data protection
- Resource tagging for cost allocation
- Backup and retention policies

**Environment Configuration:**
- Separate configurations for dev, staging, and production
- Environment-specific variable files
- Remote state management with locking
- Automated apply/destroy workflows

Please include:
1. Main Terraform configuration files
2. Variable definitions and outputs
3. Module structure for reusability
4. Security group rules
5. IAM policies and roles
6. Monitoring and alerting resources
7. Backup and disaster recovery setup
```

#### Azure Infrastructure with Terraform

```
Generate Terraform configuration for Azure-based fintech infrastructure with:

**Core Services:**
- Resource Groups with proper naming convention
- Virtual Networks with subnet segmentation
- Application Gateway with WAF enabled
- Azure Kubernetes Service (AKS) cluster
- Azure Database for PostgreSQL with encryption
- Azure Redis Cache for session management
- Azure Storage Account with blob encryption
- Azure Key Vault for secrets management
- Azure Monitor and Log Analytics workspace

**Security Configuration:**
- Network Security Groups with restrictive rules
- Azure AD integration for authentication
- Service principals with minimal permissions
- Private endpoints for PaaS services
- Azure Security Center recommendations
- Diagnostic settings for all resources

**Compliance Features:**
- Azure Policy assignments for governance
- Resource locks on critical infrastructure
- Backup policies for databases and storage
- Retention policies for logs and backups
- Cost management and budgets
- Resource tagging strategy

Include environment-specific configurations and proper module structure.
```

### Kubernetes Infrastructure

#### Production-Ready Kubernetes Manifests

```
Create production-ready Kubernetes manifests for a fintech payment application:

**Application Architecture:**
- Frontend: React application (3 replicas)
- API Gateway: Node.js service (5 replicas)
- Payment Service: Java microservice (3 replicas)
- User Service: Python microservice (3 replicas)
- Background Jobs: Redis-based queue processor

**Infrastructure Components:**
- Ingress controller with SSL certificates
- Service mesh (Istio) for traffic management
- Horizontal Pod Autoscaler (HPA) configuration
- Vertical Pod Autoscaler (VPA) for optimization
- Pod Disruption Budget (PDB) for availability
- Network policies for service isolation
- ConfigMaps and Secrets for configuration
- Persistent Volume Claims for data storage

**Security Requirements:**
- Pod Security Policies/Pod Security Standards
- RBAC for service accounts
- Network policies for micro-segmentation
- Image security scanning integration
- Runtime security monitoring
- Secrets encryption at rest

**Monitoring and Observability:**
- Prometheus monitoring setup
- Grafana dashboards
- Jaeger distributed tracing
- Fluentd log aggregation
- Service monitors and alerting rules

**Deployment Strategy:**
- Blue-green deployment configuration
- Canary release automation
- Rolling update strategy
- Database migration jobs
- Health checks and readiness probes

Provide complete YAML manifests with proper resource limits, anti-affinity rules, and production best practices.
```

### AWS CloudFormation Templates

#### Serverless Fintech Infrastructure

```
Design AWS CloudFormation templates for a serverless fintech architecture:

**Serverless Components:**
- API Gateway with request validation
- Lambda functions for business logic
- DynamoDB tables with encryption
- S3 buckets for document storage
- EventBridge for event-driven architecture
- SQS/SNS for messaging
- Step Functions for workflow orchestration

**Security and Compliance:**
- Cognito User Pools for authentication
- IAM roles with least privilege
- VPC endpoints for private access
- WAF rules for API protection
- CloudTrail for audit logging
- Config rules for compliance monitoring

**Data Management:**
- DynamoDB Global Tables for multi-region
- S3 Cross-Region Replication
- Backup and restore automation
- Data lifecycle policies
- Encryption key management

**Monitoring and Alerting:**
- CloudWatch custom metrics
- X-Ray tracing configuration
- Lambda function monitoring
- Cost optimization alerts
- Performance dashboards

Include nested stacks, parameter files, and deployment automation scripts.
```

## 🔧 Terraform Configuration Examples

### Main Infrastructure Configuration

```hcl
# main.tf
terraform {
  required_version = ">= 1.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  backend "s3" {
    bucket         = "fintech-terraform-state"
    key            = "infrastructure/terraform.tfstate"
    region         = "us-west-2"
    encrypt        = true
    dynamodb_table = "terraform-state-lock"
  }
}

provider "aws" {
  region = var.aws_region
  
  default_tags {
    tags = {
      Environment   = var.environment
      Project       = var.project_name
      Owner         = var.owner
      CostCenter    = var.cost_center
      CreatedBy     = "terraform"
      CreatedAt     = timestamp()
    }
  }
}

# Data sources
data "aws_availability_zones" "available" {
  state = "available"
}

data "aws_caller_identity" "current" {}

# VPC Module
module "vpc" {
  source = "./modules/vpc"
  
  name               = "${var.project_name}-${var.environment}"
  cidr               = var.vpc_cidr
  availability_zones = data.aws_availability_zones.available.names
  environment        = var.environment
  
  # Subnet configuration
  public_subnets  = var.public_subnets
  private_subnets = var.private_subnets
  
  # Features
  enable_nat_gateway     = true
  enable_vpn_gateway     = false
  enable_dns_hostnames   = true
  enable_dns_support     = true
  enable_flow_log        = true
  
  tags = local.common_tags
}

# Security Groups Module
module "security_groups" {
  source = "./modules/security-groups"
  
  vpc_id      = module.vpc.vpc_id
  environment = var.environment
  project     = var.project_name
  
  # CIDR blocks for access control
  trusted_cidr_blocks    = var.trusted_cidr_blocks
  database_cidr_blocks   = module.vpc.private_subnets_cidr_blocks
  application_cidr_blocks = module.vpc.private_subnets_cidr_blocks
}

# Application Load Balancer
module "alb" {
  source = "./modules/alb"
  
  name            = "${var.project_name}-${var.environment}-alb"
  vpc_id          = module.vpc.vpc_id
  subnets         = module.vpc.public_subnets
  security_groups = [module.security_groups.alb_security_group_id]
  
  # SSL configuration
  certificate_arn = var.ssl_certificate_arn
  ssl_policy      = "ELBSecurityPolicy-TLS-1-2-2017-01"
  
  # WAF integration
  enable_waf = true
  waf_acl_id = module.waf.web_acl_id
  
  tags = local.common_tags
}

# ECS Cluster
module "ecs" {
  source = "./modules/ecs"
  
  cluster_name = "${var.project_name}-${var.environment}"
  
  # Capacity providers
  capacity_providers = ["FARGATE", "FARGATE_SPOT"]
  
  # Container insights
  enable_container_insights = true
  
  tags = local.common_tags
}

# RDS Database
module "rds" {
  source = "./modules/rds"
  
  identifier = "${var.project_name}-${var.environment}-db"
  
  # Engine configuration
  engine         = "postgres"
  engine_version = "14.9"
  instance_class = var.rds_instance_class
  
  # Storage configuration
  allocated_storage     = var.rds_allocated_storage
  max_allocated_storage = var.rds_max_allocated_storage
  storage_encrypted     = true
  kms_key_id           = module.kms.rds_key_arn
  
  # Network configuration
  subnet_ids         = module.vpc.private_subnets
  vpc_security_group_ids = [module.security_groups.rds_security_group_id]
  
  # Database configuration
  db_name  = var.database_name
  username = var.database_username
  password = var.database_password
  
  # Backup configuration
  backup_retention_period = var.backup_retention_period
  backup_window          = "03:00-04:00"
  maintenance_window     = "sun:04:00-sun:05:00"
  
  # Multi-AZ for production
  multi_az = var.environment == "production" ? true : false
  
  # Performance Insights
  performance_insights_enabled = true
  performance_insights_retention_period = 7
  
  # Monitoring
  monitoring_interval = 60
  monitoring_role_arn = aws_iam_role.rds_enhanced_monitoring.arn
  
  # Deletion protection for production
  deletion_protection = var.environment == "production" ? true : false
  
  tags = local.common_tags
}

# ElastiCache Redis
module "redis" {
  source = "./modules/redis"
  
  cluster_id = "${var.project_name}-${var.environment}-redis"
  
  # Engine configuration
  engine_version    = "7.0"
  node_type        = var.redis_node_type
  num_cache_nodes  = var.redis_num_nodes
  
  # Network configuration
  subnet_group_name  = aws_elasticache_subnet_group.redis.name
  security_group_ids = [module.security_groups.redis_security_group_id]
  
  # Security
  at_rest_encryption_enabled = true
  transit_encryption_enabled = true
  auth_token                 = var.redis_auth_token
  
  # Backup
  snapshot_retention_limit = 5
  snapshot_window         = "03:00-05:00"
  
  tags = local.common_tags
}

# S3 Buckets
module "s3" {
  source = "./modules/s3"
  
  # Document storage bucket
  document_bucket_name = "${var.project_name}-${var.environment}-documents"
  
  # Static assets bucket
  assets_bucket_name = "${var.project_name}-${var.environment}-assets"
  
  # Backup bucket
  backup_bucket_name = "${var.project_name}-${var.environment}-backups"
  
  # KMS encryption
  kms_key_arn = module.kms.s3_key_arn
  
  # Lifecycle policies
  enable_lifecycle_policies = true
  
  # Versioning
  enable_versioning = var.environment == "production" ? true : false
  
  # Public access blocking
  block_public_access = true
  
  tags = local.common_tags
}

# KMS Keys
module "kms" {
  source = "./modules/kms"
  
  project     = var.project_name
  environment = var.environment
  
  # Create keys for different services
  create_rds_key    = true
  create_s3_key     = true
  create_secrets_key = true
  
  tags = local.common_tags
}

# WAF
module "waf" {
  source = "./modules/waf"
  
  name        = "${var.project_name}-${var.environment}-waf"
  environment = var.environment
  
  # Associated resources
  alb_arn = module.alb.alb_arn
  
  # Rate limiting
  rate_limit = var.waf_rate_limit
  
  # IP whitelist/blacklist
  allowed_ips = var.waf_allowed_ips
  blocked_ips = var.waf_blocked_ips
  
  tags = local.common_tags
}

# CloudWatch Monitoring
module "monitoring" {
  source = "./modules/monitoring"
  
  project     = var.project_name
  environment = var.environment
  
  # Resources to monitor
  alb_arn         = module.alb.alb_arn
  ecs_cluster_name = module.ecs.cluster_name
  rds_identifier   = module.rds.db_instance_id
  
  # Notification endpoints
  sns_endpoints = var.monitoring_sns_endpoints
  
  tags = local.common_tags
}

# Locals
locals {
  common_tags = {
    Environment = var.environment
    Project     = var.project_name
    Owner       = var.owner
    CostCenter  = var.cost_center
    Terraform   = "true"
  }
}
```

### Variables Configuration

```hcl
# variables.tf
variable "aws_region" {
  description = "AWS region for resources"
  type        = string
  default     = "us-west-2"
}

variable "environment" {
  description = "Environment name (dev, staging, production)"
  type        = string
  validation {
    condition     = contains(["dev", "staging", "production"], var.environment)
    error_message = "Environment must be dev, staging, or production."
  }
}

variable "project_name" {
  description = "Name of the project"
  type        = string
  default     = "fintech-payment"
}

variable "owner" {
  description = "Owner of the resources"
  type        = string
}

variable "cost_center" {
  description = "Cost center for billing"
  type        = string
}

# Network Configuration
variable "vpc_cidr" {
  description = "CIDR block for VPC"
  type        = string
  default     = "10.0.0.0/16"
}

variable "public_subnets" {
  description = "CIDR blocks for public subnets"
  type        = list(string)
  default     = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
}

variable "private_subnets" {
  description = "CIDR blocks for private subnets"
  type        = list(string)
  default     = ["10.0.10.0/24", "10.0.20.0/24", "10.0.30.0/24"]
}

variable "trusted_cidr_blocks" {
  description = "CIDR blocks allowed to access resources"
  type        = list(string)
  default     = []
}

# SSL Configuration
variable "ssl_certificate_arn" {
  description = "ARN of SSL certificate for ALB"
  type        = string
}

# Database Configuration
variable "rds_instance_class" {
  description = "RDS instance class"
  type        = string
  default     = "db.t3.micro"
}

variable "rds_allocated_storage" {
  description = "Initial storage allocation for RDS"
  type        = number
  default     = 20
}

variable "rds_max_allocated_storage" {
  description = "Maximum storage allocation for RDS"
  type        = number
  default     = 100
}

variable "database_name" {
  description = "Name of the database"
  type        = string
  default     = "fintech_app"
}

variable "database_username" {
  description = "Database username"
  type        = string
  default     = "admin"
}

variable "database_password" {
  description = "Database password"
  type        = string
  sensitive   = true
}

variable "backup_retention_period" {
  description = "Number of days to retain backups"
  type        = number
  default     = 7
}

# Redis Configuration
variable "redis_node_type" {
  description = "ElastiCache Redis node type"
  type        = string
  default     = "cache.t3.micro"
}

variable "redis_num_nodes" {
  description = "Number of Redis nodes"
  type        = number
  default     = 1
}

variable "redis_auth_token" {
  description = "Redis authentication token"
  type        = string
  sensitive   = true
}

# WAF Configuration
variable "waf_rate_limit" {
  description = "WAF rate limit per 5 minutes"
  type        = number
  default     = 10000
}

variable "waf_allowed_ips" {
  description = "IP addresses allowed by WAF"
  type        = list(string)
  default     = []
}

variable "waf_blocked_ips" {
  description = "IP addresses blocked by WAF"
  type        = list(string)
  default     = []
}

# Monitoring Configuration
variable "monitoring_sns_endpoints" {
  description = "SNS endpoints for monitoring alerts"
  type        = list(string)
  default     = []
}
```

### Environment-Specific Variables

```hcl
# environments/production.tfvars
environment = "production"
owner      = "platform-team"
cost_center = "engineering"

# Network
vpc_cidr = "10.0.0.0/16"
public_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
private_subnets = ["10.0.10.0/24", "10.0.20.0/24", "10.0.30.0/24"]

# Database - Production sizing
rds_instance_class = "db.r5.xlarge"
rds_allocated_storage = 100
rds_max_allocated_storage = 1000
backup_retention_period = 30

# Redis - Production sizing
redis_node_type = "cache.r5.large"
redis_num_nodes = 3

# WAF - Stricter limits for production
waf_rate_limit = 5000

# Monitoring
monitoring_sns_endpoints = [
  "arn:aws:sns:us-west-2:123456789012:production-alerts"
]

# SSL Certificate
ssl_certificate_arn = "arn:aws:acm:us-west-2:123456789012:certificate/12345678-1234-1234-1234-123456789012"
```

```hcl
# environments/dev.tfvars
environment = "dev"
owner      = "dev-team"
cost_center = "engineering"

# Network
vpc_cidr = "10.1.0.0/16"
public_subnets = ["10.1.1.0/24", "10.1.2.0/24"]
private_subnets = ["10.1.10.0/24", "10.1.20.0/24"]

# Database - Development sizing
rds_instance_class = "db.t3.micro"
rds_allocated_storage = 20
rds_max_allocated_storage = 50
backup_retention_period = 1

# Redis - Development sizing
redis_node_type = "cache.t3.micro"
redis_num_nodes = 1

# WAF - Relaxed limits for development
waf_rate_limit = 50000

# SSL Certificate (wildcard for dev)
ssl_certificate_arn = "arn:aws:acm:us-west-2:123456789012:certificate/dev-wildcard-cert"
```

## 📊 Azure Resource Manager Templates

### Complete Azure Infrastructure

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "environment": {
      "type": "string",
      "allowedValues": [
        "dev",
        "staging",
        "production"
      ],
      "metadata": {
        "description": "Environment for the deployment"
      }
    },
    "projectName": {
      "type": "string",
      "metadata": {
        "description": "Name of the project"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username for database"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password for database"
      }
    }
  },
  "variables": {
    "vnetName": "[concat(parameters('projectName'), '-', parameters('environment'), '-vnet')]",
    "appGatewayName": "[concat(parameters('projectName'), '-', parameters('environment'), '-appgw')]",
    "aksClusterName": "[concat(parameters('projectName'), '-', parameters('environment'), '-aks')]",
    "postgresServerName": "[concat(parameters('projectName'), '-', parameters('environment'), '-psql')]",
    "redisName": "[concat(parameters('projectName'), '-', parameters('environment'), '-redis')]",
    "storageAccountName": "[concat(toLower(parameters('projectName')), toLower(parameters('environment')), 'storage')]",
    "keyVaultName": "[concat(parameters('projectName'), '-', parameters('environment'), '-kv')]",
    "logAnalyticsName": "[concat(parameters('projectName'), '-', parameters('environment'), '-logs')]"
  },
  "resources": [
    {
      "type": "Microsoft.Network/virtualNetworks",
      "apiVersion": "2021-02-01",
      "name": "[variables('vnetName')]",
      "location": "[parameters('location')]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "10.0.0.0/16"
          ]
        },
        "subnets": [
          {
            "name": "frontend-subnet",
            "properties": {
              "addressPrefix": "10.0.1.0/24"
            }
          },
          {
            "name": "backend-subnet",
            "properties": {
              "addressPrefix": "10.0.2.0/24"
            }
          },
          {
            "name": "database-subnet",
            "properties": {
              "addressPrefix": "10.0.3.0/24",
              "delegations": [
                {
                  "name": "postgres-delegation",
                  "properties": {
                    "serviceName": "Microsoft.DBforPostgreSQL/flexibleServers"
                  }
                }
              ]
            }
          },
          {
            "name": "aks-subnet",
            "properties": {
              "addressPrefix": "10.0.4.0/22"
            }
          }
        ]
      }
    },
    {
      "type": "Microsoft.ContainerService/managedClusters",
      "apiVersion": "2022-05-02-preview",
      "name": "[variables('aksClusterName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Network/virtualNetworks', variables('vnetName'))]"
      ],
      "identity": {
        "type": "SystemAssigned"
      },
      "properties": {
        "dnsPrefix": "[concat(parameters('projectName'), parameters('environment'))]",
        "agentPoolProfiles": [
          {
            "name": "system",
            "count": 3,
            "vmSize": "Standard_D2s_v3",
            "osType": "Linux",
            "mode": "System",
            "vnetSubnetID": "[resourceId('Microsoft.Network/virtualNetworks/subnets', variables('vnetName'), 'aks-subnet')]"
          },
          {
            "name": "user",
            "count": 2,
            "vmSize": "Standard_D4s_v3",
            "osType": "Linux",
            "mode": "User",
            "vnetSubnetID": "[resourceId('Microsoft.Network/virtualNetworks/subnets', variables('vnetName'), 'aks-subnet')]"
          }
        ],
        "networkProfile": {
          "networkPlugin": "azure",
          "serviceCidr": "172.16.0.0/16",
          "dnsServiceIP": "172.16.0.10"
        },
        "addonProfiles": {
          "azurepolicy": {
            "enabled": true
          },
          "omsagent": {
            "enabled": true,
            "config": {
              "logAnalyticsWorkspaceResourceID": "[resourceId('Microsoft.OperationalInsights/workspaces', variables('logAnalyticsName'))]"
            }
          }
        }
      }
    },
    {
      "type": "Microsoft.DBforPostgreSQL/flexibleServers",
      "apiVersion": "2021-06-01",
      "name": "[variables('postgresServerName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Network/virtualNetworks', variables('vnetName'))]"
      ],
      "sku": {
        "name": "Standard_D2s_v3",
        "tier": "GeneralPurpose"
      },
      "properties": {
        "version": "14",
        "administratorLogin": "[parameters('adminUsername')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "storage": {
          "storageSizeGB": 128
        },
        "backup": {
          "backupRetentionDays": 7,
          "geoRedundantBackup": "Enabled"
        },
        "network": {
          "delegatedSubnetResourceId": "[resourceId('Microsoft.Network/virtualNetworks/subnets', variables('vnetName'), 'database-subnet')]"
        },
        "highAvailability": {
          "mode": "[if(equals(parameters('environment'), 'production'), 'ZoneRedundant', 'Disabled')]"
        }
      }
    },
    {
      "type": "Microsoft.Cache/Redis",
      "apiVersion": "2021-06-01",
      "name": "[variables('redisName')]",
      "location": "[parameters('location')]",
      "properties": {
        "sku": {
          "name": "Premium",
          "family": "P",
          "capacity": 1
        },
        "redisConfiguration": {
          "maxclients": "1000",
          "maxmemory-reserved": "50",
          "maxfragmentationmemory-reserved": "50",
          "maxmemory-delta": "50"
        },
        "enableNonSslPort": false,
        "redisVersion": "6"
      }
    },
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2021-06-01",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "StorageV2",
      "properties": {
        "supportsHttpsTrafficOnly": true,
        "encryption": {
          "services": {
            "blob": {
              "enabled": true
            },
            "file": {
              "enabled": true
            }
          },
          "keySource": "Microsoft.Storage"
        },
        "networkAcls": {
          "defaultAction": "Deny",
          "virtualNetworkRules": [
            {
              "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', variables('vnetName'), 'backend-subnet')]",
              "action": "Allow"
            }
          ]
        }
      }
    },
    {
      "type": "Microsoft.KeyVault/vaults",
      "apiVersion": "2021-06-01-preview",
      "name": "[variables('keyVaultName')]",
      "location": "[parameters('location')]",
      "properties": {
        "tenantId": "[subscription().tenantId]",
        "sku": {
          "family": "A",
          "name": "standard"
        },
        "accessPolicies": [],
        "networkAcls": {
          "defaultAction": "Deny",
          "virtualNetworkRules": [
            {
              "id": "[resourceId('Microsoft.Network/virtualNetworks/subnets', variables('vnetName'), 'backend-subnet')]"
            }
          ]
        },
        "enabledForDeployment": false,
        "enabledForDiskEncryption": false,
        "enabledForTemplateDeployment": false,
        "enableSoftDelete": true,
        "softDeleteRetentionInDays": 90,
        "enableRbacAuthorization": true
      }
    },
    {
      "type": "Microsoft.OperationalInsights/workspaces",
      "apiVersion": "2021-06-01",
      "name": "[variables('logAnalyticsName')]",
      "location": "[parameters('location')]",
      "properties": {
        "sku": {
          "name": "PerGB2018"
        },
        "retentionInDays": 30
      }
    }
  ],
  "outputs": {
    "aksClusterName": {
      "type": "string",
      "value": "[variables('aksClusterName')]"
    },
    "postgresServerName": {
      "type": "string",
      "value": "[variables('postgresServerName')]"
    },
    "redisName": {
      "type": "string",
      "value": "[variables('redisName')]"
    },
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    },
    "keyVaultName": {
      "type": "string",
      "value": "[variables('keyVaultName')]"
    }
  }
}
```

## 🚀 Deployment Automation

### GitHub Actions Workflow

```yaml
# .github/workflows/infrastructure.yml
name: Infrastructure Deployment

on:
  push:
    branches: [main, develop]
    paths: ['infrastructure/**']
  pull_request:
    branches: [main]
    paths: ['infrastructure/**']

env:
  TF_VERSION: 1.5.0
  AWS_REGION: us-west-2

jobs:
  validate:
    name: Validate Terraform
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: ${{ env.TF_VERSION }}

      - name: Terraform Format Check
        run: terraform fmt -check -recursive
        working-directory: infrastructure

      - name: Terraform Init
        run: terraform init -backend=false
        working-directory: infrastructure

      - name: Terraform Validate
        run: terraform validate
        working-directory: infrastructure

      - name: Run TFLint
        uses: terraform-linters/setup-tflint@v2
        with:
          tflint_version: latest

      - name: TFLint
        run: tflint --recursive
        working-directory: infrastructure

      - name: Run Checkov
        uses: bridgecrewio/checkov-action@master
        with:
          directory: infrastructure
          framework: terraform

  plan:
    name: Terraform Plan
    runs-on: ubuntu-latest
    needs: validate
    if: github.event_name == 'pull_request'
    strategy:
      matrix:
        environment: [dev, staging]
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: ${{ env.TF_VERSION }}

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Terraform Init
        run: terraform init
        working-directory: infrastructure
        env:
          TF_WORKSPACE: ${{ matrix.environment }}

      - name: Terraform Plan
        run: |
          terraform plan \
            -var-file="environments/${{ matrix.environment }}.tfvars" \
            -out="${{ matrix.environment }}.tfplan"
        working-directory: infrastructure
        env:
          TF_WORKSPACE: ${{ matrix.environment }}

      - name: Upload Plan
        uses: actions/upload-artifact@v3
        with:
          name: ${{ matrix.environment }}-tfplan
          path: infrastructure/${{ matrix.environment }}.tfplan

  deploy-dev:
    name: Deploy to Development
    runs-on: ubuntu-latest
    needs: validate
    if: github.ref == 'refs/heads/develop' && github.event_name == 'push'
    environment: development
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: ${{ env.TF_VERSION }}

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Terraform Init
        run: terraform init
        working-directory: infrastructure
        env:
          TF_WORKSPACE: dev

      - name: Terraform Apply
        run: |
          terraform apply \
            -var-file="environments/dev.tfvars" \
            -auto-approve
        working-directory: infrastructure
        env:
          TF_WORKSPACE: dev

  deploy-production:
    name: Deploy to Production
    runs-on: ubuntu-latest
    needs: validate
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    environment: production
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2
        with:
          terraform_version: ${{ env.TF_VERSION }}

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v2
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Terraform Init
        run: terraform init
        working-directory: infrastructure
        env:
          TF_WORKSPACE: production

      - name: Terraform Plan
        run: |
          terraform plan \
            -var-file="environments/production.tfvars" \
            -out="production.tfplan"
        working-directory: infrastructure
        env:
          TF_WORKSPACE: production

      - name: Manual Approval Required
        uses: trstringer/manual-approval@v1
        with:
          secret: ${{ github.TOKEN }}
          approvers: platform-team
          minimum-approvals: 2

      - name: Terraform Apply
        run: terraform apply production.tfplan
        working-directory: infrastructure
        env:
          TF_WORKSPACE: production
```

## 📋 Best Practices Checklist

### Security Best Practices

- [ ] **Encryption**
  - [ ] All data encrypted at rest
  - [ ] All data encrypted in transit
  - [ ] Key rotation policies implemented
  - [ ] Customer-managed encryption keys

- [ ] **Network Security**
  - [ ] Private subnets for databases
  - [ ] Security groups with least privilege
  - [ ] Network ACLs configured
  - [ ] VPC Flow Logs enabled

- [ ] **Access Control**
  - [ ] IAM roles instead of users
  - [ ] Principle of least privilege
  - [ ] Regular access reviews
  - [ ] MFA enforcement

- [ ] **Compliance**
  - [ ] Resource tagging strategy
  - [ ] Audit logging enabled
  - [ ] Backup and retention policies
  - [ ] Disaster recovery plan

### Operational Best Practices

- [ ] **Monitoring**
  - [ ] Infrastructure monitoring
  - [ ] Application performance monitoring
  - [ ] Log aggregation and analysis
  - [ ] Alerting and notification

- [ ] **Automation**
  - [ ] Infrastructure as Code
  - [ ] Automated testing
  - [ ] CI/CD pipelines
  - [ ] Auto-scaling configuration

- [ ] **Cost Optimization**
  - [ ] Right-sizing resources
  - [ ] Reserved instances for production
  - [ ] Cost monitoring and alerts
  - [ ] Resource cleanup automation

This Infrastructure as Code template provides comprehensive patterns for managing fintech infrastructure with security, compliance, and operational excellence in mind.