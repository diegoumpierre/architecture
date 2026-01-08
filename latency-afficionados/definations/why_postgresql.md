# Why SQL database?
- workload: transactional history
- data access patterns: More inserts for Product and Marketplace, so we need strong consistance and transaction support.
- Modelling: Product and Marketplace fit in a Relational schema

# Module
- ACID property:
-- Atomicity: Everything happens or nothing happens
-- Consistency: No invalid state
-- Isolation: One transaction do not affect the another one
-- Durability: No data loss after the transaction is committed

## Why Postgres
- Always ACID compliant.
- Support to materialized views
- Is OpenSource
- Have frequently updates

## Why NOT Postgres
- Create a new system process for each user connected to the database, alocating more memory (+- 10mb)