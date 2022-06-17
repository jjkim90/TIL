# 저장 서브프로그램



## 1. 저장 서브프로그램



### 저장 서브프로그램이란?

- 작성한 내용을 단 한 번 실행하는 블록을 이름이 정해져 있지 않은 PL/SQL 블록이라는 의미로 익명 블록(anonymous block)이라고 부름.
- 여러 번 사용할 목적으로 이름을 지정하여 오라클에 저장해 두는 PL/SQL 프로그램을 저장 서브프로그램(stored subprogram)이라고 함.
  - 오라클에 저장함.
  - 저장할 때 한 번 컴파일.
  - 공유하여 사용 가능.
  - 다른 응용 프로그램에서 호출 가능.
- 대표적인 구현 방식은 프로시저, 함수, 패키지, 트리거임.



## 2. 프로시저



### 파라미터를 사용하지 않는 프로시저

#### 프로시저 생성하기

- 작업 수행에 별다른 입력 데이터가 필요하지 않을 경우에 파라미터를 사용하지 않는 프로시저를 사용함.

```plsql
-- 프로시저 생성하기
CREATE OR REPLACE PROCEDURE pro_noparam
IS
   V_EMPNO NUMBER(4) := 7788;
   V_ENAME VARCHAR2(10);
BEGIN
   V_ENAME := 'SCOTT';
   DBMS_OUTPUT.PUT_LINE('V_EMPNO : ' || V_EMPNO);
   DBMS_OUTPUT.PUT_LINE('V_ENAME : ' || V_ENAME);
END;
/
```



#### SQL*PLUS로 프로시저 실행하기

```PLSQL
-- 생성한 프로시저 실행하기
SET SERVEROUTPUT ON;

EXECUTE pro_noparam;
```



#### PL/SQL 블록에서 프로시저 실행하기

```PLSQL
-- 익명 블록에서 프로시저 실행하기
BEGIN
   pro_noparam;
END;
/
```



#### 프로시저 내용 확인하기

- USER_SOURCE 데이터 사전에서 조회
  - NAME / TYPE / LINE / TEXT

```PLSQL
-- USER_SOURCE를 통해 프로시저 확인하기(토드)
SELECT *
  FROM USER_SOURCE
 WHERE NAME = 'PRO_NOPARAM';
```

```PLSQL
-- USER_SOURCE를 통해 프로시저 확인하기(SQL*PLUS)
SELECT TEXT
  FROM USER_SOURCE
 WHERE NAME = 'PRO_NOPARAM';
```



#### 프로시저 삭제하기

```PLSQL
-- 프로시저 삭제하기
DROP PROCEDURE PRO_NOPARAM;
```

- 이미 저장되어 있는 프로시저의 소드 코드를 변경할 때에는 CREATE OR REPLACE PROCEDURE 명령어로 프로시저를 재생성.
- ALTER PROCEDURE는 프로시저의 소스 코드 내용을 재컴파일하는 명령어이므로 작성한 코드 내용을 변경하지 않음.



#### 파라미터를 사용하는 프로시저

- 파라미터 모드
  - IN : 지정하지 않으면 기본값으로 프로시저를 호출할 때 값을 입력받음.
  - OUT : 호출할 때 값을 반환함.
  - IN OUT : 호출할 때 값을 입력받은 후 실행 결과 값을 반환함.



##### IN 모드 파라미터

- 프로시저 실행에 필요한 값을 직접 입력받는 형식의 파라미터를 지정할 때 사용함.
- 기본값이기 때문에 생략 가능함.

```PLSQL
-- 프로시저에 파라미터 지정하기
CREATE OR REPLACE PROCEDURE pro_param_in
(
   param1 IN NUMBER,
   param2 NUMBER,
   param3 NUMBER := 3,
   param4 NUMBER DEFAULT 4
)
IS

BEGIN
   DBMS_OUTPUT.PUT_LINE('param1 : ' || param1);
   DBMS_OUTPUT.PUT_LINE('param2 : ' || param2);
   DBMS_OUTPUT.PUT_LINE('param3 : ' || param3);
   DBMS_OUTPUT.PUT_LINE('param4 : ' || param4);
END;
/
```

