# Performance Testing Guide

## 🎯 Overview

This guide provides comprehensive strategies for performance testing the PayFlow fintech application. It covers load testing, stress testing, scalability testing, and performance optimization techniques using AI-enhanced approaches.

## 📊 Performance Testing Strategy

### Testing Objectives

#### Primary Performance Goals
- **Response Time**: API endpoints respond within 200ms (95th percentile)
- **Throughput**: Support 1000+ transactions per second
- **Concurrency**: Handle 10,000+ concurrent users
- **Availability**: Maintain 99.9% uptime
- **Scalability**: Linear scaling with infrastructure increases

#### Fintech-Specific Requirements
- **Payment Processing**: Complete transactions within 3 seconds
- **Fraud Detection**: Process risk assessments within 100ms
- **Compliance Reporting**: Generate reports within acceptable timeframes
- **Data Consistency**: Ensure ACID compliance under load
- **Audit Logging**: Maintain complete audit trails without performance degradation

### Performance Testing Types

#### Load Testing
Tests normal expected load conditions to validate system performance meets requirements.

#### Stress Testing  
Tests beyond normal capacity to identify breaking points and failure modes.

#### Spike Testing
Tests sudden increases in load to validate auto-scaling and resilience.

#### Volume Testing
Tests system behavior with large amounts of data.

#### Endurance Testing
Tests system stability over extended periods under normal load.

## 🛠️ Performance Testing Tools

### Load Testing Tools Comparison

#### K6 (Recommended)
```javascript
// k6-payment-load-test.js
import http from 'k6/http';
import { check, sleep } from 'k6';
import { Rate } from 'k6/metrics';

// Custom metrics
const errorRate = new Rate('errors');

export const options = {
  stages: [
    { duration: '2m', target: 100 }, // Ramp up to 100 users
    { duration: '5m', target: 100 }, // Stay at 100 users
    { duration: '2m', target: 200 }, // Ramp up to 200 users
    { duration: '5m', target: 200 }, // Stay at 200 users
    { duration: '2m', target: 0 },   // Ramp down to 0 users
  ],
  thresholds: {
    http_req_duration: ['p(95)<200'], // 95% of requests under 200ms
    http_req_failed: ['rate<0.1'],    // Error rate under 10%
    errors: ['rate<0.1'],             // Custom error rate under 10%
  },
};

// Test setup
export function setup() {
  // Authenticate and get token
  const authResponse = http.post('http://localhost:3000/api/v1/auth/login', {
    email: 'loadtest@example.com',
    password: 'loadtest123'
  });
  
  return { token: authResponse.json('token') };
}

export default function(data) {
  const headers = {
    'Authorization': `Bearer ${data.token}`,
    'Content-Type': 'application/json',
  };

  // Test payment creation
  const paymentPayload = JSON.stringify({
    amount: Math.floor(Math.random() * 50000) + 1000, // $10-$500
    currency: 'USD',
    paymentMethodId: 'pm_test_card',
    description: `Load test payment ${Date.now()}`
  });

  const paymentResponse = http.post(
    'http://localhost:3000/api/v1/payments',
    paymentPayload,
    { headers }
  );

  // Validate response
  const paymentSuccess = check(paymentResponse, {
    'payment created successfully': (r) => r.status === 201,
    'payment has ID': (r) => r.json('id') !== null,
    'response time < 200ms': (r) => r.timings.duration < 200,
  });

  errorRate.add(!paymentSuccess);

  // Test payment retrieval
  if (paymentSuccess) {
    const paymentId = paymentResponse.json('id');
    const getResponse = http.get(
      `http://localhost:3000/api/v1/payments/${paymentId}`,
      { headers }
    );

    check(getResponse, {
      'payment retrieved successfully': (r) => r.status === 200,
      'payment data complete': (r) => r.json('amount') !== null,
    });
  }

  sleep(1); // Wait 1 second between iterations
}

