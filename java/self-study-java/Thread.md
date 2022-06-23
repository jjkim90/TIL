# 스레드

<br>

> 이 문서는 [<혼자 공부하는 자바 - 신용권>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.

<br>

## 1. 멀티 스레드

- 프로세스(process) : 애플리케이션을 실행하면 운영체제로부터 실행에 필요한 메모리를 할당받아 애플리케이션이 실행됨. 운영체제에서 실행 중인 하나의 애플리케이션을 프로세스라고 부름.
- 하나의 애플리케이션은 멀티 프로세스(multi process)를 만들기도 함. 메모장 애플리케이션을 2개 실행했다면 2개의 메모장 프로세스가 생성된 것임.
- 스레드(thread) : 프로세스 내부에서 코드의 실행 흐름.

<br>

### 1.1. 스레드

- 애플리케이션은 한 프로세스 내에서 멀티 태스킹을 할 수 있도록 만들어질 수 있음. 미디어 플레이어는 동영상 재생과 음악 재생이라는 두 가지 작업을 동시에 처리함.

- 하나의 스레드는 하나의 코드 실행 흐름이기 때문에 한 프로세스 내에 스레드가 2개라면 2개의 코드 실행 흐름이 생긴다는 의미임. 이를 멀티 스레드라고 함.
- 멀티 프로세스는 자신의 메모리를 가지고 실행하므로 서로 독립적이지만, 멀티 스레드는 하나의 프로세스 내부에 생성되므로 스레드 하나가 예외를 발생시키면 프로세스 자체가 종료될 수 있어 다른 스레드도 영향을 받음.
- 멀티 스레드로 동작하는 메신저의 경우 파일을 전송하는 스레드에서 예외가 발생하면 메신저 프로세스 자체가 종료되므로 채팅 스레드도 같이 종료됨.
- 멀티 스레드에서는 예외 처리에 만전을 기해야 함.

<br>

### 1.2. 메인 스레드

- 자바의 모든 애플리케이션은 메인 스레드(main thread)가 main() 메소드를 실행하면서 시작함.
- 메인 스레드는 필요에 따라 작업 스레드들을 만들어서 병렬로 코드를 실행할 수 있음. 즉, 멀티 스레드를 생성해서 멀티 태스킹을 수행함.
- 멀티 스레드 애플리케이션에서는 실행 중인 스레드가 하나라도 있다면 프로세스는 종료되지 않음. 메인 스레드가 작업 스레드보다 먼저 종료되더라도 작업 스레드가 계속 실행 중이라면 프로세스는 종료되지 않음.

<br>

### 1.3. 작업 스레드 생성과 실행

- 멀티 스레드로 실행하는 어플리케이션을 개발하려면 먼저 몇 개의 작업을 병렬로 실행할지 결정하고 각 작업별로 스레드를 생성해야 함.
- 자바에서는 작업 스레드도 객체로 생성되기 때문에 클래스가 필요함.
- java.lang.Thread 클래스를 직접 객체화해서 생성해도 되지만, Thread 클래스를 상속해서 하위 클래스를 만들어 생성할 수도 있음.

<br>

#### Thread 클래스로부터 직접 생성

- Runnable을 매개값으로 갖는 생성자를 호출해야 함.

```java
// Thread 클래스로부터 직접 생성
Thread thread = new Thread(Runnable target);
```

- Runnable은 작업 스레드가 실행할 수 있는 코드를 가지고 있는 객체라고 해서 붙여진 이름임.
- Runnable은 인터페이스 타입임. 구현 객체를 만들어서 대입해야 함.
- Runnable에는 run() 메소드 하나가 정의되어 있음. 구현 클래스는 run()을 재정의해서 작업 스레드가 실행할 코드를 작성해야 함.

```java
// Runnable 구현 클래스 작성
class Task implements Runnable {
	public void run(){
        스레드가 실행할 코드;
    }
}
```

- Runnable 인터페이스의 구현 객체를 생성한 후, 이것을 매개값으로 해서 Thread 생성자를 호출하면 작업 스레드가 생성됨.

```java
// 작업 스레드 생성하기
Runnable task = new Task();
Thread thread = new Thread(task);
```

- Runnable 익명 객체를 매개값으로 사용할 수도 있음. 코드 절약되어 더 많이 사용됨.

```java
// 작업 스레드 생성하기 (익명 객체)
Thread thread = new Thread(new Runnable(){
    public void run(){
        스레드가 실행할 코드;
    }
});
```

- 작업 스레드는 start() 메소드가 호출되면 실행됨. 생성 즉시 실행되는 것이 아님.

```java
// 작업 스레드 호출
thread.start();
```

- start() 메소드가 호출되면, 작업 스레드는 매개값으로 받은 Runnable의 run() 메소드를 실행하면서 자신의 작업을 처리함.

- 예제. 비프음을 발생시키면서 동시에 "띵" 출력시키기. 출력은 메인 스레드가 담당하고 비프음을 들려주는 것은 작업 스레드가 담당.

```java
// 비프음을 들려주는 작업 정의
package sec01.exam01;

import java.awt.Toolkit;

public class BeepTask implements Runnable {

	@Override
	public void run() {
		Toolkit toolkit = Toolkit.getDefaultToolkit();
		for (int i = 0; i < 5; i++) {
			toolkit.beep();
			try {
				Thread.sleep(500);
			} catch (InterruptedException e) {
			}
		}
	}

}

// 메인 스레드와 작업 스레드가 동시에 실행
package sec01.exam01;

public class BeepPrintExample2 {

	public static void main(String[] args) {
		Runnable task = new BeepTask();
		Thread beepThread = new Thread(task);

		beepThread.start();

		for (int i = 0; i < 5; i++) {
			System.out.println("띵");
			try {
				Thread.sleep(500);
			} catch (Exception e) {
			}
		}

	}

}
```

```java
// Runnable 익명 구현 객체 이용
package sec01.exam03;

import java.awt.Toolkit;

public class BeepPrintExample3 {

	public static void main(String[] args) {
		Thread thread = new Thread(new Runnable() {

			@Override
			public void run() {
				Toolkit toolkit = Toolkit.getDefaultToolkit();
				for (int i = 0; i < 5; i++) {
					toolkit.beep();
					try {
						Thread.sleep(500);
					} catch (InterruptedException e) {
					}
				}

			}

		});
		thread.start();

		for (int i = 0; i < 5; i++) {
			System.out.println("띵");
			try {
				Thread.sleep(500);
			} catch (InterruptedException e) {
			}
		}

	}

}
```

<br>

#### Thread 하위 클래스로부터 생성

- 작업 스레드가 실행할 작업을 Runnable로 만들지 않고, Thread의 하위 클래스로 작업 스레드를 정의하면서 작업 내용을 포함시킬 수도 있음.
- 작업 스레드 클래스를 정의하는 방법은 Thread 클래스를 상속한 후 run() 메소드를 재정의해서 스레드가 실행할 코드를 작성하면 됨.

```java
// 작업 스레드 객체 생성
public class WorkerThread extends Thread{
	@Override
    public void run(){
        스레드가 실행할 코드;
    }
}
Thread thread = new WorkerThread();
```

- Thread 익명 객체로 작업 스레드 객체를 생성할 수도 있음.

```java
// 익명 자식 객체로 작업 스레드 객체 생성
Thread thread = new Thread(){
    public void run(){
        스레드가 실행할 코드;
    }
}
```

- 예제. 비프음을 발생시키면서 동시에 "띵" 출력시키기. 출력은 메인 스레드가 담당하고 비프음을 들려주는 것은 작업 스레드가 담당. Thread 하위 클래스 이용하기.

```java
// 비프음을 들려주는 스레드
package sec01.exam04;

import java.awt.Toolkit;

public class BeepThread extends Thread {
	@Override
	public void run() {
		Toolkit toolkit = Toolkit.getDefaultToolkit();
		for (int i = 0; i < 5; i++) {
			toolkit.beep();
			try {
				Thread.sleep(500);
			} catch (InterruptedException e) {
			}
		}
	}

}

// 메인 스레드와 작업 스레드가 동시에 실행
package sec01.exam04;

public class BeepPrintExample4 {

	public static void main(String[] args) {
		Thread thread = new BeepThread();
		thread.start();

		for (int i = 0; i < 5; i++) {
			System.out.println("띵");
			try {
				Thread.sleep(500);
			} catch (InterruptedException e) {
			}
		}
	}

}
```

```java
// Thread 익명 자식 객체 이용하기
package sec01.exam05;

import java.awt.Toolkit;

public class BeepPrintExample5 {

	public static void main(String[] args) {
		Thread thread = new Thread() {
			@Override
			public void run() {
				Toolkit toolkit = Toolkit.getDefaultToolkit();
				for (int i = 0; i < 5; i++) {
					toolkit.beep();
					try {
						Thread.sleep(500);
					} catch (Exception e) {
					}
				}
			}
		};
		thread.start();

		for (int i = 0; i < 5; i++) {
			System.out.println("띵");
			try {
				Thread.sleep(500);
			} catch (Exception e) {
			}
		}
	}

}
```

<br>

#### 스레드의 이름

- 스레드는 자신의 이름을 가지고 있음.
- 메인 스레드는 'main'이라는 이름임.
- 직접 생성한 스레드는 자동적으로 'Thread-n'이라는 이름으로 설정됨.
- 다른 이름으로 설정하고 싶다면 Thread 클래스의 setName() 메소드로 변경하면 됨.

```java
// 작업 스레드 이름 변경하기
thread.setName("스레드이름");
```

- 스레드 이름을 알고 싶은 경우에는 getName() 메소드 호출.

```java
// 작업 스레드 이름 알아내기
thread.getName();
```

- Setter와 Getter 둘 다 Thread 클래스의 인스턴스 메소드이므로 스레드 객체의 참조가 필요함.
- 만약 스레드 객체의 참조를 가지고 있지 않다면, Thread 클래스의 정적 메소드인 currentThread()를 이용해서 현재 스레드의 참조를 얻을 수 있음.

```java
// 현재 스레드의 참조 얻기
Thread thread = Thread.currentThread();
```

- 예제. 메인 스레드의 참조를 얻어 스레드 이름을 콘솔에 출력하고 새로 생성한 스레드의 이름을 setName() 메소드로 설정한 후 getName() 메소드로 읽어오기.

```java
// 메인 스레드 이름 출력 및 UserThread 생성 및 시작
package sec01.exam06;

public class ThreadNameExample {

	public static void main(String[] args) {
		Thread mainThread = Thread.currentThread();
		System.out.println("프로그램 시작 스레드 이름 : " + mainThread.getName());

		ThreadA threadA = new ThreadA();
		System.out.println("작업 스레드 이름 : " + threadA.getName());
		threadA.run();

		ThreadB threadB = new ThreadB();
		System.out.println("작업 스레드 이름 : " + threadB.getName());
		threadB.run();
	}
}
// ThreadA 클래스
package sec01.exam06;

public class ThreadA extends Thread {

	public ThreadA() {
		setName("ThreadA");
	}

	@Override
	public void run() {
		for (int i = 0; i < 2; i++) {
			System.out.println(getName() + "가 출력한 내용");
		}
	}
}
// ThreadB 클래스
package sec01.exam06;

public class ThreadB extends Thread {

	public ThreadB() {
		setName("ThreadB");
	}

	@Override
	public void run() {
		for (int i = 0; i < 2; i++) {
			System.out.println(getName() + "가 출력한 내용");
		}
	}
}
```

<br>

### 1.4. 동기화 메소드

- 싱글 스레드 프로그램에서는 1개의 스레드가 객체를 독차지해서 사용하면 되지만, 멀티 스레드 프로그램에서는 스레드들이 객체를 공유해서 작업해야 하는 경우가 있음.

<br>

#### 공유 객체를 사용할 때의 주의할 점

- 멀티 스레드 프로그램에서 스레드들이 객체를 공유해서 작업해야 하는 경우, 스레드 A가 사용하던 객체를 스레드 B가 상태를 변경할 수 있기 때문에 스레드 A가 의도했던 것과는 다른 결과를 산출할 수도 있음.

- 공유 객체 예제
  - User1 스레드가 Calculator 객체의 memory 필드에 100을 먼저 저장하고 2초간 일시 정지
  - User2 스레드가 memory 필드값을 50으로 변경
  - 2초가 지나 User1 스레드가 다시 실행 상태가 되어 memory 필드값 출력

```java
// 공유 객체 예제
// MainThreadExample.java
public class MainThreadExample {
	public static void main(String[] args) {
		Calculator calculator = new Calculator();
		
		User1 user1 = new User1();
		user1.setCalculator(calculator);
		user1.start();

		User2 user2 = new User2();
		user2.setCalculator(calculator);
		user2.start();
	}
}

// Calculator.java
public class Calculator {
	private int memory;

	public int getMemory() {
		return memory;
	}

	public void setMemory(int memory) {
		this.memory = memory;
		try {
			Thread.sleep(2000);
		} catch(InterruptedException e) {}	
		System.out.println(Thread.currentThread().getName() + ": " +  this.memory);
	}
}

// User1.java
public class User1 extends Thread {	
	private Calculator calculator;
	
	public void setCalculator(Calculator calculator) {
		this.setName("User1");
		this.calculator = calculator;
	}
	
	public void run() {
		calculator.setMemory(100);
	}
}

// User2.java
public class User2 extends Thread {	
	private Calculator calculator;
	
	public void setCalculator(Calculator calculator) {
		this.setName("User2");
		this.calculator = calculator;
	}
	
	public void run() {
		calculator.setMemory(50);
	}
}
```

<br>

#### 동기화 메소드

- 스레드가 사용 중인 객체를 다른 스레드가 변경할 수 없게 하려면 스레드 작업이 끝날 때까지 객체에 잠금을 걸어서 다른 스레드가 사용할 수 없도록 해야 함.
- 임계 영역(Critical Section) : 멀티 스레드 프로그램에서 단 하나의 스레드만 실행할 수 있는 코드 영역.
- 자바는 임계 영역을 지정하기 위해 동기화(synchronized) 메소드를 제공함.
- 스레드가 객체 내부의 동기화 메소드를 실행하면 즉시 객체에 잠금을 걸어 다른 스레드가 동기화 메소드를 실행하지 못하도록 함.
- 동기화 메소드를 만들려면 메소드 선언에 synchronized 키워드를 붙이면 됨. 인스턴스와 정적 메소드 어디든 붙일 수 있음.

- 동기화 메소드는 메소드 전체 내용이 임계 영역이므로 스레드가 동기화 메소드를 실행하는 즉시 객체에는 잠금이 일어나고, 스레드가 동기화 메소드를 실행 종료하면 잠금이 풀림.
- 만약 동기화 메소드가 여러 개 있을 경우, 스레드가 이들 중 하나를 실행할 때 다른 스레드는 해당 메소드는 물론이고 다른 동기화 메소드도 실행할 수 없음. 하지만 이때 다른 스레드에서 일반 메소드는 실행이 가능함.

```java
// 동기화 메소드로 수정된 공유 객체
public class Calculator {
	private int memory;

	public int getMemory() {
		return memory;
	}

	public synchronized void setMemory(int memory) {
		this.memory = memory;
		try {
			Thread.sleep(2000);
		} catch(InterruptedException e) {}	
		System.out.println(Thread.currentThread().getName() + ": " +  this.memory);
	}
}
```

<br>

## 2. 스레드 제어

- 스레드 객체를 생성하고 start() 메소드를 호출하면 바로 실행되는 것이 아니라 실행 대기 상태가 됨.
- 실행 대기 상태란 언제든지 실행할 준비가 되어 있는 상태를 말함.
- 운영체제는 실행 대기 상태에 있는 스레드 중에서 하나를 선택해서 실행 상태로 만듦.
- 실행 상태의 스레드는 run() 메소드를 모두 실행하기 전에 다시 실행 대기 상태로 돌아갈 수 있으며, 실행 대기 상태에 있는 다른 스레드가 선택되어 실행 상태가 되기도 함.
- 실행 상태에서 run() 메소드의 내용이 모두 실행되면 스레드의 실행이 멈추고 종료 상태가 됨.

<br>

### 2.1. 스레드 상태

- 실행 대기 상태(Runnable) : 실행을 기다리고 있는 상태.
- 실행 상태(Running) : 운영체제가 하나의 스레드를 선택하고 CPU(코어)가 run() 메소드를 실행하는 상태.
- 종료 상태(Terminated) : run() 메소드 종료되고 스레드 실행 멈춘 상태.
- 일시 정지 상태 : 스레드가 실행할 수 없는 상태.
- 일시 정지 상태에서는 바로 실행 상태로 돌아갈 수 없고, 일시 정지 상태에서 빠져나와 실행 대기 상태로 가야 함.

<br>

### 2.2. 스레드 상태 제어

- 실행 중인 스레드의 상태를 변경하는 것을 스레드 상태 제어라고 함.
- 
- 상태 변화를 가져오는 메소드의 종류
  - interrupt() : 일시 정지 상태의 스레드에서 InterruptedException을 발생시켜, 예외 처리 코드(catch)에서 실행 대기 상태로 가거나 종료 상태로 갈 수 있도록 함.
  - sleep(long millis) : 주어진 시간 동안 스레드를 일시 정지 상태로 만듦. 주어진 시간이 지나면 자동적으로 실행 대기 상태가 됨.
  - stop() : 스레드를 즉시 종료함. 불안전한 종료를 유발하므로 사용하지 않는 것이 좋음.

<br>

#### 주어진 시간 동안 일시 정지

- 실행 중인 스레드를 일정 시간 멈추게 하고 싶다면 Thread 클래스의 정적 메소드인 sleep()을 사용하면 됨.

```java
// 기본 형식

try {
    Thread.sleep(1000);
} catch(InterruptedException e) {
    // interrupt() 메소드가 호출되면 실행 
}
```

```java
// 3초 주기로 10번 비프음 발생

import java.awt.Toolkit;

public class SleepExample {
	public static void main(String[] args) {
		Toolkit toolkit = Toolkit.getDefaultToolkit();		
		for(int i=0; i<10; i++) {
			toolkit.beep();
			try {
				Thread.sleep(3000);
			} catch(InterruptedException e) {			
			}		
		}	
	}
}
```

<br>

#### 스레드의 안전한 종료

- stop() 메소드는 deprecated 되었음.

<br>

##### stop 플래그를 이용하는 방법

- stop 플래그를 이용해서 run() 메소드의 종료를 유도할 수 있음.

```java
// 기본 형식

public class XXThread extends Thread {
    private boolean stop;
    
    public void run(){
        while( !stop ){
            스레드가 반복 실행하는 코드;
        }
        //스레드가 사용한 자원 정리
    }
}
```

```java
// stop 플래그 예제
// StopFlagExample.java
public class StopFlagExample {
	public static void main(String[] args)  {
		PrintThread printThread = new PrintThread();
		printThread.start();
		
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
		}
		
		printThread.setStop(true);
	}
}

// PrintThread1.java
public class PrintThread extends Thread {
	private boolean stop;
	
	public void setStop(boolean stop) {
	  this.stop = stop;
	}
	
	public void run() {	
		while(!stop) {
			System.out.println("실행 중");
		}	
		System.out.println("자원 정리");
		System.out.println("실행 종료");
	}
}
```

<br>

##### interrupt() 메소드를 이용하는 방법

- interrupt() 메소드는 스레드가 일시 정지 상태에 있을 때 InterruptedException을 발생시키는 역할을 함. 이를 이용하면 run() 메소드를 정상 종료할 수 있음.

```java
// intterrupt() 메소드 예제
// InterruptExample.java
public class InterruptExample {
	public static void main(String[] args)  {
		Thread thread = new PrintThread();
		thread.start();
		
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
		}
		
		thread.interrupt();
	}
}
// PrintThread2.java
public class PrintThread extends Thread {
	public void run() {	
		try {
			while(true) {
				System.out.println("실행 중");
				Thread.sleep(1);
			}	
		} catch(InterruptedException e) {}
		
		System.out.println("자원 정리");
		System.out.println("실행 종료");
	}
}
```

- 스레드가 실행 대기 또는 실행 상태에 있을 때 interrupt() 메소드가 실행되면 즉시 InterruptedException이 발생하지 않고, 스레드가 미래에 일시 정지 상태가 되면 Interrupted Exception이 발생함.
- 스레드가 일시 정지 상태가 되지 않으면 interrupt() 메소드 호출은 아무런 의미가 없음.
- 일시 정지를 만들지 않고도 interrupt()의 호출 여부 알 수 있음.
- interrupt() 메소드가 호출되었다면 스레드의 interrupted()와 isInterrupted() 메소드는 true를 리턴함.
  - interruped()는 정적 메소드로 현재 스레드가 interrupted 되었는지 확인하는 것.
  - isInterruped()는 인스턴스 메소드로 현재 스레드가 interrupted 되었는지 확인 함.
  - 둘 중 어떤 것을 사용해도 무방함.

```java
// 무한 반복해서 출력하는 스레드
// PrintThread.java
public class PrintThread extends Thread {
	public void run() {	
		while(true) {
			System.out.println("실행 중");
			if(Thread.interrupted()) {
				break;
			}
		}
		
		System.out.println("자원 정리");
		System.out.println("실행 종료");
	}
}

// interruptExample.java
public class InterruptExample {
	public static void main(String[] args)  {
		Thread thread = new PrintThread();
		thread.start();
		
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
		}
		
		thread.interrupt();
	}
}
```

<br>

### 2.3. 데몬 스레드

- 데몬(daemon) 스레드는 주 스레드의 작업을 돕는 보조적인 역할을 수행하는 스레드.
- 주 스레드가 종료되면 데몬 스레드는 강제적으로 자동 종료되는데, 그 이유는 주 스레드의 보조 역할을 수행하므로 주 스레드가 종료되면 데몬 스레드의 존재 의미가 사라지기 때문임.
- 데몬 스레드의 적용 예는 워드프로세서의 자동 저장, 미디어 플레이어의 동영상 및 음악 재생, 쓰레기 수집기 등이 있는데, 이 기능들은 주 스레드(워드프로세서, 미디어 플레이어, JVM)가 종료되면 같이 종료됨.
- 스레드를 데몬으로 만들기 위해서는 주 스레드가 데몬이 될 스레드의 setDaemon(true)를 호출해주면 됨.
- start() 메소드가 호출되고 나서 setDaemon(true)를 호출하면 IlegalThreadStateException이 발생하기 때문에 start() 메소드 호출 전에 setDaemon(true)를 호출해야 함.
- 현재 실행 중인 스레드가 데몬 스레드인지 아닌지를 구별하려면 isDaemon() 메소드의 리턴값을 조사하면 됨. 데몬 스레드일 경우 true를 리턴함.

```java
// 데몬 스레드 예시
// AutoSaveThread.java
public class AutoSaveThread extends Thread {
	public void save() {
		System.out.println("작업 내용을 저장함.");
	}
	
	@Override
	public void run() {
		while(true) {
			try {
				Thread.sleep(1000);
			} catch (InterruptedException e) {
				break;
			}
			save();
		}
	}
}
// DaemonExample.java
public class DaemonExample {
	public static void main(String[] args) {
		AutoSaveThread autoSaveThread = new AutoSaveThread();
		autoSaveThread.setDaemon(true);
		autoSaveThread.start();
		
		try {
			Thread.sleep(3000);
		} catch (InterruptedException e) {
		}
		
		System.out.println("메인 스레드 종료");
	}
}
```

<br>

# References

- [혼자 공부하는 자바 / 신용권 / 한빛미디어 / 2019](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=)

<br>
