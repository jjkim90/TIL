# 상속



> 이 문서는 [<혼자 공부하는 자바 - 신용권>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.



## 1. 상속

- 객체 지향 프로그래밍에서는 부모 클래스의 멤버를 자식 클래스에게 물려줄 수 있음.
- 프로그램에서는 부모 클래스를 상위 클래스라고 부르고, 자식 클래스를 하위 클래스 또는 파생 클래스라고 부름.
- 상속은 이미 잘 개발된 클래스를 재사용해서 새로운 클래스를 만들기 때문에 중복되는 코드를 줄여 줌.
- 상속을 이용하면 부모 클래스의 수정으로 모든 자식 클래스들도 수정되는 효과를 가져오기 때문에 유지 보수 시간을 최소화할 수도 있음.



### 1.1. 클래스 상속

- 자식 클래스를 선언할 때 어떤 부모 클래스를 상속받을 것인지 결정하고, 선택된 부모 클래스는 extends 뒤에 기술함.

```java
class 자식클래스 extends 부모클래스{
	// 필드
	// 생성자
	// 메소드
}
```

#### 자바에서 상속의 특징

- 여러 개의 부모 클래스를 상속할 수 없음. extends 뒤에는 단 하나의 부모 클래스만 와야 함.
- 부모 클래스에서 private 접근 제한을 갖는 필드와 메소드는 상속 대상에서 제외됨.
- 부모와 자식 클래스가 다른 패키지라면 default 접근 제한을 갖는 필드와 메소드도 상속 대상에서 제외됨.



### 1.2. 부모 생성자 호출

- 자바에서 자식 객체를 생성하면, 부모 객체가 먼저 생성되고 그 다음에 자식 객체가 생성됨.

- 모든 객체는 클래스의 생성자를 호출해야만 생성됨.
- 상속에서 부모 생성자는 자식 생성자의 맨 첫 줄에서 호출됨.
- 자식 클래스의 생성자가 명시적으로 선언되지 않았다면 컴파일러는 다음과 같은 기본 생성자를 생성함.

```java
public 자식클래스(){
	super();
}
```

- `super()`는 부모의 기본 생성자를 호출함.
- 직접 자식 생성자를 선언하고 명시적으로 부모 생성자를 호출하고 싶다면 다음과 같이 작성하면 됨.

```java
자식클래스(매개변수선언, ...){
	super(매개값, ...);
}
```

- `super()`는 매개값의 타입과 일치하는 부모 생성자를 호출함.
- 만약 매개값의 타입과 일치하는 부모 생성자가 없을 경우 컴파일 에러가 발생.
- 부모 클래스에 기본 생성자가 없고 매개 변수가 있는 생성자만 있다면 자식 생성자에서 반드시 부모 생성자 호출을 위해 `super(매개값, ...)`를 명시적으로 호출해야 함.
- `super(매개값, ...)`는 반드시 자식 생성자 첫 줄에 위치해야 하며, 그렇지 않으면 컴파일 에러가 발생함.



### 1.3. 메소드 재정의

- 메소드 재정의(오버라이딩: Overriding) : 상속 과정에서 자식 클래스가 사용하기에 적합하지 않은 일부 메소드를 자식 클래스에서 다시 수정해서 사용하는 것.



#### 메소드 재정의 방법

- 부모의 메소드와 동일한 시그니처(리턴 타입, 메소드 이름, 매개 변수 목록)를 가져야 함.
- 접근 제한을 더 강하게 재정의할 수 없음.
  - 부모 메소드가 pubilc 접근 제한을 가지고 있는 경우 재정의하는 자식 메소드는 default나 private 접근 제한으로 수정할 수 없음.
  - 반대로 접근 제한을 완화하는 것은 가능함.
- 새로운 예외(Exception)를 throws할 수 없음.
- @Override
  - 어노테이션. 메소드 재정의 최상단에 표기. 생략해도 좋으나 이것을 붙여주면 컴파일러가 정확히 확인하기 때문에 오타 등 개발자의 실수를 줄여줌.
- 이클립스에서 재정의 메소드 자동 생성
  - 이클립스 [Source] - [Override/Implement Methods] 메뉴에서 부모 클래스에서 재정의될 메소드를 선택하고 [OK]버튼 클릭.
  - Ctrl + space 누른 후 Override할 메소드 선택하고 엔터 클릭.



#### 부모 메소드 호출

- 자식 클래스에서 부모 클래스의 메소드를 재정의하게 되면, 부모 클래스의 메소드는 숨겨지고 재정의된 자식 메소드만 사용됨.
- 자식 클래스 내부에서 재정의된 부모 클래스의 메소드를 호출해야 하는 상황이 발생한다면 명시적으로 super 키워드를 붙여서 부모 메소드를 호출할 수 있음.

```java
super.부모메소드();
```

- 부모 클래스의 메소드에 조건을 추가하면서 오버라이딩 할 때 super.부모메소드 이용됨.



### 1.4. final 클래스와 final 메소드

- final이 지정되면 초기값 설정 후 더 이상 값을 변경할 수 없음.
- 클래스와 메소드를 선언할 때 final 키워드가 지정되면 상속과 관련이 있다는 의미임.



#### 상속할 수 없는 final 클래스

- 클래스를 선언할 때 final 키워드를 class 앞에 붙이면 상속할 수 없는 클래스가 됨.
- final 클래스는 부모 클래스가 될 수 없어 자식 클래스를 만들 수 없음.

```java
public final class 클래스 {...}
```

- 자바 표준 API에서 제공하는 String 클래스가 final 클래스로 선언되어 있음. 자식 클래스를 만들 수 없음.



#### 재정의할 수 없는 final 메소드

- 메소드를 선언할 때 final 키워드를 붙이면 재정의할 수 없는 메소드가 됨.
- 부모 클래스를 상속해서 자식 클래스를 선언할 때 부모 클래스에 선언된 final 메소드는 자식 클래스에서 재정의할 수 없음.

```java
public final 리턴타입 메소드([매개변수, ...]){...}
```



### 1.5. protected 접근 제한자

- protected : 같은 패키지에서는 default와 같이 접근 제한이 없지만 다른 패키지에서는 자식 클래스만 접근을 허용함.
- protected는 필드와 생성자, 메소드 선언에 사용될 수 있음.

- 다른 패키지의 자식 클래스에서 new 연산자를 사용해 부모 클래스 생성자를 직접 호출할 수는 없음. 자식 생성자에서 super()로 부모 생성자를 호출할 수 있음.



## 2. 타입 변환과 다형성

- 다형성 : 사용 방법은 동일하지만 다양한 객체를 이용해서 다양한 실행결과가 나오도록 하는 성질.
- 자동차가 타이어를 사용하는 방법은 동일하지만 어떤 타이어를 장착하느냐에 따라 주행 성능이 달라질 수 있음.
- 다형성을 구현하려면 메소드 재정의와 타입 변환이 필요함.
  - 자식 객체가 재정의된 메소드를 가지고 있을 경우, 부모 타입으로 자동 타입 변환 후에 메소드를 호출하면 재정의된 자식 메소드가 호출되면서 다양한 실행결과를 가져올 수 있음.(부모 클래스 타입 변수를 매개 변수로 받는 메소드는 매개값으로 부모 타입 받으면 -> 부모 메소드 호출 // 자식 타입 받으면 -> 자동 타입 변환 + 재정의된 메소드 호출 되어 다양한 실행 결과 발생.
  - 자동차 객체의 달린다 메소드, 타이어 객체의 구른다 메소드, 한국타이어 객체의 재정의된 빨리구른다 메소드. 자동차 객체의 달린다 메소드를 실행할 때, 매개값으로 타이어 타입 받으면 구른다 메소드 호출, 매개값으로 한국타이어 타입 받으면 빨리구른다 메소드 호출. -> 매개변수의 다형성 구현




### 2.1. 자동 타입 변환

- 클래스의 타입 변환은 상속 관계에 있는 클래스 사이에서 발생.
- 자식 클래스는 부모 클래스 타입으로 자동 타입 변환이 가능함.

```java
부모타입 변수 = 자식타입;
// 자동 타입 변환 발생.
```

- 세부적인 과정
  - 1. 자식 클래스로 자식 객체 생성
    2. 부모 클래스로 부모 클래스 변수 만든 후 자식 클래스 변수 대입 (자동 타입 변환)
    3. 부모 클래스 변수와 자식 클래스 변수는 둘 다 자식 객체 참조함. (부모 클래스 변수와 자식 클래스 변수 == 연산해보면 true가 나옴.)
- 바로 위의 부모가 아니더라도 상속 계층에서 상위 타입이라면 자동 타입 변환이 일어날 수 있음.
- 부모 타입으로 자동 타입 변환된 이후에는 부모 클래스에 선언된 필드와 메소드만 접근이 가능함.
- 메소드가 자식 클래스에서 재정의되었다면 자식 클래스의 메소드가 대신 호출됨.

```java
// 부모 클래스
class Parent {
    void method1(){...}
    void method2(){...}
}

// 자식 클래스
class Child extends Parent {
    void method2(){...} // 재정의
    void method3(){...}
}

// 실행 클래스
class ChildExample{
    pubilc static void main (String[] args){
        Child child = new Child();
        Parent parent = child;
        
        parent.method1(); // (O) 호출 가능.
        parent.method2(); // (O) 호출 가능. 재정의된 자식 클래스의 메소드임.
        parent.method3(); // (X) 호출 불가능.
    }
}
```

- 자식 클래스 메소드 1,2,3번 // 부모 클래스 메소드 1번 // 자동 타입 변환 이후에 접근 가능한 메소드는 1번과 2번(부모의 과거 메소드 + 자식의 재정의 메소드).



### 2.2. 필드의 다형성

- 부모 타입으로 변환해서 사용하는 이유는 다형성을 구현하기 위해서임.
- 필드의 타입을 부모 타입으로 선언하면 다양한 자식 객체들이 저장될 수 있기 때문에 필드 사용 결과가 달라짐.
- 다형성 구현
  - 부모 클래스를 상속하는 자식 클래스는 부모가 가지고 있는 필드와 메소드를 가지고 있으니 사용 방법이 동일함.
  - 자식 클래스는 부모의 메소드를 재정의해서 메소드의 실행 내용을 변경함으로써 더 우수한 실행 결과가 나오게 할 수 있음.
  - 자식 타입을 부모 타입으로 변환할 수 있음.



### 2.3. 매개 변수의 다형성

- 메소드 호출할 때 자동 타입 변환이 많이 발생함.
- 메소드를 호출할 때는 매개 변수의 타입과 동일한 매개값을 지정하는 것이 정석이지만, 매개값을 다양화하기 위해 매개 변수에 자식 객체를 지정할 수도 있음.
- 매개 변수의 타입이 클래스일 경우, 해당 클래스의 객체뿐만 아니라 자식 객체까지도 매개값으로 사용할 수 있음.
- 매개 변수의 다형성은 매개값으로 어떤 자식 객체가 제공되느냐에 따라 메소드의 실행결과가 다양해질 수 있다는 것.



### 2.4. 강제 타입 변환

- 강제 타입 변환(casting)은 부모 타입을 자식 타입으로 변환하는 것.
- 자식 타입이 부모 타입으로 자동 타입 변환한 후 다시 자식 타입으로 변환하려고 할 때 강제 타입 변환을 사용할 수 있음.
- 자식 타입이 부모 타입으로 자동 타입 변환되면서 접근 제한되었던 부분(자식 타입에서 새롭게 생성된 인스턴스 멤버)이 부모 타입이 자식 타입으로 강제 타입 변환되면서 제한 해제되는 것.

```java
Parent parent = new Child(); // 자동 타입 변환
Child child = (Child) parent; // 강제 타입 변환
```

```java
Parent parent = new Parent();
Child child = (Child) parent; // 컴파일 에러 발생.
// 처음부터 부모 타입으로 생성된 객체는 자식 타입으로 변환할 수 없음. 강제 타입 변환 불가능.
```



### 2.5. 객체 타입 확인

- instanceof 연산자 : 어떤 객체가 어떤 클래스의 인스턴스인지 확인.
- 좌항에 객체가 오고 우항에 타입이 옴.
- 좌항의 객체가 우항의 인스턴스이면, 즉 우항의 타입으로 객체가 생성되었다면 true를 리턴하고 그렇지 않으면 false를 리턴.

```java
public void method(Parent parent){
    if(parent instanceof Child){ // parent 매개 변수가 참조하는 객체가 Child인지 조사
        Child child = (Child) parent;
    }
}
```

- 타입을 확인하지 않고 강제 타입 변환을 시도하면 ClassCastException 발생할 수 있음.
- 예외가 발생하면 프로그램은 즉시 종료되기 때문에 강제 타입 변환을 하기 전에 instanceof 연산자로 변환시킬 타입의 객체인지 조사해서 잘못된 매개값으로 인해 프로그램이 종료되는 것을 막아야 함.



## 3. 추상 클래스

- 사전적 의미의 추상(abstract)은 실체 간에 공통되는 특성을 추출한 것을 말함.
- 객체를 직접 생성할 수 있는 클래스를 실체 클래스라고 한다면 이 클래스들의 공통적인 특성을 추출해서 선언한 클래스를 추상 클래스라고 함.
- 추상 클래스와 실체 클래스는 상속의 관계를 가지고 있음.
- 추상 클래스가 부모, 실체 클래스가 자식으로 구현되어 실체 클래스는 추상 클래스의 모든 특성을 물려받고, 추가적인 특성을 가질 수 있음.
- 추상 클래스 : 실체 클래스에서 공통되는 필드와 메소드를 따로 선언한 클래스



### 3.1. 추상 클래스의 용도

#### 공통된 필드와 메소드의 이름을 통일할 목적

- 실체 클래스를 설계하는 사람이 여러 사람일 경우, 실체 클래스마다 필드와 메소드가 제각기 다른 이름을 가질 수 있음.
- 전원을 켜는 메소드는 Telephone에서는 turnOn()으로 설계하고, SmartPhone에서는 powerOn()이라고 설계할 수 있음.
- 데이터와 기능이 모두 동일함에도 불구하고 이름이 다르다보니 객체마다 사용 방법이 달라짐.
- Phone이라는 추상 클래스에 turnOn() 메소드를 선언하고 Telephone과 SmartPhone은 Phone을 상속함으로써 필드와 메소드 이름을 통일할 수 있음.



#### 실체 클래스를 작성할 때 시간 절약

- 공통적인 필드와 메소드는 추상 클래에서 모두 선언해두고, 다른 점만 실체 클래스에서 선언하면 실체 클래스를 작성하는 데 시간을 절약할 수 있음.



### 3.2. 추상 클래스 선언

- class 앞에 abstract 키워드 붙이기.
- abstract를 붙이면 new 연산자를 이용해서 객체를 만들지 못하고, 상속을 통해 자식 클래스만 만들 수 있음.

```java
public abstract class 클래스 { ... }
```

- 추상 클래스도 필드, 생성자, 메소드 선언을 할 수 있음.
- new 연산자로 직접 생성자를 호출할 수는 없지만, 자식 객체가 생성될 때 super(...)를 호출해서 추상 클래스 객체를 생성하므로 추상 클래스도 생성자가 반드시 있어야 함.



### 3.3. 추상 메소드와 재정의

- 추상 클래스는 실체 클래스의 멤버(필드, 메소드)를 통일하는 데 목적이 있음.
- 추상 메소드는 abstract 키워드와 함께 메소드의 선언부만 있고 메소드 실행 내용인 중괄호 {}가 없는 메소드를 말함.
- 메소드의 선언만 통일하고, 실행 내용은 실체 클래스마다 달라야 하는 경우에 추상 메소드를 사용함.

```java
[public | protected] abstract 리턴타입 메소드이름(매개변수, ...);
```

- 추상 클래스 설계 시 하위 클래스가 반드시 실행 내용을 채우도록 강제하고 싶은 메소드가 있을 경우 해당 메소드를 추상 메소드로 선언함.
- 자식 클래스는 반드시 추상 메소드를 재정의해서 실행 내용을 작성해야 함. 그렇지 않으면 컴파일 에러 발생.
