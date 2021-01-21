# 🏄‍♂️SQL(Next IT)

## 📚1장. 데이터베이스를 구성하는 객체 이해

### 1. 데이터베이스 객체

- 데이터베이스 객체란?

#### (1) 테이블

- 제약조건 : 컬럼에 대한 속성 형태로 정의하지만 엄연히 오라클 데이터베이스 객체 중 하나이며 데이터 무결성을 보장하기 위한 용도로 사용된다.
- NOT NULL(NN) : 열은 null값을 포함할 수 없습니다.
- UNIQUE KEY(UK) : 테이블의 모든 행을 유일하게 하는 값을 가진 열(null 허용)
- PRIMARY KEY(PK) : 유일하게 테이블의 각행을 식별(NOT NULL꽈 UNIQUE 조건을 만족)
- FOREIGN KEY(FK) : 열과 참조된 열 사이의 외래키 관계를 적용하고 설정합니다.
- CHECK(CK) : 참이어야 하는 조건을 지정함(대부분 업무 규칙을 설정)


#### (2) 테이블 Table.제약조건
- NULL : 값이 없음을 의미함, 별도로 지정하지 않으면 해당 컬럼은 NULL을 허용한다, NULL을 허용하지 않으려면 NOT NULL 구문을 명시해야 한다.
- NOT NULL : NOT NULL 제약조건을 명시하면 해당 컬럼에는 반드시 데이터를 입력해야 한다.

```sql
/*
table 테이블

1. 테이블명 칼럼명의 최대 크기는 30 바이트

2. 테이블명 컬럼명으로 예약어는 사용할 수 없다.

3. 테이블명 컬럼명으로 문자, 숫자,_,&,# 사용할 수 있지만 첫글자는 문자만 올 수 있다.

4. 한 테이블에 사용가능한 컬럼은 최대 255개까지다.

*/
```
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

### 2. 뷰  View
#### (1) 단순 뷰
- 하나의 테이블로 생성
- 그룹 함수의 사용이 불가능
- distinct(중복 방지)사용이 불가능
- insert/update/delete 등 사용가능

####(2)  복합 뷰
- 여러 개의 테이블로 생성
- 그룹함수의 사용이 가능
- distinct(중복 방지)사용이 불가능
- insert/update/delete 등 사용가능
```sql
--------------------------------------------------------------------
grant create view to java ;

--위의 쿼리를 자주 사용한다면, 사용할 때마다 SQL 문을
--매번 작성해야 하는 불편함이 있다.
--이때 뷰를 생성해서 참조하면 편리함

CREATE OR REPLACE VIEW emp_dept_v1 as
select a.employee_id, a.emp_name, a.department_id,
       b.department_name
from employees a,
     departments b
where a.department_id = b.department_id;

select *
from emp_dept_v1 ;

--------------------------------------------------------------------
-- study 계정에서도 emp_dept_v1 테이블 볼 수 있는 권한주는 것
grant select on emp_dept_v1 to study ;
commit;
-- java 계정에 있는 emp_dept_v1 뷰 조회
select *
from java.emp_dept_v1 ;
--------------------------------------------------------------------
-- 뷰 삭제
--------------------------------------------------------------------
drop view emp_dept_v1 ;
```
### 3. 인덱스 Index

- 테이블에 있는 데이터를 빨리 찾기 위한 용도의 데이터 베이스 객체
- 데이터가 많지 않을 때는 비효율적
- 1개 이상의 컬럼으로 인덱스를 만들 수 있음
- 인덱스 자체에 키와 매핑 주소 값을 별도로 저장한다,
- 따라서 테이블 입력, 삭제, 수정할 때 인덱스에 저장된 정보도 수정된다.
```sql
--------------------------------------------------------------------
-- ### ALL_TAB_COLUMNS : 테이블 별 컬럼정보 조회
SELECT  *
FROM ALL_TAB_COLUMNS
WHERE OWNER = 'JAVA'
AND TABLE_NAME = 'INFO';

SELECT *
FROM USER_TAB_COMMENTS
WHERE TABLE_NAME = 'INFO' ;  
--------------------------------------------------------------------

SELECT *
FROM ALL_IND_COLUMNS
WHERE index_owner = 'java';

--------------------------------------------------------------------
-- ### 시노님 synonym (동의어)
-- 개발의 용이함. 보안의 목적으로 시노님을 준다.
-- public : 모든 사용자 접근
-- private 특정 사용자만
-- public 시노님은 DBA 권한이 있는 사용자만 생성, 삭제가 가능하다.

--------------------------------------------------------------------

drop public synonym emss;

select *
from java.syn_channel ;

select *
from syn_channel ;

select *
from emss ;

--------------------------------------------------------------------

-- public synonym 생성 삭제는 DBA 권한만 가능

grant select on syn_channel to study;

grant select on emss to study ;

grant create synonym to java ;

grant create public synonym to java ;

create public synonym emss
for employees ;

--------------------------------------------------------------------

CREATE OR REPLACE SYNONYM syn_channel
FOR channels ;

create public synonym emss
for employees ;

--------------------------------------------------------------------
### 시퀀스 Sequence
-- - 자동 순번을 반환하는 데이터베이스 객체
CREATE SEQUENCE my_seq1
INCREMENT BY 1  --증강숫자
START WITH  1    --시작숫자
MINVALUE  1    --최소값(시작숫자와 같아야함)
MAXVALUE   1000   --최대값(시작숫자 보다 커야함)
NOCYCLE         --디폴트 값으로 최대나 최소값에 도달하면 생성 중지
NOCACHE   ;      --디폴트로 메모리에 시퀀스 값을 미리 할당해 놓지 않으며 디폴트 값은 20
--------------------------------------------------------------------
SELECT my_seq1.CURRVAL  --현재 시퀀스 값
FROM DUAL;
SELECT my_seq1.NEXTVAL  --증감
FROM DUAL ;

CREATE TABLE EX11_1 (
    COL1 NUMBER
    );

CREATE SEQUENCE EX11_1
INCREMENT BY    1
START WITH      1
MINVALUE        1
MAXVALUE        1000   
NOCYCLE         
NOCACHE   ;      


INSERT INTO EX11_1 (col1) VALUES (my_seq1.NEXTVAL);

SELECT *
FROM EX11_1 ;

SELECT NVL(MAX(COL1),0) + 1
FROM EX11_1;
    
delete EX11_1;   

INSERT INTO EX11_1(col1)
VALUES ((SELECT NVL(MAX(COL1),0) + 1
        FROM EX11_1)
        );


SELECT my_seq1.CURRVAL  --합
FROM DUAL;

--------------------------------------------------------------------


ALTER SEQUENCE my_seq1 INCREMENT BY -8; -- 현재 시퀀스 값 -를 하고
SELECT my_seq1.NEXTVAL                  -- 시퀀스 실행
FROM DUAL ;
ALTER SEQUENCE my_seq1 INCREMENT BY 1;  --다시 증가값 수정

--------------------------------------------------------------------
-- 최소값 1 , 최대값99999999, 1000부터 시작해서 2씩 증가하는
-- ORDERS_SEQ 라는 시퀀스를 만들어보자

CREATE TABLE ORDERS_SEQ4 (
    COL1 NUMBER ) ;

CREATE SEQUENCE ORDERS_SEQ4
INCREMENT BY    2
START WITH      1000
MINVALUE        1
MAXVALUE        99999999   
NOCYCLE         
NOCACHE   ;    

SELECT ORDERS_SEQ4.NEXTVAL   -- 시퀀스 실행
FROM DUAL ;


SELECT ORDERS_SEQ4.CURRVAL  --현재 시퀀스 값
FROM DUAL;
-- 테이블이 아니라 시퀀스에 데이터가 생성됨
--------------------------------------------------------------------

```





## 📚 2장. SQL 기본

### 1. **SELECT문**
- 가장 기본적인 SQL문으로 테이블이나 뷰에 있는 데이터를 조회할 때 사용
```sql
SELECT*혹은 컬럼
FROM[스키마.]테이블명 혹은 [스키마.]뷰명

WHERE 조건
ORDER BY 컬럼;
```
- **WHERE**절(어디에서) 어떤 데이터를 가져올 것인가.
- **ORDER BY** 정렬이 필요할 때(DESC내림차순, ASC오름차)

```sql
SELECT *
FROM DEP , EMP
where dep.deptno = emp.dno;

------------ ALTER 테이블 수정 -----------

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


### 2. **INSERT**문
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


### 3. **UPDATE**문
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

### 4. **DELETE, ROLLBACK**
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

### 5. **COMMIT** : 변경한 데이터를 데이터베이스에 마지막으로 반영하는 역할

### 6. 의사컬럼 : 테이블의 컬럼처럼 동작하지만 실제로 테이블에 저장되지는 않는 컬럼
```sql
select      rownum
        , a.*
