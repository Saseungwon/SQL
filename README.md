# 🏄‍♂️SQL(Next IT)

## 📚데이터베이스를 구성하는 객체 이해

### 1. 데이터베이스 객체

- 데이터베이스 객체란?

1. 테이블

- 제약조건 : 컬럼에 대한 속성 형태로 정의하지만 엄연히 오라클 데이터베이스 객체 중 하나이며 데이터 무결성을 보장하기 위한 용도로 사용된다.
- NOT NULL(NN) : 열은 null값을 포함할 수 없습니다.
- UNIQUE KEY(UK) : 테이블의 모든 행을 유일하게 하는 값을 가진 열(null 허용)
- PRIMARY KEY(PK) : 유일하게 테이블의 각행을 식별(NOT NULL꽈 UNIQUE 조건을 만족)
- FOREIGN KEY(FK) : 열과 참조된 열 사이의 외래키 관계를 적용하고 설정합니다.
- CHECK(CK) : 참이어야 하는 조건을 지정함(대부분 업무 규칙을 설정)


2. 테이블 Table.제약조건
- NULL : 값이 없음을 의미함, 별도로 지정하지 않으면 해당 컬럼은 NULL을 허용한다, NULL을 허용하지 않으려면 NOT NULL 구문을 명시해야 한다.
- NOT NULL : NOT NULL 제약조건을 명시하면 해당 컬럼에는 반드시 데이터를 입력해야 한다.


/*
table 테이블

1. 테이블명 칼럼명의 최대 크기는 30 바이트

2. 테이블명 컬럼명으로 예약어는 사용할 수 없다.

3. 테이블명 컬럼명으로 문자, 숫자,_,&,# 사용할 수 있지만 첫글자는 문자만 올 수 있다.

4. 한 테이블에 사용가능한 컬럼은 최대 255개까지다.

*/

```sql
CREATE TABLE ex2_1 (
col1 CHAR(10) ,
col2 VARCHAR2(10)
);

-- 삽입구문 SQL 기본에서 상세하게 다룸
INSERT INTO ex2_1(col1, col2)
VALUES('abc', 'abc');

-- LENGTH 함수 SQL 기본에서 상세하기 다룸
SELECT COL1
, LENGTH(COL1) AS LEN1
, COL2
, LENGTH(COL2) AS LEN2
FROM ex2_1;
```
- char 고정형으로 저장 공간의 효율화를 위해 VARCHAR2 사용
```sql
CREATE TABLE ex2_2(
col1 varchar2(3) , --디폴트 값인 byte 적용
col2 varchar2(3 byte),
col3 varchar2(3 char)
);

INSERT INTO ex2_2 (col1, col2, col3)
VALUES('오라클','오라클','오라클');-- 오류

INSERT INTO ex2_2 (col2, col3)
VALUES('abc', '오라클');

SELECT col1, LENGTH(col2) , LENGTHB(col2)
, col3, LENGTH(col3) , LENGTHB(col3)
FROM ex2_2;
```
```sql
CREATE TABLE ex2_3(
col1 NUMBER(3) ,
col2 NUMBER(3,2),
col3 NUMBER(5,-2)
);

INSERT INTO ex2_3 (col1) VALUES(0.7898); --반올림
INSERT INTO ex2_3 (col1) VALUES(99.5); --반올림
INSERT INTO ex2_3 (col1) VALUES(1004); --오류(정수 3자리)
INSERT INTO ex2_3 (col2) VALUES(0.7898); -- 소수점 3자리에서 반올림
INSERT INTO ex2_3 (col2) VALUES(1.2345); -- 소수점 3자리에서 반올림
INSERT INTO ex2_3 (col2) VALUES(32); -- 오류(정수부분은 1자리)

INSERT INTO ex2_3 (col3) VALUES(12345.2346); -- 12300
INSERT INTO ex2_3 (col3) VALUES(12367.2346); -- 12400
INSERT INTO ex2_3 (col3) VALUES(1234567.350);-- 1234600
```
- 날짜 데이터 타입
```sql
CREATE TABLE ex2_4 (
date_1 DATE
, date_2 TIMESTAMP
);

INSERT INTO ex2_4 (date_1, date_2)
VALUES (SYSDATE, CURRENT_TIMESTAMP);
SELECT *
FROM ex2_4;
```
- NOT NULL 제약조건
```sql
CREATE TABLE ex2_5(
col1 varchar2(20) ,
col2 varchar2(20) not null
);

INSERT INTO ex2_5 (col1, col2) values ('abc', 'abc');
INSERT INTO ex2_5 (col1) values ('abc'); -- 에러
INSERT INTO ex2_5 (col2) values ('abc'); -- col1 널 허용 (디폴트 널 허용)
```
- unique 조건 한 컬럼에 동일한 값은 삽입이 안됨
```sql
CREATE TABLE ex2_6(
col1 VARCHAR2(20) ,
col2 VARCHAR2(20) not null,
col3 VARCHAR2(20) unique
);

insert into ex2_6 (col1, col2, col3)
values ('abc', 'abc', 'abc'); --col3 동일한데이터는 삽입 안됨.
```
```sql
CREATE TABLE ex2_7(
col1 VARCHAR2(20) ,
col2 VARCHAR2(20) , -- unique
constraints uq_ex2_6_col2 unique(col2)
);

CREATE TABLE ex2_9 (
nm VARCHAR(30) not null,
age number(3),
gender char(1),
constraints ch_ex2_9_age check (age between 1 and 150) ,
constraints ch_ex2_9_gender check (gender in ('F','M'))
);

INSERT INTO ex2_9 (nm, age, gender) values('malja', 200, 'G'); -- CK_EX2_
INSERT INTO ex2_9 (nm, points) values('nolja', 250); -- CK_EX2_
INSERT INTO ex2_9 (nm, age, gender) values('malja', 200, 'G'); -- CK_EX2_
```
- default 기본값 제약조건
```sql
CREATE TABLE ex2_10
nm varchar2(30) not null,
point number(5) default 0,
gender char(1) default 'F',
reg_date date default sysdate,
constraints ck_ex2_10_gender check (gender in ('F', 'M'))
);

INSERT INTO ex2_10 (nm) values ('sunja');
INSERT INTO ex2_10 (nm, points, gender, reg_date)
values ('nolja',250,'M''1998-08-03');
select * from ex2_10
```
- 테이블 만들어서 엑셀로...
```sql
CREATE TABLE info (
INFO_NO NUMBER(3) NOT NULL,
PC_NO VARCHAR2(10) UNIQUE NOT NULL,
NM VARCHAR2(20),
EMAIL VARCHAR2(100),
HOBBY VARCHAR2(1000)
);
```

