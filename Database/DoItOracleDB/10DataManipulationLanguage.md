# 데이터 조작어



## 1. 테이블에 데이터 추가하기



### 테이블 생성하기

- 특정 테이블에 데이터를 새로 추가할 때 INSERT문을 사용함.

```
// DEPT 테이블을 복사하여 DEPT_TEMP 테이블 만들기
CREATE TABLE DEPT_TEMP
    AS SELECT * FROM DEPT;
```

- CREATE문은 DDL(Data Definition Language) 명령어.



### INSERT문 실습 전 유의점

#### 테이블을 잘못 만들었을 때

```
//테이블을 지우는 명령어
DROP TABLE 테이블이름;
```



#### 실습하는 중에 프로그램이 종료되었을 때

- "Auto commit is idsabled. ~" 경고창 뜰 경우 Commit버튼 클릭하고 끝내기.



### 테이블에 데이터를 추가하는 INSERT문

```
// INSERT문 기본 형태
INSERT INTO 테이블이름 [(열1, 열2, ... , 열N)]
VALUES (열1에 들어갈 데이터, 열2에 들어갈 데이터, ... , 열N에 들어갈 데이터);
```

- INSERT INTO절 뒤에 데이터를 추가할 테이블 이름을 명시하고, 해당 테이블의 열을 소괄호로 묶어 지정한 후 VALUES절에는 지정한 열에 입력할 데이터를 작성.

```
// DEPT_TEMP 테이블에 데이터 추가하기
INSERT INTO DEPT_TEMP (DEPTNO, DNAME, LOC)
VALUES (50, 'DATABASE', 'SEOUL');
```



#### INSERT문 오류가 발생했을 때

- INSERT문에서 지정한 열 개수와 각 열에 입력할 데이터 개수가 일치하지 않거나 자료형이 맞지 않는 경우 또는 열 길이를 초과하는 데이터를 지정하는 경우 오류 발생.



#### INSERT문으로 데이터 입력하기(열 지정을 생략할 때)

- INSERT문에 지정하는 열은 생략할 수도 있음.
- 열 지정을 생략하면 해당 테이블을 만들 때 설정한 열 순서대로 모두 나열되어 있다고 가정하고 데이터 작성.

```
// INSERT문에 열 지정 없이 데이터 추가하기
INSERT INTO DEPT_TEMP
VALUES (60, 'NETWORK', 'BUSAN');
```



### 1.4. 테이블에 NULL 데이터 입력하기

- INSERT문으로 새로운 데이터를 추가할 때 특정 열에 들어갈 데이터가 확정되지 않았거나 굳이 넣을 필요가 없는 데이터인 경우에는 NULL 사용.
- NULL을 INSERT문에 지정하는 방법은 NULL을 직접 명시적으로 입력해주는 방법과 대상 열을 생략하여 암시적으로 NULL이 입력되도록 유도하는 방식이 있음.



#### NULL의 명시적 입력

```
// NULL을 지정하여 입력하기
INSERT INTO DEPT_TEMP (DEPTNO, DNAME, LOC)
VALUES (70, 'WEB', NULL);
```

- 해당 열의 자료형이 문자열 또는 날짜형일 경우 빈 공백 문자열을 사용해도 NULL 입력 가능.

```
// 빈 공백 문자열로 NULL 입력하기
INSERT INTO DEPT_TEMP (DEPTNO, DNAME, LOC)
VALUES (80, 'MOBILE', '');
```

- 실무에서는 명시적으로 NULL 입력하는 방식을 선호함.



#### NULL의 암시적 입력

- INSERT문에 NULL이 들어가야 할 열 이름을 아예 입력하지 않으면 NULL 암시적 입력됨.

```
// 열 데이터를 넣지 않는 방식으로 NULL 데이터 입력하기
INSERT INTO DEPT_TEMP (DEPTNO, LOC)
VALUES (90, 'INCHEON');
```



### 1.5. 테이블에 날짜 데이터 입력하기

- WHERE 1 <> 1 이용하면 데이터는 가져오지 않고 열 구조만 같은 테이블을 복사할 수 있음.

```
// EMP 테이블을 복사해서 EMP_TEMP 테이블 만들기 (필드만 가져오기)
CREATE TABLE EMP_TEMP
AS SELECT *
FROM EMP
WHERE 1 <> 1;
```

- 날짜 데이터는 YYYY/MM/DD 형식의 문자열 데이터로 입력.
- YYYY-MM-MM 형식으로도 입력 가능.

```
//INSERT문으로 날짜 데이터 입력하기(날짜 사이에 / 입력)
INSERT INTO EMP_TEMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (9999, '홍길동', 'PRESIDENT', NULL, '2001/01/01', 5000, 1000, 10);
```