// Teardown
export function teardown(data) {
  // Cleanup test data if needed
  console.log('Load test completed');
}
```

#### Artillery Configuration
```yaml
# artillery-config.yml
config:
  target: 'http://localhost:3000'
  phases:
    - duration: 300  # 5 minutes
      arrivalRate: 20
      name: "Warm up phase"
    - duration: 600  # 10 minutes  
      arrivalRate: 50
      name: "Normal load"
    - duration: 300  # 5 minutes
      arrivalRate: 100
      name: "Peak load"
  plugins:
    metrics-by-endpoint:
      useOnlyRequestNames: true

scenarios:
  - name: "Payment Processing Workflow"
    weight: 60
    flow:
      - post:
          url: "/api/v1/auth/login"
          json:
            email: "{{ $randomEmail() }}"
            password: "testpass123"
          capture:
            - json: "$.token"
              as: "authToken"
      - think: 2
      - post:
          url: "/api/v1/payments"
          name: "Create Payment"
          headers:
            Authorization: "Bearer {{ authToken }}"
          json:
            amount: "{{ $randomInt(1000, 100000) }}"
            currency: "USD"  
            paymentMethodId: "pm_test_visa"
          capture:
            - json: "$.id"
              as: "paymentId"
      - think: 1
      - get:
          url: "/api/v1/payments/{{ paymentId }}"
          name: "Get Payment"
          headers:
            Authorization: "Bearer {{ authToken }}"

  - name: "User Dashboard Access"
    weight: 30
    flow:
      - post:
          url: "/api/v1/auth/login"
          json:
            email: "{{ $randomEmail() }}"
            password: "testpass123"
          capture:
            - json: "$.token"
              as: "authToken"
      - get:
          url: "/api/v1/users/profile"
          name: "Get Profile"
          headers:
            Authorization: "Bearer {{ authToken }}"
      - get:
          url: "/api/v1/payments?limit=10"
          name: "Get Payment History"
          headers:
            Authorization: "Bearer {{ authToken }}"

  - name: "API Health Checks"
    weight: 10
    flow:
      - get:
          url: "/health"
          name: "Health Check"
      - get:
          url: "/api/v1/status"
          name: "API Status"
```

### Database Performance Testing

#### PostgreSQL Performance Testing
```sql
-- performance-test-queries.sql

-- Test query performance under load
EXPLAIN (ANALYZE, BUFFERS) 
SELECT 
  u.id, u.email, u.first_name, u.last_name,
  COUNT(p.id) as payment_count,
  SUM(p.amount) as total_amount,
  AVG(p.amount) as avg_amount
FROM users u
LEFT JOIN payments p ON u.id = p.customer_id
WHERE u.created_at >= NOW() - INTERVAL '30 days'
  AND u.status = 'active'
GROUP BY u.id, u.email, u.first_name, u.last_name
ORDER BY total_amount DESC
LIMIT 100;

-- Test concurrent transaction handling
BEGIN;
SELECT * FROM payments WHERE id = 'test_payment_id' FOR UPDATE;
UPDATE payments SET status = 'completed' WHERE id = 'test_payment_id';
COMMIT;

-- Test index performance
SELECT 
  schemaname,
  tablename,
  indexname,
  idx_scan,
  idx_tup_read,
  idx_tup_fetch
FROM pg_stat_user_indexes
WHERE schemaname = 'public'
ORDER BY idx_scan DESC;

-- Monitor connection and lock statistics
SELECT 
  state,
  COUNT(*) as connection_count
FROM pg_stat_activity 
WHERE datname = 'payflow_production'
GROUP BY state;
```

## 🧪 Performance Test Implementation

### API Performance Testing

#### Express.js Performance Monitoring
```typescript
// middleware/performance-monitor.ts
import { Request, Response, NextFunction } from 'express';
import { performance } from 'perf_hooks';

interface PerformanceMetrics {
  endpoint: string;
  method: string;
  responseTime: number;
  statusCode: number;
  timestamp: Date;
  memoryUsage: NodeJS.MemoryUsage;
  cpuUsage: NodeJS.CpuUsage;
}

class PerformanceMonitor {
  private metrics: PerformanceMetrics[] = [];
  private cpuStartUsage: NodeJS.CpuUsage;

  constructor() {
    this.cpuStartUsage = process.cpuUsage();
  }

