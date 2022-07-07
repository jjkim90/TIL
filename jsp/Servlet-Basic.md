# Servlet 프로그래밍

<br>

> 이 문서는 [<한 권으로 끝내는 실무 Java 웹 개발서 - 김정현, 김계희>](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025) 책을 보고 정리한 문서입니다.

<br>

## 1. Servlet의 개요

- Java Servlet은 서버에서 수행되는 웹 프로그래밍 기술로서 Java 언어로 구현하는 기술임.
- 웹 클라이언트의 요청으로 웹서버에서 수행되는 Java 프로그램임.
- 구현이 쉽고 처리 성능이 높아 지금까지 가장 많이 활용하는 웹서버 프로그래밍 기술이 되었음.

<br>

### 1.1. Servlet의 구현

- Servlet 클래스를 생성할 때 구현하려는 기능과 관계없이 HttpServlet 클래스를 상속해야 함.
- Servlet 클래스는 javax.servlet.Servlet, javax.servlet.GenericServlet, javax.servlet.http.HttpServlet 중 하나를 상속해야 함.
- 웹 서버에서 수행되는 Servlet의 경우에는 주로 HttpServlet을 상속하여 구현함.
- main() 메소드는 구현하지 않으며 다음에 제시된 메소드들을 기능에 따라 선택적으로 오버라이딩하여 구현함.
- 이 메소드들은 HttpServlet 클래스가 가지고 있는 메소드들이므로 필요하면 오버라이딩하여 구현할 수 있음.
- HttpServlet 클래스의 메소드 종류
  - `init()`
  - `destroy()`
  - `service()`
  - `doGet()`
  - `doPost()`

<br>

#### Eclipse에서 Servlet 클래스 생성 프로세스

1. Dynamic Web Project의 하위 폴더에서 Java Resources의 src 우클릭 -> New -> Servlet 선택

2. Java package 항목에는 "core", Class Name 항목에는 "FirstServlet" 입력 후 Next 클릭

3. URL mappings 설정 후 Next 클릭
4. Inherited abstract methods와 doGet 체크 후 Finish 클릭
5. 오른쪽 하단 Server뷰의 Tomcat 서버 우클릭
6. Start 메뉴 항목 클릭(이미 구동 중이라면 Restart 메뉴 항목 클릭)
7. 웹 브라우저에서 http://localhost/edu/FirstServlet 을 입력
8. 서버로부터 응답된 내용으로 화면이 출력됐다면 성공

- 브라우저에서 Servlet을 요청할 때 사용하는 올바른 URL 문자열
  - `http://서버주소/컨택스트패스명/Servlet클래스의URLmappings명`
  - Servlet 소스의 클래스 정의 라인 위에 있는 @WebServlet("/FirstServlet")은 애노테이션 선언으로서 "이 클래스는 WebServlet으로서 URL mappings명이 /FirstServlet이다"는 것을 나타내는 내용임.
  - 컨택스트 패스명은 Eclipse에 만들어진 Dynamic Web Project에 대한 패스로서 하나의 Tomcat 서버에는 여러 개의 Dynamic Web Project를 등록할 수 있으며 각각의 Dynamic  Web Project는 컨택스트 패스명으로 구분함.
  - 일반적으로 Dynamic Web Project 명과 동일하게 컨택스트 패스명이 정해지며 여기서는 edu라는 이름이므로 컨택스트 패스명으로 /edu를 사용함.

```java
package core;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html; charset=utf-8");
		PrintWriter out = response.getWriter();
		out.println("<h3>안녕하세요?</h3>");
		out.close();
	}
}
```

- 12라인의 `@WebServlet("/hello")` 부분은 URL mappings 정보임. 바꾸고 싶다면 여기에 직접 수정하면 됨.
- 14라인은 HttpServlet에서 제공되는 메소드를 오버라이딩하는 것임. doGet() 메소드가 호출될 때 아규먼트로 HttpServletRequest 객체와 HttpServletResponse 객체가 전달됨. 전자는 요청과 관련된 정보를 추출하는 용도로 사용하고 후자는 응답 처리하는 데 사용함.
- 한글이 포함되는 HTML 응답 처리는 HttpServletResponse 객체의 getWriter() 메소드를 호출하기 전에 setContentType("text/html; charset=utf-8") 메소드를 먼저 호출해야 함.
- 16라인은 요청해온 브라우저에 대한 출력용 스트림 객체 생성. 클라이언트로 텍스트 형식의 콘텐츠를 응답할 때 반드시 호출해야 하는 메소드임.  getWriter()가 리턴하는 객체는 PrintWriter 객체로서 텍스트 기반의 출력 스트림 객체임.
- 17라인은 java.io.PrintWriter 객체의 출력 메소드들을 사용하여 브라우저로 응답을 처리하는 코드임. 응답 형식이 HTML이므로 print()와 println() 메소드의 차이가 거의 없음. HTML에서는 개행 문자를 공백 한 개로 인식하기 때문임.
- 18라인은 브라우저로의 모든 출력이 끝나면 사용했던 출력 스트림 객체의 close() 메소드를 호출하여 오픈한 자원을 해제함.
- 새로 만든 Servlet 클래스의 경우 Tomcat 서버가 바로 인식하지 못하는 관계로 Tomcat 서버를 재시작해야 함. 이미 인식하고 있는 Servlet 클래스를 수정한 경우에는 관계 없음.

