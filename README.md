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

