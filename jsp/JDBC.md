# 데이터베이스

---

# 데이터베이스란?

JSP에서는 JDBC(Java Database Connectivity)를 통해 데이터베이스와 연동함.

# 오라클 설치

오라클 설치 중간에 포트(Port)를 지정하는 화면이 나올 때가 있음. 오라클이 사용할 기본 포트를 다른 프로그램이 이미 사용하고 있어 포트가 충돌한다는 의미임. 포트 번호를 다르게 지정한 후 설치를 계속할 수 있음. 오라클이 사용하는 포트는 1521이고, 오라클 설치 시 함께 설치되는 HTTP 서버는 8080 포트를 사용함.

# 사용자 계정 생성 및 권한 설정



사용자 계정 생성 및 권한 설정

1. 명령 프롬프트 실행.
2. `sqlplus` 명령 실행.
3. user-name에는 “system”, password에는 설치 시 입력한 패스워드 입력. (oracle)
4. `create user 유저아이디 identified by 유저비밀번호;` 명령 실행.
5. `grant connect, resource to 유저아이디;` 명령 실행. 기본적인 접속 권한과 객체 생성 권한 부여.
6. `conn 유저아이디/유저비밀번호;` 새로 생성한 계정으로 오라클 접속.
7. `select * from tab;` 테이블 목록 확인. tab은 현재 접속한 계정에 생성된 테이블들의 목록을 확인할 수 있는 읽기 전용 테이블임. 새로 생성한 계정이므로 아직은 테이블이 없음.

역할(Role)

- 역할은 사용자가 다양한 권한을 효율적으로 관리할 수 있도록 관련된 권한끼리 묶어놓은 개념.
- DB 관리자(DBA) : 시스템 관리에 필요한 모든 권한을 부여함. 전체를 관리할 수 있는 권한이므로 특별한 경우에만 부여함.
- 접속(connect) : DB 접속에 필요한 가장 기본적인 시스템 권한 8가지를 묶은 권한임.
- 객체 생성(resource) : 객체(테이블, 뷰, 인덱스) 생성에 필요한 시스템 권한을 묶은 권한임.

데이터 사전(DD: Data Dictionary)

- 오라클에서 읽기전용으로 제공되는 테이블이나 뷰들의 집합.
- 데이터베이스 전반에 대한 정보를 제공함.
- 오라클은 명령이 실행될 때마다 데이터 사전을 액세스하여 구조, 권한, 데이터의 변경사항 등을 확인하거나 반영함.
- tab도 데이터 사전의 일종임.
- 데이터 사전의 테이블들
  - USER_SEQUENCES : 시퀀스의 정보 조회
  - USER_INDEXES : 인덱스의 정보 조회
  - USER_VIEWS : 뷰의 정보 조회
  - USER_CONSTRAINTS : 제약조건에 대한 정보 조회

# 테이블 및 시퀀스 생성

회원 정보를 저장할 수 있는 member 테이블과 게시글을 저장할 board 테이블 각각 생성.

## 테이블 생성

실제 서비스에서는 더 다양한 정보가 필요하지만 학습용으로 간단하게 아이디, 패스워드, 이름, 가입 날짜만 이용.

| 컬럼명   | 데이터 타입  | null 허용 | 키     | 기본값  | 설명      |
| -------- | ------------ | --------- | ------ | ------- | --------- |
| id       | varchar2(10) | N         | 기본키 |         | 아이디    |
| pass     | varchar2(10) | N         |        |         | 패스워드  |
| name     | varchar2(30) | N         |        |         | 이름      |
| regidate | date         | N         |        | sysdate | 가입 날짜 |

- member 테이블 정의. id 최대 10글자 기본키로 생성. varchar2는 사용한 만큼만 할당하는 가변 타입.

```sql
create table member (
    id varchar2(10) not null,
    pass varchar2(10) not null,
    name varchar2(30) not null,
    regidate date default sysdate not null,
    primary key (id)
);
```

- 잘 입력되었는지 확인하려면 `desc member` 명령 실행.

