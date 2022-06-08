# SQL문 속 또 다른 SQL문, 서브쿼리



## 1. 서브쿼리



### 서브쿼리란?

- SQL문을 실행하는 데 필요한 데이터를 추가로 조회하기 위해 SQL문 내부에서 사용하는 SELECT문을 의미함.
- 메인쿼리 : 서브쿼리의 결과 값을 사용하여 기능을 수행하는 영역.

```
// 서브쿼리로 JONES의 급여보다 높은 급여를 받는 사원 정보 출력하기
SELECT *
  FROM EMP
 WHERE SAL > (SELECT SAL
                FROM EMP
               WHERE ENAME = 'JONES');
```



### 서브쿼리의 특징

- 연산자와 같은 비교 또는 조회 대상의 오른쪽에 놓이며 괄호()로 묶어서 사용.
- 대부분의 서브쿼리에서는 ORDER BY절을 사용할 수 없음.
- 메인쿼리 비교 대상과 같은 자료형, 같은 개수로 지정해야 함.
- 단일행 서브쿼리와 다중행 서브쿼리로 나뉨.



## 2. 실행 결과가 하나인 단일행 서브쿼리

- single-row subquery
- 단일행 연산자 사용하여 비교. >, >=, =, <=, <, <>, ^=, !=
- 중복 가능 데이터로 단일행 서브쿼리 사용할 경우 오류 발생할 수 있음.



### 단일행 서브쿼리와 날짜형 데이터

```
// 서브쿼리의 결과 값이 날짜형인 경우
SELECT *
  FROM EMP
 WHERE HIREDATE < (SELECT HIREDATE
                     FROM EMP
                    WHERE ENAME = 'SCOTT');
```



### 단일형 서브쿼리와 함수

```
// 서브쿼리 안에서 함수를 사용한 경우
SELECT E.EMPNO, E.ENAME, E.JOB, E.SAL, D.DEPTNO, D.DNAME, D.LOC
  FROM EMP E, DEPT D
 WHERE E.DEPTNO = D.DEPTNO
   AND E.DEPTNO = 20
   AND E.SAL > (SELECT AVG(SAL)
                  FROM EMP);
```



## 3. 실행 결과가 여러 개인 다중행 서브쿼리

- 다중행 연산자 사용.
  - IN
  - ANY, SOME
  - ALL
  - EXISTS



### IN 연산자

- 메인쿼리의 데이터가 서브쿼리의 결과 중 하나라도 일치하는 데이터가 있다면 true.

```
// IN 연산자 사용하기(부서번호 20이거나 30인 사원의 정보 출력)
SELECT *
  FROM EMP
 WHERE DEPTNO IN (20, 30);
```



```
// 각 부서별 최고 급여와 동일한 급여를 받는 사원 정보 출력하기
SELECT *
  FROM EMP
 WHERE SAL IN (SELECT MAX(SAL)
                 FROM EMP
               GROUP BY DEPTNO);
```



### ANY, SOME 연산자

- 서브쿼리가 반환한 여러 결과 값 중 메인쿼리와 조건식을 사용한 결과가 하나라도 true라면 메인쿼리 조건식을 true로 반환해 주는 연산자.
- 메인쿼리와 값을 비교할 때 ANY 및 SOME 연산자를 등가 비교 연산자(=)와 함께 사용하면 IN 연산자와 정확히 같은 기능을 수행함.

```
// ANY 연산자 사용하기
SELECT *
  FROM EMP
 WHERE SAL = ANY (SELECT MAX(SAL)
                    FROM EMP
                  GROUP BY DEPTNO);

// SOME 연산자 사용하기
SELECT *
  FROM EMP
 WHERE SAL = SOME (SELECT MAX(SAL)
                     FROM EMP
                   GROUP BY DEPTNO);
```

