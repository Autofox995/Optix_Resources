## Comprehensive SQL Testing Results and Examples

This document provides detailed insights into SQL functionality supported by FactoryTalk Optix. It consolidates testing outcomes and includes robust examples for reference.

### Basic Query Testing

#### Supported

1. **Select All Columns**
   - **Query:**
     ```sql
     SELECT * FROM TestTable1
     ```
   - **Result:** Successfully returned all rows and columns.

2. **Select Specific Columns**
   - **Query:**
     ```sql
     SELECT ID, Name FROM TestTable1
     ```
   - **Result:** Successfully returned the ID and Name columns.

3. **Column Aliases**
   - **Query:**
     ```sql
     SELECT Name AS EmployeeName, Salary AS EmployeeSalary FROM TestTable1
     ```
   - **Result:** Successfully used aliases for columns.
   - **Explanation:** Column aliases provide a temporary name for columns in the result set, improving readability.

4. **Filter Rows with Conditions**
   - **Queries:**
     ```sql
     SELECT * FROM TestTable1 WHERE Salary > 60000
     SELECT * FROM TestTable1 WHERE Salary > 60000 AND DepartmentID = 101
     SELECT * FROM TestTable1 WHERE DepartmentID = 101 OR DepartmentID = 102
     SELECT * FROM TestTable1 WHERE NOT DepartmentID = 103
     SELECT * FROM TestTable2 WHERE Location = 'Chicago'
     ```
   - **Result:** Successfully filtered rows based on conditions.
   - **Explanation:** Filters apply conditional logic to limit the rows returned by the query.

5. **Sorting Results**
   - **Queries:**
     ```sql
     SELECT * FROM TestTable1 ORDER BY Salary ASC
     SELECT * FROM TestTable1 ORDER BY Salary DESC
     ```
   - **Result:** Successfully sorted results in ascending and descending order.
   - **Explanation:** Sorting organizes rows based on specified column(s).

6. **Pagination**
   - **Queries:**
     ```sql
     SELECT * FROM TestTable1 LIMIT 5
     SELECT * FROM TestTable1 LIMIT 5 OFFSET 5
     ```
   - **Result:** Successfully retrieved subsets of rows.
   - **Explanation:** Pagination breaks large datasets into manageable chunks.

7. **Complex Filtering with Multiple Conditions**
   - **Query:**
     ```sql
     SELECT * FROM TestTable1
     WHERE (Salary > 60000 AND DepartmentID = 101) OR
     (Salary < 50000 AND DepartmentID = 102)
     ```
   - **Result:** Successfully applied multiple conditional filters with both AND and OR operators.
   - **Explanation:** Demonstrates combining conditions for more specific queries.

8. **Combining `*` with Standard Column**
   - **Query:**
     ```sql
     SELECT *, Timestamp FROM TestTable4
     ```
   - **Result:** Successfully added the `Timestamp` column to the output, along with all existing columns.
   - **Example Data:**
     | ID  | Timestamp           | EventName      | Duration | Timestamp           |
     |-----|---------------------|----------------|----------|---------------------|
     | 1   | 1/1/2025 12:00 PM   | Maintenance    | 2.5      | 1/1/2025 12:00 PM   |
     | 2   | 1/1/2025 12:15 PM   | Adjustment     | 1.5      | 1/1/2025 12:15 PM   |

9. **Combining `*` with Literal Values**
   - **Query:**
     ```sql
     SELECT *, 'StaticValue' AS LiteralColumn FROM TestTable4
     ```
   - **Result:** Successfully added a constant column with the value `StaticValue` for all rows.
   - **Example Data:**
     | ID  | Timestamp           | EventName      | Duration | LiteralColumn |
     |-----|---------------------|----------------|----------|---------------|
     | 1   | 1/1/2025 12:00 PM  | Maintenance    | 2.5      | StaticValue   |
     | 2   | 1/1/2025 12:15 PM  | Adjustment     | 1.5      | StaticValue   |

10. **Special Characters in Identifiers**
   - **Query:**
     ```sql
     SELECT "Water level", ID FROM TestTable1
     ```
   - **Result:** Successfully handled column names with spaces using double quotes.
   - **Description:** Confirms support for special characters in identifiers.

11. **IS NULL and IS NOT NULL**
   - **Queries:**
     ```sql
     SELECT * FROM TestTable1 WHERE DepartmentID IS NULL;
     SELECT * FROM TestTable1 WHERE DepartmentID IS NOT NULL;
     ```
   - **Result:** Successfully filtered rows for `NULL` and `NOT NULL` conditions.
   - **Description:** Confirms proper handling of `NULL` values in filtering.

#### Unsupported

1. **DISTINCT Keyword**
   - **Query:**
     ```sql
     SELECT DISTINCT DepartmentID FROM TestTable1
     ```
   - **Error:** Syntax error.
   - **Explanation:** DISTINCT functionality is not supported in FactoryTalk Optix.

2. **Subqueries in SELECT Clause**
   - **Query:**
     ```sql
     SELECT Name, (SELECT DepartmentName FROM TestTable2 WHERE DepartmentID = TestTable1.DepartmentID) AS DepartmentName FROM TestTable1
     ```
   - **Error:** Syntax error due to unsupported subqueries in SELECT clause.

3. **Mathematical Expressions in SELECT Clause**
   - **Query:**
     ```sql
     SELECT Name, (Salary * 2) AS DoubleSalary FROM TestTable1
     SELECT Salary * 2 AS DoubleSalary FROM TestTable1
     ```
    - **Error:** Syntax error at 35: expected 'Integral', 'Real', 'RealScientific', 'Boolean', 'StringLiteral', 'DelimitedIdentifier', 'RegularIdentifier', 'EXTRACT', 'CHAR_LENGTH', 'MOD', 'COUNT', 'MIN', 'MAX', 'AVG', 'SUM', 'ROW_NUMBER', 'RANK' or 'DENSE_RANK'
    - **Analysis:** Mathematical expressions in the `SELECT` clause are not supported.

4. **Combining `*` with Multiple Derived Columns**
   - **Query:**
     ```sql
     SELECT *, Duration * 2 AS DoubleDuration, EventName || '_Processed' AS ModifiedEventName FROM TestTable4
     ```
   - **Error:** Syntax error at 33.
   - **Explanation:** Combining `*` with multiple derived columns using expressions (e.g., calculations or concatenation) is not supported.

5. **String Concatenation with || Operator**
   - **Query:**
     ```sql
     SELECT EventName || '_Processed' AS ModifiedEventName FROM TestTable4
     ```
   - **Error:** Syntax error at position 17.
   - **Analysis:** String concatenation using `||` is not supported.

6. **Explicit Column Selection with Derived Columns**
   - **Query:**
     ```sql
     SELECT ID, Timestamp, EventName || '_Processed' AS ModifiedEventName FROM TestTable4
     ```
   - **Error:** Syntax error at position 32.
   - **Explanation:** Concatenation using `||` in combination with explicit column selection is not supported.

7. **Using `CONCAT` Function Instead of `||`**
    - **Query:**
     ```sql
     SELECT CONCAT(EventName, '_Processed') AS ModifiedEventName FROM TestTable4
     ```
   - **Error:** Syntax error at position 13.
   - **Explanation:** The `CONCAT` function is not recognized or supported by Optix

8. **CASE Expression in SELECT Clause**
   - **Query:**
     ```sql
     SELECT Name, CASE WHEN Salary > 60000 THEN 'High' ELSE 'Low' END AS SalaryCategory FROM TestTable1
     ```
   - **Error:** Syntax error at position 18.
   - **Analysis:** `CASE` expressions are not supported in the `SELECT` clause.

### Aggregate Functions Testing

#### Supported

1. **Basic Aggregates**
   - **Queries:**
     ```sql
     SELECT COUNT(*) AS TotalCount FROM TestTable1
     SELECT AVG(Salary) AS AverageSalary FROM TestTable1
     SELECT SUM(Salary) AS TotalSalary FROM TestTable1
     SELECT MAX(Salary) AS HighestSalary FROM TestTable1
     SELECT MIN(Salary) AS LowestSalary FROM TestTable1
     ```
   - **Result:** Successfully executed queries for row counts, averages, sums, maximums, and minimums.
   - **Explanation:** Aggregate functions perform calculations on a set of values.

2. **Group Aggregates with Multiple Conditions**
   - **Query:**
     ```sql
     SELECT DepartmentID, COUNT(*) AS EmployeeCount, SUM(Salary) AS TotalSalary 
     FROM TestTable1 
     WHERE Salary > 50000 
     GROUP BY DepartmentID
     ```
     ```sql
     SELECT DepartmentID, AVG(Salary) AS AvgSalary
     FROM TestTable1
     GROUP BY DepartmentID
     ``` 
   - **Result:** Successfully grouped rows and applied multiple aggregate functions.
   - **Example Data:**
     | DepartmentID | EmployeeCount | TotalSalary |
     |--------------|---------------|-------------|
     | 101          | 3             | 210000      |
     | 102          | 2             | 105000      |

