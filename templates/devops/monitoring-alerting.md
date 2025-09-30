# Monitoring and Alerting Templates

## 🎯 Overview

This template provides comprehensive monitoring and alerting strategies for fintech applications using modern observability tools. It covers Prometheus, Grafana, ELK Stack, distributed tracing, and AI-enhanced monitoring approaches.

## 📊 AI Prompt Templates

### Prometheus Monitoring Setup

#### Comprehensive Monitoring Stack

```text
Create a complete Prometheus monitoring setup for a fintech payment application with the following requirements:

**Infrastructure Monitoring:**
- Kubernetes cluster metrics (nodes, pods, deployments)
- Application server metrics (CPU, memory, disk, network)
- Database performance metrics (PostgreSQL, Redis)
- Load balancer and ingress metrics
- Certificate expiration monitoring
- SSL/TLS security monitoring

**Application Metrics:**
- Payment processing metrics (success rate, latency, volume)
- API endpoint performance and error rates
- User authentication and authorization metrics
- Fraud detection system metrics
- Database query performance
- External service integration health

**Business Metrics:**
- Transaction volume and revenue tracking
- User registration and activation rates
- Payment method usage patterns
- Geographic transaction distribution
- Customer acquisition cost metrics
- Churn rate and retention metrics

**Security Monitoring:**
- Failed authentication attempts
- Suspicious IP address activity
- Rate limiting violations
- Security policy violations
- Data access patterns
- Compliance audit metrics

**Alert Configuration:**
- Critical alerts (system down, payment failures)
- Warning alerts (high latency, resource usage)
- Business alerts (transaction volume drops, fraud spikes)
- Security alerts (intrusion attempts, policy violations)
- SLA breach notifications
- Escalation policies and notification channels

Please include:
1. Prometheus configuration files
2. Service discovery setup
3. Recording rules for complex metrics
4. Alert rules with appropriate thresholds
5. Grafana dashboard configurations
6. Integration with PagerDuty/Slack
7. Multi-environment setup (dev, staging, prod)
```

#### Kubernetes Monitoring Configuration

```text
Design Kubernetes-native monitoring for a fintech microservices architecture:

**Cluster Monitoring:**
- Node resource utilization and health
- Pod resource consumption and limits
- Deployment rollout status and health
- Service mesh metrics (Istio/Linkerd)
- Ingress controller performance
- Persistent volume usage

**Application Monitoring:**
- Service-to-service communication latency
- gRPC and HTTP request metrics
- Database connection pool metrics
- Message queue depth and processing rates
- Cache hit/miss ratios
- Background job processing

**Security Monitoring:**
- Pod security policy violations
- Network policy enforcement
- RBAC access patterns
- Image vulnerability scanning results
- Runtime security events
- Compliance posture monitoring

**Cost Monitoring:**
- Resource allocation vs usage
- Cluster autoscaling efficiency
- Storage costs and optimization
- Network data transfer costs
- Multi-cloud cost comparison
- FinOps dashboards and reports

Include ServiceMonitor, PodMonitor, and PrometheusRule custom resources.
```

### Grafana Dashboard Templates

#### Executive Dashboard

```text
Create executive-level Grafana dashboards for fintech leadership:

**Financial Performance:**
- Real-time revenue tracking
- Transaction volume trends
- Payment success rates
- Average transaction value
- Geographic revenue distribution
- Payment method preference analysis

**Operational Health:**
- System uptime and availability
- Response time percentiles
- Error rate trends
- Customer satisfaction metrics
- Support ticket volume
- Incident resolution times

**Security Posture:**
- Fraud detection effectiveness
- Security incident trends
- Compliance score tracking
- Data breach indicators
- Threat intelligence feeds
- Risk assessment metrics

**Business Growth:**
- Customer acquisition rates
- User engagement metrics
- Product adoption rates
- Market expansion tracking
- Competitive analysis
- Regulatory compliance status

Include drill-down capabilities, time range selectors, and export functionality.
```

#### Technical Operations Dashboard

