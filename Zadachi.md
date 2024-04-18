## 175
```sql
SELECT firstName as firstname, lastName as lastname, city, state FROM 
Person
LEFT JOIN Address ON Person.personID = Address.personID;
```
## 176
```sql
SELECT MAX(salary) as SecondHighestSalary
FROM Employee
WHERE salary < (SELECT MAX(salary)FROM Employee);
```
## 177
```sql

CREATE OR REPLACE FUNCTION NthHighestSalary(N INT) RETURNS TABLE (Salary INT) AS $$
BEGIN
  RETURN QUERY (
    -- Write your PostgreSQL query statement below.
    SELECT DISTINCT tab.salary  as "getNthHighestSalary"
    FROM (
        SELECT 
            Employee.salary, 
            DENSE_RANK() OVER(ORDER BY Employee.salary DESC) AS rank 
        FROM Employee
  ) AS tab
  WHERE tab.rank = N
  );
END;
$$ LANGUAGE plpgsql;
```
## 178
```sql
WITH RankedScores AS (
    SELECT score,
            DENSE_RANK() OVER(ORDER BY score DESC) AS rank
            FROM Scores
)
SELECT score, rank
FROM RankedScores
ORDER BY score DESC;
```
## 181
```sql
SELECT name AS "Employee" FROM Employee e
WHERE (e.managerId IS NOT NULL) 
AND (e.salary > (SELECT salary FROM Employee WHERE Employee.id = e.managerId))
```
## 182
```sql
SELECT email 
FROM Person
GROUP BY email
HAVING COUNT(email) > 1;
```

## 570
```sql
SELECT e1.name
FROM Employee AS e1
INNER JOIN Employee AS e2
ON e1.id = e2.managerId
GROUP BY e2.managerId, e1.name
HAVING COUNT(e2.managerId) >= 5
```

## 1045
```sql
SELECT customer_id 
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(*) FROM Product)
```

## 1070
```sql
SELECT product_id, year as first_year, quantity, price
FROM Sales s
WHERE (product_id, year) IN
    (SELECT product_id, MIN(year)
    FROM Sales
    GROUP BY product_id)
```
