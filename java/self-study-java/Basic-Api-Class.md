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

<br>

### 1.1. 자바 API 도큐먼트

- API(Application Programming Interface)
  - 라이브러리(library)라고 부르기도 함.
  - 프로그램 개발에 자주 사용되는 클래스 및 인터페이스의 모음을 말함.
- 방대한 자바 표준 API 중에서 우리가 원하는 API를 쉽게 찾아 이용할 수 있도록 도와주는 API 도큐먼트가 있음.
- API 도큐먼트는 HTML 페이지로 작성되어 있고, https://docs.oracle.com/en/java/javase/index.html 에 방문하면 버전별로 볼 수 있음.

<br>

### 1.2. API 도큐먼트에서 클래스 페이지 읽는 방법

- 최상단의 SUMMARY : NESTED | FIELD | CONSTR | METHOD
  - SUMMARY는 클래스 내에 선언된 멤버가 무엇이 있는지 알려줌.
  - NESTED는 중첩 클래스나 중첩 인터페이스를 설명함.
  - FIELD, CONSTR, METHOD 는 필드, 생성자, 메소드를 설명함.
  - 링크가 없으면 해당 멤버가 없다는 뜻임.

<br>

### 1.3. Object 클래스

- 클래스 선언할 때 extends 키워드로 다른 클래스를 상속하지 않더라도 암시적으로 java.lang.Object 클래스를 상속하게 됨.
- 자바의 모든 클래스는 Object 클래스의 자식이거나 자손 클래스임.
- Object는 자바의 최상위 부모 클래스에 해당함.
- 모든 클래스에서 Object 클래스의 메소드를 사용할 수 있음.

<br>

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

<br>

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

<br>

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

<br>

### 1.4. System 클래스

- 자바 프로그램은 JVM 위에서 실행되기 때문에 운영체제의 모든 기능을 직접 이용하기 어려움.
- java.lang 패키지에 속하는 System 클래스를 이용하면 운영체제의 일부 기능을 이용할 수 있음.
- System 클래스의 모든 필드와 메소드는 정적 필드와 정적 메소드로 구성되어 있음.

<br>

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

<br>

### 1.5. Class 클래스

- 자바는 클래스와 인터페이스의 메타 데이터를 java.lang 패키지에 소속된 Class 클래스로 관리함.
- 메타 데이터는 클래스의 이름, 생성자 정보, 필드 정보, 메소드 정보를 말함.

<br>

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

<br>

#### 클래스 경로를 활용해서 리소스 절대 경로 얻기

- Class 객체는 해당 클래스의 파일 경로 정보를 가지고 있기 때문에 이 경로를 활용해서 다른 리소스 파일(이미지, XML, Property 파일)의 경로를 얻을 수 있음.
- 이 방법은 UI 프로그램에서 많이 활용됨.

```java
// 리소스 절대 경로 얻기

public class ClassExample {
	public static void main(String[] args) {
		Class clazz = Car.class;

		String photo1Path = clazz.getResource("photo1.jpg").getPath();
		String photo2Path = clazz.getResource("images/photo2.jpg").getPath();
		
		System.out.println(photo1Path);
		System.out.println(photo2Path);
	}
}
```

<br>

### 1.6. String 클래스

#### String 생성자

- 자바의 문자열은 java.lang 패키지의 String 클래스의 인스턴스로 관리됨.
- 소스상에서 문자열 리터럴은 String 객체로 자동 생성되지만, String 클래스의 다양한 생성자를 이용해서 직접 String 객체를 생성할 수도 있음.
- 파일의 내용을 읽거나, 네트워크를 통해 받은 데이터는 보통 byte[] 배열이므로 이것을 문자열로 변환하기 위해 사용됨.
- 사용 빈도수가 높은 생성자들
  - String str = new String(byte[] bytes); : 배열 전체를 String 객체로 생성
  - String str = new String(byte[] bytes, String charsetName); : 지정한 문자셋으로 디코딩
  - String str = new String(byte[] bytes, int offset, int length); : 배열의 offset 인덱스 위치부터 length만큼 String 객체로 생성
  - String str = new String(byte[] bytes, int offset, int length, String charsetName); : 지정한 문자셋으로 디코딩