```text
Design comprehensive technical dashboards for operations teams:

**Infrastructure Health:**
- Server and container resource usage
- Database performance metrics
- Network latency and throughput
- Storage utilization and IOPS
- Load balancer distribution
- CDN cache performance

**Application Performance:**
- API response times and error rates
- Database query performance
- Cache efficiency metrics
- Third-party service dependencies
- Background job processing
- Memory and CPU profiling

**Security Operations:**
- Intrusion detection alerts
- Authentication failure patterns
- Rate limiting effectiveness
- SSL certificate status
- Vulnerability scan results
- Compliance audit trails

**DevOps Metrics:**
- Deployment frequency and success
- Lead time for changes
- Mean time to recovery
- Change failure rate
- Pipeline execution times
- Infrastructure drift detection

Include alerting annotations, log correlation, and troubleshooting runbooks.
```

## 🔧 Prometheus Configuration

### Core Prometheus Setup

```yaml
# prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s
  external_labels:
    environment: 'production'
    cluster: 'fintech-main'

rule_files:
  - "/etc/prometheus/rules/*.yml"

alerting:
  alertmanagers:
    - static_configs:
        - targets:
          - alertmanager:9093

scrape_configs:
  # Prometheus itself
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  # Kubernetes API Server
  - job_name: 'kubernetes-apiservers'
    kubernetes_sd_configs:
      - role: endpoints
        namespaces:
          names:
            - default
    scheme: https
    tls_config:
      ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
      insecure_skip_verify: true
    bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
    relabel_configs:
      - source_labels: [__meta_kubernetes_namespace, __meta_kubernetes_service_name, __meta_kubernetes_endpoint_port_name]
        action: keep
        regex: default;kubernetes;https

  # Kubernetes Nodes
  - job_name: 'kubernetes-nodes'
    kubernetes_sd_configs:
      - role: node
    scheme: https
    tls_config:
      ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
      insecure_skip_verify: true
    bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
    relabel_configs:
      - action: labelmap
        regex: __meta_kubernetes_node_label_(.+)

  # Kubernetes Pods
  - job_name: 'kubernetes-pods'
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        regex: true
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
        action: replace
        target_label: __metrics_path__
        regex: (.+)
      - source_labels: [__address__, __meta_kubernetes_pod_annotation_prometheus_io_port]
        action: replace
        regex: ([^:]+)(?::\d+)?;(\d+)
        replacement: $1:$2
        target_label: __address__
      - action: labelmap
        regex: __meta_kubernetes_pod_label_(.+)
      - source_labels: [__meta_kubernetes_namespace]
        action: replace
        target_label: kubernetes_namespace
      - source_labels: [__meta_kubernetes_pod_name]
        action: replace
        target_label: kubernetes_pod_name

  # Application Services
  - job_name: 'payment-service'
    static_configs:
      - targets: ['payment-service:8080']
    metrics_path: '/actuator/prometheus'
    scrape_interval: 10s
    relabel_configs:
      - source_labels: [__address__]
        target_label: service
        replacement: 'payment-service'

  - job_name: 'user-service'
    static_configs:
      - targets: ['user-service:8080']
    metrics_path: '/actuator/prometheus'
    scrape_interval: 10s
    relabel_configs:
      - source_labels: [__address__]
        target_label: service
        replacement: 'user-service'

  - job_name: 'api-gateway'
    static_configs:
      - targets: ['api-gateway:8080']
    metrics_path: '/metrics'
    scrape_interval: 10s
    relabel_configs:
      - source_labels: [__address__]
        target_label: service
        replacement: 'api-gateway'

  # Database Monitoring
  - job_name: 'postgres-exporter'
    static_configs:
      - targets: ['postgres-exporter:9187']
    scrape_interval: 30s

  - job_name: 'redis-exporter'
    static_configs:
      - targets: ['redis-exporter:9121']
    scrape_interval: 30s

  # Infrastructure Monitoring
  - job_name: 'node-exporter'
    static_configs:
      - targets: 
        - 'node-exporter-1:9100'
        - 'node-exporter-2:9100'
        - 'node-exporter-3:9100'
    scrape_interval: 30s

  - job_name: 'nginx-exporter'
    static_configs:
      - targets: ['nginx-exporter:9113']
    scrape_interval: 30s

  # External Service Monitoring
  - job_name: 'blackbox-http'
    metrics_path: /probe
    params:
      module: [http_2xx]
    static_configs:
      - targets:
        - https://api.example.com/health
        - https://payment-gateway.example.com/status
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: blackbox-exporter:9115

  - job_name: 'blackbox-tcp'
    metrics_path: /probe
    params:
      module: [tcp_connect]
    static_configs:
      - targets:
        - postgres:5432
        - redis:6379
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: blackbox-exporter:9115
```