3. **Filter Aggregates with HAVING**
   - **Query:**
     ```sql
     SELECT DepartmentID, AVG(Salary) AS AvgSalary
     FROM TestTable1
     GROUP BY DepartmentID
     HAVING AVG(Salary) > 65000
     ```
   - **Result:** Filtered groups to include only those with an average salary greater than 65,000.
   - **Explanation:** HAVING applies filters to aggregated results.

4. **Combining `*` with Aggregate Functions**
   - **Query:**
     ```sql
     SELECT *, COUNT(*) OVER () AS TotalCount FROM TestTable4
     ```
   - **Result:** Successfully added a column displaying the total row count.
   - **Example Data:**
     | ID  | Timestamp           | EventName      | Duration | TotalCount |
     |-----|---------------------|----------------|----------|------------|
     | 1   | 1/1/2025 12:00 PM  | Maintenance    | 2.5      | 13         |
     | 2   | 1/1/2025 12:15 PM  | Adjustment     | 1.5      | 13         |

### Join Operations

#### Supported

1. **INNER JOIN**
   - **Query:**
     ```sql
     SELECT t1.Name, t2.DepartmentName 
     FROM TestTable1 AS t1 
     INNER JOIN TestTable2 AS t2 
     ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Result:** Successfully joined rows where DepartmentID matches between tables.
   - **Example Data:**
     | Name    | DepartmentName |
     |---------|----------------|
     | Alice   | Engineering    |
     | Bob     | Sales          |

2. **INNER JOIN Without Table Name Aliases**
   - **Query:**
     ```sql
     SELECT TestTable1.Name, TestTable2.DepartmentName 
     FROM TestTable1 
     INNER JOIN TestTable2 
     ON TestTable1.DepartmentID = TestTable2.DepartmentID
     ```
   - **Result:** Successfully retrieved rows combining `TestTable1` and `TestTable2` based on `DepartmentID`.
   - **Example Data:**
     | Name     | DepartmentName |
     |----------|----------------|
     | Alice    | Engineering    |
     | Bob      | Sales          |
     | Charlie  | Engineering    |

3. **Aggregates with INNER JOIN**
    - **Query:**
     ```sql
    SELECT t2.DepartmentName, COUNT(t1.ID) AS EmployeeCount, AVG(t1.Salary) AS AvgSalary 
    FROM TestTable1 AS t1 
    INNER JOIN TestTable2 AS t2 
    ON t1.DepartmentID = t2.DepartmentID 
    GROUP BY t2.DepartmentName
     ```
    - **Result:** Aggregated data for each department.
    - **Example Data:**
     | DepartmentName | EmployeeCount | AvgSalary |
     |----------------|---------------|-----------|
     | Engineering    | 3             | 67,000    |
     | HR             | 2             | 79,000    |
     | IT             | 2             | 64,000    |

4. **LEFT JOIN**
   - **Query:**
     ```sql
     SELECT t1.Name, t2.Location FROM TestTable1 AS t1 LEFT JOIN TestTable2 AS t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Result:** Successfully performed a `LEFT JOIN`, returning all rows from `TestTable1` and matching rows from `TestTable2`.
   - **Description:** Validates the `LEFT JOIN` functionality.

5. **LEFT JOIN with Filtering**
   - **Query:**
     ```sql
     SELECT t1.Name, t2.DepartmentName 
     FROM TestTable1 AS t1 
     LEFT JOIN TestTable2 AS t2 
     ON t1.DepartmentID = t2.DepartmentID
     WHERE t2.DepartmentName IS NOT NULL
     ```
   - **Result:** Successfully returned all rows from TestTable1 with matching DepartmentName from TestTable2.
   - **Example Data:**
     | Name    | DepartmentName |
     |---------|----------------|
     | Alice   | Engineering    |
     | Bob     | Sales          |

6. **Aggregates with LEFT JOIN**
   - **Query:**
     ```sql
     SELECT t2.DepartmentName, 
            COUNT(t1.ID) AS EmployeeCount, 
            AVG(t1.Salary) AS AvgSalary 
     FROM TestTable1 AS t1 
     LEFT JOIN TestTable2 AS t2 
         ON t1.DepartmentID = t2.DepartmentID 
     GROUP BY t2.DepartmentName
     ```
   - **Result:** Similar to the results of an INNER JOIN but includes unmatched rows from `TestTable1` if present.
   - **Explanation:** A LEFT JOIN returns all rows from the left table (`TestTable1`), along with matching rows from the right table (`TestTable2`). When no match is found, NULL values are included for columns from the right table.

7. **CROSS JOIN**
   - **Query:**
     ```sql
     SELECT t1.Name, t2.DepartmentName 
     FROM TestTable1 AS t1 
     CROSS JOIN TestTable2 AS t2
     ```
   - **Result:** Successfully performed a Cartesian product, combining every row in `TestTable1` with every row in `TestTable2`.
   - **Example Data:** (Partial)
     | Name  | DepartmentName |
     |-------|----------------|
     | Alice | Engineering    |
     | Alice | Sales          |
     | Bob   | Engineering    |
     | ...   | ...            |
   - **Explanation:** A CROSS JOIN produces a Cartesian product, where each row from the first table is paired with every row from the second table. This operation can result in a large dataset depending on the size of the input tables.


#### Unsupported

1. **RIGHT JOIN and FULL JOIN**
   - **Queries:**
     ```sql
     SELECT t1.Name, t2.DepartmentName 
     FROM TestTable1 AS t1 
     RIGHT JOIN TestTable2 AS t2 
     ON t1.DepartmentID = t2.DepartmentID

     SELECT t1.Name, t2.DepartmentName 
     FROM TestTable1 AS t1 
     FULL JOIN TestTable2 AS t2 
     ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Error:** FactoryTalk Optix only supports `INNER JOIN`, `LEFT JOIN`, and `CROSS JOIN`.
   - **Explanation:** The queries use `RIGHT JOIN` and `FULL JOIN` to include unmatched rows from one or both tables. However, these join types are not supported in FactoryTalk Optix.

2. **NATURAL JOIN**
   - **Query:**
     ```sql
     SELECT * FROM TestTable1 NATURAL JOIN TestTable2
     ```
   - **Error:** Syntax error at position 48.
   - **Analysis:** `NATURAL JOIN` is not supported; use explicit join conditions instead.

3. **FULL OUTER JOIN**
   - **Query:**
     ```sql
     SELECT t1.Name, t2.Location FROM TestTable1 AS t1 FULL OUTER JOIN TestTable2 AS t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Error:** SQLite supports only `INNER`, `LEFT`, and `CROSS` joins.

4. **UNION Queries**
   - **Queries:**
     ```sql
     SELECT Name FROM TestTable1 UNION SELECT DepartmentName FROM TestTable2;
     SELECT Name FROM TestTable1 UNION ALL SELECT DepartmentName FROM TestTable2;
     ```
   - **Error:** Syntax error due to unsupported `UNION` operations.

5. **UNION with Parentheses**
   - **Query:**
     ```sql
     (SELECT Name FROM TestTable1 UNION SELECT DepartmentName FROM TestTable2)
     ```
   - **Error:** Syntax error at position 0: Expected `SELECT, DELETE, or UPDATE`.
   - **Analysis:** Wrapping UNION queries in parentheses does not resolve the syntax error.

6. **UNION with Aggregations**
   - **Query:**
     ```sql
     SELECT DepartmentID, AVG(Salary) AS AvgSalary FROM TestTable1 GROUP BY DepartmentID UNION SELECT DepartmentID, SUM(Salary) AS TotalSalary FROM TestTable1 GROUP BY DepartmentID
     ```
   - **Error:** Syntax error at position 88.
   - **Analysis:** Even UNION queries combining aggregated results are not supported.

7. **UNION JOIN Example from Documentation**
   - **Query:**
     ```sql
     SELECT A.Label, A.Value FROM (SELECT Variable1 AS Label, AVG(Variable1) AS Value FROM Datalogger1) AS A UNION JOIN (SELECT Variable2 AS Label, AVG(Variable2) AS Value FROM Datalogger1 ORDER BY Label ASC) AS B ON A.Label = B.Label AND A.Value = B.Value
     ```
   - **Error:** Unable to prepare statement: (1) no such table: Datalogger1.
   - **Analysis:** The example provided in the documentation is not executable, even when adhering to its format.

8. **UNION with Nested Queries**
   - **Query:**
     ```sql
     SELECT * FROM (SELECT Name FROM TestTable1 UNION SELECT DepartmentName FROM TestTable2) AS UnionResult
     ```
   - **Error:** Syntax error at position 55: Expected `JOIN`.
   - **Analysis:** UNION operations nested in subqueries fail due to unsupported syntax.

