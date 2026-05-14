## 🏛️ Structure

### 1. 🎯 Problem Statement and Context

```
Latency Afficionados is a RETRO video game marketplace.
The platform is capable of:
  - Manage Users
  - Manage the products
  - Provide product search's
  - View products descriptions
  - Rating and review the products
  - Add comments to a product
  - Provide some recommendation based on previews browsing from the user

The desire:
  - Smallest latencies possible.
  - Render fast they can.
  - Migrate from Java 1.4 which needs to be migrated to Java
25.
  - Propose a decomposition of the monolith.
```

### 2. 🎯 Goals

- Update the React 19 (the last stable version)
- Decomposite the monolith using microservice
- Migrate to java 25 (the last LTS)
- ADD coverage tests (Using mockito, unit tests and integration tests and Jacooco/Cobertura)
- ADD stress test (JMeter to simulate multiple access)
- ADD caos tests (Chaos Toolkit to simulate problems)
- Postgresql 18 (the last LTS)

### 3. 🎯 Non-Goals

- Mobile
- Increasing the latency

### 📐 3. Principles

1. Testability: Ensuring the application continues to work correctly (unit test, integration and visual regressions tests).
2. Efficient Rendering: Optimizing how your application displays content using island-architecture.
3. Code Splitting: Single Responsibility Principle - each module has one clear purpose.
4. Observability: Time to Interactive, User session time and User retention.
5. Cache Efficiency: CDN caching: Distribute content geographically closer to users.

### 🏗️ 4. Overall Diagrams

### 🗂️ 4.1 Overall architecture: Show the big picture, relationship between macro components.

![alt text](arch_fast.drawio.png)

### 🗂️ 4.2 Deployment: Show the infra in a big picture.

![alt text](deployment-diagram.png)

### 🗂️ 4.3 Use Cases: Make 1 macro use case diagram that list the main capability that needs to be covered.

### 🧭 5. Trade-offs

<!-- Eu conversei com o Andrei e ele identificou que o diagrama e conceitos que eu estava tentando
criar e desenvolver no diagrama de arquitetura não estavam coerentes com o que ele esperava.

Eu tinha que ficar mais confiante em criar o diagrama e
E entender todos os serviços que eu estava utilizando na arquitetura pois alguns deles eu nao conhecia e
precisave entender melhor o funcionamento deles.
E assim meus tradeoff estavam ficando sem sentido.
Nao posso apresentar algo cheio de duvidas e incertezas -->

### 🌏 6. For each key major component

What is a majore component? A service, a lambda, a important ui, a generalized approach for all uis, a generazid approach for computing a workload, etc...

```
6.1 - Class Diagram              : classic uml diagram with attributes and methods
6.2 - Contract Documentation     : Operations, Inputs and Outputs
6.3 - Persistence Model          : Diagrams, Table structure, partiotioning, main queries.
6.4 - Algorithms/Data Structures : Spesific algos that need to be used, along size with spesific data structures.
```

### 6.2 Contract Documentation

### User

```
GET - /user/me

{
  "id": 0,
  "fullName": "string",
  "email": "string",
  "createdAt": "2025-10-09T16:05:28.155Z",
  "updatedAt": "2025-10-09T16:05:28.155Z",
  "enabled": true,
  "accountNonExpired": true,
  "credentialsNonExpired": true,
  "accountNonLocked": true,
  "authorities": [
    {
      "authority": "string"
    }
  ],
  "username": "string"
}
```

```
GET - /users/

[
  {
    "id": 0,
    "fullName": "string",
    "email": "string",
    "createdAt": "2025-10-09T16:06:58.156Z",
    "updatedAt": "2025-10-09T16:06:58.156Z",
    "enabled": true,
    "accountNonExpired": true,
    "credentialsNonExpired": true,
    "accountNonLocked": true,
    "authorities": [
      {
        "authority": "string"
      }
    ],
    "username": "string"
  }
]
```

### Authentication

```
POST - /auth/signup

-> Input:
{
  "email": "string",
  "password": "string",
  "fullName": "string"
}

-> Response:
{
  "id": 0,
  "fullName": "string",
  "email": "string",
  "createdAt": "2025-10-09T16:08:43.719Z",
  "updatedAt": "2025-10-09T16:08:43.719Z",
  "enabled": true,
  "accountNonExpired": true,
  "credentialsNonExpired": true,
  "accountNonLocked": true,
  "authorities": [
    {
      "authority": "string"
    }
  ],
  "username": "string"
}
```

```
POST - /auth/login

-> Input:
{
  "email": "string",
  "password": "string"
}

-> Response:
{
  "token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJkaWVnb0B1bXBpZXJyZS5jb20uYnIiLCJpYXQiOjE3NjAwMjYyMjMsImV4cCI6MTc2MDAyOTgyM30.7FyTJWMON-Q24MgJuaMwZY9qeUCelNlDoweDKxNuUdI",
  "expiresIn": 3600000
}
```