<br>

#### Servlet의 등록과 매핑

- Java 프로그램으로 구현되는 Servlet 소스는 컴파일을 통해서 .class라는 확장자를 갖는 수행 파일이 되는데 웹에서 .class라는 수행 파일을 갖는 파일의 요청은 Servlet보다 먼저 소개된 기술인 Applet에서 이미 사용되고 있음.
- Servlet 클래스 파일의 경우에는 서버에서 Servlet 프로그램으로 인식되어 처리되도록 등록과 매핑이라는 설정을 web.xml이라는 디스크립터 파일에 저장해야 함.
- web.xml은 웹 애플리케이션에 대한 다양한 정보를 설정하는 파일로서 디스크립터 파일이라고도 하며 아래와 같은 구성으로 WEB-INF 폴더에 만들어야 함.
- 생성되는 Servlet마다 등록하는 기능의 \<servlet> 태그와 매핑(URL mappings)하는 기능의 \<servlet-mapping> 태그를 정의해야 함.

```xml
...
<display-name>edu</display-name>
<servlet>
	<servlet-name>HelloServlet</servlet-name>
    <servlet-class>core.HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>HelloServlet</servlet-name>
    <url-pattern>/hellonew</url-pattern>
</servlet-mapping>
```

- Servlet 3.0부터는 위와 같이 web.xml이라는 디스크립터 파일에 Servlet을 등록하고 매핑하는 태그를 작성하는 대신 Servlet 소스 안에 Java의 애노테이션 구문으로 선언하는 방법이 추가되었음.
  - `@WebServlet("/FirstServlet")`

<br>

#### Servlet 정의 애노테이션

- Servlet 3.0부터는 web.xml에 작성하던 Servlet 등록과 매핑, 초기 파라미터 설정, 리스너나 필터 등록과 같은 내용들을 Servlet 소스 내에서 애노테이션 구문으로 구현 방법을 지원하고 있음.
- 애노테이션을 사용하면 web.xml에 일일이 설정 태그를 작성해주지 않아도 됨.
- Servlet 3.0에서 지원하는 애노테이션 리스트
  - `@WebServlet` : Servlet 프로그램의 등록과 매핑을 정의함.
  - `@WebInitParam` : Servlet 프로그램에 전달할 초기 파라미터를 정의함.
  - `@WebListener` : 리스너를 정의함.
  - `@WebFilter` : 필터를 정의함.
  - `@MultipartConfig` : Servlet 프로그램에서 다중 파티션으로 전달되는 파일 업로드를 처리할 수 있음을 정의함.
- `@WebServlet` 애노테이션은 반드시 사용함.
- `@WebServlet({"/hello1", "/hello2"})` 처럼 URL mappings 명을 2개 이상 설정하는 것도 가능함.

<br>

### 1.2. Servlet의 수행

- Java EE 기술에서 Servlet과 JSP는 웹 컨테이너(엔진이라고도 함)가 관리하고 수행하는 Web 컴포넌트로서, 다양한 Web 컴포넌트와 정적 리소스들 그리고 추가 라이브러리들이 모여 하나의 웹 애플리케이션을 구성함.
- 용어 정리
  - Application Server 기능을 포함한 Web Server(웹서버) : 일반적인 웹서버 기능에 Servlet과 같은 서버용 웹 애플리케이션을 수행시키고 그 결과를 HTTP 기반으로 서비스하는 서버로서 Web Application Server라고 하며 간단하게 WAS라고도 부름.
  - Web Container(웹 컨테이너) : 웹 애플리케이션을 관리하고 수행시키는 프로그램.
  - Web Application(웹 애플리케이션) : 웹 컨테이너가 관리하는 웹 자원들로서 Eclipse에서 생성하는 Dynamic Web Project는 하나의 웹 애플리케이션이 됨.
  - Web Components(웹 컴포넌트) : Servlet과 JSP와 같이 웹서버에서 수행되는 자원들을 뜻함.
  - web application DD(web.xml) : 웹 애플리케이션만의 환경 파일. 웹 애플리케이션이 웹 컨테이너에 배치될 때 인식됨.
  - static resources(정적 리소스) : HTML, CSS, JavaScript 등 웹 클라이언트에서 처리되는 자원들임.
  - Library(라이브러리) : 확장 API. Servlet이 내장하고 있는 API 외에 추가로 필요로 하는 API들을 의미함. 주로 jar 파일로 구성되며 JDBC 프로그래밍 시 사용되는 JDBC 드라이버도 library에 포함됨.
- 하나의 웹 컨테이너에 두 개 이상의 웹 애플리케이션이 배치(deploy)될 수 있으며 웹 애플리케이션 단위로 수행하고 관리함.
- 웹 컨테이너는 구동될 때 인식되는 웹 애플리케이션들을 하나 하나 컨텍스트 객체로 생성하여 관리함.
- 이때 컨텍스트 객체마다 고유의 이름이 부여되는데 이를 컨텍스트명이라고 함.
- 자바의 Dynamic Web Project 폴더를 톰캣에서는 컨텍스트라고 부르고, 개발자들은 웹 어플리케이션이라고 부름.
- 브라우저 -> Servlet 수행 요청 시 처리 순서
  1. 브라우저에서 Servlet을 요청하는 HTTP URL 문자열을 사용하여 수행을 요청함.
  2. WAS 안의 웹서버가 Servlet 수행 요청임을 인식하고 웹 컨테이너에게 Servlet 수행을 요청함.
  3. 웹 컨테이너는 스레드를 하나 준비하여 해당 Servlet을 수행함.
  4. 구동되었던 스레드를 반납하고 리턴함.
  5. Servlet의 수행 결과를 웹서버에 전송함.
  6. 웹서버는 Servlet의 수행 결과로 만들어진 출력 버퍼의 내용으로 HTTP 응답 헤더와 응답 바디를 구성하여 브라우저로 전송함.