#### Observations
- UNION operations, in all tested forms, are not supported in FactoryTalk Optix SQL.
- This includes basic UNION, UNION ALL, UNION with parentheses, and the UNION JOIN example from the documentation.
- Documentation examples referencing UNION functionality may not align with the actual SQL parser capabilities.
- Developers must consider alternative approaches, such as merging query results programmatically.


### Subquerys

#### Supported

1. **Subquery in FROM Clause**
   - **Query:**
     ```sql
     SELECT * 
     FROM (
         SELECT DepartmentID, AVG(Salary) AS AvgSalary 
         FROM TestTable1 
         GROUP BY DepartmentID
     ) AS SubQuery 
     WHERE AvgSalary > 65000
     ```
   - **Result:** Filtered results from the subquery.
   - **Example Data:**
     | DepartmentID | AvgSalary |
     |--------------|-----------|
     | 101          | 67,000    |
     | 103          | 79,000    |
   - **Explanation:** The subquery calculates the average salary grouped by department, and the outer query filters the results to include only departments where the average salary exceeds 65,000.

2. **Subquery with JOIN**
   - **Query:**
     ```sql
     SELECT t1.Name, SubQuery.AvgSalary 
     FROM TestTable1 AS t1 
     INNER JOIN (
         SELECT DepartmentID, AVG(Salary) AS AvgSalary 
         FROM TestTable1 
         GROUP BY DepartmentID
     ) AS SubQuery 
     ON t1.DepartmentID = SubQuery.DepartmentID
     ```
   - **Result:** Combined data from the subquery with the main query.
   - **Example Data:**
     | Name  | AvgSalary |
     |-------|-----------|
     | Alice | 67,000    |
     | Bob   | 55,500    |
   - **Explanation:** The subquery calculates the average salary for each department. The main query then joins this result with `TestTable1` to associate employee names with their respective department's average salary.

3. **Subquery in `WHERE` Clause for IN Condition**
   - **Query:**
     ```sql
     SELECT Name FROM TestTable1 WHERE Salary IN (SELECT Salary FROM TestTable1 WHERE DepartmentID = 101)
     ```
   - **Result:** Successfully executed.
   - **Output:**
     | Name     |
     |----------|
     | Alice    |
     | Charlie  |
     | Grace    |
   - **Description:** Subqueries in the `WHERE` clause with the `IN` operator are supported. This query correctly filtered names where `Salary` matches the subquery result.

4. **EXISTS Operator**
   - **Query:**
     ```sql
     SELECT Name FROM TestTable1 WHERE EXISTS (SELECT 1 FROM TestTable2 WHERE TestTable2.DepartmentID = TestTable1.DepartmentID AND TestTable2.Location = 'New York')
     ```
   - **Result:** Successfully executed.
   - **Output:**
     | Name     |
     |----------|
     | Alice    |
     | Charlie  |
     | Grace    |
   - **Description:** Queries with `EXISTS` are supported, correctly identifying employees in departments located in "New York."

5. **Subquery in `FROM` Clause**
   - **Query:**
     ```sql
     SELECT AVG(Salary) AS AvgSalary FROM (SELECT * FROM TestTable1 WHERE DepartmentID = 101) AS SubQuery
     ```
   - **Result:** Successfully executed.
   - **Output:**
     | AvgSalary |
     |-----------|
     | 67000     |
   - **Description:** Subqueries in the `FROM` clause are supported, enabling derived tables for aggregation.

6. **Multiple Subqueries with `IN`**:
   - **Query**:
     ```sql
     SELECT Name FROM TestTable1 WHERE DepartmentID IN (SELECT DepartmentID FROM TestTable2 WHERE Location = 'New York') AND Salary IN (SELECT Salary FROM TestTable1 WHERE DepartmentID = 101)
     ```
   - **Result**:
     | Name    |
     |---------|
     | Alice   |
     | Charlie |
     | Grace   |
   - **Description**: Successfully filtered employees belonging to departments in New York with salaries matching those in Department 101.

7. **Multiple Subqueries with `EXISTS`**:
   - **Query**:
     ```sql
     SELECT Name FROM TestTable1 WHERE EXISTS (SELECT 1 FROM TestTable2 WHERE TestTable1.DepartmentID = TestTable2.DepartmentID AND TestTable2.Location = 'Chicago') AND EXISTS (SELECT 1 FROM TestTable1 AS Sub WHERE Sub.Salary > 60000 AND Sub.DepartmentID = 101)
     ```
   - **Result**:
     | Name  |
     |-------|
     | Bob   |
     | Hank  |
   - **Description**: Successfully returned employees whose departments exist in Chicago and who have salaries greater than 60000 in Department 101.

8. **Mixed `IN` and `EXISTS` with Correlation**:
   - **Query**:
     ```sql
     SELECT Name FROM TestTable1 WHERE DepartmentID IN (SELECT DepartmentID FROM TestTable2 WHERE Location = 'Chicago') AND EXISTS (SELECT 1 FROM TestTable2 WHERE TestTable2.DepartmentID = TestTable1.DepartmentID AND TestTable2.Location = 'New York')
     ```
   - **Result**: No rows returned.
   - **Description**: No employees satisfied both conditions of being in Chicago and having correlated existence in New York.

9. **Subquery with Nested `IN` Clauses**:
   - **Query**:
     ```sql
     SELECT Name FROM TestTable1 WHERE DepartmentID IN (SELECT DepartmentID FROM TestTable2 WHERE Location IN (SELECT Location FROM TestTable2 WHERE DepartmentID = 101))
     ```
   - **Result**:
     | Name    |
     |---------|
     | Alice   |
     | Charlie |
     | Grace   |
   - **Description**: Successfully identified employees from departments linked through nested IN conditions.

10. **Subquery with Nested `EXISTS` Clauses**:
   - **Query**:
     ```sql
     SELECT Name FROM TestTable1 WHERE EXISTS (SELECT 1 FROM TestTable2 WHERE TestTable1.DepartmentID = TestTable2.DepartmentID AND EXISTS (SELECT 1 FROM TestTable1 AS Sub WHERE Sub.Salary > 70000))
     ```
   - **Result**:
     | Name    |
     |---------|
     | Alice   |
     | Bob     |
     | Charlie |
     | Diana   |
     | Eve     |
     | Frank   |
     | Grace   |
     | Hank    |
     | Ivy     |
     | Jack    |
   - **Description**: Successfully returned all employees whose departments exist and correlate with salaries greater than 70000.

11. **Deeper Nested Subquery with IN**:
   - **Query**:
     ```sql
     SELECT Name FROM TestTable1 WHERE DepartmentID IN (SELECT DepartmentID FROM TestTable2 WHERE Location IN (SELECT Location FROM TestTable2 WHERE Location IN (SELECT Location FROM TestTable2 WHERE DepartmentID = 101)))
     ```
   - **Result**:
     | Name    |
     |---------|
     | Alice   |
     | Charlie |
     | Grace   |
   - **Description**: Successfully identified employees through multiple nested `IN` conditions linked to DepartmentID and Location.

12. **Deeper Nested Subquery with EXISTS**:
   - **Query**:
     ```sql
     SELECT Name FROM TestTable1 WHERE EXISTS (SELECT 1 FROM TestTable2 WHERE TestTable1.DepartmentID = TestTable2.DepartmentID AND EXISTS (SELECT 1 FROM TestTable1 WHERE EXISTS (SELECT 1 FROM TestTable2 WHERE TestTable2.DepartmentID = TestTable1.DepartmentID)))
     ```
   - **Result**:
     | Name    |
     |---------|
     | Alice   |
     | Bob     |
     | Charlie |
     | Diana   |
     | Eve     |
     | Frank   |
     | Grace   |
     | Hank    |
     | Ivy     |
     | Jack    |
   - **Description**: Successfully returned all employees whose departments correlate through multiple nested `EXISTS` clauses.

13. **Subquery with Nested IN Clauses and Conditions**:
   - **Query**:
     ```sql
     SELECT Name FROM TestTable1 WHERE DepartmentID IN (SELECT DepartmentID FROM TestTable2 WHERE Location IN (SELECT Location FROM TestTable2 WHERE EXISTS (SELECT 1 FROM TestTable1 WHERE Salary > 70000)))
     ```
   - **Result**:
     | Name    |
     |---------|
     | Alice   |
     | Bob     |
     | Charlie |
     | Diana   |
     | Eve     |
     | Frank   |
     | Grace   |
     | Hank    |
     | Ivy     |
     | Jack    |
   - **Description**: Successfully linked employees based on nested `IN` and `EXISTS` conditions with salary filters.