| 컬럼명     | 데이터 타입    | null 허용 | 키     | 기본값  | 설명                                                  |
| ---------- | -------------- | --------- | ------ | ------- | ----------------------------------------------------- |
| num        | number         | N         | 기본키 |         | 일련번호, 기본키                                      |
| title      | varchar2(200)  | N         |        |         | 게시물의 제목                                         |
| content    | varchar2(2000) | N         |        |         | 내용                                                  |
| id         | varchar2(10)   | N         | 외래키 |         | 작성자의 아이디. member 테이블의 id를 참조하는 외래키 |
| postdate   | date           | N         |        | sysdate | 작성일                                                |
| visitcount | number(6)      | Y         |        |         | 조회수                                                |

- board 테이블 정의.
- number : 전체 자릿수를 지정하지 않은 상태로 컬럼을 생성하면, 입력한 값만큼 공간이 자동으로 할당됨.
- number(6) : 소수점을 포함한 전체 자릿수를 6으로 지정함.
- member 테이블에서 기본키로 사용 중인 id 컬럼을 외래키로 지정하여, 각각의 게시글을 특정 회원과 연결지음. 이렇게 하면 회원이 아닌 사람은 글을 게시할 수 없도록 DBMS가 보장함.

```sql
create table board (
    num number primary key,
    title varchar2(200) not null,
    content varchar2(2000) not null,
    id varchar2(10) not null,
    postdate date default sysdate not null,
    visitcount number(6)
);
```

## 외래키로 테이블 사이의 관계 설정

외래키는 테이블 생성 후 별도 명령을 사용해 지정함. 테이블들의 관계는 동적으로 언제든 재정립될 수 있음.

외래키는 alter table 명령으로 설정함. 외래키 생성 시 제약조건의 이름을 지정할 수 있음. 필수는 아니지만, 제약조건 위배로 오류 발생 시 제약조건 이름이 출력되므로 어떤 조건을 위배했는지를 더 쉽게 식별할 수 있음.

```sql
alter table board
		add constraint board_mem_fk foreign key (id)
    references member (id);
```

## 일련번호용 시퀀스 객체 생성

시퀀스는 순차적으로 증가하는 순번을 반환하는 데이터베이스 객체임. board 테이블의 일련번호 컬럼에서 이 시퀀스를 사용함.

```sql
create sequence seq_board_num
		increment by 1 // 1씩 증가
		start with 1 // 시작값 1
		minvalue 1 // 최솟값 1
		nomaxvalue // 최댓값은 무한대
		nocycle // 순환하지 않음
		nocache // 캐시 안 함
```

- 1부터 무한대까지 1씩 증가하는 시퀀스를 생성함.
- nocycle 대신 cycle로 설정하면 최댓값까지 도달했을 때, 그 다음은 최솟값부터 다시 시작함.
- nocache 대신 cache로 설정하면 메모리에 시퀀스 값을 미리 할당해둠.

## 동작 확인

테스트를 위해 입력하는 가짜 데이터를 더미 데이터(dummy data)라고 함.



더미 데이터 입력(에러 발생)

외래키를 이용해 board 테이블의 id 컬럼이 member 테이블의 id 컬럼을 참조하는 제약조건을 설정했음. 그런데 member 테이블에는 아직 참조가 될 부모 레코드가 없어서 오류가 발생한 것임. 따라서 먼저 member 테이블에 데이터를 채워 넣어야 함.

```sql
insert into member (id, pass, name) values ('musthave', '1234', '머스트해브');
insert into board  (num, title, content, id, postdate, visitcount) 
	values (seq_board_num.nextval, '제목1입니다', '내용1입니다', 'musthave', sysdate, 0);
commit;
```

멤버에 먼저 데이터를 넣고 보드에 넣었기 때문에 오류 없이 잘 삽입됨.

마지막에는 커밋(commit)을 해야 함. 처음 레코드를 입력하면 오라클은 입력된 레코드를 내부의 임시 테이블에 저장함. 따라서 오라클 내부에서는 레코드를 조회할 수 있으나, 외부에서는 입력된 레코드를 조회할 수 없는 상태임. 커밋이 완료되면 임시 테이블에 저장됐던 레코드가 실제 테이블에 적용되어 오라클 외부에서도 조회할 수 있게 됨.

