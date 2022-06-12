# PL/SQL 기초

- PL/SQL은 SQL만으로는 구현이 어렵거나 구현 불가능한 작업을 수행하기 위해 오라클에서 제공하는 프로그래밍 언어.
- 변수, 조건 처리, 반복 처리 등 다른 프로그래밍 언어에서도 제공하는 다양한 기능을 사용할 수 있음.



## 1. PL/SQL 구조



### 블록이란?

- PL/SQL은 데이터베이스 관련 특정 작업을 수행하는 명령어와 실행에 필요한 여러 요소를 정의하는 명령어 등으로 구성됨.
- 이러한 명령어를 모아 둔 PL/SQL 프로그램의 기본 단위를 블록(block)이라고 함.
  - DECLARE(선언부) : 실행에 사용될 변수, 상수, 커서 등을 선언(선택)
  - BEGIN(실행부) : 조건문, 반복문, SELECT, DML, 함수 등을 정의(필수)
  - EXCEPTION(예외처리부) : PL/SQL 실행 도중 발생하는 오류(예외 상황)를 해결하는 문장 기술(선택)

```plsql
-- PL/SQL 블록의 기본 형식
DECLARE
	[실행에 필요한 여러 요소 선언];
BEGIN
	[작업을 위해 실제 실행하는 명령어];
EXCEPTION
	[PL/SQL 수행 도중 발생하는 오류 처리];
END;
```

- 선언부와 예외 처리부는 생략 가능하지만 실행부는 반드시 존재해야 함.
- 필요에 따라 PL/SQL 블록 안에 다른 블록을 포함할 수도 있음. 이를 중첩 블록(nested block)이라고 함.



### Hello, PL/SQL 출력하기

```plsql
-- Hello PL/SQL 출력하기
SET SERVEROUTPUT ON; -- 실행 결과를 화면에 출력

BEGIN
   DBMS_OUTPUT.PUT_LINE('Hello, PL/SQL!');
END;
/
```

- PL/SQL 블록을 구성하는 DECLARE, BEGIN, EXCEPTION 키워드에는 세미콜론(;)을 사용하지 않음.
- PL/SQL 블록의 각 부분에서 실행해야 하는 문장 끝에는 세미콜론(;)을 사용함.
- PL/SQL문 내부에서 한 줄 주석(--)과 여러 줄 주석(/* ~ */)을 사용할 수 있음.
- PL/SQL문 작성을 마치고 실행하기 위해 마지막에 슬래시(/)를 사용함. 토드에서는 슬래시(/) 없이 실행 가능.



### PL/SQL 주석

```PLSQL
-- 한 줄 주석 사용하기
DECLARE
   V_EMPNO NUMBER(4) := 7788;
   V_ENAME VARCHAR2(10);
BEGIN
   V_ENAME := 'SCOTT';
   -- DBMS_OUTPUT.PUT_LINE('V_EMPNO : ' || V_EMPNO);
   DBMS_OUTPUT.PUT_LINE('V_ENAME : ' || V_ENAME);
END;
/
```

```plsql
-- 여러 줄 주석 사용하기
DECLARE
   V_EMPNO NUMBER(4) := 7788;
   V_ENAME VARCHAR2(10);
BEGIN
   V_ENAME := 'SCOTT';
/*
   DBMS_OUTPUT.PUT_LINE('V_EMPNO : ' || V_EMPNO);
   DBMS_OUTPUT.PUT_LINE('V_ENAME : ' || V_ENAME);
*/
END;
/
```



## 2. 변수와 상수



### 변수 선언과 값 대입하기

- 변수는 데이터를 일시적으로 저장하는 요소로 이름과 저장할 자료형을 지정하여 선언부(DECLARE)에서 작성함.
- 선언부에서 작성한 변수는 실행부(BEGIN)에서 활용함.



#### 기본 변수 선언과 사용

