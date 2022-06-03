# 예외 처리



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