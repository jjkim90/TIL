# 입출력 스트림

<br>

> 이 문서는 [<혼자 공부하는 자바 - 신용권>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.

<br>

## 1. 입출력 스트림

### 1.1. 입출력 스트림의 종류

- 바이트(byte) 기반 스트림 : 그림, 멀티미디어 등의 바이너리 데이터를 읽고 출력할 때 사용.
  - 최상위 클래스 입력 : InputStream
  - 최상위 클래스 출력 : OutputStream
  - 하위 클래스 입력 : XXXInputStream
  - 하위 클래스 출력 : XXXOutputStream
- 문자(character) 기반 스트림 : 문자  데이터를 읽고 출력할 때 사용.
  - 최상위 클래스 입력 : Reader
  - 최상위 클래스 출력 : Writer
  - 하위 클래스 입력 : XXXReader
  - 하위 클래스 출력 : XXXWriter

<br>

### 1.2. 바이트 출력 스트림: OutputStream

- OutputStream은 바이트 기반 출력 스트림의 최상위 클래스로 추상 클래스임.

- 모든 바이트 기반 출력 스트림 클래스는 OutputStream 클래스를 상속 받아서 만들어짐.
- FileOutputStream, PrintStream, BufferedOutputStream, DataOutputStream 클래스는 모두 OutputStream 클래스를 상속하고 있음.
- OutputStream 클래스의 주요 메소드
  - `write(int b)` : 1byte를 출력함.
  - `write(byte[] b)` : 매개값으로 주어진 배열 b의 모든 바이트를 출력함.
  - `write(byte[] b, int off, int len)` : 매개값으로 주어진 배열b[off]부터 len개까지의 바이트를 출력함.
  - `flush()` : 출력 버퍼에 잔류하는 모든 바이트를 출력함.
  - `close()` : 출력 스트림을 닫음.

<br>

#### write(int b) 메소드

- 매개 변수로 주어지는 int(4byte)에서 끝 1byte만 출력 스트림으로 보냄.
- 매개 변수가 int 타입이므로 4byte 모두를 보내는 것은 아님.

```java
// 1byte씩 출력하기

import java.io.FileOutputStream;
import java.io.OutputStream;

public class WriteExample {
	public static void main(String[] args) throws Exception {
		OutputStream os = new FileOutputStream("C:/Temp/test1.db");
		
		byte a = 10;
		byte b = 20;
		byte c = 30;
		
		os.write(a);
		os.write(b);
		os.write(c);
		
		os.flush();
		os.close();
	}
}
```

- 출력 스트림은 출력할 바이트를 바로 보내는 것이 아니라 내부 버퍼(저장소)에 우선 저장해 놓음.
- flush() 메소드는 이 내부 버퍼에 잔류된 바이트를 모두 출력하는 역할을 함.

- 액세스 거부가 발생한다면 이클립스를 관리자 권한으로 실행하면 해결됨.



#### write(byte[] b) 메소드

- 매개값으로 주어진 배열의 모든 바이트를 출력 스트림으로 보냄.

```java
// 배열 전체를 출력하기

import java.io.FileOutputStream;
import java.io.OutputStream;

public class WriteExample {
	public static void main(String[] args) throws Exception {
		OutputStream os = new FileOutputStream("C:/Temp/test2.db");
		
		byte[] array =  { 10, 20, 30 };
		
		os.write(array);
		
		os.flush();
		os.close();
	}
}
```



#### write(byte[] b, int off, int len) 메소드

- b[off]부터 len개의 바이트를 출력 스트림으로 보냄.

```java
// 배열 일부를 출력하기

import java.io.FileOutputStream;
import java.io.OutputStream;

public class WriteExample {
	public static void main(String[] args) throws Exception {
		OutputStream os = new FileOutputStream("C:/Temp/test3.db");
		
		byte[] array =  { 10, 20, 30, 40, 50 };
		
		os.write(array, 1, 3);
		
		os.flush();
		os.close();
	}
}
```



### 1.3. 바이트 입력 스트림: InputStream

- InputStream은 바이트 기반 입력 스트림의 최상위 클래스로 추상 클래스임.
- 모든 바이트 기반 입력 스트림은 InputStream 클래스를 상속받아서 만들어짐.
- FileInputStream, BufferedInputStream, DataInputStream 클래스는 모두 InputStream 클래스를 상속하고 있음.
- InputStream 클래스의 주요 메소드
  - `read()` : 1byte를 읽고 읽은 바이트를 리턴함.
  - `read(byte[] b)` : 읽은 바이트를 매개값으로 주어진 배열에 저장하고 읽은 바이트 수를 리턴함.
  - `read(byte[] b, int off, int len)` : len개의 바이트를 읽고 매개값으로 주어진 배열에서  b[off]부터 len개까지 저장함. 그리고 읽은 바이트 수를 리턴함.
  - `close()` : 입력 스트림을 닫음.



#### read() 메소드

- 입력 스트림으로부터 1byte를 읽고 int(4byte) 타입으로 리턴함.
- 리턴된 4byte 중 끝 1byte에만 데이터가 들어 있음.
- 더 이상 입력 스트림으로부터 바이트를 읽을 수 없다면 read() 메소드는 -1을 리턴하는데, 이것을 이용하면 읽을 수 있는 마지막 바이트까지 반복해서 1byte씩 읽을 수 있음.

```java
// 1byte씩 읽기

import java.io.FileInputStream;
import java.io.InputStream;

public class ReadExample {
	public static void main(String[] args) throws Exception {
		InputStream is = new FileInputStream("C:/Temp/test1.db");
		while(true) {
			int data = is.read();
			if(data == -1) break;
			System.out.println(data);
		}
		is.close();
	}
}
```



#### read(byte[] b) 메소드

- 입력 스트림으로부터 매개값으로 주어진 배열의 길이만큼 바이트를 읽고 해당 배열에 저장함.
- 읽은 바이트 수를 리턴함.
- 실제로 읽은 바이트 수가 배열의 길이보다 적을 경우, 읽은 수만큼만 리턴함.
- 입력 스트림으로부터 바이트를 더 이상 읽을 수 없다면 -1을 리턴하는데, 이것을 이용하면 읽을 수 있는 마지막 바이트까지 반복해서 읽을 수 있음.

```java
// 배열 길이만큼 읽기

package sec01.exam05;

import java.io.FileInputStream;
import java.io.InputStream;

public class ReadExample {
	public static void main(String[] args) throws Exception {
		InputStream is = new FileInputStream("C:/Temp/test2.db");
		
		byte[] buffer = new byte[100];
		
		while(true) {
			int readByteNum = is.read(buffer);
			if(readByteNum == -1) break;
			for(int i=0; i<readByteNum; i++) {
				System.out.println(buffer[i]);
			}
		}
		
		is.close();
	}
}
```

- 많은 양의 바이트를 읽을 때는 read(byte[] b) 메소드를 사용하는 것이 좋음.
- 입력 스트림으로부터 100개의 바이트가 들어온다면 read() 메소드는 100번을 반복해서 읽어야 하지만, read(byte[] b) 메소드는 한 번 읽을 때 배열 길이만큼 읽기 때문에 읽는 횟수가 현저히 줄어듦.



#### read(byte[] b, int off, int len) 메소드

- 입력 스트림으로부터 len개의 바이트만큼 읽고, 매개값으로 주어진 바이트 배열 b[off]부터 len개까지 저장함.
- 읽은 바이트 수인 len개를 리턴함.
- 실제로 읽은 바이트 수가 len개보다 작을 경우에는 읽은 수만큼만 리턴함.
- 입력 스트림으로부터 바이트를 더 이상 읽을 수 없다면 -1을 리턴함.
- read(byte[] b) 메소드와의 차이점은 한 번에 읽어들이는 바이트 수를 len 매개값으로 조절할 수 있고, 배열에서 저장이 시작되는 인덱스를 지정할 수 있다는 점임.
- 만약 off를 0으로, len을 배열의 길이로 준다면 read(byte[] b) 메소드와 동일함.

```java
// 지정한 길이만큼 읽기

package sec01.exam06;

import java.io.FileInputStream;
import java.io.InputStream;

public class ReadExample {
	public static void main(String[] args) throws Exception {
		InputStream is = new FileInputStream("C:/Temp/test3.db");
		
		byte[] buffer = new byte[5];
		
		int readByteNum = is.read(buffer, 2, 3);
		if(readByteNum != -1) {
			for(int i=0; i<buffer.length; i++) {
				System.out.println(buffer[i]);
			}
		}
		
		is.close();
	}
}

```



### 1.4. 문자 출력 스트림: Writer

#### write(int c) 메소드

#### write(char[] cbuf) 메소드

#### write(char[] cbuf, int off, ont len) 메소드

#### write(String str)와 write(String str, int off, int len) 메소드



### 1.5. 문자 입력 스트림: Reader

#### read() 메소드

#### read(char[] cbuf) 메소드

#### read(char[] cbuf, int off, int len) 메소드



## 2. 보조 스트림

### 2.1. 보조 스트림 연결하기

### 2.2. 문자 변환 보조 스트림

#### OutputStreamWriter

#### InputStreamReader

### 2.3. 성능 향상 보조 스트림

#### BufferedOutputStream과 BufferedWriter

#### BufferedInputStream과 BufferedReader

### 2.4. 기본 타입 입출력 보조 스트림

### 2.5. 프린터 보조 스트림

### 2.6. 객체 입출력 보조 스트림



## 3. 입출력 관련 API

### 3.1. System.in 필드

### 3.2. System.out 필드

### 3.3. Scanner 클래스

### 3.4. File 클래스

<br>

# References

- [혼자 공부하는 자바 / 신용권 / 한빛미디어 / 2019](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=)













































<br>