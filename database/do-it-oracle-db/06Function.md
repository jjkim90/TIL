# 데이터 처리와 가공을 위한 오라클 함수



## 1. 오라클 함수

### 1.1. 함수란?

- X값의 변화에 따라 Y값이 종속적으로 변함.
- 오라클 함수는 특정 결과 데이터를 얻기 위해 어떤 값이나 데이터를 입력하는데 그 값에 따라 가공 또는 연산의 과정을 거쳐 결과 값이 나옴.
- 오라클 함수는 특정한 결과 값을 얻기 위해 데이터를 입력할 수 있는 특수 명령어를 의미함.



### 1.2. 오라클 함수의 종류

- 오라클에서 기본으로 제공하는 내장 함수(built-in function)와 사용자가 필요에 의해 직접 정의한 사용자 정의 함수(user-defined function)으로 나뉨.



### 1.3. 내장 함수의 종류

- 단일행 함수(single-row finction) : 데이터가 한 행씩 입력되고 입력된 한 행당 결과가 하나씩 나오는 함수.
- 다중행 함수(multi-row function) : 여러 행이 입력되어 하나의 행으로 결과가 반환되는 함수.



## 2. 문자 함수

- 문자 함수는 문자 데이터를 가공하거나 문자 데이터로부터 특정 결과를 얻고자 할 때 사용하는 함수.



### 2.1. UPPER, LOWER, INITCAP 함수

- UPPER(문자열) : 괄호 안 문자 데이터를 모두 대문자로 변환하여 반환
- LOWER(문자열) : 괄호 안 문자 데이터를 모두 소문자로 변환하여 반환
- INITCAP(문자열) : 괄호 안 문자 데이터 중 첫 글자는 대문자로, 나머지 문자를 소문자로 변환하여 반환

```
// UPPER, LOWER, INITCAP 함수 사용하기
SELECT ENAME, UPPER(ENAME), LOWER(ENAME), INITCAP(ENAME)
FROM EMP;
```

- 게시판의 글 제목이나 글 본문과 같이 가변 길이 문자열 데이터에서 특정 문자열을 포함하는 데이터를 조회하는 경우, 조건식 양쪽 항목의 문자열 데이터를 모두 대문자나 소문자로 바꿔서 비교한다면 실제 검색어의 대소문자 여부와 상관없이 검색 단어와 일치한 문자열을 포함한 데이터를 찾을 수 있음.

```
// EMP 테이블에서 사원 이름이 대소문자 상관없이 scott인 사람을 찾기
SELECT *
FROM EMP
WHERE UPPER(ENAME) = UPPER('scott');

//EMP 테이블에서 사원 이름에 SCOTT 단어를 포함한 데이터 찾기
SELECT *
FROM EMP
WHERE UPPER(ENAME) LIKE UPPER('%scott%');
```

- 실무에서는 일반적으로 대소문자가 다른 문자열 데이터를 검색할 때 INITCAP 함수보다 UPPER나 LOWER 함수를 많이 사용함.



### 2.2. LENGTH 함수

- 특정 문자열의 길이를 구할 때 LENGTH 함수를 사용.



#### LENGTH 함수 사용

```
// 선택한 열의 문자열 길이 구하기
SELECT ENAME, LENGTH(ENAME)
FROM EMP;
```



#### WHERE절에서 LENGTH 함수 사용

```
// 사원 이름의 길이가 5 이상인 행 출력하기
SELECT ENAME, LENGTH(ENAME)
FROM EMP
WHERE LENGTH(ENAME) >= 5;
```

- LENGTH 함수는 숫자 비교도 가능함.



#### LENGTH 함수와 LENGTHB 함수 비교

```
// LENGTH 와 LENGTHB 비교
SELECT LENGTH('한글'), LENGTHB('한글')
FROM DUAL;
// 결과 값으로 앞은 2, 뒤는 4 출력
```

- LENGTHB : 문자열 데이터 길이가 아닌 바이트 수를 반환
  - 한글은 한 문자당 2바이트로 처리됨.

- DUAL 테이블은 오라클의 최고 권한 관리자 계정인 SYS 소유의 테이블로 SCOTT 계정도 사용할 수 있는 더미(dummy) 테이블임. 임시 연산이나 함수의 결과 값 확인 용도로 종종 사용됨.



### 2.3. SUBSTR 함수

