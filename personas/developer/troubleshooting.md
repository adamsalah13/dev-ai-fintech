# Developer Troubleshooting Guide

## 🔧 Common Issues and Solutions

This guide provides solutions to common problems encountered when developing with AI tools in the PayFlow fintech application.

## 🤖 AI Tool Issues

### GitHub Copilot Problems

#### Issue: Copilot Not Providing Suggestions

**Symptoms:**
- No code suggestions appearing
- Grayed-out Copilot icon
- Error messages in VS Code

**Solutions:**

1. **Check Authentication**
   ```bash
   # Verify GitHub Copilot subscription
   gh auth status
   
   # Re-authenticate if needed
   gh auth login
   ```

2. **Restart VS Code Extensions**
   - Press `Ctrl+Shift+P` (Windows) or `Cmd+Shift+P` (Mac)
   - Type "Developer: Reload Window"
   - Or disable/enable the GitHub Copilot extension

3. **Check File Type Support**
   ```javascript
   // Copilot works better with proper file extensions
   // Ensure files have correct extensions: .js, .ts, .tsx, .py, etc.
   ```

4. **Network Issues**
   ```bash
   # Check connectivity to GitHub
   ping github.com
   
   # Test Copilot API access
   curl -H "Authorization: Bearer YOUR_TOKEN" https://api.github.com/user
   ```

#### Issue: Poor Quality Suggestions

**Symptoms:**
- Generic or irrelevant code suggestions
- Code that doesn't fit the fintech context
- Missing error handling or security considerations

**Solutions:**

1. **Improve Context**
   ```typescript
   // ✅ Good: Provide clear context
   /**
    * Process a payment in a fintech application
    * Must include fraud detection and PCI compliance
    * Requirements: Validate amount, check limits, log for audit
    */
   async function processPayment(paymentData: PaymentRequest) {
       // Copilot will generate better, context-aware code
   }
   ```

2. **Use Specific Comments**
   ```typescript
   // ✅ Good: Specific requirements
   // Validate payment amount is positive and under $10,000 limit
   // Check user KYC status before processing
   // Integrate with Stripe API for card processing
   // Log transaction for compliance audit trail
   ```

3. **Reference Related Code**
   ```typescript
   // Include imports and types for better context
   import { PaymentRequest, ValidationError } from './types';
   import { StripeService } from './stripe-service';
   import { AuditLogger } from './audit-logger';
   ```

### Cursor AI Issues

#### Issue: Cursor Not Understanding Codebase

**Symptoms:**
- AI suggestions don't match existing patterns
- Inconsistent code style
- Missing context from related files

**Solutions:**

1. **Use @codebase Reference**
   ```typescript
   // In Cursor chat or command
   // @codebase How should I implement payment validation following existing patterns?
   ```

2. **Include Context Files**
   ```typescript
   // Reference related files in your prompts
   // "Looking at payment-service.ts and user-service.ts, implement transaction-service.ts"
   ```

3. **Set Project Context**
   ```json
   // In .cursor/context.json
   {
     "include": [
       "src/**/*.ts",
       "src/**/*.tsx",
       "types/**/*.ts"
     ],
     "exclude": [
       "node_modules/**",
       "dist/**",
       "coverage/**"
     ]
   }
   ```

## 🏗️ Development Environment Issues

### Node.js and npm Problems

#### Issue: Module Resolution Errors

**Symptoms:**
```bash
Error: Cannot find module 'express'
Module not found: Can't resolve './types'
```

**Solutions:**

1. **Check Package Installation**
   ```bash
   # Verify package.json dependencies
   npm list
   
   # Reinstall dependencies
   rm -rf node_modules package-lock.json
   npm install
   ```

2. **Fix Import Paths**
   ```typescript
   // ✅ Good: Correct relative imports
   import { PaymentService } from './services/payment-service';
   import { User } from '../types/user';
   
   // ❌ Avoid: Incorrect paths
   import { PaymentService } from 'services/payment-service'; // Missing ./
   ```

3. **TypeScript Configuration**
   ```json
   // tsconfig.json
   {
     "compilerOptions": {
       "baseUrl": "src",
       "paths": {
         "@/*": ["*"],
         "@types/*": ["types/*"],
         "@services/*": ["services/*"]
       }
     }
   }
   ```

#### Issue: Version Conflicts

**Symptoms:**
```bash
ERESOLVE unable to resolve dependency tree
peer dep missing: react@>=16.8.0
```

**Solutions:**

1. **Check Peer Dependencies**
   ```bash
   # Check for peer dependency warnings
   npm install --legacy-peer-deps
   
   # Or use exact versions
   npm install react@18.2.0 react-dom@18.2.0
   ```

