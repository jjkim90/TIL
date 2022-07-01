# Servlet 프로그래밍

## 1. Servlet의 개요

- Java Servlet은 서버에서 수행되는 웹 프로그래밍 기술로서 Java 언어로 구현하는 기술임.
- 웹 클라이언트의 요청으로 웹서버에서 수행되는 Java 프로그램임.
- 구현이 쉽고 처리 성능이 높아 지금까지 가장 많이 활용하는 웹서버 프로그래밍 기술이 되었음.



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



#### Servlet의 최초 요청과 두 번째 이후 요청

- 최초 요청
  - 브라우저의 Servlet 요청 -> JVM에 로딩 후 객체 생성 -> init() -> service(request, response) -> doGet() or doPost() -> 요청 보낸 브라우저로 출력 버퍼의 내용 리턴.
- 두 번째 이후 요청
  - 브라우저의 Servlet 요청 -> service() -> doGet() or doPost() -> 요청 보낸 브라우저로 출력 버퍼의 내용 리턴.



#### Servlet 객체가 메모리에서 해제되는 시점

- 컨테이너(서버)가 종료될 때
- 웹 애플리케이션이 리로드될 때
- 자동 리로드가 설정된 상태에서 Servlet이 재컴파일되었을 때



#### 여러 브라우저가 하나의 Servlet을 동시에 요청할 때

- 웹 컨테이너가 Servlet 수행을 처리할 때는 다중 스레드 방식을 사용함.
- 여러 브라우저가 동시 요청하면 각 요청마다 스레드를 구동시켜 병렬로 처리함. 각 요청마다 Servlet 클래스의 객체를 생성하는 것이 아니라 하나의 Servlet 객체를 공유하여 여러 스레드가 병렬로 처리하는 방식임. 적은 메모리로 여러 개의 요청 동시 수행 가능.



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

#### 유용한 요청 정보 추출



#### 다양한 타입의 응답 처리

- 리퀘스트/리스폰 -> HTML, XML, JSON, Image 등 Mime Type, 문자 세트 정보
- setContentType()
- 문자면 response.getWrite(); 바이너리면 response.getOutputStream



## 2. Query 문자열 처리

### 2.1. GET 방식 Query 문자열 처리

#### GET 방식으로 전달되는 Query 문자열 추출

request.getQueryString()

getParameter(name) -> 밸류 1개

getParameterValues(name) -> 밸류여러개

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



### 2.3. multipart/form-data 처리(파일 업로드)

- 클라이언트의 하드디스크에 존재하는 파일을 선택하여 서버에 전달하는 것을 파일 업로드라고 함.

#### @MultipartConfig 애노테이션

#### 멀티 파트 추출 API

#### multipart/form-data 형식의 데이터 점검

#### 파일 업로드 기능 구현 점검



## 3. 상태 정보 관리

### 3.1. HttpSession 객체를 활용하는 상태 정보 관리

### 3.2. HttpSession 객체의 활용



## 4. 요청 재지정

### 4.1. forward를 사용한 요청 재지정

### 4.2. redirect를 사용한 요청 재지정









































































