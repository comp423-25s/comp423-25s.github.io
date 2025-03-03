# Introduction to Relational Databases & SQL

## What is a Relational Database?
A **relational database** is a way to store and organize data in a structured format using tables. Each table consists of **rows** (records) and **columns** (fields). The power of relational databases comes from their ability to establish relationships between tables, allowing for efficient data retrieval and consistency.

Unlike temporary data storage solutions, relational databases **persist data to disk**, meaning information is retained even after the database system shuts down. This persistence makes relational databases essential for applications that require long-term data storage, such as banking systems, e-commerce platforms, and content management systems.

## The Importance of ACID Properties

A core principle of modern relational databases is adherence to **ACID** properties, which ensure the reliability and consistency of data transactions:

* **Atomicity:** Ensures that a transaction is treated as a single, indivisible unit, meaning either all of its operations succeed, or none of them take effect. This prevents data corruption in case of failures.
* **Consistency:** Guarantees that a transaction brings the database from one valid state to another, maintaining integrity constraints at all times.
* **Isolation:** Ensures that transactions do not interfere with each other, even when executed concurrently, preserving the integrity of data operations.
* **Durability:** Ensures that once a transaction is committed, it remains permanently stored, even in the event of a system crash.

These properties are essential for applications that require strict data accuracy, such as financial systems, inventory management, and multi-user applications where concurrent data modifications occur.

## A Brief History of Relational Databases and SQL
The concept of **relational databases** was introduced by **Edgar F. Codd** in 1970 while working at IBM. He proposed the **relational model**, which organizes data into structured tables and uses mathematical **relational algebra** for querying and manipulating data.

Before relational databases, most systems used **hierarchical** or **network databases**, which were rigid and difficult to scale. Codd’s relational model revolutionized database management by making data more flexible and easier to query.

The **Structured Query Language (SQL)** was later developed in the 1970s at IBM as a way to interact with relational databases. It became the standard language for querying and managing relational data and was eventually adopted by major database systems.

### Modern, Popular Relational Databases

Today's most popular relational databases include:

- **PostgreSQL** (open-source, widely used for modern applications)
- **MySQL** (common in web applications)
- **SQLite** (lightweight, used in mobile and embedded systems)
- **Microsoft SQL Server** (enterprise-level database solution)
- **Oracle Database** (widely used in large corporations)


## Key Abstractions in a Relational Database

A **relational database management system (RDBMS)** provides a structured way to store and manipulate data. At the core of an RDBMS are the following key abstractions:

### Tables
A **table** is the fundamental structure in an RDBMS. It represents a collection of related data and is analogous to a spreadsheet or a well-organized list. Each table consists of **columns** (fields) that define the types of data stored and **rows** (records) that contain actual data entries.

### Columns (Fields)
A **column** defines the type of data that a table can store for a particular attribute. Each column has a **data type** that constrains the values it can hold, ensuring consistency and efficiency.

Columns can have constraints such as:
- **NOT NULL** – Ensures the column cannot have empty values.
- **UNIQUE** – Guarantees that all values in the column are distinct.
- **CHECK** – Enforces a condition on the column's values.
- **DEFAULT** – Assigns a default value if no explicit value is provided.

Some columns also have **fixed maximum widths** for storage efficiency. For example, a `VARCHAR(255)` column ensures that no value stored exceeds 255 characters. Limiting column widths helps optimize performance because it enables more efficient indexing, improves retrieval speed, and reduces overall memory consumption.

Here is a table of common PostgreSQL data types:

| Data Type    | Description |
|-------------|------------|
| `INTEGER`   | Whole numbers (e.g., 1, 2, 3) |
| `SERIAL`    | Auto-incrementing integer (commonly used for primary keys) |
| `TEXT`      | Variable-length string (unlimited size) |
| `VARCHAR(n)` | String with a maximum length of `n` characters |
| `BOOLEAN`   | True or False values |
| `DATE`      | Stores dates (YYYY-MM-DD) |
| `TIMESTAMP` | Stores date and time information |
| `DECIMAL(p, s)` | Precise fixed-point decimal numbers |
| `REAL` / `DOUBLE PRECISION` | Floating-point numbers |

### Rows (Records)
A **row** represents a single entry in a table, storing a unique combination of values across all the columns. Each row corresponds to a distinct instance of the entity the table models.

An analogy to **Object-Oriented Programming (OOP)** can be helpful here:
- A **table** is like a class definition in OOP.
- **Columns** define the attributes of the class.
- **Rows** are like instances (objects) created from the class.

For example, consider a `users` table:
```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL
);
```
This is similar to defining a `User` class in OOP:
```python
class User:
    def __init__(self, id, name, email):
        self.id = id
        self.name = name
        self.email = email
```
Each row in the `users` table represents a different `User` object with unique values for `id`, `name`, and `email`.