### 🖹 7. Migrations

IF Migrations are required describe the migrations strategy with proper diagrams, text and tradeoffs.

### 🖹 8. Testing strategy

- Unit and Integration Test
  Predominant because they provide fast feedback and often have a low cost to develop and maintain.
  - All the classes need a unit test
  - All the branch's in the class need be cover
  - The acceptable cover for the project is 100%.
- Contract Tests
  Tells if any changes in a service will break the consumers.
  - All the interfaces need a contract test.
  - All the methods need be cover.
  - Need at leasst one implementation for ecah method from the interface.
- Stress Test
  To check the system can handle some peak volumes. Check the concurrency and resource utilization.
  - Need a scenario to handle peak of 100k RPS
- Chaos Test
  Add some hypothesis about failures in the system and check if the system can recover from it.
  - Need a scenario where disconnect the database
- UI tests
  To check the main user flows are working as expected.
  - All the classes need a unit test
  - All the branch's in the class need be cover
  - The acceptable cover for the project is 90%.

### 🖹 9. Observability strategy

The strategy is AWS-native

#### 9.1 Tooling

| Pillar        | Tool                                     | What it covers in our architecture                             |
| ------------- | ---------------------------------------- | -------------------------------------------------------------- |
| Logs          | CloudWatch Logs                          | Centralized application and access logs                        |
| Metrics       | CloudWatch Metrics + Container Insights  | Infrastructure, ECS task/service/cluster metrics               |
| Traces        | X-Ray (OpenTelemetry SDK in Spring Boot) | End-to-end distributed tracing with correlation ID per request |
| Dashboards    | Grafana                                  | Unified operational and business visualization                 |
| Alerts        | CloudWatch Alarms → SNS                  | Threshold-based alerts routed to on-call teams                 |
| Audit (infra) | CloudTrail → S3                          | Immutable audit trail with long-term retention                 |

#### 9.2 Key Metrics and Alerts

- Success and failures: count of successful and failed operations per endpoint.
- Latency percentiles: p50, p75, p90, p95, p99 per endpoint.
- Top queries: most frequent SQL / DynamoDB queries with execution count and avg duration.
- Execution time: transaction, query and process duration histograms.

### 🖹 10. Data Store Designs

10.1 - Database Schemas

## Table: `users`- Postgres

Store user details including authentication and account status.

| Column                  | Type         | Constraints                                                     |
| ----------------------- | ------------ | --------------------------------------------------------------- |
| id                      | BIGINT       | PRIMARY KEY, AUTO_INCREMENT                                     |
| full_name               | VARCHAR(255) | NOT NULL                                                        |
| username                | VARCHAR(150) | NOT NULL, UNIQUE                                                |
| email                   | VARCHAR(255) | NOT NULL, UNIQUE                                                |
| enabled                 | BOOLEAN      | NOT NULL, DEFAULT TRUE                                          |
| account_non_expired     | BOOLEAN      | NOT NULL, DEFAULT TRUE                                          |
| credentials_non_expired | BOOLEAN      | NOT NULL, DEFAULT TRUE                                          |
| account_non_locked      | BOOLEAN      | NOT NULL, DEFAULT TRUE                                          |
| created_at              | TIMESTAMP    | NOT NULL, DEFAULT CURRENT_TIMESTAMP                             |
| updated_at              | TIMESTAMP    | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

## Table: `authorities`- Postgres

Store roles/authorities assigned to users.

| Column    | Type         | Constraints                 |
| --------- | ------------ | --------------------------- |
| id        | BIGINT       | PRIMARY KEY, AUTO_INCREMENT |
| authority | VARCHAR(100) | NOT NULL, UNIQUE            |

## Table: `user_authorities`- Postgres

Stores the many-to-many relationship between users and their authorities.

| Column       | Type   | Constraints                                                        |
| ------------ | ------ | ------------------------------------------------------------------ |
| user_id      | BIGINT | NOT NULL, FOREIGN KEY REFERENCES users(id) ON DELETE CASCADE       |
| authority_id | BIGINT | NOT NULL, FOREIGN KEY REFERENCES authorities(id) ON DELETE CASCADE |
|              |        | PRIMARY KEY (user_id, authority_id)                                |

## Table: `products` - Postgres

Store product details.

| Column      | Type          | Constraints                                                     |
| ----------- | ------------- | --------------------------------------------------------------- |
| id          | BIGINT        | PRIMARY KEY, AUTO_INCREMENT                                     |
| name        | VARCHAR(255)  | NOT NULL                                                        |
| description | TEXT          | NULL                                                            |
| price       | DECIMAL(10,2) | NOT NULL, CHECK (price >= 0)                                    |
| quantity    | INT           | NOT NULL, DEFAULT 0                                             |
| enabled     | BOOLEAN       | NOT NULL, DEFAULT TRUE                                          |
| created_at  | TIMESTAMP     | NOT NULL, DEFAULT CURRENT_TIMESTAMP                             |
| updated_at  | TIMESTAMP     | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

