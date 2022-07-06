# JSP 기본

<br>

> 이 문서는 [성낙현의 JSP 자바 웹 프로그래밍](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791191905052&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.

<br>

## 1. 동적 웹 페이지로의 여정과 JSP

### 동적 웹 페이지

- 정적 웹 페이지(static web page)란 웹 서버에 '저장되어 있는 파일을 그대로' 웹 브라우저에 전송해 출력하는 가장 기본적인 웹 페이지를 말함.
- 동적 웹 페이지(dynamic web page)
  - 동일한 페이지라 할지라도 그때그때 내용이 달라질 수 있는 웹 페이지를 말함.
  - 서버가 클라이언트의 요청을 해석하여 가장 적절한 웹 페이지를 그때그때 생성해 보내주는 기술임.
  - '전처리' 과정을 거쳐 응답 페이지를 동적으로 생성함.
  - 동적 웹 페이지 기술에는 JSP, 서블릿, ASP, PHP 등이 있음.

<br>

### 애플릿

- 자바 애플릿(Java Applet)은 웹에서 실행되도록 설계된 자바 애플리케이션을 통째로 웹 브라우저로 전송한 후, 자바 가상 머신을 탑재한 웹 브라우저가 이를 실행하는 방식으로 구동됨.
- 동적 웹 기술이 발달하기 전 시절 한때 이목을 끌었지만, 표준 기술인 HTML과 자바스크립트가 발전하면서 지금은 더 이상 지원되지 않는 기술임.

<br>

### 서블릿

- 서블릿은 클라이언트의 요청을 받으면 서버에서 처리한 후, 응답으로는 결과값만 보내주는 구조임.
- 서블릿은 자바 파일(\*.java)을 컴파일한 클래스 파일(\*.class) 형태이며, 이를 실행하고 관리해주는 런타임을 서블릿 컨테이너라고 함.
- 대표적인 서블릿 컨테이너로는 아파치 톰캣(Apache Tomcat)이 있음.

<br>

### JSP

- 서블릿은 기본적으로 자바 코드인데, 결과로 보여줄 HTML 코드를 일일이 자바로 생성, 조합하다 보니 너무 많은 코드가 필요했음.
- 기본을 HTML로 하고 필요한 부분만 자바 코드를 삽입하는 형태인 JSP가 탄생하게 됨.
- JSP 구동 방식은 JSP 파일을 서블릿으로 변환하여 서블릿을 실행하는 방식임.
- 한 번 서블릿으로 컴파일된 JSP 파일은 캐시되므로 실질적인 성능 저하 없이 개발 생산성과 유지보수 편의성을 모두 얻을 수 있음.
- JSP는 클라이언트에 보여지는 결과 페이지를 생성할 때 주로 쓰이며, 서블릿은 UI 요소가 없는 제어나 기타 처리 용도로 쓰임.

<br>

### 오늘날의 웹 사이트

- 정적 웹 페이지는 동적 웹 페이지보다 더 만들기 쉽고 속도도 빠르며 운영 비용도 저렴함.
- 대부분의 웹 사이트나 웹 애플리케이션은 정적인 콘텐츠와 동적인 콘텐츠가 섞여 있음.

<br>

## 2. JSP 파일 기본 구조

- JSP의 주된 목적은 웹 브라우저에 띄울 HTML 파일을 생성하는 것.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%!
String str1 = "JSP";
String str2 = "안녕하세요..";
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>HelloJSP</title>
</head>
<body>
    <h2>처음 만들어보는 <%= str1 %></h2>
    <p>
        <%
        out.println(str2 + str1 + "입니다. 열공합시다^^*");
        %>
    </p>
</body>
</html>

```

- 일반적인 HTML 파일에 지시어와 스크립트 요소가 추가됨.
- 스크립트 요소는 선언부, 표현식, 스크립틀릿으로 나뉨.
- 지시어는 해당 JSP 페이지의 처리 방법을 JSP 엔진에 '지시'해주는 역할을 함.
- 스크립트 요소는 HTML 파일 중간에 자바 코드를 삽입할 때 사용함.

<br>

## 3. 지시어(Directive)

- 지시어는 JSP 페이지를 자바(서블릿) 코드로 변환하는 데 필요한 정보를 JSP 엔진에 알려줌.
- 주로 스크립트 언어나 인코딩 방식 등을 설정함.
- 지시자 혹은 디렉티브로 부르기도 함.
- `<%@ 지시어종류 속성1="값1" 속성2="값2" ... %>`
  - 지시어 종류 뒤에 다수의 속성을 지정할 수 있는 구조임.
  - page 지시어 : JSP 페이지에 대한 정보를 설정함.
  - include 지시어 : 외부 파일을 현재 JSP 페이지에 포함시킴
  - taglib 지시어 : 표현 언어에서 사용할 자바 클래스나 JSTL을 선언함.

<br>

### page 지시어

- page 지시어는 JSP 페이지에 대한 정보를 설정함.
- 문서의 타입, 에러 페이지, MIME 타입과 같은 정보를 설정함.

| 속성                     | 내용                                                         |
| ------------------------ | ------------------------------------------------------------ |
| info                     | 페이지에 대한 설명을 입력함.                                 |
| language                 | 페이지에서 사용할 스크립팅 언어를 지정함.                    |
| contentType              | 페이지에서 생성할 MIME 타입을 지정함.                        |
| pageEncoding             | charset과 같이 인코딩을 지정함.                              |
| import                   | 페이지에서 사용할 자바 패키지와 클래스를 지정함.             |
| session                  | 세션 사용 여부를 지정함.                                     |
| buffer                   | 출력 버퍼의 크기를 지정함. 버퍼를 사용하지 않으려면 "none"으로 지정함. |
| autoFlush                | 출력 버퍼가 모두 채워졌을 때 자동으로 비울지 결정함. buffer 속성이 none일 때 false로 지정하면 에러가 발생함. |
| trimDirectiveWhitespaces | 지시어 선언으로 인한 공백을 제거할지 여부를 지정함.          |
| errorPage                | 해당 페이지에서 에러가 발생했을 때 에러 발생 여부를 보여줄 페이지를 지정함. |
| isErrorPage              | 해당 페이지가 에러를 처리할지 여부를 지정함.                 |

<br>

#### language, contentType, pageEncoding 속성

- 이클립스에서 JSP 파일 생성 시 기본적으로 삽입되는 지시어.
  - `<%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>`
  - language : 스크립팅 언어는 자바를 사용함.
  - contentType : 문서의 타입, 즉 MIME 타입은 text/html이고, 캐릭터셋은 UTF-8임.
  - pageEncoding : 소스 코드의 인코딩 방식은 UTF-8임.
- 캐릭터셋이나 인코딩의 기본값은 ISO-8859-1. 영어와 서유럽어 문자만 포함하고 있어서 한글은 제대로 출력되지 않음.
- 한글을 표현하기 위해서는 EUC-KR이나 UTF-8을 사용해야 하며, 최근에는 다국어를 지원하는 UTF-8을 주로 사용함.

<br>

#### import 속성

- 자바에서 외부 클래스를 사용하려면 import문으로 해당 패키지나 클래스를 가져와야 하듯이, JSP 파일에서도 필요한 클래스가 있으면 임포트해야 함.
- java.lang 패키지에 속하지 않은 클래스를 JSP 문서에서 사용하기 위해 사용함.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ page import="java.text.SimpleDateFormat"%>  <!--필요한 외부 클래스 임포트-->
<%@ page import="java.util.Date"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>page 지시어 - import 속성</title>
</head>
<body>
<%
Date today = new Date();  // 외부 클래스 생성
SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
String todayStr = dateFormat.format(today);
out.println("오늘 날짜 : " + todayStr);  // 오늘 날짜를 웹 브라우저에 출력
%>
</body>
</html>
```

- Date와 SimpleDateFormat 클래스는 java.lang 패키지에 속하지 않아 현재 문서에서 사용하기 위해서는 페이지 상단에서 page 지시어를 사용해 임포트해야 함.

<br>

#### errorPage, isErrorPage 속성

- JSP는 실행 도중에 에러가 발생하면 "HTTP Status 500" 에러 화면을 웹 브라우저에 표시해줌.
- 실제 서비스하는 도중 에러 화면이 고객에게 노출되지 않도록 처리해야 함.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>page 지시어 - errorPage, isErrorPage 속성</title>
</head>
<body>
<%
int myAge = Integer.parseInt(request.getParameter("age")) + 10;  // 에러 발생
out.println("10년 후 당신의 나이는 " + myAge + "입니다.");  // 실행되지 않음
%>
</body>
</html>
```

- 11라인에서 내장 객체인 request로부터 "age"라는 이름의 매개변수 값을 받아와 정수로 변환함. 하지만 최초 실행 시에는 매개변수가 없으므로 null 값이 전달되어 예외(에러)가 발생함.
- 해결 방법
  - try/catch를 사용하여 직업 에러 처리.
  - errorPage, isErrorPage 속성을 사용하여 디자인이 적용된 페이지로 대체.

- try/catch 사용

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>page 지시어 - try/catch 사용하여 에러 처리</title>
</head>
<body>
<%
try {  // 예외 발생 부분을 감쌉니다.
    int myAge = Integer.parseInt(request.getParameter("age")) + 10;
    out.println("10년 후 당신의 나이는 " + myAge + "입니다.");
}
catch (Exception e) {
    out.println("예외 발생 : 매개변수 age가 null입니다.");
}
%>
</body>
</html>
```

- ErrorPage 사용

  1. ErrorPage.jsp 파일을 생성

  2. 파일 내용 작성

  3. 상단 지시어 부분에 errorPage 속성 추가

```jsp
<!-- ErrorPage.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    errorPage="IsErrorPage.jsp"%>  <!--에러 페이지 지정-->
...
```

```jsp
<!-- IsErrorPage.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    isErrorPage="true"%>  <!--isErrorPage 속성에 true를 지정-->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>page 지시어 - errorPage/isErrorPage 속성</title>
</head>
<body>
    <h2>서비스 중 일시적인 오류가 발생하였습니다.</h2>
    <p>
        오류명 : <%= exception.getClass().getName() %> <br />
        오류 메시지 : <%= exception.getMessage() %>
    </p>
</body>
</html>
```

- ErrorPage.jsp를 실행하면 주소 표시줄에는 ErrorPage.jsp가 표시되지만, 화면에는 IsErrorPage.jsp의 내용이 출력됨.
- 실제 서비스에서는 오류 메시지를 표시하지 않고 대부분 사용자에게 친근한 모습으로 디자인된 페이지를 사용하여 출력함.

<br>

#### trimDirectiveWhitespaces 속성

- jsp 파일을 실행하여 소스를 보면 상단 page 지시어가 있던 부분이 공백으로 처리됨. page 지시어가 웹 서버에서 처리된 후 공백으로 남게 되는 것임.
- 안드로이드와 같은 외부 기기와 연동 시 가끔 문제가 발생함.
- trimDirectiveWhitespaces 속성을 사용하여 공백 제거 가능함.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"
    trimDirectiveWhitespaces="true"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>page 지시어 - trimDirectiveWhitespaces 속성</title>
</head>
<body>
    <h2>page 지시어로 생긴 불필요한 공백 제거</h2>
</body>
</html>
```

<br>

#### buffer, autoFlush 속성

- JSP 파일은 서블릿 코드로 변환된 후 컴파일되어 class 파일로 만들어짐. 이를 실행한 결과물을 HTML 형태로 웹 브라우저에 보내 최종적으로 화면에 출력하는 것임.
- 이 과정에서 응답 결과를 웹 브라우저로 즉시 전송하지 않고, 출력할 내용을 버퍼에 저장했다가 일정량이 되었을 때 전송하게 됨.

- 버퍼(buffer)
  - 네트워크로 영상 데이터를 전송할 때, 작은 단위로 여러 번 전송하는 것보다 큰 단위로 묶어서 한 번에 보내는 편이 훨씬 효율적임.
  - 이때 버퍼라는 임시 저장소를 두어 데이터들이 충분히 쌓일 때까지 기다렸다가 보내는 것임.
  - JSP에서는 버퍼를 사용함으로써 포워드(forward: 페이지 전달)와 에러 페이지 처리를 할 수 있음.
  - JSP가 생성한 결과는 일단 버퍼에 저장되고 만약 실행 도중 에러가 발생하면 버퍼에 저장된 내용을 삭제하고 에러 화면을 표시함.
- page 지시어의 buffer 속성으로는 버퍼의 크기를 설정할 수 있음.
- 기본값은 8kb임.
- 사용하고 싶지 않다면 "none"으로 지정하면 됨.
- 버퍼를 사용하지 않으면 포워드나 에러 페이지 기능을 사용할 수 없음. 따라서 "none"으로 지정하는 경우는 거의 없음.
- autoFlush 속성은 버퍼가 모두 채워졌을 때의 처리 방법을 정하는 데 쓰임.
- 값은 true, false 중 선택할 수 있음.
  - true(기본값) : 버퍼가 채워지면 자동으로 플러시함.
  - false : 버퍼가 채워지면 예외를 발생시킴.
  - 플러시란 버퍼 안의 데이터를 목적지로 전송하고 버퍼를 비우는 작업을 말함.

```jsp
<!-- AutoFlushTest.jsp -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" buffer="1kb" autoFlush="false"%>  <!--버퍼 설정-->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>page 지시어 - buffer, autoFlush 속성</title>
</head>
<body>
<%
for (int i = 1; i <= 100; i++) {  // 버퍼 채우기
    out.println("abcde12345");
}
%>
</body>
</html>
```

- 2라인에서 버퍼를 "1kb"로, autoFlush를 "false"로 설정함. 버퍼 크기를 줄인 후 버퍼가 가득 차면 에러가 날 것임.
- for문을 이용해 10글자(10바이트)로 구성된 문자열을 100번 반복 출력함. 버퍼 크기에 해당하는 1kb를 출력. 이 파일에는 \<html>과 같은 태그가 포함되어 있으므로 결과적으로 1kb를 넘기게 됨.

- 버퍼를 위처럼 설정하면 JSP의 기능을 온전히 사용할 수 없음. 특수한 경우가 아니라면 거의 사용되지 않음.

<br>

### inclue 지시어

- 반복되는 부분을 별도의 파일에 작성해두고 필요한 페이지에서 include 지시어로 포함시킬 수 있음.
- `<%@ include file="포함할 파일의 경로"%>`

```jsp
<!-- 공통 UI 요소를 담은 JSP 파일(포함될 파일) -->
<%@ page import="java.time.LocalDateTime"%>
<%@ page import="java.time.LocalDate"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
LocalDate today = LocalDate.now();  // 오늘 날짜
LocalDateTime tomorrow = LocalDateTime.now().plusDays(1);  // 내일 날짜
%>
```

```jsp
<!-- 다른 JSP 파일을 포함하는 JSP 파일 -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ include file = "IncludeFile.jsp" %>  <!--다른 JSP 파일(IncludeFile.jsp) 포함-->
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>include 지시어</title>
</head>
<body>
<%
out.println("오늘 날짜 : " + today);
out.println("<br/>");
out.println("내일 날짜 : " + tomorrow);
%>
</body>
</html>

```

- 4라인에서 include 지시어를 이용하여 IncludeFile.jsp 파일을 포함시켰음. 그러면 대상 파일의 소스 자체가 이 문서에 포함됨. 그 결과 IncludeFile.jsp에서 선언한 변수인 today와 tomorrow를 사용할 수 있게 됨.

- include 지시어로 다른 문서를 포함시키면 먼저 '파일의 내용 그대로를 문서에 삽입'한 후 '컴파일'이 진행됨. 따라서 하나의 페이지가 됨.

<br>

### taglib 지시어

- taglib은 EL(표현 언어)에서 자바 클래스의 메소드를 호출하거나 JSTL(JSP 표준 태그 라이브러리)을 사용하기 위한 지시어임.
- 10장과 11장 추가 학습 예정.

<br>

## 4. 스크립트 요소(Script Elements)

- 스크립트 요소는 JSP에서 자바 코드를 직접 작성할 수 있게 해줌.
- 용도에 따라 선언부, 스크립틀릿, 표현식이 있음.
- 구체적인 과정
  - JSP는 클라이언트의 요청을 받아 실행될 때 서블릿(자바 코드)으로 변환되고, 클래스로 컴파일된 후 응답하게 됨.
  - 이 변환 과정에서 _jspService() 메소드가 생성되는데, 변환된 코드의 위치는 스크립트 요소에 따라 _jspService() 메소드 내부 혹은 외부에 놓일 수 있음.

<br>

#### 선언부(Declaration)

- 선언부에서는 스크립틀릿이나 표현식에서 사용할 멤버 변수나 메소드를 선언함.
- 서블릿으로 변환 시 _jspService() 메소드 외부에 선언됨.
- `<%! 메소드선언 %>`

<br>

#### 스크립틀릿(Scriptlet)

- JSP 페이지가 요청을 받을 때 실행되어야 할 자바 코드를 작성하는 영역임.
- 서블릿으로 변환 시 _jspService() 메소드 내부에 그대로 기술됨.
- `<% 자바 코드 %>`
- 자바에서는 메소드 내부에 또 다른 메소드를 선언하는 게 불가능함. 만약 스크립틀릿에 메소드를 선언한다면 _jspService() 내부에 또 다른 메소드를 선언하는 꼴이므로 에러가 발생함. 스크립틀릿에서는 선언부에서 정의한 메소드를 호출만 할 수 있을 뿐, 다른 메소드를 선언할 수는 없음.

<br>

#### 표현식(Expression)

- 프로그래밍 언어에서 표현식은 '실행 결과로 하나의 값이 남는 문장'을 뜻함.
- 상수, 변수, 연산자를 사용한 식, '반환값이 있는' 메소드 호출 등이 모두 표현식에 속함.
- JSP에서는 주로 변수의 값을 웹 브라우저 화면에 출력할 때 사용함.
- 스크립틀릿 안에서 변수를 출력할 때는 out.print()를 사용해야 하지만, 표현식에서는 좀 더 단순한 방법으로 출력할 수 있음.
- `<%= 자바 표현식 %>`

<br>

#### 스크립트 요소 활용

```jsp
<!-- 스크립트 요소 활용 -->
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%!  // 선언부(메서드 선언)
public int add(int num1 , int num2) {
    return num1 + num2;
}
%>
<html>
<head><title>스크립트 요소</title></head>
<body>
<%  // 스크립틀릿(자바 코드)
int result = add(10, 20);
%>
덧셈 결과 1 : <%= result %> <br />
덧셈 결과 2 : <%= add(30, 40) %>
</body>
</html>
```

- 위의 코드로 실행한 jsp 파일은 서블릿으로 변환된 후 컴파일까지 완료되어 2개의 파일이 생성됨.
- C:\workspace\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\work\Catalina\localhost\MustHaveJSP_origin\org\apache\jsp\_01DirectiveScript
- ScriptElements.jsp가 처음 실행 시 ScriptElements_jsp.java로 변환된 후 ScriptElements_jst.class로 컴파일된 것임.
- 확장자가 .java인 파일을 메모장으로 열어보면 선언부 내용은 _jspService() 외부에 선언되고, 스크립틀릿은 _jspService() 내부에 선언됨. 메소드 안에서 메소드 선언할 수 없기 때문에 스크립틀릿에 메소드 선언할 경우 에러 발생하는 것임.

<br>

# References

- [성낙현의 JSP 자바 웹 프로그래밍 - 성낙현](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791191905052&orderClick=LAG&Kc=)