- ALTER 테이블 수정 

```sql
-- 컬럼명 수정
ALTER TABLE ex_210 Rename COLUMN col1 TO col12;

-- 테이블 이름, 널유뮤, 유형 확인
DESC ex2_10; -- 확인

-- 컬럼 타입 수정
ALTER TABLE ex2_10 MODIFY col2 VARCHAR2(30);
DESC ex2_10; -- 확인

-- 컬럼 추가
ALTER TABLE ex2_10 ADD col3 NUMBER;
DESC ex2_10; -- 확인

-- 컬럼 삭제
ALTER TABLE ex2_10 DROP COLUMN;
```


##📚 SQL 기본
### SELECT
1. SELECT문
- 가장 기본적인 SQL문으로 테이블이나 뷰에 있는 데이터를 조회할 때 사용
```sql
SELECT*혹은 컬럼
FROM[스키마.]테이블명 혹은 [스키마.]뷰명

WHERE 조건
ORDER BY 컬럼;
```
- WHERE절(어디에서) 어떤 데이터를 가져올 것인가.
- ORDER BY 정렬이 필요할 때(DESC내림차순, ASC오름차)

```sql
SELECT *
FROM DEP , EMP
where dep.deptno = emp.dno;

-- ALTER 테이블 수정 -----------

-- 제컬럼명 수정
ALTER TABLE ex_210 Rename COLUMN col1 TO col12;
-- 테이블 이름, 널유뮤, 유형 확인
DESC ex2_10; -- 확인
-- 컬럼 타입 수정
ALTER TABLE ex2_10 MODIFY col2 VARCHAR2(30);
DESC ex2_10; -- 확인
-- 컬럼 추가
ALTER TABLE ex2_10 ADD col3 NUMBER;
DESC ex2_10; -- 확인
-- 컬럼 삭제
ALTER TABLE ex2_10 DROP COLUMN;


/* SELECT : 가장 기본적인 SQL DML 문
            테이블이나 뷰에 있는 데이터를 조회할 때 사용
*/
SELECT emp_name
    , email --- 조회하고자 하는 컬럼
    
SELECT *   --- * 전체 검색
FROM employees;

SELECT employees_id
    , emp_name
FROM employees

WHERE salary > 5000; -- WHERE 조건

-- ORDER BY 정렬 ASC 디폴트 오름차순 (DESC 내림차순)
SELECT  employee_id
        salary  ,
        emp_name
FROM employees
WHERE salary > 5000
ORDER BY salary DESC ;
--ORDER BY salary ;
--ORDER BY 2 desc ; -- 컬렴 명이 아닌 셀렉트 절 컬럼 순서로 가능


-- AND(그리고)[A,B조건 모두 포함 할 때]
SELECT employee_id ,
       emp_name ,
       salary ,
       job_id
FROM employees
WHERE salary > 5000
AND job_id = 'IT_PROG'
ORDER BY employee_id;

-- alias(가명, 별명)
SELECT a.employee_id ,
       a.emp_name,
       a.department_id,
       b.department_name AS dep_name  --AS 쓰면 그 당시에만 이름이 바뀌어서 나옴(일시적)
FROM employees a ,
    departments b
WHERE a. department_id = b.department_id;
```


