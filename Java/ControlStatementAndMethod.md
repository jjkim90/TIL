# 제어문과 메서드



## 1. 제어문

- 프로그래밍 언어는 제어문을 사용해 실행문을 비순차적으로 수행할 수 있게 함.
- 제어문은 실행문의 수행 순서를 변경하는 것을 조건문, 반복문, 분기문이 있음.
- 조건문
  - if문
  - switch문
- 반복문
  - for문
  - while문
  - do~while문
- 분기문
  - break문
  - continue문
- 



## 2. 조건문

### 단순 if 문

- 조건식이 true일 때만 실행문을 수행함.
- 조건식에는 true 또는 false를 산출할 수 있는 연산식이나 논릿값, 변수가 올 수 있음.
- 조건식이 true일 때 수행할  실행문이 하나라면 {}를 생략할 수 있음.
- 실무에서는 혼란을 방지하기 위해 되도록 중괄호 {} 사용을 추천함.



### if~else 문

- if 문은 else 블록과 함께 사용되어 조건식의 결과에 따라 실행 블록을 선택함.
  - if문의 조건이 true면, if문의 블록이 실행됨.
  - if문의 조건이 false이면, else 블록이 실행됨.
- if (조건식) { true일 때 실행문; } else { false일 때 실행문; }



### 다중 if 문

- 조건문이 여러 개인 if문을 말함.
- 학점 계산, 회원 등급 처리 등에 사용됨.
- if (조건식1) { 조건식1이 true일 때 실행문; } else if (조건식2) { 조건식2가 true일 때 실행문; } else { 모든 조건 false일 때 실행문; }



### 중첩 if 문

- if 문의 블록 내부에 또다른 조건문, 비교문 등이 오는 기법
  - if문 내부에 for문, while문을 사용할 수 있음.
  - if문 내부에 또다른 if문 을 사용할 수 있음.



### 조건식 예제

- 90 <= x && x <= 100 (x가 90 이상이고 100 이하) -> A학점
- x < 0 || x > 100 (x가 0 미만이거나 100 초과) -> 성적 입력 검증
- x % 3 == 0 && x % 2 != 0 (x가 3의 배수이고 2의 배수 아님)
- ch == 'y' || ch == 'Y' (ch가 소문자 y이거나 대문자 Y ) -> 약관 동의
- ch == ' ' || ch == '\t' || ch == '\n' (ch가 공백이거나 탭이거나 개행문자)
- 'A' <= ch && ch >= 'Z' (ch값이 A보다 작고 Z보다 큼) -> 대문자 검증
- 'a' <= ch && ch >= 'z' (ch값이 a보다 작고 z보다 큼) -> 소문자 검증
- '0' <= ch && ch >= '9' (ch값이 0보다 작고 9보다 큼) -> 숫자 검증
- str == "yes" -> (str의 주소값이 yes) -> 오류 발생함, equals 안쓰면 주소 참조.
- str.equals("yes") -> (str의 값이 yes) -> str 문자열의 내용이 yes인지 검증. 대소문자 구분함.
- str.equals("yes") || str.equals("Yes") || str.equals("YES") -> str 문자열의 내용이 yes인지 검증. 대소문자 구분 안함.
- str.equalsIgnoreCase("yes") -> str 문자열의 내용이 yes인지 검증. 대소문자 구분 안함.





## 3. 반복문

- 조건에 따라 같은 처리를 반복함.
- while 문, do~while 문, for 문이 있음.
- while 문, do~while 문은 반복 횟수를 모르지만 조건을 알 때 사용함. (로그인 기법)
- for 문은 반복 횟수를 미리 알고 있을 때 사용함. (구구단 프로그램)



### for 문

- 반복할 횟수를 미리 알 수 있을 때 주로 사용함.
- 조건식이 true이면 본체 실행문을 반복적으로 수행.
- 코드를 간결하게 만들어 개발 시간과 오류 확률을 줄일 수 있음.
- for (초기식; 조건식; 증감식) { 조건식이 true일 때 실행문;}
  - 초기식 : for 문을 시작할 때 한 번만 실행. for 문 앞에 초기화된 변수가 있을 경우 생략 가능함.
  - 조건식 : 최댓값까지의 데이터라고 생각하면 됨.
  - 증감식 : ++, --를 이용하여 조건식 증감을 진행함.

```java
public static void main(String[] args) {
		for (int i = 2; i <= 10; i += 2) {
			System.out.println("1~10까지 짝수 출력 : " + i);
		}
	}
```



#### 둘 이상의 초기식, 조건식, 증감식

- 초기식, 조건식, 증감식이 둘 이상인 경우가 있음.
- 초기식과 증감식에는 콤마(,)를 쓰고, 조건식에는 &&로 구분하여 작성해야 함.

