# Database Systems, Lesson 1: NoSQL Models and MongoDB

*Lecture date: November 9, 2026. Source: the professor's lecture slides and discussion. Anything marked **Extra** is my own explanation (not on the slides), and anything marked **Correction** fixes a mix-up from the lecture dictation.*

---

## 1. Relational vs NoSQL

- **Relational:** tables, predefined structure, keys, SQL.
- **NoSQL:** alternative data models, flexible structures, and commonly distributed scaling.
- NoSQL is a **broad category**, not one single database or query language.
- **Choose based on requirements, not popularity.**

![Lesson recap](../assets/lesson-1/22-lesson-recap.png)

### NoSQL does not mean "no rules"

A flexible schema still needs careful data design.

- A flexible schema lets documents or records **vary**. It does not mean data should be **unplanned**.
- Applications can **validate** required fields, data types, and business rules.
- Different NoSQL products offer different **consistency, transaction, and query** capabilities.
- Choose the model by **access patterns, relationships, availability, and operational needs**.
- Pick the model that fits the problem, not the one that is easiest or most familiar. If a scenario fits SQL better, use SQL.

![NoSQL does not mean no rules](../assets/lesson-1/16-nosql-does-not-mean-no-rules.png)

---

## 2. The Four Major NoSQL Models

NoSQL databases are classified by **the way they organize data**.

**Memory cue:** KEY • DOCUMENT • COLUMNS • RELATIONSHIPS

| Model       | Organizes data as            | Example   | Typical use                          |
|-------------|------------------------------|-----------|--------------------------------------|
| Key-Value   | Key → value                  | Redis     | Cache, sessions                      |
| Document    | Document objects             | MongoDB   | Profiles, catalogs                   |
| Wide-Column | Rows with column families    | Cassandra | Large distributed data, event data   |
| Graph       | Nodes + edges (+ properties) | Neo4j     | Relationships, recommendations, fraud links |

![Four major NoSQL models](../assets/lesson-1/01-four-nosql-models.png)

### 2.1 Key-Value

A simple model optimized for **lookup by key**. The key identifies the stored value.

```
session:U101  →  "logged-in"
cart:U101     →  ["P1", "P9"]
theme:U101    →  "dark"
```

- Very fast key-based access.
- The value may be **text, a structure, or bytes**.
- Often used for **cache and session state**.
- Not ideal for arbitrary relational joins.

![Key-value model: how it works](../assets/lesson-1/03-key-value-how-it-works.png)

**Sample databases:** Redis (in-memory key-value), Amazon DynamoDB (managed NoSQL).

| Advantages                          | Disadvantages                       |
|-------------------------------------|-------------------------------------|
| Very fast key-based access          | Limited complex searches by value   |
| Simple structure, easy to use       | Relationships not handled naturally |
| Useful for caching and sessions     | Not ideal for complex reporting     |

![Key-value: sample databases](../assets/lesson-1/06-key-value-sample-databases.png)

### 2.2 Document

A document can **group data that is commonly read together**.

```json
{
  "_id": "C102",
  "name": "Mika Reyes",
  "address": { "city": "Bacoor", "province": "Cavite" },
  "interests": ["music", "coding"]
}
```

- **Nested object:** `address`. **Array:** `interests`.
- **Fields can vary by document.**
- Related information may be **embedded** when it is commonly used together.
- MongoDB stores documents using **BSON**, a binary-encoded document format.

![Document model: nested and flexible data](../assets/lesson-1/05-document-model-nested-flexible.png)

### 2.3 Wide-Column

Organizes large amounts of data into **rows and column families**. Different rows can have different columns.

- Uses a **row key** to identify each record.
- Groups related columns into **column families**.
- Rows do not need identical columns.
- Designed for **large, distributed datasets**.

**How it works:** (1) a row key identifies the record, (2) related details are grouped in column families, (3) the application retrieves only the columns it needs. Example: USERS stores profile details, ORDERS stores order details.

![Wide-column database model](../assets/lesson-1/02-wide-column-database-model.png)

**Sample data (customer activity):**

| Row key      | Column   | Value  |
|--------------|----------|--------|
| customer:101 | activity | Login  |
| customer:101 | date     | Nov. 1 |
| customer:101 | device   | Mobile |

**Sample databases:** Cassandra (distributed NoSQL), Apache HBase (Hadoop ecosystem), Google Bigtable (managed wide-column), ScyllaDB (high performance).

| Advantages                      | Disadvantages                                        |
|---------------------------------|------------------------------------------------------|
| Scales across multiple servers  | Data modeling can be complex                         |
| Handles very large data volumes | Not ideal for many ad hoc queries                    |
| Supports high write throughput  | Joins are limited compared with relational databases |