# JDBC 설정 및 데이터베이스 연결

JDBC란 자바로 데이터베이스 연결 및 관련 작업을 할 때 사용하는 API임. JDBC API를 사용하기 위해서는 JDBC 드라이버가 있어야 함. 각 DBMS에 맞는 JDBC 드라이버를 다운로드한 후 설정하면 DBMS 종류에 상관없이 동일한 방식으로 프로그래밍할 수 있게 됨.

## 이클립스와 오라클 연동

윈도우의 명령 프롬프트에서 sqlplus로 명령 실행하는 대신, 이클립스에서 sql 명령을 실행할 수 있음.

이클립스에서 Open Perspective → Database Development → Database Connections 우클릭 New → Oracle 선택 Next → New Driver Definition (바퀴 모양) 클릭 → Name에서 Oracle Thin Driver 11 선택 → JAR List ojdbc14 삭제 후 C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib 에 있는 ojdbc6.jar 추가 후 OK → Properties에서 Server Name : xe → Host : Localhost → port number 1521 → Username : musthave → password 1234 → save password 체크 → Test Connection 클릭해서 Success 메시지 확인 → Finish



컨텍스트 패스 우클릭 New → SQL File “board”생성 → Type, Name, Database 아래와 같이 설정 후 명령어 입력 → 명령어 라인 마우스로 블록 설정 → Alt+C 키보드 누르기 → 하단의 SQL Results에 결과 화면 출력됨.



## JDBC 드라이버 설정

C:\oraclexe\app\oracle\product\11.2.0\server\jdbc\lib 경로에 확장자가 jar인 파일들이 보일 텐데, 그중 ojdbc6.jar 파일이 바로 오라클 JDBC 드라이버임.

드라이버 파일을 프로젝트와 연결하는 방법

- WAS(톰캣)가 설치된 경로 하위의 lib 폴더에 추가하는 방법.
  - 이 경우 한 번의 설정으로 해당 컴퓨터에서 실행되는 모든 웹 애플리케이션에 적용됨. 편리해 보이지만 차후 웹 애플리케이션 배포 파일에 드라이버가 포함되지 않으므로 별도의 설정을 더 해줘야 한다는 단점이 있음.
- 개별 프로젝트의 WEB-INF 하위의 lib 폴더에 추가하는 방법.
  - 프로젝트마다 동일한 설정을 반복해야 하므로 처음에는 번거롭지만 작업 공간을 변경하거나 배포 시에도 드라이버가 함께 따라 간다는 편리함 덕분에 실무에서 주로 사용되는 방법임.

윈도우 탐색기에서 ojdbc6.jar 파일을 이클립스의 {프로젝트 루트}/WebContent/WEB-INF/lib 폴더로 드래그앤드롭하면 JDBC 드라이버 설정이 완료됨.

## 연결 관리 클래스 작성

{프로젝트 루트}/Java Resources/src에서 마우스 우클릭 → New → Class를 클릭하여 common 패키지에 JDBConnect 클래스 연결.

