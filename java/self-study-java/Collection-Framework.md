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

<br>

### 1.1. List 컬렉션

- List 컬렉션은 배열과 비슷하게 객체를 인덱스로 관리함.
- 배열과의 차이점은 저장 용량(capacity)이 자동으로 증가하며, 객체를 저장할 때 자동 인덱스가 부여된다는 것임. 그리고 추가, 삭제, 검색을 위한 다양한 메소드들이 제공됨.
- List 컬렉션은 객체 자체를 저장하는 것이 아니라 객체의 번지를 참조함.
- null도 저장이 가능하며, 이 경우 해당 인덱스는 객체를 참조하지 않음.

- List 컬렉션에서 공통적으로 사용 가능한 List 인터페이스의 메소드
  - `boolean add(E e)` : 주어진 객체를 맨 끝에 추가함.
  - `void add(int index, E element)` : 주어진 인덱스에 객체를 추가함.
  - `E set(int index, E element)` : 주어진 인덱스에 저장된 객체를 주어진 객체로 바꿈.
  - `boolean contains(Object o)` : 주어진 객체가 저장되어 있는지 조사함.
  - `E get(int index)` : 주어진 인덱스에 저장된 객체를 리턴함.
  - `boolean isEmpty()` : 컬렉션이 비어 있는지 조사함.
  - `int size()` : 저장되어 있는 전체 객체 수를 리턴함.
  - `void clear()` : 저장된 모든 객체를 삭제함.
  - `E remove(int index)` : 주어진 인덱스에 저장된 객체를 삭제함.
  - `boolean remove(Object o)` : 주어진 객체를 삭제함.
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

<br>

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

<br>

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

<br>

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

<br>

### 1.2. Set 컬렉션

- List 컬렉션은 객체의 저장 순서를 유지하지만, Set 컬렉션은 저장 순서가 유지되지 않음. 또한 객체를 중복해서 저장할 수 없고, 하나의 null만 저장할 수 있음.
- Set 컬렉션은 수학의 집합과 비슷함. 순서와 상관없고 중복이 허용되지 않음.
- Set 컬렉션에서 공통적으로 사용 가능한 Set 인터페이스의 메소드.
  - `boolean add(E e)` : 주어진 객체를 저장함. 객체가 성공적으로 저장되면 true를 리턴하고 중복 객체면 false를 리턴함.
  - `boolean contains(Object o)` : 주어진 객체가 저장되어 있는지 조사함.
  - `boolean isEmpty()` : 컬렉션이 비어 있는지 조사함.
  - `Iterator<E> iterator()` : 저장된 객체를 한 번씩 가져오는 반복자를 리턴함.
  - `int size()` : 저장되어 있는 전체 객체 수를 리턴함.
  - `void clear()` : 저장된 모든 객체를 삭제함.
  - `boolean remove(Object o)` : 주어진 객체를 삭제함.

```java
// Set 컬렉션에 String 객체를 저장, 삭제

Set<String> set = ...;
set.add("홍길동");
set.add("김자바");
set.remove("홍길동");
```

- Set 컬렉션은 인덱스로 객체를 검색해서 가져오는 메소드가 없음.
- 전체 객체를 대상으로 한 번씩 반복해서 가져오는 반복자(Iterator)를 제공함.
- 반복자는 Iterator 인터페이스를 구현한 객체를 말하는데, iterator() 메소드를 호출하면 얻을 수 있음.
- `Iterator<String> iterator = set.iterator();`
- Iterator 인터페이스에 선언된 메소드
  - `boolean hasNext()` : 가져올 객체가 있으면 true를 리턴하고 없으면 false 리턴.
  - `E next()` : 컬렉션에서 하나의 객체를 가져옴.
  - `void remove()` : Set 컬렉션에서 객체를 제거함.

```java
// Set 컬렉션에서 String 객체들을 반복해서 하나씩 가져오는 코드

Set<String> set = ...;
Iterator<String> iterator = set.iterator();
while(iterator.hasNext()){
    //String 객체 하나를 가져옴
    String str = iterator.next();
}
```

- 향상된 for문으로 전체 객체를 대상으로 반복할 수 있음.

```java
for(String str : set){
}
```

- Iterator의 remove() 메소드는 Iterator의 메소드이지만, 실제 Set 컬렉션에서 객체가 제거됨.

<br>

#### HashSet