- param3, param4는 기본값이 지정되어 있는 상태이므로 호출할 때 값을 지정하지 않아도 실행이 가능함.

```plsql
-- 기본값이 지정된 파라미터 입력을 제외하고 프로시저 사용하기
EXECUTE pro_param_in(1, 2);
```

- 기본값이 지정되지 않은 파라미터 수보다 적은 수의 값을 지정하면 프로시저 실행은 실패함.
- 기본값이 지정된 파라미터와 지정되지 않은 파라미터의 순서가 섞여 있다면 기본값이 지정되지 않은 파라미터까지 값을 입력해 주어야 함.
- 파라미터 종류나 개수가 많아지면 =>를 사용하여 파라미터 이름에 직접 값을 대입할 수 있음.

```plsql
-- 파라미터 이름을 활용하여 프로시저에 값 입력하기
EXECUTE pro_param_in(param1 => 10, param2 => 20);
```



##### OUT 모드 파라미터

- 프로시저 실행 후 호출한 프로그램으로 값을 반환함.

```PLSQL
-- OUT 모드 파라미터 정의하기
CREATE OR REPLACE PROCEDURE pro_param_out
(
   in_empno IN EMP.EMPNO%TYPE,
   out_ename OUT EMP.ENAME%TYPE,
   out_sal OUT EMP.SAL%TYPE
)
IS

BEGIN
   SELECT ENAME, SAL INTO out_ename, out_sal
     FROM EMP
    WHERE EMPNO = in_empno;
END pro_param_out;
/
```

```PLSQL
-- OUT 모드 파라미터 사용하기
DECLARE
   v_ename EMP.ENAME%TYPE;
   v_sal EMP.SAL%TYPE;
BEGIN
   pro_param_out(7788, v_ename, v_sal);
   DBMS_OUTPUT.PUT_LINE('ENAME : ' || v_ename);
   DBMS_OUTPUT.PUT_LINE('SAL : ' || v_sal);
END;
/
```



##### IN OUT 모드 파라미터

```PLSQL
-- IN OUT 모드 파라미터 정의하기
CREATE OR REPLACE PROCEDURE pro_param_inout
(
   inout_no IN OUT NUMBER
)
IS

BEGIN
   inout_no := inout_no * 2;
END pro_param_inout;
/
```

```PLSQL
-- IN OUT 모드 파라미터 사용하기
DECLARE
   no NUMBER;
BEGIN
   no := 5;
   pro_param_inout(no);
   DBMS_OUTPUT.PUT_LINE('no : ' || no);
END;
/
```



### 프로시저 오류 정보 확인하기

- 서브프로그램을 만들 때 발생한 오류는 SHOW ERRORS 명령어와 USER_ERRORS 데이터 사전을 조회하여 확인할 수 있음.



#### SHOW ERRORS로 오류 확인

- 가장 최근에 생성되거나 변경된 서브프로그램의 오류 정보를 출력함.
- 줄여서 SHOW ERR로 사용할 수도 있음.



#### USER_ERRORS로 오류 확인

```PLSQL
-- USER_ERRORS로 오류 확인하기
SELECT *
  FROM USER_ERRORS
 WHERE NAME = 'PRO_ERR';
```



## 3. 함수

- 프로시저와 함수의 차이점
  - 함수는 SQL문에서 직접 실행이 가능.
  - 함수는 파리미터 지정할 때 IN 모드(또는 생략)만 사용 가능.
  - 반드시 하나의 값을 반환해야 하며 값의 반환은 RETURN절과 RETURN문을 통해 반환.



### 함수 생성하기

- CREATE [OR REPLACE] 명령어와 FUNCTION 키워드를 명시하여 생성.
- 함수에 OUT, IN OUT 모드의 파라미터가 지정되어 있으면 SQL문에서는 사용할 수 없는 함수가 되어 프로시저와 차이가 없어짐. 오라클에서도 함수 파라미터로 OUT, IN OUT 모드 사용하지 말라고 권장함.

```PLSQL
-- 함수 생성하기
CREATE OR REPLACE FUNCTION func_aftertax(
   sal IN NUMBER
)
RETURN NUMBER
IS
   tax NUMBER := 0.05;
BEGIN
   RETURN (ROUND(sal - (sal * tax)));
END func_aftertax;
/
```