### Recording Rules

```yaml
# rules/application-rules.yml
groups:
  - name: application.rules
    rules:
      # Payment Processing Metrics
      - record: payment:success_rate_5m
        expr: |
          (
            sum(rate(payment_requests_total{status="success"}[5m])) by (service, environment)
            /
            sum(rate(payment_requests_total[5m])) by (service, environment)
          ) * 100

      - record: payment:error_rate_5m
        expr: |
          (
            sum(rate(payment_requests_total{status=~"error|failed"}[5m])) by (service, environment)
            /
            sum(rate(payment_requests_total[5m])) by (service, environment)
          ) * 100

      - record: payment:latency_p95_5m
        expr: histogram_quantile(0.95, sum(rate(payment_request_duration_seconds_bucket[5m])) by (le, service, environment))

      - record: payment:latency_p99_5m
        expr: histogram_quantile(0.99, sum(rate(payment_request_duration_seconds_bucket[5m])) by (le, service, environment))

      # API Gateway Metrics
      - record: api:request_rate_5m
        expr: sum(rate(http_requests_total[5m])) by (service, method, endpoint, environment)

      - record: api:error_rate_5m
        expr: |
          (
            sum(rate(http_requests_total{status=~"5.."}[5m])) by (service, method, endpoint, environment)
            /
            sum(rate(http_requests_total[5m])) by (service, method, endpoint, environment)
          ) * 100

      - record: api:latency_p95_5m
        expr: histogram_quantile(0.95, sum(rate(http_request_duration_seconds_bucket[5m])) by (le, service, method, endpoint, environment))

      # Database Metrics
      - record: db:query_rate_5m
        expr: sum(rate(postgresql_stat_database_xact_commit[5m])) by (instance, datname)

      - record: db:connection_utilization
        expr: |
          (
            postgresql_stat_database_numbackends
            /
            postgresql_settings_max_connections
          ) * 100

      # Business Metrics
      - record: business:revenue_5m
        expr: sum(rate(payment_amount_total[5m])) by (currency, environment)

      - record: business:transaction_volume_5m
        expr: sum(rate(payment_requests_total{status="success"}[5m])) by (payment_method, environment)

      - record: business:user_registration_rate_5m
        expr: sum(rate(user_registrations_total[5m])) by (environment)

  - name: infrastructure.rules
    rules:
      # CPU Utilization
      - record: instance:cpu_utilization_5m
        expr: |
          (
            1 - avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) by (instance)
          ) * 100

      # Memory Utilization
      - record: instance:memory_utilization
        expr: |
          (
            1 - (
              node_memory_MemAvailable_bytes
              /
              node_memory_MemTotal_bytes
            )
          ) * 100

      # Disk Utilization
      - record: instance:disk_utilization
        expr: |
          (
            1 - (
              node_filesystem_avail_bytes{fstype!="tmpfs"}
              /
              node_filesystem_size_bytes{fstype!="tmpfs"}
            )
          ) * 100

      # Network Throughput
      - record: instance:network_receive_bytes_5m
        expr: sum(rate(node_network_receive_bytes_total[5m])) by (instance)

      - record: instance:network_transmit_bytes_5m
        expr: sum(rate(node_network_transmit_bytes_total[5m])) by (instance)
```

