# 커서와 예외 처리



## 1. 특정 열을 선택하여 처리하는 커서



### 커서란?

- 커서(cursor)는 SELECT문 또는 DML같은 SQL문을 실행했을 때 해당 SQL문을 처리하는 정보를 저장한 메모리 공간을 뜻함.
- 메모리 공간은 Private SQL Area라고 부르며 정확히 표현하자면 커서는 이 메모리의 포인터를 말함.
- 커서를 사용하면 실행된 SQL문의 결과 값을 사용할 수 있음.
- 예를 들어 SELECT문의 결과 값이 여러 행으로 나왔을 때 각 행별로 특정 작업을 수행하도록 기능을 구현하는 것이 가능함.



### SELECT INTO 방식

```PLSQL
-- SELECT INTO를 사용한 단일행 데이터 저장하기
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

- SELECT INTO문은 조회되는 데이터가 단 하나의 행일 때 사용 가능한 방식.
- 커서는 결과 행이 하나이든 여러 개이든 상관없이 사용할 수 있음.
- 데이터 조회의 결과 값은 하나인 경우보다 여러 개인 경우가 흔하며 결과 행이 하나일지 여러 개일지 알 수 없는 경우도 존재하므로 대부분 커서를 활용함.



### 명시적 커서

- 명시적 커서는 사용자가 직접 커서를 선언하고 사용하는 커서를 뜻함.
  - 1단계 커서 선언(declaration) : 사용자가 직접 이름을 지정하여 사용할 커서를 SQL문과 함께 선언.
  - 2단계 커서 열기(open) : 커서를 선언할 때 작성한 SQL문을 실행함.
  - 3단계 커서에서 읽어온 데이터 사용(fetch) : 실행된 SQL문의 결과 행 정보를 하나씩 읽어 와서 변수에 저장한 후 필요한 작업을 수행함.
  - 4단계 커서 닫기(close) : 모든 행의 사용이 끝나고 커서를 종료함.

```PLSQL
-- 명시적 커서 기본 형식
DECLARE
	CURSOR 커서이름 IS SQL문; -- 커서 선언
BEGIN
	OPEN 커서이름; -- 커서 열기
	FETCH 커서이름 INTO 변수 -- 커서로부터 읽어온 데이터 사용
	CLOSE 커서이름; -- 커서 닫기
END;
```

- 커서에 여러 행이 조회되는 SELECT문을 사용할 때 필요에 따라 다양한 LOOP문을 활용하여 각 행에 필요한 작업을 반복 수행할 수 있음.



#### 하나의 행만 조회되는 경우

- SELECT INTO문을 사용할 때보다 다소 번거로움.

```PLSQL
-- 단일행 데이터를 저장하는 커서 사용하기
DECLARE
   -- 커서 데이터를 입력할 변수 선언
   V_DEPT_ROW DEPT%ROWTYPE;

   -- 명시적 커서 선언(Declaration)
   CURSOR c1 IS
      SELECT DEPTNO, DNAME, LOC
        FROM DEPT
       WHERE DEPTNO = 40;

BEGIN
   -- 커서 열기(Open)
   OPEN c1;

   -- 커서로부터 읽어온 데이터 사용(Fetch)
   FETCH c1 INTO V_DEPT_ROW;

   DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || V_DEPT_ROW.DEPTNO);
   DBMS_OUTPUT.PUT_LINE('DNAME : ' || V_DEPT_ROW.DNAME);
   DBMS_OUTPUT.PUT_LINE('LOC : ' || V_DEPT_ROW.LOC);

   -- 커서 닫기(Close)
   CLOSE c1;

END;
/
```



#### 여러 행이 조회되는 경우 사용하는 LOOP문

```PLSQL
-- 여러 행의 데이터를 커서에 저장하여 사용하기(LOOP문 사용)
DECLARE
   -- 커서 데이터를 입력할 변수 선언
   V_DEPT_ROW DEPT%ROWTYPE;

   -- 명시적 커서 선언(Declaration)
   CURSOR c1 IS
      SELECT DEPTNO, DNAME, LOC
        FROM DEPT;