- 문자열 중 일부를 추출할 때 SUBSTR 함수 사용함.
- 주민등록번호 중 생년월일 앞자리만 필요하거나 전화번호의 마지막 네 자리 숫자만 추출하는 경우 등에 사용됨.
  - SUBSTR(문자열 데이터, 시작 위치, 추출 길이) : 추출 길이만큼 추출.
  - SUBSTR(문자열 데이터, 시작 위치) : 문자열 데이터 끝까지 추출.
  - 시작 위치가 음수일 경우에는 마지막 위치부터 거슬러 올라간 위치에서 끝까지 추출함.

```
// SUBSTR 함수 사용
SELECT JOB, SUBSTR(JOB, 1, 2), SUBSTR(JOB, 3, 2), SUBSTR(JOB, 5)
FROM EMP;
```



#### SUBSTR 함수와 다른 함수 함께 사용

- 다른 함수의 결과 값을 SUBSTR 함수의 입력 값으로 사용할 수도 있음.

```
// SUBSTR 안에 LENGTH 사용
SELECT JOB,
    SUBSTR(JOB, -LENGTH(JOB)),
    SUBSTR(JOB, -LENGTH(JOB), 2),
    SUBSTR(JOB, 3)
FROM EMP;
```

- 자주 사용하지는 않지만 부분 문자열을 추출할 때 바이트 수로 시작 위치나 길이를 지정할 수 있는 SUBSTRB 함수도 존재함.



### 2.4. INSTR 함수

- 문자열 데이터 안에 특정 문자나 문자열이 어디에 포함되어 있는지를 알고자 할 때 INSTR 함수 사용.
- INSTR 함수는 총 네 개의 입력 값을 지정할 수 있음.
- 원본 문자열 데이터와 원본 문자열 데이터에서 찾으려는 문자 이렇게 두 가지는 반드시 지정해줘야 함.

```
INSTR([대상 문자열 데이터], [위치를 찾으려는 부분 문자], [위치 찾기를 시작할 대상 문자열 데이터 위치], [시작 위치에서 찾으려는 문자가 몇 번째인지 지정])
```

```
// INSTR 함수로 문자열 데이터에서 특정 문자열 찾기
SELECT INSTR('HELLO, ORACLE!', 'L') AS INSTR_1,
    INSTR('HELLO, ORACLE!', 'L', 5) AS INSTR_2,
    INSTR('HELLO, ORACLE!', 'L', 2, 2) AS INSTR_3
    FROM DUAL;
```

- 위치 찾기를 시작하는 위치 값에 음수를 쓸 때 원보 문자열 데이터의 오른쪽 끝부터 왼쪽 방향으로 검색함.
- 찾으려는 문자가 문자열 데이터에 포함되어 있지 않다면 위치 값이 없으므로 0을 반환함.
- INSTR 함수를 LIKE와 비슷한 용도로 사용할 수도 있음.



#### 특정 문자를 포함하고 있는 행 찾기

```
// INSTR 함수로 사원 이름에 문자 S가 있는 행 구하기
SELECT *
FROM EMP
WHERE INSTR(ENAME, 'S') > 0;
```

```
// LIKE 함수로 사원 이름에 문자 S가 있는 행 구하기
SELECT *
FROM EMP
WHERE ENAME LIKE ('%S%');
```



### 2.5. 특정 문자를 다른 문자로 바꾸는 REPLACE 함수

- REPLACE 함수는 특정 문자열 데이터에 포함된 문자를 다른 문자로 대체할 경우에 유용한 함수.

```
REPLACE([문자열 데이터 또는 열 이름], [찾는 문자], [대체할 문자])
```

- 필수, 필수, 선택

- 만약 대체할 문자를 입력하지 않는다면 찾는 문자로 지정한 문자는 문자열 데이터에서 삭제됨.

```
// REPLACE 함수로 문자열 안에 있는 특정 문자 바꾸기
SELECT '010-1234-5678' AS REPLACE_BEFORE,
    REPLACE('010-1234-5678', '-', ' ') AS REPLACE_1,
    REPLACE('010-1234-5678', '-') AS REPLACE_2
    FROM DUAL;
```

- REPLACE 함수는 카드 번호나 주민 번호, 계좌 번호, 휴대전화 번호 또는 날짜나 시간을 나타내는 데이터처럼 특정 문자가 중간중간 끼어 있는 데이터에서 해당 문자를 없애 버리거나 다른 문잘 ㅗ바꾸어 출력할 때 종종 사용함.



### 2.6. 데이터의 빈 공간을 특정 문자로 채우는 LPAD, RPAD 함수