### Alert Rules

```yaml
# rules/alerts.yml
groups:
  - name: critical-alerts
    rules:
      # Service Down Alerts
      - alert: ServiceDown
        expr: up == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Service {{ $labels.job }} is down"
          description: "Service {{ $labels.job }} on {{ $labels.instance }} has been down for more than 1 minute."
          runbook_url: "https://runbooks.example.com/service-down"

      # Payment System Alerts
      - alert: PaymentSuccessRateLow
        expr: payment:success_rate_5m < 95
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "Payment success rate is below 95%"
          description: "Payment success rate is {{ $value }}% for service {{ $labels.service }} in {{ $labels.environment }}"
          runbook_url: "https://runbooks.example.com/payment-failures"

      - alert: PaymentLatencyHigh
        expr: payment:latency_p95_5m > 3
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Payment processing latency is high"
          description: "95th percentile payment latency is {{ $value }}s for service {{ $labels.service }}"
          runbook_url: "https://runbooks.example.com/high-latency"

      # API Alerts
      - alert: APIErrorRateHigh
        expr: api:error_rate_5m > 5
        for: 3m
        labels:
          severity: critical
        annotations:
          summary: "API error rate is high"
          description: "API error rate is {{ $value }}% for {{ $labels.service }}/{{ $labels.endpoint }}"
          runbook_url: "https://runbooks.example.com/api-errors"

      - alert: APILatencyHigh
        expr: api:latency_p95_5m > 1
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "API response time is high"
          description: "95th percentile API latency is {{ $value }}s for {{ $labels.service }}/{{ $labels.endpoint }}"

      # Database Alerts
      - alert: DatabaseDown
        expr: postgresql_up == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "PostgreSQL database is down"
          description: "PostgreSQL database {{ $labels.datname }} on {{ $labels.instance }} is down"
          runbook_url: "https://runbooks.example.com/database-down"

      - alert: DatabaseConnectionsHigh
        expr: db:connection_utilization > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Database connection utilization is high"
          description: "Database connection utilization is {{ $value }}% on {{ $labels.instance }}"

      - alert: DatabaseSlowQueries
        expr: postgresql_stat_activity_max_tx_duration > 300
        for: 2m
        labels:
          severity: warning
        annotations:
          summary: "Database has slow running queries"
          description: "Longest running transaction is {{ $value }}s on {{ $labels.instance }}"

  - name: infrastructure-alerts
    rules:
      # System Resource Alerts
      - alert: HighCPUUsage
        expr: instance:cpu_utilization_5m > 80
        for: 10m
        labels:
          severity: warning
        annotations:
          summary: "High CPU usage detected"
          description: "CPU usage is {{ $value }}% on {{ $labels.instance }}"

      - alert: HighMemoryUsage
        expr: instance:memory_utilization > 85
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High memory usage detected"
          description: "Memory usage is {{ $value }}% on {{ $labels.instance }}"

      - alert: DiskSpaceLow
        expr: instance:disk_utilization > 90
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "Disk space is running low"
          description: "Disk usage is {{ $value }}% on {{ $labels.instance }}"

      # Kubernetes Alerts
      - alert: PodCrashLooping
        expr: rate(kube_pod_container_status_restarts_total[15m]) > 0
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "Pod is crash looping"
          description: "Pod {{ $labels.pod }} in namespace {{ $labels.namespace }} is crash looping"

      - alert: PodNotReady
        expr: kube_pod_status_ready{condition="false"} == 1
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Pod is not ready"
          description: "Pod {{ $labels.pod }} in namespace {{ $labels.namespace }} is not ready"

  - name: security-alerts
    rules:
      # Authentication Alerts
      - alert: HighFailedAuthAttempts
        expr: sum(rate(auth_requests_total{status="failed"}[5m])) by (source_ip) > 10
        for: 2m
        labels:
          severity: warning
        annotations:
          summary: "High number of failed authentication attempts"
          description: "{{ $value }} failed auth attempts per second from IP {{ $labels.source_ip }}"

      # Security Policy Violations
      - alert: SecurityPolicyViolation
        expr: increase(security_policy_violations_total[5m]) > 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Security policy violation detected"
          description: "Security policy violation detected: {{ $labels.policy }} from {{ $labels.source }}"

      # SSL Certificate Expiration
      - alert: SSLCertificateExpiring
        expr: probe_ssl_earliest_cert_expiry - time() < 86400 * 7
        for: 1m
        labels:
          severity: warning
        annotations:
          summary: "SSL certificate expiring soon"
          description: "SSL certificate for {{ $labels.instance }} expires in {{ $value | humanizeDuration }}"

  - name: business-alerts
    rules:
      # Revenue Alerts  
      - alert: RevenueDrop
        expr: |
          (
            business:revenue_5m
            /
            (business:revenue_5m offset 1d)
          ) < 0.8
        for: 10m
        labels:
          severity: warning
        annotations:
          summary: "Revenue has dropped significantly"
          description: "Revenue is {{ $value | humanizePercentage }} of yesterday's level"

      # Transaction Volume Alerts
      - alert: TransactionVolumeDrop
        expr: |
          (
            business:transaction_volume_5m
            /
            (business:transaction_volume_5m offset 1d)
          ) < 0.7
        for: 15m
        labels:
          severity: warning
        annotations:
          summary: "Transaction volume has dropped"
          description: "Transaction volume is {{ $value | humanizePercentage }} of yesterday's level"

      # Fraud Detection Alerts
      - alert: FraudSpike
        expr: sum(rate(fraud_detected_total[5m])) by (environment) > 0.1
        for: 3m
        labels:
          severity: critical
        annotations:
          summary: "Fraud detection spike"
          description: "Fraud detection rate is {{ $value }} per second in {{ $labels.environment }}"
```