- HashSet은 Set 인터페이스의 구현 클래스임.
- HashSet을 생성하기 위해서는 기본 생성자를 호출하면 됨. 
- `Set<E> set = new HashSet<E>(); `
- HashSet은 객체들을 순서 없이 저장하고 동일한 객체는 중복 저장하지 않음.
- HashSet이 판단하는 동일한 객체는 1) hashCode() 리턴값이 같음 -> 2) equals() 리턴값이 같음 을 의미함.
- String 클래스는 같은 문자열일 경우 hashCode()의 리턴값은 같게, equals()의 리턴값은 true가 나오도록 hashCode()와 equals() 메소드를 재정의함.

```java
// HashSet 예제

import java.util.*;

public class HashSetExample {

	public static void main(String[] args) {
		Set<String> set = new HashSet<>();

		set.add("자바");
		set.add("JDBC");
		set.add("Servlet/JSP");
		set.add("Java");
		set.add("iBATIS");

		int size = set.size();
		System.out.println("총 객체수 : " + size);

		Iterator<String> iterator = set.iterator();
		while (iterator.hasNext()) {
			String element = iterator.next();
			System.out.println("\t" + element);
		}

		set.remove("JDBC");
		set.remove("iBATIS");

		System.out.println("총 객체수 : " + set.size());

		iterator = set.iterator();
		for (String element : set) {
			System.out.println("\t" + element);
		}

		set.clear();
		if (set.isEmpty()) {
			System.out.println("비어 있음");
		}
	}
}
```

```java
// HashSet 예제2
// Member.java
public class Member {
	public String name;
	public int age;
	
	public Member(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public boolean equals(Object obj) {
		if(obj instanceof Member) {
			Member member = (Member) obj;
			return member.name.equals(name) && (member.age==age) ;
		} else {
			return false;
		}
	}

	public int hashCode() {
		return name.hashCode() + age;
	}
}

// HashSetExample2
import java.util.*;

public class HashSetExample {
	public static void main(String[] args) {
		Set<Member> set = new HashSet<Member>();
		
		set.add(new Member("홍길동", 30));
		set.add(new Member("홍길동", 30));
		
		System.out.println("총 객체수 : " + set.size());
	}
}
```

<br>

### 1.3. Map 컬렉션

- Map 컬렉션은 키(key)와 값(value)으로 구성된 Map.Entry 객체를 저장하는 구조를 가지고 있음.
- Entry는 Map 인터페이스 내부에 선언된 중첩 인터페이스임.
- 키와 값은 모두 객체임.
- 키는 중복 저장될 수 없지만 값은 중복 저장될 수 있음.
- 기존에 저장된 키와 동일한 키로 값을 저장하면 기존의 값은 없어지고 새로운 값으로 대체됨.
- Map 컬렉션에서 공통적으로 사용 가능한 Map 인터페이스의 메소드
  - `V put(K key, V value)` : 주어진 키로 값을 저장함. 새로운 키일 경우 null을 리턴하고 동일한 키가 있을 경우 값을 대체하고 이전 값을 리턴함.
  - `boolean containsKey(Object key)` : 주어진 키가 있는지 여부를 확인.
  - `boolean containsValue(Object value)` : 주어진 값이 있는지 여부를 확인.
  - `Set<Map.Entry<K,V>> entrySet()` : 키와 값의 쌍으로 구성된 모든 Map.Entry 객체를 Set에 담아서 리턴함.
  - `V get(Object key)` : 주어진 키가 있는 값을 리턴함.
  - `boolean isEmpty()` : 컬렉션이 비어 있는지 여부를 확인함.
  - `Set<K> keySet()` : 모든 키를 Set 객체에 담아서 리턴함.
  - `int size()` : 저장된 키의 총 수를 리턴함.
  - `Collection<V> values()` : 저장된 모든 값을 Collection에 담아서 리턴함.
  - `void clear()` : 모든 Map.Entry(키와 값)를 삭제함.
  - V remove(Object key) : 주어진 키와 일치하는 Map.Entry를 삭제하고 값을 리턴함.
- 메소드의 매개 변수 타입과 리턴 타입에 K와 V라는 타입 파라미터. 저장되는 키와 객체의 타입을 Map 컬렉션을 생성할 때 결정하라는 뜻.
- 저장된 전체 객체를 대상으로 하나씩 얻고 싶을 경우 두 가지 방법
  - keySet() 메소드로 모든 키를 Set  컬렉션으로 얻은 다음, 반복자를 통해 키를 하나씩 얻고 get() 메소드를 통해 값을 얻는 방법.
  - entrySet() 메소드로 모든 Map.Entry를 Set 컬렉션으로 얻은 다음, 반복자를 통해 Map.Entry를 하나씩 얻고 getKey()와 getValue() 메소드를 이용해 키와 값을 얻는 방법.

