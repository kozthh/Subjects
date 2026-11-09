# NoSQL Models and MongoDB

## Introduction

NoSQL databases emerged to address the limitations of relational databases in handling modern web-scale data workloads. Unlike RDBMS, NoSQL systems sacrifice strict ACID compliance for scalability, flexibility, and performance.


**Slide 1: Four Major NoSQL Models**
Overview of key-value (Redis), document (MongoDB), wide-column (Cassandra), and graph (Neo4j), with a use for each. Memory cue: KEY, DOCUMENT, COLUMNS, RELATIONSHIPS.


## Types of NoSQL Models

### Document Model

The document model stores data as semi-structured documents, typically in JSON, BSON, or XML format. Each document can have a different structure, enabling flexible schemas.


**Slide 2: Wide-Column Database Model**
Definition, key characteristics (row key, column families, rows can differ), and how it works (USERS and ORDERS example).


**Key characteristics:**
- Schema-less or dynamic schema
- Nested data structures
- Horizontal scalability

### Key-Value Store

Simple mapping of unique keys to values. High performance for read/write operations. Use case: caching, session storage.


**Slide 3: Key-Value Model: How It Works**
Example records (session:U101, cart:U101, theme:U101), design notes, and Redis as the example product.


### Column-Family Store

Data organized into column families, optimized for reading and writing large datasets across many servers. Use case: analytics, time-series data.


**Slide 4: Wide-Column: Sample Databases**
Customer activity sample data, example databases (Cassandra, HBase, Bigtable, ScyllaDB), advantages and disadvantages.


### Graph Model

Nodes and edges representing entities and relationships. Use case: social networks, recommendation engines.


**Slide 5: Document Model: Nested and Flexible Data**
The customer document (Mika Reyes) showing a nested object (address) and an array (interests), plus BSON.


## MongoDB Overview

MongoDB is a document-oriented NoSQL database. It stores data in flexible JSON-like documents with optional schemas.


**Slide 6: Key-Value: Sample Databases**
Sample key-value data, Redis and DynamoDB, advantages and disadvantages.


### Core Concepts

- **Collection**: Group of documents (analogous to RDBMS tables)
- **Document**: Set of key-value pairs (analogous to RDBMS rows)
- **Field**: Key-value pair within a document
- **Index**: Special data structure that stores a small portion of the collection's data set in an easy-to-traverse form


**Slide 7: Graph Database Model**
Definition (nodes, edges, properties), key characteristics, how it works, and the ANA FRIENDS WITH MARK example.


### Data Model Example

```json
{
  "name": "Alice",
  "age": 30,
  "city": "New York",
  "tags": ["engineering", "python"],
  "metadata": {
    "created": "2023-01-15T08:30:00Z",
    "version": 1
  }
}
```


**Slide 8: Graph Model: Nodes and Edges**
Defines node, edge, and property with examples, and use cases (social networks, recommendations, fraud links).