- LPAD와 RPAD는 각각 Left Padding(왼쪽 패딩), Right Padding(오른쪽 패딩)을 뜻함.
- 데이터와 자릿수를 지정한 후 데이터 길이가 지정한 자릿수보다 작을 경우에 나머지 공간을 특정 문자로 채우는 함수
- LPAD는 남은 빈 공간을 왼쪽에 채우고, RPAD는 남은 빈 공간을 오른쪽에 채움.
- 만약 빈 공간에 채울 문자를 지정하지 않으면 빈 공간의 자릿수만큼 공백 문자로 띄움.

```
// LPAD, RPAD 함수 사용하여 출력하기
SELECT 'Oracle',
    LPAD('Oracle', 10, '#') AS LPAD_1,
    RPAD('Oracle', 10, '*') AS RPAD_1,
    LPAD('Oracle', 10) AS LPAD_2,
    RPAD('Oracle', 10) AS RPAD_2
FROM DUAL;
```

- Oracle 6글자를 제외한 4자리를 문자 혹은 공백으로 왼쪽 혹은 오른쪽에 채움.
- 패딩 처리는 데이터의 일부만 노출해야 하는 개인정보를 출력할 때 사용함.



#### 특정 문자로 자릿수 채워서 출력하기

```
// RPAD 함수를 사용하여 개인정보 두시자리 * 표시로 출력하기
SELECT
    RPAD('971225-', 14, '*') AS RPAD_SNN,
    RPAD('010-1234-', 13, '*') AS RPAD_PHONE
FROM DUAL;
```



### 2.7. 두 문자열 데이터를 합치는 CONCAT 함수

- CONCAT 함수는 두 개의 문자열 데이터를 하나의 데이터로 연결해 주는 역할을 함.
- 두 개의 입력 데이터 지정을 하고 열이나 문자열 데이터 모두 지정할 수 있음.

```
// 두 열 사이에 콜론(:) 넣고 연결하기
SELECT CONCAT(EMPNO, ENAME),
    CONCAT(EMPNO, CONCAT(':', ENAME))
FROM EMP
WHERE ENAME = 'SCOTT';
```



#### 문자열 데이터를 연결하는 || 연산자

- || 연산자는 CONCAT 함수와 유사하게 열이나 문자열을 연결함.

```
// ||연산자 사용하기
SELECT EMPNO || ENAME,
    EMPNO || ':' || ENAME
FROM EMP
WHERE ENAME = 'SCOTT';
```



### 2.8. 특정 문자를 지우는 TRIM, LTRIM, RTRIM 함수

- TRIM, LTRIM, RTRIM 함수는 문자열 데이터 내에서 특정 문자를 지우기 위해 사용함.



#### TRIM 함수의 기본 사용법

```
TRIM([삭제 옵션], [삭제할 문자] FROM [원본 문자열 데이터])
```

- 원본 문자열 데이터는 필수, 나머지는 선택
- [삭제할 문자]가 생략될 경우 기본적으로 공백을 제거함.
- 삭제 옵션
  - LEADING : 왼쪽에 있는 글자 지움
  - TRAILING : 오른쪽에 있는 글자 지움
  - BOTH : 양쪽의 글자를 지움
  - 삭제 옵션 생략하면 기본 BOTH.



#### TRIM 함수 사용하기(삭제할 문자가 없을 때)

```
// TRIM 함수로 공백 제거하여 출력
SELECT '[' || TRIM(' _ _Oracle_ _ ') || ']' AS TRIM,
    '[' || TRIM(LEADING FROM ' _ _Oracle_ _ ') || ']' AS TRIM_LEADING,
    '[' || TRIM(TRAILING FROM ' _ _Oracle_ _ ') || ']' AS TRIM_TRAILING,
    '[' || TRIM(BOTH FROM ' _ _Oracle_ _ ') || ']' AS TRIM_BOTH
FROM DUAL;
```



#### TRIM 함수 사용하기(삭제할 문자가 있을 때)

```
// TRIM 함수로 삭제할 문자 _ 삭제 후 출력
SELECT '[' || TRIM('_' FROM '_ _Oracle_ _') || ']' AS TRIM,
    '[' || TRIM(LEADING '_' FROM '_ _Oracle_ _') || ']' AS TRIM_LEADING,
    '[' || TRIM(TRAILING '_' FROM '_ _Oracle_ _') || ']' AS TRIM_TRAILING,
    '[' || TRIM(BOTH '_' FROM '_ _Oracle_ _') || ']' AS TRIM_BOTH
FROM DUAL;
```



#### LTRIM, RTRIM 함수의 기본 사용법

