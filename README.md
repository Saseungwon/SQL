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

### 10. MERGE문

- 없으면 insert
- 있으면 update

- 만약 merge 문이 안된다면
1. select 있는지 없는지 체크
if
2. 없으면 insert
else
3. 있으면 update


 #### MERGE
 - 특정 조건에 따라 어떤 때는 INSERT
 - 다른경우에는 UPDATE 문을 수행할 때 사용가능

- INTO : DATA가 UPDATE되거나 INSERT 될 테이블 또는 뷰를 지정.
- USING : 비교할 SOURCE 테이블 또는 뷰나 서브쿼리를 지정, INTO절의 테이블과 동일하거나 다를 수 있다.
- ON : UPDATE나 INSERT를 하게 될 조건으로, 해당 조건을 만족하는 DATA가 있으면 WHEN MATCHED 절을 실행하게 되고, 없으면 WHEN NOT MATCHED 이하를 실행하게 된다.
- WHEN MATCHED : ON 조건절이 TRUE인 ROW에 수행 할 내용 (UPDATE, DELETE포함 될 수 있음)
- WHEN NOT MATCHED : ON 조건절에 맞는 ROW가 없을 때 수행할 내용 (INSERT)
```sql
--------------------------------------------------------------------------------
*/
    MERGE INTO UPDATED_TABLE A
    USING   ( select b.a, b.c
              from DATA_TABLE
              where
            )  B
    ON (A.KEY = B.KEY
            AND A.ID = '001')
    WHEN MATCHED THEN  /* 매치되면? */
        UPDATE
        SET A.DATA1 = B.DATA1
    WHEN NOT MATCHED THEN  /* 매치안되면? */
        INSERT (a.KEY, a.DATA1)
        VALUES (B.KEY, B.DATA1)


      
     MERGE INTO 학생 a
     USING DUAL   -- 비교 테이블이 다르지 않을때
     ON (a.이름 = '본인이름')
     WHEN MATCHED THEN      --- ON절의 조건이 맞다면
          UPDATE SET a.주소 = '서울'
     WHEN NOT MATCHED THEN  --- ON절의 조건이 아니라면
          INSERT (a.학번 , a.이름, a.주소)
          VALUES ((select nvl(max(학번),0)+1 from 학생),'본인이름',  '주소');




SELECT *
FROM 학생 ;

merge into 학생 a
using dual              -- 비교 테이블이 다르지 않을 때
on(a.이름 = '사승원')
when MATCHED THEN       -- ON절의 조건이 맞다면
UPDATE SET a.주소 = '대전'
WHEN NOT MATCHED THEN   -- ON절의 조건이 아니라면
INSERT (a.학번, a.이름, a.주소)
VALUES ((select nvl(max(학번),0)+1 from 학생), '사승원', '대전')






CREATE TABLE ex3_3 (
       employee_id NUMBER,
       bonus_amt   NUMBER DEFAULT 0
       );
 /*
 (1) 사원테이블에서 관리자 사번 MANAGER_ID가 146 번인 사원을 찾아서

 (2) EX3_3 테이블에 있는 사원의 사번과 일치하면 보너스
     금액에 자신의 급여 1% 보너스로 갱신

 (3) 사번과 일치하는게 없으면 (1)의 결과를
     신규로 입력(이때  보너스 금액은 급여의 0.1%로 한다.
     이때 급여가 8000 미만인 사원만 처리.
  */

-- SALES 테이블에서 2000년 10 월부터
--                    2000년 10 월 까지 매출을 낸
-- 사원번호를 입력 GROUP BY 절을 추가해 사원의 중복을 없앰
INSERT INTO ex3_3 (employee_id)
SELECT e.employee_id
  FROM employees e, sales s
 WHERE e.employee_id = s.employee_id
   AND s.SALES_MONTH BETWEEN '200010' AND '200012'
 GROUP BY e.employee_id;
 
SELECT *
  FROM ex3_3
 ORDER BY employee_id;  


------------------------------------------------------------------------------------------

-- 메니저 사번이 146
SELECT employee_id, salary, manager_id
FROM employees
WHERE manager_id = 146;
-- 이 데이터 중 EX3_3 테이블에 있는 건 1퍼센트 인상

-- 업데이트 대상건
 SELECT employee_id, manager_id, salary, salary * 0.01
   FROM employees
  WHERE employee_id IN (  SELECT employee_id
                            FROM ex3_3 )
  AND manager_id = 146;
  
  
-- 인서트 대상건
 SELECT employee_id, manager_id, salary, salary * 0.001
   FROM employees
  WHERE employee_id NOT IN (  SELECT employee_id
                                FROM ex3_3 )
    AND manager_id = 146;
 

MERGE INTO ex3_3 d
     USING (SELECT employee_id, salary, manager_id
              FROM employees
             WHERE manager_id = 146) b
        ON (d.employee_id = b.employee_id)
 WHEN MATCHED THEN
      UPDATE SET d.bonus_amt = d.bonus_amt + b.salary * 0.01
 WHEN NOT MATCHED THEN
      INSERT (d.employee_id, d.bonus_amt) VALUES (b.employee_id, b.salary *.001)
       WHERE (b.salary < 8000);


 SELECT *
  FROM ex3_3
 ORDER BY employee_id;  
 
-- 업데이트 될 값을 평가해서 조건에 맞는 데이터를 삭제 할 수도 있다.
MERGE INTO ex3_3 d
     USING (SELECT employee_id, salary, manager_id
              FROM employees
             WHERE manager_id = 146) b
        ON (d.employee_id = b.employee_id)
 WHEN MATCHED THEN
      UPDATE SET d.bonus_amt = d.bonus_amt + b.salary * 0.01
      DELETE WHERE (B.employee_id = 161)
 WHEN NOT MATCHED THEN
      INSERT (d.employee_id, d.bonus_amt) VALUES (b.employee_id, b.salary *.001)
      WHERE (b.salary < 8000);

 SELECT *
  FROM ex3_3
 ORDER BY employee_id;  

MERGE INTO ex3_3 A
    USING DUAL
       ON (A.employee_id = 148)
 WHEN MATCHED THEN
      UPDATE SET A.BONUS_AMT = 10      
 WHEN NOT MATCHED THEN     
      INSERT (A.EMPLOYEE_ID, A.BONUS_AMT) VALUES (148,10);


---------------------------------------------------
-- 문제
create table ex_과목(
     과목번호  NUMBER(3) PRIMARY KEY
   , 과목이름          VARCHAR2(50)
   , 학점          NUMBER(3)    
);
INSERT INTO EX_과목 (과목번호, 과목이름, 학점 ) VALUES(501, '현대경영학', 10  );

-- ex_과목 테이블에 과목테이블과 비교하여
-- 없으면 INSERT(전체) 있으면 UPDATE(학점만)하시오

SELECT *
FROM 학생;


MERGE INTO ex_과목
     USING (SELECT 과목번호, 과목이름, 학점
              FROM 과목)과목
        ON (ex_과목.과목번호 = 과목.과목번호)
 WHEN MATCHED THEN
      UPDATE SET ex_과목.학점 = 과목.학점
 WHEN NOT MATCHED THEN
      INSERT( EX_과목.과목번호, EX_과목.과목이름, EX_과목.학점 )  
      values (과목.과목번호, 과목.과목이름, 과목.학점);

---------------------------------------------------

MERGE INTO ex_과목 a
     USING 과목  b
        ON (a.과목번호 = b.과목번호)
 WHEN MATCHED THEN
      UPDATE SET a.학점 = b.학점
 WHEN NOT MATCHED THEN
      INSERT( a.과목번호, a.과목이름, a.학점 )  
      values (b.과목번호, b.과목이름, b.학점);
```

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
    
    
## 📚 6장. PL/SQL

### 1. PL/SQL  기본구조 
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
### 2. IF문
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
### 3. LOOP문
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
### 4. WHILE문
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
### 5. FOR문
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
- 문제
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


### 6. CASE문
```sql
------------------------------------------------------------------
--CASE

DECLARE
    VN_SALARY NUMBER := 0 ;
    VN_DEPARTMENT_ID NUMBER := 0 ;
BEGIN
    VN_DEPARTMENT_ID := ROUND(DBMS_RANDOM.VALUE(10, 120), -1); --랜덤함수 10 ~ 120
    e
SELECT SALARY
INTO VN_SALARY
FROM EMPLOYEES
WHERE DEPARTMENT_ID = VN_DEPARTMENT_ID
AND ROWNUM = 1;

CASE WHEN VN_SALARY BETWEEN 1 AND 3000 THEN
        DBMS_OUTPUT.PUT_LINE('낮음');
     WHEN VN_SALARY BETWEEN 3001 AND 6000 THEN
        DBMS_OUTPUT.PUT_LINE('중간');
     WHEN VN_SALARY BETWEEN 6001 AND 10000 THEN
        DBMS_OUTPUT.PUT_LINE('높음');
     ELSE
        DBMS_OUTPUT.PUT_LINE('최상위');
    END CASE ;
END ;
------------------------------------------------------------------
```
### 7. NULL문
```sql
--NULL문
IF VN_VARIABLE = 'A' THEN
    NULL;--(처리로직1) 입력할 값이 없으면 그 자리에 null 입력
ELSE IF VN_VARIABLE = 'B' THEN
    NULL;--(처리로직2)
------------------------------------------------------------------
```
- 트리출력 
```sql
--조건문과 반복문을 활용하여
--입력 받은 수 +1 층의 트리를 프린트하시오

DECLARE
 VN_BASE type := (' ');
BEGIN
 FOR i IN 1..5
 LOOP
     DBMS_OUTPUT.PUT_LINE('*');
 END LOOP;
END ;



DECLARE
 VN_BASE varchar2(4000) :='*';
 VN_ESAB varchar2(4000) :=' ';
BEGIN
 FOR i IN 1..5
 LOOP
   DBMS_OUTPUT.PUT_LINE(VN_BASE);
    VN_BASE := VN_BASE || '*';
    FOR j IN 1..5 REVERSE
    VN_BASE := VN_BASE || '*';
    
--     DBMS_OUTPUT.PUT_LINE( (i*'*') || (j*' '));
--    LOOP
--    END LOOP ;
 END LOOP;
END ;




DECLARE
 VN_BASE varchar2(4000) :='*';
 VN_ESAB varchar2(4000) :=' ' ;
BEGIN
 FOR i IN 1..5
 LOOP
   DBMS_OUTPUT.PUT_LINE(VN_BASE);
    VN_BASE := VN_BASE || '*';
    END LOOP ;
 FOR j IN REVERSE 1..5
 IF THEN
 LOOP
   DBMS_OUTPUT.PUT_LINE(VN_ESAB || VN_BASE);
    VN_ESAB := VN_ESAB || ' ';
    END LOOP;
END ;




DECLARE
 VN_BASE varchar2(4000) :='*';
BEGIN
 FOR i IN 1..5
 LOOP
   DBMS_OUTPUT.PUT_LINE(VN_BASE);

    VN_BASE := VN_BASE || '*';

 END LOOP;
END ;

(추가해야됨)
```
### 8. PL/SQL 사용자 정의 함수
- CREATE OR REPLACE FUNCTION
```sql
------------------------------------------------------------------
--익명 블록은 한 번 사용하고 나오면 없어져 버리는 휘발성 블록이지만
--함수나 프로시저는 컴파일을 거쳐 데이터베이스 내에 저장되어 재사용이 가능하다.

CREATE OR REPLACE FUNCTION FN_GET_COUNTRY_NAME (P_COUNTRY_ID NUMBER) --타입을 정해줘야됨
    RETURN VARCHAR2 --국가명을 반환하므로 반환 데이터 타입은 VARCHAR2 날짜면 DATE
  IS
    VS_COUNTRY_NAME COUNTRIES.COUNTRY_NAME%TYPE ;
    VN_COUNT NUMBER := 0;
  BEGIN
    SELECT COUNT(*)
    INTO VN_COUNT
    FROM COUNTRIES
    WHERE COUNTRY_ID = P_COUNTRY_ID ;
  IF VN_COUNT = 0 THEN
     VS_COUNTRY_NAME := '해당국가 없음';
  ELSE
    SELECT COUNTRY_NAME
    INTO   VS_COUNTRY_NAME
    FROM    COUNTRIES
    WHERE  COUNTRY_ID = P_COUNTRY_ID ;
  END IF ;
  RETURN VS_COUNTRY_NAME ; -- 국가명 반환
END ;
    
select FN_GET_COUNTRY_NAME(52790)
from DUAL;

------------------------------------------------------------------
CREATE OR REPLACE FUNCTION  함수이름(매개변수1 타입, 매개변수2 타입,....n)
CREATE OR REPLACE FUNCTION  함수이름 <-- 매개변수가 없을때
------------------------------------------------------------------
CREATE OR REPLACE FUNCTION  함수이름(매개변수1 타입, 매개변수2 타입,....n)
RETURN 데이터타입 ; 반환할 데이터의 타입

IS
    변수, 상수 선언
BEGIN
    실행부
    RETURN 반환값
    [EXCPEPTION 예외처리부]
END ;
------------------------------------------------------------------

--이름을 입력 받아서 학번을 리턴해주는 함수를 작성하시오.

CREATE OR REPLACE FUNCTION FN_GET_HAK_NUM (FS_HAK_NAME VARCHAR2)
RETURN VARCHAR2  

    IS
    FN_HAK_NUM VARCHAR2(100);
    FN_HAK NUMBER := 0;

      BEGIN
      SELECT COUNT(*)
      INTO FN_HAK
      FROM 학생
      WHERE 이름 = FS_HAK_NAME ;
      
  IF FN_HAK = 0 THEN
     FN_HAK_NUM := '해당학생 없음';
  ELSE
    SELECT 이름
    INTO   FN_HAK_NUM
    FROM    학생
    WHERE  이름 = FS_HAK_NAME ;
  END IF ;
  RETURN FN_HAK_NUM ;
END ;

------------------------------------------------------------------
--DATE 형태의 데이터를 받아서
--얼마남았나? 얼마나 지났나 리턴해주는 함수를 작성

CREATE OR REPLACE FUNCTION FN_GET_DAY (P_QDATE DATE)
    RETURN VARCHAR2
    
  IS
  VS_DDDD VARCHAR2(100) ;
  
BEGIN
 IF P_QDATE = SYSDATE THEN
    VS_DDDD := '오늘입니다' ;
 ELSIF
    P_QDATE > SYSDATE THEN
    VS_DDDD := TRUNC(P_QDATE - SYSDATE)+1 || '남음' ;
 ELSE
    VS_DDDD := TRUNC(SYSDATE - P_QDATE) || '지남' ;
  END IF ;
  RETURN VS_DDDD ;
  END;
  
SELECT FN_GET_DAY(SYSDATE)
FROM DUAL ;
    
   ```
### 9. 프로시저

- 함수는 클라이언트에서 실행되며 리턴값이 필수이지만
- 프로시저는 서버에서 실행되며 리턴값이 없이 또는 여러 개가 가능하다.
- 주기적으로 실행되는 것을 프로시저로 많이 함
- CREATE OR REPLACE PROCEDURE 프로시저 이름
- (매개변수명1 [IN, OUT, INOUT] 데이터 값)
- IN: 프로시저 내부에서 사용가능
- OUT : 프로시저 내부에서 사용 불가, 리턴만 받아옴
- INOUT : 둘다 가능
```sql
------------------------------------------------------------------

CREATE OR REPLACE PROCEDURE MY_NEW_JOB_PROC --프로시저 이름
         (P_JOB_ID   IN JOBS.JOB_ID%TYPE,
         P_JOB_TITLE IN JOBS.JOB_TITLE%TYPE,
         P_MIN_SAL   IN JOBS.MIN_SALARY%TYPE,
         P_MAX_SAL   IN JOBS.MAX_SALARY%TYPE)   -- 매개변수 정의
IS
BEGIN
    INSERT INTO JOBS (JOB_ID, JOB_TITLE, MIN_SALARY, MAX_SALARY, CREATE_DATE, UPDATE_DATE)
                VALUES (P_JOB_ID, P_JOB_TITLE, P_MIN_SAL, P_MAX_SAL, SYSDATE, SYSDATE);
    COMMIT;
END;
-- 프로시저 실행
-- SELECT절에 사용 불가
EXEC MY_NEW_JOB_PROC ('SM_JOB4', 'SAMPLE JOB1', 1000, 5000) ;   -- 무결성 제약조건으로 오류
EXECUTE MT_NEW_JOB_PROC('SM_JOB3', 'SAMPLE JOB1', 1000, 5000) ;

--삭제
DROP PROCEDURE MY_NEW_JOB_PROC ;


------------------------------------------------------------------
     
CREATE OR REPLACE PROCEDURE MY_PARAMETER_TEST_PROC (
                P_VAR1          VARCHAR2, -- 디폴트(기본적으로 들어가있는 값)가 IN
                P_VAR2      OUT VARCHAR2,
                P_VAR3   IN OUT VARCHAR2)
IS
BEGIN
        DBMS_OUTPUT.PUT_LINE('P_VAR1 VALUE = ' || P_VAR1);  -- IN : 내부실행 돼서 A
        DBMS_OUTPUT.PUT_LINE('P_VAR2 VALUE = ' || P_VAR2);  -- OUT : 내부실행 안 돼서 출력X
        DBMS_OUTPUT.PUT_LINE('P_VAR3 VALUE = ' || P_VAR3);  -- INOUT :내부실행 돼서 C 출력
        P_VAR2 := 'B2' ;    --OUT이라 리턴
        P_VAR3 := 'C2' ;    --INOUT이라 리턴
END;

-- IN, OUT, IN OUT 변수 테스트
DECLARE     --해야 실행
    V_VAR1 VARCHAR2(10) := 'A';
    V_VAR2 VARCHAR2(10) := 'B';
    V_VAR3 VARCHAR2(10) := 'C';
BEGIN
    MY_PARAMETER_TEST_PROC(V_VAR1, V_VAR2, V_VAR3);
    DBMS_OUTPUT.PUT_LINE('V_VAR2 VALUE = ' || V_VAR2);
    DBMS_OUTPUT.PUT_LINE('V_VAR3 VALUE = ' || V_VAR3);
END ;
--P_VAR1 VALUE = A
--P_VAR2 VALUE =
--P_VAR3 VALUE = C
--V_VAR2 VALUE = B2
--V_VAR3 VALUE = C2
 ```
 - 프로시저 문제
 ```sql
------------------------------------------------------------------
--'이름을 입력받아' 학생이 있으면 'Y' 평점이
-- 3이상이면 '합격' 텍스트와 '학번'을 리턴
-- 3미만이면 '불합격' 텍스트와 '학번'을 리턴하는 프로시저를 만드시오.
-- 없으면 'N'
-- 입력1, 리턴1, 리턴, 리턴3
-- ('양지운', 'y' '불합격', '2002110112')

CREATE OR REPLACE PROCEDURE GRADE_PROC (
                P_NAME          VARCHAR2, -- 디폴트(기본적으로 들어가있는 값)가 IN
                P_NY        OUT VARCHAR2,
                P_PF        OUT VARCHAR2,
                P_NUM       OUT VARCHAR2)
        
    IS
    VN_COUNT NUMBER := 0;
    VN_GRADE VARCHAR2(100) ;
                
BEGIN
      SELECT COUNT(*)
      INTO VN_COUNT
      FROM 학생
      WHERE 이름 = P_NAME ;
      
       IF VN_COUNT = 0 THEN
          P_NY := 'N';
          ELSE      
            P_NY := 'Y';
            
            SELECT 평점, 학번       --ELSE에 대한 SELECT
             INTO   VN_GRADE, P_NUM
             FROM   학생
             WHERE  이름 = P_NAME ;
                    IF VN_GRADE >= 3 THEN
                    P_PF := '합격';
                    ELSE
                    P_PF := '불합격';
                    END IF ;
        END IF ;
END ;   

------------------------------------------------------------------
--테스트

DECLARE
                V_NAME       VARCHAR2(100)   := :A; --입력 받을 값, 입력창 뜬다.
                V_NY         VARCHAR2(100)   ;
                P_PF         VARCHAR2(100)   ;
                P_NUM        VARCHAR2(100)   ;
BEGIN
    GRADE_PROC(V_NAME, V_NY, P_PF, P_NUM);
    IF V_NY = 'Y' THEN
    DBMS_OUTPUT.PUT_LINE('이름 = ' || V_NAME);
    DBMS_OUTPUT.PUT_LINE('당락 = ' || P_PF);
    DBMS_OUTPUT.PUT_LINE('학번 = ' || P_NUM);
    
    ELSE
    DBMS_OUTPUT.PUT_LINE('학생이 없습니다.' );
    END IF;
END ;   
------------------------------------------------------------------
```