![Wide-column: sample databases](../assets/lesson-1/04-wide-column-sample-databases.png)

### 2.4 Graph

Focuses on **connections and relationships** between data. Data is represented as **nodes** (entities) and **edges** (relationships). **Properties** describe nodes and edges.

| Part     | Meaning                    | Examples                         |
|----------|----------------------------|----------------------------------|
| Node     | An entity                  | A person, product, or location   |
| Edge     | A connection between nodes | FOLLOWS, FRIENDS_WITH, PURCHASED |
| Property | Describes a node or edge   | User name, friendship date       |

**How it works:** (1) create nodes for entities, (2) connect them with named relationships, (3) follow connections to find related information.

```
ANA ──FRIENDS WITH──> MARK
```

- You need **both**: nodes give you something to represent, edges give you the relationships between them.
- Useful for **social networks, recommendation paths, and fraud-connection analysis**.

![Graph database model](../assets/lesson-1/07-graph-database-model.png)

![Graph model: nodes and edges](../assets/lesson-1/08-graph-model-nodes-and-edges.png)

**Sample graph:** Ana, Mark, Lisa are the **nodes**. FRIENDS WITH (Ana–Mark) and KNOWS (Ana–Lisa, Mark–Lisa) are the **edges**.

**Sample databases:** Neo4j (graph database), Amazon Neptune (managed graph DB).

| Advantages                          | Disadvantages                                |
|-------------------------------------|----------------------------------------------|
| Makes relationships easy to represent | Requires graph-specific skills             |
| Good for exploring connected data   | May be unnecessary for simple data           |
| Can reveal patterns in networks     | Very large graphs can be challenging to scale |

![Graph: sample databases](../assets/lesson-1/09-graph-sample-databases.png)

### 2.5 Recap slides

![Key-value and document models](../assets/lesson-1/10-key-value-and-document-models.png)

![Wide-column and graph models](../assets/lesson-1/11-wide-column-and-graph-models.png)

---

## 3. Comparing and Choosing a Model

| | Key-Value | Document | Wide-Column | Graph |
|---|---|---|---|---|
| **Data unit** | Key + value | Document | Row key + column families | Node, edge, property |
| **How you find data** | Look up by key | Fetch the whole document | Fetch selected columns by row key | Follow connections |
| **Relationships** | Not handled | Embedded in the document | Limited joins | The central feature |
| **Best for** | Cache, sessions | Profiles, catalogs | Huge distributed data, event data | Connected data |
| **Weakness** | Weak at complex searches | Less suited to heavy joins | Complex modeling, limited joins | Needs graph skills, hard to scale when huge |

### Scenario practice

*A product catalog stores items with different details. Some products have size and color, while others have warranty and technical specifications. Which model fits best?*

**Document.** Each product is its own document, fields can vary by document, and specifications can be nested. Catalogs are a typical document use case.

![Scenario question](../assets/lesson-1/12-scenario-product-catalog.png)

**How to pick:** match the **main problem** in the scenario.

- Varying details, flexible structure: **Document**
- Massive, distributed, write-heavy data (many servers or branches, event logs): **Wide-column**
- Fast cache or session lookup: **Key-value**
- Connections between things: **Graph**

"Products in a store" alone does not decide it. A catalog with varying details is document. A nationwide chain recording millions of sales updates per minute is wide-column.

### Extra: easy ways to remember the differences

*These explanations are mine, not from the slides.*

- **Key-value vs document:** key-value treats the value as a **black box** (a locker: the number opens it, nobody looks inside). A document has **fields the database understands**, so you can look up by ID *and* search inside by field.
- **Document vs wide-column:** a document is **one sheet of paper** (everything about a customer in one nested object, usually read whole). Wide-column is **one binder with tabbed dividers**: the binder is one record (row key), the dividers are column families, and you flip straight to the tab you need.
- **Key-value vs SQL:** key-value is like a SQL table with **only two columns** (key and value). It can look up by key only, with no joins and no searching by value.

---

## 4. MongoDB

### 4.1 Document-oriented NoSQL

MongoDB stores records as **BSON documents inside collections**. Documents are flexible records of **field-value pairs** that may include nested objects or arrays.

```
DATABASE ── COLLECTION ── DOCUMENT
```

A database contains collections, a collection contains documents, a document contains fields. **BSON = Binary JSON.**

![MongoDB: document-oriented NoSQL](../assets/lesson-1/13-mongodb-document-oriented-nosql.png)

### 4.2 Structure and vocabulary

| Relational (SQL)    | MongoDB            |
|---------------------|--------------------|
| Relational database | MongoDB (database) |
| Table               | Collection         |
| Row / record        | Document           |
| Column              | Field              |
| Primary key         | `_id` field        |

