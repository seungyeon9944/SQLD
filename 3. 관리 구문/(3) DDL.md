### 3. DDL

Data Definition Language, 데이터 정의

데이터 타입

- **CHAR** (’Mark’ = ‘Mark ‘), **VARCAR** (’Mark’ ≠ ‘Mark ‘), **CLOB** : 문자
- **NUMBER**
    - *✏️ NUMBER(5,2)의 경우는 5자리 중 2자리가 소수점자리. 123.456은 가능하지만 1234.5는 안됨 ! **소수점은 삽입될 때 알아서 자리 맞춰서 입력됨***
- **DATE**

### `CREATE`

테이블 생성 시 규칙 !

- 컬럼명은 고유해야 함
- 컬럼명 뒤에 데이터 유형, 데이터 크기가 명시되어야 함
- 테이블, 컬럼명은 숫자로 시작할 수 x

```sql
CREATE TABLE TEACHER (
TEACHER_NO NUMBER NOT NULL,
TEACHER_NAME VARCHAR(20) NOT NULL,
SUBJECT_ID VARCHAR(5) NOT NULL,
CONSTRAINT TEACHER_PK PRIMARY KEY (TEACHER_NO),
CONSTRAINT TEACHER_FK FOREIGN KEY (SUBJECT_ID) REFERENCES SUBJECT(SUBJECT_ID)
);
```

제약 조건 !

- `CHECK`  : 컬럼에 **저장될 수 있는 값의 범위 제한**. 예를 들어 `CONSTRAINT CHK_HIRE_AVAIL CHECK(HIRE_AVAIL IN('Y', 'N')`
- 참조 무결성 규정 관련 옵션
    - **CASCADE** : Parent값 삭제 시 Child값 **같이 삭제**
    - **SET NULL**
    - **SET DEFAULT**
    - **RESTRICT** : **Child 테이블**에 해당 **데이터가 PK가 아닌경우에만** Parent값 삭제, 수정 가능
    - **NO ACTION** : 참조 무결성 제약 걸려있으면 삭제, 수정 x

### `ALTER`

```sql
ALTER TABLE TEACHER ADD BIRTHDAY VARCHAR2(8);
ALTER TABLE TEACHER DROP COLUMN ADDRESS;
ALTER TABLE TEACHER MODIFY (BIRTHDAY VARCHAR2(8) DEFAULT '9999999' NOT NULL);
ALTER TABLE TEACHER RENAME COLUMN MOBILE_NO TO HP_NO;
ALTER TABLE TEACHER ADD CONSTRAINT TEACHER_FK FOREIGN KEY (SUBJECT_ID) REFERENCES SUBJECT(SUBEJCT_ID);
```

✏️ 오답노트 : ***DEFAULT 값이 변경되어도** 이미 삽입된 행은 반영x, **이후 삽입되는 행에만 적용.** 값이 아예 입력되지않는경우에만 DEFAULT 값 적용*

*✏️ NOT NULL은 DEFAULT 없이 불가능 .. **NOT NULL DEFAULT**라고 같이 써야함*

### `DROP`

```sql
DROP TABLE SUBJECT CASCADE CONSTRAINT; # 참조 제약조건도 삭제
```

### `RENAME`

```sql
RENAME TABLE TEACHER TO NEW_TEACHER;
```

### `TRUNCATE`

테이블에 저장되어있는 데이터 모두 제거. ROLLBACK 불가능

```sql
TRUNCATE TABLE TEACHER;
```