```java
for (int i = 0, j = 100; i <= 10 && j >= 90; i++, j--) {
System.out.println("i값의 증가 : " + i + " j값의 감소 : " + j);
}
```



#### for 문으로 1~100 합계 구하기

- 변수는 2개가 필요함.
  - sum은 합을 누적하는 변수로 활용됨.
  - i는 1부터 100까지 증가되는 변수로 활용됨.

```java
public static void main(String[] args) {
    int sum = 0;
    for (int i = 1; i <= 100; i++) {
        sum = sum + i;
    }
    System.out.println("1~100까지의 합 : " + sum);
}
```



#### for 문으로 1~100 중 3의 배수 합계 구하기

```java
//	for문을 이용하여 1부터 100까지의 정수 중에서 3의 배수의 총합을 구하는 코드
	public static void main(String[] args) {
		int sum = 0;
		for (int i = 1; i <= 100; i++) {
			if (i % 3 == 0) {
				sum += i;
			}
		}
		System.out.println("3의 배수의 합 : " + sum);

	}
```



#### for 문으로 1~N 합계 구하기

- for 문을 이용하여 최댓값을 키보드로 입력하고 합계를 출력하는 프로그램
  - Scanner를 이용하여 값을 입력 받음.
  - 입력값을 최댓값으로 활용.
- 주의사항
  - 변수의 사용 범위를 파악할 것.
  - Scanner의 입력 데이터 타입을 확인할 것.
  - 출력시 최댓값의 멘트가 적용될 것.

```java
	public static void main(String[] args) {
		int sum = 0;
		int i;
		Scanner in = new Scanner(System.in);
		System.out.print("1부터 입력한 값까지 합계가 출력됩니다. 입력값 : ");
		int userNumber = in.nextInt();
		
		for (i=1 ; i <= userNumber; i++) {
			sum = sum + i;
		}
		System.out.println("1~" + (i-1) + " 까지의 합 : " + sum);
	}
```



#### for 문 초기식으로 실수타입 사용하기

- 초기화식에서 루프 카운트 변수를 부동 소수점인 실수 방식으로 사용하면 원하는 결과와 다른 결과가 나올 수 있음.

```java
public class ForFloatCounterExam {

	public static void main(String[] args) {
		for (float x = 0.1f; x < 1.0f; x += 0.1f) {
			System.out.println(x);
		}
	}
```

- 위의 코드를 실행해보면 루프가 9번만 반복되는 오류를 확인할 수 있음.



#### for 문으로 구구단 프로그램 만들기

```java
	public static void main(String[] args) {

		for (int m = 2; m <= 9; m++) {
			System.out.println("***** " + m + "단 *******");
			for (int n = 1; n <= 9; n++) {
				System.out.println(m + " X " + n + " = " + (m * n));
			}
		}
	}
```

- 변수 m은 2부터 9까지 증가하여 단을 생성 (바깥쪽 for 문)
- 변수 n은 1부터 9까지 증가하여 곱셈을 완성 (안쪽 for 문)



### while 문

- for 문이 정해진 횟수만큼 반복한다면, while 문은 조건식이 true일 경우 계속해서 반복함.
- 조건식에는 주로 비교 또는 논리 연산식이 옴.
- 조건식이 false이면 반복 행위를 멈추고 while 문을 종료함.
- while 문은 처음 실행될 때 조건식을 항상 평가함. 조건식은 반드시 존재해야 함.
- 작성법
  - while (조건식) { 조건식이 true일 때 실행문; }
  - 조건식이 false면 본체를 한 번도 실행하지 않고 빠져나옴.

```java
	public static void main(String[] args) {
		int i = 1;
		while (i <= 10) {
			System.out.println(i);
			i++; // 증감식을 넣지 않으면 무한루프에 빠짐.
		}
	}
```



#### while 문으로 1~100 합계 구하기

```java
int i = 1;
		int sum = 0;
		while (i <= 100) {
			sum = sum + i;
			i++;
		}
		System.out.println("1~100의 합 : " + sum);
```



#### While 문으로 자동차 크루즈 프로그램 만들기

- 숫자 1을 입력하면 속도를 증가시키고 숫자 2를 입력하면 속도를 감소시키고 숫자 3을 누르면 종료하는 프로그램 만들기.
- 사용자가 입력한 키보드의 버튼에 따라 프로그램을 처리하려면 키코드를 활용하면 됨.
- 키보드로 입력 받는 키코드 리턴
  - int keyCode = System.in.read();
  - 유니코드 값을 참조 숫자1(49) 숫자2(50) 숫자3(51) 엔터(10, 13)
  - throws Exception : 예외 처리