2. INSERT문
- 신규 데이터를 입력할 때 사용하는 INSERT문은 크게  컬럼명 생략 형태
- INSERT ~ SELECT 형태로 나눌 수 있다.
```
--INSERT 기본형태
INSERT INTO [스키마.]테이블명(컬럼1,컬럼2…)
VALUES(값1, 값2…)
```
```
--컬럼명은 기술하지 않지만 VALUES 절에는 테이블의 컬럼 순서대로 해당 컬럼 값을 기술해야 한다.
--INSERT 컬럼명 기술 생략 형태
INSERT INTO [스키마.]테이블명
VALUES(값1,값2,…);
```
```sql
--INSERT 기본형태


-- 각 컬럼의 타입에 맞게 삽입해야함.
INSERT INTO ex3_1 (col1, col2, col3)
VALUES ('ABC', 10, 30); --타입에 오류

CREATE TABLE ex3_2 (
        emp_id      NUMBER,
        emp_name    VARCHAR2(100)
);

--INSERT ~ SELECT 형태
INSERT INTO ex3_2(emp_id,emp_name)
SELECT employee_id, emp_name
FROM employees
WHERE salary > 5000;
```


3. UPDATE문
- 테이블에 있는 기존 데이터를 수정할 때 사용하는 문장
```sql
UPDATE[스키마.]테이블명
SET 컬럼1 = 변경값1,
         컬럼2 = 변경값2,

WHERE 조건;
```
```sql
--전체 업데이트
update ex3_2
set emp_name = 'nick'
;

--특정조건 데이터만 업데이트
update ex3_2
set emp_name = 'nick'
where emp_id = '202';


update ex3_1
set col2 = '2', --- 여러 컬럼 업데이트 컬럼, 컬
    col3 = SYSDATE;
```
```sql
SELECT *
FROM INFO;

update INFO
set HOBBY = 'MOVIE'
where NM = '사승원';
--되돌리고 싶으면 rollback
```

4. DELETE, ROLLBACK
- 테이블에 있는 데이터를 삭제할 때 DELETE문을 사용한다.
```sql
select *
FROM ex3_2;
DELETE ex3_2 -- 전체 삭제
rollback -- 롤백

DELETE ex3_2
WHERE emp_id =201; 조건에 맞는 데이터 삭제

select ex3_2
```

5. COMMIT : 변경한 데이터를 데이터베이스에 마지막으로 반영하는 역할

6. 의사컬럼 : 테이블의 컬럼처럼 동작하지만 실제로 테이블에 저장되지는 않는 컬럼
```sql
select      rownum
        , a.*
from employees a;
```

7. 연산자
```sql
--수식연산자
SELECT employee_id
    , emp_name
    , salary * 10  -- +, -, /, *
FROM employees;

--문자연산자
SELECT'[' || employee_id || ']' || emp_name AS employee_ info
FROM employees;

--논리연산자 : >, <, >=, <=, =, <>, !=, ^=
SELECT * FROM employees WHERE salary = 2600 ; -- 같다
SELECT * FROM employees WHERE salary <> 2600 ; -- 같지 않다
SELECT * FROM employees WHERE salary != 2600 ; -- 같지 않다
SELECT * FROM employees WHERE salary < 2600 ; -- 미만
SELECT * FROM employees WHERE salary > 2600 ; -- 초과
SELECT * FROM employees WHERE salary <= 2600 ; -- 이하
SELECT * FROM employees WHERE salary >= 2600 ; -- 이상

--부서가 30이 아닌것(<>,!=,^=)
SELECT employee_id
      ,emp_name
      ,salary
      ,department_id
FROM employees
WHERE department_id ^= 30;
--WHERE department_id <>30 ;  --!= ^= 동일하게 조회 됨
```
오늘의 문제
1. products 테이블에서
상품최저 금액(prod_min_price)이 30원 이상 50원 미만의 상품명을 조회하세요