## 📊 Grafana Dashboard Configurations

### Executive Dashboard JSON

```json
{
  "dashboard": {
    "id": null,
    "title": "Executive Dashboard",
    "tags": ["executive", "business", "fintech"],
    "timezone": "browser",
    "panels": [
      {
        "id": 1,
        "title": "Real-time Revenue",
        "type": "stat",
        "targets": [
          {
            "expr": "sum(business:revenue_5m) by (currency)",
            "legendFormat": "{{currency}}"
          }
        ],
        "fieldConfig": {
          "defaults": {
            "color": {
              "mode": "palette-classic"
            },
            "custom": {
              "displayMode": "list",
              "orientation": "horizontal"
            },
            "mappings": [],
            "thresholds": {
              "steps": [
                {
                  "color": "green",
                  "value": null
                }
              ]
            },
            "unit": "currencyUSD"
          }
        },
        "gridPos": {
          "h": 8,
          "w": 12,
          "x": 0,
          "y": 0
        }
      },
      {
        "id": 2,
        "title": "Payment Success Rate",
        "type": "stat",
        "targets": [
          {
            "expr": "avg(payment:success_rate_5m)",
            "legendFormat": "Success Rate"
          }
        ],
        "fieldConfig": {
          "defaults": {
            "color": {
              "mode": "thresholds"
            },
            "mappings": [],
            "thresholds": {
              "steps": [
                {
                  "color": "red",
                  "value": null
                },
                {
                  "color": "yellow",
                  "value": 95
                },
                {
                  "color": "green",
                  "value": 99
                }
              ]
            },
            "unit": "percent"
          }
        },
        "gridPos": {
          "h": 8,
          "w": 12,
          "x": 12,
          "y": 0
        }
      },
      {
        "id": 3,
        "title": "Transaction Volume Trend",
        "type": "timeseries",
        "targets": [
          {
            "expr": "sum(business:transaction_volume_5m) by (payment_method)",
            "legendFormat": "{{payment_method}}"
          }
        ],
        "fieldConfig": {
          "defaults": {
            "color": {
              "mode": "palette-classic"
            },
            "custom": {
              "axisLabel": "",
              "axisPlacement": "auto",
              "barAlignment": 0,
              "drawStyle": "line",
              "fillOpacity": 10,
              "gradientMode": "none",
              "hideFrom": {
                "legend": false,
                "tooltip": false,
                "vis": false
              },
              "lineInterpolation": "linear",
              "lineWidth": 1,
              "pointSize": 5,
              "scaleDistribution": {
                "type": "linear"
              },
              "showPoints": "never",
              "spanNulls": false,
              "stacking": {
                "group": "A",
                "mode": "none"
              },
              "thresholdsStyle": {
                "mode": "off"
              }
            },
            "mappings": [],
            "thresholds": {
              "steps": [
                {
                  "color": "green",
                  "value": null
                }
              ]
            },
            "unit": "reqps"
          }
        },
        "gridPos": {
          "h": 8,
          "w": 24,
          "x": 0,
          "y": 8
        }
      },
      {
        "id": 4,
        "title": "System Health Overview",
        "type": "table",
        "targets": [
          {
            "expr": "up",
            "format": "table",
            "instant": true
          }
        ],
        "transformations": [
          {
            "id": "organize",
            "options": {
              "excludeByName": {
                "Time": true,
                "__name__": true
              },
              "indexByName": {},
              "renameByName": {
                "Value": "Status",
                "job": "Service",
                "instance": "Instance"
              }
            }
          }
        ],
        "fieldConfig": {
          "defaults": {
            "color": {
              "mode": "thresholds"
            },
            "custom": {
              "align": "auto",
              "displayMode": "color-background"
            },
            "mappings": [
              {
                "options": {
                  "0": {
                    "color": "red",
                    "index": 0,
                    "text": "Down"
                  },
                  "1": {
                    "color": "green",
                    "index": 1,
                    "text": "Up"
                  }
                },
                "type": "value"
              }
            ],
            "thresholds": {
              "steps": [
                {
                  "color": "green",
                  "value": null
                }
              ]
            }
          }
        },
        "gridPos": {
          "h": 8,
          "w": 24,
          "x": 0,
          "y": 16
        }
      }
    ],
    "time": {
      "from": "now-1h",
      "to": "now"
    },
    "timepicker": {},
    "timezone": "",
    "title": "Executive Dashboard",
    "uid": "executive-dashboard",
    "version": 1,
    "weekStart": ""
  }
}
```

