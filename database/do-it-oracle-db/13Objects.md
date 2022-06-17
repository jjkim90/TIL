# 객체 종류

- 테이블은 SQL문과 더불어 오라클에서 가장 많이 사용하는 객체 중 하나
- 테이블 외에 데이터 사전, 인덱스, 뷰, 시퀀스, 동의어 등이 있음.



## 1. 데이터베이스를 위한 데이터를 저장한 데이터 사전



### 데이터 사전이란?

- 오라클 데이터베이스 테이블은 사용자 테이블(user table)과 데이터 사전(data dictionary)으로 나뉨.
- 데이터 사전은 데이터베이스를 구성하고 운영하는 데 필요한 모든 정보를 저장하는 특수한 테이블로 데이터베이스가 생성되는 시점에 자동으로 만들어짐.
- 데이터베이스 메모리, 성능, 사용자, 권한, 객체 등 오라클 데이터베이스 운영에 중요한 데이터가 보관되어 있음.
- 사용자가 데이터 사전 정보에 직접 접근하거나 작업하는 것은 허용되지 않음.
- 데이터 사전 뷰(data dictionary view)를 제공하여 SELECT문으로 정보 열람 가능.
- 데이터 사전 뷰 접두어 분류
  - USER_XXXX : 현재 데이터베이스에 접속한 사용자가 소유한 객체 정보
  - ALL_XXXX : 현재 데이터베이스에 접속한 사용자가 소유한 객체 또는 다른 사용자가 소유한 객체 중 사용 허가를 받은 객체, 즉 사용 가능한 모든 객체 정보
  - DBA_XXXX : 데이터베이스 관리를 위한 정보(데이터베이스 관리 권한을 가진 SYSTEM, SYS 사용자만 열람 가능)
  - V%_XXXX : 데이터베이스 성능 관련 정보(X$\_XXXX 테이블의 뷰)

```
// 사용 가능한 데이터 사전 살펴보기
SELECT * FROM DICT;
// SELECT * FROM DICTIONARY; 도 가능함.
```



### USER_ 접두어를 가진 데이터 사전

- 현재 오라클에 접속해 있는 사용자가 소유한 객체 정보가 보관되어 있음.

```
// SCOTT 계정이 가지고 있는 객체 정보 살펴보기(USER_ 접두어 사용)
SELECT TABLE_NAME
FROM USER_TABLES;
```

- 접두어 다음에 객체가 오면 복수형 단어가 옴.



### ALL_ 접두어를 가진 데이터 사전

- 접속 사용자 소유 객체 및 다른 사용자 소유 객체 중 허락된 객체 정보. 계정이 사용할 수 있는 모든 테이블 정보를 볼 수 있음.

```
// SCOTT 계정이 사용할 수 있는 객체 정보 살펴보기(ALL_ 접두어 사용)
SELECT OWNER, TABLE_NAME
FROM ALL_TABLES;
```



### DBA_ 접두어를 가진 데이터 사전

- 데이터베이스 관리 권한을 가진 사용자만 조회할 수 있는 테이블.
- SCOTT 계정으로는 조회가 불가능.
- 조회 불가능할 경우 '존재하지 않습니다'로 출력되는데, 그 이유는 사용 권한이 없는 사용자는 해당 개체의 존재 여부조차 확인할 수 없음을 의미함.
  - 웹 서비스에서는 '아이디나 비밀번호가 틀렸다'라는 식으로 둘 중 하나가 틀린 것 같다고 제시하는 것이 일반적임. 아이디 존재 여부를 알려 주지 않음.



#### DBA_ USERS로 사용자 정보 살펴보기

```
// DBA_USERS를 사용하여 사용자 정보를 알아보기(SYSTEM  계정으로 접속했을 때)
SELECT *
FROM DBA_USERS
WHERE USERNAME = 'SCOTT';
```



## 2. 더 빠른 검색을 위한 인덱스



### 인덱스란?

- 오라클 DB에서 데이터 검색 성능의 향상을 위해 테이블 열에 사용하는 객체.
- 테이블에 보관된 특정 행 데이터의 주소, 즉 위치 정보를 책 페이지처럼 목록으로 만들어 놓은 것.
- Table Full Scan : 테이블 데이터를 처음부터 끝까지 검색하여 원하는 데이터를 찾는 방식
- Index Scan : 인덱스를 통해 데이터를 찾는 방식.
- 인덱스는 사용자가 직접 특정 테이블의 열에 지정할 수도 있고 열이 기본키 또는 고유키일 경우에 자동으로 생성됨.