14. **Subquery in JOIN Condition with Explicit Alias**:
   - **Query**:
     ```sql
     SELECT t1.Name, t2.DepartmentName FROM TestTable1 AS t1 INNER JOIN (SELECT * FROM TestTable2 WHERE Location = 'Chicago') AS t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Result**:
     | Name  | DepartmentName |
     |-------|----------------|
     | Bob   | Sales          |
     | Hank  | Sales          |
   - **Description**: Successfully returned employees whose departments match subqueries filtered by `Location = 'Chicago'`.

15. **Validate Joins with Nested Subqueries and Alias**:
   - **Query**:
     ```sql
     SELECT t1.Name FROM TestTable1 AS t1 INNER JOIN (SELECT DepartmentID FROM TestTable2 WHERE EXISTS (SELECT 1 FROM TestTable1 WHERE TestTable1.DepartmentID = TestTable2.DepartmentID)) AS t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Result**:
     | Name    |
     |---------|
     | Alice   |
     | Charlie |
     | Grace   |
     | Bob     |
     | Hank    |
     | Diana   |
     | Ivy     |
     | Eve     |
     | Jack    |
     | Frank   |
   - **Description**: Successfully returned employees through nested subqueries with explicit alias declarations.

16. **Combined Subqueries and Joins with Explicit Alias**:
   - **Query**:
     ```sql
     SELECT t1.Name, t2.DepartmentName FROM TestTable1 AS t1 LEFT JOIN (SELECT DepartmentID, DepartmentName FROM TestTable2 WHERE EXISTS (SELECT 1 FROM TestTable1 WHERE Salary > 60000)) AS t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Result**:
     | Name    | DepartmentName |
     |---------|----------------|
     | Alice   | Engineering    |
     | Bob     | Sales          |
     | Charlie | Engineering    |
     | Diana   | HR             |
     | Eve     | IT             |
     | Frank   | Marketing      |
     | Grace   | Engineering    |
     | Hank    | Sales          |
     | Ivy     | HR             |
     | Jack    | IT             |
   - **Description**: Successfully returned employees with matching department names from a left join using subqueries with `EXISTS`.

17. **Subqueries in FROM Clause**
   - **Query:**
     ```sql
     SELECT * FROM (SELECT * FROM TestTable4 WHERE Duration > 2) AS SubQuery
     ```
   - **Result:** Successfully returned all rows from the subquery where `Duration > 2`.
   - **Description:** Confirms the ability to use subqueries as a derived table in the `FROM` clause.

#### Unsupported

1. **Subqueries in `SELECT` Clause**
   - **Query:**
     ```sql
     SELECT Name, 
            (SELECT DepartmentName 
             FROM TestTable2 
             WHERE DepartmentID = t1.DepartmentID) AS Department 
     FROM TestTable1 AS t1
     ```
     ```sql
     SELECT *, (SELECT MAX(Duration) FROM TestTable4) AS MaxDuration FROM TestTable4
     ```
   - **Error:** Syntax error due to unsupported subqueries in the `SELECT` clause.
   - **Explanation:** The query attempts to use a subquery in the `SELECT` clause to dynamically retrieve department names based on `DepartmentID`. FactoryTalk Optix does not support subqueries within the `SELECT` clause, resulting in a syntax error.


2. **Subquery in `WHERE` Clause with Dynamic Reference**
   - **Query:**
     ```sql
     SELECT t1.Name, t2.DepartmentName 
     FROM TestTable1 AS t1 
     INNER JOIN TestTable2 AS t2 
     ON t1.DepartmentID = t2.DepartmentID 
     WHERE t1.Salary > (
         SELECT AVG(Salary) 
         FROM TestTable1 
         WHERE DepartmentID = t1.DepartmentID
     )
     ```
   - **Error:** Syntax error. Optix SQL does not support subqueries in the `WHERE` clause with dynamic references to outer query fields.
   - **Explanation:** This query attempts to use a subquery in the `WHERE` clause that references a field from the outer query (`t1.DepartmentID`). However, Optix SQL does not allow such dynamic references in subqueries, resulting in a syntax error.

3. **Subqueries in `WHERE` Clause with Aggregates**
   - **Queries:**
     ```sql
     SELECT t1.Name, t2.DepartmentName
     FROM TestTable1 AS t1
     INNER JOIN TestTable2 AS t2
     ON t1.DepartmentID = t2.DepartmentID
     WHERE t1.Salary > (SELECT AVG(Salary) FROM TestTable1 WHERE DepartmentID = 101)
     ```
     ```sql
     SELECT Name FROM TestTable1 WHERE Salary > (SELECT AVG(Salary) FROM TestTable1 WHERE DepartmentID = 102)
     ```
     ```sql
     SELECT Name FROM TestTable1 WHERE Salary > (SELECT MAX(Salary) FROM TestTable1 WHERE DepartmentID = 101)
     ```
   - **Errors:** Syntax error indicating unsupported subqueries with aggregate functions in the `WHERE` clause.
   - **Analysis:** Subqueries returning a scalar value (e.g., `AVG`, `MAX`) are not supported in `WHERE` conditions.

4. **Combining Multiple Subqueries in `WHERE` Clause**
   - **Query:**
     ```sql
     SELECT Name FROM TestTable1 WHERE Salary > (SELECT AVG(Salary) FROM TestTable1 WHERE DepartmentID = 101) AND Salary < (SELECT MAX(Salary) FROM TestTable1 WHERE DepartmentID = 102)
     ```
   - **Error:** Syntax error at the second subquery.

5. **Scalar Subquery in `SELECT` Clause**:
   - **Query**:
     ```sql
     SELECT Name, (SELECT AVG(Salary) FROM TestTable1 WHERE DepartmentID = 101) AS AvgSalary FROM TestTable1
     ```
   - **Error**: Syntax error at position 13.
   - **Analysis**: Scalar subqueries in the `SELECT` clause are not supported.

6. **Derived Columns with Scalar Subqueries**:
   - **Query**:
     ```sql
     SELECT Name, Salary - (SELECT AVG(Salary) FROM TestTable1 WHERE DepartmentID = 101) AS SalaryDifference FROM TestTable1
     ```
   - **Error**: Syntax error at position 20.
   - **Analysis**: Subtracting a scalar subquery result from a column is unsupported.

7. **Subquery with Complex Conditions in `SELECT` Clause**:
   - **Query**:
     ```sql
     SELECT Name, (SELECT DepartmentName FROM TestTable2 WHERE TestTable2.DepartmentID = TestTable1.DepartmentID AND TestTable2.Location IN ('New York', 'Chicago')) AS DeptName FROM TestTable1
     ```
   - **Error**: Syntax error at position 13.
   - **Analysis**: Complex subqueries combining conditions and `IN` in the `SELECT` clause are unsupported.

8. **Complex Joins with Subqueries Without Explicit Alias Declaration**:
   - **Query**:
     ```sql
     SELECT t1.Name, t2.DepartmentName FROM TestTable1 t1 INNER JOIN (SELECT * FROM TestTable2 WHERE Location = 'New York') t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Error**: Syntax error at position 50. Subqueries in the `ON` clause of a join are unsupported.

9. **Joins with Nested Subqueries Without Explicit Alias**:
   - **Query**:
     ```sql
     SELECT t1.Name FROM TestTable1 t1 INNER JOIN (SELECT DepartmentID FROM TestTable2 WHERE EXISTS (SELECT 1 FROM TestTable1 WHERE TestTable1.DepartmentID = TestTable2.DepartmentID)) t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Error**: Syntax error at position 31.
   - **Analysis**: Nested subqueries as part of `JOIN` logic are unsupported.

10. **Mixed Subqueries and Aggregation**:
   - **Query**:
     ```sql
     SELECT Name, Salary FROM TestTable1 WHERE EXISTS (SELECT 1 FROM TestTable2 WHERE TestTable1.DepartmentID = TestTable2.DepartmentID) AND Salary > (SELECT AVG(Salary) FROM TestTable1)
     ```
   - **Error**: Syntax error at position 145. Expected specific types or functions.
   - **Analysis**: Combining `EXISTS` with aggregate functions in filtering conditions is unsupported.

11. **Complex Filtering with EXISTS and Aggregation**:
   - **Query**:
     ```sql
     SELECT Name FROM TestTable1 WHERE EXISTS (SELECT 1 FROM TestTable2 WHERE TestTable1.DepartmentID = TestTable2.DepartmentID AND (SELECT MAX(Salary) FROM TestTable1 WHERE DepartmentID = 101) > 60000)
     ```
   - **Error**: Syntax error at position 128.
   - **Analysis**: Using aggregate subqueries inside an `EXISTS` clause condition is unsupported.

12. **Joining Multiple Subqueries  Without Explicit Alias**:
   - **Query**:
     ```sql
     SELECT t1.Name, t2.Location FROM (SELECT Name, DepartmentID FROM TestTable1) t1 INNER JOIN (SELECT DepartmentID, Location FROM TestTable2 WHERE Location = 'New York') t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Error**: Syntax error at position 79. Expected `AS` keyword.
   - **Analysis**: Subquery aliases in joins require explicit `AS` declarations.