## 🔔 AlertManager Configuration

### AlertManager Setup

```yaml
# alertmanager.yml
global:
  smtp_smarthost: 'smtp.gmail.com:587'
  smtp_from: 'alerts@fintech-company.com'
  smtp_auth_username: 'alerts@fintech-company.com'
  smtp_auth_password: 'app-password'

templates:
  - '/etc/alertmanager/templates/*.tmpl'

route:
  group_by: ['alertname', 'cluster', 'service']
  group_wait: 10s
  group_interval: 10s
  repeat_interval: 1h
  receiver: 'default'
  routes:
    - match:
        severity: critical
      receiver: 'critical-alerts'
      group_wait: 10s
      repeat_interval: 5m
    
    - match:
        severity: warning
      receiver: 'warning-alerts'
      group_wait: 30s
      repeat_interval: 30m
    
    - match_re:
        alertname: '^Payment.*'
      receiver: 'payment-team'
      group_wait: 5s
      repeat_interval: 2m
    
    - match_re:
        alertname: '^Database.*'
      receiver: 'database-team'
      group_wait: 15s
      repeat_interval: 10m

    - match_re:
        alertname: '^Security.*|.*Fraud.*'
      receiver: 'security-team'
      group_wait: 5s
      repeat_interval: 1m

receivers:
  - name: 'default'
    email_configs:
      - to: 'devops@fintech-company.com'
        subject: '[Default] Alert: {{ .GroupLabels.alertname }}'
        body: |
          {{ range .Alerts }}
          Alert: {{ .Annotations.summary }}
          Description: {{ .Annotations.description }}
          {{ end }}

  - name: 'critical-alerts'
    email_configs:
      - to: 'oncall@fintech-company.com'
        subject: '[CRITICAL] Alert: {{ .GroupLabels.alertname }}'
        body: |
          {{ range .Alerts }}
          CRITICAL ALERT
          
          Alert: {{ .Annotations.summary }}
          Description: {{ .Annotations.description }}
          Runbook: {{ .Annotations.runbook_url }}
          
          Labels:
          {{ range .Labels.SortedPairs }}  {{ .Name }}: {{ .Value }}
          {{ end }}
          {{ end }}
    
    slack_configs:
      - api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
        channel: '#critical-alerts'
        title: 'Critical Alert'
        text: |
          {{ range .Alerts }}
          *{{ .Annotations.summary }}*
          {{ .Annotations.description }}
          <{{ .Annotations.runbook_url }}|Runbook>
          {{ end }}
        color: 'danger'
    
    pagerduty_configs:
      - routing_key: 'YOUR_PAGERDUTY_INTEGRATION_KEY'
        description: '{{ .GroupLabels.alertname }}: {{ .Alerts | len }} alerts'
        details:
          summary: '{{ range .Alerts }}{{ .Annotations.summary }}{{ end }}'
          environment: '{{ .GroupLabels.environment }}'

  - name: 'warning-alerts'
    email_configs:
      - to: 'alerts@fintech-company.com'
        subject: '[WARNING] Alert: {{ .GroupLabels.alertname }}'
        body: |
          {{ range .Alerts }}
          WARNING ALERT
          
          Alert: {{ .Annotations.summary }}
          Description: {{ .Annotations.description }}
          {{ end }}
    
    slack_configs:
      - api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
        channel: '#alerts'
        title: 'Warning Alert'
        text: |
          {{ range .Alerts }}
          {{ .Annotations.summary }}
          {{ .Annotations.description }}
          {{ end }}
        color: 'warning'

  - name: 'payment-team'
    email_configs:
      - to: 'payment-team@fintech-company.com'
        subject: '[PAYMENT] Alert: {{ .GroupLabels.alertname }}'
        body: |
          {{ range .Alerts }}
          PAYMENT SYSTEM ALERT
          
          Alert: {{ .Annotations.summary }}
          Description: {{ .Annotations.description }}
          Runbook: {{ .Annotations.runbook_url }}
          {{ end }}
    
    slack_configs:
      - api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
        channel: '#payment-alerts'
        title: 'Payment System Alert'
        text: |
          {{ range .Alerts }}
          *{{ .Annotations.summary }}*
          {{ .Annotations.description }}
          {{ end }}
        color: 'danger'

  - name: 'database-team'
    email_configs:
      - to: 'database-team@fintech-company.com'
        subject: '[DATABASE] Alert: {{ .GroupLabels.alertname }}'
        body: |
          {{ range .Alerts }}
          DATABASE ALERT
          
          Alert: {{ .Annotations.summary }}
          Description: {{ .Annotations.description }}
          {{ end }}

  - name: 'security-team'
    email_configs:
      - to: 'security-team@fintech-company.com'
        subject: '[SECURITY] Alert: {{ .GroupLabels.alertname }}'
        body: |
          {{ range .Alerts }}
          SECURITY ALERT
          
          Alert: {{ .Annotations.summary }}
          Description: {{ .Annotations.description }}
          
          This requires immediate attention!
          {{ end }}
    
    slack_configs:
      - api_url: 'https://hooks.slack.com/services/YOUR/SLACK/WEBHOOK'
        channel: '#security-alerts'
        title: 'Security Alert'
        text: |
          {{ range .Alerts }}
          🚨 *SECURITY ALERT* 🚨
          *{{ .Annotations.summary }}*
          {{ .Annotations.description }}
          {{ end }}
        color: 'danger'

inhibit_rules:
  - source_match:
      severity: 'critical'
    target_match:
      severity: 'warning'
    equal: ['alertname', 'cluster', 'service']
```

