# 더 정확하고 다양하게 결과를 출력하는 WHERE절과 연산자



> 이 문서는 [<Do It! 오라클로 배우는 데이터베이스 입문>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791163030300&orderClick=LEa&Kc=) 책을 보고 정리한 문서입니다.



## 1. WHERE절

```
SELECT *
    FROM EMP
WHERE DEPTNO = 30;
```

- WHERE절에 작성한 DEPTNO = 30은 'EMP 테이블에서 부서 번호 값이 30인 행만 조회하라'는 뜻.

- WHERE절은 많은 데이터 중에서 어떤 조건에 일치하는 행만을 골라내서 조회하는 데 사용함.

- WHERE절에 조건식을 사용한 SELECT문은 테이블의 각 행에 조건식 열 값을 검사한 후 결과 값이 true인 데이터만 출력함.



## 2. AND, OR 연산자

- WHERE절 조건식을 여러 개 지정할 수 있음.
- 이때 사용하는 것이 논리 연산자 AND, OR임.

```
// AND 연산자
SELECT *
    FROM EMP
WHERE DEPTNO = 30
    AND JOB = 'SALESMAN';
```

- WHERE절에서는 비교하는 데이터가 문자열일 경우에는 작은따옴표(')로 묶어 줌.

- 앞뒤에 공색이 있으면 공백도 문자로 인식하기 때문에 주의해야 함.
- SQL문에 사용하는 기본 형식은 대소문자를 구별하지 않고 사용할 수 있지만 테이블 안에 들어 있는 문자 또는 문자열 데이터는 대소문자를 구별함.

```
// OR 연산자
SELECT *
    FROM EMP
WHERE DEPTNO = 30
    OR JOB = 'CLERK';
```

- AND는 피연산자가 둘 다 true여야 하고, OR는 피연산자가 둘 다 또는 둘 중 하나 true이면 결과 값이 true가 됨.



### 2.1. WHERE절 조건식의 개수

- WHERE절에 사용할 수 있는 조건식의 개수는 사실상 제한이 없음.
- 조건식을 두 개 이상 사용할 경우에도 각 조건식 사이에 AND 또는 OR 연산자를 추가하여 사용할 수 있음.



### 2.2. 실무에서의 AND, OR 연산자

- 실무에서 사용하는 SELECT문은 OR 연산자보다 AND 연산자를 많이 사용하는 경향이 있음.



## 3. 연산자 종류와 활용 방법



### 3.1. 산술 연산자

- +, -, *, / 사칙연산 제공.
- 나머지 연산자 %는 SQL문에서 제공하지 않음.
- 오라클에서는 mod 함수를 통해 나머지 연산 기능을 사용할 수 있음.

```
// 곱셈 산술 연산자를 사용한 예
SELECT *
    FROM EMP
WHERE SAL * 12 = 36000;
```



### 3.2. 비교 연산자

#### 대소 비교 연산자

- 연산자 앞뒤에 있는 데이터 값을 비교할 때 사용함.

```
// 대소 비교 연산자를 사용하여 출력
SELECT *
    FROM EMP
WHERE SAL >= 3000;
```

- 초과, 이상, 미만, 이하 구분 잘 해야 함.

- 대소 비교 연산자는 비교 대상인 데이터가 숫자가 아닌 문자열일 때도 사용할 수 있음.

```
// 문자를 대소 비교 연산자로 비교(비교 문자열 하나일 때)
SELECT *
    FROM EMP
WHERE ENAME >= 'F'; // 사원 이름의 첫 문자가 F와 같거나 뒤쪽인 것만 검색됨.
```

```
// 문자를 대소 비교 연산자로 비교(비교 문자열이 여러 개일 때)
SELECT *
    FROM EMP
WHERE ENAME <= 'FORZ';
// 이름이 FORD인 사원도 출력됨. FORD 맨 끝 글자 D가 Z보다 앞에 있어서 조건을 만족함.
```



#### 등가 비교 연산자

- 연산자 양쪽 항목이 같은 값인지 검사하는 연산자.
- = 기호가 대표적인 등가 비교 연산자임.
  - = : 양쪽 항목이 같은 값일 경우 true, 다를 경우 false
- !=, <>, ^= : 양쪽 항목이 다를 경우 true, 같을 경우 false

```
// (!=) 사용하여 출력
SELECT *
    FROM EMP
WHERE SAL != 3000; // != 대신 <>, ^= 써도 출력 결과는 같음.
```



### 3.3. 논리 부정 연산자

- NOT 연산자.
- !=, ^=, <> 와 쓰임새가 같음.

```
// 논리 부정 연산자
SELECT *
    FROM EMP
WHERE NOT SAL = 3000;
```

- NOT 연산자는 보통 IN, BETWEEN, IS NULL 연산자와 함께 복합적으로 사용하는 경우가 많음.
- 복잡한 조건식에서 정반대의 최종 결과를 원할 때, 조건식을 일일이 수정하여 작성하는 것보다 NOT 연산자로 한 번에 뒤집어서 사용하는 것이 간편함.



### 3.4. IN 연산자

- 출력하고 싶은 열의 조건이 여러 가지 일 때 사용함.
- 특정 열에 해당하는 조건을 여러 개 지정할 수 있음.

```
// IN 연산자를 사용하여 출력
SELECT *
    FROM EMP
WHERE JOB IN ('MANAGER', 'SALESMAN', 'CLERK');
```

- IN 연산자 앞에 논리 부정 연산자 NOT을 사용하면 전체 부정 결과 출력 가능함.

```
// IN 연산자와 논리 부정 연산자 사용하여 출력
SELECT *
    FROM EMP
WHERE JOB NOT IN ('MANAGER', 'SALESMAN', 'CLERK');
```



### 3.5. BETWEEN A AND B 연산자

- 특정 열 값의 최소, 최대 범위를 지정하여 해당 범위 내의 데이터만 조회할 경우에 대소 비교 연산자 대신 BETWEEN A AND B 연산자를 사용하면 더 간단하게 표현할 수 있음.

```
// BETWEEN A AND B 연산자를 사용하여 출력
SELECT *
    FROM EMP
WHERE SAL BETWEEN 2000 AND 3000;
```

- NOT 연산자와 함께 사용할 수도 있음.



### 3.6. LIKE 연산자와 와일드 카드

- LIKE 연산자는 이메일이나 게시판 제목 또는 내용 검색 기능처럼 일부 문자열이 포함된 데이터를 조회할 때 사용함.

```
// LIKE 연산자
SELECT *
    FROM EMP
WHERE ENAME LIKE 'S%';
```

- ENAME LIKE 'S%' 조건식은 ENAME 열 값이 대문자 S로 시작하는 데이터를 조회하라는 뜻.
- % 기호를 와일드 카드(wild card)라고 함.
- 와일드 카드는 특정 문자 또는 문자열을 대체하거나 문자열 데이터의 패턴을 표기하는 특수 문자.
- LIKE 연산자와 함께 사용할 수 있는 와일드 카드는 '_'와 '%'임.
  - _ : 어떤 값이든 상관없이 한 개의 문자 데이터를 의미.
  - %: 길이와 상관없이 (문자 없는 경우도 포함) 모든 문자 데이터를 의미.

```
// 사원 이름의 두 번째 글자가 L인 사원 데이터만 출력
SELECT *
    FROM EMP
WHERE ENAME LIKE '_L%'; // 2번째 문자는 반드시 'L'이고, L 앞에는 반드시 한 문자가 와야 함.
```

- 어떤 단어가 포함된 제목 또는 본문 검색과 같은 기능을 구현할 때는 원하는 문자열 앞뒤 모두 와일드 카드(%)를 붙여 줄 수 있음.

```
// 사원 이름에 AM이 포함되어 있는 사원 데이터만 출력
SELECT *
    FROM EMP
WHERE ENAME LIKE '%AM%';
```

```
// 사원 이름에 AM 포함되어 있지 않은 사원 데이터만 출력
SELECT *
    FROM EMP
WHERE ENAME NOT LIKE '%AM%';
```



#### 와일드 카드 문자가 데이터 일부일 경우

- 데이터에 와일드 카드 기호가 포함된 경우 ESCAPE절 사용.

```
// 와일드 카드 문자가 데이터 일부일 때 ESCAPE절 사용
SELECT *
	FROM SOME_TABLE
WHERE SOME_COLUMN LIKE 'A\_A%' ESCAPE '\';
```

- ESCAPE 문자는 ESCAPE절에서 임의로 지정할 수 있음. \외에 다른 문자도 지정하여 사용할 수 있음.



#### LIKE 연산자와 와일드 카드 문자의 성능

- LIKE 연산자와 와일드 카드를 활용한 SELECT문은 와일드 카드를 어떻게 사용하느냐에 따라 데이터를 조회해 오는 시간에 차이가 난다고 알려져 있음.
- 조회 성능 관련 부분은 실무에서 주요 이슈가 될 수 있음.



### 3.7. IS NULL 연산자

- NULL은 데이터 값이 완전히 '비어 있는' 상태를 말함.
- 숫자 0은 값 0이 존재한다는 뜻으로 NULL과 혼동하지 않도록 주의할 것.
- NULL의 의미 : 값이 존재하지 않음. 해당 사항 없음. 노출할 수 없는 값. 확정되지 않은 값.

```
// 등가 비교 연산자로 NULL 비교하기
SELECT *
    FROM EMP
WHERE COMM = NULL;
```

- NULL은 산술 연산자와 비교 연산자로 비교해도 결과 값이 NULL이 됨.
- 어떤 값인지 모르는 값에 숫자를 더해도 어떤 값인지 알 수 없고, 어떤 값인지 모르는 값이 특정 값보다 큰지 작은지 알 수 없는 것과 같은 이치.
- WHERE절은 조건식의 결과 값이 true인 행만 출력하는데 연산 결과 값이 NULL이 되어 버리면 조건식의 결과 값이 false도 true도 아니게 되므로 출력 대상에서 제외됨.
- 특정 열 또는 연산의 결과 값이 NULL인지 확인하기 위해 IS NULL 연산자 사용.

```
// IS NULL 연산자를 사용하여 출력
SELECT *
    FROM EMP
WHERE COMM IS NULL;
```

- 반대의 경우 IS NOT NULL 사용하면 됨.

```
// 직속 상관이 있는 사원 데이터만 출력
SELECT *
    FROM EMP
WHERE MGR IS NOT NULL;
```

- AND는 한쪽이라도 NULL일 경우 결과값 출력이 안됨.
- OR는 한쪽이 NULL이어도 다른 한 쪽의 결과값을 출력함.



### 3.8. 집합 연산자

- SQL문에서는 SELECT문을 통해 데이터를 조회한 결과를 하나의 집합과 같이 다룰 수 있는 집합 연사자를 사용할 수 있음.
- 두 개 이상의 SELECT문의 결과 값을 연결할 때 사용함.

```
// 집합 연산자 UNION을 사용하여 출력
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
WHERE DEPTNO = 10
    UNION
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
WHERE DEPTNO = 20;
```

- UNION 연산자는 합집합을 의미하는 연산자.
- 집합 연산자로 두 개의 SELECT문의 결과 값을 연결할 때 각 SELECT문이 출력하려는 열 개수와 각 열의 자료형이 순서별로 일치해야 함.

```
// 집합 연산자 실행 오류 1) 열 개수 다를 때
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
WHERE DEPTNO = 10
    UNION
SELECT EMPNO, ENAME, SAL
    FROM EMP
WHERE DEPTNO = 20;

// 집합 연산자 실행 오류 2) 출력 열의 자료형이 다를 때
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
WHERE DEPTNO = 10
    UNION
SELECT ENAME, EMPNO, DEPTNO, SAL
    FROM EMP
WHERE DEPTNO = 20;
```

- 출력 열 개수와 자료형이 같다면 서로 다른 테이블에서 조회하거나 조회하는 열 이름이 다른 것은 문제 되지 않음.

```
// 집합 연산자 사용 (출력 열 개수와 자료형 같을 때)
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
WHERE DEPTNO = 10
    UNION
SELECT SAL, JOB, DEPTNO, SAL
    FROM EMP
WHERE DEPTNO = 20;
```

- 이 경우, 최종 출력되는 열 이름은 먼저 작성한 SELECT문의 열 이름으로 표기됨.

- 오라클 DB에서 사용하는 집합 연산자 4가지
  - UNION : 합집합. 결과 값 중복 제거.
  - UNION ALL : 합집합. 중복된 결과 값 모두 출력됨.
  - MINUS : 앞의 결과 값에서 뒤의 결과 값 차집합 처리. 먼저 작성한 SELECT문의 결과 값 중 다음 SELECT문에 존재하지 않는 데이터만 출력됨.
  - INTERSECT : 교집합. 결과 값이 같은 데이터만 출력됨.

```
// 결과 값 같을 때 UNION ALL 사용
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
WHERE DEPTNO = 10
    UNION ALL
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
WHERE DEPTNO = 10;
// 결과 값 2번 반복되어 출력됨.
```

```
// MINUS 사용하여 출력
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
    MINUS
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
WHERE DEPTNO = 10;
// 전체에서 부서번호 10번을 뺀 20번, 30번이 출력됨.
```

```
// INTERSECT 사용하여 출력
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
    INTERSECT
SELECT EMPNO, ENAME, SAL, DEPTNO
    FROM EMP
WHERE DEPTNO = 10;
// 전체와 부서번호 10번의 교집합인 부서번호 10번이 출력됨.
```



### 3.9. 연산자 우선순위

- 위에 위치할수록 우선순위 높음
- 소괄호로 묶이면 우선순위와 별개로 먼저 수행됨.
- *, /
- +, -
- =, !=, ^=, <>, >, >=, <, <=
- IS (NOT) NULL, (NOT) LIKE, (NOT) IN
- BETWEEN A AND B
- NOT
- AND
- OR





