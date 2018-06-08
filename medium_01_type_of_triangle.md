
2018.3.19 MySQL
### Type of Triangle
__세 변의 길이로 삼각형 타입 판별하기__
https://www.hackerrank.com/challenges/what-type-of-triangle/problem



```sql
SELECT
  -- 삼각형인지 여부 판별
  CASE WHEN (a + b) > c
        AND (b + c) > a
        AND (a + c) > b
    -- 삼각형인 경우
    THEN
      CASE
        -- 정삼각형 판별
        WHEN a = b AND b = c
          THEN 'Equilateral'  
        -- 이등변삼각형 판별
        WHEN a = b OR b = c OR c = a
          THEN 'Isosceles'
        -- 그냥 삼각형
        ELSE 'Scalene'
      END
    -- 삼각형이 아닌 경우
    ELSE 'Not A Triangle'
  END AS type
FROM triangles;
```

아래 구문은 제대로 작동하지 않음. 해당 값으로 판별하지 못함.
```sql
(X) WHEN a=b=c THEN 'Equilateral'
(O) WHEN a=b AND b=c THEN 'Equilaternal'
```
