# 모델1 방식의 회원제 게시판 만들기

# 프로젝트 구상

## 회원제 게시판의 프로세스

![회원제 게시판 프로세스](C:\Users\MIT-김재준\Downloads\Export-4a4d6276-b7d0-42bb-9bfb-12c53ecb7074\모델1 방식의 회원제 게시판 만들기 7af3ca6fa5084dbb99e07dd6539b0788\Untitled.png)

회원제 게시판 프로세스

- 비회원(로그아웃) 상태 : 목록 보기, 상세 보기
- 회원(로그인) 상태 : 글쓰기, 수정하기, 삭제하기

기능 처리 후 페이지 이동

- 글쓰기 후 : 목록으로 이동
- 수정 후 : 상세 보기로 이동
- 삭제 후 : 목록으로 이동

## 테이블 및 시퀀스 생성

![테이블 정의서](C:\Users\MIT-김재준\Downloads\Export-4a4d6276-b7d0-42bb-9bfb-12c53ecb7074\모델1 방식의 회원제 게시판 만들기 7af3ca6fa5084dbb99e07dd6539b0788\Untitled 1.png)

테이블 정의서

member 테이블은 회원정보를 저장한 테이블로, 회원인증을 위해 사용됨. 아이디 컬럼을 기본키로 지정함. 

board 테이블은 사용자가 입력한 게시물을 젖아하는 테이블. 기본키로 사용되는 일련번호 컬럼에는 시퀀스(sequence)를 통해 부여되는 순번을 입력함.

member 테이블의 id 컬럼과 board 테이블의 id 컬럼은 외래키로 엮여 있음. member 테이블에 없는 아이디로 게시물을 작성하려 하면 제약조건 위배로 에러날 것임.

# 모델1 구조와 모델2 구조(MVC 패턴)

## MVC 패턴

웹 애플리케이션은 사용자의 요청을 받아 처리한 후 응답하는 구조. MVC는 모델, 뷰, 컨트롤러의 약자. 소프트웨어를 개발하는 방법론. 데이터 처리를 담당하는 모델과 화면 출력을 담당하는 뷰, 이 둘을 제어하는 컨트롤러가 각자의 역할을 분담하여 사용자의 요청을 처리한 후 결과를 웹 브라우저에 출력하게 됨.

![MVC 패턴의 절차](C:\Users\MIT-김재준\Downloads\Export-4a4d6276-b7d0-42bb-9bfb-12c53ecb7074\모델1 방식의 회원제 게시판 만들기 7af3ca6fa5084dbb99e07dd6539b0788\Untitled 2.png)

MVC 패턴의 절차

- 모델 : 업무 처리 로직(비즈니스 로직) 혹은 데이터베이스와 관련된 작업을 담당함.
- 뷰 : JSP 페이지와 같이 사용자에게 보여지는 부분을 담당함.
- 컨트롤러 : 모델과 뷰를 제어하는 역할을 함. 사용자의 요청을 받아서 그 요청을 분석하고, 필요한 업무 처리 로직(모델)을 호출함. 모델이 결괏값을 반환하면 출력할 뷰(JSP 페이지)를 선택한 후 전달함.

## 모델1 구조와 모델2 구조

![모델 1의 구조](C:\Users\MIT-김재준\Downloads\Export-4a4d6276-b7d0-42bb-9bfb-12c53ecb7074\모델1 방식의 회원제 게시판 만들기 7af3ca6fa5084dbb99e07dd6539b0788\Untitled 3.png)

모델 1의 구조

모델 1 방식에서는 사용자의 요청을 JSP가 받아 모델을 호출함. 모델이 요청을 처리한 후 결과를 반환하면 JSP를 통해 응답을 하게 됨. JSP에 뷰와 컨트롤러가 혼재되어 있음.

모델 1 방식은 개발 속도가 빠르고 배우기 쉬움. 하지만 뷰와 컨트롤러 두 가지 기능 모두를 JSP에서 구현해야 하므로 코드가 복잡해지고 유지보수가 어려움.

![모델 2의 구조](C:\Users\MIT-김재준\Downloads\Export-4a4d6276-b7d0-42bb-9bfb-12c53ecb7074\모델1 방식의 회원제 게시판 만들기 7af3ca6fa5084dbb99e07dd6539b0788\Untitled 4.png)

