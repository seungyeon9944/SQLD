### 1. DML

Data Manipulation, 정의한대로 데이터 입력 + 수정, 삭제, 조회

MSSQL이 아닐경우에는 별도로 COMMIT해주어야 데이터가 반영됨

✏️ ***UNIQUE 제약조건에서는 NULL 허용** . NULL이 삽입되어있는경우 UNIQUE 제약조건을 추가할 수 있음*

*✏️ 컬럼 크기는 얼마든지 늘릴 수는 있지만 **실제 데이터의 최대 길이만큼만 줄일 수 있다***

*✏️ 기본키는 **고유키 + NOT NULL 제약조건***

### `INSERT`

명시되지않으면  NULL 입력, 근데 **PK나 NOT NULL 제약조건 있으면 무조건 입력해야**함

```sql
INSERT INTO 입사 VALUES ('개발', '202201', '220101', '신입');
SELECT * FROM 입사;
```

### `UPDATE`

WHERE 절이 없으면 모든 행이 바뀜 !

```sql
UPDATE 입사 SET 구분 = '경력' WHERE 입사자사번 = '220101';
SELECT * FROM 입사;
```

### `DELETE`

WHERE 절이 없으면 모든 행이 삭제됨 !

COMMIT 전에 ROLLBACK 가능, 로그 안남길거면 `TRUNCATE`  사용

```sql
DELETE FROM 입사 WHERE 입사자사번 = '220101';
SELECT * FROM 입사;
```

### `MERGE`

입력이나 변경 작업을 한 번에 할 수 있도록

```sql
MERGE
INTO DEPARTMENTS_BACKUP DB
USING (SELECT * FROM DEPARTMENTS WHERE MANAGER_ID IS NOT NULL) D
ON (DB.DEPARTMENT_ID = D.DEPARTMENT_ID)
WHEN MATCHED THEN
     UPDATE
        SET DB.DEPARTMENT_NAME = D.DEPARTMENT_NAME,
            DB.MANAGER_ID = D.MANAGER_ID
WHEN NOT MATCHED THEN
     INSERT (DB.DEPARTMENT_ID, DB.DEPARTMENT_NAME, DB.MANAGER_ID)
     VALUES (D.DEPARTMENT_ID, D.DEPARTMENT_NAME, D.MANAGER_ID);
```