- 웹 컨테이너는 클라이언트에서 전송된 요청 정보를 가지고 HttpServletRequest와 HttpServletResponse 객체를 생성한 후, 요청된 Servlet의 객체가 생성된 상태인지 검사함. 만약 Servlet 객체가 생성되어 있는 상태라면 바로 수행시키고 객체가 생성된 상태가 아니라면 해당 Servlet의 클래스 파일을 찾아서 로딩한 후 객체를 생성하여 수행시킴. 또한 생성된 Servlet 객체는 서버를 종료할 때까지 또는 웹 애플리케이션이 리로드될 때까지 객체 상태를 계속 유지함.

<br>

#### Servlet의 최초 요청과 두 번째 이후 요청

- 최초 요청
  - 브라우저의 Servlet 요청 -> JVM에 로딩 후 객체 생성 -> init() -> service(request, response) -> doGet() or doPost() -> 요청 보낸 브라우저로 출력 버퍼의 내용 리턴.
- 두 번째 이후 요청
  - 브라우저의 Servlet 요청 -> service() -> doGet() or doPost() -> 요청 보낸 브라우저로 출력 버퍼의 내용 리턴.

<br>

#### Servlet 객체가 메모리에서 해제되는 시점

- 컨테이너(서버)가 종료될 때
- 웹 애플리케이션이 리로드될 때
- 자동 리로드가 설정된 상태에서 Servlet이 재컴파일되었을 때

<br>

#### 여러 브라우저가 하나의 Servlet을 동시에 요청할 때

- 웹 컨테이너가 Servlet 수행을 처리할 때는 다중 스레드 방식을 사용함.
- 여러 브라우저가 동시 요청하면 각 요청마다 스레드를 구동시켜 병렬로 처리함. 각 요청마다 Servlet 클래스의 객체를 생성하는 것이 아니라 하나의 Servlet 객체를 공유하여 여러 스레드가 병렬로 처리하는 방식임. 적은 메모리로 여러 개의 요청 동시 수행 가능.

<br>

#### Servlet의 수행 흐름 점검

```java
// FlowServlet.java

package core;

import java.io.IOException;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/FlowServlet")
public class FlowServlet extends HttpServlet {
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init() 메소드 호출!");
	}
	public void destroy() {
		System.out.println("destroy() 메소드 호출!");
	}
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("service() 메소드 호출!");
	}
}
```

- Servlet에서 표준 출력하는 결과는 브라우저로 응답되지 않으며 Tomcat 서버의 콘솔창에 출력됨.
- Tomcat 서버를 재시작하고 브라우저에 http://localhost/edu/FlowServlet을 사용하여 FlowServlet 수행을 요청할 경우 브라우저의 도큐먼트 영역에는 아무 것도 출력되지 않음.
- 이클립스 하단의 Console 탭 뷰를 보면 init() 메소드 호출과 service() 메소드 호출이 출력되는 것을 볼 수 있음.
- 새로고침을 눌러 두 번째 요청을 할 경우 service() 메소드 호출이 추가로 출력됨.
- 여러 번 재요청 해도 service() 메소드 호출만 반복 출력됨.
- FlowServlet 클래스의 소스를 약간 수정하고 저장하면 이클립스에 의해 Servlet이 재컴파일되고 Tomcat이 edu라는 컨텍스트를 리로딩함. 이때 생성되어 있던 Servlet 객체가 모두 메모리에서 해제되므로 destoy() 메소드 호출 결과를 확인할 수 있음.

<br>

#### Servlet의 멤버 변수와 지역 변수 점검

```java
// MemberLocalServlet.java

package core;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/MemberLocalServlet")
public class MemberLocalServlet extends HttpServlet {
	int member_v = 0;
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html; charset=utf-8");
		PrintWriter out = response.getWriter();
		int local_v = 0;
		member_v++;
		local_v++;
		out.print("<h2>member_v(멤버변수) : " + member_v + "</h2>");
		out.print("<h2>local_v(지역변수) : " + local_v + "</h2>");
		out.close();
	}
}
```

- 15라인의 member_v 멤버 변수는 MemberLocalServlet의 객체가 생성될 때 메모리가 할당되며 여러 클라이언트에 의해 공유됨. 또한, 이 Servlet의 객체 상태가 유지될 때까지 유효한 변수 영역을 사용함.
- 19라인의 local_v 지역 변수는 doGet() 메소드가 호출될 때마다 메모리 영역을 할당하고 수행이 끝나면 메모리 영역이 해제됨. 그리고 지역 변수는 각 브라우저 요청에 대하여 메모리 영역을 개별적으로 할당하여 사용함.
- 브라우저의 최초 요청시 MemberLocalServlet 클래스의 객체가 생성되고 이때 멤버 변수인 member_v 변수는 힙(heap)이라는 메모리 영역에 할당됨. 지역 변수인 local_v 변수는 doGet() 메소드가 호출되어 수행하는 동안 스택(stack)이라는 메모리 영역에 할당하고 메소드 호출이 끝나면 해제됨.
- 브라우저로 MemberLocalServlet 클래스를 요청하면 처음에는 두 변수 모두 1로 출력됨.
- 새로고침이 발생할 때마다(새로운 요청이 발생할 때마다) 멤버변수는 1씩 증가하지만 지역변수는 계속 1임.
- 멤버변수 3, 지역변수 1일때 다른 브라우저를 동작시켜서 요청하면 멤버변수 4, 지역변수 1의 출력을 볼 수 있음.
- 어떠한 브라우저가 요청하든 생성된 Servlet 객체는 재사용됨.

