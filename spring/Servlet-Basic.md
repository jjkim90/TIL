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



### 1.3. Servlet의 요청과 응답



## 2. Query 문자열 처리

### 2.1. GET 방식 Query 문자열 처리

### 2.2. POST 방식 Query 문자열 처리

### 2.3. multipart/form-data 처리(파일 업로드)



## 3. 상태 정보 관리

### 3.1. HttpSession 객체를 활용하는 상태 정보 관리

### 3.2. HttpSession 객체의 활용



## 4. 요청 재지정

### 4.1. forward를 사용한 요청 재지정

### 4.2. redirect를 사용한 요청 재지정









































































