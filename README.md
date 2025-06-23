# library-db-schema
# 📙 Library Management System Database Design

## ✅ 1. Domain: Library

This system manages books, authors, library members, staff, and book loans.

---

## ✅ 2. Entities and Relationships

### 📅 Entities

* **Authors**: Writers of books
* **Books**: Items available for borrowing
* **Members**: Registered library users
* **Staff**: People managing the library
* **Loans**: Records of book borrowings

### 🔗 Relationships

* A **Book** can have multiple **Authors** (many-to-many)
* An **Author** can write multiple **Books** (many-to-many)
* A **Member** can borrow multiple **Books** (one-to-many)
* A **Staff** member issues loans (one-to-many)

---

## ✅ 3. SQL Table Creation with Primary and Foreign Keys

```sql
-- Authors table
CREATE TABLE Authors (
    AuthorID INT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL
);

-- Books table
CREATE TABLE Books (
    BookID INT PRIMARY KEY,
    Title VARCHAR(150) NOT NULL,
    ISBN VARCHAR(13) UNIQUE,
    PublicationYear INT,
    CopiesAvailable INT
);

-- BookAuthors table (junction table for many-to-many)
CREATE TABLE BookAuthors (
    BookID INT,
    AuthorID INT,
    PRIMARY KEY (BookID, AuthorID),
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (AuthorID) REFERENCES Authors(AuthorID)
);

-- Members table
CREATE TABLE Members (
    MemberID INT PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    JoinDate DATE
);

-- Staff table
CREATE TABLE Staff (
    StaffID INT PRIMARY KEY,
    Name VARCHAR(100),
    Role VARCHAR(50)
);

-- Loans table
CREATE TABLE Loans (
    LoanID INT PRIMARY KEY,
    BookID INT,
    MemberID INT,
    StaffID INT,
    LoanDate DATE,
    ReturnDate DATE,
    FOREIGN KEY (BookID) REFERENCES Books(BookID),
    FOREIGN KEY (MemberID) REFERENCES Members(MemberID),
    FOREIGN KEY (StaffID) REFERENCES Staff(StaffID)
);
```

---

## ✅ 4. Sample Data Insertion

```sql
-- Authors
INSERT INTO Authors VALUES (1, 'J.K. Rowling');
INSERT INTO Authors VALUES (2, 'George R.R. Martin');

-- Books
INSERT INTO Books VALUES (101, 'Harry Potter', '9780747532743', 1997, 5);
INSERT INTO Books VALUES (102, 'Game of Thrones', '9780553573404', 1996, 3);

-- BookAuthors
INSERT INTO BookAuthors VALUES (101, 1);
INSERT INTO BookAuthors VALUES (102, 2);

-- Members
INSERT INTO Members VALUES (201, 'Alice', 'alice@example.com', '2023-01-10');
INSERT INTO Members VALUES (202, 'Bob', 'bob@example.com', '2023-03-15');

-- Staff
INSERT INTO Staff VALUES (301, 'Mr. John', 'Librarian');
INSERT INTO Staff VALUES (302, 'Ms. Jane', 'Assistant');

-- Loans
INSERT INTO Loans VALUES (401, 101, 201, 301, '2024-06-01', '2024-06-10');
INSERT INTO Loans VALUES (402, 102, 202, 302, '2024-06-02', NULL);
```

---

## ✅ 5. ER Diagram (Text Format)

```
[Authors]         [Books]           [Members]         [Staff]
  AuthorID ───────┐    │                  │                 │
     Name       └─>[BookAuthors]<─────┐ │                 │
                            BookID └─┐ │                 │
                            AuthorID   │                 │
                                        ▼
                                     [Loans]
                                 LoanID (PK)
                                 BookID (FK)
                                 MemberID (FK)
                                 StaffID (FK)
                                 LoanDate
                                 ReturnDate
```

---

## ✅ 6. Table Format Overview

### 📖 Books

| BookID | Title           | ISBN          | Year | CopiesAvailable |
| ------ | --------------- | ------------- | ---- | --------------- |
| 101    | Harry Potter    | 9780747532743 | 1997 | 5               |
| 102    | Game of Thrones | 9780553573404 | 1996 | 3               |

### 👤 Authors

| AuthorID | Name               |
| -------- | ------------------ |
| 1        | J.K. Rowling       |
| 2        | George R.R. Martin |

### 🔗 BookAuthors

| BookID | AuthorID |
| ------ | -------- |
| 101    | 1        |
| 102    | 2        |

### 👥 Members

| MemberID | Name  | Email                                         | JoinDate   |
| -------- | ----- | --------------------------------------------- | ---------- |
| 201      | Alice | [alice@example.com](mailto:alice@example.com) | 2023-01-10 |
| 202      | Bob   | [bob@example.com](mailto:bob@example.com)     | 2023-03-15 |

### 🧑‍🏫 Staff

| StaffID | Name     | Role      |
| ------- | -------- | --------- |
| 301     | Mr. John | Librarian |
| 302     | Ms. Jane | Assistant |

### 📄 Loans

| LoanID | BookID | MemberID | StaffID | LoanDate   | ReturnDate |
| ------ | ------ | -------- | ------- | ---------- | ---------- |
| 401    | 101    | 201      | 301     | 2024-06-01 | 2024-06-10 |
| 402    | 102    | 202      | 302     | 2024-06-02 | NULL       |

---

## ✅ 7. Concepts Explained

| Concept          | Explanation                                                   |
| ---------------- | ------------------------------------------------------------- |
| **Domain**       | The area of application (Library).                            |
| **Entity**       | A real-world object to store data about (Book, Member, etc.). |
| **Relationship** | How entities interact (e.g., Book to Author is many-to-many). |
| **CREATE TABLE** | SQL command to define a new table.                            |
| **Primary Key**  | Unique identifier for a record in a table.                    |
| **Foreign Key**  | A field that links one table to another.                      |