### 함수 실행하기

#### PL/SQL로 함수 실행하기

```plsql
-- PL/SQL에서 함수 사용하기
DECLARE
   aftertax NUMBER;
BEGIN
   aftertax := func_aftertax(3000);
   DBMS_OUTPUT.PUT_LINE('after-tax income : ' || aftertax);
END;
/
```



#### SQL문에서 함수 실행하기

```PLSQL
-- SQL문에서 함수 사용하기
SELECT func_aftertax(3000)
  FROM DUAL;
```



### 함수 삭제하기

- DROP FUNCTION 명령어 사용.

```PLSQL
-- 함수 삭제하기
DROP FUNCTION func_aftertax;
```



## 4. 패키지

- 패키지(package)는 업무나 기능 면에서 연관성이 높은 프로시저, 함수 등 여러 개의 PL/SQL 서브프로그램을 하나의 논리 그룹으로 묶어 통합, 관리하는 데 사용하는 객체를 뜻함.
- 패키지 사용하여 서브프로그램 그룹화의 장점
  - 모듈성. 모듈성은 잘 묶어 둔다는 뜻으로 프로그램의 이해를 쉽게 하고 패키지 사이의 상호 작용을 더 간편하고 명료하게 해 주는 역할을 함.
  - 쉬운 응용 프로그램 설계. 전체 소스 코드를 다 작성하기 전에 미리 패키지에 저장할 서브프로그램을 지정할 수 있어 설계가 수월해짐.
  - 정보 은닉. 패키지에 포함하는 서브프로그램의 외부 노출 여부 또는 접근 여부를 지정할 수 있어 보안을 강화할 수 있음.
  - 기능성 향상. 패키지 내부에는 서브프로그램 외에 변수, 커서, 예외 등도 선언해서 공용으로 사용할 수 있음.
  - 성능 향상. 패키지 메모리에 한 번에 로딩되는데 로딩 후 호출은 디스크 I/O를 일으키지 않아 성능이 향상됨.



### 패키지 구조와 생성

#### 패키지 명세

- 패키지 명세는 패키지에 포함할 변수, 상수, 예외, 커서 그리고 PL/SQL 서브프로그램을 선언하는 용도로 작성함.
- 패키지 명세에 선언한 여러 객체는 패키지 내부뿐만 아니라 외부에서도 참조할 수 있음.

```PLSQL
-- 패키지 생성하기
CREATE OR REPLACE PACKAGE pkg_example
IS
   spec_no NUMBER := 10;
   FUNCTION func_aftertax(sal NUMBER) RETURN NUMBER;
   PROCEDURE pro_emp(in_empno IN EMP.EMPNO%TYPE);
   PROCEDURE pro_dept(in_deptno IN DEPT.DEPTNO%TYPE);
END;
/
```

```PLSQL
-- 패키지 명세 확인하기
SELECT TEXT
  FROM USER_SOURCE
 WHERE TYPE = 'PACKAGE'
   AND NAME = 'PKG_EXAMPLE';
```



#### 패키지 본문

- 패키지 명세에서 선언한 서브프로그램 코드를 작성함.
- 패키지 명세에 선언하지 않은 객체나 서브프로그램을 정의하는 것도 가능함.
- 패키지 본문에만 존재하는 프로그램은 패키지 내부에서만 사용할 수 있음.
- 패키지 본문 이름은 패키지 명세 이름과 같게 지정해야 함.

