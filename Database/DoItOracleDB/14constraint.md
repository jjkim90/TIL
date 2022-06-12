# 제약 조건

- 제약 조건(constraint)은 테이블에 저장할 데이터를 제약하는 특수한 규칙을 뜻함.
- 제약 조건을 설정한 열에는 조건에 맞지 않는 데이터를 저장할 수 없음.
- DDL로 설정할 수 있음.



## 1. 제약 조건 종류



### 제약 조건이란?

- 테이블의 특정 열에 지정.
- 제약 조건을 지정한 열에 제약 조건에 부합하지 않는 데이터를 저장할 수 없음.
- 제약 조건 지정 방식에 따라 기존 데이터의 수정이나 삭제 가능 여부도 영향을 받음.
- 오라클 데이터베이스에 사용하는 제약 조건
  - NOT NULL : 지정한 열에 NULL을 허용하지 않음.
  - UNIQUE : 지정한 열이 유일한 값을 가져야 함. NULL은 값의 중복에서 제외됨.
  - PRIMARY KEY : 지정한 열이 유일한 값이면서 NULL을 허용하지 않음. 테이블에 하나만 지정 가능.
  - FOREIGN KEY : 다른 테이블의 열을 참조하여 존재하는 값만 입력할 수 있음.
  - CHECK : 설정한 조건식을 만족하는 데이터만 입력 가능.

- 제약 조건은 DDL 사용. 테이블 생성 시 주로 지정하고 생성 후 추가, 변경, 삭제 가능.



## 2. 빈값을 허락하지 않는 NOT NULL

- 특정 열에 데이터의 중복 여부와 상관없이 NULL의 저장을 허용하지 않는 제약 조건.
- 반드시 열에 값이 존재해야만 하는 경우에 지정.

```sql
// 테이블 생성할 때 NOT NULL 설정하기
CREATE TABLE TABLE_NOTNULL(
   LOGIN_ID VARCHAR2(20) NOT NULL,
   LOGIN_PWD VARCHAR2(20) NOT NULL,
   TEL VARCHAR2(20)
);
```

- 기존 데이터를 NULL로 수정하는 것도 불가능. 삭제도 불가능.



### 제약 조건 확인

- USER_CONSTRAINT 키워드 사용함.

```sql
// 제약 조건 살펴보기(SCOTT 계정)
SELECT OWNER, CONSTRAINT_NAME, CONSTRAINT_TYPE, TABLE_NAME
  FROM USER_CONSTRAINTS;
```

- 제약 조건 종류(CONSTRAINT_TYPE)
  - C : CHECK, NOT NULL
  - U : UNIQUE
  - P : PRIMARY KEY
  - R : FOREIGN KEY
- 제약 조건의 이름을 따로 지정하지 않을 경우 오라클이 자동으로 지정함.



### 제약 조건 이름 직접 지정

- CONSTRAINT 키워드를 사용.

```sql
// 테이블을 생성할 때 제약 조건에 이름 지정하기
CREATE TABLE TABLE_NOTNULL2(
   LOGIN_ID VARCHAR2(20) CONSTRAINT TBLNN2_LGNID_NN NOT NULL,
   LOGIN_PWD VARCHAR2(20) CONSTRAINT TBLNN2_LGNPW_NN NOT NULL,
   TEL VARCHAR2(20)
);
```

- 오라클이 자동으로 지정해 주는 이름은 제약 조건이 많아진 후 찾기 어려워질 수 있으므로 실무에서는 이름 붙이는 규칙을 정하여 제약 조건 이름을 직접 지정하는 경우가 많음.



### 이미 생성한 테이블에 제약 조건 지정

#### 생성한 테이블에 제약 조건 추가하기

- NOT NULL 제약 조건의 추가는 ALTER 명령어와 MODIFY 키워드 사용함.

```sql
// TEL 열에 NOT NULL 제약 조건 추가하기
ALTER TABLE TABLE_NOTNULL
MODIFY(TEL NOT NULL);
```

- 제약 조건 추가를 위한 명령어를 잘 작성했어도 제약 조건과 맞지 않는 데이터가 이미 있다면 제약 조건 지정에 실패함.



