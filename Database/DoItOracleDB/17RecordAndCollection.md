# 레코드와 컬렉션



## 1. 자료형이 다른 여러 데이터를 저장하는 레코드



### 레코드란?

- 레코드(record)는 자료형이 각기 다른 데이터를 하나의 변수에 저장하는 데 사용함.

```PLSQL
-- 레코드 기본 형식
TYPE 레코드이름 IS RECORD(
변수이름 자료형 NOT NULL :=[OR DEFAULT] 값또는값이도출되는여러표현식
)
```

- NOT NULL 생략 가능함.

```plsql
-- 레코드 정의해서 사용하기
DECLARE
   TYPE REC_DEPT IS RECORD(
      deptno NUMBER(2) NOT NULL := 99,
      dname DEPT.DNAME%TYPE,
      loc DEPT.LOC%TYPE
   );
   dept_rec REC_DEPT;
BEGIN
   dept_rec.deptno := 99;
   dept_rec.dname := 'DATABASE';
   dept_rec.loc := 'SEOUL';
   DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || dept_rec.deptno);
   DBMS_OUTPUT.PUT_LINE('DNAME : ' || dept_rec.dname);
   DBMS_OUTPUT.PUT_LINE('LOC : ' || dept_rec.loc);
END;
/
```



### 레코드를 사용한 INSERT

- PL/SQL문에서는 INSERT, UPDATE문에도 레코드를 사용할 수 있음.

```PLSQL
-- 레코드를 사용하여 INSERT하기
DECLARE
   TYPE REC_DEPT IS RECORD(
      deptno NUMBER(2) NOT NULL := 99,
      dname DEPT.DNAME%TYPE,
      loc DEPT.LOC%TYPE
   );
   dept_rec REC_DEPT;
BEGIN
   dept_rec.deptno := 99;
   dept_rec.dname := 'DATABASE';
   dept_rec.loc := 'SEOUL';

   INSERT INTO DEPT_RECORD
   VALUES dept_rec;
END;
/
```

- INSERT문에 레코드를 사용하면 VALUES절에 레코드 이름만 명시해도 됨.



### 레코드를 사용한 UPDATE

```PLSQL
-- 레코드를 사용하여 UPDATE하기
DECLARE
   TYPE REC_DEPT IS RECORD(
      deptno NUMBER(2) NOT NULL := 99,
      dname DEPT.DNAME%TYPE,
      loc DEPT.LOC%TYPE
   );
   dept_rec REC_DEPT;
BEGIN
   dept_rec.deptno := 50;
   dept_rec.dname := 'DB';
   dept_rec.loc := 'SEOUL';

   UPDATE DEPT_RECORD
      SET ROW = dept_rec
    WHERE DEPTNO = 99;
END;
/
```

- SET절은 ROW 키워드와 함께 레코드 이름을 명시.



### 레코드를 포함하는 레코드

- 레코드 안에 또 다른 레코드를 포함한 형태를 중첩 레코드(nested record)라고 함.

```plsql
-- 레코드에 다른 레코드 포함하기
DECLARE
   TYPE REC_DEPT IS RECORD(
      deptno DEPT.DEPTNO%TYPE,
      dname DEPT.DNAME%TYPE,
      loc DEPT.LOC%TYPE
   );
   TYPE REC_EMP IS RECORD(
      empno EMP.EMPNO%TYPE,
      ename EMP.ENAME%TYPE,
      dinfo REC_DEPT
   );
   emp_rec REC_EMP;
BEGIN
   SELECT E.EMPNO, E.ENAME, D.DEPTNO, D.DNAME, D.LOC
     INTO emp_rec.empno, emp_rec.ename,
          emp_rec.dinfo.deptno,
          emp_rec.dinfo.dname,
          emp_rec.dinfo.loc
     FROM EMP E, DEPT D
    WHERE E.DEPTNO = D.DEPTNO
      AND E.EMPNO = 7788;

   DBMS_OUTPUT.PUT_LINE('EMPNO : ' || emp_rec.empno);
   DBMS_OUTPUT.PUT_LINE('ENAME : ' || emp_rec.ename);
   DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || emp_rec.dinfo.deptno);
   DBMS_OUTPUT.PUT_LINE('DNAME : ' || emp_rec.dinfo.dname);
   DBMS_OUTPUT.PUT_LINE('LOC : ' || emp_rec.dinfo.loc);
END;
/
```



## 2. 자료형이 같은 여러 데이터를 저장하는 컬렉션

- 컬렉션은 특정 자료형의 데이터를 여러 개 저장하는 복합 자료형.
- 컬렉션은 열 또는 테이블과 같은 형태로 사용할 수 있음.