13. **Combined Subqueries and Joins Without Explicit Alias**:
   - **Query**:
     ```sql
     SELECT t1.Name, t2.DepartmentName FROM TestTable1 t1 LEFT JOIN (SELECT DepartmentID, DepartmentName FROM TestTable2 WHERE EXISTS (SELECT 1 FROM TestTable1 WHERE Salary > 60000)) t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Error**: Syntax error at position 50.
   - **Analysis**: Combining `LEFT JOIN` with subqueries having `EXISTS` clauses is unsupported.

14. **Mixed Subqueries and Aggregation with Explicit Alias**:
   - **Query**:
     ```sql
     SELECT t1.Name, t1.Salary FROM TestTable1 AS t1 WHERE EXISTS (SELECT 1 FROM TestTable2 AS t2 WHERE t1.DepartmentID = t2.DepartmentID) AND t1.Salary > (SELECT AVG(Salary) FROM TestTable1 AS t3)
     ```
   - **Error**: Syntax error at position 150.
   - **Analysis**: Combining `EXISTS` with aggregate functions in filtering conditions is unsupported.

15. **Complex Filtering with EXISTS and Aggregation**:
   - **Query**:
     ```sql
     SELECT t1.Name FROM TestTable1 AS t1 WHERE EXISTS (SELECT 1 FROM TestTable2 AS t2 WHERE t1.DepartmentID = t2.DepartmentID AND (SELECT MAX(Salary) FROM TestTable1 AS t3 WHERE t3.DepartmentID = 101) > 60000)
     ```
   - **Error**: Syntax error at position 127.
   - **Analysis**: Using aggregate subqueries inside an `EXISTS` clause condition is unsupported.

16. **Joining Multiple Subqueries with Explicit Alias**:
   - **Query**:
     ```sql
     SELECT t1.Name, t2.Location FROM (SELECT Name, DepartmentID FROM TestTable1) AS t1 INNER JOIN (SELECT DepartmentID, Location FROM TestTable2 WHERE Location = 'New York') AS t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Error**: Unable to prepare statement 'SELECT t1.Name, t2.Location FROM (SELECT Name, DepartmentID FROM TestTable1) AS t1': (1) no such column: t2.Location.
   - **Analysis**: The subquery structure in the `INNER JOIN` caused an issue where the outer query could not properly reference the subquery’s columns.

#### Recommendations
   - **Decompose Complex Queries**:
   - Simplify queries with subqueries and joins by breaking them into intermediate steps.

   - **Avoid Combining Aggregation with EXISTS**:
   - Separate aggregation and filtering logic for better execution support.

   - **Validate Subquery Column Referencing**:
   - Ensure that all referenced columns in subqueries are properly exposed to outer queries.

### Window Functions and CTE Testing

#### Supported

1. **ROW_NUMBER Window Function**
   - **Query:**
     ```sql
     SELECT Name, DepartmentID, 
            ROW_NUMBER() OVER (PARTITION BY DepartmentID ORDER BY Salary DESC) AS Rank 
     FROM TestTable1
     ```
   - **Result:** Successfully calculated row number rankings within each department.
   - **Example Data:**
     | Name    | DepartmentID | Rank |
     |---------|--------------|------|
     | Grace   | 101          | 1    |
     | Charlie | 101          | 2    |
     | Alice   | 101          | 3    |
   - **Explanation:** The `ROW_NUMBER` function assigns a unique rank to each row within a partition (grouped by `DepartmentID` in this case), ordered by `Salary` in descending order.

2. **RANK Window Function**
   - **Query:**
     ```sql
     SELECT Name, DepartmentID, 
            RANK() OVER (PARTITION BY DepartmentID ORDER BY Salary DESC) AS Rank 
     FROM TestTable1
     ```
   - **Result:** Successfully ranked employees by salary within each department.
   - **Explanation:** The `RANK` function assigns a rank to each row within a partition, ordered by `Salary` in descending order. Tied rows receive the same rank, with subsequent ranks skipping as needed.

3. **SUM with PARTITION BY**
   - **Query:**
     ```sql
     SELECT Name, DepartmentID, 
            SUM(Salary) OVER (PARTITION BY DepartmentID) AS TotalDeptSalary 
     FROM TestTable1
     ```
   - **Result:** Successfully computed the total salary within each department.
   - **Example Data:**
     | Name    | DepartmentID | TotalDeptSalary |
     |---------|--------------|-----------------|
     | Alice   | 101          | 201,000         |
     | Charlie | 101          | 201,000         |
   - **Explanation:** The `SUM` function calculates the total `Salary` for all employees within each `DepartmentID`. This value is applied to every row in the respective partition.

4. **AVG with PARTITION BY**
   - **Query:**
     ```sql
     SELECT Name, DepartmentID, 
            AVG(Salary) OVER (PARTITION BY DepartmentID) AS AvgDeptSalary 
     FROM TestTable1
     ```
   - **Result:** Successfully computed the average salary within each department.
   - **Explanation:** The `AVG` function calculates the average `Salary` for all employees within each `DepartmentID`. This value is applied to every row in the respective partition.

5. **Global Ranking Using ROW_NUMBER**
   - **Query:**
     ```sql
     SELECT Name, Salary, DepartmentID, 
            ROW_NUMBER() OVER (ORDER BY Salary DESC) AS GlobalRank 
     FROM TestTable1
     ```
   - **Result:** Successfully calculated a global rank for employees based on salary.
   - **Example Data:**
     | Name  | Salary | DepartmentID | GlobalRank |
     |-------|--------|--------------|------------|
     | Diana | 80000  | 103          | 1          |
     | Ivy   | 78000  | 103          | 2          |
     | Grace | 71000  | 101          | 3          |
   - **Explanation:** The `ROW_NUMBER` function assigns a unique global rank to each employee, ordered by salary in descending order. No partitioning is applied, so the ranking is across all rows.

6. **Ranking Within Department: Lowest to Highest Salary**
   - **Query:**
     ```sql
     SELECT Name, DepartmentID, 
            ROW_NUMBER() OVER (PARTITION BY DepartmentID ORDER BY Salary ASC) AS RankLowToHigh 
     FROM TestTable1
     ```
   - **Result:** Successfully ranked employees within each department from lowest to highest salary.
   - **Explanation:** The `ROW_NUMBER` function generates rankings within each department (partitioned by `DepartmentID`), ordering employees by their salary in ascending order.

7. **ROW_NUMBER with Multiple Columns in ORDER BY**
   - **Query:**
     ```sql
     SELECT Name, DepartmentID, 
            ROW_NUMBER() OVER (PARTITION BY DepartmentID ORDER BY Salary DESC, Name ASC) AS Rank 
     FROM TestTable1
     ```
   - **Result:** Successfully ranked employees within departments, prioritizing salary descending and resolving ties alphabetically by name.
   - **Explanation:** The `ROW_NUMBER` function ranks employees within each department, ordered first by salary in descending order. If two employees have the same salary, the `Name` column (in ascending order) is used to break ties.

8. **Combining `*` with a Window Function**
   - **Query:**
     ```sql
     SELECT *, ROW_NUMBER() OVER (ORDER BY Timestamp) AS RowNum FROM TestTable4
     ```
   - **Result:** Successfully added a `RowNum` column using the `ROW_NUMBER` window function.
   - **Example Data:**
     | ID  | Timestamp           | EventName      | Duration | RowNum |
     |-----|---------------------|----------------|----------|--------|
     | 1   | 1/1/2025 12:00 PM   | Maintenance    | 2.5      | 1      |
     | 2   | 1/1/2025 12:15 PM   | Adjustment     | 1.5      | 2      |

#### Unsupported

1. **DENSE_RANK Window Function**
   - **Query:**
     ```sql
     SELECT Name, DepartmentID, 
            DENSE_RANK() OVER (PARTITION BY DepartmentID ORDER BY Salary DESC) AS DenseRank 
     FROM TestTable1
     ```
   - **Error:** Syntax error. Optix SQL does not support `DENSE_RANK`.

2. **CTEs (Common Table Expressions)**
   - **Queries:**
     ```sql
     WITH DepartmentAvg AS (...) 
     SELECT ...

     WITH HighEarningDepts AS (...) 
     SELECT ...
     ```
   - **Error:** Syntax error at the `WITH` clause. Optix SQL does not support CTEs.

---