<br>

#### HashMap

- HashMap은 Map 인터페이스를 구현한 대표적인 Map 컬렉션.
- HashMap의 키로 사용할 객체는 hashCode()와 equals() 메소드를 재정의해서 동등 객체가 될 조건을 정해야 함.
- 객체가 달라도 동등 객체라면 같은 키로 간주하고 중복 저장되지 않도록 하기 위함임.
- 동등 객체의 조건은 hashCode()의 리턴값이 같아야 하고, equals() 메소드가 true를 리턴해야 함.
- 주로 키 타입은 String을 많이 사용하는데, String은 문자열이 같을 경우 동등 객체가 될 수 있도록 hashCode()와 equals() 메소드가 재정의되어 잇음.
- 키와 값의 타입은 기본 타입을 사용할 수 없고 클래스 및 인터페이스 타입만 사용 가능함.

```java
// HashMap 예제
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class HashMapExample {
	public static void main(String[] args) {
		//Map 컬렉션 생성
		Map<String, Integer> map = new HashMap<String, Integer>();
		
		//객체 저장
		map.put("신용권", 85);
		map.put("홍길동", 90);
		map.put("동장군", 80);
		map.put("홍길동", 95);
		System.out.println("총 Entry 수: " + map.size());
		
		//객체 찾기		
		System.out.println("\t홍길동 : " + map.get("홍길동"));
		System.out.println();
		
		//객체를 하나씩 처리
		Set<String> keySet = map.keySet();
		Iterator<String> keyIterator = keySet.iterator();
		while(keyIterator.hasNext()) {
		  String key = keyIterator.next();
		  Integer value = map.get(key);
		  System.out.println("\t" + key + " : " + value);
		}		
		System.out.println();	
		
		//객체 삭제
		map.remove("홍길동");
		System.out.println("총 Entry 수: " + map.size());
		
		//객체를 하나씩 처리
		Set<Map.Entry<String, Integer>> entrySet = map.entrySet();
		Iterator<Map.Entry<String, Integer>> entryIterator = entrySet.iterator();
		while(entryIterator.hasNext()) {
		  Map.Entry<String, Integer> entry = entryIterator.next();
		  String key = entry.getKey();
		  Integer value = entry.getValue();
		  System.out.println("\t" + key + " : " + value);
		}
		System.out.println();
		
		//객체 전체 삭제
		map.clear();
		System.out.println("총 Entry 수: " + map.size());
	}
}
```

```java
// HashMap 예제2
// Student.java
public class Student {
	public int sno;
	public String name;
	
	public Student(int sno, String name) {
		this.sno = sno;
		this.name = name;
	}

	public boolean equals(Object obj) {
		if(obj instanceof Student) {
			Student student = (Student) obj;
			return (sno==student.sno) && (name.equals(student.name)) ;
		} else {
			return false;
		}
	}

	public int hashCode() {
		return sno + name.hashCode();
	}
}

// HashMapExample.java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class HashMapExample {
	public static void main(String[] args) {
		Map<Student, Integer> map = new HashMap<Student, Integer>();
		
		map.put(new Student(1, "홍길동"), 95);
		map.put(new Student(1, "홍길동"), 95);
		
		System.out.println("총 Entry 수: " + map.size());
	}
}
```

<br>

#### Hashtable

- Hashtable은 HashMap과 동일한 내부 구조를 가지고 있음. Hashtable도 키로 사용할 객체는 hashCode()와 equals() 메소드를 재정의해서 동등 객체가 될 조건을 정해야 함.
- HashMap과의 차이점은 Hashtable은 동기화된(synchronized) 메소드로 구성되어 있기 때문에 멀티 스레드가 동시에 Hashtable의 메소드들을 실행할 수 없고, 하나의 스레드가 실행을 완료해야만 다른 스레드를 실행할 수 있다는 것임.
- 멀티 스레드 환경에서 안전하게 객체를 추가, 삭제할 수 있기 때문에 Hashtable은 스레드에 안전(thread safe)함.