모델 2의 구조

모델 2 방식은 MVC 패턴을 사용함. JSP와 서블릿의 장점을 모두 취합하여 JSP는 뷰로 사용하고, 서블릿은 컨트롤러로 사용함.

모델 2 방식에서는 사용자의 요청을 컨트롤러인 서블릿이 받음. 서블릿은 사용자의 요청을 분석한 후 모델을 호출함. 모델로부터 데이터를 받아 뷰로 전달하면 최종적으로 사용자는 요청에 대한 응답을 받을 수 있게 됨.

모델, 뷰, 컨트롤러가 각자의 역할을 수행하므로 업무 분담이 명확해지고 코드가 간결해지고 유지보수가 쉬워짐. 하지만 구조가 복잡하여 익숙하지 않다면 개발 기간이 길어질 수 있어 규모가 작은 프로젝트에서는 적합하지 않을 수도 있음.

# 목록 보기

게시판의 목록 페이지부터 제작. 페이지 개념 없이 전체 게시물을 한꺼번에 출력하는 형태로 제작. 페이지로 나누는 기능이 추가되면 코드가 다소 어려워지므로, 기본 기능들을 먼저 만든 후 추후에 페이지 기능 추가.

## DTO와 DAO 준비

```java
package model1.board; // 1번.

public class BoardDTO {
    // 2번. 멤버 변수 선언
    private String num;
    private String title;
    private String content;
    private String id;
    private java.sql.Date postdate;
    private String visitcount;
    private String name; // 3번.

    // 4번. 게터/세터
    public String getNum() {
        return num;
    }
    public void setNum(String num) {
        this.num = num;
    }
    public String getTitle() {
        return title;
    }
    public void setTitle(String title) {
        this.title = title;
    }
    public String getContent() {
        return content;
    }
    public void setContent(String content) {
        this.content = content;
    }
    public String getId() {
        return id;
    }
    public void setId(String id) {
        this.id = id;
    }
    public java.sql.Date getPostdate() {
        return postdate;
    }
    public void setPostdate(java.sql.Date postdate) {
        this.postdate = postdate;
    }
    public String getVisitcount() {
        return visitcount;
    }
    public void setVisitcount(String visitcount) {
        this.visitcount = visitcount;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}
```

1. 패키지는 model1.board로 선언함.
2. 멤버 변수는 board 테이블의 컬럼과 동일하게 작성함. 자바빈즈 규약에 따라 접근 지정자는 private으로 지정함. 자료형은 특별한 이유가 없다면 String으로 선언.
3. board 테이블에는 작성자의 아이디만 저장되므로 목록 출력 시 이름은 출력할 수 없음. 이름을 출력해야 한다면 member 테이블과 조인을 사용해야 함. 이때 name 컬럼을 사용하게 됨. 이와 같이 DTO는 필요한 경우 다른 테이블의 컬럼을 멤버 변수로 추가할 수 있음.
4. 멤버 변수를 선언하였다면 게터와 세터 메서드는 이클립스 메뉴를 이용해 자동으로 생성할 수 있음.

```java
package model1.board; // 1번.

import javax.servlet.ServletContext;

import common.JDBConnect;

public class BoardDAO extends JDBConnect { // 2번.
	public BoardDAO(ServletContext application) {
		super(application); // 3번.
	}
}
```

1. BoardDTO 클래스와 같은 패키지임.
2. 앞서 생성한 JDBConnect 클래스를 상속함.
3. 생성자에서는 부모 클래스의 생성자를 호출함. 부모 클래스인 JDBConnect에는 총 3개의 생성자를 정의하였는데, 그 중 application 내장 객체를 받는 생성자를 이용했음. 이 생성자는 매개 변수로 받은 application 내장 객체를 통해 web.xml에 정의해둔 오라클 접속 정보를 직접 가져와 DB에 연결해줌.

```java
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
```

```xml
<context-param>  <!-- 드라이버 이름 -->
    <param-name>OracleDriver</param-name>
    <param-value>oracle.jdbc.OracleDriver</param-value>
  </context-param>
```

## JSP 페이지 구현

목록에 출력할 게시물을 얻어오기 위한 메서드를 작성. BoardDAO 클래스에 메서드 2개를 추가할 것임. 