```PLSQL
-- 변수 선언 기본 형식
변수이름 자료형 := 값또는값이도출되는여러표현식;
```

```plsql
-- 변수 선언 및 변수 값 출력하기
DECLARE
   V_EMPNO NUMBER(4) := 7788;
   V_ENAME VARCHAR2(10);
BEGIN
   V_ENAME := 'SCOTT';
   DBMS_OUTPUT.PUT_LINE('V_EMPNO : ' || V_EMPNO);
   DBMS_OUTPUT.PUT_LINE('V_ENAME : ' || V_ENAME);
END;
/
```



#### 상수 정의하기

- 상수는 한 번 저장한 값이 프로그램이 종료될 때까지 유지되는 저장 요소임.

```PLSQL
-- 상수 선언 기본 형식
변수이름 CONSTANT 자료형 := 값또는값을도출하는여러표현식;
```

```PLSQL
-- 상수에 값을 대입한 후 출력하기
DECLARE
   V_TAX CONSTANT NUMBER(1) := 3;
BEGIN
   DBMS_OUTPUT.PUT_LINE('V_TEX : ' || V_TAX);
END;
/
```



#### 변수의 기본값 지정하기

- DEFAULT 키워드는 변수에 저장할 기본값을 지정함.

```PLSQL
-- 변수 기본값 지정 기본 형식
변수이름 자료형 DEFAULT 값또는값도출되는여러표현식;
```

```plsql
-- 변수에 기본값을 설정한 후 출력하기
DECLARE
   V_DEPTNO NUMBER(2) DEFAULT 10;
BEGIN
   DBMS_OUTPUT.PUT_LINE('V_DEPTNO : ' || V_DEPTNO);
END;
/
```



#### 변수에 NULL 값 저장 막기

- NOT NULL 키워드 사용함.

```PLSQL
-- NOT NULL 기본 형식
변수이름 자료형 NOT NULL :=또는DEFAULT 값또는값이도출되는여러표현식;
```

```
-- 변수에 NOT NULL을 설정하고 값을 대입한 후 출력하기
DECLARE
   V_DEPTNO NUMBER(2) NOT NULL := 10;
BEGIN
   DBMS_OUTPUT.PUT_LINE('V_DEPTNO : ' || V_DEPTNO);
END;
/
```



#### 변수 이름 정하기

