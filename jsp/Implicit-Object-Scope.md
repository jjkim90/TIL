# 내장 객체의 영역

<br>

> 이 문서는 [성낙현의 JSP 자바 웹 프로그래밍](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791191905052&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.

<br>

## 내장 객체의 영역이란?

- 각 객체가 저장되는 메모리의 유효기간.
- page 영역 : 동일한 페이지에서만 공유됨.
- request 영역 : 하나의 요청에 의해 호출된 페이지와 포워드된 페이지까지 공유됨.
- session 영역 : 클라이언트가 처음 접속한 후 웹 브라우저를 닫을 때까지 공유됨.
- application 영역 : 한 번 저장되면 웹 애플리케이션이 종료될 때까지 유지됨.
- 범위의 크기는 page < request < session < application
- 더 큰 범위의 영역은 더 작은 범위의 영역을 하나 이상 포함할 수 있음.
- 영역이 제공하는 주요 메소드
  - `void setAttribute(String name, Object value)`  : 각 영역에 속성을 저장함. 첫 번째 인수는 속성명, 두 번째 인수는 저장할 값. 값의 타입은 Object이므로 모든 타입의 객체를 저장할 수 있음.
  - `Object getAttribute(String name)` : 영역에 저장된 속성값을 얻어옴. Object로 자동 형변환되어 저장되므로 원래 타입으로 형변환 후 사용해야 함.
  - `void removeAttribute(String name)` : 영역에 저장된 속성을 삭제함. 삭제할 속성명이 존재하지 않더라도 에러는 발생하지 않음.

<br>

## 데이터 전송 객체(DTO) 준비

- 데이터 전송 객체
  - Data Transfer Object, DTO
  - 주로 데이터를 저장하거나 전송하는 데 쓰이는 객체
  - 다른 로직 없이 순수하게 데이터만을 담고 있음.
  - 데이터만 가지고 있는 객체라 하여 값 객체(Value Object, VO)라고도 함.
  - DTO는 자바빈즈(JavaBeans) 규약에 따라 작성함.

- 자바빈즈
  - 자바로 작성한 소프트웨어 컴포넌트로 자바빈즈 규약을 따르는 자바 클래스를 말함.
  - 자바빈즈 규약
    1. 자바빈즈는 기본(default) 패키지 이외의 패키지에 속해야 함.
    2. 멤버 변수(속성)의 접근 지정자는 private으로 선언함.
    3. 기본 생성자가 있어야 함.
    4. 멤버 변수에 접근할 수 있는 게터(getter) / 세터(setter) 메소드가 있어야 함.
    5. 게터/세터 메소드의 접근 지정자는 public으로 선언함.

```java
<!-- DTO(데이터 전송 객체) Person 클래스. Person.java -->
package common; // 기본 패키지 이외의 패키지(규약 1번)

public class Person {
    private String name; // private 멤버 변수(규약 2번)
    private int age;

    public Person() {} // 기본 생성자(규약 3번)
    public Person(String name, int age) {
        super();
        this.name = name;
        this.age = age;
    }

    // public 게터/세터 메소드들 (규약 4번, 5번)
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```

<br>

## page 영역

- page 영역은 기본적으로 클라이언트의 요청을 처리하는 데 관여하는 JSP 페이지마다 하나씩 생성됨.
- 이때 각 JSP 페이지는 page 영역을 사용하기 위한 pageContext 객체를 할당 받게 됨.
- pageContext 객체에 저장된 정보는 해당 페이지에서만 사용할 수 있고 페이지를 벗어나면 소멸됨.
- include 지시어로 포함한 파일은 하나의 페이지로 통합되므로 page 영역이 공유됨.

```jsp
<!-- page 영역에 속성값 저장하고 불러오기. PageContextMain.jsp -->
<%@ page import="common.Person"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
// 속성 저장
pageContext.setAttribute("pageInteger", 1000);
pageContext.setAttribute("pageString", "페이지 영역의 문자열");
pageContext.setAttribute("pagePerson", new Person("한석봉", 99)); 
%>
<html>
<head><title>page 영역</title></head>
<body>
    <h2>page 영역의 속성값 읽기</h2>
    <%
    // 속성 읽기
    int pInteger = (Integer)(pageContext.getAttribute("pageInteger"));
    String pString = pageContext.getAttribute("pageString").toString();
    Person pPerson = (Person)(pageContext.getAttribute("pagePerson"));
    %>
    <ul>
        <li>Integer 객체 : <%= pInteger %></li>
        <li>String 객체 : <%= pString %></li>
        <li>Person 객체 : <%= pPerson.getName() %>, <%= pPerson.getAge() %></li>
    </ul>

    <h2>include된 파일에서 page 영역 읽어오기</h2>
    <%@ include file="PageInclude.jsp" %>
       
    <h2>페이지 이동 후 page 영역 읽어오기</h2>
    <a href="PageLocation.jsp">PageLocation.jsp 바로가기</a>
</body>
</html>
```

- 17~19라인에서는 pageContext 객체를 통해 page 영역에 저장된 속성값들은 모든 속성 Object 타입으로 저장되어 있으므로 다시 원래의 타입으로 형변환함.
- 18라인처럼 String 타입인 경우 toString() 메소드를 통해 문자열로 변환하여 출력할 수도 있음.
- 24라인에서 Person은 DTO라서 멤버 변수가 private으로 선언되었으므로 게터 메소드를 이용함.
- 27~28라인에서 include 지시어로 다른 JSP 파일을 포함시킬 경우 말 그대로 '포함' 관계이므로 같은 페이지가 됨. page 영역이 그대로 유지됨.
- 30~31라인에서 \<a> 태그로 감싼 링크를 클릭하면 다른 페이지로 이동하게 되어 이전 페이지에서 만든 page 영역은 소멸됨.

```jsp
<!-- PageContextMain.jsp에 포함시킬 JSP 문서. PageInclude.jsp -->
<%@ page import="common.Person"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<h4>Include 페이지</h4>
<%
int pInteger2 = (Integer)(pageContext.getAttribute("pageInteger"));
//String pString2 = pageContext.getAttribute("pageString").toString();
Person pPerson2 = (Person)(pageContext.getAttribute("pagePerson"));
%>
<ul>
    <li>Integer 객체 : <%= pInteger2 %></li>
    <li>String 객체 : <%= pageContext.getAttribute("pageString") %></li>
    <li>Person 객체 : <%= pPerson2.getName() %>, <%= pPerson2.getAge() %></li>
</ul>
```

- 포함시킬 파일을 작성할 때는 새로운 JSP 파일을 생성한 후 page 지시어를 제외한 나머지 HTML 코드를 모두 삭제한 후 작성해야 함. include는 문서 안에 또 다른 문서를 포함하는 형태이므로 태그가 중복될 수 있기 때문임.
- 8라인을 주석 처리하고 대신 13라인에서 직접 출력함. 저장한 객체가 문자열이거나 기본 타입의 래퍼 클래스라면 별도의 형변환 없이 출력해도 됨.
- 변수명 뒤에 '2' 추가한 이유
  - 같은 페이지에 속하는 스크립틀릿들은 자바 코드로 변환될 때 모두 _jspService() 메소드의 본문에 추가됨.
  - 변수명을 바꿔주지 않으면 같은 이름의 변수를 중복해서 선언하는 꼴이 되어 컴파일 오류가 발생함. `Duplicate local variable pInteger`

```jsp
<!-- 태그 링크로 이동할 별도 페이지. PageLocation.jsp -->
<%@ page import="common.Person"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>page 영역</title></head>
<body>
    <h2>이동 후 page 영역의 속성값 읽기</h2>
    <%
    Object pInteger = pageContext.getAttribute("pageInteger");
    Object pString = pageContext.getAttribute("pageString");
    Object pPerson = pageContext.getAttribute("pagePerson"); 
    %>
    <ul>
        <li>Integer 객체 : <%= (pInteger == null) ? "값 없음" : pInteger %></li>
        <li>String 객체 : <%= (pString == null) ? "값 없음" : pString %></li>
        <li>Person 객체 : <%= (pPerson == null) ? "값 없음" : ((Person)pPerson).getName() %></li>
    </ul>
</body>
</html>
```

- 10~12라인에서 형변환하지 않은 이유
  - 가져오려는 속성이 존재하지 않는다면 getAttribute() 메소드가 null을 반환함.
  - null을 int 타입 변수에 담으려 시도하면 NullPointerException 발생함.
  - 대신 값을 화면에 출력할 때 null이 아닌지 확인하는 절차를 거침.
- \<a> 태그를 통한 이동은 새로운 페이지를 요청하는 것.
- 서로 다른 페이지이므로 page 영역은 공유되지 않음.
- 어떤 속성값도 출력되지 않음.

<br>

## request 영역

- 클라이언트가 요청을 할 때마다 새로운 request 객체가 생성되고, 같은 요청을 처리하는 데 사용되는 모든 JSP 페이지가 공유됨.
- request 영역에 저장된 정보는 현재 페이지와 포워드된 페이지까지 공유할 수 있음.
- 단, 페이지 이동 시에는 소멸되어 사용할 수 없게 됨.
- request 영역은 하나의 요청에 대한 응답이 완료될 때 소멸하게 되므로 page 영역보다는 접근 범위가 조금 더 넓음.

```jsp
<!-- 최초 페이지(request 영역 동작 확인용). RequestMain.jsp -->
<%@ page import="common.Person"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
request.setAttribute("requestString", "request 영역의 문자열");
request.setAttribute("requestPerson", new Person("안중근", 31)); 
%>
<html>
<head><title>request 영역</title></head>
<body>
    <h2>request 영역의 속성값 삭제하기</h2>
    <%
        request.removeAttribute("requestString"); 
        request.removeAttribute("requestInteger"); // 에러 없음
    %>
    <h2>request 영역의 속성값 읽기</h2>
    <%
    Person rPerson = (Person)(request.getAttribute("requestPerson"));
    %>
    <ul>
        <li>String 객체 : <%= request.getAttribute("requestString") %></li>
        <li>Person 객체 : <%= rPerson.getName() %>, <%= rPerson.getAge() %></li>
    </ul>
    <!-- 포워드하는 첫 번째 방법 -->
    <h2>포워드된 페이지에서 request 영역 속성값 읽기</h2>
    <%
    request.getRequestDispatcher("RequestForward.jsp?paramHan=한글&paramEng=English")  
        .forward(request, response);
    %>
    <!-- 포워드하는 두 번째 방법 -->
    <%--
    RequestDispatcher rd = request.getRequestDispatcher("RequestForward.jsp?paramHan=한글&paramEng=English");
    rd.forward(request, response);
    --%>
</body>
</html>
```

- 6~7 라인에서 request 영역에 String 객체와 Person 객체 저장함.
- 14~15 라인에서 request 영역에 저장된 속성을 삭제함. 존재하지 않는 requestInteger 를 삭제하려 시도했지만 에러가 발생하지 않음.
- 17~24 라인에서 속성값을 읽어와서 출력함.
- 25~35 라인에서 포워드로 전달된 페이지를 출력함. 원래 페이지에 더해지지 않고 위에 덧붙여지기 때문에 1~24라인의 내용을 보고 싶으면 포워드하는 소스 코드를 잠시 주석처리한 후 실행해야 함.

```jsp
<!-- 포워드되는 JSP 페이지. RequestForward.jsp -->
<%@ page import="common.Person"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>request 영역</title></head>
<body>
    <h2>포워드로 전달된 페이지</h2>
    <h4>RequestMain 파일의 리퀘스트 영역 속성 읽기</h4>
    <%  
    Person pPerson = (Person)(request.getAttribute("requestPerson"));
    %>
    <ul>
        <li>String 객체 : <%= request.getAttribute("requestString") %></li>
        <li>Person 객체 : <%= pPerson.getName() %>, <%= pPerson.getAge() %></li>
    </ul>
    <h4>매개변수로 전달된 값 출력하기</h4>
    <%
        request.setCharacterEncoding("UTF-8");
        out.println(request.getParameter("paramHan"));
        out.println(request.getParameter("paramEng"));
    %>
</body>
</html>
```

- 10~16 라인에서 RequestMain.jsp에서 저장한, request 영역에 저장된 속성들을 읽어와서 출력함.
- 20~21 라인에서 포워드하면서 쿼리스트링으로 전달한 매개변수의 값을 출력함. 값을 얻어오기 전에 19라인에서 인코딩 방식을 UTF-8로 변경함. 매개변수로 전달된 값 중 한글이 포함되어 있기 때문임.
- 웹 브라우저의 주소표시줄에는 여전히 최초 실행한 RequestMain.jsp가 표시되지만, 화면에는 RequestForward.jsp의 내용이 보임.
- 최초 페이지에서 request 영역에 저장했던 값들을 성공적으로 읽어온 것을 확인할 수 있음.
- String 객체가 null로 출력되는 이유는 최초 실행한 페이지에서 removeAttribute()로 삭제했기 때문임.
- 매개변수로 전달한 값들도 문제없이 출력됨.

<br>

## session 영역

- 클라이언트가 웹 브라우저를 최초로 열고난 후 닫을 때까지 요청되는 모든 페이지는 session 객체를 공유할 수 있음.
- 세션이란 클라이언트가 서버에 접속해 있는 상태 혹은 단위를 말하는 것으로, 주로 회원인증 후 로그인 상태를 유지하는 처리에 사용됨.
- 포털 사이트에서 웹 브라우저를 닫으면 자동으로 로그아웃이 되는 이유가 바로 session 객체의 특성 때문임.

```jsp
<!-- 최초 페이지(session 영역 동작 확인용). SessionMain.jsp -->
<%@ page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
ArrayList<String> lists = new ArrayList<String>();
lists.add("리스트");
lists.add("컬렉션");
session.setAttribute("lists", lists);
%>       
<html>
<head><title>session 영역</title></head>
<body>
    <h2>페이지 이동 후 session 영역의 속성 읽기</h2>
    <a href="SessionLocation.jsp">SessionLocation.jsp 바로가기</a>
</body>
</html>
```

- 6~9 라인, ArrayList 컬렉션을 생성한 후 2개의 String 객체를 저장. 이 컬렉션을 통째로 session 영역에 저장함.
- 15 라인, session 영역이 이동된 페이지에서도 공유되는지 확인하기 위한 링크.

```jsp
<!-- 링크를 클릭해 이동된 페이지. SessionLocation.jsp -->
<%@ page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>session 영역</title></head>
<body>
    <h2>페이지 이동 후 session 영역의 속성 읽기</h2>
    <%
    ArrayList<String> lists = (ArrayList<String>)session.getAttribute("lists"); 
    for (String str : lists)
        out.print(str + "<br/>");
    %>    
</body>
</html>
```

- 10 라인, session 영역에서 속성을 읽어온 후 형변환. 컬렉션의 타입은 ArrayList\<String>임.
- 11~12라인, for문을 이용해 컬렉션에서 객체들을 꺼내 출력함.

- SessionMain.jsp를 실행한 후 링크를 클릭하면 페이지가 이동되었지만 session 영역에 저정된 속성값은 정상 출력되는 것을 확인할 수 있음.
- session 영역의 속성값을 삭제하고 싶다면 웹 브라우저를 완전히 닫았다가 다시 열면 됨.
- 탭만 닫아서는 session이 삭제되지 않음. 웹 브라우저 전체를 닫아야 함.

- 웹 브라우저 전체를 닫은 후 SessionLocation.jsp를 실행하면 500 에러가 발생함. 웹 브라우저를 닫으면 session 객체가 삭제되고, 웹 브라우저를 다시 실행하면 그때 새로운 session 객체가 만들어짐. getAttribute("lists")로 속성값을 읽어오려 하면 null을 반환하여 NullPointerException이 발생하는 것임.

<br>

## application 영역

- 웹 어플리케이션은 단 하나의 application 객체만 생성하고, 클라이언트가 요청하는 모든 페이지가 application 객체를 공유하게 됨.
- application 객체는 웹 서버를 시작할 때 만들어지며, 웹 서버를 내릴 때 삭제됨.
- application 영역에 한 번 저장된 정보는 페이지를 이동하거나, 웹 브라우저를 닫았다가 새롭게 접속해도 삭제되지 않음.

```jsp
<!-- 최초 페이지(application 영역 동작 확인용). ApplicationMain.jsp -->
<%@ page import="java.util.HashMap"%>
<%@ page import="common.Person"%>
<%@ page import="java.util.Map"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>application 영역</title></head>
<body>
    <h2>application 영역의 공유</h2>
    <%
    Map<String, Person> maps = new HashMap<>();
    maps.put("actor1", new Person("이수일", 30));
    maps.put("actor2", new Person("심순애", 28));
    application.setAttribute("maps", maps);
    %>
    application 영역에 속성이 저장되었습니다.
</body>
</html>
```

- 12~14 라인, HashMap 컬렉션을 생성한 후 두 개의 Person 객체를 저장함.
- 15 라인, 컬렉션 채로 application 영역에 저장함.
- ApplicationMain.jsp를 실행하면 application 영역에 속성값이 저장됨.

```jsp
<!-- 결과 페이지. ApplicationResult.jsp -->
<%@ page import="java.util.Set"%>
<%@ page import="common.Person"%>
<%@ page import="java.util.Map"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<html>
<head><title>application 영역</title></head>
<body>
    <h2>application 영역의 속성 읽기</h2>
    <%
    Map<String, Person> maps
            = (Map<String, Person>)application.getAttribute("maps");
    Set<String> keys = maps.keySet(); 
    for (String key : keys) {
        Person person = maps.get(key);
        out.print(String.format("이름 : %s, 나이 : %d<br/>", 
                person.getName(), person.getAge()));
    }  
    %>
</body>
</html>
```

- 12~13 라인, application 영역에 저장한 "maps" 속성값을 읽어서 원래 형태인 Map<String, Person> 타입 변수에 저장함.
- 14 라인, Map 컬렉션에 담긴 데이터를 확인하기 위해 먼저 키들을 알아낼려고 keySet() 메소드 이용.
- 15~19 라인, 향상된 for문에서 모든 키에 해당하는 값들을 하나씩 꺼내 출력함. Map에 저장된 객체를 꺼낼 때는 get() 메소드 사용함.
- session을 삭제했을 때처럼 모든 웹 브라우저를 닫고 ApplicationResult.jsp를 실행해도 앞에서 저장한 속성값이 여전히 정상적으로 출력되는 것을 볼 수 있음. 이 속성값은 웹 서버가 종료되지 않는 한 계속 유지됨.
- 구동 중인 톰캣 서버를 Restart 한 후 ApplicationResult.jsp 파일을 실행하면 500에러 메시지를 확인할 수 있음.

<br>

- # References

  - [성낙현의 JSP 자바 웹 프로그래밍 - 성낙현](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791191905052&orderClick=LAG&Kc=)