- `selectCount()` : board 테이블에 저장된 게시물의 개수를 반환함. 목록에서 번호를 출력하기 위해 사용됨.
- `selectList()` : board 테이블의 레코드를 가져와서 반환함. 이 메서드가 반환한 ResultSet 객체로부터 게시물 목록을 반복하여 출력하게 됨.

### 게시물 개수 세기

```java
package model1.board;

import java.sql.SQLException;
import java.util.Map; // 1번.

import javax.servlet.ServletContext;

import common.JDBConnect;

public class BoardDAO extends JDBConnect {
	public BoardDAO(ServletContext application) {
		super(application);
	}
	
	// 2번. 검색 조건에 맞는 게시물의 개수를 반환.
	public int selectCount(Map<String, Object> map) {
		int totalCount = 0; // 결과(게시물 수)를 담을 변수
		
		// 3번. 게시물 수를 얻어오는 쿼리문 작성
		String query = "SELECT COUNT(*) FROM board";
		if (map.get("searchWord") != null) { // 4번.
			query += " WHERE " + map.get("searchField") + " "
					+ " Like '%" + map.get("searchWord") + "%'";
		}
		
		try {
			stmt = con.createStatement();
			rs = stmt.executeQuery(query);
			rs.next();
			totalCount = rs.getInt(1);
		} catch (Exception e) {
			System.out.println("게시물 수를 구하는 중 예외 발생");
			e.printStackTrace();
		}
		
		return totalCount;
	}
}
```

1. selectCount() 코드에서 사용하는 외부 클래스를 임포트한 것임.
2. 게시물 개수를 알려주는 메서드를 정의함. 매개변수로 받은 Map<String, Object> 컬렉션에는 게시물 검색을 위한 조건(검색어)이 담겨 있음.
3. 게시물의 개수를 얻어오는 쿼리문을 작성함. 이때 SQL이 제공하는 COUNT(*) 함수를 사용함.
4. if문을 써서 검색어가 있는 경우, 즉 Map 컬렉션에 “searchWord” 키로 저장된 값이 있을 때만 WHERE절을 추가하도록 함.
   1. 조건이 없을 때의 쿼리문 : SELECT COUNT(*) FROM board;
   2. 조건이 있을 때의 쿼리문 : SELECT COUNT(*) FROM board WHERE title like ‘%홍길동%’;
5. JDBC 프로그래밍은 기본적으로 예외처리를 해야 해서 try/catch문으로 감쌌음.
6. 정적 쿼리문을 실행하기 위해 Statement 객체를 생성함.
7. 쿼리를 실행함. 결과는 ResultSet 객체로 반환함. ResultSet 객체는 행 단위로 저장되며, 현재 행을 가리키는 커서(cursor)를 통해 값을 읽어오는 구조임. 커서는 처음에 1행 바로 앞에 위치함.
8. next()로 커서를 1행으로 이동시킴.
9. 게시물 개수를 추출함. COUNT(*) 함수가 반환하는 값은 정수이므로 getInt()를 사용함. 이때 숫자 1은 SELECT절에 명시된 컬럼의 인덱스를 의미함. ResultSet이 가리키는 행+1번 컬럼의 값을 읽어서 totalCount에 넣는 것임.
10. 추출한 값을 반환하면 모든 처리가 끝남. 이 값이 JSP로 반환하게 됨.

### 게시물 목록 가져오기

```java
...생략...

public List<BoardDTO> selectList(Map<String, Object> map) { // 1번.
		List<BoardDTO> bbs = new Vector<BoardDTO>();
		// 2번. 결과(게시물 목록)를 담을 변수
		
		String query = "SELECT * FROM board "; // 3번.
		if (map.get("searchWord") != null) { // 4번.
			query += " WHERE " + map.get("searchField") + " " + "LIKE '%" + map.get("searchWord") + "%' ";
		}
		query += " ORDER BY num DESC "; // 5번.
		
		try {
			stmt = con.createStatement(); // 6번. 쿼리문 생성.
			rs = stmt.executeQuery(query); // 7번. 쿼리 실행.
			
			while (rs.next()) { // 8번. 결과를 순화하며...
				// 한 행(게시물 하나)의 내용을 DTO에 저장
				BoardDTO dto = new BoardDTO(); // 9번.
				
				dto.setNum(rs.getString("num"));
				dto.setTitle(rs.getString("title"));
				dto.setContent(rs.getString("content"));
				dto.setPostdate(rs.getDate("postdate"));
				dto.setId(rs.getString("id"));
				dto.setVisitcount(rs.getString("visitcount"));
				
				bbs.add(dto); // 10번. 결과 목록에 저장.
			}
		} catch (Exception e) {
			System.out.println("게시물 조회 중 예외 발생");
			e.printStackTrace();
		}
		return bbs; // 11번.
	}
```

