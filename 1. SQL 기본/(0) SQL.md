### `SELECT`

```sql
SELECT (컬럼이름) FROM (테이블) WHERE (컬럼 1 = '어쩌구');

SELECT * FROM (테이블); # 테이블의 전체 ROW가 조회됨

SELECT B.BAND_NAME, BM.MEMBER_NAME
FROM BAND B, BAND_MEMBER BM # 별도의 별칭 붙여줄 수 있음
WHERE B.BAND_CODE = BM.BAND_CODE;
```

✏️ *ALIAS (줄임말) 별도 지정안하면* `SELECT hire_date` *여도 컬럼에는 ‘HIRE_DATE’라고 저장됨*

*✏️ FROM절을 쉼표로 연결하면 **Cartesian Product** 해주어야 함*

*✏️ FROM절에서 테이블별칭 ‘B’, ‘BM’ 쓰면 **WHERE절에서도 계속 테이블별칭** 써야함*

---

### WITH문 (공통테이블 식)

CTE, Common Table Expression

임시로 이름이 붙은 결과집합(가상테이블) 정의. 가독서어 + 유지보수성 위해서

---

### `CASE` = `DECODE`

~이면 ~이고, ~이면 ~이다.

```sql
SELECT SUBWAY_LINE,
       CASE SUBWAY_LINE
            WHEN '1' THEN 'BLUE'
            WHEN '2' THEN 'GREEN'
            ELSE 'GRAY'
       END AS LINE_COLOR
FROM SUBWAY_INFO;
# COLUMN은 SUBWAY_LINE, LINE_COLOR이고
# 1 - BLUE, 2 - GREEN, 3 - GRAY, 4 - GRAY
```

```sql
SELECT SUBWAY_LINE,
       DECODE(SUBWAY_LINE, '1', 'BLUE', '2', 'GREEN', 'GRAY') AS LINE_COLOR
FROM SUBWAY_INFO;
# COLUMN은 SUBWAY_LINE, LINE_COLOR이고
# 1 - BLUE, 2 - GREEN, 3 - GRAY, 4 - GRAY
```

✏️ 오답

```sql
SELECT DEPT_NAME,
       COUNT(*) AS CNT,
       CASE WHEN COUNT(*) < 10 THEN 'S' # CNT라고 하면 틀림
            WHEN COUNT(*) < 20 THEN 'M'
            ELSE 'L' *# ELSE는 마지막 WHEN절에만 가능 !!!!*
       END TEAM_SIZE
FROM EMP
GROUP BY DEPT_NAME; # 사원을 부서별로 COUNT했으니까
```

---

### WHERE 절

연산자들

- *✏️ 논리 연산자 우선순위 : **( ) → NOT → AND → OR***
- *✏️ 조건식에서 컬럼명은 우측에 위치해도 됨*
- *✏️ WHERE절은 SELECT절보다 **먼저 수행**되기때문에 SELECT절의 ALIAS 사용 x*
- `!= ^= <>` : 같지 않음
- `BETWEEN` : A와 B 사이 (A, B **포함**)
- **`LIKE` :** 값이 특정 패턴에 일치하는지 확인
    - `%` : **0개 이상의 문자열과 일치**하는지 확인 (`%son`이면 json, jason 다 가능)
    - `_` : 정확히 **1개 문자**와 일치하는지 확인 (`_son`이면 무조건 1글자만 가능)
    - `ESCAPE` 기호로 ‘%’나 ‘_’ 포함 문자 검색
        
        ```sql
        SELECT CONTENT
        FROM REVIEW
        WHERE CONTENT LIKE '%#%%' ESCAPE '#'; # 200% 만족합니다
        ```
        
- SQL은 논리 연산에 대해 `TRUE, FALSE, UNKNOWN` 이렇게 세 가지 값을 사용하기 때문에 행 조회할 때  `= NULL` 이라고 안쓰고 `IS NULL`이라고 써야함.

---

### GROUP BY 절

- `COUNT(*)` : 전체 row 세어서 반환
- `COUNT(컬럼)` : 컬럼값이 **NULL인 row 제외** 하고 세어서 반환
- `COUNT(DISTINCT 컬럼)` : 컬럼값이 **NULL 제외, 중복 제거**하여 세어서 반환

*✏️ 컬럼 별칭 사용x*

*✏️ SUM, COUNT 함수 사용x*

*✏️ 명시되지않은 컬럼을 그룹함수없이 SELECT절에 사용x*

*✏️ GROUP BY에 없으면 **SELECT절에도 사용x***

✏️ 오답노트

```sql
SELECT COUNT(*) AS RESULT FROM TAB1 WHERE COL1 = 200 GROUP BY COL1;
# WHERE 조건 만족하는 행이 없음 
# 공집합을 GROUP BY하면 아무것도 출력되지않음 (RESULT 컬럼만 출력)

SELECT SUM(COL2) AS RESULT FROM TAB1 WHERE COL1 = 100;
# WHERE 조건 만족하는 행이 있긴한데 NULL임
# 여기서 NULL이란 : 값은 존재하나 알려지지않음 (RESULT 칼럼과 그 밑에 행이 출력)
```

---

### HAVING 절

데이터 그룹핑하고 특정 **그룹** 골라낼 때 ..

```sql
SELECT STUDENT_NO, AVG(MATH_SCORE) AS AVG
FROM STUDENT_SCORE
WHERE YEAR = '2021' AND SEMESTER = '1'
GROUP BY STUDENT_NO
HAVING AVG(MATH_SCORE) >= 90;
ORDER BY AVG;
```

✏️ 혹은 `FROM - WHERE - GROUP BY - HAVING - SELECT - ORDER BY` 

✏️ 오답노트

```sql
SELECT COUNT(*)
FROM EMP
GROUP BY DEPTNO
HAVING COUNT(*) >= 60; 
# HAVING절은 또 세는게아니고 COUNT(*)에 해당하는값인 6 반환
```

### ⭐WHERE과 HAVING의 COUNT 차이

| 조건에 맞는 행(그룹)이 없는경우 ! | **COUNT** | **SUM, AVG, MIN, MAX** |
| --- | --- | --- |
| **WHERE** | 0 | **NULL** |
| **HAVING** | **공집합** | **공집합** |

---

### ORDER BY 절

FROM 절 뒤에 위치

결과를 **오름차순 (ASC, 기본 값) 혹은 내림차순 (DESC)** 으로 정렬

(Oracle에서는 NULL을 최댓값으로 취급)

✏️ *SELECT 절에 기술된 컬럼 순서를 숫자로 명시하는 것도 가능*

*✏️ GROUP BY에 없는기준은 ORDER BY에 적을수없음 !!*

```sql
SELECT COL1, COL2
FROM SAMPLE
GROUP BY COL2
ORDER BY 1 DESC, COL2;
```

---

### 산술 연산자

*✏️ 가로 연산 (다른 컬럼끼리 연산)에서 NULL이 있으면 결과는 무조건 NULL ! NULL + 100 = NULL*

*✏️ NULL과의 모든 비교 (IS NULL 제외)는 **알 수 없음을 반환***

*✏️ COUNT(COL1)했는데 만족하는 값이 **NULL이면 0 리턴**. 만족하는 값이 **없으면 비워둠.***

1순위 : `()`  - 괄호로 우선순위 조정

2순위 : `*` , `/` 

3순위 : `+` , `-` , `%` - 나머지 (0으로나누면 NULL)

### 합성 연산자

`||`  - 문자와 문자 연결

```sql
SELECT 'S'||'Q'||'L' AS SQLD
FROM DUAL;
```