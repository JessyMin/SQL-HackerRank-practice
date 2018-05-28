2018.3.20 MySQL
https://www.hackerrank.com/challenges/earnings-of-employees/problem
가장 높은 연봉과 이에 해당되는 직원 수 구하기

<br>

#### Ver.2.0
```sql
SELECT CONCAT(MAX(months * salary), ' ', COUNT(name))
FROM employee
WHERE (months * salary)
  = (SELECT MAX(months * salary) FROM employee);
```

<br>

#### Ver.1.0
```sql
SELECT CONCAT(MAX(a.total_earning), '  ', COUNT(a.name))
FROM (
  SELECT name, (months * salary) AS total_earning
  FROM employee
  WHERE (months * salary) = (SELECT MAX(months * salary) FROM employee)
  ) a;
```


__Lesson & Learned__
SELECT 내에 집계값이 오면 자동으로 full-group-by를 하게 되므로, 다른 값도 모두 집계값이어야 함.