- int offset은 배열의 번호를 뜻함. 0번부터 시작.

```java
// 바이트 배열을 문자열로 변환

public class ByteToStringExample {
	public static void main(String[] args) {
		byte[] bytes = { 72, 101, 108, 108, 111, 32, 74, 97, 118, 97  };
		
		String str1 = new String(bytes);
		System.out.println(str1);
		
		String str2 = new String(bytes, 6, 4);
		System.out.println(str2);
	}
}
```

- 예제 : 키보드로부터 읽은 바이트 배열을 문자열로 변환하는 방법
  - System.in.read() 메소드는 키보드에서 입력한 내용을 매개값으로 주어진 바이트 배열에 저장하고 읽은 바이트 수를 리턴함.
  - Hello를 입력하고 Enter키를 눌렀다면 Hello+캐리지리턴(\r)+라인피드(\n)의 코드값이 바이트 배열에 저장되고 총 7개의 바이트를 읽었기 때문에 7을 리턴함.

```java
// 바이트 배열을 문자열로 변환

import java.io.IOException;

public class KeyboardToStringExample {
	public static void main(String[] args) throws IOException {
		byte[] bytes = new byte[100]; // 읽은 바이트를 저장하기 위한 배열 생성
		
		System.out.print("입력: ");
		int readByteNo = System.in.read(bytes); // 배열에 읽은 바이트를 저장하고 읽은 바이트 수를 리턴

		String str = new String(bytes, 0, readByteNo-2); // 배열을 문자열로 변환
		System.out.println(str);
	}
}
```

<br>

#### String 메소드

| 리턴 타입 |                 메소드 이름(매개 변수)                 |                             설명                             |
| :-------: | :----------------------------------------------------: | :----------------------------------------------------------: |
|   char    |                   charAt(int index)                    |                  특정 위치의 문자를 리턴함.                  |
|  boolean  |                equals(Object anObject)                 |                     두 문자열을 비교함.                      |
|  byte[]   |                       getBytes()                       |                       byte[]로 리턴함.                       |
|  byte[]   |               getBytes(Charset charset)                |         주어진 문자셋으로 인코딩한 byte[]로 리턴함.          |
|    int    |                  indexOf(String str)                   |         문자열 내에서 주어진 문자열의 위치를 리턴함.         |
|    int    |                        length()                        |                    총 문자의 수를 리턴함.                    |
|  String   | replace(CharSequence target, CharSequence replacement) |  target 부분을 replacement로 대치한 새로운 문자열을 리턴함.  |
|  String   |               substring(int beginIndex)                |  beginIndex 위치에서 끝까지 잘라낸 새로운 문자열을 리턴함.   |
|  String   |        substring(int beginIndex, int endIndex)         | beginIndex 위치에서 endIndex 전까지 잘라낸 새로운 문자열을 리턴함. |
|  String   |                     toLowerCase()                      |        알파벳 소문자로 변환한 새로운 문자열을 리턴함.        |
|  String   |                     toUpperCase()                      |        알파벳 대문자로 변환한 새로운 문자열을 리턴함.        |
|  String   |                         trim()                         |          앞뒤 공백을 제거한 새로운 문자열을 리턴함.          |
|  String   |         valueOf(int i)<br />valueOf(double d)          |               기본 타입 값을 문자열로 리턴함.                |

<br>

##### 문자 추출(charAt())

- chatAt() 메소드는 매개값으로 주어진 인덱스의 문자를 리턴함.
- 인덱스란 0에서부터 '문자열 길이-1'까지의 번호를 말함.

```java
// 주민등록번호에서 남자와 여자를 구분하는 방법

public class StringCharAtExample {
	public static void main(String[] args) {
		String ssn = "010624-1230123";
		char sex = ssn.charAt(7);
		switch (sex) {
			case '1':
			case '3':
				System.out.println("남자 입니다.");
				break;
			case '2':
			case '4':
				System.out.println("여자 입니다.");
				break;
		}
	}
}
```

<br>