- LTRIM, RTRIM 함수는 각각 왼쪽, 오른쪽의 지정 문자를 삭제하는 데 사용함.
- 삭제할 문자를 지정하지 않을 경우 공백 문자가 삭제됨.
- TRIM 함수와 다르게 삭제할 문자를 하나만 지정하는 것이 아니라 여러 문자 지정이 가능함.

```
LTRIM([원본 문자열 데이터], [삭제할 문자 집합])
RTRIM([원본 문자열 데이터], [삭제할 문자 집합])
```

```
// TRIM, LTRIM, RTRIM 사용하여 문자열 출력
SELECT '[' || TRIM(' _Oracle_ ') || ']' AS TRIM,
    '[' || LTRIM(' _Oracle_ ') || ']' AS LTRIM,
    '[' || LTRIM('<_Oracle_>', '_<') || ']' AS LTRIM_2,
    '[' || RTRIM(' _Oracle_ ') || ']' AS RTRIM,
    '[' || RTRIM('<_Oracle_>', '>_') || ']' AS RTRIM_2
FROM DUAL;
```

- 실무에서 TRIM 함수는 검색 기준이 되는 데이터에 혹시나 들어 있을지도 모르는 양쪽 끝의 공백을 제거할 때 많이 사용함.
  - 유저가 로그인을 하려고 아이디를 입력했을 때 사용자 실수로 spacebar가 눌러져 공백이 함께 입력되는 경우.



## 3. 숫자 데이터를 연산하고 수치를 조정하는 숫자 함수

- ROUND : 지정된 숫자의 특정 위치에서 반올림한 값을 반환
- TRUNC : 지정된 숫자의 특정 위치에서 버림한 값을 반환
- CEIL : 지정된 숫자보다 큰 정수 중 가장 작은 정수를 반환
- FLOOR : 지정된 숫자보다 작은 정수 중 가장 큰 정수를 반환
- MOD : 지정된 숫자를 나눈 나머지 값을 반환



### 3.1. 특정 위치에서 반올림하는 ROUND 함수

- ROUND 함수는 TRUNC 함수와 함께 가장 많이 사용하는 숫자 함수 중 하나.
- 특정 숫자를 반올림하되 반올림할 위치를 지정할 수 있음.
- 반올림할 위치를 지정하지 않으면 소수점 첫째 자리에서 반올림한 결과 반환.

```
ROUND([숫자], [반올림 위치])
```

- 숫자는 필수, 반올림 위치 선택.

```
// ROUND 함수를 사용하여 반올림된 숫자 출력
SELECT ROUND(1234.5678) AS ROUND,
    ROUND(1234.5678, 0) AS ROUND_0,
    ROUND(1234.5678, 1) AS ROUND_1,
    ROUND(1234.5678, 2) AS ROUND_2,
    ROUND(1234.5678, -1) AS ROUND_MINUS1,
    ROUND(1234.5678, -2) AS ROUND_MINUS2
FROM DUAL;
```

- 반올림 위치를 지정하지 않은 반환값은 반올림 위치를 0으로 지정한 것과 같은 결과가 출력됨.
- 반올림 위치 값이 0에서 양수로 올라가면 반올림 위치가 한 자리씩 더 낮은 소수점 자리를 향하게 되고, 0에서 음수로 내려가면 자연수 쪽으로 한 자리씩 위로 반올림하게 됨.



### 3.2. 특정 위치에서 버리는 TRUNC 함수

- TRUNC 함수는 지정된 자리에서 숫자를 버림 처리하는 함수.
- 버림 처리할 자릿수 지정 가능.
- 반올림 위치를 지정하지 않으면 소수점 첫째자리에서 버림 처리됨.

```
TRUNC([숫자], [버림 위치])
```

- 숫자는 필수, 버림 위치는 선택.

```
// TRUNC 함수를 사용하여 숫자 출력
SELECT TRUNC(1234.5678) AS TRUNC,
    TRUNC(1234.5678, 0) AS TRUNC_0,
    TRUNC(1234.5678, 1) AS TRUNC_1,
    TRUNC(1234.5678, 2) AS TRUNC_2,
    TRUNC(1234.5678, -1) AS TRUNC_MINUS1,
    TRUNC(1234.5678, -2) AS TRUNC_MINUS2
FROM DUAL;
```



### 3.3. 지정한 숫자와 가까운 정수를 찾는 CEIL, FLOOR 함수

- CEIL 함수와 FLOOR 함수는 각각 입력된 숫자와 가까운 큰 정수, 작은 정수를 반환하는 함수

```
CEIL([숫자])
FLOOR([숫자])
```

- 숫자 필수

