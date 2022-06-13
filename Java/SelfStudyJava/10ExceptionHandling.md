# 예외 처리



> 이 문서는 [<혼자 공부하는 자바 - 신용권>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.



## 1. 예외 클래스

예외, 예외 처리

### 1.1. 예외와 예외 클래스

일반 예외. 예외 처리 코드 없으면 컴파일 에러 발생. 컴파일 체크 예외. 예외 클래스 만들어서 예외 정보 관리. java.lang.Exception 모든 예외 클래스의 최상위 부모.    exception 상속 받은 예외를 일반 예외라고 함.



### 1.2. 실행 예외

실행 예외. runtimeexception의 하위 예외. RuntimeException을 상속함. 컴파일러 체크 안함.



#### NullPointerException

가장 많이 발생. 객체 참조가 없는 상태의 참조 변수로 객체 접근 연산자 도트를 사용할 경우 발생.

String str = null;

str.length(); // 널포인터 예외 발생.

예외 해결하려면 도트연산자 찾아야 함. 

#### ArrayIndexOutOfBoundsException

배열 관련 예외. 배열에서 인덱스 범위를 초과할 경우 발생.

Int[] arr = {1, 2, 3};

int result = arr[0] + arr[3]; // 어레이인덱스아웃오브바운즈 예외 발생.

예외 예방하려면 길이를 미리 확인하고 배열 사용. if문 사용.



#### NumberFormatException

문자열을 숫자로 변환 실패한 경우.

Integer.parseInt(String s)

s에 숫자가 아닌 문자가 들어온 경우 변환 실패하면서 예외 발생.

Double.parseDouble(String s)



#### ClassCastException





## 2. 예외 처리

다중 catch

Exception e >> 전체 예외 처리 코드라 어떤 예외가 와도 무조건 실행됨. >> 마지막 순서에 exception 넣고 그 위쪽으로 각각의 익셉션 처리하면 괜찮음.



예외떠넘기기

throws 키워드 메소드에서 처리하지 않은 예외를 호출한 곳으로 넘김. 호출한 곳에 따라 예외처리 내용을 다르게 가져갈 수 있음.

throws 키워드 뒤에 떠넘길 예외 클래스 쉼표로 구분하여 나열

Exceptions throws도 가능함.

e.printStackTrace(); 개발할 때 디버깅 목적으로 어디에서 예외 발생했는지 확인하는 용도.