2. **Update Dependencies**
   ```bash
   # Check outdated packages
   npm outdated
   
   # Update specific package
   npm update package-name
   
   # Update all packages (careful with breaking changes)
   npm update
   ```

### Database Connection Issues

#### Issue: PostgreSQL Connection Failures

**Symptoms:**
```bash
Error: connect ECONNREFUSED 127.0.0.1:5432
FATAL: password authentication failed for user "payflow"
```

**Solutions:**

1. **Check Database Status**
   ```bash
   # Check if PostgreSQL is running
   docker-compose ps postgres
   
   # View database logs
   docker-compose logs postgres
   
   # Restart database service
   docker-compose restart postgres
   ```

2. **Verify Environment Variables**
   ```bash
   # Check .env file
   cat .env | grep DB_
   
   # Should contain:
   # DATABASE_URL=postgresql://payflow:payflow_password@localhost:5432/payflow_dev
   # DB_HOST=localhost
   # DB_PORT=5432
   # DB_NAME=payflow_dev
   # DB_USER=payflow
   # DB_PASSWORD=payflow_password
   ```

3. **Test Connection Manually**
   ```bash
   # Test database connection
   psql -h localhost -p 5432 -U payflow -d payflow_dev
   
   # Or using Docker
   docker-compose exec postgres psql -U payflow -d payflow_dev
   ```

4. **Reset Database**
   ```bash
   # If all else fails, reset the database
   docker-compose down -v
   docker-compose up -d postgres
   
   # Wait for startup, then run migrations
   npm run migrate
   npm run seed
   ```

### Docker Issues

#### Issue: Container Build Failures

**Symptoms:**
```bash
ERROR [internal] load metadata for docker.io/library/node:18-alpine
failed to solve with frontend dockerfile.v0
```

**Solutions:**

1. **Check Dockerfile Syntax**
   ```dockerfile
   # ✅ Good: Proper Dockerfile structure
   FROM node:18-alpine
   
   WORKDIR /app
   
   # Copy package files first for better caching
   COPY package*.json ./
   RUN npm ci --only=production
   
   # Copy application code
   COPY . .
   
   EXPOSE 3000
   CMD ["npm", "start"]
   ```

2. **Clear Docker Cache**
   ```bash
   # Clear Docker build cache
   docker system prune -a
   
   # Rebuild without cache
   docker-compose build --no-cache
   ```

3. **Check Resource Limits**
   ```bash
   # Check Docker resources
   docker system df
   
   # Increase memory limit in Docker Desktop settings
   # Recommended: 4GB+ for development
   ```

## 🧪 Testing Issues

### Jest Test Failures

#### Issue: Tests Failing Due to Async Operations

**Symptoms:**
```bash
TypeError: Cannot read property 'id' of undefined
Test timeout error (5000ms exceeded)
```

**Solutions:**

1. **Proper Async/Await Usage**
   ```typescript
   // ✅ Good: Proper async test structure
   describe('PaymentService', () => {
     it('should process payment successfully', async () => {
       // Arrange
       const paymentData = createTestPaymentData();
       
       // Act
       const result = await paymentService.processPayment(paymentData);
       
       // Assert
       expect(result.isSuccess).toBe(true);
     });
   });
   ```

2. **Mock External Dependencies**
   ```typescript
   // ✅ Good: Proper mocking
   jest.mock('./stripe-service');
   jest.mock('./database');
   
   const mockStripe = jest.mocked(StripeService);
   const mockDb = jest.mocked(Database);
   
   beforeEach(() => {
     jest.clearAllMocks();
     mockStripe.createPaymentIntent.mockResolvedValue(mockPaymentIntent);
   });
   ```

3. **Increase Timeout for Integration Tests**
   ```typescript
   // For slow operations
   it('should handle large payment processing', async () => {
     // Test implementation
   }, 10000); // 10 second timeout
   ```

#### Issue: Database Tests Interfering

**Symptoms:**
```bash
Unique constraint violation
Tests pass individually but fail when run together
```

**Solutions:**

1. **Proper Test Isolation**
   ```typescript
   // ✅ Good: Isolated test database
   describe('UserRepository', () => {
     let testDb: TestDatabase;
     
     beforeEach(async () => {
       testDb = await createTestDatabase();
     });
     
     afterEach(async () => {
       await testDb.cleanup();
     });
   });
   ```

2. **Use Transactions**
   ```typescript
   // ✅ Good: Transaction-based testing
   describe('TransactionService', () => {
     let transaction: DatabaseTransaction;
     
     beforeEach(async () => {
       transaction = await database.beginTransaction();
     });
     
     afterEach(async () => {
       await transaction.rollback();
     });
   });
   ```

