### ⭐`NVL(인수1, 인수2)`

(i) 인수1이 **NULL이면 인수 2**

(ii) 인수1이 NULL이 아니면 인수1

(ISNULL과 같음 !)

*✏️ 인수1과 인수2가 데이터유형이 일치해야함*

```sql
SELECT * FROM REVIEW;
# REVIEW_SCORE가 5, [NULL], 1, 3일 때

SELECT MEMBER_NO, 
NVL(REVIEW_SCORE, -1) AS REVIEW_SCORE
FROM REVIEW;
# COLUMN은 MEMBER_NO와 REVIEW_SCORE이고
# REVIEW_SCORE에는 5, -1, 1, 3
```

### ⭐`NVL2(인수1, 인수2, 인수3)`

(i) 인수1이 **NULL이면 인수3**

(ii) 인수1이 NULL이 아니면 인수2

```sql
SELECT * FROM REVIEW;
# REVIEW가 '좋음', [NULL], '별로', '그냥저냥'일 때

SELECT MEMBER_NO,
NVL2(REVIEW, '리뷰있음', '리뷰없음') AS REVIEW_CHECK
FROM REVIEW;
# COLUMN은 MEMBER_NO와 REVIEW_CHECK이고
# REVIEW_CHECK에는 '리뷰있음', '리뷰없음', '리뷰있음', '리뷰있음'
```

### ⭐`NULLIF(인수1, 인수2)`

(i) **인수1 = 인수2면 NULL**

(ii) 같지않으면 인수1

```sql
SELECT * FROM REVIEW;
# REVIEW_SCORE가 5, 0, 1, 3일 때

SELECT MEMBER_NO,
       NULLIF(REVIEW_SCORE, 0) AS REVIEW_SCORE,
       REVIEW
FROM REVIEW;
# COLUMN은 MEMBER_NO, REVIEW_SCORE, REVIEW이고
# REVIEW_SCORE에는 5, [NULL], 1, 3
```

### ⭐`ISNULL(인수1, 인수2)`

(i) 인수1이 **NULL이면 인수2**

### ⭐`COALESCE(인수1, 인수2, 인수3 ...)`

**NULL이 아닌 최초의 인수** 반환

```sql
SELECT * FROM MEMBERINFO;

SELECT NAME,
       COALESCE(PHONE, EMAIL, FAX) AS CONTACT;
FROM MEMBERINFO;
# COLUMN은 NAME, CONTACT이고
# NULL이 아닌 인수의 값
```

*✏️ NVL(NULL, 100)과 COALESCE(NULL, 100)과 ISNULL(NULL, 100) 모두 100을 반환. NULLIF(NULL, 100)만 결과값이 다름 !*