from employees a;
```

### 7. 연산자
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


### 8. 표현식

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
### 9. 조건식

- BETWEEN n1 AND n2 조건식 : 범위에 해당되는 값을 찾을 때 사용
- IN 조건식 : 조건철에 명시한 값이 포함된 건을 반환하는 ANY와 비슷
- LIKE 조건식 : 문자열의 패턴을 검색할 때 사용하는 조건식.


## 📚 3장. SQL 함수

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

--REPLACE(char, search_str, replace_str)
--char에서 search_str 찾아서 replace_str로 변환
SELECT REPLACE(MEN_MAIL, '@','====')
    , MEM_MAIL
FROM MEMBER;

SELECT REPLACE('a b c d ab','ab','xz')
FROM dual ;

--TRANSLATE(char, search_str, replace_str)
--REPLACE 유사하지만 문자열 자체가 아닌 한글자씩 매핑
SELECT TRANSLATE(MEM_MAIL,'@','====')
FROM MEMBER;

SELECT TRANSLATE('a b c d ab','ab','xz')
FROM dual;

--문제1
--customers 테이블에 있는 고객 전화 번호 (phone_number)
--123-4564-4564 형태에서 123/4569/4567 로 바꾸어 출력하시오

SELECT CUST_NAME
,REPLACE(CUST_MAIN_PHONE_NUMBER,'-','/') as Phonenumber
FROM CUSTOMERS ;

SELECT CUST_NAME
,REPLACE(CUST_MAIN_PHONE_NUMBER,' ','') as Phonenumber
FROM CUSTOMERS ;               --공백제거하고 싶을 때


--------------------------------------------------------------

--INSTR(n1, n2, n3, n4)n1에서 n2를 n3, n4 디폴트(1)n3:찾을 시작점, n4 몇번째
SELECT INSTR('내가 만약 외로울 때면,
                            내가 만약 괴로울 때면,
                            내가 만약 즐거울 때면', '만약') AS INSTR1,
       INSTR('내가 만약 외로울 때면,
                            내가 만약 괴로울 때면,
                            내가 만약 즐거울 때면', '만약', 5) AS INSTR2, -- 5번째부터 찾아라
       INSTR('내가 만약 외로울 때면,
                            내가 만약 괴로울 때면,
                            내가 만약 즐거울 때면', '만약', 5, 2) AS INSTR3 --5번째에서부터 찾는데,
                                                                         --2번째 만약을 찾아라
FROM dual;

SELECT mem_mail
    ,INSTR(mem_mail,'@')
    ,substr(mem_mail,1, INSTR(mem_mail, '@')-1) as id   -- 1은 1번째 나오는 것부터 찾는 것
    ,substr(mem_mail,INSTR(mem_mail, '@')+1) as domain  -- -1 +1이 있는 건 @ 앞뒤로 잘라야 돼서
FROM member;
```
### 3. 날짜함수 
```sql
-----------------------------날짜함수 ---------------------------------

--날짜 함수
-- ADD_MONTH(DATE,더할 개월 수)
SELECT ADD_MONTHS(SYSDATE, 1)       --21/02/08
     , ADD_MONTHS(SYSDATE,-1)       --20/12/08
FROM dual ;

--MONTHS_BETWEEN 월을 비교
SELECT MONTHS_BETWEEN(SYSDATE, ADD_MONTHS(SYSDATE,-2)) as "월비교"
     , MONTHS_BETWEEN(ADD_MONTHS(SYSDATE,-2),SYSDATE) as "월비교2"
     , SYSDATE
     , ADD_MONTHS(SYSDATE,-2)
FROM DUAL;

-- LAST_DAY 마지막 일자 리턴
-- NEXT_DAY 다음주의 파라미터로 받은 요일 리턴
SELECT ADD_MONTHS(SYSDATE,0)                 --21/01/08
     , NEXT_DAY(SYSDATE, '수요일')            --21/01/13
     , LAST_DAY(SYSDATE) - SYSDATE || '남음'  --23 남음
     , LAST_DAY(SYSDATE)                      --21/01/31
FROM DUAL;

                            
SELECT ROUND(to_date('20200923'), 'month')  --20/10/01 반올림한 날짜를 반환
     , TRUNC(to_date('20200923'), 'month')  --20/09/01 잘라낸 날짜를 반환
FROM DUAL;
```
### 4. 변환함수 
```sql
-----------------------------변환함수 ---------------------------------

--변환함수
--TO_CHAR 넘버, 날짜 형태의 데이터 타입을 문자형태 데이터 타입을 치환
SELECT TO_CHAR(200984123, '999,999,999') --200,984,123
     , TO_CHAR(SYSDATE, 'YYYY-MM-DD')    --2021-01-08
     , TO_CHAR(SYSDATE, 'YYYY/MM/DD')    --2021/01/08
     , TO_CHAR(SYSDATE, 'YYYY-MM-DD HH24:MI:SS') --2021-01-08 15:23:46(12H)
     , TO_CHAR(SYSDATE, 'YYYY-MM-DD HH12:MI:SS') --2021-01-08 03:27:38(24H)
     , TO_CHAR(SYSTIMESTAMP, 'YYYY-MM-DD HH:MI:SS:FF') --2021-01-08 03:23:46:377763(밀리sec까지)
     , TO_CHAR(SYSDATE, 'D') --6(일요일이1 월요일은2 오늘은 금요일이니 6)
FROM DUAL;

--TO NUMBER
SELECT TO_NUMBER('20140101')
FROM DUAL;

---------------------------------------------------------------
--TO DATE
SELECT TO_DATE('20140101', 'YYYY-MM-DD')
     , TO_DATE('20140101 13:44:55', 'YYYY-MM-DD HH24:MI:SS')
FROM DUAL;
--'20140101', 'YYYY-MM-DD' 와 '20140101 13:44:55', 'YYYY-MM-DD HH24:MI:SS' 의 차이
CREATE TABLE TB2 (
    COL1 DATE
  , COL2 DATE
);
INSERT INTO TB2 VALUES(
       TO_DATE('20140101', 'YYYY-MM-DD')
     , TO_DATE('20140101 13:44:55', 'YYYY-MM-DD HH24:MI:SS'));
     
SELECT TO_CHAR(COL1, 'YYYY-MM-DD HH24:MI:SS') -- 2014-01-01 00:00:00
     , TO_CHAR(COL2, 'YYYY-MM-DD HH24:MI:SS') -- 2014-01-01 13:44:55
FROM TB2;
--------------------------------------------------------------

--정렬시 주의
CREATE TABLE TB1 (COL1 VARCHAR2(20));
INSERT INTO TB1 VALUES ('1000');
INSERT INTO TB1 VALUES ('9000');
INSERT INTO TB1 VALUES ('90');
INSERT INTO TB1 VALUES ('910');
SELECT *
FROM TB1
ORDER BY TO_NUMBER(COL1) DESC;  --TO_NUMBER로 문자타입을 숫자타입으로 변경해야 정렬됨
SELECT *
FROM TB1
ORDER BY COL1 DESC;             --위에서 숫자타입으로 변경해서 TO_NUMBER 없이 정렬됨

--------------------------------------------------------------
SELECT 'ABC'
      , COL1
FROM TB1;
 , ADD_MONTHS(SYSDATE,1 - HIRE_DATE)  as 근속년수
--------------------------------------------------------------

--현재 일자를 기준으로 사원테이블의 입사일자(hire_date)를 참조해서
--근속년수가 22년 이상인 사원을 다음과 같은 형태로 결과를 출력하시오

SELECT EMPLOYEE_ID AS "사원번호"
    , EMP_NAME  as "사원명"
    , HIRE_DATE as "입사일자"
    , ADD_MONTHS(SYSDATE,0)  as 근속년수

select ROUND((SYSDATE - HIRE_DATE)/365) -- 근속년도
from employees;

select EMPLOYEE_ID AS "사원번호"
    , EMP_NAME  as "사원명"
    , HIRE_DATE as "입사일자"
    , ROUND((SYSDATE - HIRE_DATE)/365) as 근속년수
from employees
WHERE ROUND((SYSDATE - HIRE_DATE)/365) >= 22
ORDER BY HIRE_DATE ASC ;


select EMPLOYEE_ID AS "사원번호"
    , EMP_NAME  as "사원명"
    , HIRE_DATE as "입사일자"
    ,((SYSDATE - HIRE_DATE)/365) as 근속년수
from employees
WHERE ((SYSDATE - HIRE_DATE)/365) >= 22
ORDER BY HIRE_DATE ASC ;
```

>--------------------------------------------------------------
>**SELECT문 실행순서**
>from -> where -> group by -> having -> select절 -> order by
>--------------------------------------------------------------

### 5. NULL 관련 함수
```sql
--NVL(n1, n2)   n1이 null 일 경우 n2로 변환

SELECT employee_id
     , NVL(commission_pct,0)
     , salary * NVL(commission_pct,0)
     , salary
     , commission_pct
FROM employees;


SELECT employee_id
     , NVL(commission_pct,0)
     , salary * NVL(commission_pct,0)
FROM employees
WHERE NVL(commission_pct,0) < 0.2;

--------------------------------------------------------------
--DECODE(expr,search1,result,search2,result2,...default)
--expr과 search1을 비교해 두 값이 같으면 result1을,
--같지 않으면 search2와 비교해 값이 같으면 result2를 반환
--최종적으로 같은 값이 없으면 default값을 반환한다.
SELECT DECODE(CHANNEL_ID, 3,'DIRECT'
                         ,9,'INDIRECT'
                         ,'OTERS')
    ,PROD_ID
FROM SALES ;

SELECT DECODE(CUST_GENDER, 'F', '여자'
                         , 'M', '남자'
                         , '??')
FROM CUSTOMERS;
--------------------------------------------------------------

/* 오늘의 문제
고객 테이블(CUSTOMERS)'에는
고객의 출생년도(cust_year_of_birth) 컬럼이 있다.
현재일 기준으로 이 컬럼을 활용해 30대, 40대, 50대를 구분해 출력하고,
나머지 연령대는 '기타'로 출력하는 쿼리를 작성해보자 */
-----------------------------------------내오답----------------------------------------------
SELECT CUST_NAME AS "이름"
     , CUST_YEAR_OF_BIRTH AS "생년"
     , (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH)
     ,CASE WHEN (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH) >= 50  THEN '50대'
      WHEN (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH) < 50
      and (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH) >= 40  THEN '40대'
      ELSE (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH) '30대'
END AS (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH)        
FROM CUSTOMERS ;


,CASE WHEN (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH) >= 50  THEN "30대"
      WHEN (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH) < 50
      and (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH) >= 40  THEN "40대"
      ELSE (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH) "30대"
      
SELECT (TO_char(sysdate, 'YYYY') - CUST_YEAR_OF_BIRTH)/10
from CUSTOMERS;

-----------------------------------------정답----------------------------------------------
SELECT CUST_NAME
     , CUST_YEAR_OF_BIRTH
     , TO_CHAR(SYSDATE, 'YYYY')-CUST_YEAR_OF_BIRTH
     , DECODE((TRUNC(TO_CHAR(SYSDATE, 'YYYY')-CUST_YEAR_OF_BIRTH, '-1')), '30', '30대', '40', '40대', '50', '50대', '기타')
FROM CUSTOMERS;
-------------------------------------------------------------------------------------------
<<<<<<< HEAD
```

## 📚 4장. 그룹쿼리와 집합 연산자 

### 1. 기본 집계함수

-  COUNT, AVG, MIN, MAX, VARIANCE, STDDEV

