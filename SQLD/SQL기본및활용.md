# SQL 기본

## 관계형 데이터베이스 개요

### DBMS(DataBase Management System)

- RDBMS(관계형DBMS)
    - 데이터를 테이블 형태로 저장하고 테이블간 관계를 활용하여 관리하는 데이터베이스
    - Oracle, SQL Sever

### SQL(Structured Query Language)

- RDBMS에서 데이터를 생성, 조회, 수정, 삭제하기 위한 표준 언어

1. 데이터 정의어(DDL): 테이블 생성, 변경 삭제
    - CREATE, ALTER, DROP, RENAME, TRUNCATE

2. 데이터 조작어(DML): 데이터를 조회, 삽입, 수정, 삭제
    - SELECT, INSERT, UPDATE, DELETE

*SELECT를 데이터 조회어(DQL)로 구분하기도 함*

3. 데이터 제어어(DCL): 권한 관리
    - GRANT, REVOKE

4. 트랜잭션 제어어(TCL): 트랜잭션을 제어
    - COMMIT, ROLLBACK, SAVEPOINT


### 관계대수 연산자

*관계대수는 SQL의 이론적 기초가 되는 수학적 언어*


## SELECT 문

### SELECT 문의 구조

```sql
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
```

### SELECT 절의 활용

**별칭 사용 시 주의사항**