## 📈 ELK Stack Configuration

### Logstash Configuration

```yaml
# logstash.conf
input {
  beats {
    port => 5044
  }
  
  http {
    port => 8080
    codec => json
  }
}

filter {
  # Parse application logs
  if [fields][service] {
    mutate {
      add_field => { "service" => "%{[fields][service]}" }
    }
  }

  # Parse payment logs
  if [service] == "payment-service" {
    grok {
      match => { 
        "message" => "\[%{TIMESTAMP_ISO8601:timestamp}\] %{LOGLEVEL:level} %{DATA:logger} - %{GREEDYDATA:log_message}"
      }
    }
    
    if [log_message] =~ /payment_id/ {
      grok {
        match => {
          "log_message" => "payment_id=%{UUID:payment_id} amount=%{NUMBER:amount} currency=%{WORD:currency} status=%{WORD:payment_status}"
        }
      }
      
      mutate {
        convert => { "amount" => "float" }
      }
    }
  }

  # Parse API gateway logs
  if [service] == "api-gateway" {
    grok {
      match => {
        "message" => "%{IPORHOST:remote_ip} - %{DATA:user_name} \[%{HTTPDATE:access_time}\] \"%{WORD:http_method} %{DATA:url} HTTP/%{NUMBER:http_version}\" %{NUMBER:response_code} %{NUMBER:body_sent} \"%{DATA:referer}\" \"%{DATA:agent}\" %{NUMBER:request_time}"
      }
    }
    
    mutate {
      convert => { 
        "response_code" => "integer"
        "body_sent" => "integer"
        "request_time" => "float"
      }
    }
    
    date {
      match => [ "access_time", "dd/MMM/yyyy:HH:mm:ss Z" ]
    }
  }

  # Parse database logs
  if [service] == "postgres" {
    grok {
      match => {
        "message" => "%{TIMESTAMP_ISO8601:timestamp} \[%{NUMBER:pid}\] %{WORD:level}:  %{GREEDYDATA:log_message}"
      }
    }
    
    if [log_message] =~ /duration:/ {
      grok {
        match => {
          "log_message" => "duration: %{NUMBER:query_duration} ms  statement: %{GREEDYDATA:sql_query}"
        }
      }
      
      mutate {
        convert => { "query_duration" => "float" }
      }
    }
  }

  # Add common fields
  mutate {
    add_field => { 
      "[@metadata][environment]" => "${ENVIRONMENT:dev}"
      "[@metadata][datacenter]" => "${DATACENTER:us-west-2}"
    }
  }

  # Parse JSON logs
  if [message] =~ /^\{/ {
    json {
      source => "message"
    }
  }

  # GeoIP lookup for security analysis
  if [remote_ip] {
    geoip {
      source => "remote_ip"
      target => "geoip"
    }
  }
}

output {
  elasticsearch {
    hosts => ["elasticsearch:9200"]
    index => "fintech-logs-%{[@metadata][environment]}-%{+YYYY.MM.dd}"
    template_name => "fintech-logs"
    template => "/usr/share/logstash/templates/fintech-logs.json"
    template_overwrite => true
  }

  # Output to separate indices for different log types
  if [service] == "payment-service" {
    elasticsearch {
      hosts => ["elasticsearch:9200"]
      index => "payment-logs-%{+YYYY.MM.dd}"
    }
  }

  if [level] == "ERROR" or [response_code] >= 500 {
    elasticsearch {
      hosts => ["elasticsearch:9200"]
      index => "error-logs-%{+YYYY.MM.dd}"
    }
  }

  # Security logs to separate index
  if [service] == "security" or [log_message] =~ /failed.*auth/ {
    elasticsearch {
      hosts => ["elasticsearch:9200"]
      index => "security-logs-%{+YYYY.MM.dd}"
    }
  }

  stdout {
    codec => rubydebug
  }
}
```

This comprehensive monitoring and alerting template provides enterprise-grade observability for fintech applications with proper security, compliance, and operational considerations.