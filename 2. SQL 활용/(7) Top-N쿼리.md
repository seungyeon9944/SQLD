### Top-N 쿼리

### `ROWNUM`

실제로 존재하지않은 가짜컬럼. 

행이 반환될 때마다 순번이 1씩 증가하때문에 *✏️ WHERE **ROWNUM =** 5 이런 조건 안됨 !* < 나 ≤ 사용

WHERE ROWNUM 조건 뒤에 ORDER BY가 붙으면 그냥 랜덤으로 뽑는꼴이라서 의미x ..

```sql
SELECT ROWNUM,
       이름,
       영어,
       수학
  FROM (
    SELECT 이름,
           영어,
           수학
      FROM EXAM_SCORE
     ORDER BY 영어 DESC, 수학 DESC)
 WHERE ROWNUM <= 5;
```

✏️ 오답노트

적절한 코드의 예시

```sql
SELECT NAME, SCORE FROM (
SELECT NAME, SCORE
FROM EXAM_SCORE
ORDER BY SCORE DESC)
WHERE ROWNUM <= 5;

# 혹은

SELECT NAME, SCORE FROM (
SELECT NAME, SCORE
       ROW_NUMBER() OVER(ORDER BY SCORE DESC) RN
  FROM EXAM_SCORE)
 WHERE RN <= 5;
```

적절하지 않은 코드의 예시

```sql
SELECT NAME, SCORE
FROM EXAM_SCORE
WHERE ROWNUM <= 5 # 먼저 랜덤으로 ROWNUM이 매겨져서 틀림 !
ORDER BY SCORE DESC;
```

```sql
SELECT NAME, SCORE,
       ROW_NUMBER() OVER(ORDER BY SCORE DESC) RN
  FROM EXAM_SCORE
 WHERE RN <= 5; # WHERE절이 SELECT절보다 먼저 수행되면 RN을 사용못함 ㅜㅜ!
```

### 셀프 조인 (Self Join)

서로 **연관관계**가 있는 컬럼이 **하나의 테이블 내에 존재**할 때 수행