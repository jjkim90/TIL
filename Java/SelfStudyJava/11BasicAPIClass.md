# 기본 API 클래스



## 1. java.lang 패키지



### 1.1. 자바 API 도큐먼트

### 1.2. API 도큐먼트에서 클래스 페이지 읽는 방법

### 1.3. Object 클래스

모든 클래스는 Object 클래스의 자식이거나 자손 클래스



#### 객체 비교(equals())

비교 연산자인 ==와 동일 결과 리턴 (번지 같은지, 참조 객체 같은지 확인)

자식 클래스에서 equals() 재정의 -> 서로 다른 두 객체의 같은 필드값 비교 가능. 동등 객체 비교. 같은 종류의 객체여야 비교 가능.



#### 객체 해시코드(hashCode())

객체를 식별하는 하나의 정수값

객체 생성 시 객체 메모리 번지 이용하여 만들어지기 때문에 객체마다 다른 값을 가짐.

해시코드 재정의하는 경우 - 두 객체가 동등한지 비교할 때 필요.

서로 다른 객체 논리적으로 동등하게 만들기 위해서는 해시코드+이퀄스 둘 다 재정의해야 함.

먼저 해시코드부터 검사 -> 그 다음 이퀄스 검사 -> 둘다 true면 동등 객체로 판단. 하나라도 다르면 다른 객체로 판단함.

HashSet / Hashtable / HashMap



#### 객체 문자 정보(toString())

객체의 문자 정보 리턴

클래스이름@16진수해시코드 로 구성된 문자 정보

재정의 -> 유익한 정보 리턴하기 위해







### 1.4. System 클래스

운영체제가 가진 기능을 이용하도록 설계됨. 정적 필드와 메소드로 구성됨.



#### 프로그램 종료(exit())

exit(int 종료상태값) 0정상종료 



#### 현재 시각 읽기(currentTimeMillis(), nanoTime())

currenttimemillis -> 1/10^3 단위 long값

nanotime -> 1/10^9단위 long값

코드 실행 시간 측정에 활용됨.



### 1.5. Class 클래스

클래스와 인터페이스의 메타데이터를 Class 클래스로 관리

타입 이름, 파일 경로 정보, 필드 생성자 메소드 정보



#### Class 객체 얻기(getClass(), forName())

클래스로부터 얻는 방법

1) Class clazz = 클래스이름.class
2) Class clazz = Class.forName("패키지...클래스이름")

객체로부터 얻는 방법

3. Class clazz = 참조변수.getClass();



#### 클래스 경로를 활용해서 리소스 절대 경로 얻기

Class 객체는 파일 경로 정보 가지고 있어 이 경로 활용해 다른 리소스 파일의 경로 얻을 수 있음.

String photo1Path = clazz.getResource("photo1.jpg").getPath();

UI 프로그램에서 이미지에 대한 절대경로를 얻어야 할 때 사용함.



### 1.6. String 클래스

String str = "자바";



#### String 생성자

String 생성자 다양함.







#### String 메소드



##### 문자 추출(charAt())



##### 문자열 비교(equals())



##### 바이트 배열로 변환(getBytes())



##### 문자열 찾기(indexOf())



##### 문자열 길이(length())



##### 문자열 대치(replace())



##### 문자열 잘라내기(substring())



##### 알파벳 대소문자 변경(toLowerCase(), toUpperCase())



##### 문자열 앞뒤 공백 잘라내기(trim())



##### 문자열 변환(valueOf())



### 1.7. Wrapper(포장) 클래스



#### 박싱(Boxing)과 언박신(Unboxing)



#### 자동 박싱과 언박싱



#### 문자열을 기본 타입 값으로 변환



#### 포장 값 비교



### 1.8. Math 클래스

Math.random() : 0.0이상 1.0 미만 하나의 double 타입 값 리턴

1~10임의의 정수 리턴하기 (int) (Math.random() * 10) + 1





## 2. java.util 패키지

java.util 패키지는 날짜 정보를 제공하는 유용한 API를 포함하고 있다.



### 2.1. Date 클래스

날짜를 표현하는 클래스

객체 간 날짜 정보를 주고 받을 때 매개 변수나 리턴 타입으로 주로 사용

매개변수 : void method(Date date){...}

리턴값 : Date method(...){...}

클래스 : Date now = new Date(); (현재 os 날짜 정보 기반으로 객체 생성)

-> 영어로 된 문자열 출력됨. 원하는 형식 위해 java.text 패키지의 SimpleDataFormat 클래스와 함께 사용

SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일 hh시 mm분 ss초");

format() 메소드 호출

String strNow = sdf.format(now);



### 2.2. Calendar 클래스

달력을 표현한 클래스.

운영체제의 날짜 및 시간 기준으로 다양한 정보를 얻을 수 있음.

추상 클래스이므로 new 연산자 사용하여 인스턴스 생성 불가.

getInstance() 메소드 이용하여 Calendar 하위 객체 얻을 수 있음.

Calendar now = Calendar.getInstance();

int year = now.get(Calendar.YEAR); // 연도를 리턴

MONTH // 0~11리턴 ---> MONTH+1 // 1~12리턴

DAY_OF_MONTH // 1~31 리턴

DAY_OF_WEEK // 1~7 리턴

AM_PM // 오전/오후 리턴

HOUR, MINUTE, SECOND // 시,분,초 리턴