- 같은 블록 안에서 식별자는 고유해야 하며 중복될 수 없음.
- 대소문자 구별 안함.
- 이름은 문자로 시작.
- 이름은 30Byte 이하. 한글은 15자까지 사용 가능.
- 영문자, 숫자, 특수문자($, #, _)를 사용할 수 있음.
- SQL 키워드는 테이블 이름으로 사용할 수 없음(SELECT, FROM 등).



### 변수의 자료형



#### 스칼라형

- 스칼라형(scalar type)은 숫자, 문자열, 날짜 등과 같이 오라클에서 기본으로 정의해 놓은 자료형으로 내부 구성 요소가 없는 단일 값을 의미함.
- 스칼라형은 숫자, 문자열, 날짜, 논리 데이터로 나뉨.



#### 참조형

- 참조형(reference type)은 오라클 데이터베이스에 존재하는 특정 테이블 열의 자료형이나 하나의 행 구조를 참조하는 자료형.
- 열을 참조할 때 %TYPE, 행을 참조할 때 %ROWTYPE을 사용함.

```PLSQL
-- 참조형(열)의 변수에 값을 대입한 후 출력하기
DECLARE
   V_DEPTNO DEPT.DEPTNO%TYPE := 50;
BEGIN
   DBMS_OUTPUT.PUT_LINE('V_DEPTNO : ' || V_DEPTNO);
END;
/
```

```PLSQL
-- 참조형(행)의 변수에 값을 대입한 후 출력하기
DECLARE
   V_DEPT_ROW DEPT%ROWTYPE;
BEGIN
   SELECT DEPTNO, DNAME, LOC INTO V_DEPT_ROW
     FROM DEPT
    WHERE DEPTNO = 40;
   DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || V_DEPT_ROW.DEPTNO);
   DBMS_OUTPUT.PUT_LINE('DNAME : ' || V_DEPT_ROW.DNAME);
   DBMS_OUTPUT.PUT_LINE('LOC : ' || V_DEPT_ROW.LOC);
END;
/
```



#### 복합형, LOB형

- 복합형은 여러 종류 및 개수의 데이터를 저장하기 위해 사용자가 직접 정의하는 자료형으로 컬렉션(collection), 레코드(record)로 구분됨.
- Large Object를 의미하는 LOB형은 대용량의 텍스트, 이미지, 동영상, 사운드 데이터 등 대용량 데이터를 저장하기 위한 자료형으로 대표적으로 BLOB, CLOB 등이 있음.



## 3. 조건 제어문



### IF 조건문

- IF-THEN : 특정 조건을 만족하는 경우 작업 수행
- IF-THEN-ELSE : 만족 + 반대 경우 각각 작업 수행
- IF-THEN-ELSIF : 여러 조건에 따라 각각 지정한 작업 수행



#### IF-THEN

```PLSQL
-- 변수에 입력한 값이 홀수인지 알아보기(입력 값이 홀수일 때)
DECLARE
   V_NUMBER NUMBER := 13;
BEGIN
   IF MOD(V_NUMBER, 2) = 1 THEN
      DBMS_OUTPUT.PUT_LINE('V_NUMBER는 홀수입니다!');
   END IF;
END;
/
```



#### IF-THEN-ELSE

```PLSQL
-- 변수에 입력된 값이 홀수인지 알아보기(입력 값이 짝수일 때)
DECLARE
   V_NUMBER NUMBER := 14;
BEGIN
   IF MOD(V_NUMBER, 2) = 1 THEN
      DBMS_OUTPUT.PUT_LINE('V_NUMBER는 홀수입니다!');
   ELSE
      DBMS_OUTPUT.PUT_LINE('V_NUMBER는 짝수입니다!');
   END IF;
END;
/
```



#### IF-THEN-ELSIF

```PLSQL
-- 입력한 점수가 어느 학점인지 출력하기(IF-THEN-ELSIF 사용)
DECLARE
   V_SCORE NUMBER := 87;
BEGIN
   IF V_SCORE >= 90 THEN
      DBMS_OUTPUT.PUT_LINE('A학점');
   ELSIF V_SCORE >= 80 THEN
      DBMS_OUTPUT.PUT_LINE('B학점');
   ELSIF V_SCORE >= 70 THEN
      DBMS_OUTPUT.PUT_LINE('C학점');
   ELSIF V_SCORE >= 60 THEN
      DBMS_OUTPUT.PUT_LINE('D학점');
   ELSE
      DBMS_OUTPUT.PUT_LINE('F학점');
   END IF;
END;
/
```



### CASE 조건문

- 단순 CASE문 : 비교 기준이 되는 조건의 값이 여러 가지일 때 해당 값만 명시하여 작업 수행
- 검색 CASE문 : 특정한 비교 기준 없이 여러 조건식을 나열하여 조건식에 맞는 작업 수행



#### 단순 CASE

```PLSQL
-- 입력 점수에 따른 학점 출력하기(단순 CASE 사용)
DECLARE
   V_SCORE NUMBER := 87;
BEGIN
   CASE TRUNC(V_SCORE/10)
      WHEN 10 THEN DBMS_OUTPUT.PUT_LINE('A학점');
      WHEN 9 THEN DBMS_OUTPUT.PUT_LINE('A학점');
      WHEN 8 THEN DBMS_OUTPUT.PUT_LINE('B학점');
      WHEN 7 THEN DBMS_OUTPUT.PUT_LINE('C학점');
      WHEN 6 THEN DBMS_OUTPUT.PUT_LINE('D학점');
      ELSE DBMS_OUTPUT.PUT_LINE('F학점');
   END CASE;
END;
/
```



#### 검색 CASE

```PLSQL
-- 입력 점수에 따른 학점 출력하기 (검색 CASE 사용)
DECLARE
   V_SCORE NUMBER := 87;
BEGIN
   CASE
      WHEN V_SCORE >= 90 THEN DBMS_OUTPUT.PUT_LINE('A학점');
      WHEN V_SCORE >= 80 THEN DBMS_OUTPUT.PUT_LINE('B학점');
      WHEN V_SCORE >= 70 THEN DBMS_OUTPUT.PUT_LINE('C학점');
      WHEN V_SCORE >= 60 THEN DBMS_OUTPUT.PUT_LINE('D학점');
      ELSE DBMS_OUTPUT.PUT_LINE('F학점');
   END CASE;
END;
/
```



## 4. 반복 제어문

- 기본 LOOP : 기본 반복문
- WHILE LOOP : 특정 조건식의 결과를 통해 반복 수행
- FOR LOOP : 반복 횟수를 정하여 반복 수행
- Cusor FOR LOOP : 커서를 활용한 반복 수행
- 반복 제어문에 사용되는 명령어
  - EXIT : 수행 중인 반복 종료
  - EXIT-WHEN : 반복 종료를 위한 조건식을 지정하고 만족하면 반복 종료
  - CONTINUE : 수행 중인 반복의 현재 주기를 건너뜀
  - CONTINUE-WHEN : 특정 조건을 지정하고 조건식을 만족하면 현재 반복 주기를 건너뜀



### 기본 LOOP

```PLSQL
-- 기본 LOOP 사용하기
DECLARE
   V_NUM NUMBER := 0;
BEGIN
   LOOP
      DBMS_OUTPUT.PUT_LINE('현재 V_NUM : ' || V_NUM);
      V_NUM := V_NUM + 1;
      EXIT WHEN V_NUM > 4;
   END LOOP;
END;
/
```

```PLSQL
-- 기본 LOOP EXIT-WHEN 대신 IF조건문 활용하기
DECLARE
   V_NUM NUMBER := 0;
BEGIN
   LOOP
      DBMS_OUTPUT.PUT_LINE('현재 V_NUM : ' || V_NUM);
      V_NUM := V_NUM + 1;
      IF V_NUM > 4 THEN
      EXIT;
      END IF;
   END LOOP;
END;
/
```



### WHILE LOOP

```PLSQL
-- WHILE LOOP 사용하기
DECLARE
   V_NUM NUMBER := 0;
BEGIN
   WHILE V_NUM < 4 LOOP
      DBMS_OUTPUT.PUT_LINE('현재 V_NUM : ' || V_NUM);
      V_NUM := V_NUM + 1;
   END LOOP;
END;
/
```



### FOR LOOP

```PLSQL
-- FOR LOOP 사용하기
BEGIN
   FOR i IN 0..4 LOOP
      DBMS_OUTPUT.PUT_LINE('현재 i의 값 : ' || i);
   END LOOP;
END;
/
```

```PLSQL
-- FOR LOOP REVERSE 사용하기
BEGIN
   FOR i IN REVERSE 0..4 LOOP
      DBMS_OUTPUT.PUT_LINE('현재 i의 값 : ' || i);
      END LOOP;
END;
/
```



### CONTINUE문, CONTINUE-WHEN문

- 오라클 11g부터 사용 가능.

```PLSQL
-- FOR LOOP 안에 CONTINUE문 사용하기
BEGIN
   FOR i IN 0..4 LOOP
      CONTINUE WHEN MOD(i, 2) = 1;
      DBMS_OUTPUT.PUT_LINE('현재 i의 값 : ' || i);
   END LOOP;
END;
/
```