#### Summary
- **Supported Window Functions:** 
  - `ROW_NUMBER`, `RANK`, `SUM`, and `AVG` with `PARTITION BY`.
  - These functions allow partitioning data into logical groups (e.g., by department) and performing calculations such as ranking, summation, or averaging within each group.
  - **ROW_NUMBER**:
  - Supports global rankings, partitioned rankings (e.g., by department), and multi-column sorting for flexible ranking logic.
- **Aggregate Window Functions**:
  - Functions like `SUM` and `AVG` with `PARTITION BY` are reliable for grouped calculations.

- **Unsupported Features:** 
  - **Advanced Window Functions**:
  - `DENSE_RANK` and similar functions are not available.
  - **Common Table Expressions (CTEs)**:
  - The `WITH` clause is not supported in the current Optix SQL implementation.


### DateTime Extraction

#### Supported

1. **Extract Year, Month, Day, Hour, Minute, and Second***
   - **Query:**
     ```sql
     SELECT ID, 
            EXTRACT(YEAR FROM Timestamp) AS Year, 
            EXTRACT(MONTH FROM Timestamp) AS Month, 
            EXTRACT(DAY FROM Timestamp) AS Day, 
            EXTRACT(HOUR FROM Timestamp) AS Hour, 
            EXTRACT(MINUTE FROM Timestamp) AS Minute, 
            EXTRACT(SECOND FROM Timestamp) AS Second 
     FROM TestTable4
     ```
   - **Result:** Successfully extracted and displayed multiple components of the `Timestamp` column in a single query.
    - **Example Data (Partial):**
     | ID  | Year | Month | Day | Hour | Minute | Second |
     |-----|------|-------|-----|------|--------|--------|
     | 1   | 2025 | 1     | 1   | 12   | 0      | 0      |
     | 12  | 2025 | 2     | 28  | 23   | 59     | 59     |
     | 13  | 2024 | 2     | 29  | 12   | 0      | 0      |
   - **Explanation:** The query uses the `EXTRACT` operator multiple times within the same `SELECT` statement to retrieve and display year, month, day, hour, minute, and second for each record in `TestTable4`.

2. **Average Duration for Each Hour**
   - **Query:**
     ```sql
     SELECT EXTRACT(HOUR FROM Timestamp) AS Hour, 
            AVG(Duration) AS AvgDuration 
     FROM TestTable4 
     GROUP BY Hour 
     ORDER BY Hour
     ```
   - **Result:** Successfully calculated the average duration for each hour.
   - **Example Data:**
     | Hour | AvgDuration |
     |------|-------------|
     | 00   | 2.8         |
     | 08   | 3.75        |
     | 12   | 2.5625      |
     | 23   | 2.93        |
   - **Explanation:** The `AVG` function calculates the average `Duration` for each hour extracted from the `Timestamp`. Grouping by `Hour` ensures the calculation is performed for each unique hour.

3. **Maximum Duration for Each Day**
   - **Query:**
     ```sql
     SELECT EXTRACT(DAY FROM Timestamp) AS Day, 
            MAX(Duration) AS MaxDuration 
     FROM TestTable4 
     GROUP BY Day 
     ORDER BY Day
     ```
   - **Result:** Successfully calculated the maximum duration per day.
   - **Example Data:**
     | Day | MaxDuration |
     |-----|-------------|
     | 01  | 2.5         |
     | 02  | 4.0         |
     | 31  | 3.0         |
   - **Explanation:** The `MAX` function identifies the maximum `Duration` for each day extracted from the `Timestamp`. Grouping by `Day` ensures the calculation is performed for each unique day.

4. **Event Count by Month**
   - **Query:**
     ```sql
     SELECT EXTRACT(MONTH FROM Timestamp) AS Month, 
            COUNT(*) AS EventCount 
     FROM TestTable4 
     GROUP BY Month 
     ORDER BY Month
     ```
   - **Result:** Successfully grouped and counted events by month.
   - **Example Data:**
     | Month | EventCount |
     |-------|------------|
     | 01    | 11         |
     | 02    | 2          |
   - **Explanation:** The `COUNT(*)` function calculates the total number of events for each month extracted from the `Timestamp`. Grouping by `Month` ensures events are counted for each unique month.

5. **Grouping and Summing by Hourly Periods**
   - **Query:**
     ```sql
     SELECT
         EXTRACT(YEAR FROM Timestamp) AS Year,
         EXTRACT(MONTH FROM Timestamp) AS Month,
         EXTRACT(DAY FROM Timestamp) AS Day,
         EXTRACT(HOUR FROM Timestamp) AS Hour,
         SUM(Duration) AS TotalDuration
     FROM TestTable4
     GROUP BY Year, Month, Day, Hour
     ORDER BY Year, Month, Day, Hour
     ```
   - **Result:** The query executed successfully, grouping by extracted components (`YEAR`, `MONTH`, `DAY`, `HOUR`) and summing the `Duration` values.
   - **Example Data:**
     | Year  | Month | Day | Hour | TotalDuration |
     |-------|-------|-----|------|---------------|
     | 2024  | 2     | 29  | 12   | 5.0           |
     | 2025  | 1     | 1   | 12   | 5.25          |
     | 2025  | 1     | 1   | 13   | 0.75          |
     | 2025  | 1     | 1   | 14   | 2.0           |
     | 2025  | 1     | 2   | 8    | 7.5           |
     | 2025  | 1     | 2   | 9    | 2.0           |
     | 2025  | 1     | 2   | 23   | 1.8           |
     | 2025  | 1     | 3   | 0    | 2.8           |
     | 2025  | 1     | 31  | 23   | 3.0           |
     | 2025  | 2     | 28  | 23   | 4.0           |
    - **Grouping:** 
    - The `EXTRACT` operator works seamlessly in `GROUP BY` clauses, enabling hourly aggregation of data based on extracted components (`YEAR`, `MONTH`, `DAY`, `HOUR`).
    - **Totals:**
    - The `SUM(Duration)` function correctly calculates the total `Duration` for each distinct hourly period, as shown in the aggregated results.

6. **Grouping by Hour and Summing Duration**
   - **Query:**
     ```sql
     SELECT EXTRACT(HOUR FROM Timestamp) AS Hour, 
            SUM(Duration) AS TotalDuration 
     FROM TestTable4 
     GROUP BY Hour
     ```
   - **Result:** Successfully grouped rows by hour and calculated the total duration for each hour.
   - **Example Data:**
     | Hour | TotalDuration |
     |------|---------------|
     | 00   | 2.8           |
     | 08   | 7.5           |
     | 12   | 10.25         |
   - **Explanation:** The `SUM` function aggregates the `Duration` values for each unique hour extracted from the `Timestamp`. Grouping by `Hour` ensures the durations are summed for each specific hour of the day.

7. **Grouping by Full Hour Periods**
   - **Query:**
     ```sql
     SELECT Year, Month, Day, Hour, 
            SUM(Duration) AS TotalDuration
     FROM (
         SELECT 
             EXTRACT(YEAR FROM Timestamp) AS Year,
             EXTRACT(MONTH FROM Timestamp) AS Month,
             EXTRACT(DAY FROM Timestamp) AS Day,
             EXTRACT(HOUR FROM Timestamp) AS Hour,
             Duration
         FROM TestTable4
     ) AS SubQuery
     GROUP BY Year, Month, Day, Hour
     ORDER BY Year, Month, Day, Hour
     ```
   - **Outcome:**
     - **Successful Execution:** The query executed successfully, grouping rows by unique hourly periods defined by `Year`, `Month`, `Day`, and `Hour`.
   - **Result Data:**
     | Year | Month | Day | Hour | TotalDuration |
     |------|-------|-----|------|---------------|
     | 2024 | 2     | 29  | 12   | 5.0           |
     | 2025 | 1     | 1   | 12   | 5.25          |
     | 2025 | 1     | 1   | 13   | 0.75          |
     | 2025 | 1     | 1   | 14   | 2.0           |
     | 2025 | 1     | 2   | 8    | 7.5           |
     | 2025 | 1     | 2   | 9    | 2.0           |
     | 2025 | 1     | 2   | 23   | 1.8           |
     | 2025 | 1     | 3   | 0    | 2.8           |
     | 2025 | 1     | 31  | 23   | 3.0           |
     | 2025 | 2     | 28  | 23   | 4.0           |
   - **Analysis**
      **Accurate Grouping:**
      - Each combination of `Year`, `Month`, `Day`, and `Hour` uniquely represents a distinct hourly period.
      - The `SUM(Duration)` function aggregates values correctly within these periods, ensuring no durations are combined across different days or months.
      **Practicality:**
      - This method is ideal for time-based data analysis where precise hourly grouping is required, such as monitoring hourly trends or aggregating usage data by time period.