```
// CEIL, FLOOR 함수로 숫자 출력
SELECT CEIL(3.14),
    FLOOR(3.14),
    CEIL(-3.14),
    FLOOR(-3.14)
FROM DUAL;
```



### 3.4. 숫자를 나눈 나머지 값을 구하는 MOD 함수

- MOD 함수는 나머지를 구하는 함수.

```
MOD([나눗셈 될 숫자], [나눌 숫자])
```

- 나눗셈 될 숫자와 나눌 숫자 필수

```
// MOD 함수 사용하여 나머지 값 출력
SELECT MOD(15,6),
    MOD(10, 2),
    MOD(11, 2)
FROM DUAL;
```



## 4. 날짜 데이터를 다루는 날짜 함수

- DATE형 데이터는 간단한 연산이 가능함.
- 날짜 데이터끼리의 더하기는 연산이 되지 않음.
  - 날짜 데이터 + 숫자 : 날짜 데이터보다 숫자만큼 일수 이후의 날짜
  - 날짜 데이터 - 숫자 : 날짜 데이터보다 숫자만큼 일수 이전의 날짜
  - 날짜 데이터 - 날짜 데이터 : 두 날짜 데이터 간의 일수 차이
  - 날짜 데이터 + 날짜 데이터 : 연산 불가, 지원하지 않음.
- SYSDATE 함수 : 별다른 입력 데이터 없이 오라클 데이터베이스 서버가 놓인 OS의 현재 날짜와 시간을 보여줌.

```
// SYSDATE 함수를 사용하여 날짜 출력
SELECT SYSDATE AS NOW,
    SYSDATE-1 AS YESTERDAY,
    SYSDATE+1 AS TOMORROW
FROM DUAL;
```



### 4.1. 몇 개월 이후 날짜를 구하는 ADD_MONTHS 함수

- ADD_MONTHS 함수는 특정 날짜에 지정한 개월 수 이후 날짜 데이터를 반환하는 함수.

```
ADD_MONTHS([날짜 데이터], [더할 개월 수(정수)])
```

- 둘 다 필수

```
// SYSDATE와 ADD_MONTHS 함수로 3개월 후 날짜 구하기
SELECT SYSDATE,
    ADD_MONTHS(SYSDATE, 3)
FROM DUAL;
```

- 은근히 자주 사용됨. 윤년 등의 이유로 복잡해질 수 있는 날짜 계산을 간단하게 만들어주기 때문.

```
// 입사 10주년이 되는 사원들 데이터 출력하기
SELECT EMPNO, ENAME, HIREDATE,
    ADD_MONTHS(HIREDATE, 120) AS WORK10YEAR
FROM EMP;
```

```
// 입사 40년 미만인 사원 데이터 출력하기
SELECT EMPNO, ENAME, HIREDATE, SYSDATE
FROM EMP
WHERE ADD_MONTHS(HIREDATE, 40*12) > SYSDATE;
```



### 4.2. 두 날짜 간의 개월 수 차이를 구하는 MONTHS_BETWEEN 함수

- MONTHS_BETWEEN 함수는 두 개의 날짜 데이터를 입력하고 두 날짜 간의 개월 수 차이를 구하는 데 사용함.

```
MONTHS_BETWEEN([날짜 데이터1], [날짜 데이터2])
```

- 둘 다 필수 입력.

```
// HIREDATE와 SYSDATE 사이의 개월 수를 MONTHS_BETWEEN 함수로 출력하기
SELECT EMPNO, ENAME, HIREDATE, SYSDATE,
    MONTHS_BETWEEN(HIREDATE, SYSDATE) AS MONTHS1,
    MONTHS_BETWEEN(SYSDATE, HIREDATE) AS MONTHS2,
    TRUNC(MONTHS_BETWEEN(SYSDATE, HIREDATE)) AS MONTHS3
    FROM EMP;
```

- 비교 날짜의 입력 위치에 따라 음수 또는 양수가 나올 수 있음.



### 4.3. 돌아오는 요일, 달의 마지막 날짜를 구하는 NEXT_DAY, LAST_DAY 함수

- NEXT_DAY 함수는 날짜 데이터와 요일 문자열을 입력하여 입력한 날짜 데이터에서 돌아오는 요일의 날짜를 반환함.



#### NEXT_DAY 함수의 기본 사용법



#### LAST_DAY 함수의 기본 사용법



## 5. 자료형을 변환하는 형 변환 함수



## 6. NULL 처리 함수



## 7. 상황에 따라 다른 데이터를 반환하는 DECODE 함수와 CASE문