### 연관 배열

- 연관 배열은 인덱스라고도 불리는 키(key), 값(value)으로 구성되는 컬렉션.
- 중복되지 않은 유일한 키를 통해 값을 저장하고 불러오는 방식을 사용함.

```PLSQL
-- 연관 배열 기본 형식
TYPE 연관배열이름 IS TABLE OF 자료형 [NOT NULL]
INDEX BY 인덱스형;
```

- 연관 배열은 레코드와 마찬가지로 특정 변수의 자료형으로서 사용할 수 있음.

```PLSQL
-- 연관 배열 사용하기
DECLARE
   TYPE ITAB_EX IS TABLE OF VARCHAR2(20)
      INDEX BY PLS_INTEGER;

   text_arr ITAB_EX;

BEGIN
   text_arr(1) := '1st data';
   text_arr(2) := '2nd data';
   text_arr(3) := '3rd data';
   text_arr(4) := '4th data';

   DBMS_OUTPUT.PUT_LINE('text_arr(1) : ' || text_arr(1));
   DBMS_OUTPUT.PUT_LINE('text_arr(2) : ' || text_arr(2));
   DBMS_OUTPUT.PUT_LINE('text_arr(3) : ' || text_arr(3));
   DBMS_OUTPUT.PUT_LINE('text_arr(4) : ' || text_arr(4));
END;
/
```



#### 레코드를 활용한 연관 배열

- 연관 배열의 자료형에는 레코드를 사용할 수 있음.
- 이 경우에 다양한 자료형을 포함한 레코드를 여러 개 사용할 수 있으므로 마치 테이블과 같은 데이터 사용과 저장이 가능함.

```PLSQL
-- 연관 배열 자료형에 레코드 사용하기
DECLARE
   TYPE REC_DEPT IS RECORD(
      deptno DEPT.DEPTNO%TYPE,
      dname DEPT.DNAME%TYPE
   );

   TYPE ITAB_DEPT IS TABLE OF REC_DEPT
      INDEX BY PLS_INTEGER;

   dept_arr ITAB_DEPT;
   idx PLS_INTEGER := 0;

BEGIN
   FOR i IN (SELECT DEPTNO, DNAME FROM DEPT) LOOP
      idx := idx + 1;
      dept_arr(idx).deptno := i.DEPTNO;
      dept_arr(idx).dname := i.DNAME;

      DBMS_OUTPUT.PUT_LINE(
         dept_arr(idx).deptno || ' : ' || dept_arr(idx).dname);
   END LOOP;
END;
/
```

- 만약 특정 테이블의 전체 열과 같은 구성을 가진 연관 배열을 제작한다면 %ROWTYPE을 사용하는 것이 레코드를 정의하는 것보다 간편함.

```PLSQL
-- %ROWTYPE으로 연관 배열 자료형 지정하기
DECLARE
   TYPE ITAB_DEPT IS TABLE OF DEPT%ROWTYPE
      INDEX BY PLS_INTEGER;

   dept_arr ITAB_DEPT;
   idx PLS_INTEGER := 0;

BEGIN
   FOR i IN(SELECT * FROM DEPT) LOOP
      idx := idx + 1;
      dept_arr(idx).deptno := i.DEPTNO;
      dept_arr(idx).dname := i.DNAME;
      dept_arr(idx).loc := i.LOC;

      DBMS_OUTPUT.PUT_LINE(
      dept_arr(idx).deptno || ' : ' ||
      dept_arr(idx).dname || ' : ' ||
      dept_arr(idx).loc);
   END LOOP;
END;
/
```



### 컬렉션 메서드

```PLSQL
-- 컬렉션 메서드 사용하기
DECLARE
   TYPE ITAB_EX IS TABLE OF VARCHAR2(20)
      INDEX BY PLS_INTEGER;

   text_arr ITAB_EX;

BEGIN
   text_arr(1) := '1st data';
   text_arr(2) := '2nd data';
   text_arr(3) := '3rd data';
   text_arr(50) := '50th data';

   DBMS_OUTPUT.PUT_LINE('text_arr.COUNT : ' || text_arr.COUNT);
   DBMS_OUTPUT.PUT_LINE('text_arr.FIRST : ' || text_arr.FIRST);
   DBMS_OUTPUT.PUT_LINE('text_arr.LAST : ' || text_arr.LAST);
   DBMS_OUTPUT.PUT_LINE('text_arr.PRIOR(50) : ' || text_arr.PRIOR(50));
   DBMS_OUTPUT.PUT_LINE('text_arr.NEXT(50) : ' || text_arr.NEXT(50));

END;
/
```