```sql
------------------------- 1.기본 집계 함수 -----------------------   
SELECT COUNT(*) --쿼리 결과 전체수 반환 
FROM MEMBER;

SELECT MAX(MEM_MILEAGE) -- 최댓값
     , MIN(MEM_MILEAGE) -- 최솟값
     , AVG(MEM_MILEAGE) -- 평균값 
     , SUM(MEM_MILEAGE) -- 합 
FROM MEMBER;


--집계함수 
--count 
SELECT COUNT(*)                         -- NULL 포함
     , COUNT(DEPAPRMENT_ID)             -- defualt all
     , COUNT(ALL DEPARTMENT_ID)         -- 중복 포함
     , COUNT(DISTINCT DEPARTMENT_ID)    -- 중복 제거 
FROM EMPLOYEES;

--DISTINCT 중복 제거 
SELECT DISTINCT COUNTRY_REGION
FROM COUNTRIES ; 

-----------------------------------------------------------------
--문제1 : EMPLOYEES 부서 50의 salary의 최소값, 최대값, 인원수 를 출력하시오.

SELECT MAX(SALARY) 
     , MIN(SALARY)
     , COUNT(SALARY)
FROM EMPLOYEES 
WHERE DEPARTMENT_ID = 50 ;    --컬럼에 있는 데이터 가져올 때 WHERE

-----------------------------------------------------------------
SELECT ROUND(AVG(COMMISSION_PCT),2)
     , ROUND(AVG(NVL(COMMISSION_PCT,0)),2) --NULL이 있는 컬럼은 주의!!
FROM EMPLOYEES;

-----------------------------------------------------------------

SELECT VARIANCE(SALARY) --분산
     , STDDEV(SALARY)   --표준편차
FROM employees; 
```
### 2. GROUP BY, HAVING
```sql
-------------------------2.GROUP BY 와 HAVING-----------------------  
--GROUP BY 와 HAVING
--GROUP BY 절에는 집계함수 이외의 SELECT 절에 있는 컬럼을 써야함. 
--GROUP BY 구문은 WHERE 과 ORDER BY 절 사이에 위치한다.(특정 그룹을 묶어 데이터 집계가능)


SELECT DEPARTMENT_ID
     , COUNT(*)
     , SUM(SALARY)
FROM employees
GROUP BY department_id  --부서별 
ORDER BY 1 ; 


SELECT MEM_JOB
     , COUNT(*)
FROM MEMBER
GROUP BY MEM_JOB 
ORDER BY COUNT(*) DESC;

-----------------------------------------------------------------
--문제2 : CART 테이블에서 member별 cart_prod별 cart_qrt 의 총합을 구하고 
--총합이 많은 순으로 출력하시오 

SELECT CART_MEMBER
     , CART_PROD
     , SUM(CART_QTY)
FROM CART
GROUP BY CART_MEMBER, CART_PROD --SELECT에 쓴 절은 GROUP BY 에도 반드시 써야함 (집계함수 제외)
ORDER BY CART_MEMBER,  SUM(CART_QTY) DESC;

-----------------------------------------------------------------
--2013년도의 기간별, 지역별 총매출 잔액율을 출력시오(kor_loan_status)

--내가 쓴 답 
SELECT TRUNC(PERIOD/100) AS 기간
     , REGION
     , SUM(LOAN_JAN_AMT) as loan 
FROM kor_loan_status
WHERE TRUNC(PERIOD/100) = 2013  
GROUP BY TRUNC(PERIOD/100), REGION
ORDER BY 3 DESC;    -- ORDER BY '숫자'는 컬럼 줄 기준으로..

-----------------------------------------------------------------
--정답 
SELECT substr(period,1,4)
     ,region
     , sum(loan_jan_amt)
FROM kor_loan_status
WHERE PERIOD LIKE '2013%'
GROUP BY substr(period,1,4)
       , REGION
HAVING sum(loan_jan_amt) > 100000
ORDER BY 3 DESC ;

-----------------------------------------------------------------
--대출 총합이 100,000(HAVING 활용)

SELECT substr(period,1,4)
     ,region
     , sum(loan_jan_amt)
FROM kor_loan_status
WHERE PERIOD LIKE '2013%'
GROUP BY substr(period,1,4)
       , REGION
ORDER BY 3 DESC ;

-------------------------------------------------------------

-- 문제3 : 부서별 총급여, 사원수, 평균급여 조회(반올림 소수점 2자리까지)
-- 부서 ID null 제외 정렬은 부서 오름차순

SELECT DEPARTMENT_ID
     , SUM(SALARY)              -- 총급여 
     , COUNT(*)                 -- 사원수 
     , ROUND(AVG(SALARY),2)     -- 평균 급여 소수정 2자리까지 반올림
FROM EMPLOYEES
WHERE DEPARTMENT_ID IS NOT NULL -- NULL 제외 
GROUP BY DEPARTMENT_ID          -- GROUP BY 절에는 집계함수 제외 
ORDER BY DEPARTMENT_ID ;
-------------------------------------------------------------
--HAVING : 집계 결과에서 조건을 넣고 싶을 때 사용 
--실행순서 : FROM -> GROUP BY -> HAVING -> SELECT -> ORDER BY

SELECT DEPARTMENT_ID
     , SUM(SALARY)              -- 총급여 
     , COUNT(*)                 -- 사원수 
     , ROUND(AVG(SALARY),2)     -- 평균 급여 소수정 2자리까지 반올림
FROM EMPLOYEES
WHERE DEPARTMENT_ID IS NOT NULL -- NULL 제외 
GROUP BY DEPARTMENT_ID          -- GROUP BY 절에는 집계함수 제외 
HAVING count(*) > 10            -- 집계 결과에서 조건을 넣고 싶을 때 HAVING 
ORDER BY DEPARTMENT_ID ;
<<<<<<< HEAD
```
### 3. ROLLUP
```sql
-------------------------3.ROLLUP-----------------------  
--ROLLUP : 총합을 표현 

SELECT PERIOD 
     , SUM(LOAN_JAN_AMT) as loan 
FROM kor_loan_status
GROUP BY ROLLUP(PERIOD) -- period 별 합과 마지막 총합 출력 
ORDER BY 1 ;


select *
FROM kor_loan_status ;

-------------------------------------------------------------
/*
오늘의 문제
입사년도별 총급여, 사원수를 조회하는데
입사한 사원수가 10명 초과였던 년도만 조회하시오 
마지막 로우는 총합이 나오도록 
*/

-- 첫번째로 생각한 답 
SELECT SUBSTR(HIRE_DATE, 1, 2)  AS 연도 
     , SUM(SALARY)              as 총급여
     , COUNT(*)                 as 사원수 
FROM employees 
GROUP BY ROLLUP(SUBSTR(HIRE_DATE, 1, 2))
HAVING COUNT(*) > 10
ORDER BY 1 ;

-- 두 번째로 생각한 답 
SELECT TO_CHAR(HIRE_DATE, 'YYYY')  AS 연도 
     , SUM(SALARY)              as 총급여
     , COUNT(*)                 as 사원수 
FROM employees 
WHERE HIRE_DATE IS NOT NULL
GROUP BY ROLLUP(TO_CHAR(HIRE_DATE, 'YYYY'))
HAVING COUNT(*) > 10
ORDER BY 1 ;

------------------------------------------------------------- 
-- 보이기는 yy로 보이지만 실제 데이터에는 YYYY-MM-DD-HH-MM-SS 까지 다있다. 
SELECT TO_CHAR(HIRE_DATE,'YYYY-MM-DD')
FROM EMPLOYEES ;
------------------------------------------------------------- 
/*  문제1
부서별 평균 급여를 구하시오(소수점 3째 자리에서 반올림)
부서 ID 가 NULL 인 데이터 미포함 인원수 5명 이상인 부서 중에서
평균 급여가 높은 순서로 정렬 (employees 테이블)
부서 ID, 인원, 급여
*/

SELECT DEPARTMENT_ID        AS 부서ID
     , COUNT(*)             as 인원
     , ROUND(AVG(SALARY),2) as 급여
FROM EMPLOYEES
GROUP BY department_id
HAVING COUNT(*) >= 5   -- ~이상인 부서중엔 HAVING
ORDER BY 3 DESC;
---------------------------------------------------------
```
### 4. 집합연산자(UNION / UNION ALL / MINUS / INTERSECT)
```sql
--UNION 합집합(중복제거) UNION ALL 전체합
--MINUS 차집합 INTERSECT 교집합 - 컬럼의 수와 타입이 맞아야함.

SELECT SEQ, GOODS
FROM exp_goods_asia
WHERE country = '한국'
UNION ALL -- UNION / UNION ALL / MINUS / INTERSECT
SELECT 1, GOODS -- 컬럼을 숫자로 써도됨.
FROM exp_goods_asia
WHERE country = '일본';

----------------------------------------------------------

--ROLLUP없이 만드는법 UNION으로 만드는 법
--(결과가 완전히 동일하진 않다.)

SELECT TO_CHAR(HIRE_DATE, 'YYYY')  AS 연도
     , SUM(SALARY)              as 총급여
     , COUNT(*)                 as 사원수
FROM employees
GROUP BY TO_CHAR(HIRE_DATE, 'YYYY')
UNION
SELECT '총합'
     , SUM(SALARY)
     , COUNT(*)
FROM employees;
```

## 📚 5장. 조인과 서브쿼리

### 1. 조인
- 관계형 데이터베이스에서 SQL을 이용해 관계를 맺는 방법이 조인이다.
- 테이블 간의 연결고리로 관계를 맺고 데이터를 추출한다.

### 2. 내부 조인(INNER JOIN)과 외부조인(EQUI-JOIN)
```sql

-- 내부 조인(INNER JOIN), 동등 조인(EQUI-JOIN)
SELECT *
FROM MEMBER
   , CART
WHERE MEMBER.MEM_ID = CART.CART_member
AND MEMBER.MEM_ID = 'a001';

-----------------------------------------------------------
--ex)
--조인이 되려면 departments.department_id 라는 데이터가 각각의 테이블에 있어야 됨

SELECT employees.employee_id
     , employees.emp_name
     , employees.salary
     , employees.department_id
     , departments.department_name
FROM employees
   , departments
WHERE employees.department_id = departments.department_id
AND employees.EMPLOYEE_ID ='198';

-----------------------------------------------------------
--둘다 있는 컬럼들은 별칭을 무조건 써줘야 한다. 안 써주면 오류생김
--근데 통일성 있게 다른 컬럼들도 써주는 것이 좋다.

SELECT a.employee_id
     , a.emp_name
     , a.salary
     , a.department_id
     , b.manager_id
     , b.department_name
FROM employees a        -- from 절에서 별칭을 지정하면
   , departments b      -- 다른졸에서는 별칭으로만 접근
WHERE a.department_id = b.department_id;    --이퀄조인

-----------------------------------------------------------
/*
외부조인(OUTER JOIN)
조인조건을 만족하는 데이터뿐 아니라, 어느 한 쪽 테이블에 조인 조건에
명시된 컬럼에 값이 없거나(NULL이라도) 해당 로우가 아예 없더라도
데이터를 모두 추출한다. (+)를 뒤에 써준 컬럼에 NULL이 나옴
*/

SELECT COUNT(*)
FROM EMPLOYEES
   , DEPARTMENTS
   , JOB_HISTORY
WHERE employees.department_id = departments.department_id
AND   departments.department_id = JOB_HISTORY.DEPARTMENT_ID ;

-----------------------------------------------------
--테이블 만드는 법..

CREATE TABLE ADDR (
    NM VARCHAR2(30)
    , AGE NUMBER
    , CODE VARCHAR2(30)
    );
    
CREATE TABLE hobby (
     CODE VARCHAR2(30)
     ,code_nm VARCHAR2(30)
    );

-----------------------------------------------------------
--외부조인(OUTER JOIN)

INSERT INTO ADDR VALUES(펭수,ID,A100);

SELECT *
FROM ADDR
    ,HOBBY
WHERE ADDR.CODE(+) = HOBBY.CODE;
-- (+) 있는쪽 데이터가 NULL 생김
-- (+) 없는쪽한테 있는쪽이 맞춰주는 거라고 생각.
-----------------------------------------------------------
```


