# 제어문과 메서드



## 제어문

- 프로그래밍 언어는 제어문을 사용해 실행문을 비순차적으로 수행할 수 있게 함.
- 제어문은 실행문의 수행 순서를 변경하는 것을 조건문, 반복문, 분기문이 있음.
- 조건문
  - if문
  - switch문
- 반복문
  - for문
  - while문
  - do~while문
- 분기문
  - break문
  - continue문
- 



## 조건문

### 단순 if 문

- 조건식이 true일 때만 실행문을 수행함.
- 조건식에는 true 또는 false를 산출할 수 있는 연산식이나 논릿값, 변수가 올 수 있음.
- 조건식이 true일 때 수행할  실행문이 하나라면 {}를 생략할 수 있음.
- 실무에서는 혼란을 방지하기 위해 되도록 중괄호 {} 사용을 추천함.



### if~else 문

- if 문은 else 블록과 함께 사용되어 조건식의 결과에 따라 실행 블록을 선택함.
  - if문의 조건이 true면, if문의 블록이 실행됨.
  - if문의 조건이 false이면, else 블록이 실행됨.
- if (조건식) { true일 때 실행문; } else { false일 때 실행문; }



### 다중 if 문

- 조건문이 여러 개인 if문을 말함.
- 학점 계산, 회원 등급 처리 등에 사용됨.
- if (조건식1) { 조건식1이 true일 때 실행문; } else if (조건식2) { 조건식2가 true일 때 실행문; } else { 모든 조건 false일 때 실행문; }



### 중첩 if 문

- if 문의 블록 내부에 또다른 조건문, 비교문 등이 오는 기법
  - if문 내부에 for문, while문을 사용할 수 있음.
  - if문 내부에 또다른 if문 을 사용할 수 있음.





## 반복문





## 분기문





## switch 문

### 개념

- if 문과 마찬가지로 조건 제어문임.
- if 문처럼 조건식이 true일 때 블록 내부의 실행문을 실행하는 것이 아니라, 변수가 어떤 값을 갖느냐에 따라 실행문이 선택됨.
- if 문은 결과가 true나 false 두 가지 형태일 때 사용하고, switch 문은 결과가 여러 개일 때 많이 활용됨.



### switch 문의 특징

- switch 문은 괄호 안의 값과 동일한 값을 갖는 case로 가서 실행문을 실행시킴.
- 만약 괄호 안의 값과 동일한 값이 없으면 default로 가서 실행문을 실행시킴.
- default는 생략이 가능함.
- 실행문을 실행하고 break 문을 만나면 switch문을 빠져나옴.
- break문이 없을 경우 다음 case문이 연달아 실행됨. (오류 발생)



### break 문을 생략한 switch 문

- 특정 시점부터 시작하여 끝까지 누적적으로 실행을 원할 경우 의도적으로 break 문 전체를 생략할 수 있음.

- ```java
  int time = (int) (Math.random() * 6) + 6;
  		System.out.println("[현재 시간은 : " + time + "시]");
  
  		switch (time) {
  		case 6 :
  			System.out.println("조깅 후 세안을 하고 아침을 먹습니다.");
  		case 7 :
  			System.out.println("출근 준비를 합니다.");
  		case 8 :
  			System.out.println("버스를 타고 출근을 합니다.");
  		case 9 :
  			System.out.println("수업 준비를 합니다.");
  		case 10 :
  			System.out.println("결석자에게 전화를 합니다.");
  		default :
  			System.out.println("자바를 열심히 가르칩니다.");
  ```

  

- 묶이길 원하는 조건끼리 break문을 생략하면 break문이 나오는 지점까지 조건들에 1가지 명령을 실행할 수 있음.

- ```java
  char grade = 'b';
  
  		switch (grade) {
  		case 'A' :
  		case 'a' :
  			System.out.println("우수 회원입니다.");
  			break;
  		case 'B' :
  		case 'b' :
  			System.out.println("일반 회원입니다.");
  			break;
  		default :
  			System.out.println("손님입니다. 회원가입하시겠습니까?");
  ```

  

### String 타입을 사용한 switch 문

- 자바 과거 버전에서는 switch 문에 기본타입만 사용할 수 있었음.

- 현재는 String 타입도 switch 문에 활용할 수 있음.

- ```java
  String position = "과장";
  		
  		switch (position) {
  		case "부장" :
  			System.out.println("성과급은 1000만원");
  			break;
  		case "과장" :
  			System.out.println("성과급은 500만원");
  			break;
  		case "대리" :
  			System.out.println("성과급은 200만원");
  			break;
  		default :
  			System.out.println("성과급 없음");
  			break;
  		}
  ```





## 메서드









# Reference

- [쉽게 배우는 자바 프로그래밍 - 우종정](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791156645146&orderClick=LAG&Kc=)

- [혼자 공부하는 자바 - 신용권](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=)
- [자바의 정석 - 남궁성](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788994492032&orderClick=LAG&Kc=)

