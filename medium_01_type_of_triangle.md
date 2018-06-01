/*
2018.3.19 MySQL
Type of Triangle (https://www.hackerrank.com/challenges/what-type-of-triangle/problem)
세 변의 길이로 삼각형 타입 판별하기
*/


SELECT
  CASE
    WHEN (a + b) > c AND (b + c) > a AND (a + c) > b THEN  -- 삼각형인지 여부 판별
        CASE WHEN a = b AND b = c THEN 'Equilateral'  -- 정삼각형
             WHEN a = b OR b = c OR c = a THEN 'Isosceles'  -- 이등변삼각형
             ELSE 'Scalene'  -- 그냥 삼각형
        END
    ELSE 'Not A Triangle'  -- 삼각형이 아님
  END AS type
FROM triangles;


# 아래 구문은 제대로 작동하지 않음. 해당 값으로 판별하지 못함.
WHEN a=b=c THEN 'Equilateral'