BEGIN
   -- 커서 열기(Open)
   OPEN c1;

   LOOP
      -- 커서로부터 읽어온 데이터 사용(Fetch)
      FETCH c1 INTO V_DEPT_ROW;

      -- 커서의 모든 행을 읽어오기 위해 %NOTFOUND 속성 지정
      EXIT WHEN c1%NOTFOUND;

      DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || V_DEPT_ROW.DEPTNO
                        || ', DNAME : ' || V_DEPT_ROW.DNAME
                        || ', LOC : ' || V_DEPT_ROW.LOC);
   END LOOP;

   -- 커서 닫기(Close)
   CLOSE c1;

END;
/
```

- %NOTFOUND는 실행된 FETCH문에서 행을 추출했으면 false, 추출하지 않았으면 true를 반환함.
- FETCH문을 통해 더 이상 추출한 데이터가 없을 경우에 LOOP 반복이 끝남.
- 커서이름%FOUND : 수행된 FETCH문을 통해 추출된 행이 있으면 true, 없으면 false 반환함.
- 커서이름%ROWCOUNT : 현재까지 추출된 행 수를 반환함.
- 커서이름%ISOPEN : 커서가 열려있으면 true, 닫혀있으면 false 반환함.



#### 여러 개의 행이 조회되는 경우 (FOR LOOP문)

- 커서에 FOR LOOP문을 사용하면 OPEN, FETCH, CLOSE문을 작성하지 않음. 사용 방법이 간단하다는 장점이 있음.

```PLSQL
-- FOR LOOP문을 활용하여 커서 사용하기
DECLARE
   -- 명시적 커서 선언(Declaration)
   CURSOR c1 IS
   SELECT DEPTNO, DNAME, LOC
     FROM DEPT;

BEGIN
   -- 커서 FOR LOOP 시작 (자동 Open, Fetch, Close)
   FOR c1_rec IN c1 LOOP
      DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || c1_rec.DEPTNO
                      || ', DNAME : ' || c1_rec.DNAME
                      || ', LOC : ' || c1_rec.LOC);
   END LOOP;

END;
/
```

- 커서 각 행을 c1_rec 루프 인덱스에 저장하므로 결과 행을 저장하는 변수 선언도 필요하지 않음.



#### 커서에 파라미터 사용하기

- 고정 값이 아닌 직접 입력한 값 또는 상황에 따라 여러 값을 번갈아 사용하려면 커서에 파라미터를 지정할 수 있음.

```plsql
-- 파라미터를 사용하는 커서 알아보기
DECLARE
   -- 커서 데이터를 입력할 변수 선언
   V_DEPT_ROW DEPT%ROWTYPE;
   -- 명시적 커서 선언(Declaration)
   CURSOR c1 (p_deptno DEPT.DEPTNO%TYPE) IS
      SELECT DEPTNO, DNAME, LOC
        FROM DEPT
       WHERE DEPTNO = p_deptno;
BEGIN
   -- 10번 부서 처리를 위해 커서 사용
   OPEN c1 (10);
      LOOP
         FETCH c1 INTO V_DEPT_ROW;
         EXIT WHEN c1%NOTFOUND;
         DBMS_OUTPUT.PUT_LINE('10번 부서 - DEPTNO : ' || V_DEPT_ROW.DEPTNO
                                     || ', DNAME : ' || V_DEPT_ROW.DNAME
                                     || ', LOC : ' || V_DEPT_ROW.LOC);
      END LOOP;
   CLOSE c1;
   -- 20번 부서 처리를 위해 커서 사용
   OPEN c1 (20);
      LOOP
         FETCH c1 INTO V_DEPT_ROW;
         EXIT WHEN c1%NOTFOUND;
         DBMS_OUTPUT.PUT_LINE('20번 부서 - DEPTNO : ' || V_DEPT_ROW.DEPTNO
                                     || ', DNAME : ' || V_DEPT_ROW.DNAME
                                     || ', LOC : ' || V_DEPT_ROW.LOC);
      END LOOP;
   CLOSE c1;
END;
/
```

```plsql
-- 커서에 사용할 파라미터 입력받기
DECLARE
   -- 사용자가 입력한 부서 번호를 저장하는 변수선언
   v_deptno DEPT.DEPTNO%TYPE;
   -- 명시적 커서 선언(Declaration)
   CURSOR c1 (p_deptno DEPT.DEPTNO%TYPE) IS
      SELECT DEPTNO, DNAME, LOC
        FROM DEPT
       WHERE DEPTNO = p_deptno;