<br>

### 1.3. Servlet의 요청과 응답

- HTTP 요청 메시지와 응답 메시지 구성
  - 요청 시
    - 요청 라인 : 요청 라인에는 GET과 POST와 같은 요청 방식이 들어감.
    - 요청 헤더 메시지 : name과 value로 구성된 다양한 요청 헤더가 구성됨.
    - 요청 바디 : POST 방식 요청의 경우에만 Query 문자열이 들어감.
  - 응답 시
    - 응답 상태 라인 : 응답 상태코드가 들어감. 200은 요청이 성공했음을 나타냄.
    - 응답 헤더 메시지
    - 응답 바디 : 브라우저에 출력되는 응답 콘텐츠가 구성됨.
- 실제 요청 헤더와 응답 헤더는 크롬 브라우저에서 지원하는 개발자 도구(F12)의 네트워크 탭에서 확인할 수 있음.
- 요청 헤더와 응답 헤더 정보를 보면 요청된 서버의 자원에 대한 URL, 요청 방식, 요청한 브라우저에 대한 정보, 응답 결과, 응답되는 콘텐츠의 타입 정보와 길이 정보 등을 점검할 수 있음.
- Servlet에서 요청 헤더에 포함한 요청 정보를 추출하고자 하는 경우 HttpServletRequest 객체를 사용함.
  - HttpServletRequest는 클라이언트로부터 요청이 올 때마다 생성되는 객체로서 응답 시 사라짐.
  - 이 객체에는 요청을 보내온 클라이언트에 대한 정보 그리고 요청 헤더 정보 등이 초기화됨.
  - 리퀘스트 객체가 사용하는 메소드
    - `getHeader(name)` : 요청 헤더 메시지에서 name명으로 전달되는 value 값을 추출.
    - `getHeaders(name)` : 요청 헤더 메시지에서 name명으로 전달되는 value 값들을 추출.
    - `getHeaderNames()` : 요청 헤더 메시지에서 name명들만 추출함.
    - `getContentLength()` : 요청 바디의 길이 정보를 추출함.
    - `getContentType()` : 요청 바디의 타입 정보를 추출함.
    - `getCookies()` : 요청 헤더의 쿠키 정보를 추출함.
    - `getRequestURI()`  : 요청 URI를 추출함.
    - `getRequestURL()` : 요청 URL을 추출함.
    - `getQueryString()` : 요청 시 추가로 전달되는 name과 value로 구성된 문자열인 Query 문자열을 추출함.
    - `getProtocol()` : 요청 프로토콜 정보를 추출함.
    - `getMethod()` : GET, POST와 같은 요청 방식에 대한 정보를 추출함.
    - `getParameter(name)` : name명으로 전달되는 Query 문자열에서의 value 값을 추출함.
    - `getParameterValues(name)` : name명으로 전달되는 Query 문자열에서의 value 값들을 추출함.
    - `getParameterNames()` : name과 value로 구성된 문자열인 Query 문자열에서 name 정보만을 추출함.
    - `getRemoteAddr()` : 요청을 보내온 클라이언트의 IP 주소를 추출함.
    - `getContextPath()` : 요청된 자원이 존재하는 컨텍스트의 패스 정보를 추출함.
    - `getServerName()` : 요청이 전달된 서버의 도메인명을 추출함.
    - `getPart(name)` : 클라이언트에서 전송된 multipart/form-data 형식의 데이터에서 name이라는 이름으로 전달된 Part 객체를 추출함.
    - `getParts()` : 클라이언트에서 전송된 multipart/form-data 형식의 데이터에서 모든 Part 객체들를 추출함.
- Servlet에서 응답 처리와 관련해서는 HttpServletResponse 객체를 사용함.

<br>

#### 유용한 요청 정보 추출