  middleware() {
    return (req: Request, res: Response, next: NextFunction) => {
      const startTime = performance.now();
      const startCpu = process.cpuUsage();

      // Override res.end to capture response time
      const originalEnd = res.end;
      res.end = function(chunk?: any, encoding?: any) {
        const endTime = performance.now();
        const responseTime = endTime - startTime;
        const cpuUsage = process.cpuUsage(startCpu);

        // Record metrics
        const metrics: PerformanceMetrics = {
          endpoint: req.path,
          method: req.method,
          responseTime,
          statusCode: res.statusCode,
          timestamp: new Date(),
          memoryUsage: process.memoryUsage(),
          cpuUsage
        };

        this.recordMetrics(metrics);
        
        // Call original end
        originalEnd.call(this, chunk, encoding);
      }.bind(res);

      next();
    };
  }

  private recordMetrics(metrics: PerformanceMetrics) {
    this.metrics.push(metrics);
    
    // Keep only last 1000 entries to prevent memory leaks
    if (this.metrics.length > 1000) {
      this.metrics = this.metrics.slice(-1000);
    }

    // Log slow responses
    if (metrics.responseTime > 1000) {
      console.warn('Slow response detected:', {
        endpoint: metrics.endpoint,
        method: metrics.method,
        responseTime: metrics.responseTime,
        statusCode: metrics.statusCode
      });
    }
  }

  getMetrics(): PerformanceMetrics[] {
    return [...this.metrics];
  }

  getAverageResponseTime(endpoint?: string): number {
    const filteredMetrics = endpoint 
      ? this.metrics.filter(m => m.endpoint === endpoint)
      : this.metrics;

    if (filteredMetrics.length === 0) return 0;

    const totalTime = filteredMetrics.reduce((sum, m) => sum + m.responseTime, 0);
    return totalTime / filteredMetrics.length;
  }

  getPercentile(percentile: number, endpoint?: string): number {
    const filteredMetrics = endpoint
      ? this.metrics.filter(m => m.endpoint === endpoint)
      : this.metrics;

    if (filteredMetrics.length === 0) return 0;

    const sortedTimes = filteredMetrics
      .map(m => m.responseTime)
      .sort((a, b) => a - b);

    const index = Math.ceil((percentile / 100) * sortedTimes.length) - 1;
    return sortedTimes[index];
  }
}

export const performanceMonitor = new PerformanceMonitor();
```

#### Payment Service Performance Testing
```typescript
// tests/performance/payment-service.perf.test.ts
import { PaymentService } from '@/services/payment-service';
import { PerformanceTestHelper } from '@tests/helpers/performance-helper';