The mapping is useful for learning, but **collections and documents are not identical to tables and rows**. The concepts are similar, while the structure and how you build it differ.

![MongoDB structure and vocabulary](../assets/lesson-1/15-mongodb-structure-and-vocabulary.png)

### 4.3 Document details

```json
{
  "_id": 101,
  "name": "Ana Cruz",
  "course": "BSIT",
  "skills": ["SQL", "Python"],
  "active": true
}
```

- `_id`: the document identifier. **MongoDB creates one if omitted.**
- `name`, `course`: fields with values.
- `skills`: an **array** value. `active`: a **Boolean** value.
- This example has **no nested object**.
- BSON supports data types beyond plain JSON, including **dates and binary data**.

**Lab pattern:** given a document, identify the fields, values, arrays, nested objects, and the `_id`.

![MongoDB document: important details](../assets/lesson-1/18-mongodb-document-important-details.png)

### 4.4 Architecture and request flow

```
APPLICATION → DRIVER → MONGODB SERVER → DATABASE → COLLECTION / DOCS
```

1. The **application** sends a request through a MongoDB **driver**.
2. The **server** processes the operation and reads or updates the requested data (CRUD).
3. The **result is returned** to the application.

![MongoDB architecture](../assets/lesson-1/14-mongodb-architecture.png)

### 4.5 Components

| Term           | Meaning                                                              |
|----------------|----------------------------------------------------------------------|
| Application    | The client (web or mobile app, or any system) that uses the data     |
| Driver         | The **application library** used to connect and send operations      |
| `mongod`       | The **database server process** that stores data and handles operations |
| `mongos`       | The **query router** used in a sharded cluster                       |
| Config servers | Store **metadata** about the sharded cluster                         |
| Replica set    | A group of `mongod` members that maintain **replicated data**        |
| Shard          | A **partition** of the dataset in a sharded deployment               |

`mongod` and `mongos` are real names, not typos.

![MongoDB components](../assets/lesson-1/21-mongodb-components.png)

### 4.6 Standalone and replica set

A replica set maintains **multiple copies** for availability.

| | Standalone | Replica set |
|---|---|---|
| **What it is** | One MongoDB server process | A group of `mongod` processes holding the same dataset |
| **If the server fails** | The database is **unavailable** | A Secondary can become the new Primary |
| **Purpose** | Simple setup | **Redundancy** and **high availability** |

- The **Primary** receives writes. **Secondaries** hold copies.
- Example (adding Ana Cruz): the data is saved on the Primary, the Secondaries replicate it, and if the Primary fails a Secondary can become the new Primary.
- **Replication copies the same data for availability. Sharding distributes different data across servers.**

![Standalone and replica set](../assets/lesson-1/19-mongodb-standalone-and-replica-set.png)

### 4.7 Horizontal scaling: partitioning

**Partitioning** (often called **sharding** in MongoDB) splits a dataset into parts so different servers can store or process different portions.

- Server A: records for a selected range or key group. Server B: another partition. Server C: additional partition or capacity.
- **Benefit:** distributes storage and workload.
- **Considerations:** choosing a partition key, balancing data, routing requests.

![Horizontal scaling: partitioning](../assets/lesson-1/17-horizontal-scaling-partitioning.png)

### 4.8 Sharded cluster

Config servers store cluster **metadata**. Sharding supports **horizontal scaling**, and the **shard key** affects data distribution and query routing.

```
Client (Application) → Mongos (Query Router) → Shard 1 (Students A–M)
                                             → Shard 2 (Students N–Z)
                       Config Server (Cluster Metadata)
```

1. The client sends a request to **Mongos**.
2. Mongos routes it to the correct **shard**.
3. Each shard stores a portion of the data.
4. The **Config Server** stores cluster settings and metadata.

**Result:** the data is split across shards, so the database can handle more data and more users at the same time.

![MongoDB sharded cluster](../assets/lesson-1/20-mongodb-sharded-cluster.png)

### 4.9 CRUD operations

| Operation  | What it does            | Method        |
|------------|-------------------------|---------------|
| **C**reate | Insert a new document   | `insertOne()` |
| **R**ead   | Find matching documents | `find()`      |
| **U**pdate | Modify existing data    | `updateOne()` |
| **D**elete | Remove a document       | `deleteOne()` |

CRUD are **operations**, not separate NoSQL database models.

![MongoDB CRUD operations](../assets/lesson-1/23-mongodb-crud-operations.png)

---

## 5. Next Meeting

- Topic: **MongoDB installation and setup**, then **CRUD operations** (hands-on lab).
- **Bring your laptop.** The professor will install MongoDB together with the class so connection errors can be fixed live.

---

## 6. Corrections and Watch-Outs