2. products 테이블에서 하위카테고리가 cd_rom이고 상품최저금액이 35보다 크고 상품최저금액이 40보다 작은 상품명을 조회하세요
```sql
SELECT PROD_NAME
FROM PRODUCTS
WHERE PROD_MIN_PRICE <=30 ;
and PROD_MIN_PRICE <50 ;
 

SELECT *FROM PRODUCTS WHERE PROD_MIN_PRICE <35 ;
SELECT *FROM PRODUCTS WHERE PROD_MIN_PRICE >40 ;


SELECT *FROM PRODUCTS where prod_subcategory ='CD-ROM'; <35 ;
SELECT *FROM PRODUCTS where prod_subcategory ='CD-ROM'; > 40 ;
```


9. 표현식

```sql

--표현식 case문
-- CASE WHEN 조건1 THEN 값
--      WHEN 조건1 THEN 값2
--      ELSE 기타 값
-- END


SELECT EMPLOYEE_ID
    ,SALARY
    ,CASE WHEN SALARY <= 5000 THEN 'C등급'
          WHEN SALARY > 5000 AND SALARY <= 15000 THEN 'B등급'
    ELSE 'A등급' --그밖에
    END AS SALARY_GRADE
FROM EMPLOYEES;
```

```sql
/* MEMBER 테이블에서 마일리지 6000 이상 vvip
                            6000 미만 4000 이상 vip
                            4000 미만 2000 이상 gold
                            나머지 silver
                     */


SELECT mem_mileage
      ,mem_name
      ,mem_id
,CASE WHEN mem_mileage >= 6000  THEN 'vvip'
      WHEN mem_mileage < 6000
      and mem_mileage >= 4000  THEN 'vip'
      ELSE 'silver'
END AS mem_id           -- END AS 영문은 따옴표 없이
FROM member;


SELECT *
FROM EMPLOYEES
WHERE department_id IS NULL; -- NULL 이 있는 데이터를 찾을 때 = 안됨

SELECT *
FROM EMPLOYEES
WHERE department_id IS NOT NULL;
    
/*    
SELECT EMPLOYEE_ID
    , SALARY
    FROM EMPROYEES
    WHERE SALARY BETWEEN 2100 AND 2500; -- BETWWEEN 조건1 and 조건2 에 해당하는 것 조회
    
    SELECT employee_id
        , SALATY
    FROM EMPLOYEES
    WHERE SALARY IN(2000, 2100, 2600); -- In에 포함되는 것 조회
*/

---------------------------
SELECT MEM_ID
,MEM_NAME
,MEM_MAIL
,MEM_JOB
FROM MEMBER
WHERE MEM_JOB IN('학생','자영업')  --In은 두 개 다 해당되어야 나
ORDER BY MEM_JOB ;

---------------------------
SELECT MEM_ID
    ,MEM_NAME
    ,MEM_MAIL
    ,MEM_JOB
FROM MEMBER
WHERE MEM_NAME LIKE'%은%';  –%는 앞 혹은 뒤에 뭐가 오던 지정한 문자만 오면 찾아

--------------------------

/*
문제
CUSTOMERS 테이블에서 Barry 또는 Ashley 라는 성을 가진
모든 사람의 이름(컬럼1), 성별(컬럼2), 출생년도(컬럼3), 도시: 주소: 우편변호:  (컬럼4)출력하시오

예시) [Barry Cook][남자][1986][도시 : Ranvensburg 주소 : 87 East Harbor Avenue 우편번호 :40715]
    1. like
    2. case
    3. 문자연산자
    4. birth 어린 순서
    
    */


--------------------------------------------------
    
SELECT CUST_NAME
      ,CASE WHEN CUST_GENDER = 'M' THEN '남자'
            WHEN CUST_GENDER = 'F' THEN '여자'
      END as CUST_GENDERs

,CUST_YEAR_OF_BIRTH             -- || 문자 합치는 것
, '도시:' || CUST_CITY
|| '주소:' || CUST_STREET_ADDRESS
|| '우편번호:' || CUST_POSTAL_CODE  as 도시주소우편번호

FROM CUSTOMERS
WHERE CUST_NAME LIKE'Barry%'
or CUST_NAME LIKE'Ashley%'
ORDER BY CUST_YEAR_OF_BIRTH DESC;  --asc / desc 구분
```
10. 조건식