#### 생성한 테이블에 제약 조건 이름 직접 지정해서 추가하기

- CONSTRAINT 키워드 사용함.

```sql
// 제약 조건에 이름 지정해서 추가하기
ALTER TABLE TABLE_NOTNULL2
MODIFY(TEL CONSTRAINT TBLNN_TEL_NN NOT NULL);
```



#### 생성한 제약 조건의 이름 변경하기

- ALTER 명령어에 RENAME CONSTRAINT 키워드 사용.

```sql
// 이미 생성된 제약 조건 이름 변경하기
ALTER TABLE TABLE_NOTNULL2
RENAME CONSTRAINT TBLNN_TEL_NN TO TBLNN2_TEL_NN;
```



### 제약 조건 삭제

- ALTER 명령어에 DROP CONSTRAINT 키워드 사용.

```sql
// 제약 조건 삭제하기
ALTER TABLE TABLE_NOTNULL2
 DROP CONSTRAINT TBLNN2_TEL_NN;
```



## 3. 중복되지 않는 값 UNIQUE

- 열에 저장할 데이터의 중복을 허용하지 않고자 할 때 사용함.
- NULL은 값이 존재하지 않음을 의미하기 때문에 중복 대상에서는 제외됨.



### 테이블을 생성하며 제약 조건 지정

- CREATE문으로 테이블을 생성할 때 지정할 수 있음.

```sql
// 제약 조건 지정하기(테이블을 생성할 때)
CREATE TABLE TABLE_UNIQUE(
   LOGIN_ID VARCHAR2(20) UNIQUE,
   LOGIN_PWD VARCHAR2(20) NOT NULL,
   TEL VARCHAR2(20)
);
```



### 제약 조건 확인

- USER_CONSTRAINTS 데이터 사전에서 CONSTRAINT_TYPE 열 값이 U일 경우에 UNIQUE 제약 조건을 의미함.

```sql
/* USER_CONSTRAINTS 데이터 사전 뷰로 제약 조건 확인하기 */
SELECT OWNER, CONSTRAINT_NAME, CONSTRAINT_TYPE, TABLE_NAME
  FROM USER_CONSTRAINTS
 WHERE TABLE_NAME = 'TABLE_UNIQUE';
```



### 테이블을 생성하며 제약 조건 이름 직접 지정

- 직접 이름 지정하지 않으면 오라클이 자동으로 제약 조건 이름을 지정해줌.

```sql
// 테이블을 생성할 때 UNIQUE 제약 조건 설정하기
CREATE TABLE TABLE_UNIQUE2(
   LOGIN_ID VARCHAR2(20) CONSTRAINT TBLUNQ2_LGNID_UNQ UNIQUE,
   LOGIN_PWD VARCHAR2(20) CONSTRAINT TBLUNQ2_LGNPW_NN NOT NULL,
   TEL VARCHAR2(20)
);
```



### 이미 생성한 테이블에 제약 조건 지정

- ALTER 명령어 사용함.



#### 생성한 테이블에 제약 조건 추가하기

- ALTER 명령어에 MODIFY 키워드 사용.

```sql
// 이미 생성한 테이블 열에 UNIQUE 제약 조건 추가하기
ALTER TABLE TABLE_UNIQUE
MODIFY(TEL UNIQUE);
```

- 제약 조건을 추가할 때 해당 열에 추가하려는 제약 조건에 맞지 않는 데이터가 존재할 경우 제약 조건 추가에 실패함.



#### 생성한 테이블에 제약 조건 이름 직접 지정하거나 바꾸기

```sql
-- UNIQUE 제약 조건 이름 직접 지정하기
ALTER TABLE TABLE_UNIQUE2
MODIFY(TEL CONSTRAINT TBLUNQ_TEL_UNQ UNIQUE);
```

```sql
-- 이미 만들어져 있는 UNIQUE 제약 조건 이름 수정하기
ALTER TABLE TABLE_UNIQUE2
RENAME CONSTRAINT TBLUNQ_TEL_UNQ TO TBLUNQ2_TEL_UNQ;
```



