# SQL Practice Questions

## Problem 1: Triangle Classification

### Question:
Write a query to classify each record in the `TRIANGLES` table as one of the following based on the lengths of its sides: **Equilateral**, **Isosceles**, **Scalene**, or **Not A Triangle**.

### Explanation:
In this problem, you're asked to classify triangles based on the lengths of their sides stored in the `TRIANGLES` table. The task is to determine if a given set of side lengths forms one of the following types of triangles:

- **Equilateral**: All three sides are equal (e.g., A = B = C).
- **Isosceles**: Two sides are equal (e.g., A = B or B = C or A = C).
- **Scalene**: All three sides are different (e.g., A != B, B != C, A != C).
- **Not A Triangle**: The given sides do not form a valid triangle. For a set of side lengths to form a triangle, the triangle inequality theorem must hold, which states that the sum of the lengths of any two sides must be greater than the third side:
  - A + B > C
  - A + C > B
  - B + C > A

Question Link : [Hackerank](https://www.hackerrank.com/challenges/what-type-of-triangle/problem?isFullScreen=true)

### SQL Query:

```sql
SELECT
    CASE    
        WHEN A + B <= C OR A + C <= B OR B + C <= A THEN 'Not A Triangle'  
        WHEN A = B AND B = C THEN 'Equilateral'  
        WHEN A = B OR B = C OR A = C THEN 'Isosceles'  
        ELSE 'Scalene'  
    END AS Triangle
FROM TRIANGLES;
```

## Problem 2: Occupation Pivot

### Question:
Write a query to pivot the `OCCUPATIONS` table, so that each occupation (Doctor, Professor, Singer, Actor) has a separate column with the corresponding names listed alphabetically under each occupation. If an occupation has fewer names, fill in the missing rows with `NULL`.

### Explanation:
This problem asks you to pivot data from the `OCCUPATIONS` table, where each record represents a person's name and their occupation. The goal is to transform this data such that:

- Each occupation (Doctor, Professor, Singer, Actor) becomes a separate column.
- Under each occupation column, you should list the names of people who have that occupation, sorted alphabetically.
- If any occupation does not have a person in a particular row (i.e., fewer people in one occupation than another), you should fill those gaps with `NULL` values.

The pivoting process involves grouping the data by occupation and then distributing the names across the corresponding columns, ensuring proper sorting and handling of `NULL` values where there are fewer people in an occupation.

Question Link : [Hackerank](https://www.hackerrank.com/challenges/occupations/problem?isFullScreen=true)

### SQL Query:

```sql
WITH cte AS (
    SELECT 
        Name, 
        Occupation,
        row_number() OVER (PARTITION BY Occupation ORDER BY Name ASC) AS row_id
    FROM OCCUPATIONS
)
SELECT 
    MAX(CASE WHEN Occupation = 'Doctor' THEN Name END) AS Doctor,
    MAX(CASE WHEN Occupation = 'Professor' THEN Name END) AS Professor,
    MAX(CASE WHEN Occupation = 'Singer' THEN Name END) AS Singer,
    MAX(CASE WHEN Occupation = 'Actor' THEN Name END) AS Actor
FROM cte
GROUP BY row_id
ORDER BY row_id;
```