- 데이터 넣는 방법과 PK..

```sql
ALTER TABLE 학생 ADD CONSTRAINT PK_학생_학번 PRIMARY KEY (학번);
ALTER TABLE 과목 ADD CONSTRAINT PK_과목_과목번호 PRIMARY KEY(과목번호);
ALTER TABLE 교수 ADD CONSTRAINT PK_교수_교수번호 PRIMARY KEY(교수번호);

ALTER TABLE 강의내역 ADD CONSTRAINT PK_강의내역_내역번호 PRIMARY KEY(강의내역번호);
ALTER TABLE 수강내역 ADD CONSTRAINT PK_수강내역_내역번호 PRIMARY KEY(수강내역번호);

     
        ALTER TABLE 수강내역
        ADD CONSTRAINT FK_학생_학번 FOREIGN KEY(학번)
        REFERENCES 학생(학번);
        
        ALTER TABLE 수강내역
        ADD CONSTRAINT FK_과목_과목번호 FOREIGN KEY(과목번호)
        REFERENCES 과목(과목번호);
        
        ALTER TABLE 강의내역
        ADD CONSTRAINT FK_교수_교수번호 FOREIGN KEY(교수번호)
        REFERENCES 교수(교수번호);
        
        ALTER TABLE 강의내역
        ADD CONSTRAINT FK_강의_과목번호 FOREIGN KEY(과목번호)
        REFERENCES 과목(과목번호);
        
        
/*       강의내역, 과목, 교수, 수강내역, 학생 테이블을 만드시고 아래와 같은 제약 조건을 준 뒤
             '학생' 테이블의 PK 키를  '학번'으로 잡아준다
             '수강내역' 테이블의 PK 키를 '수강내역번호'로 잡아준다
             '과목' 테이블의 PK 키를 '과목번호'로 잡아준다
             '교수' 테이블의 PK 키를 '교수번호'로 잡아준다
             '강의내역'의 PK를 '강의내역번호'로 잡아준다.
             '학생' 테이블의 PK를 '수강내역' 테이블의 '학번' 컬럼이 참조한다 FK 키 설정
             '과목' 테이블의 PK를 '수강내역' 테이블의 '과목번호' 컬럼이 참조한다 FK 키 설정
             '교수' 테이블의 PK를 '강의내역' 테이블의 '교수번호' 컬럼이 참조한다 FK 키 설정
             '과목' 테이블의 PK를 '강의내역' 테이블의 '과목번호' 컬럼이 참조한다 FK 키 설정
            각 테이블에 엑셀 데이터를 임포트

    ex) ALTER TABLE 학생 ADD CONSTRAINT PK_학생_학번 PRIMARY KEY (학번);
        
        ALTER TABLE 수강내역
        ADD CONSTRAINT FK_학생_학번 FOREIGN KEY(학번)
        REFERENCES 학생(학번);
        ALTER TABLE 테이블명 MODIFY (컬럼명 테이터타입(데이터길이));
*/

----------------------------------------------------------

SELECT *
FROM 학생 ;

SELECT *
FROM 학생, 수강내역
--Inner join
WHERE 학생.학번 = 수강내역.학번
AND 학생.이름 ='양지운' ;

--Outer join
SELECT *
FROM 학생, 수강내역
where 학생.학번 = 수강내역.학번(+)
and 학생.이름 = '양지운';

--학생들의 수강내역 건수를 조회하시오
--이름, 학번, 수강건수

SELECT 학생.이름
     , 학생.학번
     , COUNT(수강내역.수강내역번호)
FROM 학생, 수강내역
WHERE 학생.학번 = 수강내역.학번(+)
GROUP BY 학생.이름
       , 학생.학번
ORDER by 3 desc;

---------------------------------------
--교수의 강의 건수를 출력하시오
--교수이름, 교수학위, 교수주소, 강의건수

SELECT 교수.교수이름
     , 교수.학위
     , 교수.주소
     , COUNT(강의내역.강의내역번호)
FROM 교수          --SELECT에서 사용한 테이블명
   , 강의내역
WHERE 교수.교수번호 = 강의내역.교수번호(+)
GROUP BY 교수.교수이름
       , 교수.학위
       , 교수.주소  --GROUP BY 는 SELECT에서 집계함수를 제외한 컬럼 모두 써라
ORDER BY 4 desc ;

/*
1. 조인 조건 모두에 (+) 붙여아 함.
2. (+) 붙은 조건과 OR 사용할 수 없다.
3. 한 번에 한 테이블에만 외부조인 가능
*/
SELECT 학생.이름
     , 학생.학번
     , 수강내역.수강내역번호
     , 수강내역.과목번호
     , 과목.과목번호
FROM 학생, 수강내역, 과목
WHERE 학생.학번 = 수강내역.학번(+)
and 수강내역.과목번호 = 과목.과목번호(+) --OR 사용불가능
ORDER by 3 desc;
```
### 3. 서브쿼리 
- 서브쿼리 : SUB QUERY SQL 문장 안에 보조로 사용되는 또 다른 SELECT 문

- 메인 쿼리와 연관성에 따라
  1. 연관성 없는 쿼리, 
  2. 있는 서브쿼리

- 형태에 따라
   1. 일반서브쿼리(SELECT절) --스칼라 서브쿼리
   2. 인라인 뷰(FROM절)
   3. 중첩 쿼리(WHERE절)

#### (1) 일반 서브쿼리(스칼라 서브쿼리)
```sql

1. 일반 서브쿼리(SELECT절에 위치) 스칼라 서브쿼리-select 안에 또 다른 select

select a.emp_name
     , a.employee_id
     , a.department_id
     , (select department_name
        from departments
        where department_id = a.department_id
        ) as 현재101부서
from employees a
where a.employee_id = 101 ;


---------------------------------------
SELECT 학생.이름
     , 학생.학번
     , 수강내역.수강내역번호
     , (SELECT 과목이름
        FROM 과목
        WHERE 과목.과목번호 = 수강내역.과목번호) as 과목이름
FROM 학생
   , 수강내역, 과목
WHERE 학생.학번 = 수강내역.학번(+)
ORDER by 3 desc;
```
 #### (2) 인라인 뷰
```sql
2. 인라인 뷰(FROM 절에 위치)
SELECT *
FROM(
    SELECT
            ROWNUM AS rnum -- 테이블에 없는 걸 넘버링 할 때 사용
            ,a.*
            FROM 학생 a
            ORDER BY a.학번 desc
            )T1 --- 서브쿼리를 하나의 테이블 처럼
WHERE T1.RNUM  BETWEEN 1 and 10 ; --로우넘에서 1~10 사이를 조회

------------------------------------------------------------
SELECT *
FROM(
        SELECT rownum as rnum
        , T1.*
        FROM(
              SELECT 교수.교수이름
                        , 교수.학위
                        , 교수.주소
            , COUNT(강의내역.강의내역번호)
              FROM 교수          --SELECT에서 사용한 테이블명
                 , 강의내역
              WHERE 교수.교수번호 = 강의내역.교수번호(+)
              GROUP BY 교수.교수이름
                     , 교수.학위
                     , 교수.주소  --GROUP BY 는 SELECT에서 집계함수를 제외한 컬럼 모두 써라
              ORDER BY 4 desc
) T1
) T2
WHERE T2.RNUM = 2 ;
```
 #### (3) 중첩쿼리