### 인덱스 생성

- CREATE문 사용함.

```
// 인덱스 생성 기본 형식
CREATE INDEX 인덱스이름
	ON 테이블이름(열이름1 ASC or DESC,
				열이름2 ASC or DESC,
				... 				);
```

```
// EMP 테이블의 SAL 열에 인덱스 생성하기
CREATE INDEX IDX_EMP_SAL
	ON EMP(SAL);
```

```
// 생성된 인덱스 살펴보기
SELECT * FROM USER_IND_COLUMNS;
```

- 인덱스 정렬 옵션을 지정하지 않으면 기본값은 오름차순(ASC)으로 지정됨.



### 인덱스 삭제

- 삭제는 DROP 명령어 사용함.

```
// 인덱스 삭제 기본 형식
DROP INDEX 인덱스이름;
```

- 인덱스 관련 더 자세한 내용은 SQL 튜닝 관련 서적 참고.



## 3. 테이블처럼 사용하는 뷰



### 뷰란?

- 가상 테이블(virtual table)이라고도 부름.
- 하나 이상의 테이블을 조회하는 SELECT문을 저장한 객체를 뜻함.
- 물리적 데이터를 따로 저장하지는 않음.
- 뷰를 SELECT문의 FROM절에 사용하면 특정 테이블을 조회하는 것과 같은 효과를 얻을 수 있음.
- 뷰 수정하면 원본 데이터도 수정됨.



### 뷰의 사용 목적(편리성)

- SELECT문의 복잡도를 완화하기 위해.
- 복잡한 SELECT문은 이후 수정이 필요하거나 다른 개발자가 코드를 처음부터 파악해야 하는 경우에 적잖은 시간과 노력이 듦.
- 여러 SQL문에서 자주 활용하는 SELECT문을 뷰로 저장해 놓은 후 다른 SQL문에서 활용하면 전체 SQL문의 복잡도를 완화하고 본래 목적의 메인 쿼리에 집중할 수 있어 편리함.



### 뷰의 사용 목적(보안성)

- 테이블의 특정 열을 노출하고 싶지 않은 경우
- 테이블의 일부 데이터 또는 조인이나 여러 함수 등으로 가공을 거친 데이터만 SELECT하는 뷰 열람 권한을 제공하는 것이 불필요한 데이터 노출을 막을 수 있기 때문에 더 안전한 방법임.



### 뷰 생성

- CREATE 문으로 생성

```
// 뷰 생성 기본 형식
CREATE [OR REPLACE] [FORCE | NOFORCE] VIEW 뷰이름 (열이름1, 열이름2, ...)
AS (저장할SELECT문)
[WITH CHECK OPTION [CONSTRAINT 제약 조건]]
[WITH READ ONLY [CONSTRAINT 제약 조건]];
```

```
// 뷰 생성 예제
CREATE VIEW VW_EMP20
    AS (SELECT EMPNO, ENAME, JOB, DEPTNO
          FROM EMP
         WHERE DEPTNO = 20);
```

- 생성한 뷰는 SELECT문의 FROM절에서 테이블처럼 지정하여 사용할 수 있음.



### 뷰 삭제

- 뷰를 삭제할 때 DROP문 사용

```
// 뷰 삭제하기
DROP VIEW VW_EMP20;
```

- 뷰는 실제 데이터가 아닌 SELECT문만 저장하므로 뷰를 삭제해도 테이블이나 데이터가 삭제되는 것은 아님.
- 데이터를 따로 저장하는 것이 허용되는 구체화 뷰(materialized view)도 존재함.



### 인라인 뷰를 사용한 TOP-N SQL문