Things that went wrong in the dictation or are easy to mix up:

- **Spelling:** "Sineo4j" is **Neo4j**. "Wild column" is **wide-column**. "Nose" is **nodes**. "Shaded cluster" and "shading" are **sharded cluster** and **sharding**. "Quaternary router" is the **query router** (`mongos`). "Figure set" is the **config server**.
- **Replica set vs sharding:** a replica set **copies** the same data to several servers (availability). Sharding **splits** different data across servers (scale). They are not the same thing.
- **Sharding split:** the slide's A–M and N–Z split is by the **first letter of the first name** (Ana, Mark go to Shard 1, Nina, Zach go to Shard 2). Mark **Santos** is in A–M, so it cannot be by surname.
- **Properties:** a property does not need an edge first. A node can have properties on its own (a user name), and an edge can have them too (a friendship date).
- **"SQL can't be split":** not on the slides, and not accurate. SQL databases can also be partitioned or sharded, usually with more setup.
- **Disadvantage wording:** "may be unnecessary for simple data" means a graph database is **overkill when data has few relationships**. It is not about edge names changing.
- **Large graphs:** the slide's disadvantage is that they are hard to **scale**.
- **Driver:** it is an application **library**, not a person who drives (and not an API).
- **Document vs key-value:** both look up by an ID. The difference is that a document's fields are visible to the database, so you can search inside it.

---

## 7. Quick Review

- **Q:** How do you choose between relational and NoSQL?
  **A:** Based on requirements, not popularity.
- **Q:** What do the four models organize data as?
  **A:** Key and value, document objects, column families, nodes and edges.
- **Q:** Which model fits a session cache?
  **A:** Key-value (Redis).
- **Q:** Which model fits a product catalog with varying details?
  **A:** Document (MongoDB).
- **Q:** Which model fits huge, distributed, write-heavy data?
  **A:** Wide-column (Cassandra).
- **Q:** Which model fits "who is connected to whom"?
  **A:** Graph (Neo4j).
- **Q:** In a graph, what are nodes, edges, and properties?
  **A:** Nodes are entities, edges are connections between nodes, properties describe a node or edge.
- **Q:** What does a flexible schema mean?
  **A:** Documents can vary, but data still needs careful design and validation.
- **Q:** What is the MongoDB hierarchy?
  **A:** Database, collection, document, fields.
- **Q:** What are the MongoDB names for table, row, column, and primary key?
  **A:** Collection, document, field, `_id`.
- **Q:** What does BSON stand for?
  **A:** Binary JSON.
- **Q:** What does `mongod` do? What does `mongos` do?
  **A:** `mongod` stores data and handles operations. `mongos` routes queries in a sharded cluster.
- **Q:** What does the config server store?
  **A:** Cluster settings and metadata.
- **Q:** Replica set or sharding: which copies the same data, and which splits it?
  **A:** Replica set copies it, sharding splits it.
- **Q:** What happens if a standalone server goes down?
  **A:** The database is unavailable.
- **Q:** What happens if a replica set's Primary fails?
  **A:** A Secondary can become the new Primary.
- **Q:** What does CRUD stand for, and what are the four methods?
  **A:** Create, Read, Update, Delete: `insertOne()`, `find()`, `updateOne()`, `deleteOne()`.

---

## Appendix: Slide Image Files

Save the slide screenshots in `database-systems/assets/lesson-1/` with these names:

| # | File name |
|---|-----------|
| 01 | `01-four-nosql-models.png` |
| 02 | `02-wide-column-database-model.png` |
| 03 | `03-key-value-how-it-works.png` |
| 04 | `04-wide-column-sample-databases.png` |
| 05 | `05-document-model-nested-flexible.png` |
| 06 | `06-key-value-sample-databases.png` |
| 07 | `07-graph-database-model.png` |
| 08 | `08-graph-model-nodes-and-edges.png` |
| 09 | `09-graph-sample-databases.png` |
| 10 | `10-key-value-and-document-models.png` |
| 11 | `11-wide-column-and-graph-models.png` |
| 12 | `12-scenario-product-catalog.png` |
| 13 | `13-mongodb-document-oriented-nosql.png` |
| 14 | `14-mongodb-architecture.png` |
| 15 | `15-mongodb-structure-and-vocabulary.png` |
| 16 | `16-nosql-does-not-mean-no-rules.png` |
| 17 | `17-horizontal-scaling-partitioning.png` |
| 18 | `18-mongodb-document-important-details.png` |
| 19 | `19-mongodb-standalone-and-replica-set.png` |
| 20 | `20-mongodb-sharded-cluster.png` |
| 21 | `21-mongodb-components.png` |
| 22 | `22-lesson-recap.png` |
| 23 | `23-mongodb-crud-operations.png` |