- 숫자나 특수문자 사용 불가, 예약어 금지
- 쌍 따옴표(") 사용하여 특수문자와 공백 포함 가능, 가능한 특수문자(_, $, #)
- WHERE 절은 SELECT 문 보다 먼저 수행되므로 SELECT 문에 정의된 별칭 사용 불가

*Oracle에서의 CONCAT은 ||, SQL Server에서의 CONCAT은 +*

## 함수

### 함수

- 단일행 함수: Input1 $\rightarrow$ Output1
- 다중행 함수: InputN $\rightarrow$ Output1

*Oracle에서는 함수를 테스트하기 위해 더미 테이블인 DUAL을 자주 사용*

### 문자함수

- LOWER
- UPPER
- SUBSTR
- TRIM
- LTRIM
- RTRIM
- LENGTH
- REPLACE
- ASCII

### 숫자함수

- ABS
- MOD
- ROUND
- TRUNC
- FLOOR
- CEIL
- SIGN
- POWER

### 날짜함수

- SYSDATE
- GETDATE

*SYSDATE는 Oracle, GETDATE는 SQL Server 함수*

*날짜형은 날짜 간 덧셈/뺄셈, 정렬 등의 활용 가능*

### 변환함수

- TO_NUMBER
- TO_CHAR
- TO_DATE

*날짜형과 문자형은 겉으로는 형식이 같아 보여도, 내부적으로는 완전히 다른 데이터*

### 집계함수

- COUNT, SUM, AVG, MIN, MAX

*집계함수는 다중행 함수 / NULL 제외 계산이 원칙 / WHERE 절에서 사용 불가*

*COUNT(\*), COUNT(1), COUNT(0)은 모두 NULL을 포함하여 개수를 계산*

### NULL 관련 함수

- NVL(컬럼, 대체값): 컬럼값이 NULL이면 대체값, NULL이 아니면 원래값
- NULLIF
- COALESCE(컬럼1, 컬럼2, 컬럼3, ...): 가장 먼저 NULL이 아닌 값

### CASE 문 / DECODE 함수

- CASE 문
- DECODE(컬럼, 값1, 반환1, 값2, 반환2, ..., 반환N)

*DECODE는 Oracle 전용 함수*

## WHERE 절

### WHERE 절

### 기본 비교 연산자

```sql
!=, <>, ^=
# 다르다
```

### 조건 표현 연산자

```sql
IN
# 여러 값 중 하나
BETWEEN A AND B
# 컬럼 >= A AND 컬럼 <= B
```

### 날짜 조건 조회

### LIKE 연산자

- 와일드 카드를 활용(`%`: 0개 이상의 문자를 의미 / `_`: 정확히 1개의 문자를 의미)

*`%` 혹은 `_`가 문자 자체로 포함된 값을 찾고 싶을 때, ESCAPE를 사용*

### NULL 비교 연산자

- IS NULL
- IS NOT NULL

*NULL 비교 시, =NULL 혹은 !=NULL의 표현은 오류 발생*

### 연산자 우선순위

- `()` $\rightarrow$ `*`,`/`,`%` $\rightarrow$ 비교연산자, `LIKE`, `BETWEEN`, `IN`, `IS NULL`, `IS NOT NULL` $\rightarrow$ `NOT` $\rightarrow$ `AND` $\rightarrow$ `OR`

## GROUP BY, HAVING 절

### GROUP BY 절

- 특정 컬럼을 기준으로 행들을 그룹화하여 **집계함수 결과를 산출**

### HAVING 절

*WHERE 절에서는 집계 함수 사용 불가하나, HAVING 절에서는 사용*

*GROUP BY 절은 리소스 소모가 크므로, WHERE 절로 먼저 필터링 후 수행하는 것이 성능상 유리*

## ORDER BY 절

### ORDER BY 절

*컬럼 번호를 활용해서 정렬도 가능*

### ORDER BY 절 특징

- GROUP BY 절보다 나중에 수행되므로 집계함수 사용 가능

### 데이터 형에 따른 정렬 순서(오름차순 기준)

- NULL 정렬: Oracle은 NULL을 최댓값으로, SQL Server는 NULL을 최솟값으로 처리

## 조인

### 조인(Join)

- EQUI JOIN
- NON EQUI JOIN

## 표준조인

### 표준조인

- DBMS마다 조인 방식이 다르므로, ANSI/ISO SQL 표준으로 정립한 조인

### INNER JOIN

- USING 조건절: 컬럼명이 같은 컬럼들에 대해서 선택적으로 조인 가능

```sql
SELECT 이름, 부서명 FROM 직원 INNER JOIN 부서 USING (부서ID)
```

- ON 조건절: 컬럼명이 다르더라도 조인 조건을 사용 가능

*JOIN만 단독으로 사용 시 INNER JOIN을 의미하며, NULL 값은 조회 대상이 아님*

### NATURAL JOIN

- 두 테이블 간에 동일한 이름을 갖는 모든 컬럼에 대해 EQUI JOIN 수행
- ON이나 USING 조건절 없이 조인 가능

### CROSS JOIN

- 두 테이블의 모든 행을 조합해서 출력

### OUTER JOIN

- JOIN 조건에서 동일한 값이 없는 행 반환 가능

1. LEFT OUTER JOIN
2. RIGHT OUTER JOIN
3. FULL OUTER JOIN

---

# SQL 활용

## 서브쿼리

### 서브쿼리

### 서브쿼리의 종류

1. 단일형 서브쿼리: 서브쿼리가 하나의 값만 반환

2. 다중열 서브쿼리: 서브쿼리가 두 개 이상의 열을 반환

3. 다중행 서브쿼리: 서브쿼리가 여러 행을 반환

*IN(목록에 포함), ANY(하나라도 조건 만족), ALL(모든 조건 만족), EXISTS(결과가 존재)*

4. 스칼라 서브쿼리: SELECT문 안에서 서브쿼리 결과가 하나의 컬럼

5. 인라인 뷰: FROM 절에서 임시 테이블처럼 활용

*뷰는 테이블과 달리 실제 데이터가 디스크에 저장되어 있지는 않으며, 논리적으로 생성된 객체*

**뷰의 장점**
- 보안성(전체데이터 노출X)
- 독립성(구조변경시 일관성 유지)
- 편의성(쿼리 단순화)

6. 상호연관 서브쿼리: 외부 쿼리의 행 하나하나에 대해 서브 쿼리가 실행

## 집합 연산자

### 집합 연산자

- 두 집합의 스키마(컬럼 수, 컬럼 순서, 데이터 타입)이 일치해야 동작

### 합집합

- UNION ALL: 중복 허용
- UNION: 중복 제거, 내부에서 정렬 수행

### 교집합

## 그룹 함수

### 그룹 함수

- GROUP BY 절에서 여러 행을 하나의 결과값으로 요약하는 함수
- 집계함수의 ROLLUP, CUBE, GROUPING SET 등의 함수가 존재

### ROLLUP

- ROLLUP(컬럼1, 컬럼2)인 경우, (컬럼1, 컬럼2) $\rightarrow$ 컬럼1 $\rightarrow$ 전체 행 순으로 그룹화

*ROLLUP은 소계와 총계를 구할 때 사용하며, 컬럼 순서가 변경 시 결과가 달라짐*

>N+1

### CUBE

- CUBE(컬럼1, 컬럼2)인 경우, (컬럼1, 컬럼2) $\rightarrow$ 컬럼1 $\rightarrow$ 컬럼2 $\rightarrow$ 전체 행 순으로 그룹화

*CUBE는 조합 가능한 전체 경우를 그룹화*

>N^2

### GROUPING SETS

- 그룹화할 대상을 지정 가능

*NULL 혹은 0은 모든 행에 대한 전체 그룹화를 수행*

## 윈도우 함수

### 윈도우 함수

- GROUP BY와 달리, 그룹화하지 않고 각 행에 대하여 계산된 값을 반환하는 함수
- OVER 키워드와 함께 사용

### 순위 함수

- ROW_NUMBER: 무시(중복 없이 연속 번호 부여)
- RANK: 같은 값은 같은 순위를 가지고 순위는 건너뜀
- DENSE_RANK: 같은 값은 같은 순위를 가지고 순위는 건너뛰지 않음

### 집계 함수

*OVER에 기술된 내용으로 연산만 하고 출력 순서는 보장하지 않으며, 필요 시 ORDER BY 활용*

*GROUP BY 에서는 행의 수가 줄어들지만, 윈도우 함수에서는 행을 그대로 유지*

### 행 간의 값 참조 함수

- LAG: 이전 행의 값 가져오기
- LEAD: 다음 행의 값 가져오기
- FIRST_VALUE: 정렬 기준 첫 번째 값 가져오기
- LAST_VALUE: 정렬 기준 마지막 값 가져오기

*윈도우 함수는 현재 행까지만 제한하기 때문에, LAST_VALUE는 범위 지정 키워드 사용 필요*

### 범위 지정 키워드

- ROWS/RANGE BETWEEN [시작 범위] AND [끝 범위]

*범위 지정 생략 시, RANGE BETWEEN UNBOUNDED PRECENDING AND CURRENT ROW로 동작*

```sql
SELECT 날짜, 매출
AVG(매출) OVER (ORDER BY 날짜
ROWS BETWEEN 1 PRECENDING AND 1 FOLLOWING) AS 이동평균 FROM 판매
```

```sql
SELECT 날짜, 매출
SUM(매출) OVER (ORDER BY 날짜
RANGE BETWEEN UNBOUNDED PRECENDING AND CURRENT ROW) AS 누적합 FROM 판매
```

### 비율함수

- NTILE(n): 데이터를 n등분하여 그룹 나누기
- CUME_DIST(): 누적 백분율
- PERCENT_RANK(): 백분위 순위
- RATIO_TO_REPORT(): 전체 대비 비율

## Top N 쿼리

### TOP N 쿼리

- 상위 N개의 데이터를 추출하는 쿼리

1. TOP: SQL Server에서 사용
2. LIMIT
3. FETCH FIRST
4. ROWNUM: Oracle에서 사용

### TOP N 쿼리 사용 유의사항

1. ORDER BY 사용은 필수
2. TOP 사용 시 동일한 순위도 함께 출력하려면 WITH TIES 사용
3. ROWNUM = 1 은 사용 가능하나, 1보다 큰 숫자에 대한 =, > 연산자는 사용 불가

*ROWNUM은 조회 결과에 순차 번호 부여하는 가상의 컬럼*

## 계층형 질의와 셀프 조인

### 계층형 질의

- 데이터가 계층 구조를 가지고 있을 때, 계층 구조를 탐색하기 위하여 사용

**키워드**

- LEVEL: 계층 구조에서 현재의 레벨을 반환
- START WITH: 계층 구조의 시작 지점(루트 노드)을 지정
- CONNECT BY: 부모 자식 관계를 정의하는 절
- PRIOR
    - PRIOR 자식 = 부모: 순방향(부모 $\rightarrow$ 자식)
    - PRIOR 부모 = 자식: 역방향(자식 $\rightarrow$ 부모)
- NOCYCLE: 순환구조가 발생할 경우 무한 루프를 방지
- ORDER SIBLINGS BY: 동일한 수준(LEVEL) 내에서 정렬
- CONNECT_BY_ROOT: 데이터의 최상위 루트노드 정보를 반환
- CONNECT_BY_ISLEAF: 말단(리프)노드이면 1, 아니면 0 반환
- CONNECT_BY_ISCYCLE: 순환이 존재하면 1, 아니면 0 반환
- SYS_CONNECT_BY_PATH: 루트 데이터로부터 현재 위치까지의 경로 표시

```sql
SELECT LEVEL, 사원이름, 직속상관, 직급 FROM 사원
START WITH 직속상관 IS NULL CONNECT BY PRIOR 사원이름 = 직속상관
```

### 셀프 조인

- 같은 테이블에 대하여 조인을 수행

## PIVOT 절과 UNPIVOT 절

### PIVOT 절과 UNPIVOT 절

1. PIVOT 절: 행 데이터를 특정 컬럼 기준으로 열 방향으로 재구성 / LONG DATA를 WIDE DATA로 변경

2. UNPIVOT 절: 열 데이터를 행으로 변환 / WIDE DATA를 LONG DATA로 변경

## 정규 표현식

### 정규표현식

### 정규표현식 기호

|기호|설명|예시|
|---|---|---|
|.|임의의 한 문자|a.c|
|^|문자열의 시작|^010|
|$|문자열의 끝|com$|
|*|앞 문자가 0번 이상 반복|ho* $\rightarrow$ h, ho, hoo 등|
|+|앞 문자가 1번 이상 반복|ho+ $\rightarrow$ ho, hoo 등|
|?|앞 문자의 0 또는 1회|ho? $\rightarrow$ h, ho|
|[]|문자 집합|[abc] $\rightarrow$ a,b,c / [a-z] $\rightarrow$ a 부터 z 까지|
|[^]|부정 문자 집합|[^abc] $\rightarrow$ abc를 제외한 나머지 문자|
|{n}|n회 반복|a{3} $\rightarrow$ aaa|
|{n, m}|n~m회 반복|a{2,4} $\rightarrow$ aa, aaa, aaaa|
|()|그룹핑|(ab)+ $\rightarrow$ ab, abab, ababab 등|
|\||둘 중 하나 이상 일치|^ab\|cd\$ $\rightarrow$ ab로 시작 또는 cd로 끝남|
|\\ |기능 무효화(이스케이프)||

*한글은 한 음절이 아니라 하나의 문자 단위*

### 정규표현식 함수

- REGEXP_LIKE: 문자열이 정규식과 일치하는지 여부 판단
- REGEXP_REPLACE: 정규식에 일치하는 부분 대체

---

# 관리 구문

## DML

### DML

- INSERT, UPDATE, DELETE, MERGE

### INSERT

### UPDATE

*WHERE 절이 없는 경우, 모든 튜플(행)에 대하여 적용*

### DELETE

*DDL의 TRUNCATE도 테이블의 데이터를 삭제하나, TRUNCATE는 작업취소가 불가*

### MERGE

- 두 테이블을 조건에 따라 데이터를 삽입(INSERT)하거나 수정(UPDATE)

```sql
MERGE INTO 학생 t USING 학생_최신 s
ON (t.이름 = s.이름)
WHEN MATCHED THEN
    UPDATE SET t.이름 = s.이름, t.나이 = s.나이, t.학년=s.학년
WHEN NOT MATCHED THEN
    INSERT (학번, 이름, 나이, 학년) VALUES (s.학번, s.이름, s.나이, s.학년)
```

## TCL

### TCL

- COMMIT
- ROLLBACK
- SAVEPOINT

### 트랜잭션 격리 수준

|격리수준|Dirty Read|Non-Repeatable Read|Phantom Read|
|---|---|---|---|
|Read Uncommitted|발생|발생|발생|
|Read Committed|발생하지 않음|발생|발생|
|Repeatable Read|발생하지 않음|발생하지 않음|발생|
|Serializable|발생하지 않음|발생하지 않음|발생하지 않음|

### SAVEPOINT

### 자동커밋(AUTOCOMMIT)

1. Oracle
    - DDL: 무조건 자동커밋이 수행
    - DML: 자동커밋 모드 ON/OFF에 따라 롤백이 가능
2. SQL Server
    - 자동커밋 모드 ON/OFF에 따라서 DDL과 DML이 동작

*Oracle은 기본적으로 자동커밋 모드 OFF / SQL Server는 기본적으로 자동커밋 모드 ON*

## DDL

### DDL

- 데이터의 구조(스키마)를 정의

### CREATE

### ALTER

- 테이블의 구조 변경

```sql
ALTER TABLE 학생 ADD 학년 INT
ALTER TABLE 학생 MODIFY 이름 VARCHAR(30)
ALTER TABLE 학생 DROP COLUMN 주소
ALTER TABLE 학생 RENAME COLUMN 학번 TO ID
```

### DROP

- 테이블의 삭제

```sql
DROP TABLE 학생
DROP TABLE 학생 CASCADE CONSTRAINTS
```

### TRUNCATE

- 테이블 안의 모든 데이터를 삭제하지만, 테이블 구조는 유지(스키마는 유지)

*TRUNCATE는 DELETE와 달리 기억장치에서 바로 데이터가 지워지므로 ROLLBACK이 불가*

## DCL

### DCL

- 사용자의 권한을 관리

### GRANT

- 사용자에게 권한을 부여

### REVOKE

- 사용자의 권한을 회수

### 권한 부여 옵션

1. WITH GRANT OPTION: 객체 권한 부여(SELECT, INSERT, UPDATE, DELETE 등)

2. WITH ADMIN OPTION: 시스템 권한 부여(CREATE TABLE, DROP TABLE 등)