BEGIN
   -- INPUT_DEPTNO에 부서 번호 입력받고 v_deptno에 대입
   v_deptno := &INPUT_DEPTNO;
   -- 커서 FOR LOOP 시작. c1 커서에 v_deptno를 대입
   FOR c1_rec IN c1(v_deptno) LOOP
      DBMS_OUTPUT.PUT_LINE('DEPTNO : ' || c1_rec.DEPTNO
                      || ', DNAME : ' || c1_rec.DNAME
                      || ', LOC : ' || c1_rec.LOC);
   END LOOP;
END;
/
```

- &INPUT_DEPTNO를 사용하면 실행할 때 사용자에게 INPUT_DEPTNO에 들어갈 값의 입력을 요구할 수 있음.



### 묵시적 커서

- 묵시적 커서는 별다른 선언 없이 SQL문을 사용했을 때 오라클에서 자동으로 선언되는 커서를 뜻함.
- 사용자가 OPEN, FETCH, CLOSE를 지정하지 않음.

```PLSQL
-- 묵시적 커서의 속성 사용하기
BEGIN
   UPDATE DEPT SET DNAME='DATABASE'
    WHERE DEPTNO = 50;

   DBMS_OUTPUT.PUT_LINE('갱신된 행의 수 : ' || SQL%ROWCOUNT);

   IF (SQL%FOUND) THEN
      DBMS_OUTPUT.PUT_LINE('갱신 대상 행 존재 여부 : true');
   ELSE
      DBMS_OUTPUT.PUT_LINE('갱신 대상 행 존재 여부 : false');
   END IF;

   IF (SQL%ISOPEN) THEN
      DBMS_OUTPUT.PUT_LINE('커서의 OPEN 여부 : true');
   ELSE
      DBMS_OUTPUT.PUT_LINE('커서의 OPEN 여부 : false');
   END IF;

END;
/
```

- 2행에서 UPDATE DML을 실행하므로 묵시적 커서가 자동으로 생성된 후 실행됨.
- SQL%ROWCOUNT 속성을 사용하여 DML 대상이 되는 행 수를 반환함.
- 조건문에 SQL%FOUND 속성을 대입하여 현재 갱신 대상이 존재하는지 검사함.
- 조건문에 SQL%ISOPEN 속성을 대입하여 생성된 커서가 OPEN되어 있는지 여부를 검사함.



## 2. 오류가 발생해도 프로그램이 비정상 종료되지 않도록 하는 예외 처리



### 오류란?

- 오라클에서 SQL 또는 PL/SQL이 정상 수행되지 못하는 상황을 오류(error)라고 함.
- 문법이 잘못되었거나 오타로 인한 오류는 컴파일 오류(compile Error), 문법 오류(syntax error)라고 함.
- 명령문의 실행 중 발생한 오류를 런타임 오류(runtime error), 실행 오류(execute error)라고 부름.
- 오라클에서는 후자, 즉 프로그램이 실행되는 도중 발생하는 오류를 예외(Exception)라고 함.

```PLSQL
-- 예외가 발생하는 PL/SQL
DECLARE
   v_wrong NUMBER;
BEGIN
   SELECT DNAME INTO v_wrong
     FROM DEPT
    WHERE DEPTNO = 10;
END;
/
```

- DNAME 열은 문자열(VARCHAR2) 데이터이므로, NUMBER 자료형인 v_wrong 변수에 대입할 수 없고 예외가 발생함.
- PL/SQL 실행 중 예외가 발생했을 때 프로그램이 비정상 종료되는 것을 막기 위해 특정 명령어를 PL/SQL문 안에 작성하는데 이를 '예외 처리'라고 함.
- 예외 처리는 PL/SQL문 안에서 EXCEPTION 영역에 필요 코드를 작성하는 것을 뜻함.

```PLSQL
-- 예외를 처리하는 PL/SQL(예외 처리 추가)
DECLARE
   v_wrong NUMBER;
BEGIN
   SELECT DNAME INTO v_wrong
     FROM DEPT
    WHERE DEPTNO = 10;
EXCEPTION
   WHEN VALUE_ERROR THEN
      DBMS_OUTPUT.PUT_LINE('예외 처리 : 수치 또는 값 오류 발생');
END;
/
```

- EXCEPTION 키워드 뒤에 예외 처리를 위해 작성한 코드 부분을 예외 처리부 또는 예외 처리절이라고 함.
- 예외 처리부가 실행되면 예외가 발생한 코드 이후의 내용은 실행되지 않음.

```PLSQL
-- 예외 발생 후의 코드 실행 여부 확인하기
DECLARE
   v_wrong NUMBER;