1. 연결된 데이터베이스로부터 게시물 목록을 가져오는 메서드를 정의함. selectCount()와 똑같이 검색 조건을 매개변수로 받음.
2. 테이블에서 레코드를 가져올 때는 항상 List 계열의 컬렉션에 저장함. List 컬렉션에는 데이터가 순서대로 저장되어 인덱스를 통해 가져올 수 있기 때문임. 여기서는 Vector를 사용했지만 ArrayList나 LinkedList 등 List 계열의 컬렉션이라면 모두 사용할 수 있음.
3. 목록을 가져오기 위한 쿼리문 작성.
4. 검색어가 있다면 WHERE 절을 추가하는 부분은 selectCount() 메서드와 유사함.
5. 마지막에 게시물 정렬을 위한 ORDER BY 절이 추가됨. 게시판 목록은 항상 최근 게시물이 상단에 출력되므로 일련번호 컬럼 num을 내림차순(DESC)으로 정렬한 것임.
6. Statement 객체를 생성함.
7. query에 저장한 쿼리문을 실행함. 결과는 ResultSet 객체에 저장되어 반환됨.
8. next() 메서드로 ResultSet에 저장된 행이 있는지 확인함. 있다면 true를 반환하고, 커서를 다음 행으로 이동시킴. 최초에는 첫 번째 행으로 이동됨. while문과 함께 써서 더 이상 행이 존재하지 않을 때까지 괄호 안의 작업을 반복하게 됨.
9. while문 안에서는 하나의 행(게시물 하나)의 내용을 DTO 객체에 저장함.
10. 이 DTO를 List 컬렉션에 담음. while 루프를 끝까지 돌고 나면 쿼리문으로 받아온 게시물 목록이 모두 List 컬렉션인 bbs에 저장되게 됨.
11. 쿼리 결과를 모두 담은 List 컬렉션을 JSP로 반환함.

### 게시물 목록 출력하기

목록을 화면에 출력해주는 JSP를 만들 차례임. 코드가 길어서 2부분으로 나눠서 공부.

```java
<%@ page import="java.util.List"%>
<%@ page import="java.util.HashMap"%>
<%@ page import="java.util.Map"%>
<%@ page import="model1.board.BoardDAO"%>
<%@ page import="model1.board.BoardDTO"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 1번. DAO를 생성해 DB에 연결
BoardDAO dao = new BoardDAO(application);	

// 2번. 사용자가 입력한 검색 조건을 Map에 저장
Map<String, Object> param = new HashMap<String, Object>();
String searchField = request.getParameter("searchField");
String searchWord = request.getParameter("searchWord");
if (searchWord != null) {
	param.put("searchField", searchField);
	param.put("searchWord", searchWord);
}

int totalCount = dao.selectCount(param); // 3번. 게시물 수 확인.
List<BoardDTO> boardLists = dao.selectList(param); // 4번. 게시물 목록 받기.
dao.close(); // DB 연결 닫기.
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원제 게시판</title>
</head>
<body>
... 생략 ...
</body>
</html>
```

1. 가장 먼저 BoardDAO 객체를 생성함. DAO 생성 과정에서 DB와의 연결이 완료됨.
2. 사용자가 검색폼에서 입력한 내용을 Map 컬렉션에 저장함. DAO의 메서드를 호출할 때 이 컬렉션을 매개변수로 전달할 것임. 
3. DAO의 selectCount() 메서드 호출하여 게시물 개수 가져옴.
4. DAO의 selectList() 메서드 호출하여 게시물 목록 가져옴. 3번과 4번 모두 2번의 사용자 검색어 param을 매개변수로 전달했으므로 조건에 맞는 게시물을 찾아줌.