### Primary and Foreign Keys
A **primary key** uniquely identifies each row in a table, ensuring that no two rows have the same identifier. The choice of primary key depends on the scale and nature of the data.

For smaller datasets, using an `INTEGER` or `SERIAL` column as a unique ID is common. However, for very large-scale databases—such as those storing tweets, Instagram posts, or large-scale event logs—alternative schemes like **UUIDs** (Universally Unique Identifiers) or **timestamp-based IDs** are often used. These approaches provide a much larger range of unique identifiers and offer benefits such as distributed uniqueness and time-ordering properties.

For our purposes, since we are working with small datasets, we will primarily use **serial integers** as primary keys.

A **foreign key** establishes a relationship between two tables by linking a column in one table to the primary key of another. This ensures that the referenced data exists before any related data can be added, keeping the relationships valid.

#### Why Are Foreign Keys Useful?
Foreign keys help keep data **accurate and meaningful** by preventing orphaned records—data that doesn’t have a valid connection to other required information. They ensure that data in one table cannot reference a non-existent row in another table.

Foreign keys also make **queries more efficient**. Since they enforce relationships between tables, databases can use indexes on foreign key columns to speed up searches and joins. This means that when querying related data—such as fetching all orders associated with a user—the database can quickly locate matching records instead of scanning an entire table. 

Additionally, foreign keys help **maintain consistency** by automatically enforcing rules when data changes. For example, if a user is deleted, the database can be set up to either delete related orders (cascade delete) or prevent deletion if dependent records exist.

By using foreign keys, relational databases keep data organized, ensure relationships are valid, and improve query performance by leveraging indexing.

## Understanding SQL: A Domain-Specific Language for Databases

SQL (**Structured Query Language**) is a programming language designed specifically for interacting with relational databases. Unlike general-purpose programming languages like Python or Java, SQL is **domain-specific**, meaning it is specialized for tasks related to storing, retrieving, and manipulating structured data.

SQL enables users to:

- Define and modify database structures.
- Insert, update, and delete data within those structures.
- Query and retrieve meaningful information efficiently.

SQL is divided into two main categories:

### 1. DDL (Data Definition Language)

DDL is responsible for defining and managing the structure of a database. Commands in this category affect the schema of a database, such as creating, modifying, and deleting tables.

#### Common DDL Commands:

- `CREATE TABLE` – Defines a new table structure.
- `ALTER TABLE` – Modifies an existing table.
- `DROP TABLE` – Deletes a table and all its data.

Example:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL
);
```

### 2. DML (Data Manipulation Language)

DML is used to work with the data inside database tables. These commands allow users to insert new records, update existing ones, delete data, and query stored information.

#### Common DML Commands:

- `INSERT INTO` – Adds new records to a table.
- `UPDATE` – Modifies existing records.
- `DELETE` – Removes records.
- `SELECT` – Retrieves data from tables.

Example:

```sql
INSERT INTO users (name, email) VALUES ('Alice', 'alice@email.com');
SELECT * FROM users;
```

By mastering both DDL and DML, you gain the ability to design, manage, and interact with relational databases effectively. Understanding these distinctions helps clarify how SQL serves both structural and operational purposes in database management.


## Experimenting with SQL in a PostgreSQL Docker Container

To follow along with this tutorial, you can run PostgreSQL inside a **Docker container**.

### Step 1: Pull and Run PostgreSQL

From your host machine's terminal run the following command to start a PostgreSQL container:

```sh
docker run \
  --name postgres \
  --env POSTGRES_PASSWORD=mysecretpassword \
  --publish 5432:5432 \
  --detach \
  postgres:latest
```

Explanation of flags:

- `--name postgres` → Names the container **postgres**.
- `--env POSTGRES_PASSWORD=mysecretpassword` → Sets the default password for the PostgreSQL instance.
- `--publish 5432:5432` → Maps port **5432** from the container to the host, allowing connections.
- `--detach` → Runs the container in **detached mode**, meaning it runs in the background.
- `postgres:latest` → Specifies the **PostgreSQL image** to use, with `latest` pulling the most recent stable version.

### Where is the Data Stored?
When using a Docker container without explicit volume mapping, PostgreSQL stores its data **inside the container’s filesystem**. This means that even if you stop and restart the container, your data will still be available. However, if the container is removed, the data will be lost.

For production environments, best practice is to **mount a volume** to ensure that database files persist independently of the container lifecycle. However, for simplicity in this tutorial, we are relying on the container's internal storage.

### Step 2: Connect to PostgreSQL
To interact with the running PostgreSQL container, use the following command:

```sh
docker exec \
  --interactive \
  --tty \
  --user postgres \
  postgres \
  psql