BEGIN
   SELECT DNAME INTO v_wrong
     FROM DEPT
    WHERE DEPTNO = 10;

   DBMS_OUTPUT.PUT_LINE('예외가 발생하면 다음 문장은 실행되지 않습니다');

EXCEPTION
   WHEN VALUE_ERROR THEN
      DBMS_OUTPUT.PUT_LINE('예외 처리 : 수치 또는 값 오류 발생');
END;
/
```



### 예외 종류

- 오라클에서 예외는 크게 내부 예외(internal exceptions)와 사용자 정의 예외(user-defined exceptions)로 나뉨.
- 내부 예외는 오라클에서 미리 정의한 예외를 뜻함.
- 사용자 정의 예외는 사용자가 필요에 따라 추가로 정의한 예외를 뜻함.
- 내부 예외는 다시 사전 정의된 예외(predefined name exceptions)와 이름이 없는 예외(unnamed exceptions)로 나뉨.

- 사전 정의된 예외는 비교적 자주 발생하는 예외에 이름을 붙여 놓은 것.
- 이름이 없는 예외는 예외 번호는 있지만 이름이 정해져 있지 않은 예외를 말함. 이름이 없는 예외는 예외 처리부에서 사용하기 위해 이름을 직접 붙여서 사용함.



### 예외 처리부 작성

- 여러 예외를 명시하여 작성할 수 있음.
- WHEN으로 시작하는 절을 예외 핸들러(exception handler)라고 하며, 발생한 예외 이름과 일치하는 WHEN절의 명령어를 수행함.
- IF-THEN문처럼 여러 예외 핸들러 중 일치하는 하나의 예외 핸들러 명령어만 수행함.
- OTHERS는 먼저 작성한 어느 예외와도 일치하는 예외가 없을 경우에 처리할 내용을 작성함.



#### 사전 정의된 예외 사용

```PLSQL
-- 사전 정의된 예외 사용하기
DECLARE
   v_wrong NUMBER;
BEGIN
   SELECT DNAME INTO v_wrong
     FROM DEPT
    WHERE DEPTNO = 10;

   DBMS_OUTPUT.PUT_LINE('예외가 발생하면 다음 문장은 실행되지 않습니다');

EXCEPTION
   WHEN TOO_MANY_ROWS THEN
      DBMS_OUTPUT.PUT_LINE('예외 처리 : 요구보다 많은 행 추출 오류 발생');
   WHEN VALUE_ERROR THEN
      DBMS_OUTPUT.PUT_LINE('예외 처리 : 수치 또는 값 오류 발생');
   WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('예외 처리 : 사전 정의 외 오류 발생');
END;
/
```



#### 이름 없는 예외 사용

- 선언부에서 오라클 예외 번호와 함께 이름을 붙임.

```PLSQL
-- 이름 없는 예외 사용하기
DECLARE
	예외이름1 EXCEPTION;
	PRAGMA EXCEPTION_INIT(예외이름1, 예외번호);
...
EXCEPTION
	WHEN 예외이름1 THEN
		...
END;
```



#### 사용자 정의 예외 사용

```PLSQL
-- 사용자 정의 예외 사용하기
DECLARE
	사용자예외이름1 EXCEPTION;
...
BEGIN
	IF 사용자예외발생시킬조건 THEN
		RAISE 사용자예외이름1
		...
	END IF;
EXCEPTION
	WHEN 사용자예외이름1 THEN
	...
END;
```



#### 오류 코드와 오류 메시지 사용

- SQLCODE : 오류 번호를 반환하는 함수
- SQLERRM : 오류 메시지를 반환하는 함수

```PLSQL
-- 오류 코드와 오류 메시지 사용하기
DECLARE
   v_wrong NUMBER;
BEGIN
   SELECT DNAME INTO v_wrong
     FROM DEPT
    WHERE DEPTNO = 10;

   DBMS_OUTPUT.PUT_LINE('예외가 발생하면 다음 문장은 실행되지 않습니다');

EXCEPTION
   WHEN OTHERS THEN
      DBMS_OUTPUT.PUT_LINE('예외 처리 : 사전 정의 외 오류 발생');
      DBMS_OUTPUT.PUT_LINE('SQLCODE : ' || TO_CHAR(SQLCODE));
      DBMS_OUTPUT.PUT_LINE('SQLERRM : ' || SQLERRM);
END;
/
```



