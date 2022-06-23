# 컬렉션 프레임워크

<br>

> 이 문서는 [<혼자 공부하는 자바 - 신용권>](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=) 책을 보고 정리한 문서입니다.

<br>

## 1. 컬렉션 프레임워크

- 자바는 널리 알려져 있는 자로구조(Date Structure)를 사용해서 객체들을 효율적으로 추가, 삭제, 검색할 수 있도록 인터페이스와 구현 클래스를 java.util 패키지에 제공함. 이들을 총칭해서 컬렉션 프레임워크(Collection Framework)라고 함.
- 컬렉션은 객체의 저장을 뜻함.
- 프레임워크는 사용 방법을 정해놓은 라이브러리를 뜻함.
- 실제로 컬렉션 프레임워크는 사용 방법을 정의한 인터페이스와 실제 객체를 저장하는 다양한 컬렉션 클래스(구현 클래스)를 제공함.
- 컬렉션 프레임워크의 주요 인터페이스로는 List, Set, Map이 있음.



### 1.1. List 컬렉션

- List 컬렉션은 배열과 비슷하게 객체를 인덱스로 관리함.
- 배열과의 차이점은 저장 용량(capacity)이 자동으로 증가하며, 객체를 저장할 때 자동 인덱스가 부여된다는 것임. 그리고 추가, 삭제, 검색을 위한 다양한 메소드들이 제공됨.
- List 컬렉션은 객체 자체를 저장하는 것이 아니라 객체의 번지를 참조함.
- null도 저장이 가능하며, 이 경우 해당 인덱스는 객체를 참조하지 않음.

- List 컬렉션에서 공통적으로 사용 가능한 List 인터페이스의 메소드
  - boolean add(E e) : 주어진 객체를 맨 끝에 추가함.
  - void add(int index, E element) : 주어진 인덱스에 객체를 추가함.
  - E set(int index, E element) : 주어진 인덱스에 저장된 객체를 주어진 객체로 바꿈.
  - boolean contains(Object o) : 주어진 객체가 저장되어 있는지 조사함.
  - E get(int index) : 주어진 인덱스에 저장된 객체를 리턴함.
  - boolean isEmpty() : 컬렉션이 비어 있는지 조사함.
  - int size() : 저장되어 있는 전체 객체 수를 리턴함.
  - void clear() : 저장된 모든 객체를 삭제함.
  - E remove(int index) : 주어진 인덱스에 저장된 객체를 삭제함.
  - boolean remove(Object o) : 주어진 객체를 삭제함.
- E라는 타입 파라미터는 저장되는 객체의 타입을 List 컬렉션을 생성할 때 결정하라는 뜻.

```java
// 리스트 컬렉션에 String 객체를 추가, 삽입, 검색, 삭제

List<String> list = ...;
list.add("홍길동");
list.add(1, "김자바");
String str = list.get(1);
list.remove(0);
list.remove("김자바");
```

- List\<String>으로 list 변수를 선언함. List 컬렉션에 저장되는 객체를 String 타입으로 하겠다는 뜻. 따라서 E 타입 파라미터는 String 타입이 됨.
- List 컬렉션에 저장된 모든 객체를 대상으로 하나씩 가져와 처리하고 싶다면 인덱스를 이용하는 방법과 향상된 for문을 이용하는 방법이 있음.

```java
// for문 인덱스를 이용하는 방법

List<String> list = ...;
for(int i=0; i<list.size(); i++){
    String str = list.get(i);
}
```

```java
// 향상된 for문 이용하는 방법

List<String> list = ...;
for(String str : list){
}
```



#### ArrayList

- ArrayList는 List 인터페이스의 대표적인 구현 클래스임.
- ArrayList 생성
  - `List<E> list = new ArrayList<E>();`
  - 저장할 객체 타입을 E 타입 파라미터 자리에 표기하고 기본 생성자를 호출.
  - ArrayList의 E 타입 파라미터를 생략하면 왼쪽 List에 지정된 타입을 따라감.
  - 기본 생성자로 ArrayList 객체를 생성하면 내부에 10개의 객체를 저장할 수 있는 초기 용량을 가지게 됨. 저장되는 객체 수가 늘어나면 용량이 자동으로 증가함.
- ArrayList 객체 추가
  - ArrayList에 객체를 추가하면 0번 인덱스부터 차례대로 저장됨.
  - ArrayList에서 특정 인덱스의 객체를 제거하면 바로 뒤 인덱스부터 마지막 인덱스까지 모두 앞으로 1씩 당겨짐.
  - 특정 인덱스에 객체를 삽입하면 해당 인덱스부터 마지막 인덱스까지 모두 1씩 밀려남.
  - 저장된 객체 수가 많고, 특정 인덱스에 객체를 추가하거나 제거하는 일이 빈번하다면 LinkedList를 사용하는 것이 좋음.
  - 인덱스를 이용해서 객체를 찾거나 맨 마지막에 객체를 추가하는 경우에는 ArrayList가 더 좋은 성능을 발휘함.

