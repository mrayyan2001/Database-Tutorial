# Complete Tutorial on Converting an ER Diagram into a Database Schema

Welcome to this comprehensive tutorial on converting an **Entity-Relationship (ER) Diagram** into a **Database Schema**! By the end of this guide, you'll understand how to take the visual representation of your data model and translate it into actual database tables. This will help you design effective relational databases without needing to watch the video.

## What is an ER Diagram?

An **ER Diagram** is a visual tool used to model data in a database. It consists of entities, attributes, and relationships that define how data is structured and interconnected.

### Why Convert an ER Diagram to a Database Schema?

- **Organizes Data**: Helps in organizing data into tables and columns.
- **Defines Relationships**: Clearly shows how different tables interact.
- **Guides Implementation**: Makes it easier to implement the database in a relational database management system (RDBMS).

## Steps to Convert an ER Diagram into a Database Schema

Let’s walk through the process step-by-step using a sample ER diagram.

### Step 1: Mapping Regular Entity Types

**Regular Entities** are standalone entities with their own attributes.

#### Example:

From the ER diagram:

- **Entities**: Branch, Client, Employee

For each regular entity, create a table with all its attributes. The primary key is underlined.

```plaintext
Employee (
    EmployeeID (PK),
    FirstName,
    LastName,
    BirthDate,
    Sex,
    Salary
)

Branch (
    BranchID (PK),
    BranchName,
    ManagerID (FK)
)

Client (
    ClientID (PK),
    ClientName
)
```

### Step 2: Mapping Weak Entity Types

**Weak Entities** depend on another entity for their existence.

#### Example:

- **Weak Entity**: Branch Supplier
- **Owner Entity**: Branch

Create a table for the weak entity. The primary key is a combination of the weak entity's partial key and the owner's primary key.

```plaintext
BranchSupplier (
    BranchID (PK, FK),
    SupplierName (PK),
    SupplyType
)
```

### Step 3: Mapping Binary One-to-One Relationship Types

**One-to-One Relationships** involve two entities where each instance of one entity relates to exactly one instance of the other.

#### Example:

- **Relationship**: Manages (Branch and Employee)

Include the primary key of one entity as a foreign key in the other, favoring total participation.

```plaintext
Branch (
    BranchID (PK),
    BranchName,
    ManagerID (FK) -> Employee(EmployeeID)
)
```

### Step 4: Mapping Binary One-to-Many Relationship Types

**One-to-Many Relationships** involve two entities where one instance of the first entity can relate to multiple instances of the second.

#### Example:

- **Relationships**:
  - **Works For** (Branch and Employee)
  - **Handles** (Branch and Client)
  - **Supervision** (Employee and Employee)

Include the primary key of the "one" side as a foreign key in the "many" side.

```plaintext
Employee (
    EmployeeID (PK),
    FirstName,
    LastName,
    BirthDate,
    Sex,
    Salary,
    BranchID (FK) -> Branch(BranchID),
    SuperID (FK) -> Employee(EmployeeID)
)

Client (
    ClientID (PK),
    ClientName,
    BranchID (FK) -> Branch(BranchID)
)
```

### Step 5: Mapping Binary Many-to-Many Relationship Types

**Many-to-Many Relationships** involve two entities where multiple instances of one entity can relate to multiple instances of the other.

#### Example:

- **Relationship**: Works With (Employee and Client)

Create a new table with a composite primary key consisting of both entities' primary keys. Include any relationship attributes.

```plaintext
WorksWith (
    EmployeeID (PK, FK) -> Employee(EmployeeID),
    ClientID (PK, FK) -> Client(ClientID),
    TotalSales
)
```

## Constructing the Final Database Schema

Putting it all together, here’s what the final database schema might look like:

```plaintext
Employee (
    EmployeeID (PK),
    FirstName,
    LastName,
    BirthDate,
    Sex,
    Salary,
    BranchID (FK) -> Branch(BranchID),
    SuperID (FK) -> Employee(EmployeeID)
)

Branch (
    BranchID (PK),
    BranchName,
    ManagerID (FK) -> Employee(EmployeeID)
)

Client (
    ClientID (PK),
    ClientName,
    BranchID (FK) -> Branch(BranchID)
)

BranchSupplier (
    BranchID (PK, FK) -> Branch(BranchID),
    SupplierName (PK),
    SupplyType
)

WorksWith (
    EmployeeID (PK, FK) -> Employee(EmployeeID),
    ClientID (PK, FK) -> Client(ClientID),
    TotalSales
)
```

## Visualizing Relationships

To better understand the relationships, you can draw arrows between tables to indicate foreign key constraints.

```plaintext
Employee --(SuperID)--> Employee
Employee --(BranchID)--> Branch
Branch --(ManagerID)--> Employee
Client --(BranchID)--> Branch
WorksWith --(EmployeeID)--> Employee
WorksWith --(ClientID)--> Client
BranchSupplier --(BranchID)--> Branch
```

## Example Database Tables

Here’s an example of how these tables might look when populated with data:

### Employee Table

| EmployeeID | FirstName | LastName | BirthDate  | Sex | Salary | BranchID | SuperID |
| ---------- | --------- | -------- | ---------- | --- | ------ | -------- | ------- |
| 101        | Michael   | Scott    | 1970-01-01 | M   | 60000  | 2        | NULL    |
| 102        | Angela    | Martin   | 1980-02-02 | F   | 50000  | 2        | 101     |

### Branch Table

| BranchID | BranchName | ManagerID |
| -------- | ---------- | --------- |
| 2        | Scranton   | 101       |

### Client Table

| ClientID | ClientName     | BranchID |
| -------- | -------------- | -------- |
| 301      | Dunder Mifflin | 2        |

### Works With Table

| EmployeeID | ClientID | TotalSales |
| ---------- | -------- | ---------- |
| 102        | 301      | 5000       |

### Branch Supplier Table

| BranchID | SupplierName | SupplyType |
| -------- | ------------ | ---------- |
| 2        | Sabor Inc    | Paper      |

## Conclusion

Converting an ER diagram into a database schema involves mapping entities to tables, defining primary and foreign keys, and handling various types of relationships. By following these steps, you can effectively translate your data model into a structured database schema.

# Notes

## One to One Relationship

person and passport

person can have only one passport; passport belongs to only one person

passport can't be without a person so it's a total participation; person can exist without a passport so it's a partial participation.

where the foreign key?

the favorite way is to put the foreign key in the entity that has the total participation. (passport)

the foreign key should be unique in the passport table. (one in the side of passport)

## One to Many Relationship

department and employee

one department can have many employees; one employee can belong to only one department

the foreign key should be in the many side (employee)

the foreign key should not be unique in the employee table. (many in the side of employee)

## Many to Many Relationship

student and course

one student can take many courses; one course can be taken by many students

to implement this relationship, we need a third table called the junction table.

the junction table should have the primary keys of the two entities and any other attributes related to the relationship.

the primary key of the junction table should be a composite key.