```

This command does **not** start a new container; it connects to the **existing detached container** named `postgres` and runs the `psql` command inside it.

### Exiting PostgreSQL
To quit `psql`, type:

```sh
\q
```

### Stopping, Restarting, and Removing the PostgreSQL Container
If you need to stop the PostgreSQL container but keep the data intact, run:

```sh
docker stop postgres
```

To restart the container and restore access to the database, use:

```sh
docker start postgres
```

This ensures that all changes made to the database remain intact between restarts.

If you no longer need the container and want to permanently delete it, along with all stored data, run:

```sh
docker rm postgres
```

**Important:** Removing the container deletes all stored data since we are not using a volume in this tutorial. In real applications, using a Docker volume ensures data persistence beyond container removal.
Now that you have PostgreSQL running inside Docker, you can start writing SQL commands.

## Basics of SQL: Creating Tables, Inserting Data, and Querying with SELECT

Now that we have PostgreSQL running inside Docker, let's explore some basic SQL commands. We'll cover:

- **Creating tables**
- **Inserting data**
- **Querying data with SELECT**
- **Filtering results with WHERE**
- **Sorting and limiting query results**
- **Joining related tables**

### Creating a Table
A table in SQL is defined using the `CREATE TABLE` statement. Each table consists of **columns**, where each column has a specific data type.

Let's create a `users` table:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Explanation:
- `id SERIAL PRIMARY KEY` → A unique, auto-incrementing identifier.
- `name TEXT NOT NULL` → A required text field.
- `email TEXT UNIQUE NOT NULL` → Ensures emails are unique and required.
- `created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP` → Automatically stores the time the record was created.

### Inserting Data
To add records to a table, use `INSERT INTO`:

```sql
INSERT INTO users (name, email) VALUES ('Alice', 'alice@email.com');
INSERT INTO users (name, email) VALUES ('Bob', 'bob@email.com');
INSERT INTO users (name, email) VALUES ('Charlie', 'charlie@email.com');
```

### Retrieving Data with `SELECT`
To retrieve all records from a table:

```sql
SELECT * FROM users;
```

This returns:

| id | name    | email              | created_at          |
|----|--------|--------------------|---------------------|
| 1  | Alice  | alice@email.com    | 2025-03-02 12:00:00 |
| 2  | Bob    | bob@email.com      | 2025-03-02 12:01:00 |
| 3  | Charlie| charlie@email.com  | 2025-03-02 12:02:00 |

### Filtering Results with `WHERE`
Use `WHERE` to filter results based on conditions.

#### Find a specific user by email:
```sql
SELECT * FROM users WHERE email = 'alice@email.com';
```

#### Find all users whose names start with "B":
```sql
SELECT * FROM users WHERE name LIKE 'B%';
```

#### Find users created after a specific date:
```sql
SELECT * FROM users WHERE created_at > '2025-03-01';
```

### Sorting and Limiting Query Results
#### Sort users by name (ascending):
```sql
SELECT * FROM users ORDER BY name ASC;
```

#### Get the most recently created user:
```sql
SELECT * FROM users ORDER BY created_at DESC LIMIT 1;
```

## Creating a Table with Relationships

To model relationships, we use **foreign keys**. Let's say we have an `orders` table that references `users`:

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    total_price DECIMAL(10,2) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

#### Explanation:
- `user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE` → Ensures each order belongs to a valid user.
- `ON DELETE CASCADE` → If a user is deleted, their orders are automatically removed.

### Inserting Data into Related Tables
We can now add orders for users:

```sql
INSERT INTO orders (user_id, total_price) VALUES (1, 25.50);
INSERT INTO orders (user_id, total_price) VALUES (2, 15.75);
INSERT INTO orders (user_id, total_price) VALUES (1, 45.00);
```

### Querying with `JOIN`
To fetch orders along with user names:

```sql
SELECT orders.id, users.name, orders.total_price, orders.created_at
FROM orders
JOIN users ON orders.user_id = users.id;
```

#### Example Output:
| id | name    | total_price | created_at          |
|----|--------|------------|---------------------|
| 1  | Alice  | 25.50      | 2025-03-02 13:00:00 |
| 2  | Bob    | 15.75      | 2025-03-02 13:05:00 |
| 3  | Alice  | 45.00      | 2025-03-02 13:10:00 |

This retrieves **orders** along with the **user who placed them**.

With these additions, students get a more complete and practical introduction to SQL!

## Conclusion

In this chapter, we explored the fundamentals of relational databases and SQL. We introduced relational databases, the importance of ACID properties, and the evolution of SQL. We then covered key relational database concepts, including tables, columns, rows, primary keys, and foreign keys. Additionally, we demonstrated SQL queries for creating tables, inserting data, retrieving data using SELECT, filtering with WHERE, sorting, limiting results, and using JOIN to retrieve data across related tables.

Understanding relational databases and SQL is crucial for managing structured data efficiently. By mastering these fundamental concepts, you are well-equipped to design, query, and manipulate databases effectively, forming a strong foundation for working with modern data-driven applications.