```java
package common;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;

import javax.servlet.ServletContext;

public class JDBConnect {
		// 멤버 변수 선언
    public Connection con;
    public Statement stmt;  
    public PreparedStatement psmt;  
    public ResultSet rs;

    // 기본 생성자
    public JDBConnect() {
        try {
            // JDBC 드라이버 로드
            Class.forName("oracle.jdbc.OracleDriver");

            // DB에 연결
            String url = "jdbc:oracle:thin:@localhost:1521:xe";  
            String id = "musthave";
            String pwd = "1234"; 
            con = DriverManager.getConnection(url, id, pwd); 

            System.out.println("DB 연결 성공(기본 생성자)");
        }
        catch (Exception e) {            
            e.printStackTrace();
        }
    }

    // 연결 해제(자원 반납)
    public void close() { 
        try {            
            if (rs != null) rs.close(); 
            if (stmt != null) stmt.close();
            if (psmt != null) psmt.close();
            if (con != null) con.close(); 

            System.out.println("JDBC 자원 해제");
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

- 멤버 변수 선언
  - Connection : 데이터베이스와 연결을 담당함.
  - Statement : 인파라미터가 없는 정적 쿼리문을 실행할 때 사용됨.
  - PreparedStatement : 인파라미터가 있는 동적 쿼리문을 실행할 때 사용됨. 인파라미터는 쿼리문 작성 시 매개변수로 전달된 값을 설정할 때 사용함. 물음표(?)로 표현함.
  - ResultSet : SELECT 쿼리문의 결과를 저장할 때 사용됨.
- 기본 생성자
  - 생성자는 JDBC 드라이버를 이용해 오라클 DB에 연결하는 두 가지 일을 함.
  - JDBC 드라이버 로드
    - JDBC 드라이버를 메모리에 로드함.
    - Class 클래스의 forName()은 new 키워드 대신 클래스명을 통해 직접 객체를 생성한 후 메모리에 로드하는 메소드임.
    - 인수로는 오라클 드라이버 이름을 넣으면 됨.
    - JDBC 드라이버로 사용될 클래스는 DB 종류에 따라 다르므로 오라클이 아니라면 클래스명도 다르게 지정해야 함.
  - DB에 연결
    - DB에 연결하려면 URL, ID, 패스워드가 필요함.
    - URL은 “오라클 프로토콜@IP주소:포트번호:sid” 형식으로 구성됨.
    - 프로토콜은 이미 정해져 있는 값이므로 그대로 사용하면 되고, IP 주소와 포트번호, sid는 설치 환경에 따라 달라짐. 여기서는 localhost, 1521, xe를 사용함. (Express 버전은 xe, Enterprise 버전은 orcl)
    - ID와 패스워드는 musthave/1234
    - 실제 오라클에 연결하기. 준비한 URL, ID, 패스워드를 인수로 DriverManager 클래스의 getConnection()을 호출하면 됨. 성공적으로 연결되었다면 Connection 객체가 반환됨. 여기서 구한 Connection 객체를 통해 오라클에 연결하는 것임.
  - 연결 해제(자원 반납)
    - close() 메소드
    - DB 관련 작업을 모두 마쳤다면 자원을 절약하기 위해 연결을 해제해주는 게 좋음.
    - 멤버 변수로 선언된 객체 각각을 닫아줌.
    - if문을 이용해 사용된 적이 있는 객체들만 자원을 해제함.

## 동작 확인

```java
<%@ page import="common.JDBConnect"%> 
<%@ page import="common.DBConnPool"%> 
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>JDBC</title></head>
<body>
    <h2>JDBC 테스트 1</h2>
    <%
    JDBConnect jdbc1 = new JDBConnect(); 
    jdbc1.close(); 
    %>
</body>
</html>
```

- JDBConnect 클래스를 임포트.
- 객체 생성
- 바로 닫아 자원을 해제함.
- DB 연결은 객체 생성자에서 이루어짐.



이클립스의 콘솔창을 확인해보면 DB에 연결되었다가 자원을 해제하며 접속이 종료된 것을 알 수 있음. 웹 브라우저에서 확인할 건 따로 없음.

## 연결 설정 개선

DB 접속 정보를 클래스 안에서 모두 입력하는 방식은 실무에서 사용하지 않음. 만약 서버 이전 등의 이유로 접속 정보가 변경되는 경우 클래스를 수정한 후 다시 컴파일해야 하는 불편함이 있기 때문임.

서버 환경과 관련된 정보들은 한 곳에서 관리하는 것이 좋음. 주로 web.xml에 입력해놓고 필요할 때마다 application 내장 객체를 통해 얻어옴.

오라클 접속 정보를 web.xml에 컨텍스트 초기화 매개변수(<context-param>)로 입력하기.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi= ... 생략 ...>
... 생략 ...

  <context-param>
    <param-name>OracleDriver</param-name>
    <param-value>oracle.jdbc.OracleDriver</param-value>
  </context-param>
  <context-param>
    <param-name>OracleURL</param-name>
    <param-value>jdbc:oracle:thin:@localhost:1521:xe</param-value>
  </context-param>
  <context-param>
    <param-name>OracleId</param-name>
    <param-value>musthave</param-value>
  </context-param>
  <context-param>
    <param-name>OraclePwd</param-name>
    <param-value>1234</param-value>
  </context-param>
</web-app>
```

