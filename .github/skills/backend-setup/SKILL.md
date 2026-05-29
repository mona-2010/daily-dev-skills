---
name: backend-setup
description: A practical guide to setting up scalable, maintainable, and production-grade backend applications regardless of programming language or framework. Use when: setting up new backend projects; refactoring existing backends; implementing enterprise standards; improving maintainability and developer experience.
user-invocable: true
---
# Backend Setup & Production Best Practices

A practical guide to setting up scalable, maintainable, and production-grade backend applications regardless of programming language or framework.

This guide applies to:

* Node.js
* Java
* Python
* Go
* .NET
* Spring Boot
* Django
* FastAPI
* Express.js
* NestJS
* Laravel
* and most modern backend systems

---

# Why Backend Architecture Matters

Poor backend architecture creates:

* tightly coupled systems
* unmaintainable codebases
* security vulnerabilities
* scaling problems
* inconsistent APIs
* deployment failures

Enterprise systems prioritize:

* scalability
* security
* maintainability
* observability
* clean architecture
* predictable development workflows

---

# Recommended Production Backend Structure

```txt id="jlwm1"
src/
│
├── controllers/
├── services/
├── repositories/
├── routes/
├── middleware/
├── config/
├── utils/
├── validations/
├── models/
├── database/
├── types/
├── constants/
├── logs/
├── tests/
└── app.ts
```

Framework-specific naming may differ, but architectural separation should remain consistent.

---

# Recommended Responsibilities

| Folder       | Responsibility          |
| ------------ | ----------------------- |
| controllers  | request handling        |
| services     | business logic          |
| repositories | database queries        |
| middleware   | authentication, logging |
| validations  | schema validation       |
| config       | environment configs     |
| utils        | helper functions        |
| models       | database entities       |
| tests        | automated testing       |

---

# 1. Environment Variable Management

NEVER hardcode:

* API keys
* database URLs
* secrets
* credentials

Use:

```txt id="jlwm2"
.env
```

Example:

```env id="jlwm3"
PORT=5000

DATABASE_URL=your_database_url

JWT_SECRET=your_secret

REDIS_URL=your_redis_url
```

---

# Why .env Matters

Benefits:

* security
* environment separation
* deployment flexibility
* safer credential management

---

# Important Rules

## NEVER Commit .env Files

Add:

```txt id="jlwm4"
.env
.env.local
.env.production
```

to:

```txt id="jlwm5"
.gitignore
```

---

# Validate Environment Variables

Do not assume variables exist.

Example:

```js id="jlwm6"
if (!process.env.JWT_SECRET) {
  throw new Error("JWT_SECRET missing");
}
```

Recommended:

* Zod
* Joi
* Envalid

---

# 2. Separate Business Logic

One of the most important backend principles.

Bad:

```js id="jlwm7"
app.post("/users", async (req, res) => {
  // validation
  // database query
  // business logic
  // email sending
  // response
});
```

Good:

```txt id="’wini8"
Route
  ↓
Controller
  ↓
Service
  ↓
Repository
  ↓
Database
```

---

# Example Backend Flow

```txt id="’wini9"
Client Request
     ↓
Route
     ↓
Controller
     ↓
Validation
     ↓
Service Layer
     ↓
Repository Layer
     ↓
Database
```

This creates:

* scalability
* testability
* maintainability
* clean separation of concerns

---

# 3. Controllers Should Stay Thin

Controllers should ONLY handle:

* request parsing
* validation triggering
* calling services
* sending responses

Avoid:

* business logic
* database queries
* heavy processing

---

# 4. Services Contain Business Logic

Services are the brain of the application.

Examples:

* payment calculations
* authentication logic
* permissions
* workflows
* order processing

Benefits:

* reusable logic
* easier testing
* cleaner controllers

---

# 5. Repositories Handle Database Access

Avoid database queries everywhere.

Bad:

```js id="’wini10"
const user = await User.findById(id);
```

inside multiple files.

Good:

```txt id="’wini11"
repositories/user.repository.ts
```

Benefits:

* centralized queries
* cleaner services
* easier migrations
* improved maintainability

---

# 6. Use Validation Everywhere

Never trust client input.

Validate:

* request body
* params
* query strings
* uploaded files

Recommended:

| Language | Recommended Validation |
| -------- | ---------------------- |
| Node.js  | Zod / Joi              |
| Python   | Pydantic               |
| Java     | Hibernate Validator    |
| Go       | Validator              |
| .NET     | FluentValidation       |

---

# Example Validation

```ts id="’wini12"
const loginSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
});
```

---

# 7. Centralized Error Handling

Avoid repetitive:

```js id="’wini13"
try {
} catch (err) {
}
```

everywhere.

Use centralized middleware.

Benefits:

* cleaner code
* consistent API responses
* easier debugging

---

# Recommended Error Response

```json id="’wini14"
{
  "success": false,
  "message": "Invalid credentials"
}
```

Keep responses predictable.

---

# 8. Use Proper Logging

Never rely only on:

```js id="’wini15"
console.log()
```

