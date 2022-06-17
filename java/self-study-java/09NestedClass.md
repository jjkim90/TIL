# 중첩 클래스와 중첩 인터페이스



> 이 문서는 [<혼자 공부하는 자바 - 신용권>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.



## 1. 중첩 클래스와 중첩 인터페이스 소개

- 클래스가 여러 클래스와 관계를 맺는 경우에는 독립적으로 선언하는 것이 좋으나, 특정 클래스와 관계를 맺는 경우에는 클래스 내부에 선언하는 것이 좋음.
- 중첩 클래스(nested class)란 클래스 내부에 선언한 클래스를 말함.
- 중첩 클래스를 사용하면 두 클래스의 멤버들을 서로 쉽게 접근할 수 있고, 외부에는 불필요한 관계 클래스를 감춤으로써 코드의 복잡성을 줄일 수 있다는 장점이 있음.

```java
//중첩 클래스의 코드 형태
class ClassName{
	class NestedClassName{
	}
}
```

- 중첩 인터페이스(nested interface)란 클래스 내부에 선언한 인터페이스를 말함.
- 인터페이스를 클래스 내부에 선언하는 이유는 해당 클래스와 긴말한 관계를 맺는 구현 클래스를 만들기 위해서임.

```java
//중첩 인터페이스 코드 형태
class ClassName{
    interface NestedInterFaceName{
    }
}
```



### 1.1. 중첩 클래스

- 멤버 클래스 : 클래스의 멤버로서 선언되는 중첩 클래스.
  - 인스턴스 멤버 클래스 : A 객체를 생성해야만 사용할 수 있는 B 클래스.
  - 정적 멤버 클래스 : A 클래스로 바로 접근할 수 있는 B 클래스.
- 로컬 클래스 : 생성자 또는 메소드 내부에서 선언되는 중첩 클래스.
- 멤버 클래스는 클래스나 객체가 사용 중이라면 언제든지 재사용이 가능함.
- 로컬 클래스는 메소드를 실행할 때만 사용되고 메소드가 종료되면 없어짐.

- 중첩 클래스도 하나의 클래스이기 때문에 컴파일하면 바이트 코드 파일(.class)이 별도로 생성됨.

```java
A $ B .class // 멤버 클래스의 바이트 코드 파일. 바깥 클래스 A와 멤버 클래스 B.
A $1 B .class // 로컬 클래스의 바이트 코드 파일. 바깥 클래스 A와 로컬 클래스 B.
```



#### 인스턴스 멤버 클래스

- 인스턴스 멤버 클래스는 static 키워드 없이 중첩 선언된 클래스를 말함.
- 인스턴스 멤버 클래스는 인스턴스 필드와 메소드만 선언이 가능하고 정적 필드와 메소드는 선언할 수 없음.
- A 클래스 외부에서 인스턴스 멤버 클래스 B의 객체를 생성하려면 먼저 A 객체를 생성하고 B 객체를 생성해야 함.

```java
// A클래스 외부에서 B 객체 생성하기.
A a = new A();
A.B b = a.new B();
b.field1 = 3; // 인스턴스 필드 사용
b.method1(); // 인스턴스 메소드 호출
```

- A 클래스 내부의 생성자 및 인스턴스 메소드에서는 일반 클래스처럼  B 객체를 생성할 수 있음.

- 일반적으로 A 클래스 외부에서 B 객체를 생성하는 일은 거의 없음. 대부분 A 클래스 내부에서 B 객체를 생성해서 사용함.



#### 정적 멤버 클래스

- 정적 멤버 클래스는 static 키워드로 선언된 클래스를 말함.
- 정적 멤버 클래스는 모든 종류의 필드와 메소드를 선언할 수 있음.
- A 클래스 외부에서 정적 멤버 클래스 C의 객체를 생성하기 위해서는 A 객체를 생성할 필요가 없음.

```java
// A클래스 외부에서 C 객체 생성하기.
A.C c = new A.C();
c.field1 = 3; // 인스턴스 필드 사용
c.method1(); // 인스턴스 메소드 호출
A.C.field2 = 3; // 정적 필드 사용
A.C.method2(); // 정적 메소드 호출
```



#### 로컬 클래스

- 로컬 클래스는 메소드 내에서 선언된 중첩 클래스를 말함.
- 로컬 클래스는 접근 제한자 (public, private) 및 static을 붙일 수 없음.
- 로컬 클래스 내부에는 인스턴스 필드와 메소드만 선언할 수 있고 정적 필드와 메소드는 선언할 수 없음.
- 로컬 클래스는 메소드가 실행될 때 메소드 내에서 객체를 생성하고 사용해야 함.
- 주로 비동기 처리를 위해 스레드 객체를 만들 때 사용함.



### 1.2. 중첩 클래스의 접근 제한

- 멤버 클래스 내부에서 바깥 클래스의 필드와 메소드에 접근할 때는 제한이 따름.
- 메소드의 매개 변수나 로컬 변수를 로컬 클래스에서 사용할 때도 제한이 따름.



#### 바깥 필드와 메소드에서 사용 제한

- 인스턴스 멤버 클래스는 바깥 클래스의 인스턴스 필드의 초기값이나 인스턴스 메소드에서 객체를 생성할 수 있음.
- 인스턴스 멤버 클래스는 정적 필드의 초기값이나 정적 메소드에서는 객체를 생성할 수 없음.
- 정적 멤버 클래스는 인스턴스 멤버나 정적 멤버 모두에서 객체를 생성할 수 있음.

```java
public class A {
	// 인스턴스 멤버 클래스와 정적 멤버 클래스를 바깥 클래스에서 객체 생성할 때 사용 제한

	// 인스턴스 필드
	B field1 = new B();
	C field2 = new C();

	// 인스턴스 메소드
	void method1() {
		B var1 = new B();
		C var2 = new C();
	}

	// 정적 필드
//	static B field3 = new B();
	static C field4 = new C();

	// 정적 메소드
	static void method2() {
//		B var1 = new B();
		C var2 = new C();
	}

	class B {

	}

	static class C {

	}

}
```



#### 멤버 클래스에서 사용 제한

- 멤버 클래스 내부에서 바깥 클래스의 필드와 메소드에 접근할 때에도 제한이 따름.
- 인스턴스 멤버 클래스 안에서는 바깥 클래스의 모든 필드와 메소드에 접근할 수 있음.
- 정적 멤버 클래스 안에서는 바깥 클래스의 정적 멤버에만 접근할 수 있고 인스턴스 멤버에는 접근할 수 없음.

```java
public class A {
	// 바깥 클래스의 필드와 메소드를 멤버 클래스에서 사용할 때 사용 제한

	int field1;

	void method1() {
	}

	static int field2;

	static void method2() {
	}

	class B {
		void method() {
			field1 = 3;
			method1();

			field2 = 3;
			method2();
		}
	}

	static class C {
		void method() {
//			field1 = 3;
//			method1();
			field2 = 3;
			method2();
		}
	}

}
```



#### 로컬 클래스에서 사용 제한

- 로컬 클래스의 객체는 메소드 실행이 종료되면 없어지는 것이 일반적이지만, 메소드가 종료되어도 계속 실행 상태로 존재할 수 있음.
- 로컬 스레드 객체를 사용할 때. 로컬 스레드 객체는 메소드를 실행하는 스레드와 다르므로 메소드가 종료된 후에도 실행 상태로 존재할 수 있음.
- 자바는 컴파일 시 로컬 클래스에서 사용하는 매개 변수나 로컬 변수의 값을 로컬 클래스 내부에 복사해두고 사용함.
- 매개 변수나 로컬 변수가 수정되어 값이 변경되면 로컬 클래스에서 복사해둔 값과 달라지므로 문제를 해결하기 위해 매개 변수나 로컬 변수를 final로 선언할 것을 요구함.
- 자바 7 이전까지는 final 키워드 없이 선언된 매개 변수나 로컬 변수를 로컬 클래스에서 사용하면 컴파일 에러 발생했음.
- 자바 8부터는 final 키워드 없이 선언된 매개 변수와 로컬 변수를 사용해도 컴파일 에러 발생하지 않음.
- 자바 8부터는 final 선언을 하지 않아도 값이 수정될 수 없도록 final 특성을 자동으로 부여함.



#### 중첩 클래스에서 바깥 클래스 참조 얻기

- 중첩 클래스에서 this 키워드를 사용하면 바깥 클래스의 객체 참조가 아니라, 중첩 클래스의 객체 참조가 됨.
- 중첩 클래스 내부에서 바깥 클래스의 객체 참조를 얻으려면 바깥 클래스의 이름을 this 앞에 붙여줘야 함.

```java
// 중첩 클래스에서 바깥 클래스 참조 얻기
Outter.this.field;
Outter.this.method();
```



### 1.3. 중첩 인터페이스

- 중첩 인터페이스는 클래스의 멤버로 선언된 인터페이스를 말함.
- 인터페이스를 클래스 내부에 선언하는 이유는 해당 클래스와 긴밀한 관계를 맺는 구현 클래스를 만들기 위해서임.

```java
class A {
    [static] interface I {
        void method();
    }
}
```

- 중첩 인터페이스는 인스턴스 멤버 인터페이스와 정적(static) 멤버 인터페이스 모두 가능함.
  - 인스턴스 멤버 인터페이스는 바깥 클래스의 객체가 있어야 사용 가능함.
  - 정적 멤버 인터페이스는 바깥 클래스의 객체 없이 바깥 클래스만으로 바로 접근 가능함.
- 주로 정적 멤버 인터페이스를 많이 사용함. UI 프로그래밍에서 이벤트를 처리할 목적으로 많이 활용됨.

```java
// 중첩 인터페이스
public class Button{
    OnClickListener listener;
    
    void setOnClickListener(OnClickListener listener){
        this.listener = listener;
    }
    
    void touch(){
        listener.onClick();
    }
    
    static interface OnClickListener{
        void onClick();
    }
}

// 구현 클래스
public class CallListener implements Button.OnClickListener {
    @Override
    public void onClick(){
    System.out.println("전화를 겁니다.");
	}
}

// 구현 클래스
public class MessageListener implements Button.OnClickListener{
    @Override
    public void onClick(){
        System.out.println("메시지를 보냅니다.");
    }
}

// 버튼 이벤트 처리
public class ButtonExample{
    public static void main(String[] args){
        Button btn = new Button();
        
        btn.setOnClickListener(new CallListener());
        btn.touch();
        
        btn.setOnClickListener(new MessageListener());
        btn.touch();
    }
}

/* 실행결과
전화를 겁니다.
메시지를 보냅니다.
*/
```



## 2. 익명 객체

- 익명(anonymous) 객체는 클래스 이름이 없는 객체를 말함.
- 익명 객체를 만들려면 어떤 클래스를 상속하거나 인터페이스를 구현해야만 함.



### 2.1. 익명 자식 객체 생성

- 자식 클래스를 명시적으로 선언할 때는 재사용성을 높이고 싶을 때임. 어디서건 이미 선언된 자식 클래스로 간단히 객체를 생성해서 사용할 수 있음.
- 익명 자식 객체를 생성할 때는 자식 클래스가 재사용되지 않고, 오로지 특정 위치에서 사용할 경우임.

```java
부모클래스 [필드|변수] = new 부모클래스(매개값, ...){
	//필드
    //메소드
};
```

- 하나의 실행문이므로 끝에는 세미콜론(;)을 반드시 붙여야 함.
- 익명 자식 객체는 필드 사용, 로컬 변수 사용, 매개값 사용 가능함.

```java
// 필드를 선언할 때 초기값으로 익명 자식 객체를 생성해서 대입
class A {
    Parent field = new Parent(){
        int childField;
        void childMethod(){}
        @Override
        void parentMethod(){}
    };
}

// 메소드 내에서 로컬 변수를 선언할 때 초기값으로 익명 자식 객체를 생성해서 대입
class A {
    void method(){
        Parent localVar = new Parent(){
            int childField;
            void childMethod(){}
            @Override
            void parentMethod(){}
        };
    }
}

// 메소드의 매개 변수가 부모 타입일 경우 메소드를 호출하는 코드에서 익명 자식 객체를 생성해서 매개값으로 대입
class A {
    void method1(Parent parent){}
    
    void method2(){
        method1(
        new Parent() {
            int childField;
            void childMethod(){}
            @Override
            void parentMethod(){}
        }
        );
    }
}
```

- 익명 자식 객체에 새롭게 정의된 필드와 메소드는 익명 자식 객체 내부에서만 사용되고, 외부에서는 접근할 수 없음.



### 2.2. 익명 구현 객체 생성

- 구현 클래스를 명시적으로 선언하면 어디서건 이미 선언된 구현 클래스로 간단히 객체를 생성해서 사용할 수 있음. 재사용성이 높음.
- 익명 구현 객체를 생성해서 사용하는 이유는 구현 클래스가 재사용되지 않고, 오로지 특정 위치에서 사용할 경우이기 때문임.
- 익명 구현 객체는 필드 사용, 로컬 변수 사용, 매개값 사용이 가능함.

```java
// 인터페이스
public interface RemoteControl {
	public void turnOn();
	public void turnOff();
}

// 익명 구현 객체 생성
public class Anonymous {
	//필드 초기값으로 익명 구현 객체 대입
	RemoteControl interfaceField = new RemoteControl() {
		@Override
		public void turnOn() {
			System.out.println("TV를 켭니다.");
		}
		@Override
		public void turnOff() {
			System.out.println("TV를 끕니다.");
		}
	};
	
	//로컬 변수값으로 익명 구현 객체 대입
	void method1() {
		RemoteControl interfaceLocalVar = new RemoteControl() {
			@Override
			public void turnOn() {
				System.out.println("Audio를 켭니다.");
			}
			@Override
			public void turnOff() {
				System.out.println("Audio를 끕니다.");
			}
		};
		interfaceLocalVar.turnOn();
		interfaceLocalVar.turnOff();
	}
	
	//매개값으로 익명 구현 객체 대입
	void method2(RemoteControl rc) {
		rc.turnOn();
		rc.turnOff();
	}
}

// 메인 메소드
public class AnonymousExample {

	public static void main(String[] args) {
		Anonymous anony = new Anonymous();
		
		anony.interfaceField.turnOn();
		anony.interfaceField.turnOff();
		
		anony.method1();
		
		anony.method2(new RemoteControl() {
			@Override
			public void turnOn() {
				System.out.println("SmartTV를 켭니다.");
			}
			@Override
			public void turnOff() {
				System.out.println("SmartTV를 끕니다.");
			}
		}
		);
	}
}
```

- 윈도우 및 안드로이드 등의 UI 프로그램에서 버튼의 클릭 이벤트를 처리하기 위해 익명 구현 객체를 이용하는 방법

```java
// UI 클래스
public class Button{
    OnClickListener listener;
    
    void setOnClickListener(OnClickListener listener){
        this.listener = listener;
    }
    
    void touch(){
        listener.onClick();
    }
    
    static interface OnClickListener {
        void onClick();
    }
}

// Window 클래스 (2개의 버튼 객체를 가지고 있는 창)
public class Window{
    Button button1 = new Button();
    Button button2 = new Button();
    
    // 필드 초기값으로 익명 구현 객체 대입
    Button.OnClickListener listener = new Button.OnClickListener(){
      @Override
        public void onClick(){
            System.out.println("전화를 겁니다.");
        }
    };
    
    Window(){
        button1.setOnClickListener(listener); // 매개값으로 필드 대입
        // 매개값으로 익명 구현 객체 대입
        button2.setOnClickListener(new Button.OnClickListener(){
           @Override
            public void onClick(){
                System.out.println("메시지를 보냅니다.");
            }
        });
    }
}

// 실행 클래스
public class Main{
    public static void main(String[] args){
        Window w = new Window();
        w.button1.touch();
        w.button2.touch();
    }
}

/*
실행결과
전화를 겁니다.
메시지를 보냅니다.
*/
```



### 2.3. 익명 객체의 로컬 변수 사용



