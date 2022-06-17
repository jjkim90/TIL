# 사용자, 권한, 롤 관리



## 1. 사용자 관리



### 사용자란?

- 오라클 데이터베이스에 접속하여 데이터를 관리하는 계정을 사용자(USER)로 표현함.
- 업무 분할과 효율, 보안을 고려하여 업무에 따라 여러 사용자들을 나눔.



### 데이터베이스 스키마란?

- 데이터베이스에서 데이터 간 관계, 데이터 구조, 제약 조건 등 데이터를 저장 및 관리하기 위해 정의한 데이터베이스 구조의 범위를 스키마(schema)를 통해 그룹 단위로 분류함.
- 사용자는 데이터를 사용 및 관리하기 위해 오라클 데이터베이스에 접속하는 개체를 뜻하고, 스키마는 오라클 데이터베이스에 접속한 사용자와 연결된 객체를 의미함.



### 사용자 생성

- CREATE USER문을 사용함.

```SQL
-- 사용자 생성 기본 형식
CREATE USER 사용자이름(필수)
IDENTIFIED BY 패스워드(필수)
기타 옵션
```

- 사용자 생성은 일반적으로 데이터베이스 관리 권한을 가진 사용자가 권한을 가지고 있음.
- 오라클 데이터베이스 설치할 때 자동으로 생성된 SYS, SYSTEM이 데이터베이스 관리 권한을 가진 사용자임.

```SQL
-- SYSTEM 사용자로 접속 후 ORCLSTUDY 사용자에게 권한 부여하기
GRANT CREATE SESSION TO ORCLSTUDY;
```

- GRANT문은 권한을 부여하기 위해 사용하는 명령어.



### 사용자 정보 조회

```SQL
-- 사용자 또는 사용자 소유 객체 정보를 얻기 위해 다음과 같은 데이터 사전 사용할 수 있음.
SELECT * FROM ALL_USERS
WHERE USERNAME = 'ORCLSTUDY';

SELECT * FROM DBA_USERS
WHERE USERNAME = 'ORCLSTUDY';

SELECT * FROM DBA_OBJECTS
WHERE USERNAME = 'ORCLSTUDY';
```



### 오라클 사용자의 변경과 삭제

#### 오라클 사용자 변경

- ALTER USER문 사용.

```SQL
-- 사용자 정보(패스워드) 변경하기
ALTER USER ORCLSTUDY
IDENTIFIED BY ORCL;
```



#### 오라클 사용자 삭제

- DROP USER문 사용.

```SQL
-- 사용자 삭제하기
DROP USER ORCLSTUDY;
```



#### 오라클 사용자와 객체 모두 삭제

- 사용자 스키마에 객체가 있을 경우에 CASCADE 옵션을 사용하여 사용자와 객체 모두 삭제할 수 있음.

```SQL
-- 사용자와 객체 모두 삭제하기
DROP USER ORCLSTUDY CASCADE;
```



## 2. 권한 관리

- 데이터베이스는 접속 사용자에 따라 접근할 수 있는 데이터 영역과 권한을 지정해 줄 수 있음.
- 오라클에서는 권한을 시스템 권한(system privilege)과 객체 권한(object privilege)으로 분류하고 있음.



### 시스템 권한이란?

- 사용자 생성과 정보 수정 및 삭제, 데이터베이스 접근, 오라클 데이터베이스의 여러 자원과 객체 생성 및 관리 등의 권한을 포함함.
- ANY 키워드가 들어 있는 권한은 소유자에 상관없이 사용 가능한 권한을 의미함.

- 시스템 권한 분류
  - USER(사용자)
  - SESSION(접속)
  - TABLE(테이블)
  - INDEX(인덱스)
  - VIEW(뷰)
  - SEQUENCE(시퀀스)
  - SYNONYM(동의어)
  - PROFILE(프로파일)
  - ROLE(롤)



### 시스템 권한 부여

- GRANT문 사용함.

```SQL
-- 권한 부여 기본 형식
GRANT [시스템권한] TO [사용자이름/롤이름/PUBLIC]
[WITH ADMIN OPTION];
```

- 한 번에 여러 종류의 권한을 부여하려면 쉼표(,)로 구분하여 권한 이름을 여러 개 명시해 주면 됨.
- 여러 사용자 또는 롤에 적용할 경우 쉼표(,)로 구분함.
- PUBLIC은 현재 오라클 데이터베이스의 모든 사용자에게 권한을 부여하겠다는 의미임.
- WITH ADMIN OPTION은 현재 GRANT문을 통해 부여받은 권한을 다른 사용자에게 부여할 수 있는 권한도 함께 부여받음.

```SQL
-- 사용자 권한 부여하기
GRANT RESOURCE, CREATE SESSION, CREATE TABLE TO ORCLSTUDY;
```

- RESOURCE는 오라클 데이터베이스에서 제공하는 롤(role) 중 하나.
- 롤은 여러 권한을 하나의 이름으로 묶어 권한 부여 관련 작업을 간편하게 하려고 사용함.
- RESOURCE 롤은 사용자를 생성할 때 사용 테이블 스페이스의 영역을 무제한 사용 가능(UNLIMITED TABLESPACE)하게 해 주는 권한이 포함되어 있음.
- UNLIMITED TABLESPACE 권한은 엄밀한 관리가 필요한 경우에 적절하지 않으므로 사용자를 생성 및 수정할 때 QUOTA절로 사용 영역에 제한을 두기도 함.