##### 문자열 비교(equals())

- 기본 타입 변수의 값을 비교할 때는 == 연산자를 사용하지만, 문자열을 비교할 때는 == 연산자를 사용하면 원하지 않는 결과가 나올 수 있음.
- 자바는 문자열 리터럴이 동일하다면 동일한 String 객체를 참조하도록 되어 있음.
- 두 String 객체의 문자열만을 비교하고 싶다면 == 연산자 대신에 equals() 메소드를 사용해야 함.
- 원래 equals()는 Object 클래스의 번지 비교 메소드이지만, String 클래스가 재정의해서 문자열을 비교하도록 변경했음.

```java
// 문자열 비교

public class StringEqualsExample {
	public static void main(String[] args) {
		String strVar1 = new String("신민철");
		String strVar2 = "신민철";

		if(strVar1 == strVar2) {
			System.out.println("같은 String 객체를 참조");
		} else {
			System.out.println("다른 String 객체를 참조");
		}
		
		if(strVar1.equals(strVar2)) {
			System.out.println("같은 문자열을 가짐");
		} else {
			System.out.println("다른 문자열을 가짐");
		}
	}
}
```

<br>

##### 바이트 배열로 변환(getBytes())

- 네트워크로 문자열을 전송하거나, 문자열을 암호화할 때 문자열을 바이트 배열로 변환하는 경우가 있음.
- getBytes() 메소드는 시스템의 기본 문자셋으로 인코딩된 바이트 배열을 리턴함.
- 특정 문자셋으로 인코딩된 바이트 배열을 얻으려면 매개값으로 charset을 넣으면 됨.
- 어떤 문자셋으로 인코딩하느냐에 따라 바이트 배열의 크기가 달라짐
  - EUC-KR은 getBytes()와 마찬가지로 알파벳은 1바이트, 한글은 2바이트로 변환.
  - UTF-8은 알파벳은 1바이트, 한글은 3바이트로 변환.
- getBytes() 메소드는 잘못된 문자셋을 매개값으로 줄 경우, java.io.UnsupportedEncodingException이 발생하므로 예외 처리가 필요함.
- String(byte[] bytes) 생성자를 이용해서 디코딩하면 시스템의 기본 문자셋을 이용함.
- 시스템 기본 문자셋과 다른 문자셋으로 인코딩된 바이트 배열일 경우 `String str = new String(byte[] bytes, String charsetName)` 이용해서 디코딩해야 함.

```java
// 바이트 배열로 변환

import java.io.UnsupportedEncodingException;

public class StringGetBytesExample {
	public static void main(String[] args) {
		String str = "안녕하세요";
		
		byte[] bytes1 = str.getBytes();
		System.out.println("bytes1.length: " + bytes1.length);
		String str1 = new String(bytes1);
		System.out.println("bytes1->String: " + str1);
		
		try {
			
			byte[] bytes2 = str.getBytes("EUC-KR");
			System.out.println("bytes2.length: " + bytes2.length);
			String str2 = new String(bytes2, "EUC-KR");
			System.out.println("bytes2->String: " + str2);
			
			
			byte[] bytes3 = str.getBytes("UTF-8");
			System.out.println("bytes3.length: " + bytes3.length);
			String str3 = new String(bytes3, "UTF-8");
			System.out.println("bytes3->String: " + str3);		
			
		} catch (UnsupportedEncodingException e) {
			e.printStackTrace();
		}
	}
}
```

<br>

##### 문자열 찾기(indexOf())

- indexOf() 메소드는 매개값으로 주어진 문자열이 시작되는 인덱스를 리턴함.
- 만약 주어진 문자열이 포함되어 있지 않으면 -1을 리턴함.
- indexOf() 메소드는 if문의 조건식에서 특정 문자열이 포함되어 있는지 여부에 따라 실행 코드를 달리할 때 자주 사용됨. -1 값을 리턴하면 특정 문자열이 포함되어 있지 않다는 뜻임.