Use structured logging.

Recommended:

| Language | Logging Tools       |
| -------- | ------------------- |
| Node.js  | Winston / Pino      |
| Python   | logging / structlog |
| Java     | Logback / SLF4J     |

---

# Log Important Events

Examples:

* authentication attempts
* payment failures
* server crashes
* database failures
* webhook events

---

# 9. Authentication & Authorization

Authentication verifies identity.

Authorization verifies permissions.

Never mix both.

---

# Recommended Authentication Methods

* JWT
* OAuth
* Sessions
* API Keys

---

# Security Best Practices

* hash passwords
* use HTTPS
* rotate secrets
* rate limit APIs
* validate tokens
* sanitize inputs

Recommended password hashing:

| Language | Recommended           |
| -------- | --------------------- |
| Node.js  | bcrypt                |
| Python   | passlib               |
| Java     | BCryptPasswordEncoder |

---

# 10. Database Best Practices

Use:

* migrations
* indexes
* proper relations
* transactions
* connection pooling

Avoid:

* raw queries everywhere
* duplicated schemas
* missing indexes

---

# Recommended ORM/Database Tools

| Language | Recommended      |
| -------- | ---------------- |
| Node.js  | Prisma / Drizzle |
| Python   | SQLAlchemy       |
| Java     | Hibernate        |
| Go       | GORM             |

---

# 11. API Design Best Practices

Use predictable routes.

Good:

```txt id="’wini16"
/api/users
/api/orders
/api/products
```

Bad:

```txt id="’wini17"
/api/getUsersData
```

Use proper HTTP methods:

| Method    | Purpose |
| --------- | ------- |
| GET       | fetch   |
| POST      | create  |
| PUT/PATCH | update  |
| DELETE    | remove  |

---

# 12. Use Middleware Properly

Middleware handles:

* authentication
* logging
* rate limiting
* validation
* error handling

Keep middleware reusable.

---

# 13. Add Rate Limiting

Protect APIs against abuse.

Recommended:

* express-rate-limit
* nginx rate limiting
* cloudflare protection

---

# 14. Use Async Job Queues

Do NOT process heavy tasks inside requests.

Examples:

* emails
* report generation
* video processing
* AI tasks

Use queues:

| Language | Recommended    |
| -------- | -------------- |
| Node.js  | BullMQ         |
| Python   | Celery         |
| Java     | RabbitMQ/Kafka |

---

# 15. Version Your APIs

Example:

```txt id="’wini18"
/api/v1/users
```

Benefits:

* backward compatibility
* safer frontend updates
* easier migrations

---

# 16. Write Tests

Minimum required:

* unit tests
* integration tests

Recommended:

| Language | Recommended |
| -------- | ----------- |
| Node.js  | Jest        |
| Python   | Pytest      |
| Java     | JUnit       |

---

# 17. Use Docker

Containerization improves:

* deployment consistency
* scalability
* environment parity

Basic structure:

```txt id="’wini19"
Dockerfile
docker-compose.yml
```

---

# 18. Secure Sensitive Data

Never expose:

* secrets
* tokens
* internal errors
* stack traces

in production responses.

---

# 19. Use CI/CD Pipelines

Automate:

* testing
* linting
* deployments

Recommended:

* GitHub Actions
* Jenkins
* GitLab CI

---

# 20. Monitor Backend Health

Production systems require monitoring.

Track:

* API latency
* memory usage
* CPU usage
* database performance
* failed requests

Recommended:

* Prometheus
* Grafana
* Datadog
* Sentry

---

# Common Backend Mistakes

# ❌ Massive Controllers

Controllers should remain thin.

---

# ❌ Business Logic Everywhere

Centralize inside services.

---

# ❌ No Validation

Never trust frontend input.

---

# ❌ No Error Handling

Unhandled errors crash systems.

---

# ❌ No Logging

Makes debugging production impossible.

---

# ❌ Hardcoded Secrets

Major security risk.

---

# ❌ No Rate Limiting

Leaves APIs vulnerable.

---

# ❌ Poor Folder Structure

Creates scaling problems quickly.

---

# Recommended Enterprise Backend Stack

## Backend

* Node.js / Java / Python
* TypeScript
* FastAPI / Spring Boot / Express / NestJS

## Database

* PostgreSQL
* MySQL
* MongoDB
* Redis

## Security

* JWT
* OAuth
* bcrypt

## Infrastructure

* Docker
* CI/CD
* Nginx
* Monitoring

---

# Final Advice For Junior Backend Developers

Good backend engineering is not about writing complex code.

It is about building systems that are:

* scalable
* secure
* maintainable
* predictable
* testable

Focus on:

* separation of concerns
* clean architecture
* security
* proper validation
* maintainability

Build systems teams can scale confidently in production.

---

# Key Takeaways

* Use layered architecture
* Separate controllers, services, and repositories
* Store secrets inside .env files
* Validate every request
* Use centralized error handling
* Add proper logging and monitoring
* Protect APIs using authentication and rate limiting
* Write scalable and maintainable backend systems