### 제약 조건 삭제

- ALTER 명령어에 DROP 키워드 사용.

```sql
-- 제약 조건 삭제하기
ALTER TABLE TABLE_UNIQUE2
 DROP CONSTRAINT TBLUNQ2_TEL_UNQ;
```



## 4. 유일하게 하나만 있는 값 PRIMARY KEY

- UNIQUE와 NOT NULL 제약 조건의 특성을 모두 가지는 제약 조건.
- 데이터 중복을 허용하지 않으면서 NULL도 허용하지 않음.
- 주민등록번호나 사원 번호같이 테이블의 각 행을 식별하는 데 활용됨.
  - PRIMARY KEY로 적합한 특성을 가졌다 할지라도 주민등록번호와 같은 예민한 개인 정보를 의미하는 데이터는 PRIMARY KEY로 지정하지 않음.
- PRIMARY KEY 제약 조건은 테이블에 하나밖에 지정할 수 없음.
- 특정 열을 PRIMARY KEY로 지정하면 해당 열에는 자동으로 인덱스가 만들어짐.



### 테이블을 생성하며 제약 조건 지정하기

- CREATE문으로 테이블을 생성하면서 지정 가능.

```sql
-- 테이블을 생성할 때 특정 열에 PRIMARY KEY 설정하기
CREATE TABLE TABLE_PK(
   LOGIN_ID VARCHAR2(20) PRIMARY KEY,
   LOGIN_PWD VARCHAR2(20) NOT NULL,
   TEL VARCHAR2(20)
);
```

```sql
-- 생성한 PRIMARY KEY 확인하기
SELECT OWNER, CONSTRAINT_NAME, CONSTRAINT_TYPE, TABLE_NAME
  FROM USER_CONSTRAINTS
 WHERE TABLE_NAME LIKE 'TABLE_PK%';
```

```sql
-- 생성한 PRIMARY KEY를 통해 자동 생성된 INDEX 확인하기
SELECT INDEX_NAME, TABLE_OWNER, TABLE_NAME
  FROM USER_INDEXES
 WHERE TABLE_NAME LIKE 'TABLE_PK%';
```



### 테이블을 생성하며 제약 조건 이름 직접 지정하기

```sql
-- 제약 조건의 이름을 직접 지정하여 테이블 생성하기
CREATE TABLE TABLE_PK2(
   LOGIN_ID VARCHAR2(20) CONSTRAINT TBLPK2_LGNID_PK PRIMARY KEY,
   LOGIN_PWD VARCHAR2(20) CONSTRAINT TBLPK2_LGNPW_NN NOT NULL,
   TEL VARCHAR2(20)
);
```

- 인덱스 이름도 똑같은 이름으로 지정됨.

- ALTER문의 MODIFY, RENAME, DROP을 통해 추가, 수정, 이름 변경, 삭제 등의 수행이 가능함.



### CREATE문에서 제약 조건을 지정하는 다른 방식

- CREATE문을 통해 제약 조건을 지정할 때 열 바로 옆에 제약 조건을 지정하는 형식을 인라인(inline) 또는 열 레벨(column-level) 제약 조건 정의라고 함.
- 열을 정의한 후에 별도로 제약 조건을 정의할 수 있음. 열을 명시한 후 제약 조건을 테이블 단위에 지정하는 방식을 아웃오브라인(out-of-line) 또는 테이블 레벨(table-level) 제약 조건 정의라고 함. 이 방식은 NOT NULL 제약 조건을 제외한 제약 조건 지정이 가능한 방식임.

```SQL
-- 아웃오브라인 또는 테이블 레벨 제약 조건 정의
CREATE TABLE TABLE_NAME(
	COL1 VARCHAR2(20),
	COL2 VARCHAR2(20),
	PRIMARY KEY (COL1),
	CONSTRAINT CONSTRAINT_NAME UNIQUE (COL2)
);
```



## 5. 다른 테이블과 관계를 맺는 FOREIGN KEY