```java
// 문자열 포함 여부 조사

public class StringIndexOfExample {
	public static void main(String[] args) {
		String subject = "자바 프로그래밍";
		
		int location = subject.indexOf("프로그래밍");
		System.out.println(location);
		
		if(subject.indexOf("자바") != -1) {
			System.out.println("자바와 관련된 책이군요");
		} else {
			System.out.println("자바와 관련없는 책이군요");
		}
	}
}
```

<br>

##### 문자열 길이(length())

- length() 메소드는 문자열의 길이(문자의 수)를 리턴함.
- 문자열의 길이는 공백을 포함함.

```java
// 문자열의 문자 수 얻기

public class StringLengthExample {
	public static void main(String[] args) {
		String ssn = "7306241230123";
		int length = ssn.length();
		if(length == 13) {
			System.out.println("주민번호 자리수가 맞습니다.");
		} else {
			System.out.println("주민번호 자리수가 틀립니다.");
		}
	}
}
```

<br>

##### 문자열 대치(replace())

- replace() 메소드는 첫 번째 매개값인 문자열을 찾아 두 번째 매개값인 문자열로 대치한 새로운 문자열을 생성하고 리턴함.
- String 객체의 문자열은 변경이 불가능한 특성을 갖기 때문에 replace() 메소드가 리턴하는 문자열은 원래 문자열의 수정본이 아니라 완전히 새로운 문자열임.

```java
// 문자열 대치하기

public class StringReplaceExample {
	public static void main(String[] args) {
		String oldStr = "자바는 객체지향언어 입니다. 자바는 풍부한 API를 지원합니다.";
		String newStr = oldStr.replace("자바", "JAVA");
		
		System.out.println(oldStr);
		System.out.println(newStr);
        System.out.println(oldStr == newStr);
	}
}
```

<br>

##### 문자열 잘라내기(substring())

- substring() 메소드는 주어진 인덱스에서 문자열을 추출함.
  - `substring(int beginIndex, int endIndex)` 는 주어진 시작과 끝 인덱스 사이의 문자열을 추출함.
  - `substring(int beginIndex)`는 주어진 인덱스부터 끝까지 문자열을 추출함.

```java
// 문자열 추출하기

public class StringSubstringExample {
	public static void main(String[] args) {	
		String ssn = "880815-1234567 ";
		
		String firstNum = ssn.substring(0, 6);
		System.out.println(firstNum);		
		
		String secondNum = ssn.substring(7);
		System.out.println(secondNum);
	} 
}
```

<br>

##### 알파벳 대소문자 변경(toLowerCase(), toUpperCase())

- toLowerCase() 메소드는 문자열을 모두 소문자로 바꾼 새로운 문자열을 생성한 후 리턴함.
- toUpperCase() 메소드는 문자열을 모두 대문자로 바꾼 새로운 문자열을 생성한 후 리턴함.
- 영어로 된 두 문자열을 대소문자와 관계없이 비교할 때 equals() 메소드와 함께 자주 사용됨.
- equalsIgnoreCase() 메소드를 사용하면 간단하게 비교 가능함.

```java
// 전부 소문자 또는 대문자로 변경

public class StringToLowerUpperCaseExample {
	public static void main(String[] args) {
		String str1 = "Java Programming";
		String str2 = "JAVA Programming";		
		
		System.out.println(str1.equals(str2));
		
		String lowerStr1 = str1.toLowerCase();
		String lowerStr2 = str2.toLowerCase();
		System.out.println(lowerStr1.equals(lowerStr2));
		
		System.out.println(str1.equalsIgnoreCase(str2));				
	}
}
```

<br>

##### 문자열 앞뒤 공백 잘라내기(trim())

- trim() 메소드는 문자열의 앞뒤 공백을 제거한 새로운 문자열을 생성하고 리턴함.
- 앞뒤 공백만 제거할 뿐 중간의 공백은 제거하지 않음.

```java
// 문자열 앞뒤 공백 제거

public class StringTrimExample {
	public static void main(String[] args) {
		String tel1 = "  02";
		String tel2 = "123   ";
		String tel3 = "   1234   ";
		
		String tel = tel1.trim() + tel2.trim() + tel3.trim();
		System.out.println(tel);
	}
}
```

<br>

