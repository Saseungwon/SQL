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