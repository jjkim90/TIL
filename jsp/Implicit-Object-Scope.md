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



## page 영역

## request 영역

## session 영역

## application 영역































































<br>

# References

- [성낙현의 JSP 자바 웹 프로그래밍 - 성낙현](