```
//INSERT문으로 날짜 데이터 입력하기(날짜 사이에 - 입력)
INSERT INTO EMP_TEMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (1111, '성춘향', 'MANAGER', 9999, '2001-01-05', 4000, NULL, 20);
```



#### 날짜 데이터를 입력할 때 유의점

- 일/월/년 순서로 데이터를 입력하면 오류 발생.
  - 오라클이 설치되어 있는 OS의 종류나 사용하는 기본 언어군에 따라 날짜 표기방식이 다르기 때문.
  - 날짜 데이터를 INSERT문으로 입력할 때는 문자열로 입력하는 것보다 TO_DATE 함수 사용하는 것이 좋음.

```
// TO_DATE 함수를 사용하여 날짜 데이터 입력하기
INSERT INTO EMP_TEMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (2111, '이순신', 'MANAGER', 9999, TO_DATE('07/01/2001', 'DD/MM/YYYY'), 4000, NULL, 20);

```



#### SYSDATE를 사용하여 날짜 데이터 입력하기

- 현재 시점으로 날짜를 입력할 경우 SYSDATE를 지정하여 간단히 처리할 수 있음.
- SYSDATE 방식은 데이터 입력 시점을 정확히 입력할 수 있어 자주 사용함.
- 설정에 따라 오전/오후 시간이 함께 출력될 수 있음.

```
// SYSDATE를 사용하여 날짜 데이터 입력하기
INSERT INTO EMP_TEMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
VALUES (3111, '심청이', 'MANAGER', 9999, SYSDATE, 4000, NULL, 30);
```



### 1.6. 서브쿼리를 사용하여 한 번에 여러 데이터 추가하기

- INSERT문에 서브쿼리를 사용하여 SELECT문으로 한 번에 여러 행의 데이터를 추가할 수 있음.

```
// 서브쿼리로 여러 데이터 추가하기
INSERT INTO EMP_TEMP (EMPNO, ENAME, JOB, MGR, HIREDATE, SAL, COMM, DEPTNO)
SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE, E.SAL, E.COMM, E.DEOPNO
FROM EMP E, SALGRADE S
WHERE E.SAL BETWEEN S.LOSAL AND S.HISAL
AND S.GRADE = 1;
```

- INSERT문에서 서브쿼리 사용시 유의점
  - VALUES절은 사용하지 않음.
  - 데이터가 추가되는 테이블의 열 개수와 서브쿼리의 열 개수가 일치해야 함.
  - 데이터가 추가되는 테이블의 자료형과 서브쿼리의 자료형이 일치해야 함.



## 2. 테이블에 있는 데이터 수정하기

- 특정 테이블에 저장되어 있는 데이터 내용을 수정할 때 UPDATE문 사용.



### 2.1. UPDATE문의 기본 사용법

- UPDATE 키워드 이후에 변경할 테이블 이름을 지정하고 SET절에 '변경할 열이름 = 변경할 데이터'를 지정함.
- 여러 열의 데이터를 수정할 경우 쉼표(,)로 구분함.
- WHERE절 및 조건식 추가 가능함.

```
// UPDATE 기본 형식
UPDATE [변경할 테이블]
SET [변경할 열1]=[데이터], [변경할 열2]=[데이터], ... , [변경할 열n]=[데이터]
[WHERE 데이터를 변경할 대상 행을 선별하기 위한 조건];
```



### 2.2. 데이터 전체 수정하기

```
// DEPT_TEMP2 데이터 전체 수정
UPDATE DEPT_TEMP2
SET LOC = 'SEOUL';
```



### 2.3. 수정한 내용을 되돌리고 싶을 때

```
// 수정한 직전 내용 되돌리기
ROLLBACK;
```



### 2.4. 데이터 일부분만 수정하기

```
// 테이블 데이터 중 일부분만 수정
UPDATE DEPT_TEMP2
SET DNAME = 'DATABASE',
LOC = 'SEOUL'
WHERE DEPTNO = 40;
```



### 2.5. 서브쿼리를 사용하여 데이터 수정하기



### UPDATE문 사용할 때 유의점



## 3. 테이블에 있는 데이터 삭제하기

- DELETE문은 테이블에 있는 데이터를 삭제할 때 사용.

```
// DELETE문의 기본 형식
DELETE [FROM] [테이블 이름]
[WHERE 삭제할 대상 행을 선별하기 위한 조건식];
```



### 3.1. 데이터 일부분만 삭제하기

```
// WHERE절을 사용하여 데이터 일부분만 삭제하기
DELETE FROM EMP_TEMP2
WHERE JOB = 'MANAGER';
```



### 3.2. 서브쿼리를 사용하여 데이터 삭제하기



### 3.3. 데이터 전체 삭제하기

```
// 테이블에 있는 전체 데이터 삭제하기
DELETE FROM EMP_TEMP2;
```