```java
	public static void main(String[] args) throws Exception{
		boolean run = true; // 실행 여부를 판단
		int speed = 100; // 현재 속도 100km/h
		int keyCode = 0; // 키보드로 입력되는 값

		while (run) { // run 초기값이 true이기 때문에 주행중이라는 뜻.
			if (keyCode != 13 && keyCode != 10) { // 엔터가 아니면
				System.out.println("-----------");
				System.out.println("1. 엑셀 | 2. 브레이크 | 3. 중지 ");
				System.out.print("선택 : ");
			}
			keyCode = System.in.read();
//			System.out.println(keyCode);
			
			if (keyCode == 49) {
				speed += 5;
				System.out.println("현재 속도 : " + speed);
			} else if (keyCode == 50) {
				speed -= 5;
				System.out.println("현재 속도 : " + speed);
			} else if (keyCode == 51) {
				run = false;
				System.out.println("크루즈 기능이 종료됩니다.");
				System.out.println("현재 속도 : " + speed);
				System.out.println("안전 운전하세요.");
			}
		}
		System.out.println("프로그램 종료");

	}
```



### do~while 문

- while문과 비슷하지만 조건식 평가와 본체 실행 순서가 다름.
- 최초 1번 반복문을 실행한 후 조건식을 평가함.
- 조건식이 거짓이라도 한 번은 본체를 실행함.
- 예를 들어, 키보드로 입력 받은 내용을 조사하여 계속 루프를 돌게할 것인지 판단하는 프로그램에서, 조건식은 키보드로 입력 받은 이후에 평가되어야 하므로, 최초에는 키보드로부터 입력된 내용을 받아야 함.
- do {실행문;} while (조건식);
  - 조건식이 true이면 위로 올라가 do 실행문을 진행하고 false이면 while 문을 빠져나감.



do~while 문으로 채팅 프로그램 만들기

```java
	public static void main(String[] args) {
		System.out.println("메시지를 입력하세요.");
		System.out.println("프로그램을 종료하려면 q를 입력하세요 : ");

		Scanner in = new Scanner(System.in);
		String inputString; // 키보드로 입력 받은 값을 저장.

		do {
			System.out.print(">>>");
			inputString = in.nextLine(); // String 값을 입력 받음.
			System.out.println("전송값 : " + inputString);
		} while (!inputString.equals("q"));
		System.out.println("----------");
		System.out.println("프로그램 종료");
		System.out.println("----------");
	}
```





## 4. 분기문



### break 문

- for, while, do~while 문 실행 중지할 때 사용함.
- switch 문의 본체를 벗어날 때 사용함.
- 반복문에서 break문을 사용하면 루프 도중 중단되는 현상을 볼 수 있음.



#### break 문의 레이블(label)

- break 문은 레이블과 함께 사용할 수 있음.
- 레이블이 없으면 break 문을 포함하는 맨 안쪽 반복문 종료
- 레이블이 있다면 레이블로 표시된 반복문을 종료함.
- break문이 종료되는 시점을 선택할 수 있음.

```java
	public static void main(String[] args) {
		int i = 0;
		Outter: while (true) {
			int num = (int) (Math.random() * 45) + 1;
			System.out.println("오늘의 로또 번호 : " + num);
			i++;
			if (i == 6) {
				System.out.println("프로그램 종료");
				break Outter;
			}

		}
	}
```

```java
public class BreakOutterExam {

	public static void main(String[] args) {
		Outter: for (char upper = 'A'; upper <= 'Z'; upper++) { // A~Z 바깥쪽 반복
			for (char lower = 'a'; lower <= 'z'; lower++) { // a~z 안쪽 반복
				System.out.println(upper + "-" + lower);
				if (lower == 'c') { // 소문자 c 나오면 break 문 실행
					break Outter; // 레이블을 바깥에 걸면 A-c까지 실행되고, 레이블을 안쪽에 걸면 Z-c까지 실행됨.
				}
			}
		}
	}
```



### continue 문

- continue 문은 반복문에서만 사용됨.
- 현재 반복은 건너뛴 채 나머지 반복만 계속 실행함.
- 반복문 블록 내부에서 continue 문이 실행되면 for 문의 증감식 또는 while 문의 조건식으로 이동함.
- continue 문은 반복문을 종료하지 않고, 계속 반복을 수행한다는 점에서 break 문과 다름.
- 예시) 홀수일 때 건너뛰는 for 문

```java
	public static void main(String[] args) {
		for (int i = 1; i <= 10; i++) {
			if(i%2 != 0) {
				continue;
			}
			System.out.println(i);
		}
	}
```





## switch 문

### 개념

- if 문과 마찬가지로 조건 제어문임.
- if 문처럼 조건식이 true일 때 블록 내부의 실행문을 실행하는 것이 아니라, 변수가 어떤 값을 갖느냐에 따라 실행문이 선택됨.
- if 문은 결과가 true나 false 두 가지 형태일 때 사용하고, switch 문은 결과가 여러 개일 때 많이 활용됨.