8. **Verifying Grouping and Ordering by Extracted Datetime Data in a Subquery with Non-Selected Columns**
   - **Query:**
     ```sql
     SELECT EventName, 
            SUM(Duration) AS TotalDuration
     FROM (
         SELECT 
             EXTRACT(YEAR FROM Timestamp) AS Year,
             EXTRACT(MONTH FROM Timestamp) AS Month,
             EXTRACT(DAY FROM Timestamp) AS Day,
             EXTRACT(HOUR FROM Timestamp) AS Hour,
             Duration,
             EventName
         FROM TestTable4
     ) AS SubQuery
     GROUP BY Year, Month, Day, Hour
     ORDER BY Year, Month, Day, Hour
     ```
   - **Result:**
     | EventName            | TotalDuration |
     |----------------------|---------------|
     | Leap Year Event      | 5.0           |
     | Maintenance          | 5.25          |
     | Calibration          | 0.75          |
     | Testing              | 2.0           |
     | Deployment           | 7.5           |
     | Review               | 2.0           |
     | Audit                | 1.8           |
     | Troubleshooting      | 2.8           |
     | End-of-Month Review  | 3.0           |
     | Leap Year Prep       | 4.0           |
   - **Analysis**
      **EXTRACT Functionality with Subqueries:**
      - Successfully extracted datetime components (`Year`, `Month`, `Day`, `Hour`) in the subquery.
      - Verified that grouping and ordering by these extracted components is possible, even when they are not included in the final `SELECT` clause of the main query.
      - Non-datetime columns (e.g., `EventName`) can also be included in the subquery and used in the final grouping if explicitly selected.
      **Grouping Behavior:**
      - Grouping and ordering requirements were successfully met by ensuring all relevant columns (`EventName`, `Year`, `Month`, `Day`, `Hour`) were included in the `GROUP BY` clause.
      - This approach allows for combining datetime-based grouping with additional categorical data such as `EventName`, providing flexibility for complex analysis.
      **Practical Applications:**
      - This method is ideal for scenarios requiring detailed analysis of time-based events alongside categorical data, such as aggregating event durations by specific time periods.

9. **Grouping with Subquery and Extracted Values**
   - **Query:**
     ```sql
     SELECT Hour, 
            SUM(Duration) AS TotalDuration
     FROM (
         SELECT EXTRACT(HOUR FROM Timestamp) AS Hour, 
                Duration
         FROM TestTable4
     ) AS SubQuery
     GROUP BY Hour
     ORDER BY Hour
     ```
   - **Outcome:**
     - **Successful Execution:** The query executed successfully, grouping by the `Hour` extracted from the `Timestamp` column and aggregating `Duration`.
   - **Result Data:**
     | Hour | TotalDuration |
     |------|---------------|
     | 00   | 2.8           |
     | 08   | 7.5           |
     | 09   | 2.0           |
     | 12   | 10.25         |
     | 13   | 0.75          |
     | 14   | 2.0           |
     | 23   | 8.8           |
    **Analysis**
      **Functionality of Subqueries:**
      - Using a subquery to extract and alias components of the `Timestamp` column resolves the limitations of directly using `EXTRACT` in the `GROUP BY` clause.
      **Accurate Aggregation:**
      - The `SUM(Duration)` function correctly aggregates the `Duration` values for each extracted `Hour`, ensuring accurate results.
      **Scalability:**
      - This approach is flexible and allows for more complex grouping or filtering logic by extending the subquery with additional calculations or conditions.

10. **Filtering by `EXTRACT` in `WHERE` Clause (Year)**
   - **Query:**
     ```sql
     SELECT EXTRACT(HOUR FROM Timestamp) AS Hour, SUM(Duration) AS TotalDuration
     FROM TestTable4
     WHERE EXTRACT(YEAR FROM Timestamp) = '2025'
     GROUP BY Hour
     ORDER BY Hour
     ```
   - **Result:**
     | Hour | TotalDuration |
     |------|---------------|
     | 00   | 2.8           |
     | 08   | 7.5           |
     | 09   | 2.0           |
     | 12   | 5.25          |
     | 13   | 0.75          |
     | 14   | 2.0           |
     | 23   | 8.8           |
   - **Description:** Successfully filtered data by year using `EXTRACT(YEAR FROM Timestamp)` with a string comparison and grouped by hour.

11. **Filtering by `EXTRACT` in `WHERE` Clause (Month)**
   - **Query:**
     ```sql
     SELECT EXTRACT(DAY FROM Timestamp) AS Day, AVG(Duration) AS AvgDuration
     FROM TestTable4
     WHERE EXTRACT(MONTH FROM Timestamp) = '01'
     GROUP BY Day
     ORDER BY Day
     ```
   - **Result:**
     | Day | AvgDuration |
     |-----|-------------|
     | 01  | 1.6         |
     | 02  | 2.825       |
     | 03  | 2.8         |
     | 31  | 3.0         |
   - **Description:** Successfully filtered data by month using `EXTRACT(MONTH FROM Timestamp)` with a string comparison and grouped by day.

#### Unsupported

1. **Stripping Minutes and Seconds for Hourly Period Aggregation**
   - **Query:**
     ```sql
     SELECT SUBSTR(Timestamp, 0, 14) || ':00:00' AS HourlyPeriod, 
            SUM(Duration) AS TotalDuration 
     FROM TestTable4 
     GROUP BY HourlyPeriod;
     ```
   - **Error:** Syntax error. The `SUBSTR` function combined with string manipulation to strip minutes and seconds is not supported in `GROUP BY`.
   - **Explanation:** This query attempts to use `SUBSTR` to extract the first 14 characters of the `Timestamp` and append `:00:00` to represent hourly periods. However, string manipulation like this is not supported in the `GROUP BY` clause in the current SQL implementation.

2. **Using EXTRACT Directly in GROUP BY and SELECT**
   - **Queries:**
     ```sql
     SELECT SUM(Duration) AS TotalDuration 
     FROM TestTable4 
     GROUP BY EXTRACT(HOUR FROM Timestamp) 
     ORDER BY EXTRACT(HOUR FROM Timestamp)
     ```

     ```sql
     SELECT EXTRACT(HOUR FROM Timestamp), COUNT(*) 
     FROM TestTable4 
     GROUP BY EXTRACT(HOUR FROM Timestamp) 
     ORDER BY EXTRACT(HOUR FROM Timestamp)
     ```
   - **Error:** Syntax error: expected 'RegularIdentifier' or 'DelimitedIdentifier'
   - **Analysis:** This indicates that `EXTRACT` cannot be used directly in the `GROUP BY` or `SELECT` clauses without aliasing or preprocessing.

3. **Using `DISTINCT` with `EXTRACT`**
   - **Query:**
     ```sql
     SELECT DISTINCT EXTRACT(YEAR FROM Timestamp) AS Year
     FROM TestTable4
     ```
   - **Error:** Syntax error at position 16.
   - **Analysis:** `DISTINCT` cannot be applied directly to `EXTRACT` results. Use a subquery to achieve similar functionality.

4. **`EXTRACT` in `GROUP BY` with `CAST` to String**
   - **Query:**
     ```sql
     SELECT EXTRACT(YEAR FROM Timestamp) AS Year, COUNT(*) AS EntryCount
     FROM TestTable4
     GROUP BY CAST(EXTRACT(YEAR FROM Timestamp) AS TEXT)
     ORDER BY Year
     ```
   - **Error:** Syntax error at position 127.
   - **Analysis:** Using `CAST` to convert extracted values into strings for grouping is not supported in FactoryTalk Optix SQL. This limitation restricts flexibility in certain query designs.

#### Anomalies

1. **Filtering with `EXTRACT` in `WHERE` Clause**
   - **Query:**
     ```sql
     SELECT EXTRACT(HOUR FROM Timestamp) AS Hour, SUM(Duration) AS TotalDuration
     FROM TestTable4
     WHERE EXTRACT(YEAR FROM Timestamp) = 2025
     GROUP BY Hour
     ORDER BY Hour
     ```
   - **Result:** No rows returned despite valid data for `Year = 2025` in the table.
   - **Analysis:**
     - Filtering by `EXTRACT` without string comparison may fail due to data type mismatches or a bug in Optix SQL.
     - This issue suggests that `EXTRACT` results might not align with expected numeric comparisons, impacting filtering reliability.

2. **Filtering While Grouping by Full Hour Periods**
   - **Query:**
     ```sql
     SELECT Year, Month, Day, Hour, SUM(Duration) AS TotalDuration
     FROM (
         SELECT EXTRACT(YEAR FROM Timestamp) AS Year,
                EXTRACT(MONTH FROM Timestamp) AS Month,
                EXTRACT(DAY FROM Timestamp) AS Day,
                EXTRACT(HOUR FROM Timestamp) AS Hour,
                Duration
         FROM TestTable4
     ) AS SubQuery
     WHERE Year = 2025 AND Month = 1
     GROUP BY Year, Month, Day, Hour
     ORDER BY Year, Month, Day, Hour
     ```
   - **Outcome:** Executed successfully but returned no rows, even though valid data exists for `Year = 2025` and `Month = 1`.
   - **Analysis:**
     - Possible causes include data type mismatches or preprocessing anomalies.
     - This behavior reinforces the inconsistency of `EXTRACT` in filtering scenarios.