describe('PaymentService Performance Tests', () => {
  let paymentService: PaymentService;
  let perfHelper: PerformanceTestHelper;

  beforeAll(async () => {
    paymentService = new PaymentService();
    perfHelper = new PerformanceTestHelper();
  });

  describe('Payment Processing Performance', () => {
    it('should process payments within 200ms (95th percentile)', async () => {
      const testPayments = Array.from({ length: 100 }, (_, i) => ({
        amount: 10000 + i,
        currency: 'USD',
        customerId: `customer_${i}`,
        paymentMethodId: 'pm_test_card'
      }));

      const results = [];

      for (const payment of testPayments) {
        const startTime = process.hrtime.bigint();
        
        try {
          await paymentService.processPayment(payment);
          const endTime = process.hrtime.bigint();
          const duration = Number(endTime - startTime) / 1000000; // Convert to ms
          results.push(duration);
        } catch (error) {
          console.error('Payment failed:', error);
        }
      }

      // Calculate performance metrics
      const p95 = perfHelper.calculatePercentile(results, 95);
      const p99 = perfHelper.calculatePercentile(results, 99);
      const average = results.reduce((a, b) => a + b, 0) / results.length;

      console.log(`Payment Processing Performance:
        Average: ${average.toFixed(2)}ms
        95th percentile: ${p95.toFixed(2)}ms  
        99th percentile: ${p99.toFixed(2)}ms
        Total tests: ${results.length}
      `);

      // Assert performance requirements
      expect(p95).toBeLessThan(200);
      expect(average).toBeLessThan(100);
    });

    it('should handle concurrent payment processing', async () => {
      const concurrentPayments = 50;
      
      const paymentPromises = Array.from({ length: concurrentPayments }, (_, i) => ({
        amount: 10000,
        currency: 'USD',
        customerId: `concurrent_customer_${i}`,
        paymentMethodId: 'pm_test_card'
      })).map(payment => 
        perfHelper.measureAsyncOperation(() => 
          paymentService.processPayment(payment)
        )
      );

      const results = await Promise.allSettled(paymentPromises);
      
      const successful = results.filter(r => r.status === 'fulfilled');
      const failed = results.filter(r => r.status === 'rejected');

      console.log(`Concurrent Payment Results:
        Successful: ${successful.length}
        Failed: ${failed.length}
        Success Rate: ${(successful.length / concurrentPayments * 100).toFixed(2)}%
      `);

      // Assert concurrent processing requirements
      expect(successful.length / concurrentPayments).toBeGreaterThan(0.95); // 95% success rate
    });
  });

  describe('Database Query Performance', () => {
    it('should retrieve payment history efficiently', async () => {
      const customerId = 'test_customer_performance';
      
      // Create test data
      await perfHelper.createTestPayments(customerId, 1000);

      const startTime = process.hrtime.bigint();
      const payments = await paymentService.getPaymentHistory(customerId, {
        limit: 50,
        offset: 0
      });
      const endTime = process.hrtime.bigint();
      
      const duration = Number(endTime - startTime) / 1000000; // Convert to ms

      console.log(`Payment History Query Performance:
        Duration: ${duration.toFixed(2)}ms
        Results: ${payments.length}
      `);

      expect(duration).toBeLessThan(50); // Sub-50ms query time
      expect(payments.length).toBeLessThanOrEqual(50);
    });
  });

  describe('Memory Usage Performance', () => {
    it('should not leak memory during payment processing', async () => {
      const initialMemory = process.memoryUsage();
      
      // Process many payments
      for (let i = 0; i < 1000; i++) {
        await paymentService.processPayment({
          amount: 10000,
          currency: 'USD',
          customerId: `memory_test_${i}`,
          paymentMethodId: 'pm_test_card'
        });

        // Force garbage collection every 100 iterations
        if (i % 100 === 0 && global.gc) {
          global.gc();
        }
      }

      const finalMemory = process.memoryUsage();
      const memoryIncrease = finalMemory.heapUsed - initialMemory.heapUsed;
      const memoryIncreasePercent = (memoryIncrease / initialMemory.heapUsed) * 100;

      console.log(`Memory Usage Analysis:
        Initial heap: ${(initialMemory.heapUsed / 1024 / 1024).toFixed(2)}MB
        Final heap: ${(finalMemory.heapUsed / 1024 / 1024).toFixed(2)}MB
        Increase: ${(memoryIncrease / 1024 / 1024).toFixed(2)}MB (${memoryIncreasePercent.toFixed(2)}%)
      `);

      // Memory increase should be reasonable (less than 50MB or 100% increase)
      expect(memoryIncrease).toBeLessThan(50 * 1024 * 1024); // 50MB
      expect(memoryIncreasePercent).toBeLessThan(100); // 100% increase
    });
  });
});
```

### Frontend Performance Testing

#### React Component Performance
```typescript
// tests/performance/payment-form.perf.test.ts
import { render, fireEvent, waitFor } from '@testing-library/react';
import { PaymentForm } from '@/components/PaymentForm';
import { PerformanceObserver } from 'perf_hooks';