```sql  

-------------------------------------------------------------

--연결성 없는 서브쿼리 메인쿼리와 연관성이 없고
--3.(중첩쿼리) 서브쿼리에서 평균 급여를 구한뒤 메인 쿼리에 평균 값보다 큰 사원 조회

SELECT count(*)
FROM employees
WHERE salary >= (SELECT AVG(salary)
                FROM employees);
                

-------------------------------------------------------------
SELECT *
FROM(
    SELECT rownum as rnum
        , T1.*
    FROM(
        SELECT 교수.교수이름
     , 교수.학위
     , 교수.주소
     , COUNT(강의내역.강의내역번호)
FROM 교수          --SELECT에서 사용한 테이블명
   , 강의내역
WHERE 교수.교수번호 = 강의내역.교수번호(+)
GROUP BY 교수.교수이름
       , 교수.학위
       , 교수.주소  --GROUP BY 는 SELECT에서 집계함수를 제외한 컬럼 모두 써라
ORDER BY 4 desc
) T1
) T2
WHERE T2.RNUM = 2 ;
                
-------------------------------------------------------------
--employees의 아이디, 이름, 월급, 직업이름
--을 조회하는데 '100부서의 평균 월급'보다 월급을 많이 받는
--월급이 큰 순으로 상위 10명을 조회 하시오
-------------------------내 오답------------------------------
select *
from(
select rownum as rnum
    , T1.*
    from(
            select EMPLOYEE_ID
                 , EMP_NAME
                 , SALARY
                 , JOB_ID
            from employees
            where DEPARTMENT_ID = 100
            GROUP BY EMPLOYEE_ID
                   , EMP_NAME
                   , SALARY
                   , JOB_ID
            having AVG(DEPARTMENT_ID(salary))
            order by 3 desc    
)T1
)T2
WHERE T2 rnum between 1 and 10 ;

--------------------------정답-----------------------------

select *
from
    (select t1.*
         , rownum as rnum
    from (
            select a.employee_id
                 , a.emp_name
                 , a.salary
                 , b.job_title  
              from employees a, jobs b  
             where a.job_id = b.job_id
             and a.salary > (SELECT AVG(salary)
                             FROM employees
                             where department_id =100 )
             order by a.salary desc
        )t1
    )t2
where t2.rnum between 1 and 10 ;
```
```sql
---------------------------------------------------------------
--오늘의 문제
--employees 테이블 부서 id가 있는 데이터에서 조회
-- 부서별, 입사월별 사원수, 총급여(중요), employees 테이블, hire_date, salary 컬럼 이용

SELECT DEPARTMENT_ID AS 부서명
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'01', 1, 0)) AS "1월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'02', 1, 0)) AS "2월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'03', 1, 0)) AS "3월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'04', 1, 0)) AS "4월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'05', 1, 0)) AS "5월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'06', 1, 0)) AS "6월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'07', 1, 0)) AS "7월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'08', 1, 0)) AS "8월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'09', 1, 0)) AS "9월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'10', 1, 0)) AS "10월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'11', 1, 0)) AS "11월"
     , SUM(DECODE(TO_CHAR(HIRE_DATE, 'MM'),'12', 1, 0)) AS "12월"
     , SUM(SALARY) AS 총급여
FROM EMPLOYEES
GROUP BY ROLLUP(DEPARTMENT_ID)
ORDER BY 1 ASC ;
-------------------------------------------------------------------    
-- 오늘 문제
-- sales 테이블에서 년도별 상품 매출액(amount_sold)을
-- 일요일부터 월요일까지 구분해 출력하시오
--날짜 일요일, 월요일, 화요일, 수요일, 목요일, 금요일, 토요일, 년합의 매출을 구하시오
-------------------------------------------------------------
--내 오답
SELECT    TO_CHAR(SALES_DATE, 'YYYY')
        ,DECODE(TO_CHAR(SALES_DATE, 'D'),'1', amount_sold, 0) as 일요일
        ,DECODE(TO_CHAR(SALES_DATE, 'D'),'2', amount_sold, 0) as 월요일
        ,DECODE(TO_CHAR(SALES_DATE, 'D'),'3', amount_sold, 0) as 화요일
        ,DECODE(TO_CHAR(SALES_DATE, 'D'),'4', amount_sold, 0) as 수요일
        ,DECODE(TO_CHAR(SALES_DATE, 'D'),'5', amount_sold, 0) as 목요일
        ,DECODE(TO_CHAR(SALES_DATE, 'D'),'6', amount_sold, 0) as 금요일
        ,DECODE(TO_CHAR(SALES_DATE, 'D'),'7', amount_sold, 0) as 토요일
FROM SALES
GROUP BY sales_date, amount_sold
ORDER BY 1 ASC;

-----------------------------------------------------------------------------------------
-- 정답
SELECT   TO_CHAR(SALES_DATE, 'YYYY') 날짜
        ,TO_CHAR(SUM(DECODE(TO_CHAR(SALES_DATE, 'D'),'1', amount_sold, 0)),'999,999,999,999') 일요일
        ,TO_CHAR(SUM(DECODE(TO_CHAR(SALES_DATE, 'D'),'2', amount_sold, 0)),'999,999,999,999') 월요일
        ,TO_CHAR(SUM(DECODE(TO_CHAR(SALES_DATE, 'D'),'3', amount_sold, 0)),'999,999,999,999') 화요일
        ,TO_CHAR(SUM(DECODE(TO_CHAR(SALES_DATE, 'D'),'4', amount_sold, 0)),'999,999,999,999') 수요일
        ,TO_CHAR(SUM(DECODE(TO_CHAR(SALES_DATE, 'D'),'5', amount_sold, 0)),'999,999,999,999') 목요일
        ,TO_CHAR(SUM(DECODE(TO_CHAR(SALES_DATE, 'D'),'6', amount_sold, 0)),'999,999,999,999') 금요일
        ,TO_CHAR(SUM(DECODE(TO_CHAR(SALES_DATE, 'D'),'7', amount_sold, 0)),'999,999,999,999') 토요일
FROM SALES
GROUP BY ROLLUP (TO_CHAR(SALES_DATE, 'YYYY')) ;

---------------------------------------------------------------
--연관성 있는 서브쿼리
--메인쿼리와 연관성 있는 서브쿼리 (세미 조인 semi-join)
--서브쿼리를 사용해 서브쿼리에 존재하는 데이터만 메인쿼리에서 추출하는 조인방법
SELECT a.department_id
     , a. department_name
FROM departments a
WHERE EXISTS(SELECT 1
                FROM job_history b
                WHERE a.department_id = b.department_id);

----------------------------------------------------------------
--학생 테이블에서 수강이력이 없는
--학생의 정보만 모두 조회하시오

--첫번째 방법
select *
--       a.학번    --모두 부를때는 *를 하면 된다.
--     , a.이름
--     , a.주소
--     , a.부전공
--     , a.생년월일
--     , a.학기
--     , a.평점
from 학생 a
where not exists(select 1
                    from 수강내역 b
                    where a.학번 = b.학번) ;
----------------------------------------------------------------                    
--두번째 방법                     
select *
from 학생
where 학생.학번 not in (select 수강내역.학번
                    from 수강내역) ;                    

--------------------------------------------------------------
--문제1
--학생의 정보를 출력하시오
--이름, 학번, 전공, 부전공, 수강과목명

select 학생.이름
     , 학생.학번
     , 학생.전공
     , 학생.부전공
     , (select 과목이름
        from 과목
        where 과목.과목번호 = 수강내역.과목번호)as 과목명
from 학생, 수강내역, 과목
where 학생.학번 = 수강내역.학번(+)
and 수강내역.과목번호 = 과목.과목번호(+) ;

--------------------------------------------------------------
--문제2
--학생정보의 전공, 부전공, 전체 수강건수, 전체 이수학점 을 출력하시오

select 학생.이름
     , 학생.전공
     , 학생.부전공
     , COUNT(수강내역.수강내역번호) -- 컬럼 먼저 써놓고  
     , SUM(과목.학점)             -- 밑에 where에다가  =
from 학생, 수강내역, 과목
where 수강내역.학번(+) = 학생.학번
and 과목.과목번호(+) = 수강내역.과목번호
group by 학생.이름
, 학생.전공
, 학생.부전공
order by 4 desc ;

--------------------------------------------------------------
--문제3
--가장 많이 구매한 amount_sold
--사람의 구매이력정보를 출력하시오(customers, sales, products)활용
--이름, 카테고리, 서브카테고리, 금액합계

--- 가장 많이 구매한 사람 찾는 쿼리
select a.cust_id
, SUM(nvl(b.amount_sold,0))      --nvl(n1,n2)   n1이 null일 경우 n2로 변환
from customers a
   , sales b   
where a.cust_id = b.cust_id(+)
group by a.cust_id
order by 2 desc;

---
select  a.CUST_NAME             --컬럼 내에 같은 이름을 가진 데이터를 합치려면
      , c.PROD_CATEGORY         --select에서 같은 이름을 가진 데이터를 합치면
      , c.PROD_SUBCATEGORY      --나오는 옆 컬럼을 sum으로 묶어주고
      , sum(b.amount_sold)      --group by 에서는 sum으로 묶은 컬럼은 제외하고 작성
from customers a
   , sales b   
   , products c
where a.cust_id = 11407
and a.cust_id = b.cust_id(+)
and b.PROD_ID = c.PROD_ID(+)
group by a.CUST_NAME
        , c.PROD_CATEGORY
        , c.PROD_SUBCATEGORY
order by 2 asc;
        
```
## 🚦 테스트 
```sql
-----------------------------테스트------------------------------------
--내 답...
-----------1번 문제 ---------------------------------------------------
--ITEM 테이블에서 카테고리 아이디가 FOOD인 것만 출력하시오.
SELECT CATEGORY_ID
     , PRODUCT_NAME
     , PRODUCT_DESC
FROM ITEM
WHERE category_id LIKE 'FOOD' ;
-----------------------------------------------------------------------
-----------2번 문제 ---------------------------------------------------
/*
CUSTOMER 테이블에서 출생년도가 1996년 부터 조회하시오 (1996 ~ 2020)
MAIL_ID 컬럼은 이메일 앞부분@
MAIL_DOMAIN 컬럼은 이메일 @뒷부분
REG_DATE, BIRTH 은 'YYYY-MM-DD'형태로
*/

SELECT CUSTOMER_NAME
     , PHONE_NUMBER
     , SUBSTR(EMAIL, 1, INSTR(EMAIL, '@')-1) AS MAIL_ID
     , SUBSTR(EMAIL, INSTR(EMAIL, '@')+1) AS MAIL_DOMAIN
     , TO_CHAR(FIRST_REG_DATE, 'YYYY-MM-DD') AS REG_DATE
     , DECODE(SEX_CODE, 'F', '여자', 'M', '남자') AS SEX_NM
     , TO_CHAR(TO_DATE(BIRTH), 'YYYY-MM-DD') AS BIRTH
     , REPLACE(TO_CHAR(ZIP_CODE, '999,999'), ',', '-')
FROM CUSTOMER
WHERE TO_CHAR(TO_DATE(BIRTH), 'YYYY') >= 1996
ORDER BY 5 ASC;


-----------------------------------------------------------------------
-----------3번 문제 ---------------------------------------------------
/*
CUSTOMER에 있는 회원의 ZIP_CODE를 활용하여
서울의 구별로 남자,여자,회원정보없는,전체 인원 수를 출력하시오
*/


SELECT ADDRESS.ADDRESS_DETAIL                             AS ZIP_NM
     , SUM(DECODE(TO_CHAR(CUSTOMER.SEX_CODE), 'M', 1, 0)) AS 남자회원
     , SUM(DECODE(TO_CHAR(CUSTOMER.SEX_CODE), 'F', 1, 0)) AS 여자회원
     , SUM(DECODE(TO_CHAR(CUSTOMER.SEX_CODE), '', 1, 0))  AS 성별없음
     , COUNT(*)                                           AS 전체
FROM CUSTOMER
   , ADDRESS
WHERE CUSTOMER.ZIP_CODE = ADDRESS.ZIP_CODE
GROUP BY ADDRESS.ADDRESS_DETAIL
ORDER BY 5 DESC;


-----------------------------------------------------------------------
-----------4번 문제 ---------------------------------------------------
/*
고객별 지점 방문횟수와 방문객의 합을 출력하시오
방문횟수가 4번이상만 조회 (예약 취소건 제외)
*/


SELECT CUSTOMER.CUSTOMER_ID
     , CUSTOMER.CUSTOMER_NAME
     , RESERVATION.BRANCH
     , COUNT(RESERVATION.BRANCH)
     , SUM(RESERVATION.VISITOR_CNT)
FROM CUSTOMER, RESERVATION
WHERE CUSTOMER.CUSTOMER_ID = RESERVATION.CUSTOMER_ID
and RESERVATION.CANCEL = 'N'      -- 제외할 항목은 where and에서 제외하고 조회  
GROUP BY CUSTOMER.CUSTOMER_ID
       , CUSTOMER.CUSTOMER_NAME
       , RESERVATION.BRANCH
HAVING COUNT(RESERVATION.BRANCH) >= 4
ORDER BY 4 DESC, 5 DESC   ;       -- 정렬은 여러 개 놓을 수 있다.(앞에 놓은 게 우선)
       
-----------------------------------------------------------------------
-----------5번 문제 ---------------------------------------------------
/*
    4번 문제에서 가장많이 동일지점에 방문한 1명만 출력하시오
*/

SELECT *
FROM(
    SELECT ROWNUM AS RNUM
        , T1. *
    FROM(
        SELECT  CUSTOMER.CUSTOMER_ID
              , COUNT(RESERVATION.BRANCH)
        FROM CUSTOMER
            ,RESERVATION
            WHERE CUSTOMER.CUSTOMER_ID = RESERVATION.CUSTOMER_ID
            GROUP BY CUSTOMER.CUSTOMER_ID
            ORDER BY 2 DESC
) T1
) T2
WHERE T2. RNUM = 1 ;

W1338910


-----------------------------------------------------------------------
-----------6번 문제 ---------------------------------------------------
/*
5번 문제 고객의 구매 품목별 합산금액을 출력하시오(5번문제의 쿼리를 활용하여)
*/

------------------오류 나오는데 왜 나오는지 모르겠습니다..-------------------
SELECT ITEM.PRODUCT_NAME
     , SUM(ITEM.PRICE)
FROM ITEM
WHERE RESERVATION.CUSTOMER_ID = 'W1338910'
AND RESERVATION.RESERV_NO = ORDER_INFO.RESERV_NO
AND ORDER_INFO.ITEM_ID = ITEM.ITEM_ID
GROUP BY ITEM.PRODUCT_NAME
ORDER BY 2 DESC ;

---------------------------------------------------------------------

```
## 🚦 테스트 정답 
```sql
-- 문제 정답
---------------------------------------------------------------------
/*
 user 를 (유저이름/비번 study/study)생성하고 권한을 주고(어제 안했다면)
 create_table 스크립트를 실해하여
 테이블 생성후 1~ 5 데이터를 임포트한 뒤
 아래 문제를 출력하시오
 (문제에 대한 출력물은 이미지 참고)
*/

-----------1번 문제 --------------------------------------------------
ITEM 테이블에서 카테고리 아이디가 FOOD인 것만 출력하시오.
---------------------------------------------------------------------
SELECT CATEGORY_ID
     , PRODUCT_NAME
     , PRODUCT_DESC
FROM ITEM
WHERE CATEGORY_ID = 'FOOD';

-----------2번 문제 ---------------------------------------------------
CUSTOMER 테이블에서 출생년도가 1996년 부터 조회하시오 (1996 ~ 2020)
MAIL_ID 컬럼은 이메일 @앞부분
MAIL_DOMAIN 컬럼은 이메일 @뒷부분
REG_DATE, BIRTH 은 'YYYY-MM-DD'형태로
---------------------------------------------------------------------

SELECT CUSTOMER_NAME
     , PHONE_NUMBER
     , SUBSTR(EMAIL, 0 ,INSTR(EMAIL,'@')-1) AS MAIL_ID
     , SUBSTR(EMAIL,INSTR(EMAIL,'@')+1) AS MAIL_DOMAIN
     , TO_CHAR(FIRST_REG_DATE,'YYYY-MM-DD') AS REG_DATE
     , DECODE(SEX_CODE, 'M','남자', 'F', '여자') AS SEX_NM
     , SUBSTR(BIRTH,1,4) || '-' ||SUBSTR(BIRTH,5,2) || '-' ||SUBSTR(BIRTH,7) AS BIRTH
     , SUBSTR(ZIP_CODE,0,3) || '-' || SUBSTR(ZIP_CODE,4) AS ZIPCODE
FROM CUSTOMER
WHERE TO_NUMBER(SUBSTR(BIRTH, 1,4)) > 1995
ORDER BY FIRST_REG_DATE ASC;



-----------3번 문제 ---------------------------------------------------
CUSTOMER에 있는 회원의 ZIP_CODE를 활용하여
서울의 구별로 남자,여자,회원정보없는,전체 인원 수를 출력하시오
---------------------------------------------------------------------

SELECT (SELECT ADDRESS_DETAIL FROM ADDRESS WHERE ZIP_CODE = T1.ZIP_CODE ) AS ZIP_NM
     ,  T1.M_CNT AS 남자회원
     ,  T1.F_CNT AS 여자회원
     ,  T1.NULL_CNT AS 성별없음
     ,  T1.CNT AS 전체
FROM (
        SELECT  ZIP_CODE
              , SUM(DECODE(SEX_CODE, 'M', 1, 0)) AS M_CNT
              , SUM(DECODE(SEX_CODE, 'F', 1, 0)) AS F_CNT
              , SUM(DECODE(SEX_CODE,  NULL, 1, 0)) AS NULL_CNT
              , COUNT(*) AS CNT
        FROM CUSTOMER
        GROUP BY ZIP_CODE
        ORDER BY 5 DESC
    ) T1;


-----------4번 문제 ---------------------------------------------------
-- 고객별 지점 방문횟수와 방문객의 합을 출력하시오
-- 방문횟수가 4번이상만 조회 (예약 취소건 제외)
---------------------------------------------------------------------
    SELECT A.CUSTOMER_ID
         , A.CUSTOMER_NAME
         , B.BRANCH
         , COUNT(B.BRANCH) BRANCH_CNT
         , SUM(B.VISITOR_CNT) VISITOR_SUM_CNT
    FROM CUSTOMER A
        ,RESERVATION B
    WHERE A.CUSTOMER_ID = B.CUSTOMER_ID
    AND B.CANCEL = 'N'
    GROUP BY A.CUSTOMER_ID  
           , A.CUSTOMER_NAME
           , B.BRANCH
    HAVING COUNT(B.BRANCH)>= 4
    ORDER BY 4 DESC,5 DESC;


-----------5번 문제 ---------------------------------------------------
5번 문제에서 가장많이 동일지점에 방문한 1명만 출력하시오
---------------------------------------------------------------------
    SELECT CUSTOMER_ID
    FROM (
            SELECT A.CUSTOMER_ID
                 , A.CUSTOMER_NAME
                 , B.BRANCH
                 , COUNT(B.BRANCH) BRANCH_CNT
                 , SUM(B.VISITOR_CNT) SUM_CNT
            FROM CUSTOMER A
                ,RESERVATION B
            WHERE A.CUSTOMER_ID = B.CUSTOMER_ID
            AND B.CANCEL = 'N'
            GROUP BY A.CUSTOMER_ID  
                   , A.CUSTOMER_NAME
                   , B.BRANCH
            ORDER BY COUNT(B.BRANCH) DESC
            ) T1
    WHERE ROWNUM = 1;
    
    

-----------6번 문제 ---------------------------------------------------
5번 문제 고객의 구매 품목별 합산금액을 출력하시오(5번문제의 쿼리를 활용하여)
---------------------------------------------------------------------
SELECT (SELECT PRODUCT_NAME
        FROM ITEM
        WHERE ITEM_ID = T1.ITEM_ID) AS CATEGORY
      , SUM(T1.SALES) AS SUM_SALES
FROM ORDER_INFO T1
WHERE T1.RESERV_NO IN (SELECT RESERV_NO
                       FROM RESERVATION
                       WHERE CANCEL = 'N'
                       AND CUSTOMER_ID = (SELECT CUSTOMER_ID
                                            FROM (
                                                    SELECT A.CUSTOMER_ID
                                                         , A.CUSTOMER_NAME
                                                         , B.BRANCH
                                                         , COUNT(B.BRANCH) BRANCH_CNT
                                                         , SUM(B.VISITOR_CNT) SUM_CNT
                                                    FROM CUSTOMER A
                                                        ,RESERVATION B
                                                    WHERE A.CUSTOMER_ID = B.CUSTOMER_ID
                                                    AND B.CANCEL = 'N'
                                                    GROUP BY A.CUSTOMER_ID  
                                                           , A.CUSTOMER_NAME
                                                           , B.BRANCH
                                                    ORDER BY COUNT(B.BRANCH) DESC
                                                 ) T1
                                            WHERE ROWNUM = 1)
                     )
GROUP BY T1.ITEM_ID
ORDER BY 2 DESC;


-----------7번 문제 ---------------------------------------------------
ITEM 테이블에 신규 데이터를 입력하는 구문의 첫번째 데이터인 코드값 ()에 들어갈 쿼리를 완성하여 입력하시오.
ITEM 코드 값을 조회해서 생성하시오
EX) SELECT ITEM_ID FROM ITEM 로 조회되는 값의 수에 +1 즉 총자리수는 6자리임 문자1 + 숫자5 현재 M00011 있으니 M00012 이 입력되어야함.

INSERT INTO ITEM VALUES ((),'SOUP','스프','FOOD',7000);
---------------------------------------------------------------------

INSERT INTO ITEM VALUES ( (SELECT LPAD(NVL(MAX(TO_NUMBER(REPLACE(ITEM_ID,'M',''))),0) + 1 , 5 , 'M0000')
                           FROM ITEM)
                           ,'SOUP'
                           ,'스프'
                           ,'FOOD'
                           ,7000  
                         );



SELECT LPAD(NVL(MAX(TO_NUMBER(REPLACE(ITEM_ID,'M',''))),0) + 1 , 5 , 'M0000')
FROM ITEM ;

-----------8번 문제 ---------------------------------------------------
-- 7번 문제 스프 -> 수프로
        7000 -> 7500 으로 수정하시오
---------------------------------------------------------------------
UPDATE ITEM
SET PRODUCT_DESC = '수프'
,   PRICE = 7500
WHERE ITEM_ID = 'M0011';
COMMIT;
-----------9번 문제 ---------------------------------------------------
 전체 상품의 총 판매량과 총 매출액, 전용 상품의 판매량과 매출액을 출력하시오
 reservation, order_info 테이블을 활용하여
 온라인 전용상품의 총매출을 구하시오
 ---------------------------------------------------------------------


SELECT SUM(B.quantity) 총판매량,
       SUM(B.sales) 총매출,
       SUM(DECODE(B.item_id,'M0001',B.quantity,0)) 전용상품판매량,
       SUM(DECODE(B.item_id,'M0001',B.sales,0)) 전용상품매출
FROM reservation A, order_info B
WHERE A.reserv_no = B.reserv_no(+)
AND A.CANCEL = 'N';



-----------10번 문제 ---------------------------------------------------
매출월별 총매출, 전용상품이외의 매출, 전용상품 매출, 전용상품판매율, 총예약건, 예약완료건, 예약취소건, 예약취소율을 출력하시오
 ---------------------------------------------------------------------

SELECT T1.매출월
    ,  T1.총매출
    ,  T1.전용상품외매출
    ,  T1.전용상품매출
    ,  T1.전용상품판매율
    ,  T2.총예약건
    ,  T2.예약완료건
    ,  T2.예약취소건
    ,  T2.예약취소율
FROM   (
        SELECT  SUBSTR(A.reserv_date,1,6) 매출월
              , SUM(B.sales) 총매출
              , SUM(B.sales)- SUM(DECODE(B.item_id,'M0001',B.sales,0)) 전용상품외매출
              , SUM(DECODE(B.item_id,'M0001',B.sales,0)) 전용상품매출
              , ROUND(SUM(DECODE(B.item_id,'M0001',B.sales,0))/SUM(B.sales)*100,1)||'%' 전용상품판매율
        FROM reservation A, order_info B
        WHERE A.reserv_no = B.reserv_no
        GROUP BY SUBSTR(A.reserv_date,1,6)
        ORDER BY SUBSTR(A.reserv_date,1,6)
        )T1
      , (
	SELECT SUBSTR(A.reserv_date,1,6) 매출월
	      , COUNT(DISTINCT A.reserv_no) 총예약건
	      , SUM(DECODE(A.cancel,'N',1,0)) 예약완료건
	      , SUM(DECODE(A.cancel,'Y',1,0)) 예약취소건
	      , ROUND(SUM(DECODE(A.cancel,'Y',1,0))/COUNT(A.reserv_no)*100,1)||'%' 예약취소율
	FROM reservation A
	GROUP BY SUBSTR(A.reserv_date,1,6)
	ORDER BY SUBSTR(A.reserv_date,1,6)
	) T2

WHERE T1.매출월 = T2.매출월;
-------------------------------------------------------------------------------
--
select (select department_name -- 데이터는 무조건 한 건만
        from departments
        where department_id = a.department_id)
        , a.department_id
        , a.job_id
        ,(select job_title from jobs where job_id = a.job_id) as job_nm
from employees a ;

```
### 4. ANSI 조인  
- 기존 문법과 ANSI조인의 차이점은 조인 조건이 WHERE절이 아닌 FROM절에 들어간다는 점이다.
#### (1) ANSI 동등조인
```sql
-------------------------------------------------------------------------------
-- 일반 동등조인 : where 절에 조인 조건이 들어감
select a.employee_id
     , b.department_name
from employees a
    ,departments b
where a.department_id = b.department_id ;

--ANSI INNER JOIN : FROM 절에 조인 조건이 들어감
select a.employee_id
     , b.department_name
from employees a
inner join departments b
on(a.department_id = b.department_id ) ;
------------------------------------------------------------------------------
--컬럼이 동일할 때 as 말고 USING을 써도 됨
SELECT a.employee_id
     , department_id
     , b.department_name
FROM employees a
INNER JOIN departments b
USING(department_id) ;

--다중 조인 INNER JOIN 구문 추가로 작성
select a.employee_id
     , b.department_name
from employees a
INNER JOIN departments b
on(a.department_id = b.department_id)
INNER JOIN jobs c
on(a.job_id = c.job_id) ;
```
#### (2) ANSI 아우터조인
```sql
-------------------------------------------------------------------------------
-- 2. 아우터조인
select *
from 학생
    , 수강내역
where 학생.학번 = 수강내역.학번(+) ;
-- ANSI 아우터 조인(LEFT, RIGHT)
select *
from 학생
LEFT OUTER JOIN 수강내역
ON(학생.학번 = 수강내역.학번);

select *
from 학생
RIGHT OUTER JOIN 수강내역
ON(학생.학번 = 수강내역.학번);
-------------------------------------------------------------------------------

select *
from addr a
    ,hobby b
    where a.code(+) = b.code ;
```
#### (3) ANSI FULL OUTER JOIN 
```sql
-------------------------------------------------------------------------------  
--FULL OUTER JOIN
--where a.code(+) = b.code(+) ; <-- 와 같은 의미
--ANSI FULL OUTER JOIN 양쪽 테이블에 널이 있어도 포함시키고자 할 때
--FULL OUTER JOIN은 ANSI 문법만 가능
select *
from addr a
FULL OUTER JOIN hobby b
ON(a.code = b.code); --where a.code(+) = b.code(+) ; <-- 와 같은 의미
-------------------------------------------------------------------------------    
/*
오늘의 문제
ANSI와 일반 구문 둘다 작성해보세요
월별 온라인 전용 상품 매출액을 일요일부터 월요일까지 구분해 출력하시오
날짜, 상품명, 일요일,월요일, 화요일 수요일, 목요일, 금요일, 토요일의
매출을 구하시오
*/

select TO_CHAR(TO_DATE(RESERVATION.RESERV_DATE, 'YYYY-MM-DD'), 'YYYYMM') AS 날짜
     , ITEM.PRODUCT_NAME
     , SUM(DECODE(TO_CHAR(TO_DATE(RESERVATION.RESERV_DATE), 'D'), 1, ORDER_INFO.SALES, 0)) AS 일요일
     , SUM(DECODE(TO_CHAR(TO_DATE(RESERVATION.RESERV_DATE), 'D'), 2, ORDER_INFO.SALES, 0)) AS 월요일
     , SUM(DECODE(TO_CHAR(TO_DATE(RESERVATION.RESERV_DATE), 'D'), 3, ORDER_INFO.SALES, 0)) AS 화요일
     , SUM(DECODE(TO_CHAR(TO_DATE(RESERVATION.RESERV_DATE), 'D'), 4, ORDER_INFO.SALES, 0)) AS 수요일
     , SUM(DECODE(TO_CHAR(TO_DATE(RESERVATION.RESERV_DATE), 'D'), 5, ORDER_INFO.SALES, 0)) AS 목요일
     , SUM(DECODE(TO_CHAR(TO_DATE(RESERVATION.RESERV_DATE), 'D'), 6, ORDER_INFO.SALES, 0)) AS 금요일
     , SUM(DECODE(TO_CHAR(TO_DATE(RESERVATION.RESERV_DATE), 'D'), 7, ORDER_INFO.SALES, 0)) AS 토요일
FROM RESERVATION
    ,ITEM
    ,ORDER_INFO
WHERE ORDER_INFO.RESERV_NO = RESERVATION.RESERV_NO
AND ORDER_INFO.ITEM_ID = 'M0001'
AND ITEM.ITEM_ID = 'M0001'
GROUP BY TO_CHAR(TO_DATE(RESERVATION.RESERV_DATE, 'YYYY-MM-DD'), 'YYYYMM')
        ,ITEM.PRODUCT_NAME
ORDER BY 1 ASC;



-------------------------------------------------------------------------------    
--ANSI로 바꾼 것
select TO_CHAR(TO_DATE(r.RESERV_DATE, 'YYYY-MM-DD'), 'YYYYMM') AS 날짜
     , i.PRODUCT_NAME
     , SUM(DECODE(TO_CHAR(TO_DATE(r.RESERV_DATE), 'D'), 1, o.SALES, 0)) AS 일요일
     , SUM(DECODE(TO_CHAR(TO_DATE(r.RESERV_DATE), 'D'), 2, o.SALES, 0)) AS 월요일
     , SUM(DECODE(TO_CHAR(TO_DATE(r.RESERV_DATE), 'D'), 3, o.SALES, 0)) AS 화요일
     , SUM(DECODE(TO_CHAR(TO_DATE(r.RESERV_DATE), 'D'), 4, o.SALES, 0)) AS 수요일
     , SUM(DECODE(TO_CHAR(TO_DATE(r.RESERV_DATE), 'D'), 5, o.SALES, 0)) AS 목요일
     , SUM(DECODE(TO_CHAR(TO_DATE(r.RESERV_DATE), 'D'), 6, o.SALES, 0)) AS 금요일
     , SUM(DECODE(TO_CHAR(TO_DATE(r.RESERV_DATE), 'D'), 7, o.SALES, 0)) AS 토요일
FROM RESERVATION r
INNER JOIN ORDER_INFO o
ON (o.RESERV_NO = r.RESERV_NO)
INNER JOIN ITEM i
ON (o.ITEM_ID = i.ITEM_ID)
WHERE o.ITEM_ID = 'M0001'
GROUP BY TO_CHAR(TO_DATE(r.RESERV_DATE, 'YYYY-MM-DD'), 'YYYYMM')
        ,i.PRODUCT_NAME
ORDER BY 1 ASC;
-------------------------------------------------------------------------------    
```
    
    
## 📚 6장. SQL 함수

