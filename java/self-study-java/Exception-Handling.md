# 예외 처리

<br>

> 이 문서는 [<혼자 공부하는 자바 - 신용권>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.

<br>

## 1. 예외 클래스

- 예외(exception)란 사용자의 잘못된 조작 또는 개발자의 잘못된 코딩으로 인해 발생하는 프로그램 오류를 말함.
- 예외 처리(exception handling)를 통해 프로그램을 종료하지 않고 정상 실행 상태가 유지되도록 할 수 있다는 점에서 에러와 다름.

<br>

### 1.1. 예외와 예외 클래스

- 일반 예외(exception)
  - 컴파일러 체크 예외라고도 함.
  - 프로그램 실행 시 예외가 발생할 가능성이 높기 때문에 자바 소스를 컴파일하는 과정에서 해당 예외 처리 코드가 있는지 검사함.
  - 만약 예외 처리 코드가 없다면 컴파일 오류가 발생함.
- 실행 예외(runtime exception)
  - 컴파일러 넌 체크 예외라고도 함.
  - 실행 시 예측할 수 없이 갑자기 발생하기 때문에 컴파일하는 과정에서 예외 처리 코드가 있는지 검사하지 않음.
- java.lang.Exception 클래스
  - 자바에서는 예외를 클래스로 관리함.
  - JVM은 프로그램을 실행하는 도중에 예외가 발생하면 해당 예외 클래스로 객체를 생성함.
  - 그리고 나서 예외 처리 코드에서 예외 객체를 이용할 수 있도록 해줌.
  - 모든 예외 클래스는 java.lang.Exception 클래스를 상속받음.
- Runtime Exception의 하위 클래스가 아니면 일반 예외 클래스이고, 하위 클래스이면 실행 예외 클래스임. 클래스 상속 관계에서 부모(조상)에 RuntimeException이 있다면 실행 예외 클래스임.

<br>

### 1.2. 실행 예외

- 실행 예외는 자바 컴파일러가 체크하지 않기 때문에 오로지 개발자의 경험에 의해서 예외 처리 코드를 작성해야 함.
- 만약 개발자가 실행 예외에 대해 예외 처리 코드를 넣지 않은 경우, 해당 예외가 발생하면 프로그램은 곧바로 종료됨.

<br>

#### NullPointerException

- 자바 프로그램에서 가장 빈번하게 발생하는 실행 예외임.
- 객체 참조가 없는 상태, 즉 null 값을 갖는 참조 변수로 객체 접근 연산자인 도트(.)를 사용했을 때 발생함.

```java
// NullPointerException 발생 예제
public class NullPointerExceptionExample {
	public static void main(String[] args) {
		String data = null;
		System.out.println(data.toString());
	}
}
```

- 프로그램에서 예외가 발생하면 예외 메시지가 Console 뷰에 출력되면서 프로그램이 종료됨.
- Console 뷰에 출력되는 내용에는 어떤 예외가 어떤 소스의 몇 번째 코드에서 발생했는지에 대한 정보가 들어 있음.
  - NullPounterExceptionExample.java:6 -> 소스의 6번째 코드에서 발생했다고 알려주는 것임. 클릭하면 이클립스에서 해당 라인에 하이라이팅을 해줌.

<br>

#### ArrayIndexOutOfBoundsException

- 배열에서 인덱스 범위를 초과할 경우 실행 예외인 java.lang.ArrayIndexOutOfBoundsException이 발생함.

```java
// ArrayIndexOutOfBoundsException 예외
public class ArrayIndexOutOfBoundsExceptionExample {
	public static void main(String[] args) {
		String data1 = args[0];
		String data2 = args[1];
		
		System.out.println("args[0]: " + data1);
		System.out.println("args[1]: " + data2);
	}
}
// 실행 매개값을 주지 않은 상태로 args[0], args[1] 사용하려고 했기 때문에 예외 발생함.
```

- 배열 값을 읽기 전에 배열의 길이를 먼저 조사하면 예외 발생하지 않는 좋은 프로그램이 됨.

```java
// 배열 값 읽기 전에 배열 길이 먼저 조사하기
public class ArrayIndexOutOfBoundsExceptionExample {
	public static void main(String[] args) {
		if(args.length == 2) {
			String data1 = args[0];
			String data2 = args[1];
			System.out.println("args[0]: " + data1);
			System.out.println("args[1]: " + data2);
		} else {
			System.out.println("두개의 실행 매개값이 필요합니다.");
		}
	}
}
```

<br>

#### NumberFormatException

- 문자열 데이터를 숫자로 변경하는 경우 `Integer.parseInt(String s)`, `Double.parseDouble(String s)` 등을 자주 사용함.
- Interger, Double은 포장(Wrapper) 클래스라고 함.
- 이 클래스의 정적 메소드인 parseXXX() 메소드를 이용하면 문자열을 숫자로 변환할 수 있음.
- 숫자로 변환될 수 없는 문자가 변환에 포함되어 있다면 java.lang.NumberFormatException 발생함.

```java
// NumberFormatException 예외

public class NumberFormatExceptionExample {
	public static void main(String[] args) {
		String data1 = "100";
		String data2 = "a100";
				
		int value1 = Integer.parseInt(data1);
		int value2 = Integer.parseInt(data2);
		
		int result = value1 + value2;
		System.out.println(data1 + "+" + data2 + "=" + result);
	}
}
// 7라인은 정상적으로 실행되지만, 8라인에서 "a100" 문자열은 숫자로 변환할 수 없기 때문에 예외 발생함.
```

<br>

#### ClassCastException

- 타입 변환(Casting)은 상위 클래스와 하위 클래스 간에 발생하고 구현 클래스와 인터페이스 간에도 발생함.
- 이러한 관계가 아니라면 클래스는 다른 타입으로 변환할 수 없기 때문에 ClassCastException이 발생함.
- ClassCastException을 발생시키지 않으려면 타입 변환 전에 변환이 가능한지 instanceof 연산자로 확인하는 것이 좋음.
- instanceof 연산의 결과가 true이면 좌항 객체를 우항 타입으로 변환이 가능하다는 뜻임.

```java
// ClassCastException 예외
public class ClassCastExceptionExample {
	public static void main(String[] args) {
		Dog dog = new Dog();
		changeDog(dog);
		
		Cat cat = new Cat();
		changeDog(cat);
	}
	
	public static void changeDog(Animal animal) {
		//if(animal instanceof Dog) {
			Dog dog = (Dog) animal; 				//ClassCastException 발생 가능
		//} 
	}
}

class Animal {}
class Dog extends Animal {}
class Cat extends Animal {}
```

<br>

## 2. 예외 처리

### 2.1. 예외 처리 코드

- try-catch-finally 블록
  - 생성자 내부와 메소드 내부에서 작성되어 일반 예외와 실행 예외가 발생할 경우 예외 처리를 할 수 있도록 해줌.
  - try 블록에는 예외 발생 가능 코드가 위치함.
  - try 블록의 코드가 예외 발생 없이 정상 실행되면 catch 블록의 코드는 실행되지 않고 finally 블록의 코드를 실행함.
  - try 블록의 코드에서 예외가 발생하면 즉시 실행을 멈추고 catch 블록으로 이동하여 예외 처리 코드를 실행함. 그리고 finally 블록의 코드를 실행함.
  - finally 블록은 생략 가능함.
  - 예외 발생 여부에 상관없이 항상 실행할 내용이 있을 경우에만 finally 블록을 작성함.
  - try 블록과 catch 블록에서 return문을 사용하더라도 finally 블록은 항상 실행됨.
- 이클립스는 일반 예외가 발생할 가능성이 있는 코드를 작성하면 빨간 밑줄을 그어 예외 처리 코드의 필요성을 알려줌.
- 빨간 밑줄에 마우스 포인터를 가져다 놓으면 Unhandled exception(처리되지 않은 예외) 정보를 알 수 있음.

```java
// 일반 예외 처리

public class TryCatchFinallyExample {
	public static void main(String[] args) {
		try {
			Class clazz = Class.forName("String2");
		} catch(ClassNotFoundException e) {
			System.out.println("클래스가 존재하지 않습니다.");
		}
	}
}
```

- 실행 예외는 컴파일러가 예외 처리 코드를 체크하지 않기 때문에 이클립스에서도 빨간 밑줄이 생기지 않음.
- 오로지 개발자의 경험에 의해 예외 처리를 작성해야 함.

```java
// 실행 예외 처리

public class TryCatchFinallyRuntimeExceptionExample {
	public static void main(String[] args) {
		String data1 = null;
		String data2 = null;
		try {
			data1 = args[0];
			data2 = args[1];
		} catch(ArrayIndexOutOfBoundsException e) {
			System.out.println("실행 매개값의 수가 부족합니다.");
			return;
		} 
		
		try { 
			int value1 = Integer.parseInt(data1);
			int value2 = Integer.parseInt(data2);
			int result = value1 + value2;
			System.out.println(data1 + "+" + data2 + "=" + result);
		} catch(NumberFormatException e) {
			System.out.println("숫자로 변환할 수 없습니다.");
		} finally {
			System.out.println("다시 실행하세요.");
		}
	}
}
```

<br>

### 2.2. 예외 종류에 따른 처리 코드

#### 다중 catch

- 발생되는 예외별로 예외 처리 코드를 다르게 하려면 다중 catch 블록을 작성해야 함.
- try 블록에서 하나의 예외가 발생하면 즉시 실행을 멈추고 해당 catch 블록으로 이동하기 때문에 catch 블록이 여러 개라 할지라도 단 하나의 catch 블록만 실행됨.

```java
// 다중 catch

public class CatchByExceptionKindExample {
	public static void main(String[] args) {
		try {
			String data1 = args[0];
			String data2 = args[1];
			int value1 = Integer.parseInt(data1);
			int value2 = Integer.parseInt(data2);
			int result = value1 + value2;
			System.out.println(data1 + "+" + data2 + "=" + result);
		} catch(ArrayIndexOutOfBoundsException e) {
			System.out.println("실행 매개값의 수가 부족합니다.");
		} catch(NumberFormatException e) {
			System.out.println("숫자로 변환할 수 없습니다.");
		} finally {
			System.out.println("다시 실행하세요.");
		}
	}
}
```

<br>

#### catch 순서

- 다중 catch 블록을 작성할 때는 상위 예외 클래스가 하위 예외 클래스보다 아래쪽에 위치해야 함.
- try 블록에서 예외가 발생하면 예외를 처리해줄 catch 블록은 위에서부터 차례로 검색됨.
- 하위 예외는 상위 예외를 상속했기 때문에 상위 예외 타입에도 포함됨. 따라서 상위 예외 클래스의 catch 블록이 위에 있다면 하위 예외 클래스의 catch 블록은 실행되지 않음.
- `Exception e`는 최상위 예외 클래스이므로 catch 블록의 최하단에 작성해야 여러 예외를 처리할 수 있음.

```java
// catch 블록의 순서

public class CatchOrderExample {
	public static void main(String[] args) {
		try {
			String data1 = args[0];
			String data2 = args[1];
			int value1 = Integer.parseInt(data1);
			int value2 = Integer.parseInt(data2);
			int result = value1 + value2;
			System.out.println(data1 + "+" + data2 + "=" + result);
		} catch(ArrayIndexOutOfBoundsException e) {
			System.out.println("실행 매개값의 수가 부족합니다.");
		} catch(Exception e) {
			System.out.println("실행에 문제가 있습니다.");
		} finally {
			System.out.println("다시 실행하세요.");
		}
	}
}
```

<br>

### 2.3. 예외 떠넘기기

- throws 키워드는 메소드 선언부 끝에 작성되어 메소드에서 처리하지 않은 예외를 호출한 곳으로 떠넘기는 역할을 함.
- throws 키워드 뒤에는 떠넘길 예외 클래스를 쉼표로 구분해서 나열해주면 됨.
- 발생할 수 있는 예외의 종류별로 throws 뒤에 나열하는 것이 일반적이지만, throws Exception만으로 모든 예외를 간단히 떠넘길 수도 있음.
- throws 키워드가 붙어 있는 메소드는 반드시 try 블록 내에서 호출되어야 함. 그리고 catch 블록에서 떠넘겨 받은 예외를 처리해야 함.
- 메소드A에서 throws 키워드를 사용한 메소드B를 호출할 때 메소드A의 try 블록 내에서 호출하거나 메소드A에도 throws 키워드를 다시 사용하여 예외를 떠넘길 수 있음. 이 경우, 메소드A를 호출하는 곳에서 try-catch 블록을 사용해서 예외를 처리해야 함.

```java
// 예외 처리 떠넘기기

public class ThrowsExample {
	public static void main(String[] args) {
		try {
			findClass();
		} catch(ClassNotFoundException e) {
			System.out.println("클래스가 존재하지 않습니다.");
		}
	}
	
	public static void findClass() throws ClassNotFoundException {
		Class clazz = Class.forName("java.lang.String2");
	}
}
```

- main() 메소드에서도 throws 키워드를 사용하여 예외를 떠넘길 수 있음.
- 이 경우 JVM이 최종적으로 예외를 처리하게 됨.
- JVM은 예외의 내용을 콘솔에 출력하는 것으로 예외 처리를 함.
- main() 메소드에서 throws Exception을 붙이는 것은 좋지 못한 예외 처리 방법임. 프로그램 사용자는 프로그램이 알 수 없는 예외 내용을 출력하고 종료되는 것을 좋아하지 않음. 따라서 main()에서 try-catch 블록으로 예외를 최종 처리하는 것이 바람직함.

<br>