describe('PaymentForm Performance Tests', () => {
  beforeEach(() => {
    // Enable performance measurement
    if (typeof window !== 'undefined') {
      window.performance.mark = jest.fn();
      window.performance.measure = jest.fn();
    }
  });

  it('should render within performance budget', async () => {
    const startTime = performance.now();
    
    const { getByTestId } = render(
      <PaymentForm onSubmit={jest.fn()} />
    );
    
    const endTime = performance.now();
    const renderTime = endTime - startTime;

    console.log(`PaymentForm render time: ${renderTime.toFixed(2)}ms`);
    
    // Component should render within 100ms
    expect(renderTime).toBeLessThan(100);
    
    // Verify form is interactive
    expect(getByTestId('amount-input')).toBeInTheDocument();
    expect(getByTestId('submit-button')).toBeInTheDocument();
  });

  it('should handle user input without blocking UI', async () => {
    const { getByTestId } = render(
      <PaymentForm onSubmit={jest.fn()} />
    );

    const amountInput = getByTestId('amount-input');
    
    // Measure input response time
    const startTime = performance.now();
    
    fireEvent.change(amountInput, { target: { value: '100.00' } });
    
    await waitFor(() => {
      expect(amountInput).toHaveValue('100.00');
    });
    
    const endTime = performance.now();
    const inputResponseTime = endTime - startTime;

    console.log(`Input response time: ${inputResponseTime.toFixed(2)}ms`);
    
    // Input should be responsive (< 16ms for 60fps)
    expect(inputResponseTime).toBeLessThan(50);
  });

  it('should submit form within acceptable time', async () => {
    const mockSubmit = jest.fn().mockResolvedValue({ success: true });
    
    const { getByTestId } = render(
      <PaymentForm onSubmit={mockSubmit} />
    );

    // Fill form
    fireEvent.change(getByTestId('amount-input'), { target: { value: '100.00' } });
    fireEvent.change(getByTestId('card-number'), { target: { value: '4242424242424242' } });
    
    const startTime = performance.now();
    
    fireEvent.click(getByTestId('submit-button'));
    
    await waitFor(() => {
      expect(mockSubmit).toHaveBeenCalled();
    });
    
    const endTime = performance.now();
    const submitTime = endTime - startTime;

    console.log(`Form submission time: ${submitTime.toFixed(2)}ms`);
    
    // Form submission should be fast
    expect(submitTime).toBeLessThan(200);
  });
});
```

## 🤖 AI-Enhanced Performance Testing

### AI-Powered Load Testing

#### Intelligent Load Pattern Generation
```typescript
// ai-load-patterns.ts
export class AILoadPatternGenerator {
  /**
   * Generate realistic load patterns based on business requirements
   * 
   * AI Prompt: "Generate load testing patterns for a fintech payment application
   * considering business hours, geographic distribution, and seasonal variations.
   * Include gradual ramp-up, sustained load, and peak traffic scenarios."
   */
  
  static generateBusinessHoursPattern(): LoadPattern {
    return {
      name: 'Business Hours Simulation',
      stages: [
        { duration: '1m', target: 10 },   // Early morning
        { duration: '2m', target: 50 },   // Morning peak
        { duration: '3m', target: 100 },  // Midday high
        { duration: '2m', target: 150 },  // Afternoon peak
        { duration: '1m', target: 75 },   // Evening decline
        { duration: '1m', target: 20 },   // Night low
      ]
    };
  }

  static generateBlackFridayPattern(): LoadPattern {
    return {
      name: 'Black Friday Spike',
      stages: [
        { duration: '30s', target: 100 },  // Normal baseline
        { duration: '10s', target: 1000 }, // Sudden spike
        { duration: '5m', target: 800 },   // Sustained high
        { duration: '2m', target: 1200 },  // Peak surge
        { duration: '10m', target: 600 },  // Gradual decline
        { duration: '5m', target: 200 },   // Return to elevated normal
      ]
    };
  }

  static generateFailureRecoveryPattern(): LoadPattern {
    return {
      name: 'Failure Recovery',
      stages: [
        { duration: '2m', target: 200 },   // Normal load
        { duration: '30s', target: 0 },    // Complete failure
        { duration: '10s', target: 50 },   // Initial recovery
        { duration: '1m', target: 300 },   // Recovery surge
        { duration: '2m', target: 200 },   // Back to normal
      ]
    };
  }
}
```

#### AI Performance Analysis
```typescript
// ai-performance-analyzer.ts
export class AIPerformanceAnalyzer {
  /**
   * Use AI to analyze performance test results and identify bottlenecks
   * 
   * AI Prompt: "Analyze these performance metrics and identify bottlenecks,
   * optimization opportunities, and potential failure points. Provide specific
   * recommendations for improving system performance."
   */
  