### 1. PL/SQL
- PL/SQL 의 기본 구조
- 일반 프로그래밍 언어와 다른 점은 모든 코드가 DB 내부에 만들어지며 처리됨으로써 수행 속도와 성능 측면에서 큰 장점이 있다.

- 블록(pl/sql 소스프로그램의 기본 단위)
  1. 이름부 : 블록의 명칭이 옴. 생략하면 익명 블록이 됨
  2. 선언부 : DECLARE로 시작됨. 문장의 마지막은 세미콜론을 넣어야 하며 사용할변수나 상수가없다면 생략 가능
  3. 실행부 : 실제로 로직을 처리하는 부분

- := 오른쪽 값을 왼쪽에 할등한다.
- dbms_output.put_line는 매개변수 값을 출력한다.
- SET SERVEROUTPUT ON : OUTPUT 패키지를 사용해 값을 보려면
- SET TIMING ON       : 블록의 소요시간을 보려면
```sql
SET SERVEROUTPUT ON
SET TIMING ON

DECLARE -- 익명블록(이름이 없는 PL/SQL)
    vi_num NUMBER ;
BEGIN
    vi_num := 100;
    DBMS_OUTPUT.PUT_LINE('출력 :' ||vi_num);
END ;
    

DECLARE
    vi_num CONSTANT NUMBER := 100 ;  --상수
BEGIN
    vi_num := 101;    --오류
    DBMS_OUTPUT.PUT_LINE('출력 :' ||vi_num);
END ;


DECLARE
VI_NUM NUMBER := 2*2;
BEGIN
    VI_NUM := VI_NUM **10; ---연산 가능
    DBMS_OUTPUT.PUT_LINE('출력 :' ||vi_num);
END ;


------------------------------------------------------------------
DECLARE
    VS_EMP_NAME    VARCHAR(80) ; --사원명 변수
    VS_DEP_NAME    VARCHAR(80) ; --부서명 변수
BEGIN
    SELECT A.EMP_NAME, B.DEPARTMENT_NAME
    INTO VS_EMP_NAME, VS_DEP_NAME
    FROM EMPLOYEES A,
        DEPARTMENTS B
    WHERE A.DEPARTMENT_ID = B.DEPARTMENT_ID
    AND A.EMPLOYEE_ID = 100;
        DBMS_OUTPUT.PUT_LINE(VS_EMP_NAME || ' - ' || VS_DEP_NAME);
END ;


/*
상수로 '최숙경' 선언하고 학생 테이블에서 이름을 조회하여 학번을 변수에 담는
DML 문을 작성하고 아래와 같이 출력하시오
최숙경 : 1997131542
*/
DECLARE
    VS_EMP_NAME CONSTANT VARCHAR2(80) := :A; --바인딩(값을 받아서 출력)
    VS_HAK_NUM NUMBER;
BEGIN
    SELECT 학번
    INTO VS_HAK_NUM
    FROM 학생
    WHERE 이름 = VS_EMP_NAME ;
    DBMS_OUTPUT.PUT_LINE(VS_EMP_NAME || ':' || VS_HAK_NUM) ;
END ;

------------------------------------------------------------------

DECLARE
    VS_EMP_NAME CONSTANT 학생.이름%TYPE := :A; --바인딩(값을 받아서 출력)
    VS_HAK_NUM           학생.학번%TYPE ;      --특정 테이블의 변수의 타입
BEGIN
    SELECT 학번
    INTO VS_HAK_NUM
    FROM 학생
    WHERE 이름 = VS_EMP_NAME ;
    DBMS_OUTPUT.PUT_LINE(VS_EMP_NAME || ':' || VS_HAK_NUM) ;
END ;

------------------------------------------------------------------
--변수 선언이 필요 없을 경우 DECLARE 표시 안 해도 됨  

BEGIN
    DBMS_OUTPUT.PUT_LINE(1 * 3); --3
    DBMS_OUTPUT.PUT_LINE(2 * 3); --6
    DBMS_OUTPUT.PUT_LINE(3 * 3); --9
    DBMS_OUTPUT.PUT_LINE(4 * 3); --12
END ;

------------------------------------------------------------------
--변수 선언시 타입은 테이블명.컬럼명%TYPE ;
--사원(employees) 테이블에서 201번 사원의 '이름'과
--'이메일주소'를 출력하는 익명 블록을 만들어 보자
--아래처럼
--Michael Hartstein사원의 E-MAIL : MHARTSTE
DECLARE
    VS_EMP_NAME employees.emp_name%TYPE ;
    VS_EMAIL employees.email%TYPE ;
BEGIN
SELECT EMP_NAME , EMAIL
INTO VS_EMP_NAME, VS_EMAIL --변수
FROM EMPLOYEES
WHERE EMPLOYEE_ID = : a ;
    DBMS_OUTPUT.PUT_LINE(VS_EMP_NAME || ':' || VS_EMAIL) ;
END;

------------------------------------------------------------------

/*
(1) 사원 테이블에서 사원번호가 제일 큰 사원을 찾아낸 뒤,
vn_max_empno 변수를 담고
(2) vn_max_empno + 1 하여 사원 번호를 입력하는
insert구문으로 사원테이블에 신규 입력하는 익명 블록을 만들어보자.
<사원명> :
<이메일> : HARRIS
<입사일자> : 현재일자
<부서번호> : 50
*/

DECLARE
    VN_MAX_EMPNO EMPLOYEES.EMPLOYEE_ID%TYPE;
BEGIN
    SELECT MAX(EMPLOYEE_ID)
    INTO VN_MAX_EMPNO
    FROM EMPLOYEES ;
    
    INSERT INTO EMPLOYEES (EMPLOYEE_ID, EMP_NAME, EMAIL, HIRE_DATE, DEPARTMENT_ID)
    VALUES(VN_MAX_EMPNO+1, 'Harrion Ford', 'HARRIS', sysdate, 50);
    commit;
END ;

SELECT *
FROM EMPLOYEES
ORDER BY
------------------------------------------------------------------
```
- IF문
```SQL
-- IF문
DECLARE
 VN_NUM1 NUMBER :=1;
 VN_NUM2 NUMBER :=2;
BEGIN
 IF VN_NUM1  > VN_NUM2 THEN
    DBMS_OUTPUT.PUT_LINE(VN_NUM1 || '이 큰수 ');
ELSE
    DBMS_OUTPUT.PUT_LINE(VN_NUM2 || '이 큰수 ');
 END IF ;
END;

------------------------------------------------------------------
--학점이 3이상 A출력
--학점이 3미만 B출력
--이름 : 바인딩

DECLARE
 VN_HAK_NUM 학생.평점%TYPE ;
 VN_NAME 학생.이름%TYPE := :이름;
BEGIN
    SELECT 평점
    INTO   VN_HAK_NUM
    FROM   학생
    WHERE  이름 = VN_NAME ;
    
IF VN_HAK_NUM >= 3 THEN
     DBMS_OUTPUT.PUT_LINE(VN_NAME || '의 평점 A');
ELSE
     DBMS_OUTPUT.PUT_LINE(VN_NAME || '의 평점 B');
END IF ;
END ;
------------------------------------------------------------------
```
- LOOP문
```SQL
--LOOP문

DECLARE
  VN_BASE NUMBER :=3;
  VN_CNT  NUMBER :=1;
BEGIN
LOOP
    DBMS_OUTPUT.PUT_LINE(VN_BASE || '*' || VN_CNT || '=' || VN_BASE*VN_CNT);
    VN_CNT := VN_CNT + 1; --VN_CNT 값을 1씩 증가
    EXIT WHEN VN_CNT > 9; --VN_CNT가 9보다 크면 루프 종료
      END LOOP;
END;
------------------------------------------------------------------
```
- WHILE문
```SQL
--WHILE문
DECLARE
  VN_BASE NUMBER :=3;
  VN_CNT  NUMBER :=1;
BEGIN
WHILE VN_CNT <= 9  --VN_CNT가 9보다 작거나 같을 경우에만 반복
LOOP
    DBMS_OUTPUT.PUT_LINE(VN_BASE || '*' || VN_CNT || '=' || VN_BASE*VN_CNT);
    VN_CNT := VN_CNT + 1; --VN_CNT 값을 1씩 증가
END LOOP;
END;
------------------------------------------------------------------
```
- FOR문
```SQL
--FOR문

DECLARE
 VN_BASE NUMBER :=3;
BEGIN
 FOR i IN 1..9 --i 참조는 가능하나 변경은 불가능(..은 규칙)
 LOOP
     DBMS_OUTPUT.PUT_LINE(VN_BASE || '*' || i || '=' || VN_BASE*i);
 END LOOP;
END ;

--FOR REVERSE

DECLARE
 VN_BASE NUMBER :=3;
BEGIN
 FOR i IN REVERSE 1..9 --REVERSE 반대로
 LOOP
     DBMS_OUTPUT.PUT_LINE(VN_BASE || '*' || i || '=' || VN_BASE*i);
 END LOOP;
END ;
------------------------------------------------------------------
```
- 구구단
```SQL
--구구단 (for - loop - end loop)
BEGIN

 FOR i IN 2..9 --i 참조는 가능하나 변경은 불가능(..은 규칙)
 LOOP
     FOR k IN 1..9
     LOOP
     DBMS_OUTPUT.PUT_LINE(i || '*' || k || '=' || i*k);
     END LOOP;
 DBMS_OUTPUT.PUT_LINE('=============');
 END LOOP;
END ;
-- 건너뛸때 (Continue)
BEGIN

 FOR i IN 2..9 --i 참조는 가능하나 변경은 불가능(..은 규칙)
 LOOP
     CONTINUE WHEN i = 5 ; --해당 조건일 때 건너뜀
     CONTINUE WHEN i = 7 ; --해당 조건일 때 건너뜀

     FOR k IN 1..9
     LOOP
     DBMS_OUTPUT.PUT_LINE(i || '*' || k || '=' || i*k);
     END LOOP;
 DBMS_OUTPUT.PUT_LINE('=============');
 END LOOP;
END ;
```
```SQL
/*
신입생에게 기존학번의 가장 높은 번호를 찾아
높은 번호의 앞자리 4자리(올해년도)와 같다면 max학번에 +1을
다른 년도라면 해당 년도에 학번 자리수 반큼 자릿수를 맞추고 +1을 해주어
학번을 생성하고
학생 테이블에 INSERT 햬주는 익명블록을 만드시오
                    :이름, : 나이는 입력받으시오
insert 데이터 학번, 이름, 나이
*/
------------------------------------------------------------------

BEGIN
     DBMS_OUTPUT.PUT_LINE(TO_CHAR(SYSDATE, 'YYYY'));
     IF TO_CHAR(SYSDATE, 'YYYY') = '2021' THEN
             DBMS_OUTPUT.PUT_LINE('올햬');
     END IF ;
END ;
------------------------------------------------------------------

SELECT
IF
(2021000001) 학번을 생성하여 변수에 할당
INSERT


DECLARE
 학생학번 학생.학번%TYPE ;
 학생이름 학생.이름%TYPE := :이름;
 학생생년월일 학생.생년월일%TYPE := :생년월일;
BEGIN
    SELECT 학번
    INTO   학생학번
    FROM 학생
    WHERE 학생생년월일 = 생년월일
     IF SUBSTR(학번, 1, 4)  = '2021' THEN
             DBMS_OUTPUT.PUT_LINE('올해');
     END IF ;
END ;


DECLARE
DBMS_OUTPUT.PUT_LINE(SUBSTR(학번, 1, 4));
BEGIN
    SELECT 학번
    INTO 학생학번
END ;

IF(SELECT SUBSTR(MAX(학번), 1, 4)) < '학번'
    DBMS_OUTPUT.PUT_LINE(학번);
from 학생 ;


DECLARE
 학생학번 학생.학번%TYPE ;
 학생이름 학생.이름%TYPE := :이름;
 학생생년월일 학생.생년월일%TYPE := :생년월일;
BEGIN
    SELECT 학번
    INTO   학생학번
    FROM   학생
    WHERE 학생생년월일 = 생년월일
IF 학생학번  > SUBSTR(MAX(학번), 1, 4) THEN
    DBMS_OUTPUT.PUT_LINE(학생학번);
ELSE
    DBMS_OUTPUT.PUT_LINE(학생학번 + 1);
END IF ;
END;


select * from 학생;

select substr(학번,1,4) from 학생;
select rpad(학번,10,0) from 학생;
------------------------------------------------------------------

/*
기존의 학번과 현재 시스템 년도의 년도가 같다면 기존학번에 +1 을 해준다음 학번 이름 생년월일을 입력해주고
년도가 다르다면 생년월일의 년도와 기존학번의 자리수를 맞춰준다음 빈공간은 0으로 채워 주어라! +1해주어라!
INSERT INTO 테이블명(컬럼명)values(값);

*/

DECLARE
 nums 학생.학번%TYPE;
 names 학생.이름%TYPE:= :이름;
 births 학생.생년월일%TYPE:= :생년월일;
 
BEGIN
    SELECT max(학번)
    INTO   nums
    FROM   학생 ;

    IF substr(to_char(nums),1,4)  = to_char(sysdate,'YYYY') then
        INSERT INTO 학생(학번,이름,생년월일) values (nums+1,names,births);
    else
        INSERT INTO 학생(학번,이름,생년월일)values(rpad(to_char(sysdate,'YYYY'),10,0)+1,names,births);
        
        
    END IF ;
END;

select * from 학생;

------------------------------------------------------------------
```