```sql
---------------------------------------------------------------------------------------------------------------------------
--문제를 위해 다음과 같이 부서테이블의 복사본을 만든다.

----(1) 테이블 생성
CREATE TABLE ch10_departments
AS
SELECT department_id, department_name
  FROM departments;

----(2) 테이블 제약조건 추가
ALTER TABLE ch10_departments
ADD CONSTRAINTS pk_ch10_departments PRIMARY KEY (department_id);  
  

--실행예제  : exec ch10_departments_crud(flag, 부서번호, 부서명);

--flag I ->   INSERT 입력 받은 2개 매개변수를 입력
--flag U ->   UPDATE ID 값에 해당하는 것에 name 을 업데이트
--flag D ->   DELETE ID 값에 해당하는 것을 삭제

--부서번호, 부서명, 작업 flag(I: insert, U:update, D:delete)을
--매개변수로 받아 ch10_departments 테이블에
--각각 INSERT, UPDATE, DELETE 하는 ch10_iud_dep_proc 란 이름의 프로시저를 만들어보자.

--ex) EXEC ch10_iud_dep_proc ('I', 280, '개발2팀');  -- INSERT됨
--ex) EXEC ch10_iud_dep_proc ('U', 280, '개발3팀');  -- UPDATE됨
--ex) EXEC ch10_iud_dep_proc ('D', 280);           -- DELETE됨



CREATE OR REPLACE PROCEDURE ch10_iud_dep_proc(
         P_FLAG             VARCHAR2
        ,P_DEPARTMENT_ID    ch10_departments.department_id%TYPE
        ,P_DEPARTMENT_NAME  ch10_departments.department_NAME% TYPE := NULL
)
IS
BEGIN
    IF (P_FLAG = 'I') THEN
    INSERT INTO ch10_departments(DEPARTMENT_ID, DEPARTMENT_NAME)
    VALUES(P_DEPARTMENT_ID, P_DEPARTMENT_NAME);
    
    ELSIF(P_FLAG = 'U') THEN
    UPDATE ch10_departments
    SET DEPARTMENT_NAME = P_DEPARTMENT_NAME
    WHERE DEPARTMENT_ID = P_DEPARTMENT_ID ;


    ELSIF(P_FLAG = 'D') THEN
    DELETE
    FROM ch10_departments
    WHERE DEPARTMENT_ID = P_DEPARTMENT_ID ;
END IF ;
COMMIT;
END;

DECLARE
    VS_FLAG VARCHAR2(100) := :1;
    VS_DEPARTMENT_ID VARCHAR2(100) := :2;
    VS_DEPARTMENT_NAME VARCHAR2(100) := :3;
BEGIN
    ch10_iud_dep_proc(VS_FLAG, VS_DEPARTMENT_ID, VS_DEPARTMENT_NAME);
    END;


select *
from ch10_departments;
```
```sql
------------------------------------------------------------------------------------------
	 INSERT INTO employees ( employee_id, emp_name, hire_date, department_id )
              VALUES ( vn_employee_id, p_emp_name, TO_DATE(p_hire_month || '01'), p_department_id );
              
   COMMIT;              
              
EXCEPTION WHEN ex_invalid_depid THEN -- 사용자 정의 예외 처리
               DBMS_OUTPUT.PUT_LINE('성공');   
          WHEN OTHERS THEN
               DBMS_OUTPUT.PUT_LINE('해당 부서번호가 없습니다');              
	
END;    

------------------------------------------------------------------------------------------

EXEC ch10_iud_dep_proc ('I', 10, '총무기획부');

ch10_departments 테이블은 department_id가 PRIMARY KEY인데 이미 존재하는 10번 부서에 대해 INSERT 작업을 하므로
시스템 오류(무결성 제약조건 위반)가 발생한다.  다음장 예외처리에서 해결
-----------------------------------------------------------------------------------------
```
## 📚 7장. 예외처리와 트랜잭션
### 1. 예외처리
- 개발하다 보면 다양한 경우의 수를 산정해서 오류 확인 및 예외처리를 한다.
- 실행할 때 발생하는 오류를 예외라고 한다.
- 예외에는 시스템 예외와 사용자 정의 예외(오라클에서는 예외가 아니지만 사용자가 예외로 지정)로 구분
```sql
EXCEPTION WHEN 예외명1 THEN 예외처리1 구문2
          WHEN 예외명2 THEN 예외처리2 구문2
          ....
          WHEN OTEHRS THEN 예외처리 구문n;
          

--division


DECLARE
    VI_NUM NUMBER := 0;
BEGIN
    VI_NUM := 10 / 0 ;
    DBMS_OUTPUT.PUT_LINE('SUCCESS!')
EXCEPTION WHEN ZERO DIVIDE THEN
          DBMS_OUTPUT.PUT_LINE('ZERO_DIVIDE 오류가 발생했습니다.')

          WHEN OTHERS THEN
          DBMS_OUTPUT.PUT_LINE('오류가 발생했습니다')

--------------------------------------------------
CREATE OR REPLACE PROCEDURE ch10_exception_proc
IS
  vi_num NUMBER := 0;
BEGIN
	vi_num := 10 / 0;
	DBMS_OUTPUT.PUT_LINE('Success!');
	
EXCEPTION WHEN ZERO_DIVIDE THEN
	          	 DBMS_OUTPUT.PUT_LINE('오류1');		
	             DBMS_OUTPUT.PUT_LINE('SQL ERROR MESSAGE1: ' || SQLERRM);
	        WHEN OTHERS THEN
	          	 DBMS_OUTPUT.PUT_LINE('오류2');		
	             DBMS_OUTPUT.PUT_LINE('SQL ERROR MESSAGE2: ' || SQLERRM);	
END;	

EXEC ch10_exception_proc;
```
- 오류구문
```sql
-------------------------------------------------- 오류구문

CREATE OR REPLACE PROCEDURE CH10_NO_EXCEPTION_PROC
IS
    VI_NUM NUMBER := 0;
BEGIN
    VI_NUM := 10 / 0 ;
    DBMS_OUTPUT.PUT_LINE('SUCCESS!');
END;

-- 오류 테스트
BEGIN
    CH10_NO_EXCEPTION_PROC;
    DBMS_OUTPUT.PUT_LINE('SUCCESS!');
END;
```
- 오류나지 않게 수정
```sql
-------------------------------------------------- 오류 나지 않게 수정
CREATE OR REPLACE PROCEDURE CH10_EXCEPTION_PROC
IS
    VI_NUM NUMBER := 0;
BEGIN
    VI_NUM := 10 / 0 ;
    DBMS_OUTPUT.PUT_LINE('SUCCESS!');
EXCEPTION WHEN OTHERS THEN
    DBMS_OUTPUT.PUT_LINE('오류가 발생했습니다');
    
END;
```
- 오류 수정 테스트
```sql
-- 오류 수정 테스트
BEGIN
    CH10_EXCEPTION_PROC;
    DBMS_OUTPUT.PUT_LINE('SUCCESS!'|| SQLCODE);
    DBMS_OUTPUT.PUT_LINE('SUCCESS!'|| SQLERRM);
    DBMS_OUTPUT.PUT_LINE(SQLERRM(SQLCODE);
END;

--------------------------------------------------
EXEC CH10_EXCEPTION_PROC ;
-- SQLCODE, SQLERRM을 이용한 예외정보 참조

            CREATE OR REPLACE PROCEDURE ch10_exception_proc
            IS
              vi_num NUMBER := 0;
            BEGIN
	            vi_num := 10 / 0;
	            DBMS_OUTPUT.PUT_LINE('Success!');
	            
            EXCEPTION WHEN OTHERS THEN
             DBMS_OUTPUT.PUT_LINE('오류가 발생했습니다');		
             DBMS_OUTPUT.PUT_LINE( 'SQL ERROR CODE: '    || SQLCODE);
             DBMS_OUTPUT.PUT_LINE( 'SQL ERROR MESSAGE: ' || SQLERRM); -- 매개변수 없는 SQLERRM
             DBMS_OUTPUT.PUT_LINE( SQLERRM(SQLCODE)); -- 매개변수 있는 SQLERRM

            END;	

            EXEC ch10_exception_proc;
```
- RETURN문
```sql
--------------------------------------------------
-- RETURN 문
CREATE OR REPLACE PROCEDURE my_new_job_proc
          ( p_job_id    IN  JOBS.JOB_ID%TYPE,
            p_job_title IN  JOBS.JOB_TITLE%TYPE,
            p_min_sal   IN  JOBS.MIN_SALARY%TYPE := 10,
            p_max_sal   IN  JOBS.MAX_SALARY%TYPE := 100
            --p_upd_date  OUT JOBS.UPDATE_DATE%TYPE  
            )
IS
  vn_cnt NUMBER := 0;
  vn_cur_date JOBS.UPDATE_DATE%TYPE := SYSDATE;
BEGIN
	-- 1000 보다 작으면 메시지 출력 후 빠져나간다
	IF p_min_sal < 1000 THEN
	   DBMS_OUTPUT.PUT_LINE('최소 급여값은 1000 이상이어야 합니다');
	   RETURN;
  END IF;
	
	-- 동일한 job_id가 있는지 체크
	SELECT COUNT(*)
	  INTO vn_cnt
	  FROM JOBS
	 WHERE job_id = p_job_id;
	
	-- 없으면 INSERT
	IF vn_cnt = 0 THEN
	
	   INSERT INTO JOBS ( job_id, job_title, min_salary, max_salary, create_date, update_date)
	             VALUES ( p_job_id, p_job_title, p_min_sal, p_max_sal, vn_cur_date, vn_cur_date);
	             
	ELSE -- 있으면 UPDATE
	
	   UPDATE JOBS
	      SET job_title   = p_job_title,
	          min_salary  = p_min_sal,
	          max_salary  = p_max_sal,
	          update_date = vn_cur_date
	    WHERE job_id = p_job_id;
	
  END IF;  
	COMMIT;
END ;   

EXEC my_new_job_proc ('SM_JOB1', 'Sample JOB1', 999, 6000); --최소 급여값은 1000 이상이어야 합니다


--------------------------------------------------


CREATE OR REPLACE PROCEDURE ch10_upd_jobid_proc
                  ( p_employee_id employees.employee_id%TYPE,
                    p_job_id      jobs.job_id%TYPE )
IS
  vn_cnt NUMBER := 0;
BEGIN
	SELECT COUNT(*)
	  INTO vn_cnt
	  FROM JOBS
	 WHERE JOB_ID = p_job_id;
	IF vn_cnt = 0 THEN
	   DBMS_OUTPUT.PUT_LINE('job_id가 없습니다');
	   RETURN;
	ELSE
	   UPDATE employees
	      SET job_id = p_job_id
	    WHERE employee_id = p_employee_id;
  END IF;
  
  COMMIT;
	
END;

EXEC ch10_upd_jobid_proc (200, 'SM_JOB2');

--------------------------------------------------

CREATE OR REPLACE PROCEDURE ch10_upd_jobid_proc
                  ( p_employee_id employees.employee_id%TYPE,
                    p_job_id      jobs.job_id%TYPE )
IS
  vn_cnt NUMBER := 0;
BEGIN
	SELECT 1
	  INTO vn_cnt
	  FROM JOBS
	WHERE JOB_ID = p_job_id;
	
   UPDATE employees
      SET job_id = p_job_id
    WHERE employee_id = p_employee_id;
	
  COMMIT;
  
  EXCEPTION WHEN NO_DATA_FOUND THEN
                 DBMS_OUTPUT.PUT_LINE(SQLERRM);
                 DBMS_OUTPUT.PUT_LINE(p_job_id ||'에 해당하는 job_id가 없습니다');
            WHEN OTHERS THEN
                 DBMS_OUTPUT.PUT_LINE('기타 에러: ' || SQLERRM);
END;
                   

EXEC ch10_upd_jobid_proc (100, 'SM_JOB4');
--------------------------------------------------


CREATE OR REPLACE PROCEDURE ch10_upd_jobid_proc
                  ( p_employee_id employees.employee_id%TYPE,
                    p_job_id      jobs.job_id%TYPE)
IS
  vn_cnt NUMBER := 0;
BEGIN
	
	SELECT 1
	  INTO vn_cnt
	  FROM JOBS
	 WHERE JOB_ID = p_job_id;
	
   UPDATE employees
      SET job_id = p_job_id
    WHERE employee_id = p_employee_id;
	
  COMMIT;
  
  EXCEPTION WHEN NO_DATA_FOUND THEN
                 DBMS_OUTPUT.PUT_LINE(SQLERRM);
                 DBMS_OUTPUT.PUT_LINE(p_job_id ||'에 해당하는 job_id가 없습니다');
            WHEN OTHERS THEN
                 DBMS_OUTPUT.PUT_LINE('기타 에러: ' || SQLERRM);
END;
```
- 사용자 정의 예외
```sql
-- 사용자 정의 예외
-- 시스템 예외 이외에 사용자가 직접 예외를 정의

    CREATE OR REPLACE PROCEDURE ch10_ins_emp_proc (
                      p_emp_name       employees.emp_name%TYPE,
                      p_department_id  departments.department_id%TYPE )
    IS
       vn_employee_id  employees.employee_id%TYPE;
       vd_curr_date    DATE := SYSDATE;
       vn_cnt          NUMBER := 0;
       ex_invalid_depid EXCEPTION; -- 잘못된 부서번호일 경우 예외 정의
    BEGIN
	     -- 부서테이블에서 해당 부서번호 존재유무 체크
	     SELECT COUNT(*)
	       INTO vn_cnt
	       FROM departments
	      WHERE department_id = p_department_id;
	     IF vn_cnt = 0 THEN
	        RAISE ex_invalid_depid; -- 사용자 정의 예외 발생
	     END IF;
	     -- employee_id의 max 값에 +1
	     SELECT MAX(employee_id) + 1
	       INTO vn_employee_id
	       FROM employees;
	     -- 사용자예외처리 예제이므로 사원 테이블에 최소한 데이터만 입력함
	     INSERT INTO employees ( employee_id, emp_name, hire_date, department_id )
                  VALUES ( vn_employee_id, p_emp_name, vd_curr_date, p_department_id );
       COMMIT;        
          
    EXCEPTION WHEN ex_invalid_depid THEN -- 사용자 정의 예외 처리
                   DBMS_OUTPUT.PUT_LINE('해당 부서번호가 없습니다');
              WHEN OTHERS THEN
                   DBMS_OUTPUT.PUT_LINE(SQLERRM);              
    END;                	


EXEC ch10_ins_emp_proc ('홍길동', 999);--해당 부서번호가 없습니다.
--------------------------------------------------

CREATE OR REPLACE PROCEDURE ch10_ins_emp_proc (
                  p_emp_name       employees.emp_name%TYPE,
                  p_department_id  departments.department_id%TYPE,
                  p_hire_month  VARCHAR2  )
IS
   vn_employee_id  employees.employee_id%TYPE;
   vd_curr_date    DATE := SYSDATE;
   vn_cnt          NUMBER := 0;
   
   ex_invalid_depid EXCEPTION; -- 잘못된 부서번호일 경우 예외 정의
   
   ex_invalid_month EXCEPTION; -- 잘못된 입사월인 경우 예외 정의
   PRAGMA EXCEPTION_INIT ( ex_invalid_month, -1843); -- 예외명과 예외코드 연결
BEGIN
	
	 -- 부서테이블에서 해당 부서번호 존재유무 체크
	 SELECT COUNT(*)
	   INTO vn_cnt
	   FROM departments
	  WHERE department_id = p_department_id;
	  
	 IF vn_cnt = 0 THEN
	    RAISE ex_invalid_depid; -- 사용자 정의 예외 발생
	 END IF;
	
	 -- 입사월 체크 (1~12월 범위를 벗어났는지 체크)
	 IF SUBSTR(p_hire_month, 5, 2) NOT BETWEEN '01' AND '12' THEN
	    RAISE ex_invalid_month; -- 사용자 정의 예외 발생
	
	 END IF;
	
	
	 -- employee_id의 max 값에 +1
	 SELECT MAX(employee_id) + 1
	   INTO vn_employee_id
	   FROM employees;
	
	 -- 사용자예외처리 예제이므로 사원 테이블에 최소한 데이터만 입력함
	 INSERT INTO employees ( employee_id, emp_name, hire_date, department_id )
              VALUES ( vn_employee_id, p_emp_name, TO_DATE(p_hire_month || '01'), p_department_id );
              
   COMMIT;              
              
EXCEPTION WHEN ex_invalid_depid THEN -- 사용자 정의 예외 처리
               DBMS_OUTPUT.PUT_LINE('해당 부서번호가 없습니다');
          WHEN ex_invalid_month THEN -- 입사월 사용자 정의 예외 처리
               DBMS_OUTPUT.PUT_LINE(SQLCODE);
               DBMS_OUTPUT.PUT_LINE(SQLERRM);
               DBMS_OUTPUT.PUT_LINE('1~12월 범위를 벗어난 월입니다');               
          WHEN OTHERS THEN
               DBMS_OUTPUT.PUT_LINE(SQLERRM);              
	
END;    

EXEC ch10_ins_emp_proc ('홍길동', 110, '201314'); -- 1~12월 범위를 벗어난 월입니다
--------------------------------------------------

```

- 시스템 예외
```sql
-------------------------------------------------------------------------------------
-- divisor is equal to zero 오류
DECLARE
   vi_num NUMBER := 0;
BEGIN
	 vi_num := 10 / 0;
	 DBMS_OUTPUT.PUT_LINE('Success!');     --- 실행되지 않음
EXCEPTION WHEN OTHERS THEN
	 DBMS_OUTPUT.PUT_LINE('오류가 발생했습니다');	
END;

-----------------------------------------------------  오류구문
CREATE OR REPLACE PROCEDURE ch10_no_exception_proc
IS
  vi_num NUMBER := 0;
BEGIN
    vi_num := 10 / 0;
    DBMS_OUTPUT.PUT_LINE('Success!');
END;	
------------------------------------------------------ 오류가 나지 않게 수정
CREATE OR REPLACE PROCEDURE ch10_exception_proc
IS
  vi_num NUMBER := 0;
BEGIN
    vi_num := 10 / 0;
    DBMS_OUTPUT.PUT_LINE('Success!');
EXCEPTION WHEN OTHERS THEN
     DBMS_OUTPUT.PUT_LINE('오류가 발생했습니다');		
END;	

BEGIN
     ch10_no_exception_proc;            -- 해당 프로시져 오류 발생으로 중단됨
     DBMS_OUTPUT.PUT_LINE('Success!');
END;
 ---------------------------------------------
BEGIN
     ch10_exception_proc;               -- 해당 프로시져 오류 발생되지만 예외처리로
     DBMS_OUTPUT.PUT_LINE('Success!');  -- 중단되지 않고 정상종료
END;
```
- SQLCODE 실행부에서 발생한 예외에 해당하는 코드를 반환
- SQLERRM 발생한 예외에 대한 오류 메시지 반환.
```sql
CREATE OR REPLACE PROCEDURE ch10_exception_proc
IS
  vi_num NUMBER := 0;
BEGIN
    vi_num := 10 / 0;
    DBMS_OUTPUT.PUT_LINE('Success!');
    
EXCEPTION WHEN OTHERS THEN
 DBMS_OUTPUT.PUT_LINE('오류가 발생했습니다');		
 DBMS_OUTPUT.PUT_LINE( 'SQL ERROR CODE: '    || SQLCODE);
 DBMS_OUTPUT.PUT_LINE( 'SQL ERROR MESSAGE: ' || SQLERRM); -- 매개변수 없는 SQLERRM
END;	

EXEC ch10_exception_proc;

```

- 시스템 예외명 있는 오류
```sql

CREATE OR REPLACE PROCEDURE ch10_exception_proc
IS
  vi_num NUMBER := 0;
BEGIN
	vi_num := 10 / 0;
	DBMS_OUTPUT.PUT_LINE('Success!');
EXCEPTION WHEN ZERO_DIVIDE THEN      --<-- ZERO_DIVIDE 정의되어있는 오류
	 DBMS_OUTPUT.PUT_LINE('오류가 발생했습니다');		
	 DBMS_OUTPUT.PUT_LINE('SQL ERROR CODE: ' || SQLCODE);
	 DBMS_OUTPUT.PUT_LINE('SQL ERROR MESSAGE: ' || SQLERRM);
END;	

EXEC ch10_exception_proc;

CREATE OR REPLACE PROCEDURE ch10_exception_proc
IS
  vi_num NUMBER := 0;
BEGIN
	vi_num := 10 / 0;
	DBMS_OUTPUT.PUT_LINE('Success!');
	
EXCEPTION WHEN ZERO_DIVIDE THEN
	          	 DBMS_OUTPUT.PUT_LINE('오류1');		
	             DBMS_OUTPUT.PUT_LINE('SQL ERROR MESSAGE1: ' || SQLERRM);
	        WHEN OTHERS THEN
	          	 DBMS_OUTPUT.PUT_LINE('오류2');		
	             DBMS_OUTPUT.PUT_LINE('SQL ERROR MESSAGE2: ' || SQLERRM);	
END;	

EXEC ch10_exception_proc;


CREATE OR REPLACE PROCEDURE ch10_upd_jobid_proc
                  ( p_employee_id employees.employee_id%TYPE,
                    p_job_id      jobs.job_id%TYPE )
IS
  vn_cnt NUMBER := 0;
BEGIN
	SELECT COUNT(*)
	  INTO vn_cnt
	  FROM JOBS
	 WHERE JOB_ID = p_job_id;
	IF vn_cnt = 0 THEN
	   DBMS_OUTPUT.PUT_LINE('job_id가 없습니다');
	   RETURN;              --RETURN 위에 에러 있으면 바로 빠져나간다.
	ELSE
	   UPDATE employees
	      SET job_id = p_job_id
	    WHERE employee_id = p_employee_id;
  END IF;
  
  COMMIT;
	
END;

EXEC ch10_upd_jobid_proc (200, 'SM_JOB2');



CREATE OR REPLACE PROCEDURE ch10_upd_jobid_proc
                  ( p_employee_id employees.employee_id%TYPE,
                    p_job_id      jobs.job_id%TYPE )
IS
  vn_cnt NUMBER := 0;
BEGIN
	SELECT COUNT(*)
	  INTO vn_cnt
	  FROM JOBS
	WHERE JOB_ID = p_job_id;
	
   UPDATE employees
      SET job_id = p_job_id
    WHERE employee_id = p_employee_id;
	
  COMMIT;
  
  EXCEPTION WHEN NO_DATA_FOUND THEN
                 DBMS_OUTPUT.PUT_LINE(SQLERRM);
                 DBMS_OUTPUT.PUT_LINE(p_job_id ||'에 해당하는 job_id가 없습니다');
            WHEN OTHERS THEN
                 DBMS_OUTPUT.PUT_LINE('기타 에러: ' || SQLERRM);
END;
                   

EXEC ch10_upd_jobid_proc (100, 'SM_JOB4');


CREATE OR REPLACE PROCEDURE ch10_upd_jobid_proc
                  ( p_employee_id employees.employee_id%TYPE,
                    p_job_id      jobs.job_id%TYPE)
IS
  vn_cnt NUMBER := 0;
BEGIN
	
	SELECT 1
	  INTO vn_cnt
	  FROM JOBS
	 WHERE JOB_ID = p_job_id;
	
   UPDATE employees
      SET job_id = p_job_id
    WHERE employee_id = p_employee_id;
	
  COMMIT;
  
  EXCEPTION WHEN NO_DATA_FOUND THEN
                 DBMS_OUTPUT.PUT_LINE(SQLERRM);
                 DBMS_OUTPUT.PUT_LINE(p_job_id ||'에 해당하는 job_id가 없습니다');
            WHEN OTHERS THEN
                 DBMS_OUTPUT.PUT_LINE('기타 에러: ' || SQLERRM);
END;
```
## 예외 복습 
#### 1.사용자 정의 예외
-   시스템 예외 이외에 사용자가 직접 예외를 정의
-   개발자가 직접 예외를 정의하는 방법.
(1) 예외 정의 : 사용자_정의_예외명 EXCEPTION;
(2) 예외발생시키기 : RAISE 사용자_정의_예외명;
    - 시스템 예외는 해당 예외가 자동으로 검출 되지만, 사용자 정의 예외는 직접 예외를 발생시켜야 한다.
    - RAISE 예외명 형태로 사용한다.
  
    (3) 발생된 예외 처리 : EXCEPTION WHEN 사용자_정의_예외명 THEN ..
```sql

    CREATE OR REPLACE PROCEDURE ch10_ins_emp_proc (
                      p_emp_name       employees.emp_name%TYPE,
                      p_department_id  departments.department_id%TYPE )
    IS
       vn_employee_id  employees.employee_id%TYPE;
       vd_curr_date    DATE := SYSDATE;
       vn_cnt          NUMBER := 0;
       ex_invalid_depid EXCEPTION; -- (1) 잘못된 부서번호일 경우 예외 정의
    BEGIN
	     -- 부서테이블에서 해당 부서번호 존재유무 체크
	     SELECT COUNT(*)
	       INTO vn_cnt
	       FROM departments
	      WHERE department_id = p_department_id;
          
	     IF vn_cnt = 0 THEN
	        RAISE ex_invalid_depid; -- (2) 사용자 정의 예외 발생 - 발생하면 EXCEPTION WHEN으로 간다.
            
	     END IF;
	     -- employee_id의 max 값에 +1
	     SELECT MAX(employee_id) + 1
	       INTO vn_employee_id
	       FROM employees;
	     -- 사용자예외처리 예제이므로 사원 테이블에 최소한 데이터만 입력함
         
	     INSERT INTO employees ( employee_id, emp_name, hire_date, department_id )
                  VALUES ( vn_employee_id, p_emp_name, vd_curr_date, p_department_id );
      
       COMMIT;        
          
    EXCEPTION WHEN ex_invalid_depid THEN --(3) 사용자 정의 예외 처리구간
                   DBMS_OUTPUT.PUT_LINE('해당 부서번호가 없습니다');
              WHEN OTHERS THEN
                   DBMS_OUTPUT.PUT_LINE(SQLERRM);              
    END;                	

EXEC ch10_ins_emp_proc ('홍길동', 999);
```


#### 2. 시스템 예외에 이름 부여하기
 - 시스템 예외에는 ZERO_DIVIDE, INVALID_NUMBER .... 와같이 정의된 예외가 있다 하지만 이들처럼 예외명이 부여된 것은 시스템 예외 중 극소수이고 나머지는 예외코드만 존재한다. 이름이 없는 코드에 이름 부여하기.

