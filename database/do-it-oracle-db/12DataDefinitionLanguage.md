# 데이터 정의어 (DDL: Data Definition Language)

- 데이터 정의어 : 객체를 새로 만들거나 기존에 존재하던 객체를 변경하거나 삭제하는 등의 기능을 수행하는 명령어.



## 1. 데이터 정의어

- 데이터 정의어는 데이터베이스 데이터를 보관하고 관리하기 위해 제공되는 여러 객체의 생성, 변경, 삭제 관련 기능을 수행함.



### 데이터 정의어를 사용할 때 유의점

- 데이터 정의어는 명령어를 수행하자마자 데이터베이스에 수행한 내용이 바로 반영됨.
- 실행하면 자동 COMMIT되기 때문에 이전에 사용한 데이터 조작어는 영구히 데이터베이스에 반영됨.
- ROLLBACK을 통한 취소가 불가능함.



## 2. CREATE

- 오라클 데이터베이스 객체 생성에 사용하는 명령어.

```
//CREATE 기본 형식
CREATE TABLE 소유계정.테이블이름(
열1이름 열1자료형
열2이름 열2자료형
...
열N이름 열N자료형
);
```

- 테이블 이름 생성 규칙
  - 숫자로 시작 X
  - 30Byte 이하. 영어30자, 한글15자.
  - 같은 사용자 소유의 테이블 이름 중복 불가능.
  - SQL키워드 사용 불가능.
- 열 이름 생성 규칙
  - 문자로 시작
  - 30Byte 이하.
  - 한 테이블의 열 이름 중복 X.
  - $,#,_ 사용 가능.
  - SQL키워드 사용 불가능.

```
// 모든 열의 각 자료형을 정의해서 테이블 생성하기
CREATE TABLE EMP_DDL(
   EMPNO      NUMBER(4),
   ENAME      VARCHAR2(10),
   JOB        VARCHAR2(9),
   MGR        NUMBER(4),
   HIREDATE   DATE,
   SAL        NUMBER(7,2),
   COMM       NUMBER(7,2),
   DEPTNO     NUMBER(2)
);
```

- NUMBER(7, 2)는 소수점 이하 두 자리 숫자를 포함한 7자리 숫자를 저장할 수 있음을 뜻함.
- DATE는 길이 지정이 필요 없는 자료형이기 때문에 소괄호를 사용하지 않음.



### 기존 테이블 열 구조와 데이터를 복사하여 새 테이블 생성

```
// 다른 테이블을 복사하여 테이블 생성하기
CREATE TABLE DEPT_DDL
    AS SELECT * FROM DEPT;

DESC DEPT_DDL;
```



### 기존 테이블 열 구조와 일부 데이터만 복사하여 새 테이블 생성

```
// 다른 테이블의 일부를 복사하여 테이블 생성하기
CREATE TABLE EMP_DDL_30
    AS SELECT *
         FROM EMP
        WHERE DEPTNO = 30;

SELECT * FROM EMP_DDL_30;
// EMP 테이블과 열 구조는 같지만 30번 부서 사원 데이터만 저장함.
```



### 기존 테이블의 열 구조만 복사하여 새 테이블 생성

```
// 다른 테이블을 복사하여 테이블 생성하기
CREATE TABLE EMPDEPT_DDL
    AS SELECT E.EMPNO, E.ENAME, E.JOB, E.MGR, E.HIREDATE,
              E.SAL, E.COMM, D.DEPTNO, D.DNAME, D.LOC
         FROM EMP E, DEPT D
        WHERE 1 <> 1;

SELECT * FROM EMPDEPT_DDL;
// 1 <> 1 결과 값는 항상 false이기 때문에 EMP 테이블과 DEPT 테이블을 조인한 모든 결과 행이 출력 대상에서 제외되므로 CREATE문으로 만들어지는 테이블에는 행이 저장되지 않음.
```



## 3. ALTER

- 객체 변경할 때 사용.
- 새 열을 추가 또는 삭제, 열의 자료형 또는 길이를 변경하는 등 테이블 구조 변경과 관련된 기능을 수행함.



### 테이블에 열 추가하는 ADD

```
// ALTER 명령어로 HP 열 추가하기
ALTER TABLE EMP_ALTER
  ADD HP VARCHAR2(20);
```



### 열 이름을 변경하는 RENAME

```
// ALTER 명령어로 HP 열 이름을 TEL로 변경하기
ALTER TABLE EMP_ALTER
RENAME COLUMN HP TO TEL;
```



### 열의 자료형을 변경하는 MODIFY

```
// ALTER 명령어로 EMPNO 열 길이 변경하기
ALTER TABLE EMP_ALTER
MODIFY EMPNO NUMBER(5);
```

- 늘리는 것은 괜찮지만 길이를 줄이거나 다른 자료형으로 변경하는 것은 저장된 데이터 상태에 따라 결정됨.
- 테이블에 저장된 데이터에 문제가 생기지 않는 범위 내에서만 허용됨.



### 특정 열을 삭제할 때 사용하는 DROP

```
// ALTER 명령어로 TEL 열 삭제하기
ALTER TABLE EMP_ALTER
 DROP COLUMN TEL;
```



## 4. RENAME

- 테이블 이름을 변경할 때 사용.

```
// 테이블 이름 변경하기
RENAME EMP_ALTER TO EMP_RENAME;
```



## 5. TRUNCATE

- 특정 테이블의 모든 데이터를 삭제.
- 데이터만 삭제하고 테이블 구조에는 영향을 주지 않음.

```
// EMP_RENAME 테이블의 전체 데이터 삭제하기
TRUNCATE TABLE EMP_RENAME;
```

- 테이블 데이터 삭제는 데이터 조작어 중 WHERE절을 명시하지 않은 DELETE문의 수행으로도 가능함.
- TRUNCATE는 데이터 정의어이기 때문에 ROLLBACK이 되지 않는다는 점에서 DELETE문과 다름.



## 6. DROP

- 데이터베이스 객체를 삭제하는 데 사용됨.
- 테이블이 삭제되므로 테이블에 저장된 데이터도 모두 삭제됨.

```
// EMP_RENAME 테이블 삭제하기
DROP TABLE EMP_RENAME;
```

- 오라클 10g부터는 윈도우의 휴지통 기능과 같은 FLASHBACK 기능을 사용하여 DROP 명령어로 삭제된 테이블 복구 가능함.