
# Claim Insurance Database Project

A practice project to develop SQL skills, this project involves designing a relational database schema for an insurance company, populating the database with realistic sample data, and creating analytical queries to extract business insights. It also includes performance optimization strategies and role-based permissions.

## Table of Contents
1. [Project Overview](#project-overview)
2. [Database Schema](#database-schema)
3. [Data Population](#data-population)
4. [Analytical Queries](#analytical-queries)
5. [Performance Optimization](#performance-optimization)
6. [Roles and Permissions](#roles-and-permissions)
7. [Lessons Learned](#lessons-learned)


## Project Overview

The goal of this project is to create a database for managing customer policies, claims, and policy types, along with queries to gain insights into claims frequency, distribution, and policy details. This project is useful for demonstrating skills in SQL, relational database design, and basic data analysis.

Key features include:
- Database schema creation with relational tables for `Customers`, `Policies`, `Claims`, and `PolicyTypes`
- Insertion of realistic data for testing
- Analytical queries for data insights on claims frequency and claim amounts
- Index creation for performance optimization
- User roles with specific permissions

## Database Schema

### 1. Customers Table
Stores customer information with details such as ID, name, birth date, gender, and address.

| Column         | Type         | Description                     |
|----------------|--------------|---------------------------------|
| `CustomerID`   | `SERIAL`     | Primary key                     |
| `FirstName`    | `VARCHAR(50)`| Customer's first name           |
| `LastName`     | `VARCHAR(50)`| Customer's last name            |
| `DateOfBirth`  | `DATE`       | Customer's date of birth        |
| `Gender`       | `CHAR(1)`    | Customer's gender               |
| `Address`      | `VARCHAR(100)`| Customer's address             |
| `City`         | `VARCHAR(50)`| Customer's city                 |
| `State`        | `VARCHAR(50)`| Customer's state                |
| `ZipCode`      | `VARCHAR(10)` | Customer's ZIP code            |

### 2. PolicyTypes Table
Defines various insurance policy types (e.g., Auto, Home, Life, Health) with descriptions.

| Column           | Type           | Description                    |
|------------------|----------------|--------------------------------|
| `PolicyTypeID`   | `SERIAL`       | Primary key                    |
| `PolicyTypeName` | `VARCHAR(50)`  | Type of insurance policy       |
| `Description`    | `TEXT`         | Description of policy coverage |

### 3. Policies Table
Links each customer to a policy type and specifies policy duration and premium amount.

| Column           | Type           | Description                    |
|------------------|----------------|--------------------------------|
| `PolicyID`       | `SERIAL`       | Primary key                    |
| `CustomerID`     | `INT`          | Foreign key referencing Customers table |
| `PolicyTypeID`   | `INT`          | Foreign key referencing PolicyTypes table |
| `PolicyStartDate`| `DATE`         | Start date of the policy       |
| `PolicyEndDate`  | `DATE`         | End date of the policy         |
| `Premium`        | `DECIMAL(10,2)`| Premium amount                 |

### 4. Claims Table
Stores information related to claims made on policies, including claim date, amount, description, and status.

| Column           | Type           | Description                    |
|------------------|----------------|--------------------------------|
| `ClaimID`        | `SERIAL`       | Primary key                    |
| `PolicyID`       | `INT`          | Foreign key referencing Policies table |
| `ClaimDate`      | `DATE`         | Date of the claim              |
| `ClaimAmount`    | `DECIMAL(10,2)`| Amount claimed                 |
| `ClaimDescription`| `TEXT`        | Description of the claim       |
| `ClaimStatus`    | `VARCHAR(50)`  | Status of the claim (e.g., Approved, Pending) |

## Data Population

The project includes sample data insertion to demonstrate a variety of scenarios:
- **Policy Types**: Types of insurance such as Auto, Home, Life, Health.
- **Customers**: Example customers with diverse demographic information.
- **Policies**: Different policies with various start and end dates, and premium amounts.
- **Claims**: Sample claims on policies with different claim amounts, descriptions, and statuses.

### Example Inserts
```sql
INSERT INTO PolicyTypes (PolicyTypeName, Description) VALUES
('Auto', 'Insurance coverage for automobiles'),
('Home', 'Insurance coverage for residential homes'),
('Life', 'Long-term insurance coverage upon the policyholder''s death'),
('Health', 'Insurance coverage for medical and surgical expenses');
```

# Analytical Queries
Total Number of Claims per Policy Type
```sql
SELECT
    pt.PolicyTypeName,
    COUNT(c.ClaimID) AS TotalClaims
FROM
    Claims c
JOIN
    Policies p ON c.PolicyID = p.PolicyID
JOIN
    PolicyTypes pt ON p.PolicyTypeID = pt.PolicyTypeID
GROUP BY
    pt.PolicyTypeName
ORDER BY
    TotalClaims DESC;
```

Monthly Claim Frequency and Average Claim Amount
```sql
SELECT
    DATE_TRUNC('month', ClaimDate) AS ClaimMonth,
    COUNT(*) AS ClaimFrequency,
    AVG(ClaimAmount) AS AverageClaimAmount
FROM
    Claims
GROUP BY
    ClaimMonth
ORDER BY
    ClaimMonth;
```

# Performance Optimization
An index is created on the ClaimDate column in the Claims table to improve query performance, especially for time-based claims analysis.

```sql
CREATE INDEX idx_claims_claimdate ON Claims(ClaimDate);

```


# Roles and Permissions
   1- ClaimsAnalyst: Read-only access to claims and policies data.
   2- ClaimsManager: Full access to claims data and ability to update policy information.

   ```sql
  CREATE ROLE ClaimsAnalyst LOGIN PASSWORD 'password1';
 CREATE ROLE ClaimsManager LOGIN PASSWORD 'password2';

 GRANT SELECT ON Claims, Policies, PolicyTypes TO ClaimsAnalyst;
 GRANT SELECT, INSERT, UPDATE, DELETE ON Claims, Policies, PolicyTypes TO ClaimsManager;
```


# Lessons Learned
  This project provided valuable experience in:
    1- Designing and normalizing database schemas
    2- Writing SQL queries for both operational and analytical purposes
    3- Indexing strategies for performance optimization
    4- Managing access control and user roles in a database environment