```java
// arrayList에 String 객체를 추가, 검색, 삭제

import java.util.*;

public class ArrayListExample {
	public static void main(String[] args) {
		List<String> list = new ArrayList<String>();
		
		list.add("Java");
		list.add("JDBC");
		list.add("Servlet/JSP");
		list.add(2, "Database");
		list.add("iBATIS");

		int size = list.size();
		System.out.println("총 객체수: " + size);		
		System.out.println();
		
		String skill = list.get(2);
		System.out.println("2: " + skill);
		System.out.println();

		for(int i=0; i<list.size(); i++) {
			String str = list.get(i);
			System.out.println(i + ":" + str);
		}
		System.out.println();
		
		list.remove(2);
		list.remove(2);
		list.remove("iBATIS");		
		
		for(int i=0; i<list.size(); i++) {
			String str = list.get(i);
			System.out.println(i + ":" + str);
		}
	}
}
```



#### Vector

- Vector는 ArrayList와 동일한 내부 구조를 가지고 있음.
- Vector를 생성하기 위해서는 저장할 객체 타입을 타입 파라미터로 표기하고 기본 생성자를 호출하면 됨.
- `List<E> list = new Vector<E>();`
- Vector는 동기화된 메소드로 구성되어 있기 때문에 멀티 스레드가 동시에 Vector의 메소드들을 실행할 수 없고, 하나의 스레드가 메소드 실행을 완료해야만 다른 스레드가 메소드를 실행할 수 있음.
- 멀티 스레드 환경에서 안전하게 객체를 추가, 삭제할 수 있음. 이것을 스레드에 안전(thread safe)하다고 표현함.

```java
// Vector를 이용해서 Board 객체를 추가, 삭제, 검색
// VectorExample.java
import java.util.*;

public class VectorExample {
	public static void main(String[] args) {
		List<Board> list = new Vector<Board>();
	
		list.add(new Board("제목1", "내용1", "글쓴이1"));
		list.add(new Board("제목2", "내용2", "글쓴이2"));
		list.add(new Board("제목3", "내용3", "글쓴이3"));
		list.add(new Board("제목4", "내용4", "글쓴이4"));
		list.add(new Board("제목5", "내용5", "글쓴이5"));
		
		list.remove(2);
		list.remove(3);
		
		for(int i=0; i<list.size(); i++) {
			Board board = list.get(i);
			System.out.println(board.subject + "\t" + board.content + "\t" + board.writer);
		}
	}
}

// Board.java
public class Board {
	String subject;
	String content;
	String writer;
	public Board(String subject, String content, String writer) {
		this.subject = subject;
		this.content = content;
		this.writer = writer;
	}
}
```



#### LinkedList

- LinkedList는 List 구현 클래스이므로 ArrayList와 사용 방법은 똑같은데 내부 구조는 완전 다름.
- ArrayList는 내부 배열에 객체를 저장해서 관리하지만, LinkedList는 인접 참조를 링크해서 체인처럼 관리함.
- LinkedList에서 특정 인덱스의 객체를 제거하면 앞뒤 링크만 변경되고 나머지 링크는 변경되지 않음. 특정 인덱스에 객체를 삽입할 때에도 마찬가지임.
- 삭제되는 객체의 앞뒤 객체에서 삭제되는 객체와의 링크를 끊고 자기들끼리 새로 연결함.
- ArrayList는 중간 인덱스의 객체를 제거하면 뒤에 있는 객체의 인덱스가 1씩 앞으로 당겨지기 때문에 빈번한 객체 삭제와 삽입이 일어나는 곳에서는 ArrayList보다 LinkedList가 좋은 성능을 발휘함.
- LinkedList를 생성하기 위해서는 저장할 객체 타입을 타입 파라미터(E)에 표기하고 기본 생성자를 호출하면 됨.

```java
// ArrayList와 LinkedList의 실행 성능 비교

import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class LinkedListExample {
	public static void main(String[] args) {
		List<String> list1 = new ArrayList<String>();
		List<String> list2 = new LinkedList<String>();
		
		long startTime;
		long endTime;
		
		startTime = System.nanoTime();
		for(int i=0; i<10000; i++) {
			list1.add(0, String.valueOf(i));
		}
		endTime = System.nanoTime();
		System.out.println("ArrayList 걸린시간: " + (endTime-startTime) + " ns");
		
		startTime = System.nanoTime();
		for(int i=0; i<10000; i++) {
			list2.add(0, String.valueOf(i));
		}
		endTime = System.nanoTime();
		System.out.println("LinkedList 걸린시간: " + (endTime-startTime) + " ns");
	}
}
```

- 끝에서부터 순차적으로 추가 또는 삭제하는 경우는 ArrayList가 빠르지만, 중간에 추가, 삭제하는 경우는 앞뒤 링크 정보만 변경하면 되는 LinkedList가 더 빠름. ArrayList는 뒤쪽 인덱스들을 모두 1씩 증가 또는 감소시키는 시간이 필요하므로 처리 속도가 느림.



### 1.2. Set 컬렉션

#### HashSet

### 1.3. Map 컬렉션

#### HashMap

#### Hashtable



## 2. LIFO와 FIFO 컬렉션

### 2.1. Stack

### 2.2. Queue





# References

- [혼자 공부하는 자바 / 신용권 / 한빛미디어 / 2019](