##### 문자열 변환(valueOf())

- valueOf() 메소드는 기본 타입의 값을 문자열로 변환함.

```java
// 기본 타입 값을 문자열로 변환

public class StringValueOfExample {
	public static void main(String[] args) {
		String str1 = String.valueOf(10);
		String str2 = String.valueOf(10.5);
		String str3 = String.valueOf(true);		
		
		System.out.println(str1);
		System.out.println(str2);
		System.out.println(str3);
	}
}
```

<br>

### 1.7. Wrapper(포장) 클래스

- 기본 타입의 값을 갖는 객체를 포장(Wrapper) 객체라고 함.
- 포장하고 있는 기본 타입 값은 외부에서 변경할 수 없음.
- 내부의 값을 변경하고 싶다면 새로운 포장 객체를 만들어야 함.
- 포장 객체는 주로 컬렉션 프레임워크에서 기본 타입 값을 객체로 생성해서 관리할 때 사용됨.

<br>

#### 박싱(Boxing)과 언박싱(Unboxing)

- 기본 타입의 값을 포장 객체로 만드는 과정을 박싱(Boxing)이라고 하고, 반대로 포장 객체에서 기본 타입의 값을 얻어내는 과정을 언박싱(Unboxing)이라고 함.
- 박싱하려면 포장 클래스의 생성자 매개값으로 기본 타입의 값 또는 문자열을 넘겨주면 됨.

|       기본 타입의 값을 줄 경우       |          문자열을 줄 경우          |
| :----------------------------------: | :--------------------------------: |
|       Byte obj = new Byte(10);       |     Byte obj = new Byte("10");     |
| Character obj = new Character('가'); |                없음                |
|     Short obj = new Short(100);      |   Short obj = new Short("100");    |
|   Integer obj = new Integer(1000);   | Integer obj = new Integer("1000"); |
|     Long obj = new Long(10000);      |   Long obj = new Long("10000");    |
|     Float obj = new Float(2.5F);     |   Float obj = new Float("2.5F");   |
|    Double obj = new Double(3.5);     |  Double obj = new Double("3.5");   |
|   Boolean obj = new Boolean(true);   | Boolean obj = new Boolean("true"); |

- 생성자를 이용하지 않아도 각 포장 클래스마다 가지고 있는 정적 valueOf() 메소드를 사용할 수도 있음.
  - `Integer obj = Integer.valueOf(1000);`
  - `Integer obj = Integer.valueOf("1000")`
- 자바 9부터는 포장 클래스의 생성자 매개값을 받아 박싱하는 방법은 권장되지 않음(Deprecated). valueOf() 메소드 이용이 권장됨.

- 언박싱하기 위해서는 각 포장 클래스마다 가지고 있는 '기본 타입 이름 + Value()' 메소드를 호출하면 됨.
  - byte num = obj.byteValue();
  - char ch = obj.charValue();
  - short num = obj.shortValue();
  - int num = obj.intValue();
  - long num = obj.longValue();
  - float num = obj.floatValue();
  - double num = obj.doubleValue();
  - boolean bool = obj.booleanValue();

```java
// 기본 타입의 값을 박싱하고 언박싱하기

public class BoxingUnBoxingExample {
	public static void main(String[] args) {
		//Boxing
		Integer obj1 = new Integer(100);
		Integer obj2 = new Integer("200");
		Integer obj3 = Integer.valueOf("300");
		
		//Unboxing
		int value1 = obj1.intValue();
		int value2 = obj2.intValue();
		int value3 = obj3.intValue();
		
		System.out.println(value1);
		System.out.println(value2);
		System.out.println(value3);
	}
}
```

<br>

#### 자동 박싱과 언박싱

- 자동 박싱은 포장 클래스 타입에 기본값이 대입될 경우 발생함.
- `Integer obj = 100;` <- 자동 박싱
- 자동 언박싱은 기본 타입에 포장 객체가 대입되는 경우와 연산에서 발생함.
- `Integer obj = new Integer(200);`
- `int value1 = obj;` <- 자동 언박싱
- `int value2 = obj + 100` <- 자동 언박싱