```java
// HttpServletRequest를 사용한 요청 정보 추출
package core;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.regex.Pattern;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/requestinfo")
public class RequestInfoServlet extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		String contextPath = request.getContextPath();
		String method = request.getMethod();
		String protocol = request.getProtocol();
		String remoteAddr = request.getRemoteHost();
		String requestURI = request.getRequestURI();
		String requestURL = new String(request.getRequestURL());
		String serverName = request.getServerName();
		String userAgent = request.getHeader("user-agent");
		String referer = request.getHeader("referer");
		String clientMachine = "";
		boolean result = Pattern.matches(".*[win16|win32|win64|linux|mac].*", userAgent.toLowerCase());
        if(result)
        	clientMachine = "PC";
        else 
        	clientMachine = "모바일";
		String browser = "";
		if (userAgent.indexOf("Trident") > 0 || userAgent.indexOf("MSIE") > 0) {
			browser = "IE";
		} else if (userAgent.indexOf("Opera") > 0) {
			browser = "Opera";
		} else if (userAgent.indexOf("Firefox") > 0) {
			browser = "Firefox";
		} else if (userAgent.indexOf("Safari") > 0) {
			if (userAgent.indexOf("Edge") > 0) {
				browser = "Edge";
			} else if (userAgent.indexOf("Chrome") > 0) {
				browser = "Chrome";
			} else {
				browser = "Safari";
			}
		}
		response.setContentType("text/html; charset=utf-8");
		PrintWriter out = response.getWriter();
		out.println("<h3>요청 정보를 통해서 추출한 내용</h3>");
		out.println("<ul>");
		out.println("<li>요청 컨텍스트 패스 : " + contextPath + "</li>");
		out.println("<li>요청 방식 : " + method + "</li>");
		out.println("<li>요청 프로토콜 : " + protocol + "</li>");
		out.println("<li>요청 클라이언트 주소 : " + remoteAddr + "</li>");
		out.println("<li>요청 URI : " + requestURI + "</li>");
		out.println("<li>요청 URL : " + requestURL + "</li>");
		out.println("<li>요청 서버명 : " + serverName + "</li>");
		out.println("<li>요청 클라이언트 시스템 종류 : " + clientMachine + "</li>");
		out.println("<li>요청 브라우저 종류 : " + browser + "</li>");
		out.println("<li>이 콘텐트를 요청한 웹 페이지 : " + referer + "</li>");
		out.println("</ul>");
		out.close();
	}
}

```

- 요청해온 클라이언트가 PC인지 모바일인지 체크하는 부분과 브라우저의 종류를 체크하는 부분 그리고 Servlet을 요청한 페이지가 누구인지 추출하는 referer 정보도 점검함.
- 26라인은 referer라는 이름으로 전달되는 요청 헤더 정보를 추출함. 이 요청 헤더 정보는 URL을 입력하여 Servlet을 직접 요청한 경우에는 null이 추출되고 \<a>, \<form> 등 HTML 문서를 통해서 요청되는 경우에는 요청한 HTML 문서에 대한 URL이 추출됨.
- 27~32라인은 user-agent라는 이름으로 전달되는 요청 헤더 정보를 이용하여 PC용 운영체제로부터의 요청인지 체크함.
- 33~48라인은 요청을 보내온 브라우저의 종류를 체크함.
- 테스트용 서버와 클라이언트가 동일 시스템인 관계로 서버의 도메인명에 localhost를 사용하였기 때문에 요청 서버명에 localhost가 그리고 요청 클라이언트 주소에는 0:0:0:0:0:0:0:1(IPv6의 자기 자신을 의미하는 루프백 주소) 출력됨.
- URI는 Uniform Resource Identity의 약어로 서버에 요청하는 자원에 대한 패스 정보만을 추출하게 됨.
- 마지막에는 이 Servlet이 요청될 때 다른 자원을 거치치 않고 직접 URL을 입력하여 요청했기 때문에 referer 정보는 null이 추출되는 것을 볼 수 있음.
- 다른 페이지에서 요청할 경우 RequestInfoServlet을 요청한 페이지의 URL 정보가 추출됨. 이 referer 정보는 이전 페이지로 되돌아가기 기능을 처리할 때 유용하게 사용함.

<br>

#### 다양한 타입의 응답 처리

- 리퀘스트/리스폰 -> HTML, XML, JSON, Image 등 Mime Type, 문자 세트 정보
- setContentType()
- 문자면 response.getWrite(); 바이너리면 response.getOutputStream

<br>

## 2. Query 문자열 처리

### 2.1. GET 방식 Query 문자열 처리

#### GET 방식으로 전달되는 Query 문자열 추출

request.getQueryString()

getParameter(name) -> 밸류 1개

getParameterValues(name) -> 밸류여러개

<br>

### 2.2. POST 방식 Query 문자열 처리

- POST 요청 방식은 GET 방식을 보완하기 위해 추가된 요청 방식임.
- 전달되는 Query 문자열의 길이에 제한이 없고 내용이 브라우저의 주소 필드에 보이지 않음.
- POST 요청 방식은 Query 문자열이 요청 바디에 담겨 전달됨. (GET 방식은 요청 바디가 비어 있음.)
- POST 방식의 요청은 \<FORM> 태그를 사용하거나, JavaScript로 구현하여 요청할 수 있음.
- `request.getQueryString()` 불가능함. 
- `request.getParameter(name)`, `request.getParameterValues(name)` 사용함.

```java
@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		doGet(request, response);
	}
```

```html
<h1>이름과 좋아하는 숫자 전달하기(POST)</h1>
<form method="POST" action="/edu/querypost">
	<input type="text" name="guestName" placeholder="이름을 입력하세요" required><br>
	<input type="number" name="num" placeholder="좋아하는 숫자를 입력하세요" required><br>
	<input type="submit" value="요청(POST)">
</form>
```

<br>

### 2.3. multipart/form-data 처리(파일 업로드)

- 클라이언트의 하드디스크에 존재하는 파일을 선택하여 서버에 전달하는 것을 파일 업로드라고 함.

<br>

#### @MultipartConfig 애노테이션

<br>

#### 멀티 파트 추출 API

<br>

#### multipart/form-data 형식의 데이터 점검

<br>

#### 파일 업로드 기능 구현 점검

<br>

## 3. 상태 정보 관리

