### 4. DCL

Data Control Language, 유저 생성 + 유저에게 데이터 컨트롤할 수 있는 권한 부여 or 회수

### `CREATE USER`

```sql
CREATE USER MARK IDENTIFIED BY PW9982;
```

### `ALTER USER`

```sql
ALTER USER JAEHYUN IDENTIFIED BY PW0214;
```

### `DROP USER`

```sql
DROP USER JAEHYUN;
```

### `GRANT`

사용자에게 권한 부여

*✏️ DBA 권한이 없어도 특정 객체나 시스템 권한에 대해 **재부여 옵션 부여받으면 권한 줄 수 있음***

```sql
GRANT CREATE USER TO MARK;
GRANT CREATE TABLE TO MARK;
```

### ⭐`REVOKE`

사용자에게 권한 회수

*✏️ **WITH GRANT OPTION** : 관리자가 제3자의 권한을 직접 회수할 수는 없지만 **중간관리자 권한**을 회수하면 제3자에게 부여한 권한도 함께 회수*

*✏️ **WITH ADMIN OPTION** : 중간관리자 권한 회수 시 **제 3자에게 부여한 권한 함께 회수 x***

```sql
REVOKE CREATE TABLE FROM MARK;
```

### ROLE 관련 명령어

ROLE 생성 → ROLE에게 권한 부여 → 사용자에게 ROLE 부여

```sql
CREATE ROLE CREATE_R;
GRANT CREATE SESSION, CREATE USER, CREATE TABLE TO CREATE_R;
GRANT CREATE_R TO MARK;
```