```html
...생략...
<body>
    <jsp:include page="../Common/Link.jsp" />  <!-- 공통 링크 -->

    <h2>목록 보기(List)</h2>
    <!-- 1번. 검색폼 --> 
    <form method="get">  <!-- 2번. -->
    <table border="1" width="90%">
    <tr>
        <td align="center">
            <select name="searchField">  <!-- 3번. -->
                <option value="title">제목</option> 
                <option value="content">내용</option>
            </select>
            <input type="text" name="searchWord" />
            <input type="submit" value="검색하기" />
        </td>
    </tr>   
    </table>
    </form>
    <!-- 4번. 게시물 목록 테이블(표) --> 
    <table border="1" width="90%">
        <!-- 5번. 각 칼럼의 이름 --> 
        <tr>
            <th width="10%">번호</th>
            <th width="50%">제목</th>
            <th width="15%">작성자</th>
            <th width="10%">조회수</th>
            <th width="15%">작성일</th>
        </tr>
        <!-- 6번. 목록의 내용 --> 
<%
if (boardLists.isEmpty()) {
    // 7번. 게시물이 하나도 없을 때 
%>
        <tr>
            <td colspan="5" align="center">
                등록된 게시물이 없습니다^^*
            </td>
        </tr>
<%
}
else {
    // 8번. 게시물이 있을 때 
    int virtualNum = 0;  // 화면상에서의 게시물 번호
    for (BoardDTO dto : boardLists)
    {
        virtualNum = totalCount--;  // 전체 게시물 수에서 시작해 1씩 감소
%>
        <tr align="center">
            <td><%= virtualNum %></td>  <!--게시물 번호-->
            <td align="left">  <!--제목(+ 하이퍼링크)-->
                <a href="View.jsp?num=<%= dto.getNum() %>"><%= dto.getTitle() %></a> // 9번.
            </td>
            <td align="center"><%= dto.getId() %></td>          <!--작성자 아이디-->
            <td align="center"><%= dto.getVisitcount() %></td>  <!--조회수-->
            <td align="center"><%= dto.getPostdate() %></td>    <!--작성일-->
        </tr>
<%
    }
}
%>
    </table>
    <!-- 10번. 목록 하단의 [글쓰기] 버튼-->
    <table border="1" width="90%">
        <tr align="right">
            <td><button type="button" onclick="location.href='Write.jsp';">글쓰기
                </button></td>
        </tr>
    </table>
</body>
</html>
```

1. 가장 위에는 검색폼이 옴.
2. 전송 방식은 get 방식임. action 속성을 지정하지 않았으므로 submit하면 폼값이 현재 페이지로 전송됨.
3. 검색 항목(searchField)은 제목과 내용 중 선택할 수 있음. 검색어가 없을 때와 있을 때를 구분하여 처리하게 됨.
4. 검색폼 아래에 검색 목록이 표 형태로 출력됨.
5. 표의 첫 줄은 각 컬럼의 이름이 옴.
6. 그 아래에 목록의 내용이 나옴.
7. List 컬렉션에 저장된 내용이 하나도 없다면 간략하게 “등록된 게시물이 없습니다^^*”라고 출력함.
8. 게시물이 있다면 for문을 돌면서 하나씩 출력함. 게시물 정보는 모두 DTO 객체로부터 얻음.
9. 특히 제목은 상세 보기 페이지로 이동하기 위한 링크가 추가됨. 상세 보기는 일련번호를 통해 게시물을 조회하므로 num을 매개변수로 전달하고 있음.
10. 화면의 맨 아래에는 글쓰기 버튼이 자리함.

### 게시물 검색하기

# 글쓰기

## 로그인 여부 확인

## 글쓰기 페이지 구현

## DAO에 글쓰기 메서드 추가

## 글쓰기 처리 페이지 작성

## 동작 확인

# 상세 보기

## DAO 준비

## 상세 보기 화면 작성

# 수정하기

## 수정 폼 화면 작성

## DAO 준비

## 수정 처리 페이지 작성

## 동작 확인

# 삭제하기

## 삭제하기 버튼에 삭제 요청 로직 달기

## DAO 준비

## 삭제 처리 페이지 작성

## 동작 확인