```java
// 자동 박싱과 언박싱

public class AutoBoxingUnBoxingExample {
	public static void main(String[] args) {
		//자동 Boxing
		Integer obj = 100;
		System.out.println("value: " + obj.intValue());
	
		//대입시 자동 Unboxing
		int value = obj;  	
		System.out.println("value: " + value);
		
		//연산시 자동 Unboxing
		int result = obj + 100;
		System.out.println("result: " + result);
	}
}
```

<br>

#### 문자열을 기본 타입 값으로 변환

- 포장 클래스는 문자열을 기본 타입 값으로 변환할 때에도 많이 사용됨.
- 대부분의 포장 클래스에는 'parse + 기본 타입 이름'으로 되어 있는 정적 메소드가 있음.
- 정적 메소드는 문자열을 매개값으로 받아 기본 타입 값으로 변환함.
  - `byte num = Byte.parseByte("10");`
  - `int num = Integer.parseInt("1000");`

```java
// 문자열을 기본 타입 값으로 변환

public class StringToPrimitiveValueExample {
	public static void main(String[] args) {
		int value1 = Integer.parseInt("10");
		double value2 = Double.parseDouble("3.14");
		boolean value3 = Boolean.parseBoolean("true");
		
		System.out.println("value1: " + value1);
		System.out.println("value2: " + value2);
		System.out.println("value3: " + value3);
	}
}
```

<br>

#### 포장 값 비교

- 포장 객체는 내부의 값을 비교하기 위해 ==와 != 연산자를 사용하지 않는 것이 좋음.
- 박싱된 값이 특정 범위의 값이라면 ==와 != 연산자로 내부의 값을 바로 비교할 수 있지만, 그 이외의 경우는 언박싱한 값을 얻어 비교해야 함.
- 따라서 포장 객체에 정확히 어떤 값이 저장될지 모르는 상황이라면 직접 내부 값을 언박싱해서 비교하거나, equals() 메소드로 내부 값을 비교하는 것이 좋음.
- 포장 클래스의 equals() 메소드는 내부의 값을 비교하도록 재정의되어 있음.

```java
// 포장 객체 비교

public class ValueCompareExample {
	public static void main(String[] args) {
		System.out.println("[-128~127 초과값일 경우]");
		Integer obj1 = 300;
		Integer obj2 = 300;
		System.out.println("==결과: " + (obj1 == obj2));
		System.out.println("언박싱후 ==결과: " + (obj1.intValue() == obj2.intValue()));
		System.out.println("equals() 결과: " + obj1.equals(obj2));
		System.out.println();
		
		System.out.println("[-128~127 범위값일 경우]");
		Integer obj3 = 10;
		Integer obj4 = 10;
		System.out.println("==결과: " + (obj3 == obj4));
		System.out.println("언박싱후 ==결과: " + (obj3.intValue() == obj4.intValue()));
		System.out.println("equals() 결과: " + obj3.equals(obj4));
	}
}
```

<br>

### 1.8. Math 클래스

- java.lang.Math 클래스는 수학 계산에 사용할 수 있는 메소드를 제공함.
- Math 클래스가 제공하는 메소드는 모두 정적 메소드이므로 Math 클래스로 바로 사용 가능.

