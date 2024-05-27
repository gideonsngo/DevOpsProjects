# Study Notes on MERN Stack
Database Management Systems (DBMS) are software systems that enable the creation, management, and manipulation of databases. There are several types of DBMS, each suited to different use cases and data models. Here’s an overview of the main types:

### 1. Hierarchical DBMS
- **Structure**: Data is organized in a tree-like structure where each record has a single parent and can have multiple children.
- **Use Case**: Suitable for applications with a clear hierarchical relationship, such as organizational charts and file systems.
- **Example**: IBM Information Management System (IMS).

### 2. Network DBMS
- **Structure**: Data is organized in a graph structure, allowing more complex relationships. Records can have multiple parent and child records.
- **Use Case**: Ideal for applications requiring flexible relationships, like telecommunications networks and complex inventories.
- **Example**: Integrated Data Store (IDS).

### 3. Relational DBMS (RDBMS)
- **Structure**: Data is stored in tables (relations) with rows and columns. Each table represents an entity type, and relationships are maintained through foreign keys.
- **Use Case**: Best for structured data with well-defined relationships, such as transactional systems, financial applications, and ERP systems.
- **Example**: MySQL, PostgreSQL, Oracle Database, Microsoft SQL Server.

### 4. Object-Oriented DBMS (OODBMS)
- **Structure**: Data is stored as objects, similar to object-oriented programming. Supports classes, inheritance, and polymorphism.
- **Use Case**: Suitable for applications with complex data representations, like CAD/CAM, multimedia applications, and software engineering.
- **Example**: ObjectDB, db4o.

### 5. NoSQL DBMS
- **Structure**: Encompasses various database models designed to handle large volumes of unstructured, semi-structured, or structured data. Includes key-value stores, document stores, column-family stores, and graph databases.
- **Use Case**: Ideal for big data applications, real-time web applications, and scenarios requiring horizontal scaling and flexible schema design.
- **Example**: MongoDB (document store), Cassandra (column-family store), Redis (key-value store), Neo4j (graph database).

### 6. NewSQL DBMS
- **Structure**: Combines the scalability of NoSQL with the ACID (Atomicity, Consistency, Isolation, Durability) guarantees of traditional relational databases.
- **Use Case**: Suitable for high-transaction environments requiring strong consistency and scalability.
- **Example**: Google Spanner, CockroachDB.

## Relational DBMS vs. NoSQL DBMS

### Relational DBMS (RDBMS)
- **Data Model**: Uses a structured schema based on tables with rows and columns. Relationships are defined using foreign keys.
- **ACID Transactions**: Ensures strong consistency, reliability, and integrity through ACID properties.
- **Query Language**: Uses SQL (Structured Query Language) for defining and manipulating data.
- **Scalability**: Generally scales vertically (adding more power to a single server), though some systems support horizontal scaling.
- **Use Cases**: Suitable for applications with complex querying needs, transactional consistency, and structured data (e.g., banking, finance, ERP systems).

### NoSQL DBMS
- **Data Model**: Supports various models (document, key-value, column-family, graph) and flexible schemas.
- **BASE Transactions**: Emphasizes availability and partition tolerance over strong consistency, often adhering to the BASE (Basically Available, Soft state, Eventual consistency) principle.
- **Query Language**: Varies depending on the type of NoSQL database; often uses JSON-like query languages or specific APIs.
- **Scalability**: Designed for horizontal scaling (adding more servers), making them suitable for large-scale, distributed environments.
- **Use Cases**: Best for applications with massive amounts of unstructured data, needing high availability, and requiring flexible schema design (e.g., social networks, big data analytics, real-time web apps).

In summary, while RDBMS is ideal for applications requiring structured data and transactional consistency, NoSQL databases excel in handling unstructured data and scaling horizontally to manage large volumes of data across distributed systems. The choice between RDBMS and NoSQL depends on the specific needs and characteristics of the application in question.

## Web Application Frameworks
Web application frameworks are software frameworks designed to support the development of web applications, including web services, web resources, and web APIs. They provide a standardized way to build and deploy web applications by offering tools, libraries, and best practices to streamline development.

### Server-Side (Backend) Frameworks
Server-side frameworks handle the backend logic of a web application, which includes database interactions, user authentication, application logic, and server configuration. These frameworks are executed on the server and generate the HTML, CSS, and JavaScript that the client-side (frontend) will display. Popular Server-Side Framweorks include: Jango, ExpressJS, Laravel etc

### Client-Side (Frontend) Frameworks
Client-side frameworks handle the frontend logic of a web application, focusing on what the user interacts with directly in the browser. These frameworks are used to create dynamic and responsive user interfaces. Popular Client-Side Framweorks include: React, Angular, Vue.js etc