## 🔒 Security and Compliance Issues

### Authentication Problems

#### Issue: JWT Token Validation Failures

**Symptoms:**
```bash
JsonWebTokenError: invalid signature
TokenExpiredError: jwt expired
```

**Solutions:**

1. **Check JWT Configuration**
   ```typescript
   // ✅ Good: Proper JWT configuration
   const jwtConfig = {
     secret: process.env.JWT_SECRET, // Must be consistent
     algorithm: 'HS256' as const,
     expiresIn: '15m',
     issuer: 'payflow-api',
     audience: 'payflow-client'
   };
   ```

2. **Verify Environment Variables**
   ```bash
   # Check JWT secret is set and consistent
   echo $JWT_SECRET
   
   # Generate new secret if needed
   node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
   ```

3. **Handle Token Expiration**
   ```typescript
   // ✅ Good: Proper token refresh handling
   export class AuthService {
     async refreshToken(refreshToken: string): Promise<TokenPair | null> {
       try {
         const decoded = jwt.verify(refreshToken, process.env.REFRESH_SECRET!);
         const user = await this.userService.findById(decoded.sub);
         
         if (!user || !user.isActive) {
           return null;
         }
         
         return this.generateTokens(user);
       } catch (error) {
         logger.warn('Token refresh failed', { error: error.message });
         return null;
       }
     }
   }
   ```

### CORS Issues

#### Issue: Cross-Origin Requests Blocked

**Symptoms:**
```bash
Access to fetch at 'http://localhost:3000/api/v1/payments' blocked by CORS policy
```

**Solutions:**

1. **Configure CORS Properly**
   ```typescript
   // ✅ Good: Proper CORS configuration
   app.use(cors({
     origin: process.env.CORS_ORIGINS?.split(',') || ['http://localhost:3100'],
     credentials: true,
     methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH', 'OPTIONS'],
     allowedHeaders: ['Content-Type', 'Authorization', 'X-Requested-With']
   }));
   ```

2. **Handle Preflight Requests**
   ```typescript
   // ✅ Good: Handle OPTIONS requests
   app.options('*', cors()); // Enable preflight for all routes
   ```

## 🚀 Performance Issues

### API Response Time Problems

#### Issue: Slow API Responses

**Symptoms:**
```bash
API responses taking >2 seconds
Database query timeouts
High memory usage
```

**Solutions:**

1. **Database Query Optimization**
   ```sql
   -- ✅ Good: Optimized query with proper indexes
   EXPLAIN ANALYZE 
   SELECT u.id, u.email, p.amount, p.status 
   FROM users u 
   JOIN payments p ON u.id = p.customer_id 
   WHERE u.status = 'active' 
   AND p.created_at >= NOW() - INTERVAL '30 days'
   ORDER BY p.created_at DESC 
   LIMIT 50;
   
   -- Add indexes if missing
   CREATE INDEX CONCURRENTLY idx_payments_customer_created 
   ON payments(customer_id, created_at DESC);
   ```

2. **Implement Caching**
   ```typescript
   // ✅ Good: Redis caching implementation
   export class UserService {
     async getUser(userId: string): Promise<User | null> {
       const cacheKey = `user:${userId}`;
       
       // Try cache first
       const cached = await this.redis.get(cacheKey);
       if (cached) {
         return JSON.parse(cached);
       }
       
       // Fetch from database
       const user = await this.userRepository.findById(userId);
       
       if (user) {
         // Cache for 5 minutes
         await this.redis.setex(cacheKey, 300, JSON.stringify(user));
       }
       
       return user;
     }
   }
   ```

3. **Connection Pool Optimization**
   ```typescript
   // ✅ Good: Optimized connection pool
   const pool = new Pool({
     host: process.env.DB_HOST,
     port: parseInt(process.env.DB_PORT || '5432'),
     database: process.env.DB_NAME,
     user: process.env.DB_USER,
     password: process.env.DB_PASSWORD,
     min: 2,      // Minimum connections
     max: 20,     // Maximum connections  
     idleTimeoutMillis: 30000,
     connectionTimeoutMillis: 2000
   });
   ```

### Memory Leaks

#### Issue: Memory Usage Growing Over Time

**Symptoms:**
```bash
Node.js process memory usage increasing
Application becomes unresponsive
Container restarts due to memory limits
```

**Solutions:**

1. **Profile Memory Usage**
   ```bash
   # Use Node.js built-in profiler
   node --inspect --inspect-brk app.js
   
   # Or use clinic.js
   npm install -g clinic
   clinic doctor -- node app.js
   ```