  async analyzeResults(testResults: PerformanceTestResults): Promise<AnalysisReport> {
    const metrics = this.extractMetrics(testResults);
    const patterns = this.identifyPatterns(metrics);
    const bottlenecks = this.identifyBottlenecks(metrics);
    const recommendations = await this.generateRecommendations(patterns, bottlenecks);

    return {
      summary: this.generateSummary(metrics),
      bottlenecks,
      patterns,
      recommendations,
      riskAssessment: this.assessRisk(metrics),
      nextSteps: this.suggestNextSteps(bottlenecks)
    };
  }

  private identifyBottlenecks(metrics: PerformanceMetrics): Bottleneck[] {
    const bottlenecks: Bottleneck[] = [];

    // Database bottlenecks
    if (metrics.database.averageQueryTime > 100) {
      bottlenecks.push({
        type: 'database',
        severity: 'high',
        description: 'Database queries exceeding 100ms average',
        impact: 'Response time degradation',
        recommendations: [
          'Add database indexes for frequently queried columns',
          'Implement query result caching',
          'Consider database connection pooling optimization'
        ]
      });
    }

    // Memory bottlenecks
    if (metrics.memory.peakUsage > metrics.memory.available * 0.8) {
      bottlenecks.push({
        type: 'memory',
        severity: 'medium',
        description: 'Memory usage exceeding 80% of available',
        impact: 'Potential garbage collection pauses',
        recommendations: [
          'Implement object pooling for frequently created objects',
          'Review memory leaks in long-running processes',
          'Consider increasing available memory'
        ]
      });
    }

    // CPU bottlenecks
    if (metrics.cpu.averageUtilization > 70) {
      bottlenecks.push({
        type: 'cpu',
        severity: 'high',
        description: 'CPU utilization consistently above 70%',
        impact: 'Request queuing and response time increases',
        recommendations: [
          'Profile CPU-intensive operations',
          'Implement caching for expensive computations',
          'Consider horizontal scaling'
        ]
      });
    }

    return bottlenecks;
  }

  private async generateRecommendations(
    patterns: Pattern[], 
    bottlenecks: Bottleneck[]
  ): Promise<Recommendation[]> {
    // AI-generated recommendations based on patterns and bottlenecks
    const recommendations: Recommendation[] = [];

    // Performance optimization recommendations
    if (bottlenecks.some(b => b.type === 'database')) {
      recommendations.push({
        category: 'database-optimization',
        priority: 'high',
        title: 'Database Performance Optimization',
        description: 'Optimize database queries and indexing strategy',
        actions: [
          'Run EXPLAIN ANALYZE on slow queries',
          'Add composite indexes for common query patterns',
          'Implement read replicas for read-heavy operations',
          'Consider database connection pooling'
        ],
        expectedImpact: 'Response time improvement of 40-60%'
      });
    }

    // Caching recommendations
    if (patterns.some(p => p.type === 'repeated-queries')) {
      recommendations.push({
        category: 'caching',
        priority: 'medium',
        title: 'Implement Caching Strategy',
        description: 'Add caching layers to reduce database load',
        actions: [
          'Implement Redis caching for user sessions',
          'Add application-level caching for static data',
          'Use HTTP caching headers for API responses',
          'Consider CDN for static assets'
        ],
        expectedImpact: 'Database load reduction of 30-50%'
      });
    }

    return recommendations;
  }
}
```

## 📊 Performance Monitoring and Alerting

### Real-time Performance Monitoring

#### Prometheus Metrics Collection
```typescript
// metrics/performance-metrics.ts
import { register, Counter, Histogram, Gauge } from 'prom-client';

export class PerformanceMetricsCollector {
  private requestDuration: Histogram<string>;
  private requestCount: Counter<string>;
  private activeConnections: Gauge<string>;
  private databaseQueryDuration: Histogram<string>;