- 외래키, 외부키로도 부르는 FOREIGN KEY는 서로 다른 테이블 간 관계를 정의하는 데 사용하는 제약 조건.
- 특정 테이블에서 PRIMARY KEY 제약 조건을 지정한 열을 다른 테이블의 특정 열에서 참조하겠다는 의미로 지정할 수 있음.
- 참조 대상 테이블을 부모, 참조하는 테이블을 자식으로 표현함.
- FOREIGN KEY가 참조하는 열에 존재하지 않는 데이터를 입력할 경우 '부모 키가 없습니다' 오류 메시지 출력됨.



### FOREIGN KEY 지정하기

```SQL
-- FOREIGN KEY 지정 기본 형식
CREATE TABLE 테이블이름(
...(다른 열 정의),
열이름 자료형 CONSTRAINT 제약조건이름 REFERENCES 참조테이블 (참조할열이름)
);
```



### FOREIGN KEY로 참조 행 데이터 삭제하기

- 참조 대상 데이터는 삭제할 수 없음. 삭제하려고 하면 오류 발생함.
- 데이터를 삭제하려면
  - 현재 삭제하려는 열 값을 참조하는 데이터를 먼저 삭제하기.
  - 현재 삭제하려는 열 값을 참조하는 데이터 수정하기.
  - 현재 삭제하려는 열을 참조하는 자식 테이블의 FOREIGN KEY 제약 조건을 해제하기.
- 열 데이터를 삭제할 때 이 데이터를 참조하고 있는 데이터도 함께 삭제할 수 있음.

```SQL
CONSTRAINT [제약조건이름] REFERENCES 참조테이블 (참조할열) ON DELETE CASCADE
```

- DEPT_FK 테이블의 DEPTNO 열 값이 10인 데이터를 삭제하면 이를 참조하는 EMP_FK 테이블의 DEPTNO 열 값이 10인 데이터도 함께 삭제됨.
- 열 데이터를 삭제할 때 이 데이터를 참조하는 데이터를 NULL로 수정할 수 있음.

```
CONSTRAINT [제약조건이름] REFERENCES 참조테이블 (참조열) ON DELETE SET NULL
```



## 6. 데이터 형태와 범위를 정하는 CHECK

- 열에 저장할 수 있는 값의 범위 또는 패턴을 정의할 때 사용함.

```SQL
-- 테이블을 생성할 때 CHECK 제약 조건 설정하기
CREATE TABLE TABLE_CHECK(
   LOGIN_ID VARCHAR2(20) CONSTRAINT TBLCK_LOGINID_PK PRIMARY KEY,
   LOGIN_PWD VARCHAR2(20) CONSTRAINT TBLCK_LOGINPW_CK CHECK (LENGTH(LOGIN_PWD) > 3),
   TEL VARCHAR2(20)
);
```

- 비밀번호는 3글자 이상만 저장할 수 있도록 제한을 둔 것.
- CHECK 제약 조건은 단순 연산뿐만 아니라 함수 활용도 가능함.



## 7. 기본값을 정하는 DEFAULT

- 제약 조건과는 별개로 특정 열에 저장할 값이 지정되지 않았을 경우에 기본값을 지정할 수 있음.
- DEFAULT 키워드 사용함.

```
-- 테이블을 생성할 때 DEFAULT 제약 조건 설정하기
CREATE TABLE TABLE_DEFAULT(
   LOGIN_ID VARCHAR2(20) CONSTRAINT TBLCK2_LOGINID_PK PRIMARY KEY,
   LOGIN_PWD VARCHAR2(20) DEFAULT '1234',
   TEL VARCHAR2(20)
);
```

- 데이터 입력 시 명시적으로 값을 지정하지 않으면 기본값이 들어감.



### 제약 조건 비활성화, 활성화

- 신규 개발 또는 테스트 시 제약 조건을 비활성화, 활성화하는 경우가 많음.

```SQL
-- 제약 조건 비활성화
ALTER TABLE 테이블이름
DISABLE [NOVALIDATE / VALIDATE] CONSTRAINT 제약조건이름;
```

```SQL
-- 제약 조건 활성화
ALTER TABLE 테이블이름
ENABLE [NOVALIDATE / VALIDATE] CONSTRAINT 제약조건이름;
```