## Table: `product_recommendations` - Postgres

Store product recommendation details.

| Column                 | Type         | Constraints                                                     |
| ---------------------- | ------------ | --------------------------------------------------------------- |
| id                     | BIGINT       | PRIMARY KEY, AUTO_INCREMENT                                     |
| product_id             | BIGINT       | NOT NULL, FOREIGN KEY REFERENCES products(id) ON DELETE CASCADE |
| recommended_product_id | BIGINT       | NOT NULL, FOREIGN KEY REFERENCES products(id) ON DELETE CASCADE |
| score                  | DECIMAL(5,4) | NOT NULL, CHECK (score >= 0 AND score <= 1)                     |
| observation            | VARCHAR(255) | NULL                                                            |
| created_at             | TIMESTAMP    | NOT NULL, DEFAULT CURRENT_TIMESTAMP                             |

## Table: `marketplaces` - Postgres

Store marketplace details.

| Column        | Type         | Constraints                                                     |
| ------------- | ------------ | --------------------------------------------------------------- |
| id            | BIGINT       | PRIMARY KEY, AUTO_INCREMENT                                     |
| name          | VARCHAR(255) | NOT NULL, UNIQUE                                                |
| description   | TEXT         | NULL                                                            |
| owner_user_id | BIGINT       | NOT NULL, FOREIGN KEY REFERENCES users(id) ON DELETE CASCADE    |
| enabled       | BOOLEAN      | NOT NULL, DEFAULT TRUE                                          |
| created_at    | TIMESTAMP    | NOT NULL, DEFAULT CURRENT_TIMESTAMP                             |
| updated_at    | TIMESTAMP    | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

## Table: `marketplace_products` - Postgres

Store products listed in marketplaces with specific pricing and stock.

| Column         | Type          | Constraints                                                         |
| -------------- | ------------- | ------------------------------------------------------------------- |
| marketplace_id | BIGINT        | NOT NULL, FOREIGN KEY REFERENCES marketplaces(id) ON DELETE CASCADE |
| product_id     | BIGINT        | NOT NULL, FOREIGN KEY REFERENCES products(id) ON DELETE CASCADE     |
| price          | DECIMAL(10,2) | NOT NULL, CHECK (price >= 0)                                        |
| stock          | INT           | NOT NULL, DEFAULT 0                                                 |
| enabled        | BOOLEAN       | NOT NULL, DEFAULT TRUE                                              |
| created_at     | TIMESTAMP     | NOT NULL, DEFAULT CURRENT_TIMESTAMP                                 |
| updated_at     | TIMESTAMP     | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP     |
|                |               | PRIMARY KEY (marketplace_id, product_id)                            |

## Table: `carts`

Store shopping cart details.

| Column         | Type        | Constraints                                                      |
| -------------- | ----------- | ---------------------------------------------------------------- |
| id             | BIGINT      | PRIMARY KEY, AUTO_INCREMENT                                      |
| user_id        | BIGINT      | NOT NULL, FOREIGN KEY REFERENCES users(id) ON DELETE CASCADE     |
| marketplace_id | BIGINT      | NULL, FOREIGN KEY REFERENCES marketplaces(id) ON DELETE SET NULL |
| status         | VARCHAR(30) | NOT NULL, DEFAULT 'ACTIVE'                                       |
| created_at     | TIMESTAMP   | NOT NULL, DEFAULT CURRENT_TIMESTAMP                              |
| updated_at     | TIMESTAMP   | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP  |

## Table: `cart_items`

Store items added to shopping carts.

| Column         | Type          | Constraints                                                     |
| -------------- | ------------- | --------------------------------------------------------------- |
| id             | BIGINT        | PRIMARY KEY, AUTO_INCREMENT                                     |
| cart_id        | BIGINT        | NOT NULL, FOREIGN KEY REFERENCES carts(id) ON DELETE CASCADE    |
| marketplace_id | BIGINT        | NOT NULL                                                        |
| product_id     | BIGINT        | NOT NULL                                                        |
| quantity       | INT           | NOT NULL, DEFAULT 1, CHECK (quantity > 0)                       |
| unit_price     | DECIMAL(10,2) | NOT NULL, CHECK (unit_price >= 0)                               |
| created_at     | TIMESTAMP     | NOT NULL, DEFAULT CURRENT_TIMESTAMP                             |
| updated_at     | TIMESTAMP     | NOT NULL, DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP |

10.2 - Main Queries