1. 사용자 정의 예외 선언
2. 사용자 정의 예외명과 시스템 예외 코드 연결 (PRAGMA EXCEPTION_INIT(사용자 정의 예외명, 시스템_예외_코드)

		/*
		   PRAGMA 컴파일러가 실행되기 전에 처리하는 전처리기 역할

		   PRAGMA EXCEPTION_INIT(예외명, 예외번호)
		   사용자 정의 예외 처리를 할 때 사용되는것으로
		   특정 예외번호를 명시해서 컴파일러에 이 예외를 사용한다는 것을 알리는 역할
		   (해당 예외번호에 해당되는 시스템 에러시 발생)
    
		*/
3. 발생된 예외 처리:EXCEPTION WHEN 사용자 정의 예외명 THEN .... 
```sql
CREATE OR REPLACE PROCEDURE ch10_ins_emp_proc (
                  p_emp_name       employees.emp_name%TYPE,
                  p_department_id  departments.department_id%TYPE,
                  p_hire_month  VARCHAR2  )
IS
   vn_employee_id  employees.employee_id%TYPE;
   vd_curr_date    DATE := SYSDATE;
   vn_cnt          NUMBER := 0;
   
   ex_invalid_depid EXCEPTION; -- 잘못된 부서번호일 경우 예외 정의
   
   ex_invalid_month EXCEPTION; -- 잘못된 입사월인 경우 예외 정의
   PRAGMA EXCEPTION_INIT (ex_invalid_month, -1843); -- 예외명과 예외코드 연결
BEGIN
	
	 -- 부서테이블에서 해당 부서번호 존재유무 체크
	 SELECT COUNT(*)
	   INTO vn_cnt
	   FROM departments
	  WHERE department_id = p_department_id;
	  
	 IF vn_cnt = 0 THEN
	    RAISE ex_invalid_depid; -- 사용자 정의 예외 발생
	 END IF;
	
	 -- 입사월 체크 (1~12월 범위를 벗어났는지 체크)
	 IF SUBSTR(p_hire_month, 5, 2) NOT BETWEEN '01' AND '12' THEN
	    RAISE ex_invalid_month; -- 사용자 정의 예외 발생
	
	 END IF;
	
	
	 -- employee_id의 max 값에 +1
	 SELECT MAX(employee_id) + 1
	   INTO vn_employee_id
	   FROM employees;
	
	 -- 사용자예외처리 예제이므로 사원 테이블에 최소한 데이터만 입력함
	 INSERT INTO employees ( employee_id, emp_name, hire_date, department_id )
              VALUES ( vn_employee_id, p_emp_name, TO_DATE(p_hire_month || '01'), p_department_id );
              
   COMMIT;              
              
EXCEPTION WHEN ex_invalid_depid THEN -- 사용자 정의 예외 처리
               DBMS_OUTPUT.PUT_LINE('해당 부서번호가 없습니다');
          WHEN ex_invalid_month THEN -- 입사월 사용자 정의 예외
               DBMS_OUTPUT.PUT_LINE(SQLCODE);
               DBMS_OUTPUT.PUT_LINE(SQLERRM);
               DBMS_OUTPUT.PUT_LINE('1~12월 범위를 벗어난 월입니다');               
          WHEN OTHERS THEN
               DBMS_OUTPUT.PUT_LINE(SQLERRM);              	
END;    

EXEC ch10_ins_emp_proc ('홍길동', 110, '201314');
```

#### 3. 사용자 예외를 시스템 예외에 정의된 예외명을 사용
- RAISE 사용자 정의 예외 발생시 오라클에서 정의되어 있는 예외를 발생 시킬수 있다.

```sql
CREATE OR REPLACE PROCEDURE ch10_raise_test_proc ( p_num NUMBER)
IS
--선언부에 아무것도 없다.
BEGIN
	IF p_num <= 0 THEN
	   RAISE INVALID_NUMBER; --오라클에 정의되어있는 오류 ex)INVALID_NUMBER
  END IF;
  
  DBMS_OUTPUT.PUT_LINE(p_num);
  
EXCEPTION WHEN INVALID_NUMBER THEN
               DBMS_OUTPUT.PUT_LINE('양수만 입력받을 수 있습니다');
          WHEN OTHERS THEN
               DBMS_OUTPUT.PUT_LINE(SQLERRM);
	
END;

EXEC ch10_raise_test_proc (-10);   
```


#### 4. 예외를 발생시킬 수 있는 내장 프로시저 
- RAISE_APPLICATOIN_ERROR(예외코드, 예외 메세지);
- 예외 코드와 메세지를 사용자가 직접 정의 /  -20000 ~ -20999 번까지 만 사용가능
- 왜냐면 오라클에서 이미 사용하고 있는 예외들이 위 번호 구간은 사용하지 않고 있기 때문에

```sql

CREATE OR REPLACE PROCEDURE ch10_raise_test_proc ( p_num NUMBER)
IS

BEGIN
	IF p_num <= 0 THEN
	   RAISE_APPLICATION_ERROR (-20000, '양수만 입력받을 수 있단 말입니다!');
	END IF;
  
  DBMS_OUTPUT.PUT_LINE(p_num);
  
EXCEPTION WHEN OTHERS THEN
               DBMS_OUTPUT.PUT_LINE(SQLCODE);
               DBMS_OUTPUT.PUT_LINE(SQLERRM);
	
END;

EXEC ch10_raise_test_proc (-10);       
```
### 2. 트랜잭션
- 트랜잭션은 '거래'라는 뜻으로 은행에서 사용하는 입금과 출금을 하는 거래를 말한다.
- 성공적으로 절차가 끝나면 이를 완전한 거래로 승인하고
- 거래 도중 오류가 발생하면 아예 없던 거래로 되돌리는 것이다.
- 이러한 거래의 안정성을 확보하는  방법이 트랜잭션이다.

          트랜잭션 Transaction '거래'
          은행에서 입금과 출금을 하는 그 거래를 말하는 단어로
          프로그래밍 언어나 오라클에서 말하는 트랜잭션도 이 개념에서 차용한것

          A 은행 (출금 하여 송금) -> B 은행
          송금 중에 오류가 발생
          A 은행 계좌에서 돈이 빠져나가고
          B 은행 계좌에 입금되지 않음.

          오류를 파악하여 A계좌 출금 취소 or 출금된 만큼 B 은행으로 다시 송금
          but 어떤 오류인지 파악하여 처리하기에는 많은 문제점이 있다.

          그래서 나온 해결책 -> 거래가 성공적으로 모두 끝난 후에야 이를 완전한 거래로 승인,
                               거래 도중 뭔가 오류가 발생했을 때는 이 거래를 처음부터 없었던 거래로 되돌린다.

          거래의 안정성을 확보하는 방법이 바로 트랜잭션
1. COMMIT 과 ROLLBACK
```sql        
-------------------------------------------------------------------------------------

        CREATE TABLE ch10_sales (
               sales_month   VARCHAR2(8),
               country_name  VARCHAR2(40),
               prod_category VARCHAR2(50),
               channel_desc  VARCHAR2(20),
               sales_amt     NUMBER );
               
               
        CREATE OR REPLACE PROCEDURE iud_ch10_sales_proc
                    ( p_sales_month ch10_sales.sales_month%TYPE )
        IS

        BEGIN
	        INSERT INTO ch10_sales (sales_month, country_name, prod_category, channel_desc, sales_amt)
	        SELECT A.SALES_MONTH,
               C.COUNTRY_NAME,
               D.PROD_CATEGORY,
               E.CHANNEL_DESC,
               SUM(A.AMOUNT_SOLD)
          FROM SALES A, CUSTOMERS B, COUNTRIES C, PRODUCTS D, CHANNELS E
         WHERE A.SALES_MONTH = p_sales_month
           AND A.CUST_ID = B.CUST_ID
           AND B.COUNTRY_ID = C.COUNTRY_ID
           AND A.PROD_ID = D.PROD_ID
           AND A.CHANNEL_ID = E.CHANNEL_ID
         GROUP BY A.SALES_MONTH,
                 C.COUNTRY_NAME,
               D.PROD_CATEGORY,
               E.CHANNEL_DESC;
	        
        END;            
        -- COMMIT을 안 했기 때문에 물리적으로 저장된 게 아니다.
        EXEC iud_ch10_sales_proc ( '199901');



        -- sqlplus 접속하여 조회
        -- 건수가 0
        SELECT COUNT(*)
        FROM ch10_sales ;


        CREATE OR REPLACE PROCEDURE iud_ch10_sales_proc
                    ( p_sales_month ch10_sales.sales_month%TYPE )
        IS

        BEGIN
	        INSERT INTO ch10_sales (sales_month, country_name, prod_category, channel_desc, sales_amt)	   
	        SELECT A.SALES_MONTH,
               C.COUNTRY_NAME,
               D.PROD_CATEGORY,
               E.CHANNEL_DESC,
               SUM(A.AMOUNT_SOLD)
          FROM SALES A, CUSTOMERS B, COUNTRIES C, PRODUCTS D, CHANNELS E
         WHERE A.SALES_MONTH = p_sales_month
           AND A.CUST_ID = B.CUST_ID
           AND B.COUNTRY_ID = C.COUNTRY_ID
           AND A.PROD_ID = D.PROD_ID
           AND A.CHANNEL_ID = E.CHANNEL_ID
         GROUP BY A.SALES_MONTH,
                 C.COUNTRY_NAME,
               D.PROD_CATEGORY,
               E.CHANNEL_DESC;
               
         COMMIT;
         --ROLLBACK; 하면 되돌아감
	        
        END;         




        TRUNCATE TABLE ch10_sales;

        EXEC iud_ch10_sales_proc ( '199901');

        SELECT COUNT(*)
        FROM ch10_sales ;


        TRUNCATE TABLE ch10_sales;

        ALTER TABLE ch10_sales ADD CONSTRAINTS pk_ch10_sales PRIMARY KEY (sales_month, country_name, prod_category, channel_desc);
        -- 제약조건 설정 후 테스트


        CREATE OR REPLACE PROCEDURE iud_ch10_sales_proc
                    ( p_sales_month ch10_sales.sales_month%TYPE )
        IS

        BEGIN
	        
	        INSERT INTO ch10_sales (sales_month, country_name, prod_category, channel_desc, sales_amt)	   
	        SELECT A.SALES_MONTH,
               C.COUNTRY_NAME,
               D.PROD_CATEGORY,
               E.CHANNEL_DESC,
               SUM(A.AMOUNT_SOLD)
          FROM SALES A, CUSTOMERS B, COUNTRIES C, PRODUCTS D, CHANNELS E
         WHERE A.SALES_MONTH = p_sales_month
           AND A.CUST_ID = B.CUST_ID
           AND B.COUNTRY_ID = C.COUNTRY_ID
           AND A.PROD_ID = D.PROD_ID
           AND A.CHANNEL_ID = E.CHANNEL_ID
         GROUP BY A.SALES_MONTH,
                 C.COUNTRY_NAME,
               D.PROD_CATEGORY,
               E.CHANNEL_DESC;

         -- 다 처리되고 오류가 없을시 커밋
         COMMIT;

        EXCEPTION WHEN OTHERS THEN
                       DBMS_OUTPUT.PUT_LINE(SQLERRM);
                       ROLLBACK;
	        
        END;   
```
2. SAVEPOINT
- 보통 ROLLBACK을 명시하면 INSERT, DELETE, UPDATE, MERGE 작업 전체가 취소되는데 전체가 아닌 특정 부분에서 트랜잭션을 취소시킬 수 있다.
- 이렇게 하려면 취소하려는 지점을 명시한 뒤, 그 지점까지 작업을 취소하는 식으로 사용하는데 이 지점을 SAVEPOINT라고 한다.
```sql
-------------------------------------------------------------------------------------
        CREATE TABLE EX8_1 (

              EX_NO NUMBER
             ,EX_NM VARCHAR2(1000)

        );

        CREATE OR REPLACE PROCEDURE ex_8_1_proc (flag varchar2)
        IS
            point1 EXCEPTION;
            point2 EXCEPTION;
            vn_num number;
        BEGIN
            INSERT INTO EX8_1 VALUES (1, 'POINT BEFORE');
            INSERT INTO EX8_1 VALUES (2, 'POINT1');
            SAVEPOINT mysavepoint1; --롤백 지점1

            INSERT INTO EX8_1 VALUES (3, 'POINT1');
            INSERT INTO EX8_1 VALUES (4, 'POINT2');
            SAVEPOINT mysavepoint2; --롤백 지점2
            
            INSERT INTO EX8_1 VALUES (5, 'POINT2');
            
            INSERT INTO EX8_1 VALUES (6, 'ELSE');
            INSERT INTO EX8_1 VALUES (7, 'ELSE');
          	INSERT INTO EX8_1 VALUES (8, 'ELSE');
            INSERT INTO EX8_1 VALUES (9, 'ELSE');

            IF flag = '1' then
	              RAISE point1;
	        ELSIF  flag = '2' then
                 RAISE point2;    
            ELSE
                vn_num := 10 / 0;  --1도 아니고 2도 아니면  다 롤백
            END IF;

        EXCEPTION WHEN point1 THEN
                    DBMS_OUTPUT.PUT_LINE('POINT1');
                    ROLLBACK TO mysavepoint1;   --세이브포인트1 지점까지만 커밋
                    COMMIT;
                  WHEN point2 THEN
                    DBMS_OUTPUT.PUT_LINE('POINT2');
            	    ROLLBACK TO mysavepoint2;   --세이브포인트2 지점까지만 커밋
                    COMMIT;
                  WHEN OTHERS THEN
                    DBMS_OUTPUT.PUT_LINE('ORDERS');
              	    ROLLBACK;                   --롤백
        END;


        EXEC ex_8_1_proc(3);

        SELECT *
        FROM EX8_1;

        TRUNCATE TABLE EX8_1;



        EXEC iud_ch10_sales_proc ( '199901');

        SELECT COUNT(*)
        FROM ch10_sales ;


        CREATE TABLE ch10_country_month_sales (
                       sales_month   VARCHAR2(8),
                       country_name  VARCHAR2(40),
                       sales_amt     NUMBER,
                       PRIMARY KEY (sales_month, country_name) );
                      
        CREATE OR REPLACE PROCEDURE iud_ch10_sales_proc
                    ( p_sales_month  ch10_sales.sales_month%TYPE,
                      p_country_name ch10_sales.country_name%TYPE )
        IS

        BEGIN
	        
	        --기존 데이터 삭제
	        DELETE ch10_sales
	         WHERE sales_month  = p_sales_month
	           AND country_name = p_country_name;
	              
	        -- 신규로 월, 국가를 매개변수로 받아 INSERT
	        -- DELETE를 수행하므로 PRIMARY KEY 중복이 발생치 않음
	        INSERT INTO ch10_sales (sales_month, country_name, prod_category, channel_desc, sales_amt)	   
	        SELECT A.SALES_MONTH,
               C.COUNTRY_NAME,
               D.PROD_CATEGORY,
               E.CHANNEL_DESC,
               SUM(A.AMOUNT_SOLD)
          FROM SALES A, CUSTOMERS B, COUNTRIES C, PRODUCTS D, CHANNELS E
         WHERE A.SALES_MONTH  = p_sales_month
           AND C.COUNTRY_NAME = p_country_name
           AND A.CUST_ID = B.CUST_ID
           AND B.COUNTRY_ID = C.COUNTRY_ID
           AND A.PROD_ID = D.PROD_ID
           AND A.CHANNEL_ID = E.CHANNEL_ID
         GROUP BY A.SALES_MONTH,
                 C.COUNTRY_NAME,
               D.PROD_CATEGORY,
               E.CHANNEL_DESC;
               
         -- SAVEPOINT 확인을 위한 UPDATE

          -- 현재시간에서 초를 가져와 숫자로 변환한 후 * 10 (매번 초는 달라지므로 성공적으로 실행 시 이 값은 매번 달라짐)
         UPDATE ch10_sales
            SET sales_amt = 10 * to_number(to_char(sysdate, 'ss'))
          WHERE sales_month  = p_sales_month
	           AND country_name = p_country_name;
	           
         -- SAVEPOINT 지정      

         SAVEPOINT mysavepoint;      
         
         
         -- ch10_country_month_sales 테이블에 INSERT
         -- 중복 입력 시 PRIMARY KEY 중복됨
         INSERT INTO ch10_country_month_sales
               SELECT sales_month, country_name, SUM(sales_amt)
                 FROM ch10_sales
                WHERE sales_month  = p_sales_month
	                AND country_name = p_country_name
	              GROUP BY sales_month, country_name;         
               
         COMMIT;

        EXCEPTION WHEN OTHERS THEN
                       DBMS_OUTPUT.PUT_LINE(SQLERRM);
                       ROLLBACK TO mysavepoint; -- SAVEPOINT 까지만 ROLLBACK
                       COMMIT; -- SAVEPOINT 이전까지는 COMMIT

	        
        END;   

        TRUNCATE TABLE ch10_sales;

        EXEC iud_ch10_sales_proc ( '199901', 'Italy');

        SELECT DISTINCT sales_amt
        FROM ch10_sales;



        EXEC iud_ch10_sales_proc ( '199901', 'Italy');

        SELECT DISTINCT sales_amt
        FROM ch10_sales;
```
```sql
----------------------------------------------------------
- 문제
----------------------------------------------------------
/*
 1번 문제
 익명블럭 or 프로시져 상관없음

 (1) 어떤 학번의 학생이 몇학기에 몇학점을 들었고
 (2) 앞으로 몇학점, 몇학기가 필요한지 학생 테이블의 모든 학생에 대해 출력해 주세요.
 (3) 1학기에 3과목 수강가능(9학점을 취득할 수 있다), 졸업 학점은 36학점

*/


SELECT 학생.학번
     , 학생.이름
     , nvl(학생.학기 , 0)
     , nvl(sum(과목.학점),0)
FROM 학생 , 수강내역 , 과목
WHERE 수강내역.학번(+) = 학생.학번
and 수강내역.과목번호 = 과목.과목번호(+)


GROUP BY  학생.학번
        , 학생.이름
        , 학생.학기
 ;



     CREATE OR REPLACE PROCEDURE jolup_proc
                    ( p_학번  IN OUT 학생.학번%TYPE,
                      p_이름  IN OUT 학생.이름%TYPE,
                      p_학기  IN OUT 학생.학기%TYPE,
                      p_학점  IN OUT 과목.학점%TYPE)
        IS
        vn_학점          학생.학번%TYPE;
        vn_cnt           NUMBER ;
        BEGIN
          select count(*)
          into vn_cnt
          from 학생 ;
          
            FOR i in  1..vn_cnt
            LOOP
            SELECT t2.학번, t2.이름, t2.학학, t2.과학
                        INTO p_학번, p_이름, p_학기, p_학점
            FROM(
              SELECT ROWNUM RNUM ,  t1.학번, t1.이름, t1.학학, t1.과학
                FROM
                    (SELECT 학생.학번
                         , 학생.이름
                         , nvl(학생.학기 , 0) as 학학
                         , nvl(sum(과목.학점),0)as 과학
                 
                    FROM 학생 , 수강내역 , 과목
                    WHERE 수강내역.학번(+) = 학생.학번
                    and 수강내역.과목번호 = 과목.과목번호(+)
                    
                    GROUP BY  학생.학번
                            , 학생.이름
                            , 학생.학기 )T1) T2
                WHERE T2.RNUM = i ;
                DBMS_OUTPUT.PUT_LINE('[기본자료]학번 : ' || p_학번 || ' 이름 : ' || p_이름 || ' 수강학기 : '|| p_학기 || ' 취득학번 : ' || p_학점);
             END LOOP;           


        END;           
        
        
        DECLARE
        p_학번    VARCHAR2(100);
        p_이름    VARCHAR2(100);
        p_학기    NUMBER;
        p_학점    NUMBER;
        
        BEGIN
        jolup_proc(p_학번, p_이름 ,p_학기, p_학점);
        END ;
        
        
EXEC jolup_proc(2001110141);


select count(*)
        
          from 학생 ;
수강내역.학번 = 학생.학번
수강내역.과목번호 = 과목.과목번호
과목.학점

SELECT *
            FROM(
              SELECT ROWNUM RNUM ,  T1.*
                FROM
                    (SELECT 학생.학번
                         , 학생.이름
                         , nvl(학생.학기 , 0)
                         , nvl(sum(과목.학점),0)
                
                    FROM 학생 , 수강내역 , 과목
                    WHERE 수강내역.학번(+) = 학생.학번
                    and 수강내역.과목번호 = 과목.과목번호(+)
                    AND ROWNUM = 1  
                    
                    GROUP BY  학생.학번
                            , 학생.이름
                            , 학생.학기 )T1) T2
                WHERE T2.RNUM = 1 ;
 ----------------------------------------------------------
[기존 자료]학번:2001110141 이름:윤지미 수강 학기:3 취득 학점:0
[산출 자료]학번:2001110141 이름:윤지미 최소 학기:4 필요 학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:2021000001 이름:팽수 수강 학기:0 취득 학점:0
[산출 자료]학번:2021000001 이름:팽수 최소 학기:4 필요 학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:2001211121 이름:박지훈 수강 학기:4 취득 학점:8
[산출 자료]학번:2001211121 이름:박지훈 최소 학기:4 필요 학점:28
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:2001110131 이름:김기성 수강 학기:6 취득 학점:8
[산출 자료]학번:2001110131 이름:김기성 최소 학기:4 필요 학점:28
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:1997131542 이름:최숙경 수강 학기:3 취득 학점:14
[산출 자료]학번:1997131542 이름:최숙경 최소 학기:3 필요 학점:22
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:2021000003 이름:팽수3 수강 학기:0 취득 학점:0
[산출 자료]학번:2021000003 이름:팽수3 최소 학기:4 필요 학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:2002110112 이름:양지운 수강 학기:5 취득 학점:0
[산출 자료]학번:2002110112 이름:양지운 최소 학기:4 필요 학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:2001110141 이름:윤지미 수강 학기:3 취득 학점:0
[산출 자료]학번:2001110141 이름:윤지미 최소 학기:4 필요 학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:1998131205 이름:정지미 수강 학기:6 취득 학점:5
[산출 자료]학번:1998131205 이름:정지미 최소 학기:4 필요 학점:31
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:2021000002 이름:팽수2 수강 학기:0 취득 학점:0
[산출 자료]학번:2021000002 이름:팽수2 최소 학기:4 필요 학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:1999232102 이름:이완재 수강 학기:6 취득 학점:8
[산출 자료]학번:1999232102 이름:이완재 최소 학기:4 필요 학점:28
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:2002110110 이름:홍길동 수강 학기:5 취득 학점:3
[산출 자료]학번:2002110110 이름:홍길동 최소 학기:4 필요 학점:33
----------------------------------------------------------
----------------------------------------------------------
[기존 자료]학번:2002110111 이름:김성동 수강 학기:4 취득 학점:0
[산출 자료]학번:2002110111 이름:김성동 최소 학기:4 필요 학점:36
----------------------------------------------------------


----------------------------------------------------------
--  2번 문제 위 익명블럭을 작성 후 과목 정보도 나올 수 있도록 출력하시오
----------------------------------------------------------
[기존자료]
 학번:2021000001 이름:팽수 수강학기:0 취득학점:0
[수강 내용]
 자료가 없습니다.
[산출자료]
 최소학기:4 필요학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:2001211121 이름:박지훈 수강학기:4 취득학점:8
[수강 내용]
 과목:현대경영학 학점:2
 과목:회계학원론 학점:3
 과목:경영정보시스템 학점:3
[산출자료]
 최소학기:4 필요학점:28
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:2001110131 이름:김기성 수강학기:6 취득학점:8
[수강 내용]
 과목:현대경영학 학점:2
 과목:경영정보시스템 학점:3
 과목:회계학원론 학점:3
[산출자료]
 최소학기:4 필요학점:28
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:1997131542 이름:최숙경 수강학기:3 취득학점:14
[수강 내용]
 과목:현대경영학 학점:2
 과목:경영정보시스템 학점:3
 과목:회계학원론 학점:3
 과목:마케팅개론 학점:3
 과목:인사관리 학점:3
[산출자료]
 최소학기:3 필요학점:22
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:2021000003 이름:팽수3 수강학기:0 취득학점:0
[수강 내용]
 자료가 없습니다.
[산출자료]
 최소학기:4 필요학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:2002110112 이름:양지운 수강학기:5 취득학점:0
[수강 내용]
 자료가 없습니다.
[산출자료]
 최소학기:4 필요학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:2001110141 이름:윤지미 수강학기:3 취득학점:0
[수강 내용]
 자료가 없습니다.
[산출자료]
 최소학기:4 필요학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:1998131205 이름:정지미 수강학기:6 취득학점:5
[수강 내용]
 과목:현대경영학 학점:2
 과목:재무관리 학점:3
[산출자료]
 최소학기:4 필요학점:31
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:2021000002 이름:팽수2 수강학기:0 취득학점:0
[수강 내용]
 자료가 없습니다.
[산출자료]
 최소학기:4 필요학점:36
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:1999232102 이름:이완재 수강학기:6 취득학점:8
[수강 내용]
 과목:현대경영학 학점:2
 과목:회계학원론 학점:3
 과목:마케팅개론 학점:3
[산출자료]
 최소학기:4 필요학점:28
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:2002110110 이름:홍길동 수강학기:5 취득학점:3
[수강 내용]
 과목:마케팅개론 학점:3
[산출자료]
 최소학기:4 필요학점:33
----------------------------------------------------------
----------------------------------------------------------
[기존자료]
 학번:2002110111 이름:김성동 수강학기:4 취득학점:0
[수강 내용]
 자료가 없습니다.
[산출자료]
 최소학기:4 필요학점:36
----------------------------------------------------------

```



## 7. 커서, 레코드, 컬렉션
### 1. 커서
- 커서 : 특정 sql 문장을 처리한 결과를 담고 있는 영역을 가리키는 일종의 포인터로, 커서를 사용하면 처리된 sql 문장의 결과 집합에 접근할 수 있다. 잠깐 참조하는 느낌
- 커서의 종류
  - 묵시적 커서 : 오라클 내부에서 자동으로 생성되어 사용
  - 명시적 커서 : 사용자가 직접 정의해서 사용하는 커서


1. 묵시적 커서 
```sql
-------------------------------------------------------------------------------------
SET SERVEROUTPUT ON
SET TIMING ON
-- 묵시적 커서와 커서속성
/**
    커서란 특정 SQL문장을 처리한 결과를 담고 있는 영역(PRIVATE SQL 이라는 메모리 영역)을 가리키는 일종의 포인터로,
    커서를 사용하면 처리된 SQL 문장의 결과 집합에 접근할 수 있다.
    
    개별 로우에 순차적으로 접근이 가능하다.
    
    종류
    1. 묵시적 커서 (오라클 내부에서 자동생성) PL/SQL 블로에서 실행하는 SQL 문장
      (INSERT, UPDATE, SELECT .. ) 이 실행될 때마다 자동으로 생성.
    2. 명시적 커서 (사용자가 직접 정의)
    순서 open -> fetch -> close
    명시적은 선언도 필요
*/

--1
DECLARE
  vn_department_id employees.department_id%TYPE := 80;
BEGIN
	-- 80번 부서의 사원이름을 자신의 이름으로 갱신
	 UPDATE employees
	     SET emp_name = emp_name
	   WHERE department_id = vn_department_id;	   
	   
	-- 몇 건의 데이터가 갱신됐는지 출력  없으면 0을 반환
	DBMS_OUTPUT.PUT_LINE('묵시적 커서 :'||SQL%ROWCOUNT); --묵시적 커서 :34
	COMMIT;
END;

```

2. 명시적 커서 
```sql
-- 명시적 커서
--2
DECLARE
   -- 사원명을 받아오기 위한 변수 선언
   vs_emp_name employees.emp_name%TYPE;

   --1.단계 커서 선언 : 명칭과 사용 쿼리 선언 매개변수로 부서코드를 받는다.
   -- CURSOR 커서명 [(매개변수1, 매개변수2, ..)]

   CURSOR cur_emp_dep ( cp_department_id employees.department_id%TYPE )

   IS

   SELECT emp_name
     FROM employees
    WHERE department_id = cp_department_id;

BEGIN
	--2.단계 커서 오픈 (매개변수로 90번 부서를 전달)
	OPEN cur_emp_dep (90);
	--3.단계 패치 단계에서 커서 사용
	LOOP
	
    	  -- 반복문을 통한 커서 패치작업
	  -- 커서 결과로 나온 로우를 패치함 (사원명을 변수에 할당)
	  FETCH cur_emp_dep INTO vs_emp_name;
	  -- 더 이상 패치된 참조로우가 없는 경우 LOOP 탈출
	  EXIT WHEN cur_emp_dep%NOTFOUND;
	  -- 사원명을 출력
	  DBMS_OUTPUT.PUT_LINE(vs_emp_name);

  END LOOP;
  --4.단계 커서 닫기(반드시 닫아야함)
  CLOSE cur_emp_dep;

END;
```
3. 커서와 FOR문
```sql
	
-- 커서와 FOR문
--3
DECLARE
   -- 커서 선언, 매개변수로 부서코드를 받는다.
   CURSOR cur_emp_dep ( cp_department_id employees.department_id%TYPE )

   IS               --is begin 사이에 커서를 사용할 커리를 작성
   SELECT emp_name
     FROM employees
    WHERE department_id = cp_department_id;
    
BEGIN
	-- FOR문을 통한 커서 패치작업 ("초깃값.. 최종값" 대신 커서가 위치한다)
	-- FOR 레코드 IN 커서명(매개변수1, 매개변수2,...)

	FOR emp_rec IN cur_emp_dep(90)
	LOOP
	  -- 사원명을 출력, 레코드 타입은 레코드명.컬럼명 형태로 사용
	  DBMS_OUTPUT.PUT_LINE(emp_rec.emp_name);
  END LOOP;
END;

--FOR문에 직접 커서 정의 넣을 수 있다.
--4
DECLARE
BEGIN
	-- FOR문을 통한 커서 패치작업 ( 커서 선언시 정의 부분을 FOR문에 직접 기술)
	FOR emp_rec IN ( SELECT emp_name
                         FROM employees
                         WHERE department_id = 90	
	               )
	LOOP
	  -- 사원명을 출력, 레코드 타입은 레코드명.컬럼명 형태로 사용
	  DBMS_OUTPUT.PUT_LINE(emp_rec.emp_name);
	END LOOP;
END;

/**
 명시적 커서는 "CURSOR 커서명 IS SELECT ..." 형태로 선언한 뒤, '커서명'을 참조해서 사용했다.
 즉 명시적 커서를 사용할 때는 커서명을 마치 변수처럼 사용했는데, 정확히 말하면
 변수라기보다는 '상수'라고 할 수 있다.
 커서를 변수처럼 할당한뒤 다시 다른 값을 할당해서 사용하려면 커서변수를 사용해야한다.
**/
--5
DECLARE
   -- 사원명을 받아오기 위한 변수 선언
   vs_emp_name employees.emp_name%TYPE;

   TYPE emp_dep_curtype IS REF CURSOR;                               -- 약한 커서타입 선언
-- TYPE emp_dep_curtype IS REF CURSOR RETURN departments%ROWTYPE;    -- 강한 커서타입 선언 (집합을 고정해서)
   -- 커서변수 선언
   emp_dep_curvar emp_dep_curtype; -- 커서변수 선언
BEGIN
  -- 커서변수를 사용한 커서정의 및 오픈
  OPEN emp_dep_curvar FOR SELECT emp_name
                          FROM employees
                          WHERE department_id = 90	;
  -- LOOP문
  LOOP
     -- 커서변수를 사용해 결과집합을  vs_emp_name 변수에 할당
     FETCH emp_dep_curvar INTO vs_emp_name;
	  -- 더 이상 패치된 참조로우가 없는 경우 LOOP 탈출(커서변수를 이용한 커서속성 참조)
	  EXIT WHEN emp_dep_curvar%NOTFOUND;
	  -- 사원명을 출력
	  DBMS_OUTPUT.PUT_LINE(vs_emp_name);
  END LOOP;
  CLOSE emp_dep_curvar;
END;

--6------------------------------------------------------------------------------------
DECLARE
   -- 사원명을 받아오기 위한 변수 선언
   vs_emp_name employees.emp_name%TYPE;
   -- SYS_REFCURSOR 타입의 커서변수 선언
   -- oracle 에서 제공하는 커서타입
   emp_dep_curvar SYS_REFCURSOR;
BEGIN
  -- 커서변수를 사용한 커서정의 및 오픈
  OPEN emp_dep_curvar FOR SELECT emp_name
                     FROM employees
                    WHERE department_id = 90;
  -- LOOP문
  LOOP
     -- 커서변수를 사용해 결과집합을  vs_emp_name 변수에 할당
     FETCH emp_dep_curvar INTO vs_emp_name;
	  -- 더 이상 패치된 참조로우가 없는 경우 LOOP 탈출(커서변수를 이용한 커서속성 참조)
	  EXIT WHEN emp_dep_curvar%NOTFOUND;
	  -- 사원명을 출력
	  DBMS_OUTPUT.PUT_LINE(vs_emp_name);
  END LOOP;
  CLOSE emp_dep_curvar;
END;

-- 커서변수를 매개변수로 전달
--7----------------------------------------------------------------------------------------------
DECLARE
    -- (ⅰ) SYS_REFCURSOR 타입의 커서변수 선언
   emp_dep_curvar SYS_REFCURSOR;
    -- 사원명을 받아오기 위한 변수 선언
   vs_emp_name employees.emp_name%TYPE;

   -- (ⅱ) 커서변수를 매개변수르 받는 프로시저, 매개변수는 SYS_REFCURSOR 타입의 IN OUT형
   PROCEDURE test_cursor_argu ( p_curvar IN OUT SYS_REFCURSOR)
   IS
       c_temp_curvar SYS_REFCURSOR;
   BEGIN
       -- 커서를 오픈한다
       OPEN c_temp_curvar FOR
             SELECT emp_name
               FROM employees
             WHERE department_id = 90;
        -- (ⅲ) 오픈한 커서를 IN OUT 매개변수에 다시 할당한다.
        p_curvar := c_temp_curvar;
   END;

BEGIN
   -- 프로시저를 호출한다.
   test_cursor_argu (emp_dep_curvar);
   -- (ⅳ) 전달해서 받은 매개변수를 LOOP문을 사용해 결과를 출력한다.
   LOOP
     -- 커서변수를 사용해 결과집합을  vs_emp_name 변수에 할당
     FETCH emp_dep_curvar INTO vs_emp_name;
	  -- 더 이상 패치된 참조로우가 없는 경우 LOOP 탈출(커서변수를 이용한 커서속성 참조)
	  EXIT WHEN emp_dep_curvar%NOTFOUND;
	  -- 사원명을 출력
	  DBMS_OUTPUT.PUT_LINE(vs_emp_name);
  END LOOP;
END;

--8
------------------------------------------------------------------------------------------------------
CREATE  PROCEDURE test_cursor_argu ( p_curvar IN OUT SYS_REFCURSOR)
   IS
       c_temp_curvar SYS_REFCURSOR;
   BEGIN
       -- 커서를 오픈한다
       OPEN c_temp_curvar FOR
             SELECT emp_name
               FROM employees
             WHERE department_id = 90;
        -- (ⅲ) 오픈한 커서를 IN OUT 매개변수에 다시 할당한다.
        p_curvar := c_temp_curvar;
   END;

-- 위에 프로시져를 아래와 같이 사용가능 -------------------------------------------------
DECLARE
    -- (ⅰ) SYS_REFCURSOR 타입의 커서변수 선언
   emp_dep_curvar SYS_REFCURSOR;
    -- 사원명을 받아오기 위한 변수 선언
   vs_emp_name employees.emp_name%TYPE;
   -- (ⅱ) 커서변수를 매개변수르 받는 프로시저, 매개변수는 SYS_REFCURSOR 타입의 IN OUT형
BEGIN
   -- 프로시저를 호출한다.
   test_cursor_argu (emp_dep_curvar);
   -- (ⅳ) 전달해서 받은 매개변수를 LOOP문을 사용해 결과를 출력한다.
   LOOP
     -- 커서변수를 사용해 결과집합을  vs_emp_name 변수에 할당
     FETCH emp_dep_curvar INTO vs_emp_name;
	  -- 더 이상 패치된 참조로우가 없는 경우 LOOP 탈출(커서변수를 이용한 커서속성 참조)
	  EXIT WHEN emp_dep_curvar%NOTFOUND;
	  -- 사원명을 출력
	  DBMS_OUTPUT.PUT_LINE(vs_emp_name);
  END LOOP;
END;
```
3. 커서 표현식
```sql

--9 커서 표현식 ----------------------------------------------------------------------------------------------

DECLARE
    -- 커서표현식을 사용한 명시적 커서 선언
    CURSOR mytest_cursor IS
         SELECT d.department_name,      
                  CURSOR ( SELECT e.emp_name
                                 FROM employees e
                                WHERE e.department_id = d.department_id) AS emp_name        
          FROM departments d
        WHERE d.department_id = 90;
    -- 부서명을 받아오기 위한 변수
    vs_department_name departments.department_name%TYPE;
    --커서표현식 결과를 받기 위한 커서타입변수
    c_emp_name SYS_REFCURSOR;
    -- 사원명을 받는 변수
    vs_emp_name employees.emp_name%TYPE;
BEGIN
    -- 커서오픈
    OPEN mytest_cursor;
    -- 명시적 커서를 받아오는 첫 번째 LOOP
    LOOP
       -- 부서명은 변수, 사원명 결과집합은 커서변수에 패치
       FETCH mytest_cursor INTO vs_department_name, c_emp_name;
       EXIT WHEN mytest_cursor%NOTFOUND;
       DBMS_OUTPUT.PUT_LINE ('부서명 : ' || vs_department_name);
       -- 사원명을 출력하기 위한 두 번째 LOOP
       LOOP
          -- 사원명 패치
          FETCH c_emp_name INTO vs_emp_name;
          EXIT WHEN c_emp_name%NOTFOUND;
          DBMS_OUTPUT.PUT_LINE('   사원명 : ' || vs_emp_name);
       END LOOP; -- 두 번째 LOOP 종료    
    END LOOP; -- 첫 번째 LOOP 종료
    CLOSE mytest_cursor;
END;

--10------------------------------------------------------------------------------------------------------------------------------------------------------------------- 커서 끝
--커서를 활용해서 어제 문제를 수정 하시오(해결X)

/*

 (1) 어떤 학번의 학생이 몇학기에 몇학점을 들었고
 (2) 앞으로 몇학점, 몇학기가 필요한지 학생 테이블의 모든 학생에 대해 출력해 주세요.
 (3) 1학기에 3과목 (9학점을 취득할 수 있다.), 졸업 학점은 36학점
 (4)고정 값은 상수로 지정해 주세요.

*/

 ----------------------------------------------------------
[기존 자료]학번:2001110141 이름:윤지미 수강 학기:3 취득 학점:0
[산출 자료]학번:2001110141 이름:윤지미 최소 학기:4 필요 학점:36
----------------------------------------------------------

---학기,학점 쿼-------------------------------------------------------------------------------------------------------
 
SELECT B.hak_no
     , B.name
     , B.max_hak_gi
     , B.sum_hak_jum
FROM (
        SELECT A.*
              , rownum  as rnum
        FROM (
                SELECT 학생.학번            AS hak_no
                      ,학생.이름            AS name
                      ,NVL(MAX(학생.학기),0)       AS max_hak_gi
                      ,NVL(SUM(과목.학점),0)       AS sum_hak_jum
                FROM 학생, 수강내역, 과목
                WHERE 학생.학번 = 수강내역.학번 (+)
                AND   수강내역.과목번호 = 과목.과목번호(+)
                GROUP BY 학생.학번
                       , 학생.이름
                ORDER BY  NVL(SUM(과목.학점),0)  DESC      
             ) A
    )B
WHERE B.rnum = 1;
---과목쿼리 -----------------------------------------------------------------------------------------------------

SELECT A.subject
    ,  A.hak_jum
FROM (
        SELECT  과목.과목이름 AS subject
                , 과목.학점    AS  hak_jum
              ,rownum      as sub_rnum
        FROM 수강내역, 과목
        WHERE 수강내역.과목번호 = 과목.과목번호(+)
        AND 수강내역.학번 = 1997131542
     ) A
WHERE A.sub_rnum = 1;
------------------------------------------------------------------------------------------------
--익명블럭으로----------------------------------------------------------------------------------------------
  
  
DECLARE
   --상수------------------------------
   cn_max_num   CONSTANT NUMBER := 9;        /*1년 수강하가능 학점*/
   cn_grade_num CONSTANT NUMBER := 36;       /*졸업가능 학점*/
   --반복건수-------------------------
   vn_sum_cnt            NUMBER;             /*전체 학생 수*/
   --기존자료---------------------------
   vn_hak_no             NUMBER := 0;        /*학점*/
   vn_name               VARCHAR2 (20);      /*이름*/
   vn_before_hak_gi      NUMBER := 0;        /*이수학기*/
   vn_before_hak_jum     NUMBER := 0;        /*이수학점*/
   --필요자료---------------------------
   vn_after_hak_gi       NUMBER := 0;        /*필요학점*/
   vn_after_hak_jum      NUMBER := 0;        /*필수학기*/

BEGIN
   SELECT COUNT(DISTINCT 학번) AS CNT
   INTO vn_sum_cnt
   FROM 학생;
   
   FOR i in 1..vn_sum_cnt -- vn_cnt가 9보다 작거나 같을 경우에만 반복처리
   LOOP
        SELECT B.hak_no
             , B.name
             , NVL(B.max_hak_gi,0) as max_hak_gi
             , NVL(B.sum_hak_jum,0) as sum_hak_jum
        INTO vn_hak_no, vn_name, vn_before_hak_gi, vn_before_hak_jum    
        FROM (
                SELECT A.*
                      , rownum  as rnum
                FROM (
                        SELECT 학생.학번            AS hak_no
                              ,학생.이름            AS name
                              ,MAX(학생.학기)       AS max_hak_gi
                              ,SUM(과목.학점)       AS sum_hak_jum
                        FROM 학생, 수강내역, 과목
                        WHERE 학생.학번 = 수강내역.학번 (+)
                        AND   수강내역.과목번호 = 과목.과목번호(+)
                        GROUP BY 학생.학번
                               , 학생.이름
                     ) A
            )B
        WHERE B.rnum = i;
        
        vn_after_hak_jum := cn_grade_num - vn_before_hak_jum;
        vn_after_hak_gi  := CEIL(vn_after_hak_jum / cn_max_num);

        DBMS_OUTPUT.PUT_LINE ('----------------------------------------------------------') ;
        DBMS_OUTPUT.PUT_LINE ('[기존 자료]학번:'|| vn_hak_no || ' 이름:' || vn_name || ' 수강 학기:' || vn_before_hak_gi || ' 취득 학점:'||vn_before_hak_jum );
        DBMS_OUTPUT.PUT_LINE ('[산출 자료]학번:'|| vn_hak_no || ' 이름:' || vn_name || ' 최소 학기:' || vn_after_hak_gi || ' 필요 학점:'||vn_after_hak_jum );
        DBMS_OUTPUT.PUT_LINE ('----------------------------------------------------------') ;
   END LOOP;    
END;
  
---(2) 과목 추가 익명블---------------------------------------------------------------------------------------------

DECLARE
   --상수------------------------------
   cn_max_num   CONSTANT NUMBER := 9;        /*1년 수강하가능 학점*/
   cn_grade_num CONSTANT NUMBER := 36;       /*졸업가능 학점*/
   --반복건수---------------------------
   vn_sum_cnt            NUMBER;             /*전체 학생 수*/
   --기존자료---------------------------
   vn_hak_no             NUMBER := 0;        /*학점*/
   vn_name               VARCHAR2 (20);      /*이름*/
   vn_before_hak_gi      NUMBER := 0;        /*이수학기*/
   vn_before_hak_jum     NUMBER := 0;        /*이수학점*/
   --------------------------------
   vn_after_hak_gi       NUMBER := 0;        /*필요학점*/
   vn_after_hak_jum      NUMBER := 0;        /*필수학기*/
   --------------------------------
   vn_sub_cnt            NUMBER := 0;        /*과목 건수*/
   vn_subject            VARCHAR2(100);      /*과목 이름*/
   vn_subject_hak_jum    NUMBER := 0;        /*과목 점수*/
   
BEGIN
   /* 전체 대상 학생 건수*/
   SELECT COUNT(DISTINCT 학번) AS CNT
   INTO vn_sum_cnt
   FROM 학생;
   /* 전체 대상 학생 건수만큼 반복*/
   FOR i in 1..vn_sum_cnt -- vn_cnt가 9보다 작거나 같을 경우에만 반복처리
   LOOP
        /* 기존 학생정보 검색*/
        SELECT B.hak_no
             , B.name
             , NVL(B.max_hak_gi,0)  as max_hak_gi
             , NVL(B.sum_hak_jum,0) as sum_hak_jum
        INTO vn_hak_no, vn_name, vn_before_hak_gi, vn_before_hak_jum    
        FROM (
                SELECT A.*
                      , rownum  as rnum
                FROM (
                        SELECT 학생.학번            AS hak_no
                              ,학생.이름            AS name
                              ,MAX(학생.학기)       AS max_hak_gi
                              ,SUM(과목.학점)       AS sum_hak_jum
                        FROM 학생, 수강내역, 과목
                        WHERE 학생.학번 = 수강내역.학번 (+)
                        AND   수강내역.과목번호 = 과목.과목번호(+)
                        GROUP BY 학생.학번
                               , 학생.이름
                     ) A
            )B
        WHERE B.rnum = i;
                    /* 필요학점*/
        vn_after_hak_jum := cn_grade_num - vn_before_hak_jum;
                     /* 최소학기 ( 남은 학점/1년 이수가능학점(9) )올림함수 사용 */
        vn_after_hak_gi  := CEIL(vn_after_hak_jum / cn_max_num);

        DBMS_OUTPUT.PUT_LINE ('----------------------------------------------------------') ;
        DBMS_OUTPUT.PUT_LINE ('[기존자료]');
        DBMS_OUTPUT.PUT_LINE (' 학번:'|| vn_hak_no || ' 이름:' || vn_name ||' 수강학기:' || vn_before_hak_gi || ' 취득학점:'||vn_before_hak_jum);
                    /* 이수과목 건수 */

	
	SELECT COUNT(*) AS sub_cnt
	INTO vn_sub_cnt
        FROM 수강내역, 과목
        WHERE 수강내역.과목번호 = 과목.과목번호(+)
        AND 수강내역.학번 = vn_hak_no;
       
        DBMS_OUTPUT.PUT_LINE ('[수강 내용]');
                    /* 이수과목 정보가 있을 경우 */
        IF vn_sub_cnt != 0 THEN
                              /* 이수과목 정보건수 만큼 */
            FOR i in 1..vn_sub_cnt LOOP
                SELECT A.subject
                    ,  A.hak_jum
                INTO vn_subject, vn_subject_hak_jum
                FROM (
                        SELECT 과목.과목이름 AS subject
                                 ,과목.학점    AS hak_jum
                              ,rownum      AS sub_rnum
                        FROM 수강내역, 과목
                        WHERE 수강내역.과목번호 = 과목.과목번호(+)
                        AND 수강내역.학번 = vn_hak_no
                     ) A
                WHERE A.sub_rnum = i;
                DBMS_OUTPUT.PUT_LINE (' 과목:' || vn_subject || ' 학점:'||vn_subject_hak_jum );
            END LOOP;
                    /* 이수과목 정보가 없을 경우 */    
        ELSE
            DBMS_OUTPUT.PUT_LINE(' 자료가 없습니다.');	
        END IF;
        DBMS_OUTPUT.PUT_LINE ('[산출자료]');
        DBMS_OUTPUT.PUT_LINE (' 최소학기:' || vn_after_hak_gi || ' 필요학점:'||vn_after_hak_jum );
        DBMS_OUTPUT.PUT_LINE ('----------------------------------------------------------') ;


      vn_cnt := vn_cnt + 1; -- vn_cnt 값을 1씩 증가
   END LOOP;    
END;

----과목프로시--------------------------------------------------------------------------------------------------------------------------------
create or replace PROCEDURE subject_proc(
                    p_hakno NUMBER
                    )
IS
   vn_sub_cnt          NUMBER;
   vn_subject          과목.과목이름%TYPE;   /*과목명*/
   vn_subject_hak_jum  NUMBER ;            /*학점*/
BEGIN

    SELECT COUNT(*) AS sub_cnt
	INTO vn_sub_cnt
    FROM 수강내역, 과목
    WHERE 수강내역.과목번호 = 과목.과목번호(+)
    AND 수강내역.학번 = p_hakno;
        DBMS_OUTPUT.PUT_LINE ('[수강 내용]'); /* 이수과목 정보가 있을 경우 */
        IF vn_sub_cnt != 0 THEN
            FOR i in 1..vn_sub_cnt /* 이수과목 정보건수 만큼 */
            LOOP
                SELECT A.subject
                    ,  A.hak_jum
                INTO vn_subject, vn_subject_hak_jum
                FROM (
                        SELECT 과목.과목이름 AS subject
                                                                            ,과목.학점    AS hak_jum
                              ,rownum      AS sub_rnum
                        FROM 수강내역, 과목
                        WHERE 수강내역.과목번호 = 과목.과목번호(+)
                        AND 수강내역.학번 = p_hakno
                     ) A
                WHERE A.sub_rnum = i;
                DBMS_OUTPUT.PUT_LINE (' 과목:' || vn_subject || ' 학점:'||vn_subject_hak_jum );
            END LOOP;
                    /* 이수과목 정보가 없을 경우 */    
        ELSE
            DBMS_OUTPUT.PUT_LINE(' 자료가 없습니다.');	
        END IF;
END;

CREATE OR REPLACE PROCEDURE haksa_proc(
                    p_hakno NUMBER ,
                    p_out_name out VARCHAR2,
                    p_out_b_hakgi out NUMBER,
                    p_out_b_hakjum out NUMBER,
                    p_out_n_hakgi out NUMBER,
                    p_out_n_hakjum out NUMBER
                    )
IS
   vn_cnt                NUMBER;
   cn_max_num   CONSTANT NUMBER := 9;        /*1년 수강하가능 학점*/
   cn_grade_num CONSTANT NUMBER := 36;       /*졸업가능 학점*/
BEGIN
   SELECT COUNT(*) AS CNT
   INTO vn_cnt
   FROM 학생
   WHERE 학번 = p_hakno;
   
   IF vn_cnt > 0 THEN
        
        SELECT 학생.이름            AS name
              ,NVL(MAX(학생.학기),0)       AS max_hak_gi
              ,NVL(SUM(과목.학점),0)       AS sum_hak_jum
        INTO p_out_name, p_out_b_hakgi, p_out_b_hakjum   
        FROM 학생, 수강내역, 과목
        WHERE 학생.학번 = 수강내역.학번 (+)
        AND 학생.학번 = p_hakno
        AND   수강내역.과목번호 = 과목.과목번호(+)
        GROUP BY 학생.학번
               , 학생.이름;
        
        p_out_n_hakjum := cn_grade_num - p_out_b_hakjum;
        p_out_n_hakgi  := CEIL(p_out_n_hakjum / cn_max_num);
   ELSE
     RETURN;
   END IF;
END;
----프로시져사--------------------------------------------------------------------------------------------------------------------------------
DECLARE
   --반복건수-------------------------
   vn_sum_cnt            NUMBER;             /*전체 학생 수*/
   --기존자료---------------------------
   vn_hak_no             NUMBER := 0;        /*학점*/
   vn_name               VARCHAR2 (20);      /*이름*/
   vn_before_hak_gi      NUMBER := 0;        /*이수학기*/
   vn_before_hak_jum     NUMBER := 0;        /*이수학점*/
   --필요자료---------------------------
   vn_after_hak_gi       NUMBER := 0;        /*필요학점*/
   vn_after_hak_jum      NUMBER := 0;        /*필수학기*/
BEGIN
   SELECT COUNT(DISTINCT 학번) AS CNT
   INTO vn_sum_cnt
   FROM 학생;
   
   FOR i IN 1..vn_sum_cnt
   LOOP
        SELECT hak_no
        INTO vn_hak_no
        FROM (
                SELECT A.*
                      , rownum  as rnum
                FROM (
                        SELECT DISTINCT 학생.학번 AS hak_no
                        FROM 학생
                     ) A
            )B
        WHERE B.rnum = i;
        haksa_proc(vn_hak_no, vn_name, vn_before_hak_gi, vn_before_hak_jum, vn_after_hak_gi, vn_after_hak_jum);
        DBMS_OUTPUT.PUT_LINE ('----------------------------------------------------------') ;
        DBMS_OUTPUT.PUT_LINE ('[기존 자료]학번:'|| vn_hak_no || ' 이름:' || vn_name || ' 수강 학기:' || vn_before_hak_gi || ' 취득 학점:'||vn_before_hak_jum );
        subject_proc(vn_hak_no);
        DBMS_OUTPUT.PUT_LINE ('[산출 자료]학번:'|| vn_hak_no || ' 이름:' || vn_name || ' 최소 학기:' || vn_after_hak_gi || ' 필요 학점:'||vn_after_hak_jum );
        DBMS_OUTPUT.PUT_LINE ('----------------------------------------------------------') ;
   END LOOP;    
END;
```
#### 2. 레코드
 - PL/SQL 에서 제공하는 데이터 타입 중 하나로, 문자형, 숫자형 같은 기본 타입과 달리 복합형 구조이다
 - 일반 변수는 한 번에 하나의 값만 가질 수 있지만 레코드는 여러 개의 값을 가질 수 있다.
 - 테이블과 흡사하며, 여러 개의 컬럼을 각기 다른 테이터 타입으로 선언해서 사용할 수 있다 (하지만 로우는 1개) 1개이상을 쓰려면 컬렉션사용.
```sql
--11------------------------ 레코드------------------------------------------

DECLARE
  -- 부서레코드 타입선언
   TYPE depart_rect IS RECORD (
         department_id     NUMBER(6),
         department_name   VARCHAR2(80),
         parent_id         NUMBER(6),
         manager_id        NUMBER(6)   
   );
   
  -- 위에서 선언된 타입으로 레코드 변수 선언  
   vr_dep depart_rect;

BEGIN
 ...
END;


DECLARE
  -- 부서레코드 타입선언
   TYPE depart_rect IS RECORD (
         department_id     departments.department_id%TYPE,
         department_name  departments.department_name%TYPE,
         parent_id          departments.parent_id%TYPE,
         manager_id        departments.manager_id%TYPE
   );
   
  -- 위에서 선언된 타입으로 레코드 변수 선언  
   vr_dep depart_rect;
BEGIN
-- …
END;


--12
------------------------------------------------------------------------------------------
DECLARE
  -- 부서레코드 타입선언
   TYPE  depart_rect IS RECORD (
         department_id     departments.department_id%TYPE,
         department_name  departments.department_name%TYPE,
         parent_id          departments.parent_id%TYPE,
         manager_id        departments.manager_id%TYPE
   );
   
  -- 위에서 선언된 타입으로 레코드 변수 선언  
   vr_dep depart_rect;
  -- 두 번째 변수 선언
   vr_dep2 depart_rect;
BEGIN

   vr_dep.department_id := 999;
   vr_dep.department_name := '테스트부서';
   vr_dep.parent_id := 100;
   vr_dep.manager_id := NULL;
   
   -- 두 번째 변수에 첫 번째 레코드변수 대입
   vr_dep2 := vr_dep;
   
   DBMS_OUTPUT.PUT_LINE( 'vr_dep2.department_id :'   || vr_dep2.department_id);
   DBMS_OUTPUT.PUT_LINE( 'vr_dep2.department_name :' ||  vr_dep2.department_name);
   DBMS_OUTPUT.PUT_LINE( 'vr_dep2.parent_id :'       ||  vr_dep2.parent_id);
   DBMS_OUTPUT.PUT_LINE( 'vr_dep2.manager_id :'      ||  vr_dep2.manager_id);
END;

--13
------------------------------------------------------------------------------------------
DECLARE
  -- 부서레코드 타입선언
   TYPE depart_rect IS RECORD (
         department_id     departments.department_id%TYPE,
         department_name  departments.department_name%TYPE,
         parent_id          departments.parent_id%TYPE,
         manager_id        departments.manager_id%TYPE
   );
  -- 위에서 선언된 타입으로 레코드 변수 선언  
   vr_dep depart_rect;
  -- 두 번째 변수 선언
   vr_dep2 depart_rect;
BEGIN
   vr_dep.department_id := 999;
   vr_dep.department_name := '테스트부서';
   vr_dep.parent_id := 100;
   vr_dep.manager_id := NULL;
   -- 두 번째 변수의 department_name에만 할당
   vr_dep2.department_name := vr_dep.department_name;
   
   DBMS_OUTPUT.PUT_LINE( 'vr_dep2.department_id :' || vr_dep2.department_id);
   DBMS_OUTPUT.PUT_LINE( 'vr_dep2.department_name :' ||  vr_dep2.department_name);
   DBMS_OUTPUT.PUT_LINE( 'vr_dep2.parent_id :' ||  vr_dep2.parent_id);
   DBMS_OUTPUT.PUT_LINE( 'vr_dep2.manager_id :' ||  vr_dep2.manager_id);
END;

--14
------------------------------------------------------------------------------------------
CREATE TABLE ch11_dep AS
SELECT department_id, department_name, parent_id, manager_id
  FROM DEPARTMENTS ;
  
TRUNCATE TABLE   ch11_dep;
  
 DECLARE
  -- 부서레코드 타입선언
   TYPE depart_rect IS RECORD (
         department_id     departments.department_id%TYPE,
         department_name  departments.department_name%TYPE,
         parent_id          departments.parent_id%TYPE,
         manager_id        departments.manager_id%TYPE
   );
   
  -- 위에서 선언된 타입으로 레코드 변수 선언  
   vr_dep depart_rect;

BEGIN

   vr_dep.department_id := 999;
   vr_dep.department_name := '테스트부서';
   vr_dep.parent_id := 100;
   vr_dep.manager_id := NULL;
   
   -- 레코드 필드를 명시해서 INSERT
   INSERT INTO ch11_dep VALUES ( vr_dep.department_id, vr_dep.department_name, vr_dep.parent_id, vr_dep.manager_id);
   
   -- 레코드 필드 순서와 개수, 타입이 같다면 레코드변수명으로만 INSERT 가능
   INSERT INTO ch11_dep VALUES vr_dep;
   COMMIT;
END;
--15
------------------------------------------------------------------------------------------
CREATE TABLE ch11_dep2 AS
SELECT *
  FROM DEPARTMENTS;

TRUNCATE TABLE   ch11_dep2;



-- 테이블형 레코드
DECLARE
  -- 테이블형 레코드 변수 선언
   vr_dep departments%ROWTYPE;

BEGIN
   -- 부서 테이블의 모든 정보를 레코드 변수에 넣는다.
   SELECT *
     INTO vr_dep
     FROM departments
    WHERE department_id = 20;
   -- 레코드 변수를 이용해 ch11_dep2 테이블에 데이터를 넣는다.
   INSERT INTO ch11_dep2 VALUES vr_dep;
   COMMIT;
END;
```
- 커서형 레코드
```sql 
--16
------------------------------------------------------------------------------------------
-- 커서형 레코드
DECLARE
  -- 커서 선언
   CURSOR c1 IS
       SELECT department_id, department_name, parent_id, manager_id
         FROM departments;       
   -- 커서형 레코드변수 선언  
   vr_dep c1%ROWTYPE;
BEGIN
   -- 데이터 삭제
   DELETE ch11_dep;
   -- 커서 오픈
   OPEN c1;
   -- 루프를 돌며 vr_dep 레코드 변수에 값을 넣고, 다시 ch11_dep에 INSERT
   LOOP
     FETCH c1
     INTO vr_dep;
     EXIT WHEN c1%NOTFOUND;
     -- 레코드 변수를 이용해 ch11_dep2 테이블에 데이터를 넣는다.
     INSERT INTO ch11_dep VALUES vr_dep;
   END LOOP;
   COMMIT;
END;

--17
----------------------------------------------------------------------------------------------------------------
DECLARE
   -- 레코드 변수 선언
   vr_dep ch11_dep%ROWTYPE;

BEGIN
 
   vr_dep.department_id := 20;
   vr_dep.department_name := '테스트';
   vr_dep.parent_id := 10;
   vr_dep.manager_id := 200;
     
   -- ROW를 사용하면 해당 로우 전체가 갱신됨
     UPDATE ch11_dep
          SET ROW = vr_dep
      WHERE department_id = vr_dep.department_id;
   
   COMMIT;
END;

```
- 중첩 레코드
```sql
-- 중첩 레코드
--18
----------------------------------------------------------------------------------------------------------------
DECLARE
  -- 부서번호, 부서명을 필드로 가진 dep_rec 레코드 타입 선언
  TYPE  dep_rec IS RECORD (
        dep_id      departments.department_id%TYPE,
        dep_name departments.department_name%TYPE );
        
  --사번, 사원명 그리고 dep_rec(부서번호, 부서명) 타입의 레코드 선언
  TYPE emp_rec IS RECORD (
        emp_id      employees.employee_id%TYPE,
        emp_name employees.emp_name%TYPE,
        dep          dep_rec                          );
        
   --  emp_rec 타입의 레코드 변수 선언
   vr_emp_rec emp_rec;
BEGIN
   -- 100번 사원의 사번, 사원명, 부서번호, 부서명을 가져온다.
   SELECT a.employee_id, a.emp_name, a.department_id, b.department_name
     INTO vr_emp_rec.emp_id, vr_emp_rec.emp_name, vr_emp_rec.dep.dep_id, vr_emp_rec.dep.dep_name
     FROM employees a,
             departments b
    WHERE a.employee_id = 100
       AND a.department_id = b.department_id;
       
    -- 레코드 변수 값 출력    
    DBMS_OUTPUT.PUT_LINE('emp_id : ' ||  vr_emp_rec.emp_id);
    DBMS_OUTPUT.PUT_LINE('emp_name : ' ||  vr_emp_rec.emp_name);
    DBMS_OUTPUT.PUT_LINE('dep_id : ' ||  vr_emp_rec.dep.dep_id);
    DBMS_OUTPUT.PUT_LINE('dep_name : ' ||  vr_emp_rec.dep.dep_name);
END;
--------------------------------------------------------------------------------
```
#### 3. 컬렉션 
1. 연관배열 

- Associative Array 키와 값으로 구성된 컬렉션 (키를 index라고 부르기도 하기때문에 index-by 테이블 이라고도 한다. )
- 문자형이나 PLS_INTEGER 값을 사용가능
- TYPE 연관_배열명 IS TABLE OF 연관_배열_값타입 INDEX BY 인덱스타입;
```sql
-----------------------------------컬렉션----------------------------------------- 
--19

DECLARE
   -- 숫자-문자 쌍의 연관배열 선언
   TYPE av_type IS TABLE OF VARCHAR2(40) INDEX BY PLS_INTEGER;
        
   -- 연관배열 변수 선언
   vav_test av_type;
BEGIN
  -- 연관배열에 값 할당
  vav_test(10) := '10에 대한 값';
  vav_test(20) := '20에 대한 값';
  
  --연관배열 값 출력
  DBMS_OUTPUT.PUT_LINE(vav_test(10));
  DBMS_OUTPUT.PUT_LINE(vav_test(20));

END;
--20
DECLARE
   -- 숫자-문자 쌍의 연관배열 선언
   TYPE av_type IS TABLE OF VARCHAR2(40) INDEX BY VARCHAR2(30);
        
   -- 연관배열 변수 선언
   vav_test av_type;
BEGIN
  -- 연관배열에 값 할당
  vav_test('A') := '10에 대한 값';
  vav_test('B') := '20에 대한 값';
  
  --연관배열 값 출력
  DBMS_OUTPUT.PUT_LINE(vav_test('A'));
  DBMS_OUTPUT.PUT_LINE(vav_test('B'));
------------------------------------------------------------------------------------------
--문제
--키값을 member 이름  aaa('김은대') := id
-- value 값을 아이디로 하는 연관배열 을 만들어 member 테이블에 있는 요소를 담고출력하시오


DECLARE
   -- 숫자-문자 쌍의 연관배열 선언
   TYPE av_type IS TABLE OF VARCHAR2(40) INDEX BY VARCHAR2(30) ;
   -- 연관배열 변수 선언
   v_mem av_type;
BEGIN
  FOR rec IN (SELECT * FROM MEMBER)
  LOOP
  v_mem(rec.mem_name) := rec.mem_id ;
  END LOOP ;
  
  FOR rec IN (SELECT * FROM MEMBER)
  LOOP
    DBMS_OUTPUT.PUT_LINE('key : ' || rec.mem_name || 'value : ' || v_mem(rec.mem_name));
  END LOOP ;
END ;

```
2. VARRAY

- Variavle - Size Array 는 가변 길이 배열로서 연관 배열과는 달리 크 크기에 제한이 있다.
- 즉 선언 할때 크기(요소 개수)를 지정하면 이보다 큰 수로 요소를 만들 수 없다.
- 자동으로 순번이 매겨지며 최솟값은 1이다.
```sql
--21
DECLARE
   -- 5개의 문자형 값으로 이루어진 VARRAY 선언
   TYPE va_type IS VARRAY(5) OF VARCHAR2(20);
   -- VARRY 변수 선언
   vva_test va_type;
   vn_cnt NUMBER := 0;
BEGIN
  -- 생성자를 사용해 값 할당 (총 5개지만 최초 3개만 값 할당)
  vva_test := va_type('FIRST', 'SECOND', 'THIRD', '', '');
  
  LOOP
     vn_cnt := vn_cnt + 1;     
     -- 크기가 5이므로 5회 루프를 돌면서 각 요소 값 출력
     IF vn_cnt > 5 THEN
        EXIT;
     END IF;
     -- VARRY 요소 값 출력
     DBMS_OUTPUT.PUT_LINE(vva_test(vn_cnt));
  END LOOP;
  -- 값 변경
  vva_test(2) := 'TEST';
  vva_test(4) := 'FOURTH';
  -- 다시 루프를 돌려 값 출력
  vn_cnt := 0;
  LOOP
     vn_cnt := vn_cnt + 1;     
     -- 크기가 5이므로 5회 루프를 돌면서 각 요소 값 출력
     IF vn_cnt > 5 THEN
        EXIT;
     END IF;
     -- VARRY 요소 값 출력
     DBMS_OUTPUT.PUT_LINE(vva_test(vn_cnt));
  END LOOP;
END;
```
- 중첩 테이블 Nasted Table
```sql
--22
/**
 중첩 테이블 Nested Table 컬렉션 타입의 한 종류로 중첩 테이블은 크기에 제한이 없다(숫자형 인덱스만 사용가능)
 TYPE 중첩_테이블명 IS TABLE OF 값타입;
*/

DECLARE
  -- 중첩 테이블 선언
  TYPE nt_typ IS TABLE OF VARCHAR2(10);
  
  -- 변수 선언
  vnt_test nt_typ;
BEGIN

  -- 생성자를 사용해 값 할당
  vnt_test := nt_typ('FIRST', 'SECOND', 'THIRD', '');
  
  vnt_test(4) := 'FOURTH';
  
  -- 값 출력
  DBMS_OUTPUT.PUT_LINE (vnt_test(1));
  DBMS_OUTPUT.PUT_LINE (vnt_test(2));
  DBMS_OUTPUT.PUT_LINE (vnt_test(3));
  DBMS_OUTPUT.PUT_LINE (vnt_test(4));
  
END;
```
- 컬렉션 메소드 
```sql
--23
/*
 컬렉션 메소드란 컬렉션의 요소에 접근해 값을 가져오고 수정하고 삭제하는 기능을 하는 일련의 빌트인(내장형) 프로시저와 함수를 말한다.
*/
-- DELETE 메소드 프로시저 타입에 : 컬렉션의 요소를 삭제
DECLARE
   -- 숫자-문자 쌍의 연관배열 선언
   TYPE av_type IS TABLE OF VARCHAR2(40) INDEX BY VARCHAR2(10);
        
   -- 연관배열 변수 선언
   vav_test av_type;
   
   vn_cnt number := 0;
BEGIN
  -- 연관배열에 값 할당
  vav_test('A') := '10에 대한 값';
  vav_test('B') := '20에 대한 값';
  vav_test('C') := '20에 대한 값';
  
  vn_cnt := vav_test.COUNT;
  DBMS_OUTPUT.PUT_LINE('삭제 전 요소 개수: ' || vn_cnt);  
  
  vav_test.DELETE('A', 'B');
  
  vn_cnt := vav_test.COUNT;
  DBMS_OUTPUT.PUT_LINE('삭제 후 요소 개수: ' || vn_cnt);
END;
```
- TRIM 메소드
```sql
--24
-- TRIM 메소드 메소드 프로시저 타입에 : VARRAY나 중첩 테이블의 끝에서 요소를 삭제
DECLARE
  -- 중첩 테이블 선언
  TYPE nt_typ IS TABLE OF VARCHAR2(10);
  
  -- 변수 선언
  vnt_test nt_typ;
BEGIN
  -- 생성자를 사용해 값 할당
  vnt_test := nt_typ('FIRST', 'SECOND', 'THIRD', 'FOURTH', 'FIFTH');

  -- 맨 마지막부터 2개 요소 삭제
  vnt_test.TRIM(2);
  
  DBMS_OUTPUT.PUT_LINE(vnt_test(1));
  DBMS_OUTPUT.PUT_LINE(vnt_test(2));
  DBMS_OUTPUT.PUT_LINE(vnt_test(3));
  DBMS_OUTPUT.PUT_LINE(vnt_test(4));
  
  EXCEPTION WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE(SQLERRM);
      DBMS_OUTPUT.PUT_LINE( DBMS_UTILITY.FORMAT_ERROR_BACKTRACE);
END;

```
- EXTEND 메소드
```sql
--25
-- EXTEND 메소드 프로시저 타입에 : VARRAY나 중첩 테이블의 끝에서 요소를 추가
DECLARE
  -- 중첩 테이블 선언
  TYPE nt_typ IS TABLE OF VARCHAR2(10);
  
  -- 변수 선언
  vnt_test nt_typ;
BEGIN
  -- 생성자를 사용해 값 할당
  vnt_test := nt_typ('FIRST', 'SECOND', 'THIRD');

  -- 맨 끝에 NULL 요소 추가한 뒤 값 할당 후 출력
  vnt_test.EXTEND;
  vnt_test(4) := 'fourth';
  DBMS_OUTPUT.PUT_LINE(vnt_test(4));
  
  -- 맨 끝에 첫 번째 요소를 2개 복사해 추가 후 출력
  vnt_test.EXTEND(2, 1);
  DBMS_OUTPUT.PUT_LINE('첫번째 : ' || vnt_test(1));
  -- 첫 번째 요소를 복사해 2개 추가했으므로 추가된 요소는 5, 6
  DBMS_OUTPUT.PUT_LINE('추가한 요소1 : ' || vnt_test(5));
  DBMS_OUTPUT.PUT_LINE('추가한 요소2 : ' || vnt_test(6));

END;

```
- First Last 메소드
```sql
--26
-- FIRST와 LAST 메소드 함수 타입에 : 컬렉션의 첫번째 인덱스 반환, 마지막인덱스 반환
DECLARE
  -- 중첩 테이블 선언
  TYPE nt_typ IS TABLE OF VARCHAR2(10);
  
  -- 변수 선언
  vnt_test nt_typ;
BEGIN
  -- 생성자를 사용해 값 할당
  vnt_test := nt_typ('FIRST', 'SECOND', 'THIRD', 'FOURTH', 'FIFTH');

  -- FIRST와 LAST 메소드를 FOR문에서 사용해 컬렉션 값을 출력
  FOR i IN vnt_test.FIRST..vnt_test.LAST
  LOOP
  
     DBMS_OUTPUT.PUT_LINE(i || '번째 요소 값: ' || vnt_test(i));
  END LOOP;

END;
```
- COUNT, LIMIT 메소드
```sql
--27
-- COUNT와 LIMIT 메소드 함수 타입에 : 컬렉션의 요소의 총 수를 반환, 컬렉션이 가질 수 있는 요소의 최대 수를 반환

DECLARE
  
  TYPE nt_typ IS TABLE OF VARCHAR2(10);      -- 중첩테이블 선언
  TYPE va_type IS VARRAY(5) OF VARCHAR2(10); -- VARRAY 선언
 
  -- 변수 선언
  vnt_test nt_typ;
  vva_test va_type;
BEGIN
  -- 생성자를 사용해 값 할당
  vnt_test := nt_typ('FIRST', 'SECOND', 'THIRD', 'FOURTH'); -- 중첩테이블
  vva_test := va_type('첫번째', '두번째', '세번째', '네번째'); -- VARRAY
  
  DBMS_OUTPUT.PUT_LINE('VARRAY COUNT: ' || vva_test.COUNT);
  DBMS_OUTPUT.PUT_LINE('중첩테이블 COUNT: ' || vnt_test.COUNT);

  DBMS_OUTPUT.PUT_LINE('VARRAY LIMIT: ' || vva_test.LIMIT);
  DBMS_OUTPUT.PUT_LINE('중첩테이블 LIMIT: ' || vnt_test.LIMIT);  

END;
```
- PRIOR, NEXT
```sql
--28
-- PRIOR와 NEXT
DECLARE
  TYPE va_type IS VARRAY(5) OF VARCHAR2(10); -- VARRAY 선언
  -- 변수 선언
  vva_test va_type;
BEGIN
  -- 생성자를 사용해 값 할당
  vva_test := va_type('첫번째', '두번째', '세번째', '네번째'); -- VARRAY
  
  DBMS_OUTPUT.PUT_LINE('FIRST의 PRIOR : ' || vva_test.PRIOR(vva_test.FIRST));
  DBMS_OUTPUT.PUT_LINE('LAST의 NEXT : ' || vva_test.NEXT(vva_test.LAST));

  DBMS_OUTPUT.PUT_LINE('인덱스3의 PRIOR :' || vva_test.PRIOR(3));
  DBMS_OUTPUT.PUT_LINE('인덱스3의 NEXT :' || vva_test.NEXT(3));

END;


---------------------------------------------------------------------------------------
--문제
--member 테이블의 이름을 가져와서 중첩테이블 형태에 변수에 담고
--중첩테이블의 첫 번째 인덱스의 값 ~ 마지막 인덱스의 값을 출력하시오

DECLARE
 TYPE mem_typ IS TABLE OF VARCHAR2(200);
   -- 연관배열 변수 선언
   mem mem_typ;
BEGIN
mem := mem_typ();
FOR rec IN(select mem_name from member)
LOOP
mem.EXTEND;
mem(mem.last):=rec.mem_name;
END LOOP ;
for i in mem.first..mem.last
loop
    DBMS_OUTPUT.PUT_LINE('mem : ' || mem(i));
    end loop ;
    end ;
```
- 사용자 정의 데이터 타입 
```sql
---------------------------------------------------------------------------------------
--29
-- 사용자 정의 데이터 타입 (유형 트리에 생성됨)
-- 5개의 문자형 값으로 이루어진 VARRAY 사용자정의타입 선언
CREATE OR REPLACE TYPE ch11_va_type IS VARRAY(5) OF VARCHAR2(20);

--30
-- 문자형 값의 중첩테이블 사용자정의타입 선언
CREATE OR REPLACE TYPE ch11_nt_type IS TABLE OF VARCHAR2(20);


--31
-- 사용자정의타입인 va_type와 nt_type 사용
DECLARE
   vva_test ch11_va_type;  -- VARRAY인 va_type 변수선언   
   vnt_test ch11_nt_type;  -- 중첩테이블인  nt_type 변수선언   

BEGIN
    -- 생성자를 사용해 값 할당 (총 5개지만 최초 3개만 값 할당)
    vva_test := ch11_va_type('FIRST', 'SECOND', 'THIRD', '', '');
    vnt_test := ch11_nt_type('FIRST', 'SECOND', 'THIRD', '');
    
    DBMS_OUTPUT.PUT_LINE('VARRAY의 1번째 요소값: ' || vva_test(1));
    DBMS_OUTPUT.PUT_LINE('중첩테이블의 1번째 요소값: ' || vnt_test(1));

END;

```
- 컬렉션 타입별 차이점과 활용
```sql
-- 다차원 컬렉션
--32
DECLARE
    -- 첫 번째 VARRAY 타입선언 (구구단중 각단 X5값을 가진  요소를 갖는 VARRAY )
    TYPE va_type1 IS VARRAY(5) OF NUMBER;
    
    -- 위에서 선언한 va_type1을 요소타입으로 하는 VARRAY 타입 선언 (구구단중 1~3단까지 요소를 갖는 VARRAY)
    TYPE va_type11 IS VARRAY(3) OF va_type1;
    -- 두번째 va_type11 타입의 변수 선언
    va_test va_type11;
BEGIN
    -- 생성자를 이용해 값 초기화,
    va_test := va_type11( va_type1(1, 2, 3, 4, 5),
                          va_type1(2, 4, 6, 8, 10),
                          va_type1(3, 6, 9, 12, 15)
                       );
                        
   -- 구구단 출력                               
   DBMS_OUTPUT.PUT_LINE('2곱하기 3은 ' || va_test(2)(3));             
   DBMS_OUTPUT.PUT_LINE('3곱하기 5는 ' || va_test(3)(5));               

END;


--33
DECLARE
    -- 요소타입을 employees%ROWTYPE 로 선언, 즉 테이블형 레코드를 요소 타입으로 한 중첩테이블
    TYPE nt_type IS TABLE OF employees%ROWTYPE;

   -- 중첩테이블 변수선언
   vnt_test nt_type;
BEGIN
  -- 빈 생성자로 초기화
  vnt_test := nt_type();
  
  -- 중첩테이블에 요소 1개 추가
  vnt_test.EXTEND;
  
  -- 사원테이블에서 100번 사원의 정보를 가져온다.
  SELECT *
     INTO vnt_test(1) -- 위에서 요소1개를 추가했으므로 인덱스는 1
    FROM employees
   WHERE employee_id = 100;
   
  -- 100반 사원의 사번과 성명 출력
  DBMS_OUTPUT.PUT_LINE(vnt_test(1).employee_id);
  DBMS_OUTPUT.PUT_LINE(vnt_test(1).emp_name);

END;


--34
DECLARE
    -- 요소타입을 employees%ROWTYPE 로 선언, 즉 테이블형 레코드를 요소 타입으로 한 중첩테이블
    TYPE nt_type IS TABLE OF employees%ROWTYPE;

   -- 중첩테이블 변수선언
   vnt_test nt_type;
BEGIN
  -- 빈 생성자로 초기화
  vnt_test := nt_type();
  
  -- 사원테이블 전체를 중첩테이블에 담는다.
  FOR rec IN ( SELECT * FROM employees)
  LOOP
     -- 요소 1개 추가
     vnt_test.EXTEND;
     
     -- LAST 메소드를 사용하면 항상 위에서 추가한 요소의 인덱스를 가져온다.
     vnt_test ( vnt_test.LAST) := rec;
     
  END LOOP;
   
  -- 출력
  FOR i IN vnt_test.FIRST..vnt_test.LAST
  LOOP
       DBMS_OUTPUT.PUT_LINE(vnt_test(i).employee_id || ' - ' ||   vnt_test(i).emp_name);
  END LOOP;

END;
```

 ## 📚8장 고급 SQL
### 1. 계층형 쿼리
```sql
---------------------------------------------------
SELECT department_id as 부서번호
    , LPAD(' ', 3*(LEVEL-1)) || department_name as 부서명
    , SYS_CONNECT_BY_PATH(department_name, '||')
    -- 루트 노트에서 시작해 자신의 행까지 연결된 경로 정보를 반환
    , LEVEL
    , CONNECT_BY_ISLEAF     --마지막 노드면 1, 자식이 있으면 0
    , CONNECT_BY_ROOT department_name as root_name -- 최상위
    , CONNECT_BY_ROOT department_id as root_id     -- 최상위
FROM departments
START WITH parent_id IS NULL                       --< 이 조건에 맞는 로우부터 시작함.
CONNECT BY PRIOR department_id = parent_id         --< 계층형 구조가 어떤식으로 연결되는지 기술(조건)
                                                    -- PRIOR의 위치 중요
ORDER SIBLINGS BY DEPARTMENT_NAME -- 정렬가능
-- 시작을 PARENT_ID 컬럼이 null인 걸로 하고
-- DEPARTMENT_ID의 이전 부서들의 PARENT_ID를 찾으라는 뜻

---------------------------------------------------
--문제
create table jojicdo3 (
name varchar2(50),
class varchar2(50),
class_id varchar2(50),
parent_class varchar2(50)   default null
);

drop table jojicdo3;

select *
from jojicdo3;

insert into jojicdo3(name, class, class_id, parent_class)
values('이사장', '사장', '10', '');

insert into jojicdo3(name, class, class_id, parent_class)
values('김부장', '부장', '20', '10');

insert into jojicdo3(name, class, class_id, parent_class)
values('서차장', '차장', '30', '20');

insert into jojicdo3(name, class, class_id, parent_class)
values('장과장', '과장', '40', '30');

insert into jojicdo3(name, class, class_id, parent_class)
values('이대리', '대리', '50', '40');

insert into jojicdo3(name, class, class_id, parent_class)
values('최사원', '사원', '60', '50');

insert into jojicdo3(name, class, class_id, parent_class)
values('강사원', '사원', '61', '50');

insert into jojicdo3(name, class, class_id, parent_class)
values('박과장', '과장', '41', '30');

insert into jojicdo3(name, class, class_id, parent_class)
values('김대리', '대리', '51', '41');

insert into jojicdo3(name, class, class_id, parent_class)
values('남궁사원', '사원', '62', '51');

---------------------------------------------------
SELECT name  
    , LPAD(' ', 3*(LEVEL-1)) || class
    , LEVEL
FROM jojicdo3
START WITH parent_class IS NULL                      
CONNECT BY PRIOR class_id = parent_class    ; 
```


- 무한 루프 상황   
```sql
UPDATE departments
   SET parent_id = 170
 WHERE department_id = 30;
 
SELECT department_id, LPAD(' ' , 3 * (LEVEL-1)) || department_name, LEVEL,
       parent_id
  FROM departments
  START WITH department_id = 30
CONNECT BY PRIOR department_id  = parent_id;
--- 무한 루프 걸림

SELECT department_id, LPAD(' ' , 3 * (LEVEL-1)) || department_name AS depname, LEVEL,
       CONNECT_BY_ISCYCLE IsLoop,      -- 무한루프 원인 찾음 ( 현재 로우가 자식을 갖고 있는데 동시에 그 자식 로우가 부모 로우면 1, 아니면 0
       parent_id
  FROM departments
  START WITH department_id = 30
CONNECT BY NOCYCLE PRIOR department_id  = parent_id;
```
- CONNECT BY 절을 이용하여 계층 질의에서 상위계층(부모행)과 하위계층(자식행)의 관계를 규정 할 수 있다.
- PRIOR 연산자와 함께 사용하여 계층구조로 표현할 수 있다.
- CONNECT BY PRIOR 자식컬럼 = 부모컬럼 : 부모에서 자식으로 트리구성 (Top Down)
- CONNECT BY PRIOR 부모컬럼 = 자식컬럼 : 자식에서 부모로 트리 구성 (Bottom Up)
- CONNECT BY NOCYCLE PRIOR : NOCYCLE 파라미터를 이용하여 무한루프 방지 서브쿼리를 사용할 수 없다.
        
```sql
select department_id
     , lpad(' ', (level - 1) * 3) ||  department_name
     , parent_id
     , level as lvl
  from departments
 start with department_id = 90
connect by  department_id = prior parent_id ;  -- bottom -> top
-- connect by prior department_id = parent_id ;  --  top -> bottom
-------------------------------------------------------------------------------

```
### 2.계층형 쿼리 응용
- level은 오라클에서 실행되는 모든 쿼리 내에서 사용 가능한 가상열로서,
- 트리 내에서 어떤 단계(level)에 있는지를 나타내는 정수값이다.
- 계층적인 쿼리가 아니라면 다음과 같이 모든 값이 0, 즉 같은 단계를 가질 것이다.
```sql
-- 샘플 데이터 생성
SELECT level
FROM DUAL
CONNECT BY LEVEL <= 12 ; -- 1~12 생성
---------------------------------------------------------------
SELECT '2021년' || LPAD(LEVEL,2,0) || '월' as num -- 01~12 생성
FROM DUAL
CONNECT BY LEVEL <=12;
---------------------------------------------------------------
--201712월 금전지점의 예약 수를 구하시오 월요일 ~ 일요일

SELECT DECODE(T1.요일, 1 , '일요일'
                    , 2 , '월요일'
                    , 3 , '화요일'
                    , 4 , '수요일'
                    , 5 , '목요일'
                    , 6 , '금요일'
                    , 7 , '토요일')as 요일
             ,nvl(t2.건수, 0)
    from(
        select level as 요일
        from dual
        connect by level <= 7)t1 ,
    (SELECT TO_CHAR(TO_DATE(RESERV_DATE), 'D')as 요일
         , COUNT(RESERV_NO) as 건수
    FROM RESERVATION
    WHERE branch = '금천'
    and substr(RESERV_DATE,1,6) = '201712'
    GROUP BY TO_CHAR(TO_DATE(RESERV_DATE), 'D')
        )T2
where t1.요일 = t2.요일(+)
order by t1.요일 ;
```
- 문제
```sql
/*
 모든상품의 월별 매출액을 구하시오
 매출월, SPECIAL_SET, PASTA, PIZZA, SEA_FOOD, STEAK, SALAD_BAR, SALAD, SANDWICH, WINE, JUICE
*/

SELECT SUBSTR(A.reserv_date,1,6) 매출월,  
       SUM(DECODE(B.item_id,'M0001',B.sales,0)) SPECIAL_SET,
       SUM(DECODE(B.item_id,'M0002',B.sales,0)) PASTA,
       SUM(DECODE(B.item_id,'M0003',B.sales,0)) PIZZA,
       SUM(DECODE(B.item_id,'M0004',B.sales,0)) SEA_FOOD,
       SUM(DECODE(B.item_id,'M0005',B.sales,0)) STEAK,
       SUM(DECODE(B.item_id,'M0006',B.sales,0)) SALAD_BAR,
       SUM(DECODE(B.item_id,'M0007',B.sales,0)) SALAD,
       SUM(DECODE(B.item_id,'M0008',B.sales,0)) SANDWICH,
       SUM(DECODE(B.item_id,'M0009',B.sales,0)) WINE,
       SUM(DECODE(B.item_id,'M0010',B.sales,0)) JUICE
FROM reservation A, order_info B
WHERE A.reserv_no = B.reserv_no
GROUP BY SUBSTR(A.reserv_date,1,6)
ORDER BY SUBSTR(A.reserv_date,1,6);
```
- 문제
```sql
/*
문제
월별 온라인_전용 상품 매출액을 일요일부터 월요일까지 구분해 출력하시오
날짜, 상품명, 일요일, 월요일, 화요일, 수요일, 목요일, 금요일, 토요일의 매출을 구하시오
*/
select  t1.날짜,
        nvl(t2.일,0)일 , nvl(t2.월,0)월,
        nvl(t2.화,0)화, nvl(t2.수,0)수,
        nvl(t2.목,0)목, nvl(t2.금,0)금, nvl(t2.토,0)토
from(select '2017'||lpad(level,2,0) as 날짜
     from dual
     connect by level <= 12)t1,
            (SELECT SUBSTR(A.reserv_date,1,6) 날짜,  
                    sum(decode(TO_CHAR(TO_DATE(A.reserv_date), 'D'),'1', B.sales, 0))일,
                    sum(decode(TO_CHAR(TO_DATE(A.reserv_date), 'D'),'2', B.sales, 0))월,
                    sum(decode(TO_CHAR(TO_DATE(A.reserv_date), 'D'),'3', B.sales, 0))화,
                    sum(decode(TO_CHAR(TO_DATE(A.reserv_date), 'D'),'4', B.sales, 0))수,
                    sum(decode(TO_CHAR(TO_DATE(A.reserv_date), 'D'),'5', B.sales, 0))목,
                    sum(decode(TO_CHAR(TO_DATE(A.reserv_date), 'D'),'6', B.sales, 0))금,
                    sum(decode(TO_CHAR(TO_DATE(A.reserv_date), 'D'),'7', B.sales, 0))토
        
            FROM reservation A, order_info B
            WHERE A.reserv_no = B.reserv_no
            and B.ITEM_ID = 'M0001'
            GROUP BY SUBSTR(A.reserv_date,1,6)
            ORDER BY SUBSTR(A.reserv_date,1,6)
                )t2
where t1.날짜 = t2.날짜(+)
order by t1.날짜;

---------------------------------------------------------------

```

### 3.WITH절
- 별칭으로 사용한 SELECT문의 FROM절에 다른 SELECT 구문의 별칭 참조가 가능하다.
- 반복되는 서브쿼리면 빼서 변수처럼 사용, 통계쿼리나 튜닝할 때, 많이 사용
  1. temp라는 임시 테이블을 사용해서 장시간 걸리는 쿼리의 결과를 저장해놓고 저장해놓은데이터를 엑세스하기 때문에 성능이 좋다.
  2. 임시로 테이블을 생성해서 쿼리를 해야햘 때에 굳이 테이블을 생성하지 않고 with절을 사용해서 임시로 테이블을 생성하면 된다.
```sql
WITH A AS

        (SELECT EMPLOYEE_ID
              , EMP_NAME
              , DEPARTMENT_ID
              , JOB_ID
              , SALARY
         FROM EMPLOYEES)
SELECT*
FROM A;
----------------------------------------
WITH A AS

        (SELECT EMPLOYEE_ID
              , EMP_NAME
              , DEPARTMENT_ID
              , JOB_ID
              , SALARY
         FROM EMPLOYEES),
B AS(SELECT DEPARTMENT_ID
          , SUM(SALARY) AS DEP_SUM
          , COUNT(DEPARTMENT_ID) AS DEP_CNT
          FROM A
          GROUP BY DEPARTMENT_ID)
SELECT*
FROM A, B
WHERE A.DEPARTMENT_ID = B.DEPARTMENT_ID ;
```

- 문제 : 일반 select 문으로
```sql
----------------------------------------
--kor_loan_status 테이블에서 '연도별' '최종월(마지막월)' 기준 가장 대출이 많은 도시와 잔액을 구하시오
----------------------------------------
-- 1. 연도별 최종 : 2011년의 최종년도는 12월 이지만 2013년은 11월 이므로 연도별 최종월을 알아야 한다.
-- tip.. 그룹쿼리로 연도별 가장 큰 월을 구한다. max
-- 2. 연도별 최종월을 대상으로 대출잔액이 가장 큰 금액을 추출한다.
-- tip 1번과 조인을 해서 연도별로 가장 큰 잔액을 구한다. max
-- 3. 월별, 지역별 대출 잔액과 2의 결과를 비교해 금액이 같은 건을 추출한다.
-- tip2와 조인(년도, 잔액)을 해서 두 금액이 같은 건을 구한다.
----------------------------------------
--월별 지역별 잔액의 합
select  period
     , region
     , sum(loan_jan_amt)
from kor_loan_status
group by period
        ,region;
----------------------------------------
--년도별 최종월

select max(period)
from kor_loan_status
group by substr(period, 1, 4);
----------------------------------------
select b.period
     , max(b.jan_amt)       -- 최종월의 맥스(전체지역에서 가장 큰값)
from (select  period
            , region
            , sum(loan_jan_amt) as jan_amt  --월별 지역별 잔액의 합
      from kor_loan_status
      group by period
              ,region ) b,
     (select max(period) as max_period      --잔액의
      from kor_loan_status
      group by substr(period, 1, 4)
      )a
            where b.period = a.max_period
            group by b.period ;
            
----------------------------------------
--WITH절 활용
WITH a AS

       ( select max(period) as max_period      --잔액의
      from kor_loan_status
      group by substr(period, 1, 4)),
b AS(select  period
            , region
            , sum(loan_jan_amt) as jan_amt  --월별 지역별 잔액의 합
      from kor_loan_status
      group by period
              ,region)
SELECT b.period
     , max(b.jan_amt)
FROM a, b
WHERE a.max_period = b.period
group by b.period ;
            
            
            
            
                
---------------------------------------------------------------
select b2.*
from(
    select period, region, sum(loan_jan_amt) as jan_amt  
      from kor_loan_status
      group by period, region
    )b2,-----------------------------------------------------반복됨
     (
     select b.period
            ,max(b.jan_amt) as jan_amt  
      from(
          select period, region, sum(loan_jan_amt) as jan_amt  
          from kor_loan_status
          group by period, region
          )b,------------------------------------------------반복됨
     (select max(period) as max_period      --잔액의
      from kor_loan_status
      group by substr(period, 1, 4)
      )a
    where b.period = a.max_period
    group by b.period
    )c
where b2.period = c.period
and b2.jan_amt = c.jan_amt;
---------------------------------------------------------------
--년월별 아이템의 합계와 전체 합계를 구하시오
select  t1.날짜,
        nvl(t2.SPECIAL_SET,0)SPECIAL_SET ,
        nvl(t2.PASTA,0)PASTA,
        nvl(t2.PIZZA,0)PIZZA,
        nvl(t2.SEA_FOOD,0)SEA_FOOD,
        nvl(t2.STEAK,0)STEAK,
        nvl(t2.SALAD_BAR,0)SALAD_BAR,
        nvl(t2.SALAD,0)SALAD,
        nvl(t2.SANDWICH,0)SANDWICH,
        nvl(t2.WINE,0)WINE,
        nvl(t2.JUICE,0)JUICE
from(select '2017'||lpad(level,2,0) as 날짜
     from dual
     connect by level <= 12)t1,
(SELECT SUBSTR(A.reserv_date,1,6) 매출월,  
       SUM(DECODE(B.item_id,'M0001',B.sales,0)) SPECIAL_SET,
       SUM(DECODE(B.item_id,'M0002',B.sales,0)) PASTA,
       SUM(DECODE(B.item_id,'M0003',B.sales,0)) PIZZA,
       SUM(DECODE(B.item_id,'M0004',B.sales,0)) SEA_FOOD,
       SUM(DECODE(B.item_id,'M0005',B.sales,0)) STEAK,
       SUM(DECODE(B.item_id,'M0006',B.sales,0)) SALAD_BAR,
       SUM(DECODE(B.item_id,'M0007',B.sales,0)) SALAD,
       SUM(DECODE(B.item_id,'M0008',B.sales,0)) SANDWICH,
       SUM(DECODE(B.item_id,'M0009',B.sales,0)) WINE,
       SUM(DECODE(B.item_id,'M0010',B.sales,0)) JUICE
FROM reservation A, order_info B
WHERE A.reserv_no = B.reserv_no
GROUP BY ROLLUP(SUBSTR(A.reserv_date,1,6))
ORDER BY SUBSTR(A.reserv_date,1,6)
) t2
where t1.날짜 = t2.매출월(+)
GROUP BY
order by t1.날짜;

---------------------------------------------------------------



select  t1.날짜,
        nvl(t2.SPECIAL_SET,0)SPECIAL_SET , nvl(t2.PASTA,0)PASTA,
        nvl(t2.PIZZA,0)PIZZA, nvl(t2.SEA_FOOD,0)SEA_FOOD,
        nvl(t2.STEAK,0)STEAK, nvl(t2.SALAD_BAR,0)SALAD_BAR, nvl(t2.SALAD,0)SALAD
        , nvl(t2.SANDWICH,0)SANDWICH, nvl(t2.WINE,0)WINE, nvl(t2.JUICE,0)JUICE
from(select '2017'||lpad(level,2,0) as 날짜
     from dual
     connect by level <= 12)t1,
(SELECT SUBSTR(A.reserv_date,1,6) 매출월,  
       SUM(DECODE(B.item_id,'M0001',B.sales,0)) SPECIAL_SET,
       SUM(DECODE(B.item_id,'M0002',B.sales,0)) PASTA,
       SUM(DECODE(B.item_id,'M0003',B.sales,0)) PIZZA,
       SUM(DECODE(B.item_id,'M0004',B.sales,0)) SEA_FOOD,
       SUM(DECODE(B.item_id,'M0005',B.sales,0)) STEAK,
       SUM(DECODE(B.item_id,'M0006',B.sales,0)) SALAD_BAR,
       SUM(DECODE(B.item_id,'M0007',B.sales,0)) SALAD,
       SUM(DECODE(B.item_id,'M0008',B.sales,0)) SANDWICH,
       SUM(DECODE(B.item_id,'M0009',B.sales,0)) WINE,
       SUM(DECODE(B.item_id,'M0010',B.sales,0)) JUICE
       
FROM reservation A, order_info B
WHERE A.reserv_no = B.reserv_no
GROUP BY SUBSTR(A.reserv_date,1,6)

ORDER BY SUBSTR(A.reserv_date,1,6)
) t2
where t1.날짜 = t2.매출월(+)
order by t1.날짜;
             
---------------------------------------------------------------

select  DECODE(t1.날짜, 201713 , '전체', t1.날짜),
        nvl(t2.SPECIAL_SET,0)SPECIAL_SET ,
        nvl(t2.PASTA,0)PASTA,
        nvl(t2.PIZZA,0)PIZZA,
        nvl(t2.SEA_FOOD,0)SEA_FOOD,
        nvl(t2.STEAK,0)STEAK,
        nvl(t2.SALAD_BAR,0)SALAD_BAR,
        nvl(t2.SALAD,0)SALAD,
        nvl(t2.SANDWICH,0)SANDWICH,
        nvl(t2.WINE,0)WINE,
        nvl(t2.JUICE,0)JUICE
from(select '2017'||lpad(level,2,0) as 날짜
     from dual
     connect by level <= 13)t1,
(SELECT NVL(SUBSTR(A.reserv_date,1,6),201713) 매출월,  
       SUM(DECODE(B.item_id,'M0001',B.sales,0)) SPECIAL_SET,
       SUM(DECODE(B.item_id,'M0002',B.sales,0)) PASTA,
       SUM(DECODE(B.item_id,'M0003',B.sales,0)) PIZZA,
       SUM(DECODE(B.item_id,'M0004',B.sales,0)) SEA_FOOD,
       SUM(DECODE(B.item_id,'M0005',B.sales,0)) STEAK,
       SUM(DECODE(B.item_id,'M0006',B.sales,0)) SALAD_BAR,
       SUM(DECODE(B.item_id,'M0007',B.sales,0)) SALAD,
       SUM(DECODE(B.item_id,'M0008',B.sales,0)) SANDWICH,
       SUM(DECODE(B.item_id,'M0009',B.sales,0)) WINE,
       SUM(DECODE(B.item_id,'M0010',B.sales,0)) JUICE
FROM reservation A, order_info B
WHERE A.reserv_no = B.reserv_no
GROUP BY ROLLUP(SUBSTR(A.reserv_date,1,6))
ORDER BY SUBSTR(A.reserv_date,1,6)
) t2
where t1.날짜 = t2.매출월(+)
order by t1.날짜;
```

 ### 4. 분석 함수와 window 함수
 - 분석함수란 테이블에 있는 로우에 대해 특정 그룹별로 집계 값을 산출할 때 사용된다.
 - ROW_NUMBER()로우에 대한 순번반환(단순순번)
 - partition by 에 department_id를 통해 부서별로
```sql
--------------------------------------------------------
select department_id
     , emp_name
     , salary
     , ROW_NUMBER() OVER (PARTITION BY department_id
                          ORDER BY emp_name desc) as dep_rows--그룹별로 로우에 대한 순번반환(단순순번)
from employees;

--------------------------------------------------------
--member table에서 mem_name, mem_job, mem_mileage, 직업별로 순번 ㅎ->ㄱ 순으로 정렬
select mem_name
     , mem_job
     , mem_mileage
     , ROW_NUMBER() OVER (PARTITION BY  mem_job
                          ORDER BY mem_name desc) as job_rows
from member;
--------------------------------------------------------
```
- RANK() , DENSE_RANK()
```sql
--RANK()
SELECT department_id
     , emp_name
     , salary,
--RANK 동일한 값이 있을 경우 1 2 2 4 다음 번호를 중복 수 만큼 넘어감
     RANK() OVER (PARTITION BY department_id    -- 부서별로 순위
                  ORDER BY salary) dep_rnak
    ,RANK() OVER (ORDER BY salary) dep_all_rank -- 전체 순위
    
--DENSE_RANK 동일한 값이 있을 경우 1 2 2 3 다음 번호를 건너 뛰지 않는다.
    ,DENSE_RANK() OVER (PARTITION BY department_id
                  ORDER BY salary) dep_dense_rank
FROM employees;

--------------------------------------------------------
-- 사번, 사원명, 급여, 해당부서의 총합,
select employee_id
     , emp_name
     , salary
     , department_id
     , SUM(salary) over (partition by department_id)
     , SUM(salary) over ()
     , avg(salary) over (partition by department_id)
FROM employees;

--------------------------------------------------------
--부서별 salary가 가장 큰 한 명씩 출력하시오 salary null 제외
 select *
 from departments;
 
 select d.department_id
      , e.emp_name
      , e.salary
from departments d
    ,employees e
where d.department_id = e.department_id
group by d.department_id
        ,e.emp_name
        ,e.salary
;


SELECT *
FROM (SELECT department_id, emp_name,
             salary,
             DENSE_RANK() OVER (PARTITION BY department_id
                                ORDER BY nvl(salary,0) desc) dep_rank
      FROM employees
      )
WHERE dep_rank = 1;
             
--------------------------------------------------------
--employee 전체에서 salary 급여 많은 순위 10위까지 출력하시오

SELECT *
FROM
        (
        SELECT EMP_NAME
             , NVL(SALARY,0)
             , DENSE_RANK() OVER (  -- 전체를 대상으로 할 때는 파티션 빼도됨
                            ORDER BY NVL(SALARY,0) DESC) 순위
        FROM employees
        )
WHERE 순위 <= 10 ;

--------------------------------------------------------
--CART, PROD 활용하여 물품별 PROD_SALE 합계의 순위(큰순)를 출력하시오(동일 순위 건너뜀)
--PROD_LGU가 'P101' 인 상품의




SELECT P.PROD_ID
     , P.PROD_NAME
     , SUM(P.PROD_SALE * c.cart_qty)
     , RANK() OVER (ORDER BY P.PROD_SALE DESC) RANK
FROM  PROD P
    , CART C
WHERE C.CART_PROD = P.PROD_ID
AND P.PROD_ID LIKE 'P101%'
GROUP BY P.PROD_ID
       , P.PROD_NAME
       , P.PROD_SALE;
```

- NTILE(expr) : 파티션 별로 명시된 값만큼 분할한 결과를 반환
  - NTILE(3)이면 1~3 사이 수를 반환 분할하는 수를 버킷 수라고 하며 3면 3개로 나눠담는 뜻(로우의 수보다 큰 수를 명시하면 반환되는 수는 무시한다.)
```sql
--------------------------------------------------------

select employee_id
     , emp_name
     , salary
     , NTILE(3) OVER(PARTITION BY department_id
                     ORDER BY salary
            )NTILES
FROM employees
WHERE department_id IN(30, 60) ;

--------------------------------------------------------
```
- WITH_BUCKET(expr, min_value, max_value, num_buckets)
  - NTILE 처럼 분할 결과를 반환하는데, 다른 점음 expr 값에 따라 min_value 최소값, num_buckets 최대값을 주어 num_buckets 수만큼 분할한다.
```sql

select employee_id
     , emp_name
     , salary
     , NTILE(4) OVER(PARTITION BY department_id
                     ORDER BY salary
            )NTILES
     , WIDTH_BUCKET(salary, 1000, 10000, 4 )as widthbucket
FROM employees
WHERE department_id = 60 ;
--------------------------------------------------------
```
- LAG
- LEAD
```sql
--주어진 그룹과 순서에 따라 로우에 있는 값을 참조할 때 사용
--LAG

select employee_id
     , emp_name
     , salary
     , department_id
     , hire_date
     , LAG(emp_name,1,'가장 높음') over (
                partition by department_id order by salary desc) ap_emp_lag -- 선행
     , LEAD(emp_name,1,'아래') over (
                partition by department_id order by salary desc) ap_emp_lead -- 후행
    from employees
where department_id in (30, 60);
--------------------------------------------------------
-- 학생의 전공별에서 평점 기준 과탑을 출력하시오

select a.*
from
        (
        select 전공
             , 이름
             , LAG(전공,1,'학과탑') over (
                        partition by 전공 order by 평점 desc) ap_emp_lag -- 선행
        from 학생 ) a
where 전공 is not null
and ap_emp_lag  = '학과탑'
;
--------------------------------------------------------
```

```sql
------------------------------------------------------------------------------
select employee_id
     , emp_name
     , salary
    
      , SUM(salary) over (partition by department_id order by hire_Date
                         rows between unbounded preceding and unbounded following
                         ) as all_salary -- 파티션에서 처음부터 끝까지 합
      , SUM(salary) over (partition by department_id order by hire_Date
                         rows between unbounded preceding and current row
                         ) as first_current_sal -- 파티션에서 처음부터 나까지
      , SUM(salary) over (partition by department_id order by hire_Date
                         rows between current row and unbounded following
                         ) as current_end_sal  -- 파티션에서 나부터 끝까지
FROM employees
where department_id in (30, 90);

------------------------------------------------------------------------------
select employee_id
     , emp_name
     , salary
      , SUM(salary) over (partition by department_id order by hire_Date
                         rows between 1 preceding and current row
                         ) as preceding_jun1_na --
      , SUM(salary) over (partition by department_id order by hire_Date
                         rows between 1 preceding and 1 following
                         ) as preceding_jun1_na_daum1 --
      , SUM(salary) over (partition by department_id order by hire_Date
                         rows between current row and 2 following
                         ) as following_na_daum2  --
FROM employees
where department_id in (90, 30);
------------------------------------------------------------------------------
-- kor_loan_status 테이블을 활용하여 년도별 서울의 대출금액과 년도별 누적 합산 금액을 출력하시오
            
select t2.yy
     , t2.REGION
     , t2.sum_loan
     , SUM(t2.sum_loan) over (order by t2.yy
                         rows between unbounded preceding and CURRENT row
                         ) as LOAN_JAN_AMT2
from
    (
                  select substr(PERIOD,1,4) as yy
                      , REGION
                      , sum(LOAN_JAN_AMT) as sum_loan
                 from kor_loan_status
                 where REGION = '서울'
                 group by substr(PERIOD,1,4), REGION
     )t2;

------------------------------------------------------------------------------
```
- RANGE 논리적 범위(숫자와 날짜의 형태로 범위를 줄 수 있다)
```sql
-- RANGE 논리적 범위(숫자와 날짜의 형태로 범위를 줄 수 있다)
select employee_id
     , emp_name
     , salary
      , SUM(salary) over (partition by department_id order by hire_Date
                         range between unbounded preceding and unbounded following
                         ) as all_salary
      , SUM(salary) over (partition by department_id order by hire_Date
                         range 365 preceding
                         ) as range_sall
      , SUM(salary) over (partition by department_id order by hire_Date
                         range between 365 preceding and current row
                         ) as range_sal2  
FROM employees
where department_id in (30, 90);
------------------------------------------------------------------------------
--지역별 대출종류별, 월별 대출잔액과 지역별 파티션을 만들어 대출종류별 대출잔액의 %를 구하는 쿼리를 작성해보자.
--KOR_LOAN_STATUS 테이블
select *
from KOR_LOAN_STATUS ;

select REGION 지역
     , GUBUN  대출종류
     , SUM(DECODE(TO_CHAR(PERIOD), 201111, LOAN_JAN_AMT, 0)) "201111"
     , SUM(DECODE(TO_CHAR(PERIOD), 201112, LOAN_JAN_AMT, 0)) "201112"
     , SUM(DECODE(TO_CHAR(PERIOD), 201210, LOAN_JAN_AMT, 0)) "201210"
     , SUM(DECODE(TO_CHAR(PERIOD), 201211, LOAN_JAN_AMT, 0)) "201211"
     , SUM(DECODE(TO_CHAR(PERIOD), 201212, LOAN_JAN_AMT, 0)) "201212"
     , SUM(DECODE(TO_CHAR(PERIOD), 201310, LOAN_JAN_AMT, 0)) "201310"
     , SUM(DECODE(TO_CHAR(PERIOD), 201311, LOAN_JAN_AMT, 0)) "201311"
from KOR_LOAN_STATUS
GROUP BY REGION
     , GUBUN
order by 3 desc;


     
select REGION 지역
     , GUBUN  대출종류
     , LOAN_JAN_AMT
     , SUM(DECODE(TO_CHAR(PERIOD), 201111, LOAN_JAN_AMT, 0)) "201111"
     , RATIO_TO_REPORT(LOAN_JAN_AMT) over (partition by (DECODE(TO_CHAR(PERIOD), 201111, LOAN_JAN_AMT, 0)) ) * 100 as "2"
     , SUM(DECODE(TO_CHAR(PERIOD), 201112, LOAN_JAN_AMT, 0)) "201112"
     , SUM(DECODE(TO_CHAR(PERIOD), 201210, LOAN_JAN_AMT, 0)) "201210"
     , SUM(DECODE(TO_CHAR(PERIOD), 201211, LOAN_JAN_AMT, 0)) "201211"
     , SUM(DECODE(TO_CHAR(PERIOD), 201212, LOAN_JAN_AMT, 0)) "201212"
     , SUM(DECODE(TO_CHAR(PERIOD), 201310, LOAN_JAN_AMT, 0)) "201310"
     , SUM(DECODE(TO_CHAR(PERIOD), 201311, LOAN_JAN_AMT, 0)) "201311"
from KOR_LOAN_STATUS
GROUP BY REGION
     , GUBUN
     , LOAN_JAN_AMT
order by 3 desc;



select t1.지역
     , t1.대출종류
     , t1."201111" || '(' || round(RATIO_TO_REPORT("201111") over (partition by 지역)*100) ||'%)' as "201111"
     , t1."201112" || '(' || round(RATIO_TO_REPORT("201111") over (partition by 지역)*100) ||'%)' as "201112"
     , t1."201210" || '(' || round(RATIO_TO_REPORT("201111") over (partition by 지역)*100) ||'%)' as "201210"
     , t1."201211" || '(' || round(RATIO_TO_REPORT("201111") over (partition by 지역)*100) ||'%)' as "201211"
     , t1."201212" || '(' || round(RATIO_TO_REPORT("201111") over (partition by 지역)*100) ||'%)' as "201212"
     , t1."201310" || '(' || round(RATIO_TO_REPORT("201111") over (partition by 지역)*100) ||'%)' as "201310"
     , t1."201311" || '(' || round(RATIO_TO_REPORT("201111") over (partition by 지역)*100) ||'%)' as "201311"
from
    (
                select REGION 지역
                     , GUBUN  대출종류
                     , SUM(DECODE(TO_CHAR(PERIOD), 201111, LOAN_JAN_AMT, 0)) "201111"
                     , SUM(DECODE(TO_CHAR(PERIOD), 201112, LOAN_JAN_AMT, 0)) "201112"
                     , SUM(DECODE(TO_CHAR(PERIOD), 201210, LOAN_JAN_AMT, 0)) "201210"
                     , SUM(DECODE(TO_CHAR(PERIOD), 201211, LOAN_JAN_AMT, 0)) "201211"
                     , SUM(DECODE(TO_CHAR(PERIOD), 201212, LOAN_JAN_AMT, 0)) "201212"
                     , SUM(DECODE(TO_CHAR(PERIOD), 201310, LOAN_JAN_AMT, 0)) "201310"
                     , SUM(DECODE(TO_CHAR(PERIOD), 201311, LOAN_JAN_AMT, 0)) "201311"
                from KOR_LOAN_STATUS
                GROUP BY REGION
                     , GUBUN
               
    )t1
    order by 1, 2 desc;
    
```


## 💯SQL 마지막 간단한 이론 정리 

```sql
----------  계정 생성

CREATE USER 생성유저명 IDENTIFIED BY 비밀번호 ;


----------  계정 권한

◆ DBA의 시스템 권한

 CREATE USER	 사용자를 생성할 수 있는 권한
 DROP USER	 사용자를 삭제할 수 있는 권한
 DROP ANY TABLE	 모든 테이블을 삭제할 수 있는 권한
 QUERY REWRITE	 함수기반인덱스를 생성하기 위한 권한
 BACKUP ANY TABLE	 Export 유틸리티를 사용해서 임의의 테이블을 백업할 수 있는 권한


◆ 일반 사용자의 시스템 권한

 CREATE SESSION	 데이터베이스에 접속할 수 있는 권한
 CREATE TABLE	 테이블을 생성할 수 있는 권한
 CREATE SEQUENCE	 시퀀스를 생성할 수 있는 권한
 CREATE VIEW	 뷰를 생성할 수 있는 권한
 CREATE PROCEDURE	 프로시저, 함수, 패키지를 생성할 수 있는 권한


◆ 롤(Roll)
다수 사용자와 다양한 권한을 효과적으로 관리하기 위하여 서로 관련된 권한을 그룹화한 개념이다.
즉, 사용자별로 일일이 권한을 주기보다는 그룹에 권한을 주는 것이 훨씬 효과적이기 때문에 권한을 그룹화해서 관리하는 것이다.
롤은 쉽게 생각해서 권한들을 다발로 묶어놓은 것으로 생각하면 된다. 필요시 얼마든지 다양한 롤을 정의할 수 있다.

● CONNECT 롤
사용자가 데이터베이스에 접속하여 세션을 생성하거나 테이블 또는 뷰와 같은 객체를 생성할 수 있는 권한을 그룹화한 롤.
● RESOURCE 롤
사용자에게 자신의 테이블, 시퀀스, 프로시저, 트리거와 같은 객체를 생성할 수 있는 권한을 부여한 롤.
● DBA 롤
시스템 자원의 무제한적인 사용이나 시스템 관리에 필요한 모든 권한 그리고 DBA권한을 다른 사용자에게 부여할 수 있는 강력한 권한을 보유한 롤이다.
또한 모든 사용자의 CONNECT, RESOURCE, DBA권한을 포함한 어떠한 권한을 부여하거나 철회할 수 있다.





CONNECT - ALTER SESSION, CREATE SESSION, CREATE DATABASE LINK, CREATE SEQUENCE, CREATE SESSION, CREATE SYNONYM, CREATE SESSION, CREATE VIEW
RESOURCE - CREATE CLUSTER/INDEXTYPE/OPERATOR/PROCEDURE/SEQUENCE/TABLE/

◆ 권한 할당
grant 권한 to 계정명;

◆ 권한 해제
revoke 권한 from 계정명;

---------- DBA 권한 빼고 모든권한주기

GRANT CONNECT, RESOURCE TO 계정명 ;


----------------------------------- 데이터베이스 객체
테이블(table)   : 데이터를 담고 있는 객체
           DBMS 상에서 가장 기본적인 객체로 로우(행)컬럼(열)으로 구성된 2 차원 형태 (표)의 객체.
뷰(view)       : 하나 이상의 테이블을 연결해 마치 테이블인 것처럼 사용하는 객체
                 하나 이상의 테이블이나 다른 뷰의 데이터를 볼 수 있게 하는 데이터베이스 객체
                 실제 데이터는 뷰를 구성하는 테이블에 담겨 있지만 마치 테이블 처럼 사용 할 수 있다.
                 테이블 뿐만 아니라 다른 뷰를 참조해 새로운 뷰를 만들어 사용할 수 있다.
생성            : CREATE OR REPLACE 뷰이름 AS
                 SELECT 컬럼명2, 컬럼명3
                 FROM 테이블 ;
인덱스(index)   : 테이블에 있는 데이터를 빨리 찾기 위한 용도의 데이터베이스 객체.

시노님(synonim)   : 데이터베이스 객체에 대한 별칭을 부여한 객체
          '동의어' 란 뜻으로 데이터베이스 객체는 각자 고유한 이름이 있는데 , 이 객체들에 대한 동의어를 만드는 것
          PUBLIC 과 PRIVATE 시노님이 있다.
          
시퀀스(sequence)   : 일련번호 채번을 할 때 사용되는 객체
                    sequence(시퀀스) 는 테이블과 독립적이므로 여러 곳에서 사용가능
                    pk를 설정할 후보키가 없거나, 특별히 의미 있게 만들지 않아도 되는 경우 또는
                    자동으로 순서적인 번호가 필요한 경우 사용
          
함수(function)     :  특정연산을 하고 값을 반환하는 객체로
                     프로시저와 동일하게 절차형 SQL을 활용
                     일련의 연산 처리 결과를 단일값으로 반환
                     DBMS에서 내장함수처럼 사용자 정의함수의 호출을 통해 실행
                     종료 시 단일값을 반환한다는 것과 클라이언트에서 실행되는 것이 프로시저와 가장 큰 차이점.
                     

프로시저(procedure) : 업무적으로 복잡한 구문을 별도의 구문을 작성하여 DB에 저장하고 실행가능
                     고유한 기능을 수행하는 함수와 유사하지만 함수와 다르게 리턴값이 없게 할 수 있으며
                     서버에서 실행된다.
                     반환값이 없을 수 있기 때문에 SELECT 절에서 사용 할 수 없다.
            
패키지(pakage)     : 용도에맞게 함수나 프로시저를 하나로 묶어 놓은 객체

--------------------------------------------------------------------------------------------

트리거(TRIGGER) 의 개념 : 특정 테이블에 삽입, 수정, 삭제 등의 데이터 변경 이벤트가 발생하면
                        DBMS에서 자동적으로 실행되도록 구현된 프로그램을 트리거라고 한다.
                        - 전체 행/ 개별 행으로 구분하여 작업 할 수 있다.


--------------------------------------------------------------------------------------------------------------------------------
예외(Exception)  : 런타임 에러(=Exception)  :
      PL/SQL 블럭이 실행되는 동안 발생하는 에러로 일반적으로 런타임에러를 "Exception"이라 부른다.
    예외에는 크게 두가지로 시스템 예외 오라클에서 발생시키는 것과 사용자 정의 예외로 구분할 수 있다.            


트랜잭션(Transaction) :  ‘거래’ 라는 뜻으로 은행에서 사용하는 입금과 출금을 하는 거래를 말한다.
                                        사용자, 오라클 서버, 애플리케이션 개발자, DBA 등에게 데이터
                                        일치성과 데이터 동시발생을 보장하기 위해 트랜잭션 처리를 한다.
                                        업무적으로 가장 작은 단위로 구분하여
                                        해당 업무가 성공하면 COMMIT을 통하여 DB 에 적용하며
                                        하나라도 실패하면 ROLLBACK을 통하여 모든 작업이력을 제거한다.

--------------------------------------------------------------------------------------------------------------------------------


** DML은 ( Data Manipulation Language ) 데이터 조작어

INSERT/UPDATE/DELETE/SELECT

* DDL은 ( Data Definition Language ) 데이터 정의어

CREATE/ALTER/DROP/RENAME/TRUNCATE

DCL은 ( Data Control Languae ) 데이터 제의어

GRANT/REVOKE/




--------------  SQL

-- DDL

            -- CREATE
            CREATE TABLE EX_M (
                MEM_ID     VARCHAR2(20)
               ,MEM_NAME   VARCHAR2(100)
               ,MEM_JOB    VARCHAR2(100)   
               ,MEM_MILEAGE   NUMBER
            );

            -- DROP
            DROP TABLE EX_M ;
            DROP TABLE EX_M CASCADE CONSTRAINTS;

            -- ALTER
            ALTER TABLE EX_M RENAME COLUMN MEM_ID TO MEM_IDD;
            ALTER TABLE EX_M MODIFY MEM_IDD VARCHAR2(30);
            ALTER TABLE EX_M ADD MEM_IDD2 NUMBER;
            ALTER TABLE EX_M DROP COLUMN MEM_IDD2 ;

-- TRUNCATE
TRUNCATE TABLE EX_M;



-- DML -------------
-- SELECT

SELECT *
FROM CART;

SELECT *
FROM CART
WHERE CART_QTY  = 5;

SELECT *
FROM CART
ORDER BY CART_QTY DESC;


-- INSERT

INSERT INTO EX_M VALUES ('leeapgil001','이앞길' ,'SW개발자', 1000000 );
INSERT INTO EX_M (MEM_ID, MEM_MILEAGE)VALUES ('leeapgil002', 1000000 );

-- UPDATE

UPDATE EX_M
SET MEM_MILEAGE = 500000000
WHERE MEM_ID = 'leeapgil001';

-- DELETE

DELETE EX_M
WHERE MEM_ID = 'leeapgil002';

DELETE EX_M;

------ 그룹바이

SELECT CART.CART_MEMBER
     , AVG(PROD.PROD_PRICE)
FROM CART, PROD
WHERE CART.CART_PROD = PROD.PROD_ID
GROUP BY CART_MEMBER;


------ 분석함수

SELECT CART_MEMBER
     , CART_PROD
     , ROW_NUMBER() OVER (PARTITION BY CART_MEMBER ORDER BY CART_PROD )  AS cart_rows        -- 그불별로 로우에 대한 순번반환(단순순번)
FROM CART;
```
## 20210209 SQL 문제  
```sql
----------------------------------------------------------------
--문제 1

CREATE TABLESPACE TS_STUDY
DATAFILE '/u01/app/oracle/oradata/XE/ts_study.dbf' SIZE 100M
AUTOEXTEND ON NEXT 5M;


----------------------------------------------------------------
--문제 2
create user java2 IDENTIFIED BY oracle
default TABLESPACE TS_STUDY
TEMPORARY TABLESPACE temp;


----------------------------------------------------------------
--문제 3
GRANT CONNECT, RESOURCE to java2;


----------------------------------------------------------------
--문제 4

create table EX_MEM(
    MEM_ID          VARCHAR2(10)    NOT NULL
,   MEM_NAME        VARCHAR2(20)    NOT NULL
,   MEM_JOB         VARCHAR2(30)
,   MEM_MILEAGE     NUMBER(8,2)     default 0
,   MEM_REG_DATE    DATE            default sysdate
,   CONSTRAINT PK_EX_MEM PRIMARY KEY (MEM_ID)
);



COMMENT ON TABLE EX_MEM IS '임시회원테이블' ;       
     
COMMENT ON COLUMN EX_MEM.MEM_ID IS '아이디';

COMMENT ON COLUMN EX_MEM.MEM_NAME IS '회원명';

COMMENT ON COLUMN EX_MEM.MEM_JOB IS '직업';

COMMENT ON COLUMN EX_MEM.MEM_MILEAGE IS '마일리지';

COMMENT ON COLUMN EX_MEM.MEM_REG_DATE IS '등록일';




----------------------------------------------------------------
--문제 5
ALTER TABLE EX_MEM MODIFY MEM_NAME VARCHAR2(50);

----------------------------------------------------------------
--문제 6

CREATE SEQUENCE SEQ_CODE
INCREMENT BY    1
START WITH      1000
MINVALUE        1
MAXVALUE        9999  
CYCLE         
NOCACHE   ;    


SELECT SEQ_CODE.NEXTVAL  
FROM DUAL ;

----------------------------------------------------------------
--문제 7
insert into EX_MEM (MEM_ID, MEM_NAME, MEM_JOB, MEM_MILEAGE, MEM_REG_DATE)
values('hong', '홍길동', '주부', '', sysdate);


----------------------------------------------------------------
--문제 8
INSERT INTO EX_MEM(MEM_ID, MEM_NAME, MEM_JOB, MEM_MILEAGE, MEM_REG_DATE)
SELECT MEM_ID, MEM_NAME, MEM_JOB, MEM_MILEAGE, sysdate  FROM member
where MEM_LIKE = '독서'  
or MEM_LIKE = '등산'
or MEM_LIKE = '바둑';

----------------------------------------------------------------
--문제 9

DELETE EX_MEM
WHERE MEM_NAME like '김%';

----------------------------------------------------------------
--문제 10

select MEM_ID
     , MEM_NAME
     , MEM_JOB
     , MEM_MILEAGE
from member
where mem_job = '주부'
and MEM_MILEAGE >= 1000
and MEM_MILEAGE <= 3000
order by 4 desc;

----------------------------------------------------------------
--문제 11

select prod_id, prod_name, prod_sale
from prod
WHERE prod_sale = 23000
or prod_sale = 26000
or prod_sale = 33000;

----------------------------------------------------------------
--문제 12

select MEM_JOB
     , count(*)                    MEM_CNT
     , max(MEM_MILEAGE)            MEX_MLG
     , trunc(avg(MEM_MILEAGE),0)   AVG_MLG
from member
group by MEM_JOB
having count(*) >= 3 ;


----------------------------------------------------------------
--문제 13

select m.MEM_ID
     , m.MEM_NAME
     , m.MEM_JOB
     , c.CART_PROD
     , c.CART_QTY
from member m
   , cart c
where m.MEM_ID = c.cart_member
and CART_NO like '20050728%';

----------------------------------------------------------------
--문제 14

select m.MEM_ID
     , m.MEM_NAME
     , m.MEM_JOB
     , c.CART_PROD
     , c.CART_QTY
from member m
inner join cart c
on(m.MEM_ID = c.cart_member)
where CART_NO like '20050728%';

----------------------------------------------------------------
--문제 15

select MEM_ID
     , MEM_NAME
     , MEM_JOB
     , MEM_MILEAGE
     , DENSE_RANK() OVER (partition by MEM_JOB ORDER BY MEM_MILEAGE desc) MEM_RANK
from member ;

----------------------------------------------------------------



```


## 📚 9장 정규 표현식
### 1. 정규식이란?
 - 정규표현식(正規表現式, Regular Expression)은 문자열을 처리하는 방법 중의 하나로 특정한 조건의 문자를 '검색'하거나 '치환'하는 과정을 매우 간편하게 처리 할 수 있도록 하는 수단이다.
- ORACLE 정규식 (Reqular Expression) Oracle 10g에서는 REGEXP_로 시작하는 함수를 지원합니다.
   - 강력한 Text 분석 도구로써 LIKE 의 한계를 극복
   - 유닉스의 정규식과 같음.
   - Pattern-Matching-Rule

 - 함수와 표현식 패턴 (Regular Expression Pattern)을 이용하여 원하는 값을 얻는다.
 - 표현식 패턴에 사용하는 패턴을 만들기 위한 기호

 ### 2. 정규식 기본 Syntax
   * 정규식은 언제 사용 ?  ETL/전환/이행, Data Mining, Data Cleansing, 데이터 검증 ....
   * 중복, 공백, 데이터 검증, 패턴 체크 ...

   - .(dot) 은 모든 문자와 match된다.


 #### 정규식 함수
 - REGEXP_LIKE    : like 연산과 유사하며 정규식 패턴을 검색
 - REGEXP_REPLACE : 정규식 패턴을 검색하여 대체 문자열로 변경
 - REGEXP_INSTR   : 정규식 패턴을 검색하여 위치를 반환
 - REGEXP_SUBSTR  : 정규식 패턴을 검색하여 부분 문자 추출
 - REGEXP_COUNT   : 정규식 패턴을 검색하여 발견된 횟수 반환

------------------------------------------------------------------------------------------
1. REGEXP_LIKE(srcstr, pattern [,match_option])

- LIKE 연산자와 유사하며, 표현식 패턴 (Regular Expression Pattern)을 수행하여, 일치하는 값을 반환
 
   - srcstr : 소스 문자열, 검색하고자 하는 값.
   - pattern : Regular Expression Operator를 통해 문자열에서 특정 문자를 보다 다양한 pattern으로 검색하는 것이 가능.
   - match_option : match를 시도할 때의 옵션.

```sql
*/
[1-1]
.(dot) 은 모든 문자와 match 된다. 단 엔터(new line)은 예외임.

.< 1자리 아무거나
SELECT MEM_NAME
     , MEM_HOMETEL
FROM MEMBER
WHERE REGEXP_LIKE(MEM_HOMETEL, '...-...-....');



regex 예약 문자 표현
\. 점을 표현(. 만있으면 모든 문자중 하나라는 표현이므로 점만 표현하기 위해 \ 사용)
\\:역슬레시
^ $ . * + ? = ! : | \ / () [] {}
그 밖에 위의 문자들은 정규 표현식에 사용하는 문자이므로
일반 문자를 사용하려면 역슬래시(\) 를 앞에 두어야 합니다.


SELECT EMPLOYEE_ID
     , PHONE_NUMBER
FROM EMPLOYEES
WHERE REGEXP_LIKE(PHONE_NUMBER, '...\....\.....');

[1-2]
[]MACHING SET
대괄호의 경우 개별 리터널 문자를 둘러싸서 문자클래스로 문자열을 매치합니다.
대괄호의 경우 개별 리터널 문자를 둘러싸서 문자클래스로 문자열을 매치합니다.
안에 '-' 는 범위 , '^'는 제외 (NOT) 괄호 밖에서 '^'시작을 의미

[a-z]: 영어 소문자
[A-Z]: 영어 대문자
[0-9] :모든 숫자
[a-zA-Z0-9]모든 영어 문자와 숫자
[^abc] a,b,c 를 제외한 모든 문자
[가-힝] : 한글

SELECT MEM_NAME
     , MEM_HOMETEL
FROM MEMBER
WHERE REGEXP_LIKE(MEM_HOMETEL, '[0-9][0-9][0-9]-[0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]');


SELECT MEM_NAME
     , MEM_HOMETEL
FROM MEMBER
WHERE REGEXP_LIKE(MEM_HOMETEL, '[0][2]-[0-9][0-9][0-9]-[0-9][0-9][0-9][0-9]');



SELECT mem_hometel
FROM member
WHERE REGEXP_LIKE(mem_hometel, '^[0-9]{2}-');

SELECT mem_mail
FROM member
WHERE REGEXP_LIKE(mem_mail, '^[a-z]{3}@');


SELECT *
FROM member
WHERE REGEXP_LIKE(mem_pass, '7$');


{m} 정확히 m 회 매치
{m,} 최소한 m회 매치
{m, n} 최소 m 회 최대 n 회 매치
? : 0 또는 1번 (={0,1}) ? : 0 또는 1번 (={0,1})
+ : 1번 이상 (={1,})1회 또는 그이상 횟수로 매치
* : 0번 이상 (={0,}) 0회 또는 그 이상 횟수로 매치


SELECT MEM_NAME
     , MEM_HOMETEL
FROM MEMBER
WHERE REGEXP_LIKE(MEM_HOMETEL, '[0-9]+{2}[0-9]{3}-[0-9]{4}');

SELECT MEM_NAME
     , MEM_HOMETEL
FROM MEMBER
WHERE REGEXP_LIKE(MEM_HOMETEL, '^[0-9]{2}-[0-9]{3}-[0-9]{4}');


MEM_ADD2 주소 컬럼의 데이터가
숫자 공백 숫자 형태 또는 한글 공백 숫자 형태인 데이터를 출력하시오
-----------------------------------------------------------------------
select MEM_ADD2
from member
where REGEXP_LIKE(MEM_ADD2, '([0-9][ ][0-9]|)|()[가-힝][ ][0-9]');

-----------------------------------------------------------------------
--한글이 한 글자도 포함되어있지 않은 경우  
select MEM_ADD2
from member
where not REGEXP_LIKE(MEM_ADD2, '[가-힝]');
-----------------------------------------------------------------------
select MEM_ADD2
from member
where REGEXP_LIKE(MEM_ADD2, '^[가-힝]');

-----------------------------------------------------------------------


() 는 GROUPING EXPRESSION 을 의미
WITH T1 AS (
    SELECT '(02)456-7708' AS PNUM
    FROM DUAL
    UNION ALL
    SELECT '(042)456-7708' AS PNUM
    FROM DUAL
    UNION ALL
    SELECT '010-456-7708' AS PNUM
    FROM DUAL
)
SELECT *
FROM T1
WHERE REGEXP_LIKE(PNUM,'(\([0-9]{2}\)|\([0-9]{3}\))');


--숫자가 아닌 문자열을 찾자.

 WITH V_TEST AS (
  SELECT 'ABCD1234' AS C1 FROM DUAL
  UNION ALL
  SELECT '1234ABCD' AS C1 FROM DUAL
  UNION ALL
  SELECT '12345678' AS C1 FROM DUAL
)
SELECT C1
  FROM V_TEST
 WHERE REGEXP_LIKE(C1, '[^[:digit:]]');
 

-- 하나라도 숫자가 아닌것이 있으면 선택, REGEXP_LIKE(ID, ‘[^[:digit:]]’)
WITH BASE AS (
    SELECT 'aA11'   AS ID FROM DUAL UNION ALL   
    SELECT '1qw211e' AS ID FROM DUAL UNION ALL   
    SELECT '1qwe#'  AS ID FROM DUAL UNION ALL   
    SELECT 'e*'     AS ID FROM DUAL UNION ALL   
    SELECT '@!'     AS ID FROM DUAL UNION ALL   
    SELECT '123'    AS ID FROM DUAL)
SELECT ID FROM BASE
WHERE REGEXP_LIKE(ID, '[^[:digit:]]');

--하나라도 숫자를 포함하는 문자열, REGEXP_LIKE(ID, ‘[[:digit:]]’)
WITH BASE AS (
    SELECT 'aA11'   AS ID FROM DUAL UNION ALL   
    SELECT '1qw211e' AS ID FROM DUAL UNION ALL   
    SELECT '1qwe#'  AS ID FROM DUAL UNION ALL   
    SELECT 'e*'     AS ID FROM DUAL UNION ALL   
    SELECT '@!'     AS ID FROM DUAL UNION ALL   
    SELECT '123'    AS ID FROM DUAL)
SELECT ID FROM BASE
 WHERE REGEXP_LIKE(ID, '[[:digit:]]');


-- 제품 이름에 'SS' 다음 'P'나 'S'를 포함하는 문자열을 찾자.

WITH V_TEST AS (
  SELECT 'SSA' AS C1 FROM DUAL UNION ALL
  SELECT 'SSP' AS C1 FROM DUAL UNION ALL
  SELECT 'ASSS' AS C1 FROM DUAL
)
SELECT C1
  FROM V_TEST
-- WHERE REGEXP_LIKE(C1, 'SS[^PS]'); -- SS 다음 P나 S를 포함하는 것
 WHERE REGEXP_LIKE(C1, 'SS[^P]'); -- SS 다음 P 를 포함하지 않는 것

SELECT  MEM_NAME
FROM   MEMBER
WHERE  REGEXP_LIKE (MEM_NAME, '이[쁜진]');




REGEXP_LIKE(emp_name, '^J')       ---> like 'J%'  
REGEXP_LIKE(emp_name, '^J.(N|M)') ---> J 로 시작하며, 세번째 문자가 M 또는 N 인 데이터를 찾는것
REGEXP_LIKE(emp_name, '^ab*')     ---> ab 로 시작하는것 검색
REGEXP_LIKE(emp_name, '^The')     ---> The 로 시작하는 문자열
REGEXP_LIKE(emp_name, 'ab*')      ---> a 다음에 b 가 0개 이상등 다수
REGEXP_LIKE(emp_name, 'notice')   ---> notice 가 들어있는 문자열

REGEXP_LIKE(zip, '[^[:digit:]]')
             zip  
             ab123
             123xy
             007ab
             abcxy

5 개의 숫자로 구성된 우편번호 패턴의 시작부분 반환   --->  [[:digit:]]{5}

 

where regexp_like(prof_no,'^[^LC || ^PW]')

     --> prof_no 중에서 LC로 시작 하거나 PW로 시작하는것을 제외한 것들을 출력


CREATE TABLE EX12_5 (
    TESTNO NUMBER
   ,TEXT VARCHAR2(100)
)

INSERT INTO EX12_5 VALUES ( 1, 'ABC123') ;
INSERT INTO EX12_5 VALUES ( 2, 'ABC 123') ;
INSERT INTO EX12_5 VALUES ( 3, 'abc 123') ;
INSERT INTO EX12_5 VALUES ( 4, 'a1b2c3') ;
INSERT INTO EX12_5 VALUES ( 5, 'aabbcc123') ;
INSERT INTO EX12_5 VALUES ( 6, '?/!@#$%^&*') ;
INSERT INTO EX12_5 VALUES ( 7, '|~*().') ;
INSERT INTO EX12_5 VALUES ( 8, '123123') ;
INSERT INTO EX12_5 VALUES ( 9, '123abc') ;
INSERT INTO EX12_5 VALUES ( 10, 'ABCD123');
INSERT INTO EX12_5 VALUES ( 11, 'ABCD1234');



SELECT * FROM EX12_5 WHERE REGEXP_LIKE(txt, '[a-z][0-9]');
--[a-z], [0-9]는 소문자 전체와 0부터9까지의 숫자를 나타낸다.

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '[a-z] [0-9]');
--[a-z]와[0-9]사이에 공백이 있는 것이 보이시죠? 이렇게 공백도 구분값으로 사용할 수 있다.

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '[a-z]?[0-9]');
SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '[a-z]*[0-9]');
--[a-z]?[0-9]와 [a-z]*[0-9] 이 뜻은 공백이 여러개 포함한다는 뜻

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '*[0-9]');
--숫자를포함한 모든 문자를 select 합니다.

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '*[a-z]');
--소문자 영어 를 포함한 모든 문자를 select 합니다.

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '[A-Z]{3}');
--대문자 영어가 연속으로 3자리 있는 행을 출력합니다.

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '[0-9]{3}');
--숫자가 연속으로 3자리 있는 행을 출력합니다.

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '[A-Z][0-9]{3}');
--영어 대문자, 숫자 대분자 모두 3개 이상

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '^[0-9]');
--숫자로 시작되는 행

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '[a-z]$');
--소문자로끝나는행

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '^[^0-9]');
--숫자로시작하지 않는 행

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, 'A|1');
--‘A’나 1을 포함하고 있는행

SELECT * FROM EX12_5 WHERE REGEXP_LIKE(text, '\?');
--'?' 문자가 들어가는행
-- 한글만
WITH BASE AS (
    SELECT 'aA11'   AS ID FROM DUAL UNION ALL
    SELECT '1qw211e' AS ID FROM DUAL UNION ALL
    SELECT '1qwe#'  AS ID FROM DUAL UNION ALL
    SELECT 'e*'     AS ID FROM DUAL UNION ALL
    SELECT '@!'     AS ID FROM DUAL UNION ALL
    SELECT '123'    AS ID FROM DUAL UNION ALL
    SELECT 'aa최a'    AS ID FROM DUAL UNION ALL
    SELECT '유신'    AS ID FROM DUAL
)
SELECT ID
  FROM BASE
WHERE REGEXP_LIKE(id, '[가-힝]');


-----------------------------------------------------------------
(1)
-- member 테이블의 mem_add2 데이터서 -숫자2개로 끝나는
(2)
-- member 테이블의 mem_add2 데이터서 -한글이 없는 데이터만 조회하시오

select mem_add2
from member
WHERE REGEXP_LIKE(mem_add2, '-[0-9]{2}$');

select mem_add2
from member
WHERE not REGEXP_LIKE(mem_add2, '^-[가-힝]');
```



2. REGEXP_SUBSTR(srcstr, pattern, [,position[,occurrence[,match_option]]]) 

- SUBSTR 함수의 기능을 확장함. 주어진 문자열을 대상으로 정규 표현식 패턴을 수행하여, 일치하는 하위 문자열을 반환한다.

- 문법 : REGEXP_SUBSTR(srcstr, pattern, [,position[,occurrence[,match_option]]])
  - srcstr : 소스 문자열
  - position : Oracle이 문자열에서 특정 문자를 어디에서 찾아야 하는지 위치를 나타냄. - 기본으로 1로 설정되어 있으므로, 문자열의 처음부터 검색을 시작.
  - occurrence : 검색하고자 하는 문자열에서 특정 문자의 발생 횟수. 기본으로 1로 설정되어 있으며, 이는 Oracle이 문자열에서 첫번째 발생 pattern을 찾는다는 의미.
  - match_option : match를 시도할 때의 옵션 option ( i : 대소문자 구분하지 않음 , c 대소문자 구분, ... 기본은 구분함.
```sql



*/


SELECT REGEXP_SUBSTR(MEM_MAIL, '[^@]+', 1, 1) AS "ID"
     , REGEXP_SUBSTR(MEM_MAIL, '[^@]+', 1, 2) AS "MailAddr"
     , MEM_MAIL
FROM MEMBER;




SELECT MEM_NAME, MEM_MAIL, REGEXP_SUBSTR(MEM_MAIL, '[^@]+') AS SUBEMAIL
FROM MEMBER;

/*
사용방법
REGEXP_SUBSTR(대상 문자, 패턴, 시작 위치(최소값1),매칭순번)
REGEXP_SUBSTR('C-01-02','[^-]+',1,1)
 결과 = C
REGEXP_SUBSTR('C-01-02','[^-]+',1,2)
 결과 = 01
REGEXP_SUBSTR('C-01-02','[^-]+',1,3)
 결과 = 02




앞의 대상문자와 패턴에 의해 나누어진 문자들을 몇번째 INDEX에서 시작하여 몇번째의 나누어진 문자를 가져올것인지에 대한 PARAMETER
위 예제에서 (1,2)는 C // 01 // 02 의 나누어진 문자중 1번째 INDEX부터 시작하는 2번째 문자를 가져오라는 뜻


  대괄호 [] 안의 ^ 는  NOT의 의미를 나타냄

^ 문자가 대괄호 밖에서 사용되면 문자열의 시작을 의미함
+ 는 문자패턴이 1개이상 연결될 때를 나타냄, 위 예제에서 01,02등 2개이상 나타내기 위함
+ 시작위치 & 매칭 순번

*/

SPACE 첫번째 단어 출력
SELECT REGEXP_SUBSTR(MEM_ADD1, '[^ ]*')
FROM MEMBER;




SELECT REGEXP_SUBSTR('아빠_엄마_형님_동생', '[^_]+', 1,3) from dual ;



SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[0-9]')
FROM DUAL;
-- 1
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[^0-9]')
FROM DUAL;
-- L
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[^0-9]+')
FROM DUAL;
-- LEE gildong ok
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', ' [^0-9]+')
FROM DUAL;
-- gildong ok
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[^0-9].+')
FROM DUAL;
--LEE gildong ok 12345
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[[:digit:]]')
FROM DUAL;
--1
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[[:digit:]]+')
FROM DUAL;
--12345
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[[:digit:]]..')
FROM DUAL;
--123
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[[:alpha:]]')
FROM DUAL;
--L
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', ' [[:alpha:]]')
FROM DUAL;
-- g
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', ' [[:alpha:]]{3}')
FROM DUAL;
-- gil
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[^ ]+',1,1)
FROM DUAL;
--LEE
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[^ ]+',1,2)
FROM DUAL;
--gildong
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[^ ]+',1,3)
FROM DUAL;
--ok
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[^ ]+',1,4)
FROM DUAL;
--12345
SELECT REGEXP_SUBSTR('LEE gildong ok 12345', '[^[:digit:]]+')
FROM DUAL;
--LEE gildong ok

------------------------------------------
```

3. REGEXP_REPLACE 

- 주어진 문자열을 대상으로 정규 표현식 패턴을 조사하여, 다른 문자로 대체한다.
- 문법 : REGEXP_REPLACE(srcstr, pattern [,replacestr[,position[,occurrence[,match_option]]]])
  - replacestr : 대체하고자 하는 문자열을 나타낸다.
 
```sql


Ellen Hildi Smith라는 이름을 SPACE 1칸으로 구분해서 각각
Smith, Ellen Hildi로 변환하라

SELECT REGEXP_REPLACE(
'Ellen Hildi Smith',
'(.*) (.*) (.*)', '\3, \1 \2') TEXT
FROM dual;

[:cntrl:] (출력되지 않는) 컨트롤 문자
[:punct:] 구두점 기호
[:space:] 출력되지 않는 공백 문자(carriage return, newline, vertical tab, form feed) 등
[:alnum:] 알파벳/숫자
[:digit:] 숫자
[:upper:] 대문자 알파벳 문자
[:lower:] 소문자 알파벳 문자
[:alpha:] 알파벳 문자
[:print:] 출력 가능한 문자

------------------------------------------------
SELECT REGEXP_REPLACE(
'Joe      Smith','( ){2,}',' ') AS RX_REPLACE -- 띄어쓰기가 두 번 이상인 것 찾아서 한 번으로 바꿔줌
FROM dual;

SELECT REPLACE(
'Joe      Smith','  ',' ') AS RX_REPLACE -- 띄어쓰기가 정확히 두 번인 것을 찾아서 한 번으로 바꿔줌
FROM dual;
-------------------------------------------------

WITH V_TEST AS (
  SELECT '7907051234567' SSN FROM dual
)
SELECT REGEXP_REPLACE(SSN, '[0-9]', '*', 7) AS "SSN"
FROM V_TEST;


--둘 이상의 공백 문자를 하나로 대체하여 가독성을 높이자.

SELECT REGEXP_REPLACE('Oracle is the   Information Company', '( ){2,}', ' ') AS "Result"
FROM dual;


--전화번호의 표현 방식을 3자리, 3자리, 4자리로 묶어 식별력을 높이자.

SELECT REGEXP_REPLACE('555.123.4567','([[:digit:]]{3})\.([[:digit:]]{3})\.([[:digit:]]{4})','(\1) \2 - \3') AS "Result1"
  FROM dual;


---------------member 테이블에서 MEM_ADD1 id 'p001' 제외하고 대전이 포함되어 있는 주소를 다음과 같이 추출하시오
select REGEXP_REPLACE(MEM_ADD1, '.*시', '대전')
from member
where MEM_ADD1 like '대전%'
and MEM_ID != 'p001';


select REGEXP_REPLACE(MEM_ADD1, '대전광역시|대전시', '대전')
from member
where MEM_ADD1 like '대전%'
and MEM_ID != 'p001';
-------------------------------------------------
```

4. REGEXP_INSTR

- 정규 표현을 만족하는 부분의 위치를 반환한다.

- 문법 
REGEXP_INSTR(srcstr, pattern [,position[,occurrence[,returnparam[,match_option]]]])
  - position : 검색 시작 위치
  - occurrence : 발생 횟수.
  - returnparam : 반환 옵션.
  - match_option : match를 시도할 때의 옵션.

```sql
SELECT REGEXP_INSTR('Regular Expression', 'a') AS "REGEXP_INSTR"
 FROM dual;


/**

 한정자 : 앞의 문자(그룹, 식) 제한  
   * : 0개 이상
   + : 1개 이상
   ? : 0,1 개
   {n} : n번
   {n,} : n번 이상
   {n,m} : n번 이상  m번이하

  한정자 : Greedy : 기본적을 패턴에 일치하는 최대를 찾죠 , 이것을 최소한으로 찾도록
  한정자  다음에 ? 사용시 (Non Greedy )

   *? , +?  , {n,}?
  
  앵커
   ^  : 시작할때  , $ : 끝날때  , \b : 단어의 경계   
  
  문자
   . : 임의 한문자(\n 제외)  
   [abc]
   [a-c]
   [^a-c]

   펄표기법
    \w , \W = [a-zA-Z0-9_]
    \d , \D = [0-9]
    \s , \S = [ \t\r\f\v]   
   
   | : 또는
   () : 그룹, 처음부터 1..9개 까지     
   역참조  
   <td>  aaaaaa   </td>
   <li>  bbbb     </aaa>
   <tr> ....   </tr>
   -- be 문자가 있는데 .... be 전에 문자하고 , be 다음의 문자하고 같은것 찾기 ???
   abea  abec    abex  cbex cbec  zbea  zbez   
    
*/

-- 사원명에  'ae' 또는 'un' 있는

select employee_id, emp_name, salary, phone_number
  from employees
 where regexp_like(emp_name, 'ae|un' );  


-- 위치
select employee_id, emp_name, salary, phone_number
     , regexp_instr(emp_name, 'an|en')
from employees;

-- 전화번호에서 번호가 동일한 번호가 나오는 사원조회?
--  xxx.xxxx.8080 , 7979
select employee_id, emp_name, salary, phone_number
  from employees
 where regexp_like(phone_number,'(\d\d)\1$');


---------------------------------------------
--REGEXP_COUNT
-- (source, 표현식, 시작위치, Matching Modifiers )

SELECT REGEXP_COUNT(
‘http://scidb.tistory.com’,
‘.', 1, 'i') AS "Count TEST"
FROM dual ;
```
