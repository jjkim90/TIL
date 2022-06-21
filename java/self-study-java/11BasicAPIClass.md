# 기본 API 클래스

<br>

> 이 문서는 [<혼자 공부하는 자바 - 신용권>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.

<br>

## 1. java.lang 패키지

- java.lang 패키지는 자바 프로그램의 기본적인 클래스를 담고 있는 패키지임.
- java.lang 패키지에 있는 클래스와 인터페이스는 import 없이 사용할 수 있음.
- 지금까지 사용한 String과 System 클래스도 java.lang 패키지에 포함되어 있기 때문에 import하지 않고 사용 가능했음.

- Object 클래스
  - 자바 클래스의 최상위 클래스로 사용
- System 클래스
  - 표준 입력 장치(키보드)로부터 데이터를 입력받을 때 사용.
  - 표준 출력 장치(모니터)로 출력하기 위해 사용.
  - 자바 가상 기계를 종료할 때 사용.
  - 쓰레기 수집기를 실행 요청할 때 사용.
- Class 클래스
  - 클래스를 메모리로 로딩할 때 사용.
- String 클래스
  - 문자열을 저장하고 여러 가지 정보를 얻을 때 사용.
- Wrapper 클래스
  - Byte, Short, Character, Integer, Float, Double, Boolean, Long 클래스
  - 기본 타입의 데이터를 갖는 객체를 만들 때 사용.
  - 문자열을 기본 타입으로 변환할 때 사용.
  - 입력값 검사에 사용.
- Math 클래스
  - 수학 함수를 이용할 때 사용.



### 1.1. 자바 API 도큐먼트

- API(Application Programming Interface)
  - 라이브러리(library)라고 부르기도 함.
  - 프로그램 개발에 자주 사용되는 클래스 및 인터페이스의 모음을 말함.
- 방대한 자바 표준 API 중에서 우리가 원하는 API를 쉽게 찾아 이용할 수 있도록 도와주는 API 도큐먼트가 있음.
- API 도큐먼트는 HTML 페이지로 작성되어 있고, https://docs.oracle.com/en/java/javase/index.html 에 방문하면 버전별로 볼 수 있음.



### 1.2. API 도큐먼트에서 클래스 페이지 읽는 방법

- 최상단의 SUMMARY : NESTED | FIELD | CONSTR | METHOD
  - SUMMARY는 클래스 내에 선언된 멤버가 무엇이 있는지 알려줌.
  - NESTED는 중첩 클래스나 중첩 인터페이스를 설명함.
  - FIELD, CONSTR, METHOD 는 필드, 생성자, 메소드를 설명함.
  - 링크가 없으면 해당 멤버가 없다는 뜻임.



### 1.3. Object 클래스

- 클래스 선언할 때 extends 키워드로 다른 클래스를 상속하지 않더라도 암시적으로 java.lang.Object 클래스를 상속하게 됨.
- 자바의 모든 클래스는 Object 클래스의 자식이거나 자손 클래스임.
- Object는 자바의 최상위 부모 클래스에 해당함.
- 모든 클래스에서 Object 클래스의 메소드를 사용할 수 있음.



#### 객체 비교(equals())

- equals() 메소드의 매개 타입은 Object인데, 이것은 모든 객체가 매개값으로 대입될 수 있음을 말함(자동 타입 변환).
- Object 클래스의 equals() 메소드는 비교 연산자인 ==과 동일한 결과를 리턴함.
- equals() 메소드는 두 객체를 비교해서 논리적으로 동등하면 true, 아니면 false를 리턴함.
- 논리적으로 동등하다는 것은 같은 객체이건 다른 객체이건 상관없이 객체가 저장하고 있는 데이터가 동일함을 뜻함.
- String 객체의 equals() 메소드
  - String 객체의 equals() 메소드는 String 객체의 번지를 비교하는 것이 아니고, 문자열이 동일한지 조사해서 같다면 true, 다르면 false를 리턴함.
  - String 클래스가 Object의 equals() 메소드를 재정의(오버라이딩)해서 번지 비교가 아닌 문자열 비교로 변경했기 때문임.
- 일반적으로 Object의 equals() 메소드는 직접 사용되지 않고 하위 클래스에서 재정의하여 논리적으로 동등 비교할 때 이용함.

```java
// 객체 동등 비교(equals() 메소드)

public class Member {
	public String id;
	
	public Member(String id) {
		this.id = id;
	}
	
	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Member) {
			Member member = (Member) obj;
			if(id.equals(member.id)) {
				return true;
			}
		}
		return false;
	}
}

public class MemberExample {
	public static void main(String[] args) {
		Member obj1 = new Member("blue");
		Member obj2 = new Member("blue");
		Member obj3 = new Member("red");
		
		if(obj1.equals(obj2)) {
			System.out.println("obj1과 obj2는 동등합니다.");
		} else {
			System.out.println("obj1과 obj2는 동등하지 않습니다.");
		}
		
		if(obj1.equals(obj3)) {
			System.out.println("obj1과 obj3은 동등합니다.");
		} else {
			System.out.println("obj1과 obj3은 동등하지 않습니다.");
		}
	}
}
```



#### 객체 해시코드(hashCode())

- 객체 해시코드란 객체를 식별하는 하나의 정수값을 말함.
- Object 클래스의 hashCode() 메소드는 객체의 메모리 번지를 이용해서 해시코드를 만들어 리턴하기 때문에 객체마다 다른 값을 가지고 있음.
- 논리적 동등 비교 시 hashCode()를 오버라이딩할 필요가 있는데, 13장에서 배울 컬렉션 프레임워크에서 HashSet, HashMap, Hashtable은 다음과 같은 방법으로 두 객체가 동등한지 비교함.
  - 우선 hashCode() 메소드를 실행해서 리턴된 해시코드 값이 같은지를 봄.
  - 해시코드 값이 다르면 다른 객체로 판단함.
  - 해시코드 값이 같으면 equals() 메소드로 다시 비교함.
  - hashCode() 메소드가 true가 나와도 equals()의 리턴값이 다르면 다른 객체가 됨.

```java
// hashCode() 메소드를 재정의하지 않음

public class Key {
	public int number;
	
	public Key(int number) {
		this.number = number;
	}
	
	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Key) {
			Key compareKey = (Key) obj;
			if(this.number == compareKey.number) {
				return true;
			}
		} 
		return false;
	}
}
```

- 위의 경우 HashMap의 식별키로 Key 객체를 사용하면 저장된 값을 찾아오지 못함.
- number 필드값이 같더라도 hashCode() 메소드에서 리턴하는 해시코드가 다르므로 다른 식별키로 인식하기 때문임.

```java
// 다른 키로 인식함

import java.util.HashMap;

public class KeyExample {
	public static void main(String[] args) {
		//Key 객체를 식별키로 사용해서 String 값을 저장하는 HashMap 객체 생성
		HashMap<Key, String> hashMap = new HashMap<Key, String>();
		
		//식별키 "new Key(1)" 로 "홍길동"을 저장함
		hashMap.put(new Key(1), "홍길동");
		
		//식별키 "new Key(1)" 로 "홍길동"을 읽어옴
		String value  = hashMap.get(new Key(1));
		System.out.println(value);
	}
}
```

- 실행결과 null값이 나옴.
- 의도한 대로 "홍길동"을 읽으려면 재정의한 hashCode() 메소드를 Key 클래스에 추가하면 됨.

```java
// hashCode() 메소드 재정의 추가

public class Key{
    ...
	@Override
	public int hashCode() {
		return number;
	}        
}
```

- 저장할 때의 new Key(1)과 읽을 때의 new Key(1)은 사실 서로 다른 객체이지만 HashMap은 hashCode()의 리턴값이 같고, equals()의 리턴값이 true가 되기 때문에 동등한 객체로 평가함. 즉, 같은 식별키로 인식함.
- 객체의 동등 비교를 위해서는 Object의 equals() 메소드만 재정의하지 말고 hashCode() 메소드도 재정의해서 논리적으로 동등한 객체일 경우 동일한 해시코드가 리턴되도록 해야 함.

```java
// hashCode() 메소드 재정의 추가

public class Key {
	public int number;
	
	public Key(int number) {
		this.number = number;
	}
	
	@Override
	public boolean equals(Object obj) {
		if(obj instanceof Key) {
			Key compareKey = (Key) obj;
			if(this.number == compareKey.number) {
				return true;
			}
		} 
		return false;
	}
	
	@Override
	public int hashCode() {
		return number;
	}
}
```



#### 객체 문자 정보(toString())

- Object 클래스의 toString() 메소드는 객체의 문자 정보를 리턴함.
- 객체의 문자 정보란 객체를 문자열로 표현한 값을 말함.
- 기본적으로 Object 클래스의 toString() 메소드는 '클래스이름@16진수해시코드'로 구성된 문자 정보를 리턴함.
- Object의 toString() 메소드의 리턴값은 자바 애플리케이션에서는 별 값어치가 없는 정보이므로 Object 하위 클래스는 toString() 메소드를 재정의(오버라이딩)하여 간결하고 유익한 정보를 리턴하도록 되어 있음.
- java.util 패키지의 Date 클래스는 toString() 메소드를 재정의하여 현재 시스템의 날짜와 시간 정보를 리턴함.
- String 클래스는 toString() 메소드를 재정의해서 저장하고 있는 문자열을 리턴함.

```java
// 객체의 문자 정보(toString() 메소드)

import java.util.Date;

public class ToStringExample {
	public static void main(String[] args) {
		Object obj1 = new Object();
		Date obj2 = new Date();		
		System.out.println(obj1.toString());
		System.out.println(obj2.toString());
	}
}
```

- 실제 프로그램 개발 시에도 toString() 메소드를 재정의해서 좀 더 유용한 정보를 리턴하도록 할 수 있음.

```java
// Smartphone 클래스에서 toString() 메소드를 오버라이딩하여 제작회사와 운영체제를 리턴하기
// SmartPhone.java
public class SmartPhone {
	private String company;
	private String os;
	
	public SmartPhone(String company, String os) {
		this.company = company;
		this.os = os;
	}
	
	@Override
	public String toString() {
		return company + ", " + os;
	}
}
// SmartPhoneExample.java
public class SmartPhoneExample {
	public static void main(String[] args) {
		SmartPhone myPhone = new SmartPhone("구글", "안드로이드");
		
		String strObj = myPhone.toString();
		System.out.println(strObj);
		
		System.out.println(myPhone);
	}
}
```

- System.out.println() 메소드는 매개값으로 객체를 주면 객체의 toString() 메소드를 호출하여 리턴값을 받아 출력하도록 되어 있음.



### 1.4. System 클래스

- 자바 프로그램은 JVM 위에서 실행되기 때문에 운영체제의 모든 기능을 직접 이용하기 어려움.
- java.lang 패키지에 속하는 System 클래스를 이용하면 운영체제의 일부 기능을 이용할 수 있음.
- System 클래스의 모든 필드와 메소드는 정적 필드와 정적 메소드로 구성되어 있음.



#### 프로그램 종료(exit())

- 강제적으로 JVM을 종료시키고 싶을 때 System 클래스의 exit() 메소드 호출.
- exit() 메소드는 현재 실행하고 있는 프로세스를 강제 종료시키는 역할을 함.
- exit() 메소드는 int 매개값을 지정하도록 되어 있음. 이 값을 종료 상태값이라고 함.
- 일반적으로 정상 종료일 경우 0 값을 줌.

```java
// exit() 메소드

public class ExitExample {
	public static void main(String[] args)  {
		for(int i=0; i<10; i++) {
			if(i == 5) {
				System.exit(i);
				//break;
			}
		}
		System.out.println("마무리 코드");
	}
}
// 7라인과 8라인을 번갈아가며 주석 처리하고 실행해볼 것.
```



#### 현재 시각 읽기(currentTimeMillis(), nanoTime())

- System 클래스의 currentTimeMillis() 메소드와 nanotime() 메소드는 컴퓨터의 시계로부터 현재 시간을 읽어서 밀리세컨드(1/1000초) 단위와 나노세컨드(1/10^9초) 단위의 long 값을 리턴함.
- 리턴값은 주로 프로그램의 실행 소요 시간 측정에 사용됨.
- 프로그램 시작 시 시각을 읽고, 프로그램이 끝날 때 시각을 읽어서 차이를 구하면 프로그램 실행 소요 시간이 나옴.

```java
// 프로그램 실행 소요 시간 구하기

public class SystemTimeExample {
	public static void main(String[] args) {
		long time1 = System.nanoTime();
		
		int sum = 0;
		for(int i=1; i<=1000000; i++) {
			sum += i;
		}
		
		long time2 = System.nanoTime();
		
		System.out.println("1~1000000까지의 합: " + sum);
		System.out.println("계산에 " + (time2-time1) + " 나노초가 소요되었습니다.");
	}
}
```



### 1.5. Class 클래스

- 자바는 클래스와 인터페이스의 메타 데이터를 java.lang 패키지에 소속된 Class 클래스로 관리함.
- 메타 데이터는 클래스의 이름, 생성자 정보, 필드 정보, 메소드 정보를 말함.



#### Class 객체 얻기(getClass(), forName())

- 객체 없이 클래스 이름만 가지고 Class 객체를 얻는 방법
  - Class clazz = 클래스이름.class
  - Class clazz = Class.forName("패키지...클래스이름")
- 클래스로부터 객체가 이미 생성되어 있을 경우에 Class 객체를 얻는 방법
  - Class clazz = 참조변수.getClass( );

```java
// Class 객체 정보 얻기

public class ClassExample {
	public static void main(String[] args) throws Exception {
		//how1
		Class clazz = Car.class;
		
		//how2
		//Class clazz = Class.forName("sec01.exam09.Car");
		
		//how3
		//Car car = new Car();
		//Class clazz = car.getClass();
		
		System.out.println(clazz.getName());
		System.out.println(clazz.getSimpleName());
		System.out.println(clazz.getPackage().getName());
	}
}
```



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

# References

- [<혼자 공부하는 자바 - 신용권>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=)