  constructor() {
    this.requestDuration = new Histogram({
      name: 'http_request_duration_seconds',
      help: 'Duration of HTTP requests in seconds',
      labelNames: ['method', 'route', 'status_code'],
      buckets: [0.1, 0.3, 0.5, 0.7, 1, 3, 5, 7, 10]
    });

    this.requestCount = new Counter({
      name: 'http_requests_total',
      help: 'Total number of HTTP requests',
      labelNames: ['method', 'route', 'status_code']
    });

    this.activeConnections = new Gauge({
      name: 'http_active_connections',
      help: 'Number of active HTTP connections'
    });

    this.databaseQueryDuration = new Histogram({
      name: 'database_query_duration_seconds',
      help: 'Duration of database queries in seconds',
      labelNames: ['query_type', 'table'],
      buckets: [0.01, 0.05, 0.1, 0.3, 0.5, 1, 3, 5]
    });

    register.registerMetric(this.requestDuration);
    register.registerMetric(this.requestCount);
    register.registerMetric(this.activeConnections);
    register.registerMetric(this.databaseQueryDuration);
  }

  recordRequest(method: string, route: string, statusCode: number, duration: number) {
    this.requestDuration
      .labels(method, route, statusCode.toString())
      .observe(duration);
    
    this.requestCount
      .labels(method, route, statusCode.toString())
      .inc();
  }

  recordDatabaseQuery(queryType: string, table: string, duration: number) {
    this.databaseQueryDuration
      .labels(queryType, table)
      .observe(duration);
  }

  setActiveConnections(count: number) {
    this.activeConnections.set(count);
  }
}

