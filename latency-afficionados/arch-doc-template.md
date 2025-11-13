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
21.
  - Propose a decomposition of the monolith.
```

### 2. 🎯 Goals

- Update the React 19 (the last stable version)
- Decomposite the monolith using microservice
- Migrate to java 21 (the last LTS)
- ADD coverage tests (Using mockito, unit tests and integration tests and Jacooco/Cobertura)
- ADD stress test (JMeter to simulate multiple access)
- ADD caos tests (Chaos Toolkit to simulate problems)
- Postgresql 17 (the last LTS)

### 3. 🎯 Non-Goals

- Mobile
- Increasing the latency

### 📐 3. Principles

- SSR - Server-Side Rendering (SSR) is when the HTML of a web page is generated on the server and sent fully rendered to the browser, instead of being built dynamically with JavaScript on the client.

### 🏗️ 4. Overall Diagrams

### 🗂️ 4.1 Overall architecture: Show the big picture, relationship between macro components.

![alt text](arch_fast.drawio.png)

### 🗂️ 4.2 Deployment: Show the infra in a big picture.

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
  - The acceptable cover for the project is 90%.
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

The idea is to instrument the code and publish customer metrics, the metrics will be colleted are:

- Success and failures: Count all successful and fail operations.
- Latency percentiles: Measure the latency of operations and publish p50, p70, p75, p90, p95 and p99 percentiles.
- Top used queries: Most frequent run queries.
- Execution time: Transactions, query or process execution time.

### 🖹 10. Data Store Designs

For each different kind of data store i.e (Postgres, Memcached, Elasticache, S3, Neo4J etc...) describe the schemas, what would be stored there and why, main queries, expectations on performance. Diagrams are welcome but you really need some dictionaries.

### 🖹 11. Technology Stack

- Test
- Junit 5
- Chaos Toolkit
- Chaos Monkey for Spring Bott
- K6

- Observability
- Prometheus
- Grafana

Describe your stack, what databases would be used, what servers, what kind of components, mobile/ui approach, general architecture components, frameworks and libs to be used or not be used and why.

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