```
// 30번 부서 사원들의 최대 급여보다 적은 급여를 받는 사원 정보 출력하기
SELECT *
  FROM EMP
 WHERE SAL < ANY (SELECT SAL
                    FROM EMP
                   WHERE DEPTNO = 30)
                  ORDER BY SAL, EMPNO;
```

- < ANY는 MAX보다 작은 모든 값 출력, > ANY는 MIN보다 큰 모든 값 출력.



### ALL 연산자

- 서브쿼리의 모든 결과가 조건식에 맞아떨어져야만 메인쿼리의 조건식이 true가 되는 연산자
- <ALL은 MIN보다 작은 모든 값 출력, >ALL은 MAX보다 큰 모든 값 출력.

```
// 30번 부서 사원들의 최대 급여보다 많은 급여를 받는 사원 정보 출력하기
SELECT *
  FROM EMP
 WHERE SAL > ALL (SELECT SAL
                    FROM EMP
                   WHERE DEPTNO = 30);
```



### EXISTS 연산자

- 서브쿼리에 결과 값이 하나 이상 존재하면 조건식이 모두 true, 존재하지 않으면 모두 false가 되는 연산자.

```
// 서브쿼리결과 값이 존재하는 경우
SELECT *
  FROM EMP
 WHERE EXISTS (SELECT DNAME
                 FROM DEPT
                WHERE DEPTNO = 10);

// 서브쿼리 결과 값이 존재하지 않는 경우
SELECT *
  FROM EMP
 WHERE EXISTS (SELECT DNAME
                 FROM DEPT
                WHERE DEPTNO = 50);
```



## 4. 비교할 열이 여러 개인 다중열 서브쿼리

- 서브쿼리의 SELECT절에 비교할 데이터를 여러 개 지정하는 방식

```
// 다중열 서브쿼리 사용하기
SELECT *
  FROM EMP
 WHERE (DEPTNO, SAL) IN (SELECT DEPTNO, MAX(SAL)
                           FROM EMP
                         GROUP BY DEPTNO);
```



## 5. FROM절에 사용하는 서브쿼리와 WITH절

- FROM절에 사용하는 서브쿼리는 인라인 뷰(inline view)라고도 부름.
- 인라인 뷰는 특정 테이블 전체 데이터가 아닌 SELECT문을 통해 데이터를 먼저 추출해 온 후 별칭을 주어 사용함.

```
// 인라인 뷰 사용하기
SELECT E10.EMPNO, E10.ENAME, E10.DEPTNO, D.DNAME, D.LOC
  FROM (SELECT * FROM EMP WHERE DEPTNO = 10) E10,
       (SELECT * FROM DEPT) D
 WHERE E10.DEPTNO = D.DEPTNO;
```

- 오라클 9i부터 제공하는 WITH절은 메인쿼리가 될 SELECT문 안에서 사용할 서브쿼리와 별칭을 먼저 지정한 후 메인쿼리에서 사용함.

```
// WITH절 사용하기
WITH
E10 AS (SELECT * FROM EMP WHERE DEPTNO = 10),
D AS (SELECT * FROM DEPT)
SELECT E10.EMPNO, E10.ENAME, E10.DEPTNO, D.DNAME, D.LOC
  FROM E10, D
 WHERE E10.DEPTNO = D.DEPTNO;
```



## 6. SELECT절에 사용하는 서브쿼리

- SELECT절에 사용하는 서브쿼리를 스칼라 서브쿼리(scalar subquery)라고 부름.
- SELECT절에 하나의 열 영역으로서 결과를 출력할 수 있음.

```
// SELECT절에 서브쿼리 사용하기
SELECT EMPNO, ENAME, JOB, SAL,
       (SELECT GRADE 
          FROM SALGRADE
         WHERE E.SAL BETWEEN LOSAL AND HISAL) AS SALGRADE,
       DEPTNO,
      (SELECT DNAME
         FROM DEPT
        WHERE E.DEPTNO = DEPT.DEPTNO) AS DNAME
FROM EMP E;
```