### switch 문의 특징

- switch 문은 괄호 안의 값과 동일한 값을 갖는 case로 가서 실행문을 실행시킴.
- 만약 괄호 안의 값과 동일한 값이 없으면 default로 가서 실행문을 실행시킴.
- default는 생략이 가능함.
- 실행문을 실행하고 break 문을 만나면 switch문을 빠져나옴.
- break문이 없을 경우 다음 case문이 연달아 실행됨. (오류 발생)



### break 문을 생략한 switch 문

- 특정 시점부터 시작하여 끝까지 누적적으로 실행을 원할 경우 의도적으로 break 문 전체를 생략할 수 있음.

- ```java
  int time = (int) (Math.random() * 6) + 6;
  		System.out.println("[현재 시간은 : " + time + "시]");
  
  		switch (time) {
  		case 6 :
  			System.out.println("조깅 후 세안을 하고 아침을 먹습니다.");
  		case 7 :
  			System.out.println("출근 준비를 합니다.");
  		case 8 :
  			System.out.println("버스를 타고 출근을 합니다.");
  		case 9 :
  			System.out.println("수업 준비를 합니다.");
  		case 10 :
  			System.out.println("결석자에게 전화를 합니다.");
  		default :
  			System.out.println("자바를 열심히 가르칩니다.");
  ```

  

- 묶이길 원하는 조건끼리 break문을 생략하면 break문이 나오는 지점까지 조건들에 1가지 명령을 실행할 수 있음.

- ```java
  char grade = 'b';
  
  		switch (grade) {
  		case 'A' :
  		case 'a' :
  			System.out.println("우수 회원입니다.");
  			break;
  		case 'B' :
  		case 'b' :
  			System.out.println("일반 회원입니다.");
  			break;
  		default :
  			System.out.println("손님입니다. 회원가입하시겠습니까?");
  ```

  

### String 타입을 사용한 switch 문

- 자바 과거 버전에서는 switch 문에 기본타입만 사용할 수 있었음.

- 현재는 String 타입도 switch 문에 활용할 수 있음.

- ```java
  String position = "과장";
  		
  		switch (position) {
  		case "부장" :
  			System.out.println("성과급은 1000만원");
  			break;
  		case "과장" :
  			System.out.println("성과급은 500만원");
  			break;
  		case "대리" :
  			System.out.println("성과급은 200만원");
  			break;
  		default :
  			System.out.println("성과급 없음");
  			break;
  		}
  ```



### 주민번호를 입력받아 남,여를 구분하는 switch 문

- .charAt(숫자) -> 문자열 중 숫자에 해당하는 문자를 추출함. 숫자 0번부터 시작함.

- ```java
  switch (gender) {
  		case '1': case '3' : case '5' : case '7' :
  			System.out.println("당신은 남자입니다.");
  			break;
  		case '2': case '4' : case '6' : case '8' :
  			System.out.println("당신은 여자입니다.");
  			break;
  		default:
  			System.out.println("올바른 주민번호를 입력해주세요");
  			break;
  		}
  ```

- if문으로도 비슷하게 만들 수 있음.

- ```java
  if (gender%2==0) {
  			System.out.println("당신은 남자입니다.");
  		} else if (gender%2==1) {
  			System.out.println("당신은 여자입니다.");
  		} else {
  			System.out.println("올바른 주민번호를 입력해주세요");
  		}
  ```




### 개선된 switch 문

- case 문을 간략히 하고 '->'를 이용하여 문법을 수정함.

- ```java
  	public static void main(String[] args) {
  		// Scanner로 입력 받은 값을 판단하는 switch문 구현
  		Scanner in = new Scanner(System.in);
  		System.out.println("동물의 이름을 입력하세요 >>");
  		String Monster = in.nextLine();
  
  		whoIsIt(Monster);
  
  	} // main 메소드 종료
  
  	static void whoIsIt(String bio) {
  		String kind = "미지의 생물";
  		switch (bio) {
  		case "호랑이", "사자", "강아지", "고양이" -> kind = "포유류";
  		case "독수리", "참새", "까마귀", "비둘기" -> kind = "조류";
  		case "고등어", "연어", "삼치", "갈치" -> kind = "어류";
  		default -> System.out.println("어이쿠!!!");
  		}
  		System.out.printf("%s는 %s이다.\n", bio, kind);
  	}
  
  ```

  





## 5. 메서드









# Reference

- [쉽게 배우는 자바 프로그래밍 - 우종정](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791156645146&orderClick=LAG&Kc=)

- [혼자 공부하는 자바 - 신용권](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9791162241875&orderClick=LAG&Kc=)
- [자바의 정석 - 남궁성](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&mallGb=KOR&barcode=9788994492032&orderClick=LAG&Kc=)