export const performanceMetrics = new PerformanceMetricsCollector();
```

#### Alert Configuration
```yaml
# alerting/performance-alerts.yml
groups:
  - name: payflow-performance
    rules:
      - alert: HighResponseTime
        expr: histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m])) > 0.2
        for: 2m
        labels:
          severity: warning
          service: payflow-api
        annotations:
          summary: "High response time detected"
          description: "95th percentile response time is {{ $value }}s for the last 5 minutes"

      - alert: HighErrorRate
        expr: rate(http_requests_total{status_code=~"5.."}[5m]) / rate(http_requests_total[5m]) > 0.05
        for: 1m
        labels:
          severity: critical
          service: payflow-api
        annotations:
          summary: "High error rate detected"
          description: "Error rate is {{ $value | humanizePercentage }} for the last 5 minutes"

      - alert: DatabaseSlowQueries
        expr: histogram_quantile(0.95, rate(database_query_duration_seconds_bucket[5m])) > 0.1
        for: 3m
        labels:
          severity: warning
          service: payflow-database
        annotations:
          summary: "Slow database queries detected"
          description: "95th percentile database query time is {{ $value }}s"

      - alert: MemoryUsageHigh
        expr: (node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes > 0.8
        for: 5m
        labels:
          severity: warning
          service: payflow-infrastructure
        annotations:
          summary: "High memory usage"
          description: "Memory usage is above 80% for 5 minutes"
```

## 📈 Performance Optimization Strategies

### Code-Level Optimizations

#### Database Query Optimization
```typescript
// Optimized query example
class OptimizedPaymentRepository {
  // ❌ Inefficient: N+1 query problem
  async getPaymentsWithUsersBad(limit: number): Promise<PaymentWithUser[]> {
    const payments = await this.db.query('SELECT * FROM payments LIMIT $1', [limit]);
    
    const paymentsWithUsers = [];
    for (const payment of payments.rows) {
      const user = await this.db.query('SELECT * FROM users WHERE id = $1', [payment.user_id]);
      paymentsWithUsers.push({
        ...payment,
        user: user.rows[0]
      });
    }
    
    return paymentsWithUsers;
  }

  // ✅ Efficient: Single query with JOIN
  async getPaymentsWithUsersGood(limit: number): Promise<PaymentWithUser[]> {
    const result = await this.db.query(`
      SELECT 
        p.id, p.amount, p.currency, p.status, p.created_at,
        u.id as user_id, u.email, u.first_name, u.last_name
      FROM payments p
      JOIN users u ON p.user_id = u.id
      ORDER BY p.created_at DESC
      LIMIT $1
    `, [limit]);

    return result.rows.map(row => ({
      id: row.id,
      amount: row.amount,
      currency: row.currency,
      status: row.status,
      created_at: row.created_at,
      user: {
        id: row.user_id,
        email: row.email,
        first_name: row.first_name,
        last_name: row.last_name
      }
    }));
  }

  // ✅ Even better: With proper indexing and pagination
  async getPaymentsWithUsersPaginated(
    limit: number, 
    offset: number,
    userId?: string
  ): Promise<PaymentWithUser[]> {
    const whereClause = userId ? 'WHERE p.user_id = $3' : '';
    const params = userId ? [limit, offset, userId] : [limit, offset];

    const result = await this.db.query(`
      SELECT 
        p.id, p.amount, p.currency, p.status, p.created_at,
        u.id as user_id, u.email, u.first_name, u.last_name
      FROM payments p
      JOIN users u ON p.user_id = u.id
      ${whereClause}
      ORDER BY p.created_at DESC
      LIMIT $1 OFFSET $2
    `, params);

    return result.rows;
  }
}

// Required indexes for optimal performance:
/*
CREATE INDEX CONCURRENTLY idx_payments_user_created 
ON payments(user_id, created_at DESC);

CREATE INDEX CONCURRENTLY idx_payments_created 
ON payments(created_at DESC);

CREATE INDEX CONCURRENTLY idx_users_id 
ON users(id);
*/
```

#### Memory Management Optimization
```typescript
// Memory-efficient data processing
class MemoryEfficientProcessor {
  // ❌ Memory inefficient: Loading all data at once
  async processAllPaymentsBad(): Promise<ProcessingResult[]> {
    const allPayments = await this.paymentRepo.findAll(); // Could be millions
    
    return allPayments.map(payment => {
      return this.processPayment(payment); // All in memory
    });
  }

  // ✅ Memory efficient: Stream processing
  async processAllPaymentsGood(): Promise<ProcessingResult[]> {
    const results: ProcessingResult[] = [];
    const batchSize = 1000;
    let offset = 0;

    while (true) {
      const batch = await this.paymentRepo.findBatch(batchSize, offset);
      
      if (batch.length === 0) break;

      // Process batch and release memory
      const batchResults = await Promise.all(
        batch.map(payment => this.processPayment(payment))
      );

      results.push(...batchResults);
      offset += batchSize;

      // Force garbage collection hint
      if (global.gc && offset % 10000 === 0) {
        global.gc();
      }
    }

    return results;
  }

  // ✅ Even better: Streaming with async generators
  async* processPaymentsStream(): AsyncGenerator<ProcessingResult> {
    const batchSize = 1000;
    let offset = 0;

    while (true) {
      const batch = await this.paymentRepo.findBatch(batchSize, offset);
      
      if (batch.length === 0) break;

      for (const payment of batch) {
        yield await this.processPayment(payment);
      }

      offset += batchSize;
    }
  }
}
```

### Infrastructure Optimizations

#### Caching Strategy Implementation
```typescript
// Multi-level caching implementation
class CachingStrategy {
  constructor(
    private memoryCache: LRU<string, any>,
    private redisCache: Redis,
    private database: Database
  ) {}

  async getUser(userId: string): Promise<User | null> {
    // L1: Memory cache (fastest)
    const memCached = this.memoryCache.get(`user:${userId}`);
    if (memCached) {
      return memCached;
    }

    // L2: Redis cache (fast)
    const redisCached = await this.redisCache.get(`user:${userId}`);
    if (redisCached) {
      const user = JSON.parse(redisCached);
      this.memoryCache.set(`user:${userId}`, user); // Populate L1
      return user;
    }

    // L3: Database (slow)
    const user = await this.database.findUser(userId);
    if (user) {
      // Populate all cache levels
      this.memoryCache.set(`user:${userId}`, user);
      await this.redisCache.setex(`user:${userId}`, 300, JSON.stringify(user));
    }

    return user;
  }

  async invalidateUser(userId: string): Promise<void> {
    // Invalidate all cache levels
    this.memoryCache.del(`user:${userId}`);
    await this.redisCache.del(`user:${userId}`);
  }
}
```

This performance testing guide provides comprehensive strategies for ensuring the PayFlow fintech application meets its performance requirements under various load conditions while maintaining security and compliance standards.