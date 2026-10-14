# Fliipazon Retail: Sales and Support Analysis using SQL

SQL analysis of sales trends, customer behaviour and agent performance for Fliipazon Retail, written in MySQL.

## Business Problem
Fliipazon Retail wants to understand its business performance and make better decisions. As a consultant data analyst, I used SQL to clean the data and answer questions on sales, customers and agents.

## Dataset
Three tables in the `interview` database:

| Table | Rows | Description |
|---|---|---|
| `agents` | 12 | Sales agents and their commission |
| `customer` | 25 | Customer details, grade, outstanding amount, assigned agent |
| `orders` | 36 | Orders with amount, advance paid, date, customer and agent |

## Data Cleaning
| Issue found | Fix |
|---|---|
| Trailing spaces in text columns (agent code, name, city, etc.) | `TRIM()` |
| Hidden carriage return (`\r`) in `agents.COUNTRY` and `orders.ORD_DESCRIPTION` | `REPLACE(col, CHAR(13), '')` |
| `agents.COUNTRY` had no real values | Set to `NULL` |
| 23 of 25 customer phone numbers were placeholders (e.g. `BBBBBBB`) | Set to `NULL` using `REGEXP` |
| 2 orders reference agent `A013`, who is not in `agents` | Kept as-is and flagged, no assumption made |
| No primary keys on `customer` and `orders` | Added primary keys |
| Oversized data types (`ORD_NUM`, `GRADE`) | Changed to `INT` and `TINYINT` |
| No duplicate records | Checked by ID and by order details |
| Grades only range from 0 to 3 (no grade 4 or 5) | Noted; affects Segments 2 and 3 |

## Key Findings
- **Sales:** 36 orders, total revenue 78,600, average order value 2,183.33.
- **Top cities by order amount:** Chennai (17,000), Mumbai (12,700), London (12,500).
- **Customers:** 25 customers; 32% (8 of 25) are repeat customers. Top spender is Ramanathan (10,500).
- **Top 5 agents by order amount:** Santakumar, Mukesh, Ivan, Anderson, Alford.
- **Most orders handled:** Mukesh (7 orders).
- **Commission revision:** Anderson (0.13 to 0.26) and Alford (0.12 to 0.24). No agent falls in the 750-1000 band.
- **Grade split:** grade 1 = 36%, grade 2 = 40%, grade 3 = 20%, grades 4 and 5 = 0%.

## Assumptions
- "Maximum order amount" for the agent bonus is taken as the total order amount per agent.
- Repeat customers are those with more than one order.
- `grade = 5` returns no rows because no customer has that grade; a data-driven version uses the highest grade that exists.
- Orders of the unknown agent A013 are excluded from agent-level reports.

## SQL Skills Used
Data cleaning, JOINs, GROUP BY / HAVING, subqueries, CTEs (including recursive), window functions (`RANK`, `DENSE_RANK`, `PARTITION BY`), `CASE`, user-defined function, stored procedures, cursor.

## Repository Files
- `data_setup.sql`: creates the database, tables and data
- `sales_support_analysis.sql`: cleaning and all analysis queries (Segments 1 to 5)

## How to Run
1. Run `data_setup.sql` in MySQL Workbench.
2. Run `sales_support_analysis.sql` once from top to bottom (MySQL 8.0+ required).