- 웹브라우저에서 웹 서버에 정보를 요청하면서 만들어진 결과물을 상태 정보라고 함.
  - 무상태(Stateless)
    - 서버가 클라이언트의 상태를 보존하지 않음을 의미함.
    - 서버의 확장성이 높아 대량의 트래픽 발생 시에도 대처 수월함.
    - 클라이언트 요청에 상대적으로 Stateful보다 더 많은 데이터 소모됨. (최종 목적을 위해 지나는 과정마다 점점 전달해야 하는 내용이 많아짐.)
  - 상태 유지(Stateful)
    - 서버가 클라이언트의 상태를 보존함을 의미함.
    - 대량의 트래픽이 몰려 서버를 긴급하게 늘려 서버가 바뀔 경우 문제가 발생함.
- HTTP 프로토콜은 기본적으로 Stateless 무상태 특징을 기본적으로 가지고 있음. 특별한 일이 없다면 무상태를 지향해야 하며 정말 필요한 경우에만 상태 유지를 해야 함.

- 상태 정보 유지 방법
  - Cookie 기술을 이용한 방법
  - HttpSession 기술을 이용한 방법 -> Servlet 기술로 구현함.
  - URL 문자열 뒤에 추가하는 방법
  - \<form> 태그의 hidden 타입을 사용하는 방법

<br>

#### Cookie 기술 : 클라이언트 저장

- 각 클라이언트별 상태 정보를 브라우저 안에 저장함.
- 브라우저에 저장하므로 보안이 중요한 정보는 저장하면 안됨.
- 저장하면서 3년 내의 유효시간을 설정할 수 있음.
- 저장 위치가 클라이언트이므로 웹서버에 부담이 되지 않는 방법이므로 가벼운 정보를 일정 시간 동안 유지하려는 경우 많이 선택하는 방법임.

<br>

#### HttpSession 기술 : 서버 저장

- 클라이언트별 상태 정보를 웹서버의 HttpSession 객체에 저장함.
- 서버에 저장하므로 보안이 중요한 정보를 저장할 수 있음.
- 모든 브라우저의 상태 정보를 웹서버 혼자서 저장하므로 웹서버에 부담이 될 수 있기 때문에 저장을 유지하는 시간은 브라우저가 구동되어 있는 동안으로 제한하고 있음.
- 서버에 저장하므로 관리가 용이하며 다양한 기능을 구현할 수 있음.
- 로그인 기능, 쇼핑 카트 기능 등 모두 HttpSession 기술을 사용함.

<br>

### 3.1. HttpSession 객체를 활용하는 상태 정보 관리

- HttpSession 객체
  - 세션 객체라고도 함
  - Servlet 프로그래밍 API 중 하나인 javax.servlet.http 패키지의 HttpSession 인터페이스를 객체로 생성함.
  - 인터페이스이므로 객체를 생성하려면 팩토리 메소드(대신 객체를 생성해주는 메소드)를 사용해야 함.
- 팩토리 메소드 특징
  - HttpSession 객체는 HttpServletRequest 객체의 getSession() 또는 getSession(true)를 호출하여 생성함.
  - HttpSession 객체는 클라이언트별로 한 개의 객체만 생성됨. 클라이언트에 대한 HttpSession 객체가 생성된 이후에는 더 이상 생성되지 않으며 생성된 객체를 계속 사용함.
  - HttpSession 객체는 자동 생성되는 것이 아니므로 필요 시 Servlet 프로그램 안에서 직접 생성해서 사용함.
  - 서버에 생성되는 HttpSession 객체는 세션 ID라고 하는 ID가 한 개 부여되고 이 ID로 HttpSession 객체를 관리함.
  - 브라우저에 보관되는 세션 ID는 브라우저가 구동되어 있는 동안만 유효함.
  - 서버에 생성된 HttpSession 객체는 클라이언트로부터의 요청이 일정 시간 전송되지 않으면 자동 삭제됨.
  - 서버에 생성된 HttpSession 객체는 필요 시 강제 삭제가 가능함.
  - HttpSession 객체에 보관되는 상태 정보는 객체로 만들어서 유일한 이름과 함께 저장함. 즉, name 과 value 쌍으로 저장함.

<br>

#### HttpSession을 이용한 상태 정보 유지 구현 과정

1. HttpSession 객체를 생성하거나 추출함.
   `HttpSession session = request.getSession();`
2. HttpSession 객체에 상태 정보를 보관할 객체를 등록함.(한 번만 등록함)
   `session.setAttribute("xxx", new Date());`
3. HttpSession 객체에 등록되어 있는 상태 정보 객체의 참조값을 얻어서 사용함. (읽기, 변경)
   `Date ref = (Date)session.getAttribute("xxx");`
4. HttpSession 객체에 등록되어 있는 상태 정보 객체가 더 이상 필요 없으면 삭제할 수도 있음.
   `session.removeAttribute("xxx");`
5. HttpSession 객체는 웹서버에 의해 자동 삭제되기도 하지만 필요 시 직접 삭제할 수도 있음.
   `session.invalidate();`

<br>

#### HttpSession 객체의 생성과 추출

- `getSession()` : HttpSession 객체를 추출하거나 새로 생성함.
- `getSession(true)` : HttpSession 객체를 추출하거나 새로 생성함.
- `getSession(false)` : HttpSession 객체를 추출하여 리턴하고 없으면 null을 리턴함.

<br>