|                          메소드                           |         설명         |                          예제 코드                           |          리턴값          |
| :-------------------------------------------------------: | :------------------: | :----------------------------------------------------------: | :----------------------: |
|         int abs(int a)<br />double abs(double a)          |        절대값        |   int v1 = Math.abs(-5);<br />double v2 = Math.abs(-3.14);   |  v1 = 5<br />v2 = 3.14   |
|                   double ceil(double a)                   |        올림값        | double v3 = Math.ceil(5.3);<br />double v4 = Math.ceil(-5.3); | v3 = 6.0<br />v4 = -5.0  |
|                  double floor(double a)                   |        버림값        | double v5 = Math.floor(5.3);<br />double v6 = Math.floor(-5.3); | v5 = 5.0<br />v6 = -6.0  |
| int max(int a, int b)<br />double max(double a, double b) |        최대값        | int v7 = Math.max(5, 9);<br />double v8 = Math.max(5.3, 2.5); |   v7 = 9<br />v8 = 5.3   |
| int min(int a, int b)<br />double min(double a, double b) |        최소값        | int v9 = Math.min(5, 9);<br />double v10 = Math.min(5.3, 2.5); |  v9 = 5<br />v10 = 2.5   |
|                      double random()                      |        랜덤값        |                 double v11 = Math.random();                  |     0.0 <= v11 < 1.0     |
|                   double rint(double a)                   | 가까운 정수의 실수값 | double v12 = Math.rint(5.3);<br />double v13 = Math.rint(5.7); | v12 = 5.0<br />v13 = 6.0 |
|                   long round(double a)                    |       반올림값       | long v14 = Math.round(5.3);<br />long v15 = Math.round(5.7); |   v14 = 5<br />v15 = 6   |

```java
// Math의 수학 메소드

public class MathExample {
	public static void main(String[] args) {
		int v1 = Math.abs(-5);
		double v2 = Math.abs(-3.14);
		System.out.println("v1=" + v1);
		System.out.println("v2=" + v2);

		double v3 = Math.ceil(5.3);
		double v4 = Math.ceil(-5.3);
		System.out.println("v3=" + v3);
		System.out.println("v4=" + v4);
		
		double v5 = Math.floor(5.3);
		double v6 = Math.floor(-5.3);
		System.out.println("v5=" + v5);
		System.out.println("v6=" + v6);
		
		int v7 = Math.max(5, 9);
		double v8 = Math.max(5.3, 2.5);
		System.out.println("v7=" + v7);
		System.out.println("v8=" + v8);
		
		int v9 = Math.min(5, 9);
		double v10 = Math.min(5.3, 2.5);
		System.out.println("v9=" + v9);
		System.out.println("v10=" + v10);
		
		double v11 = Math.random();
		System.out.println("v11=" + v11);
		
		double v12 = Math.rint(5.3);
		double v13 = Math.rint(5.7);
		System.out.println("v12=" + v12);
		System.out.println("v13=" + v13);
		
		long v14 = Math.round(5.3);
		long v15 = Math.round(5.7);
		System.out.println("v14=" + v14);
		System.out.println("v15=" + v15);
		
		double value = 12.3456;
		double temp1 = value * 100;
		long temp2 = Math.round(temp1);
		double v16 = temp2 / 100.0;
		System.out.println("v16=" + v16);
	}
}
```

- Math.random() 메소드
  - 0.0과 1.0 사이의 범위에 속하는 하나의 double 타입의 값을 리턴함.
  - 0.0은 범위에 포함되고 1.0은 포함되지 않음.
- Math.random() 메소드 활용하여 1부터 10까지의 정수 난수 얻기
  - 각 변에 10을 곱하기 -> 0.0 <= x < 10.0
  - 각 변을 int 타입으로 강제 타입 변환하기 -> 0<= x < 10
  - 각 변에 1을 더하기 -? 1<= x <11 ->> 1~10 정수 난수 발생.
  - `int num = (int) (Math.random() * 10) + 1`

```java
// 임의의 주사위의 눈 얻기

public class MathRandomExample {
	public static void main(String[] args) {
		int num = (int) (Math.random()*6) + 1;
		System.out.println("주사위 눈: " + num);
	}
}
```

<br>

## 2. java.util 패키지

- java.util 패키지는 프로그램 개발에서 자주 사용되는 자료구조일 뿐만 아니라, 날짜 정보를 제공해주는 유용한 API를 포함하고 있음.

<br>

### 2.1. Date 클래스

- Date 클래스는 날짜를 표현하는 클래스.
- Date는 객체 간에 날짜 정보를 주고받을 때 매개 변수나 리턴 타입으로 주로 사용됨.
- 현재 시각의 Date 객체는 `Date now = new Date();`로 생성 가능.
- Date 객체의 toString() 메소드는 영문으로 된 날짜를 리턴하기 때문에 원하는 날짜 형식의 문자열을 얻고 싶다면 java.text 패키지의 SimpleDateFormat 클래스와 함께 사용하는 것이 좋음.
- `SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일 hh시 mm분 ss초");`
- SimpleDateFormat 객체를 얻었다면 format() 메소드를 호출해서 원하는 형식의 날짜 정보를 얻을 수 있음.
- format() 메소드의 매개값은 Date 객체임
- `String strNow = sdf.format(now);`

