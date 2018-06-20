2018.3.23 MySQL
### Top Competitors
https://www.hackerrank.com/challenges/full-score/problem
만점을 받은 challenge의 횟수가 1회 이상인 hacker만 추출

<br>

#### Ver.1.0
```sql
SELECT
  h.hacker_id
, h.name
FROM (
      SELECT
        s.hacker_id
      , COUNT(DISTINCT(s.challenge_id)) AS num_challenge
      FROM
        submissions s
      JOIN
        challenges c
        ON s.challenge_id = c.challenge_id
      JOIN
        difficulty d
        ON c.difficulty_level = d.difficulty_level
      --만점인 submission만 필터링  
      WHERE
        s.score = d.score  
      GROUP BY
        hacker_id
      --만점받은 challenge가 1회 이상인 hacker만 필터링
      HAVING
        num_challenge > 1  
      ) tmp
JOIN hackers h
  ON h.hacker_id = tmp.hacker_id
ORDER BY num_challenge DESC, hacker_id;
```

4개의 테이블을 JOIN하는 문제였음. 좀 더 효율적인 방식이 있는가?

#### Ver.1.5
아래 쿼리와 같이 WHERE의 조건을 JOIN ON에 사용해도 EXPLAIN결과는 동일함. 가독성만 나빠짐.

```sql
EXPLAIN(
SELECT
  h.hacker_id
, h.name
FROM (
      SELECT
        s.hacker_id
      , COUNT(DISTINCT(s.challenge_id)) AS num_challenge
      FROM
        submissions s
      JOIN (
           SELECT
             c.challenge_id
           , d.score
           FROM
             challenges c
           JOIN
             difficulty d
             ON c.difficulty_level = d.difficulty_level
           ) t1
        ON s.challenge_id = t1.challenge_id
           AND s.score = t1.score
      GROUP BY
        hacker_id
      HAVING
        num_challenge > 1
      ) t2
JOIN
  hackers h
  ON h.hacker_id = t2.hacker_id
ORDER BY
  num_challenge DESC
, hacker_id;
```

#### Ver.2.0
각 챌린지별 최고 점수를 받은 submission만 먼저 추출하여 JOIN 수행

```sql
SELECT
    h.hacker_id
  , h.name
FROM (
      SELECT
          m.hacker_id
        , COUNT(m.challenge_id) as num_challenge
      FROM (
            SELECT
                hacker_id
              , challenge_id
              , MAX(score) AS max_score
            FROM
              submissions
            GROUP BY
              hacker_id, challenge_id
            ) m  --최고 점수만 추출
      JOIN challenges c
        ON m.challenge_id = c.challenge_id
      JOIN difficulty d
        ON c.difficulty_level = d.difficulty_level
      WHERE
        m.max_score = d.score  --만점을 받은 챌린지만 필터링
      GROUP BY
        hacker_id
      HAVING
        num_challenge > 1
      ) list
JOIN
  hackers h
  ON list.hacker_id = h.hacker_id
ORDER BY
  num_challenge DESC
, hacker_id;
```
