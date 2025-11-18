JOIN은 모든 발생가능한 조건 그려놓고 ON으로 걸러내는 순서로 진행 !

### ⭐`INNER JOIN`

SELECT ~ FROM ~ INNER JOIN ~ **ON** ~

✏️ 이벤트 응모한 횟수가 10회 이상인 회원 정보 출력할 때

```sql
# [회원] 테이블에는 회원번호 / 이름, 핸드폰번호
# [이벤트응모] 테이블에는 회원번호(FK), 응모순번 / 응모일자

SELECT A.이름, A.핸드폰 번호
FROM 회원 A
INNER JOIN 이벤트 응모 B ON A.회원번호 = B.회원번호
GROUP BY A.이름, A.핸드폰 번호 # SELECT 절의 컬럼과 같은 컬럼으로 정의되어야 함
HAVING COUNT(A.회원번호) >= 10;

# 조건 2가지 충족
# 1) 이벤트응모 테이블에 회원정보 존재 2) 이벤트응모 테이블에 저장된 row가 10건 이상인 회원
```

OUTER JOIN : **모든 행**이 출력되는 테이블의 **반대편** 테이블 옆에 **(+) 기호**. 스탠다드에서는 JOIN 조건에 충족하는 데이터가 아니어도 출력될 수 있는 방식

---

### ⭐`LEFT OUTER JOIN`

왼쪽에 표기된 테이블 데이터는 무조건 출력

오른쪽 테이블에 JOIN되는 데이터가 없는 row들의 값은 NULL로 출력됨

### ⭐`RIGHT OUTER JOIN`

오른쪽에 표기된 테이블 데이터는 무조건 출력

왼쪽 테이블에 JOIN되는 데이터가 없는 row들의 값은 NULL로 출력됨

| COL1 | COL2 |
| --- | --- |
| 2 | G |
| 3 | H |
| 3 | I |

✏️ 조건이 있는 RIGHT OUTER JOIN의 경우

| COL1 | COL2 |
| --- | --- |
| 1 | A |
| 2 | B |
| 3 | NULL |
| 4 | D |
| 5 | E |

```sql
SELECT *
FROM SAMPLE1 A RIGHT OUTER JOIN SAMPLE2 B
ON (A.COL1 = B.COL1 AND B.COL2 IS NOT NULL);

# RIGHT OUTER JOIN이니까 B는 다 출력하고 A는 조건에 맞는 것만 출력 (나머지는 NULL로 채움)
```

정답 :

| COL1 | COL2 | COL1 | COL2 |
| --- | --- | --- | --- |
| NULL | NULL | 1 | A |
| 2 | G | 2 | B |
| NULL | NULL | 3 | NULL |
| NULL | NULL | 4 | D |
| NULL | NULL | 5 | E |

✏️ *WHERE 문이 있는 RIGHT OUTER JOIN의 경우*

```sql
SELECT *
FROM SAMPLE1 A RIGHT OUTER JOIN SAMPLE2 B
ON A.COL1 = B.COL1
WHERE B.COL2 IS NOT NULL;

# RIGHT OUTER JOIN이지만 WHERE 조건에 의해 최종적으로 필터링 됨
```

정답 :

| COL1 | COL2 | COL1 | COL2 |
| --- | --- | --- | --- |
| NULL | NULL | 1 | A |
| 2 | G | 2 | B |
| NULL | NULL | 4 | D |
| NULL | NULL | 5 | E |

---

### `FULL OUTER JOIN`

데이터 모두 출력 (합집합, 중복 제거)

---
### ⭐`NATURAL JOIN`

**같은 이름 가진 칼럼들이 모두 동일한 데이터** 가지고있을 경우 (MSSQL에서는 X)

✏️ ***ON 사용 불가** !!* 

*✏️ USING, ON, WHERE절에서 **조건 정의 X***

✏️ *공통 컬럼 앞에 **테이블명이나 줄임말 붙이기 X***

✏️ 오라클에서는 `USING` 이용해서 원하는 컬럼만 JOIN에 이용 *(USING 절로 정의된 컬럼 앞에는 별도의 테이블명이나 줄임말 사용 X)*

| COL1 | COL2 |
| --- | --- |
| 2 | G |
| 3 | H |
| 3 | I |

| COL1 | COL2 |
| --- | --- |
| 1 | A |
| 2 | B |
| 3 | H |
| 4 | D |
| 5 | I |

정답 : 13

| COL1 | COL2 |
| --- | --- |
| 2 | G |
| 3 | H |
| 4 | D |
| 4 | I |

```sql
SELECT SUM(COL1)
FROM SAMPLE1 A JOIN SAMPLE2 B
USING (COL1);
# COL1이 기준이니까 COL1 겹치는 값들의 행 출력
```

---
### `CROSS JOIN`

조합할 수 있는 모든 경우 출력.

별도의 `JOIN` 조건이 없으면 Cartesian Product해주면 됨