```

## MongoDB CRUD Operations

### Create

Insert a single document or multiple documents into a collection.


**Slide 9: Graph: Sample Databases**
Sample graph (Ana, Mark, Lisa), Neo4j and Amazon Neptune, advantages and disadvantages.


### Read

Query documents using filter criteria, projection, sorting, and limiting.


**Slide 10: Key-Value and Document Models**
Recap slide with user001 and user002 as key-value, a student document as JSON, and Redis vs MongoDB.


### Update

Modify existing documents using update operators ($set, $inc, $unset, etc.).


**Slide 11: Wide-Column and Graph Models**
Recap slide with Cassandra for event data and large-scale workloads, and Neo4j for friends, recommendations, and fraud links.


### Delete

Remove documents from a collection.


**Slide 12: Scenario Question**
A catalog where some products have size and color and others have warranty and specs. Answer: document model.


## Data Modeling Patterns

### Embedding vs. Referencing

- **Embedding**: Store related data within a single document. Use when the related data is always accessed together or the "one-to-few" relationship.
- **Referencing**: Store related data in separate documents and use references (IDs). Use when the related data is accessed independently or the "one-to-many" relationship is unbounded.


**Slide 13: MongoDB: Document-Oriented NoSQL**
Database, collection, document, fields hierarchy, and BSON = Binary JSON.


### Schema Evolution

MongoDB's flexible schema allows adding fields without downtime. Use `db.collection.updateMany()` with `$set` to populate new fields across existing documents.


**Slide 14: MongoDB Architecture**
Request flow (application, driver, server, database, collection) and the five key components.


## Aggregation Framework

The aggregation pipeline processes documents through a series of stages to return computed results.

### Common Stages

- `$match`: Filter documents (similar to `find`)
- `$group`: Group documents and apply aggregations
- `$sort`: Sort documents by specified fields
- `$project`: Reshape documents (add/remove fields)
- `$lookup`: Join with another collection


**Slide 15: MongoDB Structure & Vocabulary**
Maps relational terms to MongoDB (table to collection, row to document, column to field, primary key to _id).


### Example: Average Age by City

```javascript
db.users.aggregate([
  { $group: { _id: "$city", averageAge: { $avg: "$age" } } },
  { $sort: { averageAge: -1 } }
]);
```

## Indexing Strategies

### Single Field Index

```javascript
db.users.createIndex({ age: 1 });
```

### Compound Index

```javascript
db.orders.createIndex({ customerId: 1, orderDate: -1 });
```

### Text Index

```javascript
db.products.createIndex({ name: "text" });
db.products.find({ name: { $search: "laptop" } });
```

### Geospatial Index

```javascript
db.locations.createIndex({ coordinates: "2d" });
db.locations.find({
  coordinates: {
    $near: { $maxDistance: 1000, $geometry: { type: "Point", coordinates: [0, 0] } }
  }
});
```


**Slide 16: NoSQL Does Not Mean "No Rules"**
A flexible schema still needs careful design, validation, and choosing the model by requirements.


## Administration

### Common Commands

```javascript
db.adminCommand({ listDatabases: 1 });
db.adminCommand({ collMod: "users", validator: { $jsonSchema: { bsonType: "object", required: ["name", "email"] } } });
db.adminCommand({ reIndex: "products" });
```


**Slide 17: Horizontal Scaling: Partitioning**
Splitting a dataset across Servers A, B, and C (sharding in MongoDB), with benefits and considerations.


### Backup and Restore

- `mongodump` / `mongorestore` - Logical backups
- `mongosh` - Interactive JavaScript shell
- Atlas snapshots - Cloud-backed backups


**Slide 18: MongoDB Document: Important Details**
The Ana Cruz document: _id, fields, an array (skills), a Boolean (active), and BSON data types.


## Comparison: MongoDB vs. RDBMS

| Feature | MongoDB | RDBMS |
|---|---|---|
| Data Model | Document (JSON-like) | Relational (tables, rows, columns) |
| Schema | Flexible/Dynamic | Fixed/Strict |
| Scaling | Horizontal (sharding) | Vertical (mostly) |
| Queries | JSON-based queries | SQL |
| Transactions | Multi-document ACID (since 4.0) | Row-level ACID |
| Relationships | Embedding or referencing | Foreign keys, joins |


**Slide 19: MongoDB Architecture: Standalone and Replica Set**
One server vs a Primary with two Secondaries, the failover scenario, and replication vs sharding.


## Extra Note: Common Migration Pitfalls

When migrating from RDBMS to MongoDB:

1. **Over-normalizing**: Avoid creating too many collections with references; consider embedding for frequent joint access.
2. **Assuming joins are free**: DB joins are expensive; denormalize or use application-level joins.
3. **Under-utilizing indexes**: Without proper indexes, queries scan entire collections.
4. **Ignoring schema evolution**: Plan for field additions/removals; use compatible updates.


**Slide 20: MongoDB Architecture: Sharded Cluster**
Client, Mongos, Shard 1 (A-M) and Shard 2 (N-Z), with a Config Server and the student example.


## Correction: Terminology Clarification

- "NoSQL" does not mean "No SQL"; it means "not only SQL" or "non-relational".
- ACID in NoSQL: MongoDB provides multi-document ACID transactions since version 4.0, but not all NoSQL systems guarantee this.
- CAP Theorem: No system can simultaneously provide Consistency, Availability, and Partition tolerance; trade-offs are inherent.


**Slide 21: MongoDB Components: What Each Does**
Vocabulary list: mongod, mongos, config servers, replica set, shard, driver.


## Image Index

| # | Slide | Context | Image |
|---|-------|---------|-------|
| 1 | Four Major NoSQL Models | Overview of key-value (Redis), document (MongoDB), wide-column (Cassandra), and graph (Neo4j), with a use for each. Memory cue: KEY, DOCUMENT, COLUMNS, RELATIONSHIPS. | ![](../assets/lesson-1/slide-1.svg) |
| 2 | Wide-Column Database Model | Definition, key characteristics (row key, column families, rows can differ), and how it works (USERS and ORDERS example). | ![](../assets/lesson-1/slide-2.svg) |
| 3 | Key-Value Model: How It Works | Example records (session:U101, cart:U101, theme:U101), design notes, and Redis as the example product. | ![](../assets/lesson-1/slide-3.svg) |
| 4 | Wide-Column: Sample Databases | Customer activity sample data, example databases (Cassandra, HBase, Bigtable, ScyllaDB), advantages and disadvantages. | ![](../assets/lesson-1/slide-4.svg) |
| 5 | Document Model: Nested and Flexible Data | The customer document (Mika Reyes) showing a nested object (address) and an array (interests), plus BSON. | ![](../assets/lesson-1/slide-5.svg) |
| 6 | Key-Value: Sample Databases | Sample key-value data, Redis and DynamoDB, advantages and disadvantages. | ![](../assets/lesson-1/slide-6.svg) |
| 7 | Graph Database Model | Definition (nodes, edges, properties), key characteristics, how it works, and the ANA FRIENDS WITH MARK example. | ![](../assets/lesson-1/slide-7.svg) |
| 8 | Graph Model: Nodes and Edges | Defines node, edge, and property with examples, and use cases (social networks, recommendations, fraud links). | ![](../assets/lesson-1/slide-8.svg) |
| 9 | Graph: Sample Databases | Sample graph (Ana, Mark, Lisa), Neo4j and Amazon Neptune, advantages and disadvantages. | ![](../assets/lesson-1/slide-9.svg) |
| 10 | Key-Value and Document Models | Recap slide with user001 and user002 as key-value, a student document as JSON, and Redis vs MongoDB. | ![](../assets/lesson-1/slide-10.svg) |
| 11 | Wide-Column and Graph Models | Recap slide with Cassandra for event data and large-scale workloads, and Neo4j for friends, recommendations, and fraud links. | ![](../assets/lesson-1/slide-11.svg) |
| 12 | Scenario Question | A catalog where some products have size and color and others have warranty and specs. Answer: document model. | ![](../assets/lesson-1/slide-12.svg) |
| 13 | MongoDB: Document-Oriented NoSQL | Database, collection, document, fields hierarchy, and BSON = Binary JSON. | ![](../assets/lesson-1/slide-13.svg) |
| 14 | MongoDB Architecture | Request flow (application, driver, server, database, collection) and the five key components. | ![](../assets/lesson-1/slide-14.svg) |
| 15 | MongoDB Structure & Vocabulary | Maps relational terms to MongoDB (table to collection, row to document, column to field, primary key to _id). | ![](../assets/lesson-1/slide-15.svg) |
| 16 | NoSQL Does Not Mean "No Rules" | A flexible schema still needs careful design, validation, and choosing the model by requirements. | ![](../assets/lesson-1/slide-16.svg) |
| 17 | Horizontal Scaling: Partitioning | Splitting a dataset across Servers A, B, and C (sharding in MongoDB), with benefits and considerations. | ![](../assets/lesson-1/slide-17.svg) |
| 18 | MongoDB Document: Important Details | The Ana Cruz document: _id, fields, an array (skills), a Boolean (active), and BSON data types. | ![](../assets/lesson-1/slide-18.svg) |
| 19 | MongoDB Architecture: Standalone and Replica Set | One server vs a Primary with two Secondaries, the failover scenario, and replication vs sharding. | ![](../assets/lesson-1/slide-19.svg) |
| 20 | MongoDB Architecture: Sharded Cluster | Client, Mongos, Shard 1 (A-M) and Shard 2 (N-Z), with a Config Server and the student example. | ![](../assets/lesson-1/slide-20.svg) |
| 21 | MongoDB Components: What Each Does | Vocabulary list: mongod, mongos, config servers, replica set, shard, driver. | ![](../assets/lesson-1/slide-21.svg) |
| 22 | Lesson Recap | Topic 1 (relational vs NoSQL) and Topic 2 (the four models and the MongoDB hierarchy). | ![](../assets/lesson-1/slide-22.svg) |
| 23 | MongoDB CRUD Operations | Create, Read, Update, Delete with insertOne(), find(), updateOne(), and deleteOne(). | ![](../assets/lesson-1/slide-23.svg) |

## Appendix: Slide Image Files

This appendix lists all slide image files available in the `assets/lesson-1/` directory for reference.

Available slide images:
- slide-1.svg through slide-25.svg

## Quick Review

<details>
<summary>Click to reveal answers</summary>

**Q1: What are the four main types of NoSQL models?**
- Document, Key-Value, Column-Family, Graph

**Q2: How does MongoDB differ from RDBMS in terms of schema?**
- MongoDB has a flexible/dynamic schema; RDBMS has a fixed schema.

**Q3: When would you choose embedding over referencing in MongoDB?**
- Choose embedding when related data is always accessed together or is "one-to-few".

**Q4: What does the $lookup aggregation stage do?**
- It performs a left outer join to another collection.

**Q5: Since which MongoDB version are multi-document ACID transactions available?**
- Version 4.0

</details>