- Get User by ID and authorities
- `SELECT
    u.id,
    u.full_name,
    u.username,
    u.email,
    u.enabled,
    a.authority
FROM users u
LEFT JOIN user_authorities ua ON ua.user_id = u.id
LEFT JOIN authorities a ON a.id = ua.authority_id
WHERE u.id = :user_id;
`

- List Products with Pagination
- `SELECT
    p.id,
    p.name,
    p.description,
    p.price,
    p.quantity,
    p.enabled
FROM products p
WHERE p.enabled = TRUE
ORDER BY p.created_at DESC
LIMIT :limit OFFSET :offset;
`

- Get Product Recommendations
- `SELECT
    pr.recommended_product_id,
    p.name,
    p.description,
    p.price,
    pr.score,
    pr.observation
FROM product_recommendations pr
JOIN products p ON p.id = pr.recommended_product_id
WHERE pr.product_id = :product_id
ORDER BY pr.score DESC
LIMIT :limit;
`

- Get Marketplace Products
  - `SELECT
    mp.product_id,
    p.name,
    p.description,
    mp.price,
    mp.stock
FROM marketplace_products mp
JOIN products p ON p.id = mp.product_id
WHERE mp.marketplace_id = :marketplace_id
    AND mp.enabled = TRUE
ORDER BY p.name ASC
    LIMIT :limit OFFSET :offset;
`
- Get Cart by User ID
- `SELECT
    c.id,
    c.user_id,
    c.marketplace_id,
    c.status,
    c.created_at,
    c.updated_at
FROM carts c
WHERE c.user_id = :user_id
    AND c.status = 'ACTIVE';
`

- Get Cart Items by Cart ID
- `SELECT
    ci.id,
    ci.product_id,
    ci.quantity,
    ci.unit_price,
    ci.created_at,
    ci.updated_at
FROM cart_items ci
WHERE ci.cart_id = :cart_id;
`

### 🖹 11. Technology Stack

## Frontend

- React 19
- New features like concurrent rendering and automatic batching
- Testing: Jest + React Testing Library + Playwright
  - Unit, integration, E2E and visual regression testing

## Backend

- Language: Java 25 (LTS)
  - Modern language features
  - Strong performance under high concurrency.
- Framework: Spring Boot 4.0
  - Multiple API built-in features
  - Large community and ecosystem
- Testing:
  - JUnit 6 + Mockito
    - Unit and integration testing
  - Chaos Toolkit
    - Open-source chaos engineering framework
    - Integrates with Spring Boot applications
    - Chaos experiments defined in YAML
  - Chaos Monkey for Spring Boot
    - Open-source library
    - Simulate latency, exceptions, and application restarts
    - Easy integration with Spring Boot applications
  - K6
    - Open-source load testing tool
    - Scriptable in JavaScript
    - Integrates with CI/CD pipelines

## Databases

- PostgresSQL 18
  - One database for each microservice
  - ACID property:
    - Atomicity: Everything happens or nothing happens
    - Consistency: No invalid state
    - Isolation: One transaction do not affect the another one
    - Durability: No data loss after the transaction is committed

## Storage

- Amazon S3
  - Product image storage
  - CloudFront CDN for image delivery
  - Presigned URLs for secure uploads

## Observability

AWS-native, to match the ECS / RDS / DynamoDB / API Gateway stack and avoid running a parallel logging or metrics platform. Detailed in Section 9.

- CloudWatch Logs — application logs (4 ECS services) and access logs (3 ALBs + API Gateway)
- CloudWatch Metrics + Container Insights — infrastructure and custom RED metrics
- AWS X-Ray (OpenTelemetry SDK) — distributed tracing with W3C `traceparent` propagation
- Amazon Managed Grafana — unified dashboards (per-service, SRE, business)
- CloudWatch Alarms → SNS → PagerDuty — SLO burn-rate alerting
- CloudTrail → S3 (Object Lock) — immutable infra audit trail

### 🖹 12. References

- Architecture Anti-Patterns: https://architecture-antipatterns.tech/
- EIP https://www.enterpriseintegrationpatterns.com/
- SOA Patterns https://patterns.arcitura.com/soa-patterns
- API Patterns https://microservice-api-patterns.org/
- Anti-Patterns https://sourcemaking.com/antipatterns/software-development-antipatterns
- Refactoring Patterns https://sourcemaking.com/refactoring/refactorings
- Database Refactoring Patterns https://databaserefactoring.com/
- Data Modelling Redis https://redis.com/blog/nosql-data-modeling/
- Cloud Patterns https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/introduction.html
- 12 Factors App https://12factor.net/
- Relational DB Patterns https://www.geeksforgeeks.org/design-patterns-for-relational-databases/
- Rendering Patterns https://www.patterns.dev/vanilla/rendering-patterns/
- REST API Design https://blog.stoplight.io/api-design-patterns-for-rest-web-services
