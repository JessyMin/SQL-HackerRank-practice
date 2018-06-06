2018.3.20 MySQL
### The Report
https://www.hackerrank.com/challenges/the-report/problem
점수에 따라 Grade 부여 및 정렬

```sql
SELECT
    CASE WHEN g.grade >=8 THEN s.name
      ELSE NULL END AS name
  , g.grade
  , s.marks
FROM
  students s
JOIN
  grades g
	ON s.marks BETWEEN g.min_mark AND g.max_mark
ORDER BY
  g.grade DESC,
  CASE WHEN g.grade >= 8 THEN s.name
    ELSE s.marks END
;
```

* `ORDER BY` 절 내에 `CASE` 문을 쓰는 경우, `DESC`는 `CASE`문 바깥에만 사용 가능.

* `GROUP BY`나 `ORDER BY`에도 alias를 써줘야 하는가?
  -> 안써도 에러는 발생하지 않음.
