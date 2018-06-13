2018.3.21 MySQL
### Placements
https://www.hackerrank.com/challenges/placements/problem
친구 연봉이 본인보다 높은 사람 추출하기



#### Ver.2.0
```sql
SELECT s.name
FROM (
      SELECT
        f.id
        , pf.salary AS friend_salary
      FROM friends f
      -- 본인의 연봉 join
      JOIN packages p
        ON f.id = p.id  
      -- 친구의 연봉 join  
      JOIN packages pf
        ON f.friend_id = pf.id  
      -- 친구 연봉이 본인보다 높은 사람  
      WHERE p.salary < pf.salary  
      ) AS list
-- 이름은 가장 나중에 join      
JOIN students s
	ON s.id = list.id
ORDER BY list.friend_salary
;
```

#### Ver 1.0
개선점 : 값을 최대한 줄인 뒤 JOIN하는 원칙을 지키지 않았음. (students를 먼저 JOIN)

```sql
SELECT s.name
FROM students s
JOIN packages p
    ON s.id = p.id
JOIN (
      SELECT
        f.id
        , f.friend_id
        , p.salary AS friend_salary
      FROM friends f
      JOIN packages p
      ON f.friend_id = p.id
     ) AS fp
  ON s.id = fp.id
     AND p.salary < fp.friend_salary
ORDER BY fp.friend_salary
;
```
* JOIN할 때는 두 테이블의 모든 컬럼을 먼저 JOIN한 뒤 SELECT한 컬럼을 뿌려주게 됨. 따라서 ON에 사용되는 키값이 SELECT되지 않아도 무방.