#### Recommendations

- **Reliable Uses:**
  - Filtering and grouping with `EXTRACT` are reliable when string comparisons are explicitly used (e.g., `EXTRACT(YEAR FROM Timestamp) = '2025'`).
  - Grouping by extracted components in `SELECT` and `GROUP BY` clauses works effectively.

- **Known Issues:**
  - `EXTRACT` results behave as strings, requiring explicit string comparisons in filters.
  - Direct numeric comparisons with `EXTRACT` may fail or produce inconsistent results.
  - Casting extracted values (e.g., `CAST(EXTRACT(...) AS TEXT)`) is unsupported.

- **Recommendations:**
  - Avoid numeric comparisons in `WHERE` clauses when filtering by `EXTRACT`. Use string-based conditions instead.
  - Preprocess data externally if transformations or string casting are essential.


### Temporary Tables

#### Supported

1. **Temporary Tables with Quoted Names**
   - **Query:**
     ```sql
     CREATE TEMPORARY TABLE "##TempTable" AS SELECT DepartmentID, AVG(Salary) AS AvgSalary FROM TestTable1 GROUP BY DepartmentID
     ```
   - **Result:** Successfully created a temporary table using double quotes around the name.
   - **Description:** Temporary tables can be created with quoted names and are fully accessible for querying and joins.

2. **Accessing Temporary Tables**
   - **Query:**
     ```sql
     SELECT * FROM "##TempTable"
     ```
   - **Result:** Successfully retrieved data from the temporary table.
   - **Example Data:**
     | DepartmentID | AvgSalary |
     |--------------|-----------|
     | 101          | 67000     |
     | 102          | 55500     |
     | 103          | 79000     |
     | 104          | 64000     |
     | 105          | 62000     |
   - **Description:** Data in the temporary table is accessible and accurately reflects the aggregation performed during creation.

3. **Joining Temporary Tables**
   - **Query:**
     ```sql
     SELECT t1.Name, t2.AvgSalary FROM TestTable1 AS t1 INNER JOIN "##TempTable" AS t2 ON t1.DepartmentID = t2.DepartmentID
     ```
   - **Result:** Successfully performed a join between `TestTable1` and the temporary table.
   - **Example Data:**
     | Name    | AvgSalary |
     |---------|-----------|
     | Alice   | 67000     |
     | Bob     | 55500     |
     | Charlie | 67000     |
     | Diana   | 79000     |
     | Eve     | 64000     |
   - **Description:** Demonstrates that temporary tables can participate in joins effectively.

4. **Dropping Temporary Tables**
   - **Query:**
     ```sql
     DROP TABLE "##TempTable"
     ```
   - **Result:** Successfully dropped the temporary table.
   - **Description:** Temporary tables can be removed after their use, preventing residual data issues.

5. **Filtered Temporary Tables**
   - **Query:**
     ```sql
     CREATE TEMPORARY TABLE "##FilteredTemp" AS SELECT ID, Name, Salary FROM TestTable1 WHERE Salary > 60000
     ```
   - **Result:** Successfully created a filtered temporary table.
   - **Description:** Temporary tables can be created with filtered data subsets for specific use cases.

6. **Accessing Filtered Temporary Tables**
   - **Query:**
     ```sql
     SELECT * FROM "##FilteredTemp"
     ```
   - **Result:** Successfully retrieved filtered data.
   - **Example Data:**
     | ID | Name    | Salary |
     |----|---------|--------|
     | 3  | Charlie | 70000  |
     | 4  | Diana   | 80000  |
     | 5  | Eve     | 65000  |
     | 7  | Grace   | 71000  |
   - **Description:** Data in the filtered temporary table accurately matches the `WHERE` clause criteria used during creation.

7. **Aggregations on Temporary Tables**
   - **Query:**
     ```sql
     SELECT COUNT(*) AS HighSalaryCount FROM "##FilteredTemp"
     ```
   - **Result:** Successfully counted rows in the filtered temporary table.
   - **Example Data:**
     | HighSalaryCount |
     |-----------------|
     | 7               |
   - **Description:** Aggregations on temporary tables work as expected.

#### Unsupported

1. **Temporary Table Creation Without ## as Prefix**
   - **Query:**
     ```sql
     CREATE TEMPORARY TABLE TempTable AS SELECT DepartmentID, AVG(Salary) AS AvgSalary FROM TestTable1 GROUP BY DepartmentID
     ```
   - **Error:** Syntax error; temporary tables must start with `##`.

2. **Standard Temporary Tables with `##` Prefix**
   - **Query:**
     ```sql
     CREATE TEMPORARY TABLE ##TempTable AS SELECT DepartmentID, AVG(Salary) AS AvgSalary FROM TestTable1 GROUP BY DepartmentID
     ```
   - **Error:** Syntax error at position 23.
   - **Analysis:** Temporary tables with the `##` prefix are not supported when not enclosed in double quotes.

3. **Temporary Tables with Parentheses in SELECT Clause**
   - **Query:**
     ```sql
     CREATE TEMPORARY TABLE ##TempTable AS (SELECT DepartmentID, AVG(Salary) AS AvgSalary FROM TestTable1 GROUP BY DepartmentID)
     ```
   - **Error:** Syntax error at position 23.
   - **Analysis:** Parentheses around the `SELECT` statement do not resolve the issue with creating temporary tables.

4. **Temporary Tables with Explicit Column Definitions**
   - **Query:**
     ```sql
     CREATE TEMPORARY TABLE ##TempTable (DepartmentID INTEGER, AvgSalary REAL) AS SELECT DepartmentID, AVG(Salary) AS AvgSalary FROM TestTable1 GROUP BY DepartmentID
     ```
   - **Error:** Syntax error at position 23.
   - **Analysis:** Including explicit column definitions in `CREATE TEMPORARY TABLE` syntax is not supported.

5. **Temporary Tables Without AS Clause**
   - **Query:**
     ```sql
     CREATE TEMPORARY TABLE ##TempTable (DepartmentID INTEGER, AvgSalary REAL)
     ```
   - **Error:** Syntax error at position 23.
   - **Analysis:** Attempting to define temporary tables without an `AS` clause is unsupported.

6. **TEMP Modifier Instead of TEMPORARY**
   - **Query:**
     ```sql
     CREATE TEMP TABLE ##TempTable AS SELECT DepartmentID, AVG(Salary) AS AvgSalary FROM TestTable1 GROUP BY DepartmentID
     ```
   - **Error:** Syntax error at position 7: expected 'TABLE', 'INDEX', 'TEMPORARY', or 'UNIQUE'.
   - **Analysis:** `TEMP` modifier is not recognized as a valid keyword for creating temporary tables.

7. **Temporary Tables Without `##` Prefix**
   - **Query:**
     ```sql
     CREATE TEMPORARY TABLE TempTable AS SELECT DepartmentID, AVG(Salary) AS AvgSalary FROM TestTable1 GROUP BY DepartmentID
     ```
   - **Error:** Syntax error at position 33: the name of temporary table must start with `##`.
   - **Analysis:** Temporary tables must have a `##` prefix, but the standard implementation does not support them without quotes.

8. **Entire Statement Wrapped in Parentheses**
   - **Query:**
     ```sql
     (CREATE TEMPORARY TABLE ##TempTable AS SELECT DepartmentID, AVG(Salary) AS AvgSalary FROM TestTable1 GROUP BY DepartmentID)
     ```
   - **Error:** Syntax error at position 0: expected `SELECT`, `DELETE`, or `UPDATE`.
   - **Analysis:** Wrapping the `CREATE TEMPORARY TABLE` statement in parentheses is not supported.

9. **Temporary Tables with Minimal SELECT Clause**
   - **Query:**
     ```sql
     CREATE TEMPORARY TABLE ##TempTable AS SELECT DepartmentID FROM TestTable1
     ```
   - **Error:** Syntax error at position 23.
   - **Analysis:** Simplifying the `SELECT` clause does not enable the creation of temporary tables with the `##` prefix.

10. **Updating Temporary Tables**
   - **Query:**
     ```sql
     UPDATE "##FilteredTemp" SET Salary = Salary * 1.1 WHERE ID = 3
     ```
   - **Error:** Syntax error at position 44.
   - **Analysis:** Updates to temporary tables are not supported, limiting their utility for iterative data manipulations.

#### Observations
- Temporary tables with the `##` prefix can be created and used effectively with double-quoted names.
- These tables support querying, joining, and aggregations but do not allow updates.
- Dropping temporary tables works as expected, ensuring proper cleanup of resources.



