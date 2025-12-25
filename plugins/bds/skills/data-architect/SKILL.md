---
name: data-architect
description: Use this skill when you need expert guidance on data modeling, database schema design, normalization strategies, query optimization, data warehouse architecture, or choosing between database technologies. This includes designing relational databases (PostgreSQL, MySQL, SQL Server), NoSQL data models (MongoDB, Cassandra, DynamoDB), ETL pipelines, and data lake architectures. Trigger when user mentions "design database", "create schema", "normalize tables", "optimize queries", "data warehouse", "choose database", or asks about entity relationships and cardinalities.
---

# Data Architect

Expert guidance for data modeling, database design, and data infrastructure.

## Approach

When approaching data architecture tasks:

1. **Understand domain and access patterns first** - Extract business requirements before designing
2. **Identify entities, relationships, and cardinalities** - Map the data model
3. **Consider both transactional and analytical workloads** - OLTP vs OLAP needs
4. **Balance normalization with performance** - 3NF/BCNF when appropriate, denormalize strategically
5. **Recommend specific technologies** - Based on actual use case requirements

## Key Questions to Ask

Before designing, clarify:

- Expected data volume and growth rate
- Read/write ratio and query patterns
- Consistency vs availability requirements (CAP theorem trade-offs)
- Budget and operational constraints
- Existing technology stack and team expertise

## Design Principles

### Normalization Decisions

| Form | When to Use | Trade-off |
|------|-------------|-----------|
| 3NF | Transactional systems, data integrity critical | More joins, slower reads |
| 2NF | Moderate read performance needed | Some redundancy acceptable |
| Denormalized | Read-heavy, analytics, caching | Write complexity, potential inconsistency |

### Database Selection

| Use Case | Recommended Technology |
|----------|----------------------|
| ACID transactions, complex queries | PostgreSQL, MySQL |
| Document storage, flexible schema | MongoDB |
| Wide-column, time-series | Cassandra, TimescaleDB |
| Key-value, caching | Redis, DynamoDB |
| Analytics, data warehouse | Snowflake, BigQuery, Redshift |
| Graph relationships | Neo4j, Amazon Neptune |

### Scaling Strategies

- **Vertical**: Increase resources (CPU, RAM, storage) - simpler but limited
- **Horizontal**: Sharding, replication - complex but scalable
- **Caching layers**: Redis/Memcached for hot data
- **Read replicas**: Separate read/write workloads

## Schema Design Patterns

### One-to-Many Relationship

```sql
-- Parent table
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL
);

-- Child table with foreign key
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER NOT NULL REFERENCES customers(id),
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total_amount DECIMAL(10,2)
);

-- Index for the foreign key (critical for join performance)
CREATE INDEX idx_orders_customer_id ON orders(customer_id);
```

### Many-to-Many Relationship

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

CREATE TABLE categories (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL
);

-- Junction table
CREATE TABLE product_categories (
    product_id INTEGER REFERENCES products(id) ON DELETE CASCADE,
    category_id INTEGER REFERENCES categories(id) ON DELETE CASCADE,
    PRIMARY KEY (product_id, category_id)
);
```

### Audit/History Pattern

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE user_history (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL,
    email VARCHAR(255),
    name VARCHAR(100),
    changed_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    changed_by INTEGER,
    operation VARCHAR(10) -- INSERT, UPDATE, DELETE
);
```

## Index Strategy

```sql
-- Primary lookup pattern
CREATE INDEX idx_users_email ON users(email);

-- Composite index for common query pattern
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date DESC);

-- Partial index for active records only
CREATE INDEX idx_active_subscriptions ON subscriptions(user_id)
WHERE status = 'active';

-- Covering index to avoid table lookup
CREATE INDEX idx_products_category_price ON products(category_id)
INCLUDE (name, price);
```

## Entity-Relationship Notation (Text Format)

When documenting designs, use this format:

```
CUSTOMER (1) ----< (M) ORDER
    |
    +-- id (PK)
    +-- email (UNIQUE)
    +-- name

ORDER (M) >---- (1) CUSTOMER
    |
    +-- id (PK)
    +-- customer_id (FK -> CUSTOMER.id)
    +-- order_date
    +-- status
```

Notation: `(1)` = one, `(M)` = many, `----<` = one-to-many, `>----<` = many-to-many

## Output Format

When providing schema designs, include:

1. **Entity list** with attributes and constraints
2. **Relationship diagram** in text format
3. **SQL DDL** for implementation
4. **Index recommendations** with rationale
5. **Migration strategy** if refactoring existing systems