2. **Fix Common Memory Leaks**
   ```typescript
   // ✅ Good: Proper cleanup
   export class PaymentProcessor {
     private timeouts: Set<NodeJS.Timeout> = new Set();
     
     schedulePayment(paymentId: string, delay: number) {
       const timeout = setTimeout(() => {
         this.processPayment(paymentId);
         this.timeouts.delete(timeout); // Clean up reference
       }, delay);
       
       this.timeouts.add(timeout);
     }
     
     destroy() {
       // Cleanup on shutdown
       this.timeouts.forEach(timeout => clearTimeout(timeout));
       this.timeouts.clear();
     }
   }
   ```

3. **Limit Object Growth**
   ```typescript
   // ✅ Good: Use LRU cache with size limits
   import LRU from 'lru-cache';
   
   const cache = new LRU<string, User>({
     max: 1000,        // Maximum 1000 items
     ttl: 1000 * 60 * 5 // 5 minutes TTL
   });
   ```

## 🔍 Debugging Strategies

### Using Chrome DevTools

1. **Start Node.js with Inspector**
   ```bash
   node --inspect-brk=0.0.0.0:9229 app.js
   ```

2. **Connect Chrome DevTools**
   - Open Chrome and go to `chrome://inspect`
   - Click "Configure" and add `localhost:9229`
   - Click "inspect" on your Node.js process

### VS Code Debugging

1. **Create Debug Configuration**
   ```json
   // .vscode/launch.json
   {
     "version": "0.2.0",
     "configurations": [
       {
         "name": "Debug PayFlow API",
         "type": "node",
         "request": "launch",
         "program": "${workspaceFolder}/backend/api-gateway/src/app.js",
         "env": {
           "NODE_ENV": "development"
         },
         "console": "integratedTerminal",
         "sourceMaps": true
       }
     ]
   }
   ```

### Logging Best Practices

1. **Structured Logging**
   ```typescript
   // ✅ Good: Structured logging
   logger.info('Payment processing started', {
     paymentId: payment.id,
     userId: payment.userId,
     amount: payment.amount,
     timestamp: new Date().toISOString()
   });
   ```

2. **Error Context**
   ```typescript
   // ✅ Good: Rich error context
   try {
     await this.processPayment(paymentData);
   } catch (error) {
     logger.error('Payment processing failed', {
       error: {
         name: error.name,
         message: error.message,
         stack: error.stack
       },
       paymentData: {
         id: paymentData.id,
         amount: paymentData.amount,
         userId: paymentData.userId
       },
       context: {
         timestamp: new Date().toISOString(),
         requestId: req.id,
         userAgent: req.headers['user-agent']
       }
     });
     throw error;
   }
   ```

## 📞 Getting Help

### Internal Resources

1. **Team Communication**
   - Use GitHub Issues for bug reports
   - Create detailed reproduction steps
   - Include environment information
   - Tag relevant team members

2. **Documentation**
   - Check existing documentation first
   - Update docs when you solve issues
   - Create runbooks for complex procedures

### External Resources

1. **Stack Overflow**
   - Search existing questions first
   - Create minimal reproducible examples
   - Tag questions appropriately (node.js, typescript, fintech)

2. **GitHub Issues**
   - Check official repositories for known issues
   - Follow issue templates when reporting bugs
   - Provide version information and environment details

### AI-Assisted Debugging

1. **GitHub Copilot Chat**
   ```bash
   # In VS Code, use Copilot Chat for debugging help
   /explain # Understand complex error messages
   /fix     # Get suggestions for fixing issues
   /tests   # Generate tests to reproduce issues
   ```

2. **Cursor AI**
   ```typescript
   // Use Cursor's debugging features
   // Select error-prone code and ask:
   // "Debug this function - it's causing memory leaks"
   // "Why is this async function not working properly?"
   // "Optimize this database query for better performance"
   ```

## ✅ Prevention Checklist

### Before Committing Code

- [ ] All tests pass locally
- [ ] Code follows team style guidelines
- [ ] No console.log statements in production code
- [ ] Environment variables are properly configured
- [ ] Database migrations run successfully
- [ ] Security vulnerabilities checked
- [ ] Performance impact assessed
- [ ] Documentation updated

### Before Deploying

- [ ] All CI/CD checks pass
- [ ] Database migrations tested
- [ ] Environment configurations verified
- [ ] Monitoring and alerting configured
- [ ] Rollback plan documented
- [ ] Stakeholders notified
- [ ] Load testing completed (for major changes)

This troubleshooting guide should be regularly updated as new issues are discovered and resolved. Remember to document solutions and share knowledge with the team.