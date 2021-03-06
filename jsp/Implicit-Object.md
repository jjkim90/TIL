# 내장 객체

<br>

> 이 문서는 [성낙현의 JSP 자바 웹 프로그래밍](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791191905052&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.

<br>

## 1. 내장 객체란?

- 기본적인 요청과 응답, 화면 출력 등 거의 모든 웹 프로그래밍에 있어 필수임.
- JSP의 내장 객체는 요청과 응답 혹은 HTTP 헤더 등의 정보를 쉽게 다룰 수 있도록 해줌.
- 내장 객체는 JSP 페이지가 실행될 때 컨테이너가 자동으로 생성해줌.
- _jspService() 메소드 안에 내장 객체를 선언하고 초기화하는 선언문들이 들어 있음.
- 내장 객체의 특징
  - 컨테이너가 미리 선언해놓은 참조 변수를 이용해 사용함.
  - 별도의 객체 생성 없이 각 내장 객체의 메소드를 사용할 수 있음.
  - JSP 문서 안의 <% 스크립틀릿 %>과 <%= 표현식 %>에서만 사용할 수 있음.
  - <%! 선언부 %>에서는 즉시 사용하는 건 불가능하고, 매개변수로 전달받아 사용할 수는 있음.
- 9가지 내장 객체의 종류
  - request
  - response
  - out
  - session
  - application
  - pageContext
  - page
  - config
  - exception

<br>

## 2. request 객체

- JSP에서 가장 많이 사용되는 객체
- 클라이언트(주로 웹 브라우저)가 전송한 요청 정보를 담고 있는 객체임.
- 주요 기능
  - 클라이언트와 서버에 대한 정보 읽기
  - 클라이언트가 전송한 요청 매개변수에 대한 정보 읽기
  - 요청 헤더 및 쿠키 정보 읽기

<br>

#### 클라이언트와 서버의 환경정보 읽기

- 요청은 GET 방식 혹은 POST 방식으로 구분되고, 요청 URL, 포트 번호, 쿼리스트링 등을 명시할 수 있음.
- request 내장 객체를 이용하면 이러한 정보를 얻어올 수 있음.

```jsp
<!-- 요청 페이지. RequestMain.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>내장 객체 - request</title></head>
<body>
    <h2>1. 클라이언트와 서버의 환경정보 읽기</h2>
    <a href="./RequestWebInfo.jsp?eng=Hello&han=안녕">  <!--GET 방식으로 요청-->
      GET 방식 전송
    </a>
    <br />
    <form action="RequestWebInfo.jsp" method="post">  <!--POST 방식으로 요청-->
        영어 : <input type="text" name="eng" value="Bye" /><br />
        한글 : <input type="text" name="han" value="잘 가" /><br />
        <input type="submit" value="POST 방식 전송" />
    </form>

    <h2>2. 클라이언트의 요청 매개변수 읽기</h2>
    <form method="post" action="RequestParameter.jsp">  <!--다양한 <input> 태그 사용-->
        아이디 : <input type="text" name="id" value="" /><br />
        성별 :
        <input type="radio" name="sex" value="man" />남자
        <input type="radio" name="sex" value="woman" checked="checked" />여자
        <br />
        관심사항 :
        <input type="checkbox" name="favo" value="eco" />경제
        <input type="checkbox" name="favo" value="pol" checked="checked" />정치
        <input type="checkbox" name="favo" value="ent" />연예<br />
        자기소개:
        <textarea name="intro" cols="30" rows="4"></textarea>
        <br />
        <input type="submit" value="전송하기" />
    </form>

    <h2>3. HTTP 요청 헤더 정보 읽기</h2>
    <a href="RequestHeader.jsp">  <!--HTTP 요청 헤더 읽기-->
        요청 헤더 정보 읽기
    </a>    
</body>
</html>
```

- 예제용 메인 페이지.

```jsp
<!-- 환경정보 읽기. RequestWebinfo.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>내장 객체 - request</title></head>
<body>
    <h2>1. 클라이언트와 서버의 환경정보 읽기</h2>
    <ul>
        <li>데이터 전송 방식 : <%= request.getMethod() %></li>
        <li>URL : <%= request.getRequestURL() %></li>
        <li>URI : <%= request.getRequestURI() %></li>
        <li>프로토콜 : <%= request.getProtocol() %></li>
        <li>서버명 : <%= request.getServerName() %></li>
        <li>서버 포트 : <%= request.getServerPort() %></li>
        <li>클라이언트 IP 주소 : <%= request.getRemoteAddr() %></li>
        <li>쿼리스트링 : <%= request.getQueryString() %></li>
        <li>전송된 값 1 : <%= request.getParameter("eng") %></li>
        <li>전송된 값 2 : <%= request.getParameter("han") %></li>
    </ul>
</body>
</html>
```

- 'GET 방식 전송' 링크나 [POST 방식 전송] 버튼을 클릭했을 때 나타나는 페이지의 소스.

- 서블릿에서 환경정보 읽기
  -  `String method = request.getMethod();` 
  - `PrintWriter out = response.getWriter();`
  -  `out.println("<li>요청 방식 : " + method + "</li>");` 
  - `out.close();`
- JSP에서는 <%= 표현식 %>을 활용하여 훨씬 간편하게 환경정보를 읽을 수 있음.

<br>

#### 클라이언트의 요청 매개변수 읽기

- \<form> 태그 하위 요소를 통해 입력한 값들도 서버로 전송됨.
- 전송된 값은 서버에서 읽은 후 변수에 저장하고, 적절한 처리를 위해 컨트롤러나 모델로 전달됨.
- 대표적으로 회원가입이나 로그인 등을 예로 들 수 있음.

```jsp
<!-- 요청 매개변수 읽기. RequestParameter.jsp>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>내장 객체 - request</title></head>
<body>
<%
request.setCharacterEncoding("UTF-8");  
String id = request.getParameter("id");  
String sex = request.getParameter("sex");
String[] favo = request.getParameterValues("favo");  
String favoStr = "";
if (favo != null) {  
    for (int i = 0; i < favo.length; i++) {
        favoStr += favo[i] + " ";
    }
}
String intro = request.getParameter("intro").replace("\r\n", "<br/>");  
%>
<ul>
    <li>아이디 : <%= id %></li>
    <li>성별 : <%= sex %></li>
    <li>관심사항 : <%= favoStr %></li>
    <li>자기소개 : <%= intro %></li>
</ul>
</body>
</html>
```

- RequestMain.jsp 실행 화면 중 '2. 클라이언트의 요청 매개변수 읽기' 부분에 값을 적당히 입력하고 [전송하기] 버튼을 클릭하면 POST 방식으로 RequestParameter.jsp에 전송됨.
- POST 방식으로 넘긴 데이터의 한글이 깨질 때는 파라미터를 받는 곳에서 `request.setCharacterEncoding("UTF-8");` 이렇게 UTF-8로 인코딩하면 됨.

- 전송되는 값이 하나라면 getParameter() 메소드로 받음. 주로 type 속성이 text, radio, password인 경우 사용됨.
- 전송되는 값이 여러 개라면 getParameterValues() 메소드로 받음. type 속성이 checkbox인 경우 사용됨.
- 값이 2개 이상이므로 String 배열로 반환함. for문을 이용해 String 배열에 담긴 값들을 하나의 문자열로 합침.
- textarea 태그는 텍스트를 여러 줄 입력할 수 있으므로 출력 시에 enter 키를 \<br> 태그로 변환해야 줄바꿈이 제대로 반영됨. enter는 특수 기호 \r\n으로 입력됨.

<br>

#### HTTP 요청 헤더 정보 읽기

- HTTP 프로토콜은 헤더에 부가적인 정보를 담도록 하고 있음.
- 웹 브라우저의 종류나 선호하는 언어 등 일반적인 HTML 문서 데이터 외의 추가 정보를 서버와 클라이언트가 교환할 수 있도록 문서의 선두에 삽입할 수 있음.

```JSP
<!-- 요청 헤더 읽기. RequestHeader.jsp -->
<%@ page import="java.util.Enumeration"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>내장 객체 - request</title></head>
<body>
    <h2>3. 요청 헤더 정보 출력하기</h2>
    <%
    Enumeration headers = request.getHeaderNames();  
    while (headers.hasMoreElements()) {  
        String headerName = (String)headers.nextElement();  
        String headerValue = request.getHeader(headerName); 
        out.print("헤더명 : " + headerName + ", 헤더값 : " + headerValue + "<br/>");
    }
    %>
    <p>이 파일을 직접 실행하면 referer 정보는 출력되지 않습니다.</p>
</body>
</html>
```

- RequestMain.jsp 실행 화면에서 '3. HTTP 요청 헤더 정보 읽기' 부분에서 '요청 헤더 정보 읽기' 링크를 클릭하면 실행되는 코드임.
- 10라인의 getHeaderNames() 메소드는 모든 요청 헤더의 이름을 반환함.
  - 반환 타입은 Enumeration임. Enumeration은 자바의 순환 인터페이스임. Iterator의 스레드 안전 버전이라고 보면 됨.
  - Iterator 가 hasNext() 를 사용하듯이 Enumeration은 hasMoreElements() 메소드를 사용함.
- user-agent : 웹 브라우저의 종류를 알 수 있음. 크롬, 파이어폭스, 익스플로러 등 여러가지 웹 브라우저에서 테스트해보면 조금씩 다른 결과가 출력됨.
- referer : 리퍼러는 웹을 서핑하면서 링크를 통해 다른 사이트로 방문 시 남는 흔적을 말함. 리퍼러는 웹 사이트 방문객이 어떤 경로로 접속하였는지 알아볼 때 유용함.
- cookie : 서버와 클라이언트 사이에 존재하는 정보 중 하나. 쿠키는 클라이언트, 세션은 서버가 가지고 있는 정보를 말함.

<br>

## 3. response 객체

- response 내장 객체는 요청에 대한 응답을 웹 브라우저로 보내주는 역할을 함.
- 주요 기능으로는 페이지 이동을 위한 리다이렉트와 HTTP 헤더에 응답 헤더 추가가 있음.
- 이 객체를 통해 JSP 프로그래머는 쿠키 추가, 날짜 스탬프 추가, HTTP 상태 코드 추가, 버퍼 삭제, 브라우저가 업데이트된 페이지를 요청하는 시간 지정(새로고침 시간 지정) 등을 할 수 있음.
- 주요 메소드들은 JSP의 지시어(Directive) 부분에서 설정할 수 있음 (ContentType, Encoding 등)
- response 객체의 메소드는 https://www.tutorialspoint.com/jsp/jsp_server_response.htm 여기에서 확인 가능.

<br>

### sendRedirect()로 페이지 이동하기

- 페이지를 이동하기 위해 HTML은 \<a> 태그를 사용하고, 자바스크립트에서는 location 객체를 사용함.
- JSP에서는 response 내장 객체의 sendRedirect()를 이용함.

```jsp
<!-- 로그인 폼과 응답 헤더 설정 페이지. ResponseMain.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>내장 객체 - response</title></head>
<body>
    <h2>1. 로그인 폼</h2>
    <% 
    String loginErr = request.getParameter("loginErr");
    if (loginErr != null) out.print("로그인 실패");
    %>
    <form action="./ResponseLogin.jsp" method="post">
        아이디 : <input type="text" name="user_id" /><br />
        패스워드 : <input type="text" name="user_pwd" /><br />
        <input type="submit" value="로그인" />
    </form>

    <h2>2. HTTP 응답 헤더 설정하기</h2>
    <form action="./ResponseHeader.jsp" method="get">
        날짜 형식 : <input type="text" name="add_date" value="2021-10-25 09:00" /><br />  
        숫자 형식 : <input type="text" name="add_int" value="8282" /><br />
        문자 형식 : <input type="text" name="add_str" value="홍길동" /><br />
        <input type="submit" value="응답 헤더 설정 & 출력" />
    </form>
</body>
</html>
```

```jsp
<!-- 로그인 처리하기. ResponseLogin.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>내장 객체 - Response</title></head>
<body>
<%
String id = request.getParameter("user_id");
String pwd = request.getParameter("user_pwd"); 
if (id.equalsIgnoreCase("must") && pwd.equalsIgnoreCase("1234")) {
    response.sendRedirect("ResponseWelcome.jsp");
}
else {
    request.getRequestDispatcher("ResponseMain.jsp?loginErr=1")
       .forward(request, response); 
}
%>
</body>
</html>
```

- 8~9라인은 request 내장 객체로 전송된 매개변수를 얻어온 다음, 10~16라인은 회원 인증을 진행함. 데이터베이스 연동하지 않고 간단한 예제 코드임.

- 인증에 성공하면 11라인이 실행되며 sendRedirect() 메소드에 건넨 응답 페이지로 이동함. 자바 스크립트의 location.href와 같은 기능임.

- 인증에 실패하면 request 내장 객체를 통해 로그인 페이지, 즉 ResponseMain.jsp로 포워드(forward : 전달)됨.

  - 포워드는 페이지 이동과는 다르게 제어 흐름을 넘겨주고자 할 때 사용함.
  - 쿼리스트링으로 loginErr 매개변수를 전달하여 로그인 성공 여부를 알려줌.

- RequestDispatcher

  - 현재 request에 담긴 정보를 저장하고 있다가 그 다음 페이지 그 다음 페이지에도 해당 정보를 볼 수 있게 계속 저장하는 기능.
  - RequestDispatcher 없이 forward를 하면 a.jsp -> Servlet -> b.jsp까지는 파라미터 정보가 넘어가지만 그 다음 단계에서 a.jsp의 파라미터를 별도로 저장하지 않는다면 소실됨.
  - sendRedirect
    - a.jsp request하면서 파라미터 넘김 -> 서버 파라미터 받았음을 a.jsp에 응답 -> 다시 a.jsp는 서버에 b.jsp 호출 요청 -> 서버는 b.jsp 응답
  - RequestDispatcher
    - a.jsp request하면서 파라미터 넘김 -> 서버는 a,b의 값을 처리 -> b.jsp 응답

  - a.jsp로 들어온 요청을 a.jsp 내에서 RequestDispatcher를 사용하여 b.jsp로 요청을 보낼 수 있음. 또는 a.jsp 에서 b.jsp로 처리를 요청하고 b.jsp에서 처리한 결과 내용을 a.jsp의 결과에 포함시킬 수 있음.
  - sendRedirect()는 HTTP 리다이렉션을 이용하기 때문에 하나의 요청 범위 안에서 처리를 하는 것이 아니라 브라우저에게 Response 후 브라우저 측에서 지정받은 요청 경로로 다시 재요청을 하는 방식이기에 두 번의 HTTP 트랜잭션이 발생하며, 서버측에서는 최초에 받은 요청 중에 처리한 내용을 리다이렉트된 요청 안에서 공유할 수 없는 문제가 있음.
  - HttpServletResponse를 통해 리다이렉트 하는 방식은 현재 어필르케이션 이외의 다른 자원의 경로를 요청할 수 있는 반면 RequestDispatcher는 현재 처리 중인 서블릿이 속해 있는 웹 애플리케이션 범위 내에서만 요청을 제어할 수 있음.
  - forward() 메소드
    - 대상 자원으로 제어를 넘기는 역할을 함.
    - 브라우저에서 a.jsp로 요청했을 때 a.jsp에서 forward()를 실행하여 b.jsp로 제어를 넘길 수 있음.
    - 제어를 넘겨 받은 b.jsp는 처리 결과를 최종적으로 브라우저에 출력함.
    - 브라우저 입장에서는 a.jsp를 요청했지만 받은 결과는 b.jsp의 결과임.
    - 이때 HTTP 리다이렉트 방식과는 달리 하나의 HTTP 요청 범위 안에서 동작이 이루어짐.

- forward, 리다이렉트 차이는 MVC 패턴 공부한 후 다시 보면 이해가 쉬움.

```jsp
<!-- 로그인 성공 페이지. ResponseWelcome.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>내장 객체 - response</title></head>
<body>
    <h2>로그인 성공</h2>
</body>
</html>
```

- 로그인 실패 시 화면에는 ResponseMain.jsp의 내용이 출력되었지만, 웹 브라우저의 주소표시줄을 보면 ResponseLogin.jsp로 표시되어 있음. 포워드는 이처럼 실행의 흐름만 특정한 페이지로 넘겨주는 역할을 함.

<br>

### HTTP 헤더에 응답 헤더 추가하기

- response 내장 객체는 응답 헤더에 정보를 추가하는 기능을 제공함. add계열과 set 계열이 있음.
- add 계열은 헤더값을 새로 추가할 때 사용함.
- set 계열은 기존의 헤더를 수정할 때 사용함.

- Response 기본 객체의 HTTP 헤더 조작 관련 메소드
  - `addDateHeader(String name, long date)` : 추가할 헤더값이 날짜 형식인 경우 사용함. 정수를 입력하는데 1970-01-01 00:00:00 000(ms)를 기준으로 흘러간 시간을 밀리초 단위로 입력함. 1L인 경우 1970-01-01 00:00:00 001(ms)를 의미함.
  - `addHeader(String name, String value)` : 추가할 헤더이름을 name에, 추가할 헤더값을 value에 넣어 헤더를 추가함.
  - `addIntHeader(String name, int value)` : 추가할 헤더이름을 name에, 추가할 헤더값을 value에 넣어 헤더를 추가함.
  - `setDateHeader(String name, long date)` : 설정할 헤더값이 날짜 형식인 경우 사용함.
  - `setHeader(String name, int value)` : 설정할 헤더이름을 name에, 설정할 헤더값을 value에 넣음.
  - `setIntHeader(String name, int value)` : 설정할 헤더이름을 name에, 설정할 헤더값을 value에 넣음.

```jsp
<!-- 응답 헤더에 값 추가하기. ResponseHeader.jsp -->
<%@ page import="java.util.Collection"%>
<%@ page import="java.text.SimpleDateFormat"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 응답 헤더에 추가할 값 준비(타입 변환)
SimpleDateFormat s = new SimpleDateFormat("yyyy-MM-dd HH:mm");
long add_date = s.parse(request.getParameter("add_date")).getTime();
// 헤더 날짜를 추가할 때 long 타입으로 추가함. 문자로 받은 값을 parse를 통해 Date 타입으로 변환 -> .getTime을 통해 하나의 숫자 형태로 변환하여 long 타입 add_date 변수에 저장.

java.sql.Date date2 = new java.sql.Date(add_date); 
System.out.println(date2);
// java.sql.Date는 long 타입의 날짜 데이터에서 년월일만 추출하는 객체임. date2는 print를 통해 Console에 출력됨.

int add_int = Integer.parseInt(request.getParameter("add_int"));
String add_str = request.getParameter("add_str");

// 응답 헤더에 값 추가
response.addDateHeader("myBirthday", add_date);
response.addIntHeader("myNumber", add_int);
response.addIntHeader("myNumber", 1004); // 추가
response.addHeader("myName", add_str);
response.setHeader("myName", "안중근");  // 수정
%>
<html>
<head><title>내장 객체 - response</title></head>
<body>
    <h2>응답 헤더 정보 출력하기</h2>
    <%
    Collection<String> headerNames = response.getHeaderNames();
    for (String hName : headerNames) {
        String hValue = response.getHeader(hName);
    %>
        <li><%= hName %> : <%= hValue %></li>
    <%
    }   
    %>
    
    <h2>myNumber만 출력하기</h2>
    <%
	Collection<String> myNumber = response.getHeaders("myNumber");
	for (String myNum : myNumber) {
	%>
		<li>myNumber : <%= myNum %></li>
	<%
	}
	%>
</body>
</html>
```

- java.sql.Date는 넘어온 값의 날짜만 추출함. `System.out.println(date2);` -> Console에만 출력됨.
- `int add_int = Integer.parseInt(request.getParameter("add_int"));` -> 폼 text는 숫자로 입력되어도 문자열 타입임. 정수 타입으로 변경.
- 출력값에 myNumber : 8282가 2번 출력되는데 getHeader() 메소드의 특징임. 중복된 네임의 값이 여러 개여도 첫 번째 값만 출력됨. 실제 개발자 도구에서 헤더를 확인해보면 myNumber : 8282 / myNumber : 1004 두 개의 응답 헤더를 확인할 수 있음.

<br>

## 4. out 객체

- 웹 브라우저에 변수 등의 값을 출력할 때 주로 사용함.
- 대부분의 상황에서 <%= 표현식 %>이 더 편리하기 때문에 JSP에서 많이 사용되지 않음.
- <% 스크립틀릿 %> 내에서 변수를 웹 브라우저에 출력해야 한다면 표현식보다는 out 내장 객체를 사용하는 편이 좋음.
- out 내장 객체는 버퍼를 사용함. 출력되는 모든 정보는 버퍼에 먼저 저장된 후 웹 브라우저에 출력됨.

```jsp
<!-- out 객체로 값 출력하기. OutMain.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>내장 객체 - out</title></head>
<body>
    <%
    // 버퍼 내용 삭제하기
    out.print("출력되지 않는 텍스트");  // 버퍼에 저장  
    out.clearBuffer();  // 버퍼를 비움(윗 줄의 출력 결과 사라짐) 

    out.print("<h2>out 내장 객체</h2>");

    // 버퍼 크기 정보 확인
    out.print("출력 버퍼의 크기 : " + out.getBufferSize() + "<br>");
    out.print("남은 버퍼의 크기 : " + out.getRemaining() + "<br>"); 

    out.flush();  // 버퍼 내용 출력 
    out.print("flush 후 버퍼의 크기 : " + out.getRemaining() + "<br>");

    // 다양한 타입의 값 출력
    out.print(1);
    out.print(false);
    out.print('가');
    %>
</body>
</html>
```

- 9라인에서 버퍼에 들어간 정보를 10라인에서 clearBuffer() 메소드를 통해 삭제함. 9라인의 문자열은 출력되지 않음.
- getBufferSize() 메소드는 현재 페이지에 설정된 버퍼의 크기를 가져옴. 버퍼 크기를 page 지시어로 따로 선언하지 않을 경우 기본값인 8KB가 됨.
- getRemaining() 메소드는 사용하고 남은 버퍼의 크기를 알려줌.
- flush() 메소드는 버퍼에 담긴 내용을 강제 플러시함. 버퍼는 원래 모두 채워졌을 때 플러시되면서 내용을 출력하지만, 필요한 경우에는 이처럼 즉시 출력할 수 있음.
- println() 메소드는 print() 메소드에 줄바꿈 문자(\r\n)가 추가됨. HTML 문서에는 줄 바꿈에 \<br> 태그를 사용해야 함. HTML 문서에서 println() 메소드는 띄어쓰기 한 칸이 적용되는 정도만 차이가 있음.

<br>

## 5. application 객체

- javax.servlet.ServletContext 객체

- application 내장 객체는 웹 애플리케이션당 하나만 생성되며, 모든 JSP 페이지에서 접근할 수 있음.
- application 내장 객체는 ServletContext 타입임.
- 주로 웹 애플리케이션 전반에서 이용하는 정보를 저장하거나, 서버의 정보, 서버의 물리적 경로 등을 얻어오는 데 사용함.
- web.xml에 설정한 컨텍스트 초기화 매개변수를 읽어오고 폴더의 물리적 경로를 알아낼 수 있음.
- web.xml은 웹 애플리케이션에 대한 여러 가지 설정을 저장하는 곳으로 배포 서술자(deployment descriptor)라고 부름.
- 물리적 경로란 특정 파일이나 폴더의 전체 경로, 즉 절대 경로를 말하며, 파일을 업로드하거나 입출력 작업 시에 주료 사용함.
- application 내장 객체의 메소드
  - `getServerInfo()` : 웹 컨테이너의 이름과 버전을 리턴함.
  - `getMimeType(fileName)` : 지정한 파일의 MIME 타입을 리턴함.
  - `getRealPath(path)` : 주어진 가상 경로에 해당하는 실제 경로를 가져옴.
  - `log(message)` : 로그 파일에 message를 기록함.
  - `setAttribute(String name, Object o)` : application 객체에 추가 객체를 저장함.
  - `getAttribute(String name)` : 추가된 객체를 가져옴
  - `removeAttribute(String name)` : 특정 Attribute를 지움.

```jsp
<!-- 초기화 매개변수 추가하기. web.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi= ... 생략 ...
  <display-name>MustHaveJSP</display-name>
  <welcome-file-list>
	... 생략 ...
  </welcome-file-list>

  <context-param>
    <param-name>INIT_PARAM</param-name>
    <param-value>web.xml에 저장한 초기화 매개변수</param-value>
  </context-param>
</web-app>
```

```jsp
<!-- 다양한 서버 정보 얻어오기. ApplicationMain.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>내장 객체 - application</title></head>
<body>
    <h2>web.xml에 설정한 내용 읽어오기</h2>
    초기화 매개변수 : <%= application.getInitParameter("INIT_PARAM") %>

    <h2>서버의 물리적 경로 얻어오기</h2>
    application 내장 객체 : <%= application.getRealPath("/02ImplicitObject") %>

    <h2>선언부에서 application 내장 객체 사용하기</h2>
    <%!
    public String useImplicitObject() {
        return this.getServletContext().getRealPath("/02ImplicitObject");
    }
    public String useImplicitObject(ServletContext app) {
        return app.getRealPath("/02ImplicitObject");
    }
    %>
    <ul>
        <li>this 사용 : <%= useImplicitObject() %></li>
        <li>내장 객체를 인수로 전달 : <%= useImplicitObject(application) %></li>
    </ul>
</body>
</html>

```

- getInitParameter() 메소드는 web.xml에 설정한 초기화 매개변수를 읽어오는 메소드. 설정된 파라미터가 없으면 null 반환.
- getRealPath() 메소드는 현재 예제를 작성 중인 폴더("/02ImplicitObject")의 물리적 경로를 얻어와서 출력함. 인수로는 '컨텍스트 루트를 제외한' 경로를 입력하면 됨.

- 선언부에서 application 내장 객체를 사용하려면 this를 통해 application 내장 객체를 얻어오거나, 인수로 전달해야지만 사용할 수 있음.
- 이클립스를 사용했기 때문에 물리적 경로가 김. 이클립스는 작성한 JSP 파일을 실행하면 별도의 디렉토리를 생성하여 서블릿 변환과 컴파일을 진행하기 때문. 톰캣에 배포한 후 실행하면 JSP 파일이 있는 위치가 그대로 출력됨.

<br>

## 6. exception 객체

- try ~ catch에서 오류명과 오류 메시지를 출력하는 부분에서 사용됨.
- JSP에서 그 이상으로 사용되는 경우는 거의 없음.

- JSP에서 빈번하게 발생되는 에러 코드
  - 404 : Not Found. 클라이언트가 요청한 경로에서 문서를 찾을 수 없음. URL 상의 경로명이나 파일명이 제대로 입력되었는지 확인.
  - 405 : Method Not Allowed. 허용되지 않는 메소드, GET 방식 혹은 POST 방식으로 요청했는데 이를 처리할 컨트롤러가 없다는 뜻. 주로 서블릿으로 개발할 때 발생하게 되는데 doGet(), doPost() 메소드가 적절히 오버라이딩되었는지 확인.
  - 500 : Internal Server Error. 서버 내부 오류, 코드에 오타가 있거나 로직에 문제가 있어 웹 서버가 요청 사항을 수행할 수 없음. 가장 많이 발생하는 에러. 개발 중인 코드를 전반적으로 확인하여 오류 처리해야 함.

```jsp
<!-- web.xml에 에러별 출력 페이지 설정. web.xml -->
 <error-page>
    <error-code>404</error-code>
    <location>/02ImplicitObject/Exception.jsp</location>
  </error-page>
  <error-page>
    <error-code>405</error-code>
    <location>/02ImplicitObject/Exception.jsp</location>
  </error-page>
  <error-page>
    <error-code>500</error-code>
    <location>/02ImplicitObject/Exception.jsp</location>
  </error-page>
```

- \<location> 엘리먼트에 경로를 명시할 때는 컨텍스트 루트를 제외한 나머지를 기술하면 됨.

```jsp
<!-- 에러 출력 페이지. Exception.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>내장 객체 - exception</title></head>
<body>
<%
int status = response.getStatus();  // response 내장 객체로부터 에러 코드 확인

// 에러 코드에 따라 적절한 메시지 출력
if (status == 404) {
    out.print("404 에러가 발생하였습니다.");
    out.print("<br/>파일 경로를 확인해주세요.");
}
else if (status == 405) {
    out.print("405 에러가 발생하였습니다.");
    out.print("<br/>요청 방식(method)을 확인해주세요.");
}
else if (status == 500) {
    out.print("500 에러가 발생하였습니다.");
    out.print("<br/>소스 코드에 오류가 없는지 확인해주세요.");
}
%>
</body>
</html>

```

- 실습 과정에서 위에서 web.xml에 추가한 내용은 주석으로 처리하는 것이 좋음. 실제 서비스에서 오류 발생 시 코드가 고객에게 그대로 노출되는 문제를 피하기 위해서 설정했지만 개발 시에는 자세한 에러 메시지를 확인하면서 디버깅해야 하므로 에러 페이지 지정은 개발이 완료된 후 하는 것이 좋음.

<br>

# References

- [성낙현의 JSP 자바 웹 프로그래밍 - 성낙현](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791191905052&orderClick=LAG&Kc=)