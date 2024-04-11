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