#### HttpSession 객체의 ID

- HttpSession 객체가 생성될 때 세션 ID가 하나 부여되며 이 세션 ID는 요청을 보내온 클라이언트의 브라우저에 Cookie 기술을 이용하여 JSESSIONID라는 이름으로 저장되며 브라우저는 서버에 요청을 보낼 때마다 이 Cookie 정보를 요청 헤더에 담아 서버로 전송함.
- 브라우저에 저장되는 세션 ID에 대한 Cookie는 최대 유지 시간이 브라우저가 구동되어 있는 동안임.
- 브라우저가 재구동되어 세션 ID를 분실하게 되면 서버에 생성된 HttpSession 객체는 더 이상 사용할 수 없음.
- 클라이언트로부터 일정 시간 동안 요청이 없는 경우(Inactive Interval Time : 기본 30분)도 서버에 생성된 HttpSession 객체는 더 이상 사용할 수 없음.

<br>

#### HttpSession의 기타 주요 메소드

- `getAttributeNames()` : HttpSession 객체에 등록된 객체들의 이름을 추출함.
- `getCreationTime()` : HttpSession 객체가 만들어진 시간을 밀리초 단위로 리턴함.
- `getid()` : HttpSession 객체에 지정된 세션 ID를 리턴함.
- `getLastAccessedTime()` : 클라 요청이 마지막으로 시도된 시간을 밀리초로 리턴함.
- `getMaxInactiveInterval()` : 클라의 요구가 없을 때 서버가 현재의 HttpSession 객체를 언제까지 유지할지를 초시간 단위로 리턴함.
- `isNew()` : HttpSession 객체가 생성된 경우에는 true를 리턴하고 기존의 HttpSession 객체가 유지되고 있는 경우라면 false를 리턴함.
- `setMaxInactiveInterval(seconds)` : HttpSession 객체의 Inactive Interval Time을 설정함.(초단위)

<br>

#### HttpSession 객체의 생성, 추출, 삭제 구현

```java
package core;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Date;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
@WebServlet("/sessiontest")
public class SessionTestServlet extends HttpServlet {

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html; charset=utf-8");
		PrintWriter out = response.getWriter();
		String command = request.getParameter("comm");
		HttpSession session = request.getSession();		
		String msg="";
		long time = session.getCreationTime();
		String id = session.getId();
	   if(command.equals("view")) {
			if(session.isNew()) {
				msg = "세션 객체 생성 : "; 
			} else {
				msg = "세션 객체 추출 : "; 
			}
			msg += "<br>id : " + id + " <br>time : " +
			                new Date(time);
		} else if (command.equals("delete")) {
			session.invalidate();
			msg = id + "을 id로 갖는 세션 객체 삭제!!";
		} else {
			msg = "요청시 Query 문자열로 comm=view 또는 comm=delete 를 "
					+ "전달해주세요!!";
		}
		out.print("<h2>"+ msg+"</h2>");
		out.close();
	}
}
```

- 17라인은 "comm"이라는 이름으로 전달되는 Query 문자열의 value 값을 추출함. 이 예제는 브라우저에서 요청할 때 URL 문자열 뒤에 comm=view 또는 comm=delete 등의 Query 문자열을 GET 방식으로 전달해야 함.
- 18라인은 HttpSession 객체를 생성 또는 추출하는 메소드 호출임.
- 20라인은 HttpSession 객체의 생성 시간을 추출함.
- 21라인은 생성된 HttpSession 객체의 세션 ID를 추출함.
- 23라인은 생성된 HttpSession 객체가 이번 요청에서 생성된 것인지 점검함.
- 30라인은 생성된 HttpSession 객체를 삭제함.
- 정해진 규격의 Query 문자열을 전달하지 않고 SessionTestServlet을 요청하면 소스 22라인에서 NullPointerException이 발생되어 500 응답 상태 코드 출력됨.

<br>

### 3.2. HttpSession 객체의 활용

#### 브라우저별 요청 카운트 유지

```java
package core;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
@WebServlet("/CountServlet")
public class CountServlet extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html; charset=utf-8");
		PrintWriter out = response.getWriter();
		HttpSession session = request.getSession();
		if(session.getAttribute("cnt") == null) {
			session.setAttribute("cnt", new int[1]);
		}
		int[] count = (int[])session.getAttribute("cnt");
		count[0]++;
		out.print("<h3>당신은 "+ count[0] + 
				                       "번째 방문입니다.</h3>");		
		out.close();
	}
}
```

<br>

#### HttpSession 객체를 이용해서 로또 응모 횟수 제한하기

```java
package core;

import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

@WebServlet("/lottolimit")
public class LottoServletLimit extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		HttpSession session = request.getSession();
		if (session.getAttribute("lottocnt") == null) {
			session.setAttribute("lottocnt", new int[1]);
		}
		int[] count = (int[]) session.getAttribute("lottocnt");
		int answer = (int) (Math.random() * 10) + 1;
		int input = Integer.parseInt(request.getParameter("guess"));

		String msg = "";
		if (++count[0] > 3) {
			msg = "<h3>더이상 응모할 수 없습니다.</h3><h3> 브라우저를 재기동하여 응모하세요.</h3>";
		} else {
			if (answer == input) {
				msg = "<h3>축하합니다... 당첨되었어요!!</h3>";
				count[0] = 4;
			} else {
				msg = "<h3>다음 기회를....</h3><a href='/edu/servletexam/lottolimit.html'>재도전</a>";
			}
		}
		response.setContentType("text/html; charset=utf-8");
		PrintWriter out = response.getWriter();
		out.println(msg);
		out.close();
	}
}
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>LottoLimit</title>
</head>
<style>
#title{ color : red;}
</style>
<body>
	<h2 id = "title">로또 번호를 맞춰보세요!!!</h2>
	<hr>
	<form method = "get" action = "/edu/lottolimit">
		1부터 10까지의 숫자를 입력하세요 :
		<input name = "guess" type="number" min="1" max ="10" required>
		<input id = "img" type="image" src= "../image/clover.jpg" width=30px>
	</form>
</body>
</html>
```

