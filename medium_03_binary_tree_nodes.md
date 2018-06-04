2018.3.20 MySQL
### Binary Tree Nodes
https://www.hackerrank.com/challenges/binary-search-tree-1/problem
이진트리의 각 노드 타입 분류하기

```sql
SELECT
    n
  , CASE
      -- 부모노드가 없으면 Root
      WHEN p IS NULL THEN 'Root'  
      -- 어떤 노드에게도 부모노드가 아니면 Leaf
      WHEN n NOT IN (SELECT p FROM bst WHERE p IS NOT NULL)
        THEN 'Leaf'
      --그 외 나머지는 Inner  
      ELSE 'Inner'  
    END AS type
FROM bst
ORDER BY n
;
```

아래는 올바르게 동작하지 않는 쿼리.

```sql
SELECT
    n
  , CASE
      WHEN p IS NULL THEN 'Root'
      WHEN n NOT IN (SELECT p FROM bst) THEN 'Leaf'
      ELSE 'Inner'
    END AS type
FROM bst
ORDER BY n
;
```

Leaf를 검출하지 못함.
참고 :
 * <a href='https://stackoverflow.com/questions/14245671/mysql-not-in-from-another-column-in-the-same-table'> NOT IN (Set containing null) always returns no rows.</a>
 * <a href='
https://stackoverflow.com/questions/1001144/mysql-select-x-from-a-where-not-in-select-x-from-b-unexpected-result'> SELECT X FROM NOT IN </a>
