# 스레드



> 이 문서는 [<혼자 공부하는 자바 - 신용권>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.



## 1. 멀티 스레드

- 프로세스(process) : 애플리케이션을 실행하면 운영체제로부터 실행에 필요한 메모리를 할당받아 애플리케이션이 실행됨. 운영체제에서 실행 중인 하나의 애플리케이션을 프로세스라고 부름.
- 하나의 애플리케이션은 멀티 프로세스(multi process)를 만들기도 함. 메모장 애플리케이션을 2개 실행했다면 2개의 메모장 프로세스가 생성된 것임.
- 스레드(thread) : 프로세스 내부에서 코드의 실행 흐름.



### 1.1. 스레드

- 애플리케이션은 한 프로세스 내에서 멀티 태스킹을 할 수 있도록 만들어질 수 있음. 미디어 플레이어는 동영상 재생과 음악 재생이라는 두 가지 작업을 동시에 처리함.

- 하나의 스레드는 하나의 코드 실행 흐름이기 때문에 한 프로세스 내에 스레드가 2개라면 2개의 코드 실행 흐름이 생긴다는 의미임. 이를 멀티 스레드라고 함.
- 멀티 프로세스는 자신의 메모리를 가지고 실행하므로 서로 독립적이지만, 멀티 스레드는 하나의 프로세스 내부에 생성되므로 스레드 하나가 예외를 발생시키면 프로세스 자체가 종료될 수 있어 다른 스레드도 영향을 받음.
- 멀티 스레드로 동작하는 메신저의 경우 파일을 전송하는 스레드에서 예외가 발생하면 메신저 프로세스 자체가 종료되므로 채팅 스레드도 같이 종료됨.
- 멀티 스레드에서는 예외 처리에 만전을 기해야 함.



### 1.2. 메인 스레드

- 자바의 모든 애플리케이션은 메인 스레드(main thread)가 main() 메소드를 실행하면서 시작함.
- 메인 스레드는 필요에 따라 작업 스레드들을 만들어서 병렬로 코드를 실행할 수 있음. 즉, 멀티 스레드를 생성해서 멀티 태스킹을 수행함.
- 멀티 스레드 애플리케이션에서는 실행 중인 스레드가 하나라도 있다면, 프로세스는 종료되지 않음. 메인 스레드가 작업 스레드보다 먼저 종료되더라도 작업 스레드가 계속 실행 중이라면 프로세스는 종료되지 않음.



### 1.3. 작업 스레드 생성과 실행

- 멀티 스레드로 실행하는 어플리케이션을 개발하려면 먼저 몇 개의 작업을 병렬로 실행할지 결정하고 각 작업별로 스레드를 생성해야 함.
- 자바에서는 작업 스레드도 객체로 생성되기 때문에 클래스가 필요함.
- java.lang.Thread 클래스를 직접 객체화해서 생성해도 되지만, Thread 클래스를 상속해서 하위 클래스를 만들어 생성할 수도 있음.



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



### 1.4. 동기화 메소드

#### 공유 객체를 사용할 때의 주의할 점

- 멀티 스레드 프로그램에서 스레드들이 객체를 공유해서 작업해야 하는 경우, 스레드 A가 사용하던 객체를 스레드 B가 상태를 변경할 수 있기 때문에 스레드 A가 의도했던 것과는 다른 결과를 산출할 수도 있음.







#### 동기화 메소드



## 2. 스레드 제어



### 2.1. 스레드 상태



### 2.2. 스레드 상태 제어



#### 스레드의 안전한 종료



##### stop 플래그를 이용하는 방법



##### interrupt() 메소드를 이용하는 방법



### 2.3. 데몬 스레드