```SQL
-- QUOTA절 사용
ALTER USER ORCLSTUDY
QUOTA 2M ON USERS;
```



### 시스템 권한 취소

- REVOKE문 사용함.

```SQL
-- 시스템 권한 취소 기본 형식
REVOKE [시스템권한] FROM [사용자이름/롤이름/PUBLIC];
```



### 객체 권한이란?

- 객체 권한(object privilege)은 특정 사용자가 생성한 테이블, 인덱스, 뷰, 시퀀스 등과 관련된 권한임.



### 객체 권한 부여

- GRANT문 사용

```SQL
-- 객체 권한 부여 기본 형식
GRANT [객체 권한/ALL PRIVILEGES]
ON [스키마.객체이름]
TO [사용자이름/롤이름/PUBLIC]
[WITH GRANT OPTION];
```

- 객체 권한 여러 종류 부여하려면 쉼표로 구분함.
- ALL PRIVILEGES는 객체의 모든 권한을 부여함을 의미함.

```SQL
-- ORCLSTUDY 사용자에게 TEMP 테이블 권한 부여하기
CONN SCOTT/tiger

CREATE TABLE TEMP(
   COL1 VARCHAR(20),
   COL2 VARCHAR(20)
);

GRANT SELECT ON TEMP TO ORCLSTUDY;

GRANT INSERT ON TEMP TO ORCLSTUDY;
```

```SQL
-- ORCL에게 TEMP 테이블의 여러 권한을 한 번에 부여하기
GRANT SELECT, INSERT ON TEMP
   TO ORCLSTUDY;
```



### 객체 권한 취소

- REVOKE문 사용.

```SQL
-- 객체 권한 취소 기본 형식
REVOKE [객체권한/ALL PRIVILEGES]
ON [스키마.객체이름]
FROM [사용자이름/롤이름/PUBLIC]
[CASCADE CONSTRAINTS/FORCE](선택);
```

```SQL
-- ORCLSTUDY에 부여된 TEMP 테이블 사용 권한 취소하기
CONN SCOTT/tiger

REVOKE SELECT, INSERT ON TEMP FROM ORCLSTUDY;
```



## 3. 롤 관리



### 롤이란?

- 롤은 여러 종류의 권한을 묶어 놓은 그룹을 뜻함.
- 롤을 사용하면 여러 권한을 한 번에 부여하고 해제할 수 있으므로 권한 관리 효율을 높일 수 있음.

- 롤은 오라클 데이터베이스를 설치할 때 기본으로 제공되는 사전 정의된 롤(predefined roles)과 사용자 정의 롤(user roles)로 나뉨.



### 사전 정의된 롤



#### CONNECT 롤

- 사용자가 데이터베이스에 접속하는 데 필요한 CREATE SESSION 권한을 가지고 있음.
- 10g전에는 8가지 권한을 가지고 있었지만 10g부터는 CREATE SESSION 권한만 가지고 있음.



#### RESOURCE 롤

- 사용자가 테이블, 시퀀스를 비롯한 여러 객체를 생성할 수 있는 기본 시스템 권한을 묶어 놓은 롤임.
- 보통 새로운 사용자를 생성하면 CONNECT 롤과 RESOURCE 롤을 부여하는 경우가 많음.
- 뷰와 동의어 생성 권한은 따로 부여해 줘야 함.



#### DBA 롤

- 데이터베이스를 관리하는 시스템 권한을 대부분 가지고 있음.
- 오라클 11g 버전 기준 202개 권한을 가진 매우 강력한 롤임.



### 사용자 정의 롤

- 사용자 정의 롤은 필요에 의해 직접 권한을 포함시킨 롤을 뜻함.
  - CREATE ROLE문으로 롤을 생성.
  - GRANT 명령어로 생성한 롤에 권한을 포함시킴.
  - GRANT 명령어로 권한이 포함된 롤을 특정 사용자에게 부여함.
  - REVOKE 명령어로 롤을 취소시킴.



#### 롤 생성과 권한 포함

```SQL
-- SYSTEM 계정으로 ROLESTUDY 롤 생성 및 권한 부여하기
CONN SYSTEM/oracle

CREATE ROLE ROLESTUDY;

GRANT CONNECT, RESOURCE, CREATE VIEW, CREATE SYNONYM
   TO ROLESTUDY;
```

```SQL
-- ORCLSTUDY 사용자에게 ROLESTUDY 롤 부여하기
GRANT ROLESTUDY TO ORCLSTUDY;
```



#### 부여된 롤과 권한 확인

```SQL
-- ORCLSTUDY에 부여된 롤과 권한 확인하기
CONN ORCLSTUDY/ORACLE

SELECT * FROM USER_SYS_PRIVS;

SELECT * FROM USER_ROLE_PRIVS;
```



#### 부여된 롤 취소

```SQL
-- 부여된 롤 취소하기
REVOKE ROLESTUDY FROM ORCLSTUDY;
```



#### 롤 삭제

```SQL
-- 롤 삭제하기
DROP ROLE ROLESTUDY;
```