- BETWEEN n1 AND n2 조건식 : 범위에 해당되는 값을 찾을 때 사용
- IN 조건식 : 조건철에 명시한 값이 포함된 건을 반환하는 ANY와 비슷
- LIKE 조건식 : 문자열의 패턴을 검색할 때 사용하는 조건식.


## 📚 SQL 함수

### 1. 숫자함수
- https://docs.oracle.com/cd/B19306_01/server.102/b14200/functions001.htm#i88893
- ROUND(n,i) : 매개변수 n을 소수점 기준 (i+1)번 째에서 반올림한 결과를 반환
- TRUNC(n1,n2) : 반올림을 하지 않고 n1을 소수점 기준 n2 자리에서 무조건 잘라낸다
```sql
--ABS(n) : 절대값 반환, 두 개의 매개변소나 문자 들어가면 오류
SELECT ABS(-10)  --10
FROM DUAL ;      --DAUL : 임시테이블


--CEIL 함수 매개변수 n과 '같거나' 가장작은 정수를 반환
SELECT CEIL(10.00) --10
    ,  CEIL(10.01) --11
    ,  CEIL(11.01) --12
FROM DUAL;


/*
ROUND(n, i)
매개변수 n을 소수점 기준 (i+1)번째에서 반올림한 결과를 반환
n이 0일 때 i 입력된 숫자에 상관없이 0을 반환하며,
i가 음수이면 소수점을 기준으로 왼쪽 i번째에서 반올림이 일어난다.
*/

SELECT ROUND(10.154)        --10
     , ROUND(10.541)        --11
     , ROUND(10.541, 1)     --10.5
     , ROUND(11.561, -1)    --10
FROM DUAL;

/* TRUNC(n,i) 디폴트 i = 0
매개변수 n을 소수점 기준(i+1) 번째에서 버림한 결화를 반환
n이 0 일때 i 입력된 숫자에 상관없이 0을 반환하며,
i가 음수이면 소수점을 기준으로 왼쪽 i번째에서 반올림이 일어난다.
*/
SELECT TRUNC(10.154)        --10
     , TRUNC(10.541)        --10
FROM DUAL;


/*
POWER(n2, n1)
n2를 n1 제곱한 결과를 반환한다.
*/

-- MOD(m,n) m을 n으로 나누었얼 때 나머지를 반환
SELECT MOD(19, 4)
     , MOD(18, 2)
FROM DUAL;
--SORT n의 제곱근을 반환한다.
SELECT SORT(4)
     , SORT(5)
FROM DUAL;
```

### 2. 문자함수
```sql
-- 문자열 함수

SELECT LOWER(EMP_NAME) -- 소문자로
     , UPPER(EMP_NAME) -- 대문자로   
     ,EMP_NAME
FROM EMPLOYEES ;

SELECT *
FROM EMPLOYEES
WHERE upper(EMP_NAME) LIKE 'Donal%';

SELECT *
FROM EMPLOYEES
WHERE LOWER(EMP_NAME) LIKE LOWER('DONALD%%');

SELECT INITCAP('ABCDEF ASDF') -- 앞글자만 대문자로
FROM DUAL;


/*
SUBSTR(char, pos, len)자를 문자열인 char 의 pos 번째 문자부터
len 길이 만큼 자른뒤 반환
pos 값으로 0이 오면 디폴트 값인 1, 즉 첫번째 문자를 가르키며, 음수가 오면
char 문자열 맨 끝에서 시작한 상대적 위치를 의미한다.
len 값이 생략되면 pos번째 문자부터 나머지 모든 문자를 반환
*/
SELECT SUBSTR('ABCDEFG', 1, 4) AS "1자리 부터 4자리"
     , SUBSTR('ABCDEFG', 2)     AS "2자리 부터 끝까지"
     , SUBSTR('ABCDEFG', -4, 2) AS "뒤번2자리만큼"
     , SUBSTR('ABCD',1, 2)     AS "byte2"
     , SUBSTR('홍길동이', 1, 4)     AS "byte2한글"
FROM DUAL;

-- TRIM 양쪽 공, LTRIM 왼쪽 공백, RIRTM 오른쪽 공백 제거
SELECT TRIM(' A ')
    ,LTRIM(' A ')
    ,RTRIM(' A ')
FROM DUAL;
```
