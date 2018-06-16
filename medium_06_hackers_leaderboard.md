2018.3.22 MySQL
### Contest Leaderboard
https://www.hackerrank.com/challenges/contest-leaderboard/problem
해커들의 challenge별 최고 점수를 합산해 total score를 계산.


```sql
SELECT
  h.hacker_id
  , h.name
  , t.total_score
--hacker별 점수 총합 산출  
FROM (
      SELECT
        hacker_id
        , SUM(c_score) AS total_score
      --각 hacker의 challenge별 최대 점수 추출  
      FROM (
            SELECT
              hacker_id
              , MAX(score) AS c_score
            FROM submissions
            GROUP BY hacker_id, challenge_id
            ) s  
      GROUP BY
        hacker_id
      HAVING
        total_score != 0  --점수 총합이 0인 사람 제외
      ) t  
JOIN
  hackers h
  ON h.hacker_id = t.hacker_id
ORDER BY
  total_score DESC, hacker_id;
```

# 잘못된 쿼리
```sql
SELECT h.hacker_id, name, SUM(score) AS total_score
FROM (
      SELECT hacker_id, challenge_id, MAX(score) AS score
	    FROM submissions
	    GROUP BY hacker_id, challenge_id
      ) s
JOIN hackers AS h
  ON h.hacker_id = s.hacker_id
GROUP BY hacker_id
HAVING total_score != 0
ORDER BY total_score DESC, hacker_id;
```

#### 잘못된 이유
1.
full_group_by한 값과 SELECT하는 값을 섞어서 쓰는 실수를 반복한다.
위의 쿼리는 먼저 JOIN한 뒤 GROUP BY 하게 된다. 따라서 name이 문제가 된다.

발생 에러
ERROR 1055 (42000) at line 3: Expression #2 of SELECT list is not in GROUP BY clause
and contains nonaggregated column 'run_gqv6fauv4gg.h.name'
which is not functionally dependent on columns in GROUP BY clause;
this is incompatible with sql_mode=only_full_group_by

2.
위 쿼리를 GROUP BY hacker_id, name으로 수정하면
제대로 돌아가고 맞는 결과를 도출하긴 한다.
하지만 JOIN하기 전에 먼저 데이터셋을 최소화하기 위해, HAVING절을 먼저 적용하는 것이 맞다.
위 쿼리는 효율이 상대적으로 떨어진다.


#### Lesson Learned
컬럼명에 쓰는 alias를 최소화 하려면 JOIN하는 경우만 SELECT 문과 ON 절에만 써주면 된다.
그러나 다른 곳에도 써주는게 좋은 코딩스타일인지는 고민해봐야 겠다.
GROUP BY, ORDER BY가 고민된다.