- 오라클에 접속하기 위한 드라이버명, 접속URL, 계정 아이디, 패스워드를 각각 입력함.

다음으로 접속 정보를 외부로부터 받는 생성자를 JDBConnect 클래스에 추가함.

```java
... 생략 ...
// 두 번째 생성자
    public JDBConnect(String driver, String url, String id, String pwd) {
        try {
            // JDBC 드라이버 로드
            Class.forName(driver);  

            // DB에 연결
            con = DriverManager.getConnection(url, id, pwd);

            System.out.println("DB 연결 성공(인수 생성자 1)");
        }
        catch (Exception e) {            
            e.printStackTrace();
        }
    }
```

- 하드코딩했던 값들을 모두 외부에서 전달받도록 함.
- 나중에 web.xml에서 정보를 읽어와 이 생성자를 호출할 것임.
- 이 생성자는 전달받은 값으로 드라이버를 로드하고 데이터베이스에 연결함.
- 하드코딩했던 접속 정보가 모두 제거되어 코드가 훨씬 간결해짐.

이어서 접속 정보를 web.xml로부터 읽어 새로운 생성자를 호출하는 코드를 ConnectionTest.jsp에 추가함.

```java
... 생략 ...

<h2>JDBC 테스트 2</h2>
    <%
    String driver = application.getInitParameter("OracleDriver"); 
    String url = application.getInitParameter("OracleURL");
    String id = application.getInitParameter("OracleId");
    String pwd = application.getInitParameter("OraclePwd");

    JDBConnect jdbc2 = new JDBConnect(driver, url, id, pwd); 
    jdbc2.close();
    %>
```

- application 내장 객체의 getInitParameter()로 web.xml의 컨텍스트 초기화 매개변수를 가져올 수 있음.
- 이렇게 가져온 네 가지 설정값을 새로운 생성자에 전달함.



## 연결 설정 개선 2

접속 정보를 web.xml에 입력한 후 내장 객체를 통해 가져오는 방식은 DB 접속이 필요할 때마다 동일한 코드를 JSP에서 반복해서 기술해야 함. 컨텍스트 초기화 매개변수를 생성자에서 직접 가져올 수 있도록 정의하는 것이 좋음.

```java
import javax.servlet.ServletContext;

... 생략 ...
// 세 번째 생성자
    public JDBConnect(ServletContext application) {
        try {
            // JDBC 드라이버 로드
            String driver = application.getInitParameter("OracleDriver"); 
            Class.forName(driver); 

            // DB에 연결
            String url = application.getInitParameter("OracleURL"); 
            String id = application.getInitParameter("OracleId");
            String pwd = application.getInitParameter("OraclePwd");
            con = DriverManager.getConnection(url, id, pwd);

            System.out.println("DB 연결 성공(인수 생성자 2)"); 
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }
... 생략 ...
```

- 세 번째 생성자는 매개변수로 application 내장 객체를 받도록 정의함.
- application 내장 객체를 이용해 web.xml로부터 접속 정보를 직접 가져온다는 점만 빼면 기존 생성자와 동일함.
- application 내장 객체의 타입인 ServletContext를 사용할 수 있도록 임포트함.

세 번째 생성자를 사용하는 코드를 JSP 페이지에 추가함.

```java
... 생략 ...

<h2>JDBC 테스트 3</h2>
    <%
    JDBConnect jdbc3 = new JDBConnect(application); 
    jdbc3.close();
    %>
</body>
</html>
```

- application 내장 객체를 인수로 전달함.
- 이 방식은 DB에 접속할 때마다 JSP에서 컨텍스트 초기화 매개변수를 읽어오지 않아도 되므로 편리하며, 코드 중복도 줄일 수 있음.



# 커넥션 풀로 성능 개선

# 간단한 쿼리 작성 및 실행

# 레퍼런스

- [성낙현의 JSP 자바 웹 프로그래밍](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791191905052&orderClick=LEa&Kc=)