```PLSQL
-- 패키지 본문 생성하기
CREATE OR REPLACE PACKAGE BODY pkg_example
IS
   body_no NUMBER := 10;

   FUNCTION func_aftertax(sal NUMBER) RETURN NUMBER
      IS
         tax NUMBER := 0.05;
      BEGIN
         RETURN (ROUND(sal - (sal * tax)));
   END func_aftertax;

   PROCEDURE pro_emp(in_empno IN EMP.EMPNO%TYPE)
      IS
         out_ename EMP.ENAME%TYPE;
         out_sal EMP.SAL%TYPE;
      BEGIN
         SELECT ENAME, SAL INTO out_ename, out_sal
           FROM EMP
          WHERE EMPNO = in_empno;

         DBMS_OUTPUT.PUT_LINE('ENAME : ' || out_ename);
         DBMS_OUTPUT.PUT_LINE('SAL : ' || out_sal);
   END pro_emp;

PROCEDURE pro_dept(in_deptno IN DEPT.DEPTNO%TYPE)
   IS
      out_dname DEPT.DNAME%TYPE;
      out_loc DEPT.LOC%TYPE;
   BEGIN
      SELECT DNAME, LOC INTO out_dname, out_loc
        FROM DEPT
       WHERE DEPTNO = in_deptno;

      DBMS_OUTPUT.PUT_LINE('DNAME : ' || out_dname);
      DBMS_OUTPUT.PUT_LINE('LOC : ' || out_loc);
   END pro_dept;
END;
/
```



#### 서브프로그램 오버로드

- 같은 패키지에서 사용하는 파라미터의 개수, 자료형, 순서가 다를 경우에 한해서만 이름이 같은 서브프로그램을 정의할 수 있음. 이를 서브프로그램 오버로딩이라고 함.
- 같은 기능을 수행하는 여러 서브프로그램이 입력 데이터를 각각 다르게 정의할 때 사용함.

```PLSQL
-- 프로시저 오버로드하기
CREATE OR REPLACE PACKAGE pkg_overload
IS
   PROCEDURE pro_emp(in_empno IN EMP.EMPNO%TYPE);
   PROCEDURE pro_emp(in_ename IN EMP.ENAME%TYPE);
END;
/
```

```PLSQL
-- 패키지 본문에서 오버로드된 프로시저 작성하기
CREATE OR REPLACE PACKAGE BODY pkg_overload
IS
   PROCEDURE pro_emp(in_empno IN EMP.EMPNO%TYPE)
      IS
         out_ename EMP.ENAME%TYPE;
         out_sal EMP.SAL%TYPE;
      BEGIN
         SELECT ENAME, SAL INTO out_ename, out_sal
           FROM EMP
          WHERE EMPNO = in_empno;

         DBMS_OUTPUT.PUT_LINE('ENAME : ' || out_ename);
         DBMS_OUTPUT.PUT_LINE('SAL : ' || out_sal);
      END pro_emp;

   PROCEDURE pro_emp(in_ename IN EMP.ENAME%TYPE)
      IS
         out_ename EMP.ENAME%TYPE;
         out_sal EMP.SAL%TYPE;
      BEGIN
         SELECT ENAME, SAL INTO out_ename, out_sal
           FROM EMP
          WHERE ENAME = in_ename;

         DBMS_OUTPUT.PUT_LINE('ENAME : ' || out_ename);
         DBMS_OUTPUT.PUT_LINE('SAL : ' || out_sal);
      END pro_emp;

END;
/
```



### 패키지 사용하기

```PLSQL
-- 패키지에 포함된 서브프로그램 실행하기
BEGIN
   DBMS_OUTPUT.PUT_LINE('--pkg_example.func_aftertax(3000)--');
   DBMS_OUTPUT.PUT_LINE('after-tax:' || pkg_example.func_aftertax(3000));

   DBMS_OUTPUT.PUT_LINE('--pkg_example.pro_emp(7788)--');
   pkg_example.pro_emp(7788);

   DBMS_OUTPUT.PUT_LINE('--pkg_example.pro_dept(10)--' );
   pkg_example.pro_dept(10);

   DBMS_OUTPUT.PUT_LINE('--pkg_overload.pro_emp(7788)--' );
   pkg_overload.pro_emp(7788);

   DBMS_OUTPUT.PUT_LINE('--pkg_overload.pro_emp(''SCOTT'')--' );
   pkg_overload.pro_emp('SCOTT');
END;
/
```



### 패키지 삭제하기

- 패키지 명세와 본문을 한 번에 삭제하거나 패키지 본문만 삭제할 수 있음.
- 패키지에 포함된 서브프로그램을 따로 삭제하는 것은 불가능함.
- CREATE OR REPLACE문을 활용하여 패키지 안의 객체 또는 서브프로그램을 수정 및 삭제할 수 있음.



## 5. 트리거