```java
// 현재 날짜를 출력하기

import java.text.*;
import java.util.*;

public class DateExample {
	public static void main(String[] args) {
		Date now = new Date();
		String strNow1 = now.toString();		
		System.out.println(strNow1);
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy년 MM월 dd일 hh시 mm분 ss초");
		String strNow2 = sdf.format(now);
		System.out.println(strNow2);
	}
}
```

- SimpleDateFormat의 매개값에서 월은 대문자 M, 분은 소문자 m을 사용함.

<br>

### 2.2. Calendar 클래스

- Calendar 클래스는 달력을 표현한 클래스임.
- 추상 클래스이므로 new 연산자를 사용해서 인스턴스 생성 불가능함.
- 정적 메소드인 getInstance() 메소드를 이용하면 현재 운영체제에 설정되어 있는 시간대를 기준으로 한 Calendar 하위 객체를 얻을 수 있음.
- `Calendar now = Calendar.getInstance();`
- Calendar 객체를 얻었다면 get() 메소드를 이용해서 날짜와 시간에  대한 정보를 읽을 수 있음.
  - `int year = now.get(Calendar.YEAR);` // 연도를 리턴
  - `int month = now.get(Calendar.MONTH) + 1` // 월을 리턴
  - `int day = now.get(Calendar.DAY_OF_MONTH)` // 일을 리턴
  - `String week = now.get(Calendar.DAY_OF_WEEK)` // 요일을 리턴
  - `String amPm = now.get(Calendar.AM_PM)` // 오전/오후를 리턴
  - `int hour = now.get(Calendar.HOUR)` // 시를 리턴
  - `int minute = now.get(Calendar.MINUTE)` // 분을 리턴
  - `int second = now.get(Calendar.SECOND)` // 초를 리턴
- get() 메소드를 호출할 때 사용한 매개값은 모두 Calendar 클래스에 선언되어 있는 상수들임.

```java
// 운영체제의 시간대를 기준으로 Calendar 얻기

import java.util.*;

public class CalendarExample {
	public static void main(String[] args) {
		Calendar now = Calendar.getInstance();
		
		int year    = now.get(Calendar.YEAR);                
		int month  = now.get(Calendar.MONTH) + 1;          
		int day    = now.get(Calendar.DAY_OF_MONTH);     
		
		int week    = now.get(Calendar.DAY_OF_WEEK);        
		String strWeek = null;
		switch(week) {
			case Calendar.MONDAY:
				strWeek = "월";
				break;
			case Calendar.TUESDAY:
				strWeek = "화";
				break;
			case Calendar.WEDNESDAY:
				strWeek = "수";
				break;
			case Calendar.THURSDAY:
				strWeek = "목";
				break;
			case Calendar.FRIDAY:
				strWeek = "금";
				break;
			case Calendar.SATURDAY:
				strWeek = "토";
				break;
			default:
				strWeek = "일";
		}
		
		int amPm  = now.get(Calendar.AM_PM);   
		String strAmPm = null;
		if(amPm == Calendar.AM) {
			strAmPm = "오전";
		} else {
			strAmPm = "오후";
		}
		
		int hour    = now.get(Calendar.HOUR);                 
		int minute  = now.get(Calendar.MINUTE);             
		int second  = now.get(Calendar.SECOND);              

		System.out.print(year + "년 ");
		System.out.print(month + "월 ");
		System.out.println(day + "일 ");
		System.out.print(strWeek + "요일 ");
		System.out.println(strAmPm + " ");
		System.out.print(hour + "시 ");
		System.out.print(minute + "분 ");
		System.out.println(second + "초 ");
	}
}
```

<br>

# References

- [혼자 공부하는 자바 / 신용권 / 한빛미디어 / 2019](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=)