- SQL문에서 일회성으로 만들어서 사용하는 뷰를 인라인 뷰(inline view)라고 함.
- SELECT문에서 사용되는 서브쿼리, WITH절에서 미리 이름을 정의해 두고 사용하는 SELECT문 등이 이에 해당함.
- ROWNUM은 의사 열(pseudo column)이라고 하는 특수 열. 의사 열은 데이터가 저장되는 실제 테이블에 존재하지 않지만 특정 목적을 위해 테이블에 저장되어 있는 열처럼 사용 가능한 열을 뜻함.
- ROWNUM, 서브쿼리, ORDER BY를 조합하여 상위 N명의 데이터만 출력하는 것이 가능함.

```
// 인라인 뷰로 TOP-3 추출하기 (서브쿼리 사용)
SELECT ROWNUM, E.*
  FROM (SELECT *
          FROM EMP E
        ORDER BY SAL DESC) E
 WHERE ROWNUM <= 3;
```





## 4. 규칙에 따라 순번을 생성하는 시퀀스



### 시퀀스란?

- 오라클 DB에서 특정 규칙에 맞는 연속 숫자를 생성하는 객체.
- 지속적이고 효율적인 번호 생성이 가능하므로 여러모로 자주 사용하는 객체.



### 시퀀스 생성

- MAX 함수에 1을 더한 값을 사용할 경우 데이터가 많아질수록 가장 큰 데이터를 찾고 새로운 번호를 계산하는 시간이 함께 늘어나므로 아쉬운 부분이 있음.
- 시퀀스는 지속적이고 효율적인 번호 생성이 가능함.
  - INCREMENT BY n : 시퀀스에서 생성할 번호의 증가값(기본값은 1)
  - START WITH n : 시퀀스에서 생성할 번호의 시작값(기본값은 1)
  - MAXVALUE n : 시퀀스에서 생성할 번호의 최댓값 지정.
  - MINVALUE n : 시퀀스에서 생성할 번호의 최소값 지정.
  - CYCLE : 시퀀스에서 생성한 번호가 최댓값에 도달했을 경우 시작 값에서 다시 시작.
  - CACHE n : 시퀀스가 생성할 번호를 메모리에 미리 할당해 놓은 수를 지정.


```
// DEPT 테이블을 사용하여 DEPT_SEQUENCE 테이블 생성하기
CREATE TABLE DEPT_SEQUENCE
    AS SELECT *
         FROM DEPT
        WHERE 1 <> 1;

// 시퀀스 생성하기
CREATE SEQUENCE SEQ_DEPT_SEQUENCE
   INCREMENT BY 10
   START WITH 10
   MAXVALUE 90
   MINVALUE 0
   NOCYCLE
   CACHE 2;

// 생성한 시퀀스 확인하기
SELECT *
  FROM USER_SEQUENCES;
```



### 시퀀스 사용

- 생성한 시퀀스 사용할 때는 [시퀀스이름.CURRVAL]과 [시퀀스이름.NEXTVAL] 사용.
- CURRVAL은 마지막 생성 번호 반환, NEXTVAL은 다음 번호 생성.
- 시퀀스 생성 직후에 CURRVAL 사용하면 번호가 만들어진 적이 없어 오류 발생.

```
// 시퀀스에서 생성한 순번을 사용한 INSERT문 실행하기
INSERT INTO DEPT_SEQUENCE (DEPTNO, DNAME, LOC)
VALUES (SEQ_DEPT_SEQUENCE.NEXTVAL, 'DATABASE', 'SEOUL');
```



### 시퀀스 수정

- ALTER 명령어로 수정.

```
// 시퀀스 옵션 수정하기
ALTER SEQUENCE SEQ_DEPT_SEQUENCE
   INCREMENT BY 3
   MAXVALUE 99
   CYCLE;
```



### 시퀀스 삭제

- DROP SEQUENCE 사용하여 삭제.

```
// 시퀀스 삭제
DROP SEQUENCE SEQ_DEPT_SEQUENCE;
```

- 프로젝트에서 테스트할 때는 테스트 끝난 후 지우고 재생성하면서 연습해야 번호 안 꼬임.



## 5. 공식 별칭을 지정하는 동의어



### 동의어란?

- 객체 이름 대신 사용할 수 있는 다른 이름을 부여하는 객체.
- 테이블 이름 길 때 일종의 별명 부여.



### 동의어 생성

```
// EMP 테이블의 동의어 생성
CREATE SYNONYM E
   FOR EMP;
```



### 동의어 삭제

```
// 동의어 삭제
DROP SYNONYM E;
```