<br>

## 4. 요청 재지정

- 요청 재지정이란 클라이언트에서 요청한 Servlet의 응답 대신 다른 자원(Servlet, JSP, HTML 등)의 수행 결과를 클라이언트에 대신 응답하는 기능.
- redirect 방법과 forward 방법이 있음.
- forwad
  - 클라이언트에서는 요청이 재지정된 사실을 알 수 없음.
  - 동일 서버에서도 동일 웹 애플리케이션의 자원으로만 요청을 재지정할 수 있음.
- redirect
  - 302라는 응답 코드와 재요청할 B자원에 대한 URL 정보를 가지고 응답함.
  - 302라는 응답 코드의 의미는 "요구한 데이터가 변경된 URL에 있음을 명시"라는 의미임.
  - 브라우저가 직접 요청하여 응답 받게 되므로 브라우저 주소 필드의 URL 문자열이 B에 대한 내용으로 변경됨.
  - 클라이언트 사용자는 요청이 재지정된 사실을 알게 됨.
  - 동일 서버뿐만 아니라 다른 웹사이트의 자원으로도 요청을 재지정할 수 있음. 요청 재지정의 대상에 제한이 없음.

<br>

#### 요청 재지정 구현 시 사용되는 API

- forward
  - forward 기능으로 요청을 재지정하는 경우에는 RequestDispatcher 객체의 forward() 메소드를 사용함.
  - RequestDispatcher 객체 생성 시 요청을 재지정할 대상의 URI(컨텍스트 패스를 생략한 URI)를 지정하며 forward() 메소드 호출 시에는 HttpServletRequest 객체와 HttpServletResponse 객체를 전달함.
  - 동일 웹 애플리케이션의 자원으로만 forward 할 수 있게 제한하기 위하여 컨텍스트 패스를 생략해야 함.
- redirect
  - HttpServletResponse 객체의 sendRedirect() 메소드를 사용함.
  - sendRedirect() 호출 시 아규먼트로 재지정할 대상의 URI 또는 IRL을 설정함.

<br>

### 4.1. forward를 사용한 요청 재지정

```java
package core;
import java.io.IOException;
import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
@WebServlet("/forward")
public class ForwardServlet extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("ForwardServlet 수행");
		RequestDispatcher rd = 
				request.getRequestDispatcher("/edu/servletexam/output.html");
	/*	RequestDispatcher rd = 
				request.getRequestDispatcher("http://www.naver.com/");*/
		rd.forward(request,  response);
	}
}
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>forward 요청 재지정</title>
</head>
<body>
	<h3>클라이언트가 요청한 서블릿을 대신하여 응답했어요..</h3>
</body>
</html>
```

- /edu라는 컨텍스트 패스를 생략해야 함. 컨텍스트 패스는 자동으로 추가됨.
- 만일 /edu/servletexam/output.html과 같이 컨텍스트 패스를 직접 설정하면 /edu/edu/servletexam/output.html과 같이 컨텍스트 패스 /edu가 추가되어 재지정될 대상을 찾을 수 없게 되어 오류가 발생함.
- 브라우저에서 ForwardServlet을 요청하면 output.html의 콘텐츠 내용이 대신 응답하게 됨. `localhost/edu/forward`
- forward 되는 대상 URI를 네이버의 IRL 문자열로 변경할 경우 forward 되는 대상을 동일한 웹 애플리케이션 범위 내로 제한함으로 다른 웹사이트는 불가능함. 오류 화면이 응답됨.

<br>

### 4.2. redirect를 사용한 요청 재지정

```java
package core;
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
@WebServlet("/redirect")
public class RedirectServlet extends HttpServlet {
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("RedirectServlet 수행");
		response.sendRedirect("/edu/servletexam/output.html");
		//response.sendRedirect("http://www.naver.com/");
	}
}
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>redirect 요청 재지정</title>
</head>
<body>
	<h3>클라이언트가 요청한 서블릿을 대신하여 응답했어요..</h3>
</body>
</html>
```

- 재지정될 대상에 대한 정보는 URI 문자열과 URL 문자열의 제한이 없음.
- 다른 웹사이트의 URL 문자열도 설정할 수 있음.
- 브라우저에서 RedirectServlet을 요청하면 output.html의 콘텐츠 내용이 대신 응답함. 
- 브라우저의 주소 입력 필드에는 RedirectServlet 요청 시 사용된 URL 문자열이 output.html을 요청하는 URL 문자열로 변경됨.

<br>

# References

- [한 권으로 끝내는 실무 Java 웹 개발서 / 김정현, 김계희 / 남가람북스 / 2019](http://www.kyobobook.co.kr/product/detㄴailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025)

<br>