```java
// Hashtable 예시
import java.util.*;

public class HashtableExample {
	public static void main(String[] args) {
		Map<String, String> map = new Hashtable<>();

		map.put("spring", "12");
		map.put("summer", "123");
		map.put("fall", "1234");
		map.put("winter", "12345");
		
		Scanner scanner = new Scanner(System.in);
		
		while(true) {
			System.out.println("아이디와 비밀번호를 입력해주세요");
			System.out.print("아이디: ");
			String id = scanner.nextLine();
			
			System.out.print("비밀번호: ");
			String password = scanner.nextLine();
			System.out.println();
			
			if(map.containsKey(id)) {
				if(map.get(id).equals(password)) {
					System.out.println("로그인 되었습니다");
					break;
				} else {
					System.out.println("비밀번호가 일치하지 않습니다.");
				}				
			} else {
				System.out.println("입력하신 아이디가 존재하지 않습니다");
			}
		}
	}
}
```

<br>

## 2. LIFO와 FIFO 컬렉션

- 컬렉션 프레임워크에는 LIFO(후입선출) 자료구조를 제공하는 Stack 클래스와 FIFO(선입선출) 자료구조를 제공하는 Queue 인터페이스가 있음.
- 후입선출(LIFO: Last In First Out)은 나중에 넣은 객체가 먼저 빠져나가는 자료구조를 말함.
- 선입선출(FIFO: First In First Out)은 먼저 넣은 객체가 먼저 빠져나가는 자료구조를 말함.

<br>

### 2.1. Stack

- Stack 클래스는 LIFO 자료구조를 구현한 클래스임.
- 주요 메소드
  - `E push(E item)` : 주어진 객체를 스택에 넣음.
  - `E peek()` : 스택의 맨 위 객체를 가져옴. 객체를 스택에서 제거하지 않음.
  - `E pop()` : 스택의 맨 위 객체를 가져옴. 객체를 스택에서 제거함.
- Stack 객체 생성
  - `Stack<E> stack = new Stack<>();`
  - `Stack<E> stack = new Stack<E>();`

```java
// Stack 예제 : 동전 케이스
// Coin.java
public class Coin {
	private int value;
	
	public Coin(int value) {
		this.value = value;
	}
	
	public int getValue() {
		return value;
	}
}

// StackExample.java
import java.util.Stack;

public class StackExample {
	public static void main(String[] args) {
		Stack<Coin> coinBox = new Stack<Coin>();
		
		coinBox.push(new Coin(100));
		coinBox.push(new Coin(50));
		coinBox.push(new Coin(500));
		coinBox.push(new Coin(10));
		
		while(!coinBox.isEmpty()) {
			Coin coin = coinBox.pop();
			System.out.println("꺼내온 동전 : " + coin.getValue() + "원");
		}
	}
}
```

<br>

### 2.2. Queue

- Queue 인터페이스는 FIFO 자료구조에서 사용되는 메소드를 정의하고 있음.
- Queue 인터페이스에 정의되어 있는 메소드
  - `boolean offer(E e)` : 주어진 객체를 넣음.
  - `E peek()` : 객체 하나 가져옴. 객체를 큐에서 제거하지 않음.
  - `E poll()` : 객체 하나 가져옴. 객체를 큐에서 제거함.
- Queue 인터페이스를 구현한 대표적인 클래스는 LinkedList임.
- LinkedList는 List 인터페이스를 구현했기 때문에 List 컬렉션이기도 함.
- `Queue<E> queue = new LinkedList<>();`

```java
// Queue 예제 : 간단한 메시지 처리
// Message.java
public class Message {
	public String command;
	public String to;
	
	public Message(String command, String to) {
		this.command = command;
		this.to = to;
	}
}

// QueueExample.java
import java.util.LinkedList;
import java.util.Queue;

public class QueueExample {
	public static void main(String[] args) {
		Queue<Message> messageQueue = new LinkedList<Message>();
		
		messageQueue.offer(new Message("sendMail", "홍길동"));
		messageQueue.offer(new Message("sendSMS", "신용권"));
		messageQueue.offer(new Message("sendKakaotalk", "홍두께"));
		
		while(!messageQueue.isEmpty()) {
			Message message = messageQueue.poll();
			switch(message.command) {
				case "sendMail":
					System.out.println(message.to + "님에게 메일을 보냅니다.");
					break;
				case "sendSMS":
					System.out.println(message.to + "님에게 SMS를 보냅니다.");
					break;
				case "sendKakaotalk": 
					System.out.println(message.to + "님에게 카카오톡를 보냅니다.");
					break;
			}
		}
	}
}
```

<br>

# References

- [혼자 공부하는 자바 / 신용권 / 한빛미디어 / 2019](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=)

<br>