2018.3.18 MySQL

#### 1. 모음으로 시작하는 city명 추출

```sql
SELECT DISTINCT(city)
FROM station
WHERE city REGEXP '^[AEIOU]';
```

다수의 LIKE 대신 REGEXP를 사용한다.


#### 2. 모음으로 끝나는 city명 추출

```sql
SELECT DISTINCT(city)
FROM station
WHERE city REGEXP '[AEIOU]$';
```

MySQL은 $가 뒤에 붙음.


#### 3. 시작과 끝이 모음인 city명 추출

__틀린 표현__
```sql
SELECT DISTINCT(city)
FROM station
WHERE city REGEXP '^[AEIOU]$';
```

__맞는 표현__
```sql
SELECT DISTINCT(city)
FROM station
WHERE city REGEXP '^[AEIOU].*[AEIOU]$';
```

 __해석__
 ^			// start of string
 [aeiou]			// a single vowel
 .			// any character...
 \*			// ...repeated any number of times
 [aeiou]			// another vowel
 $			// end of string
 
