## 1. JavaScript의 시작

<br>

> 이 문서는 [<한 권으로 끝내는 실무 Java 웹 개발서 - 김정현, 김계희>](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025) 책을 보고 정리한 문서입니다.

<br>

### 1.1. JavsScript 역사와 특징

- 1995년 넷스케이프에서 일하던 브랜든 아이크에 의해 처음 개발.
- JavaScript의 구현
  - 코어(ECMAScript)
  - 문서 객체 모델(DOM)
  - 브라우저 객체 모델(BOM)
- 스크립트란 컴파일되지 않고 애플리케이션이 실행되는 동안 Line 단위로 해석되는 명령어나 문장들의 집합.



### 1.2. JavaScript 구현 방법

- HTML에 동적인 기능을 부여하는 프로그램 언어.
- 일반적으로 \<HEAD>와 \</HEAD> 사이에서 작성.
- type="text/javascript"는 JavaScript의 유형이 텍스트이고 사용 언어는 JavaScript라는 뜻.
- `<script type="text/javascript"> 실행문; </script>`
- 주석은 /* */ , //
- 삽입하는 방법 3가지
  - \<head> 태그 안에서 작성.
  - \<body> 태그 안에서 작성.
  - 확장자를 js로 하는 외부 파일을 불러서 사용.



### 1.3. JavaScript 주요 문법

- 대소문자 구분함.
- 원칙적으로 세미콜론(;)으로 닫지만 Enter로 라인을 구분하기 때문에 생략이 가능함.



#### 변수 선언과 데이터 타입

- 변수(variable)란 데이터를 저장하는 공간.
- 변수에 저장되는 값을 데이터 값 또는 상수(constant)라고 함.
- 자바스크립트에서는 변수의 타입을 따로 선언하지 않음.

- `var 변수명;`
- var는 variable의 약자이며 생략이 가능함.
- 변수명은 대소문자를 구별하고 영문, $, _, 숫자를 포함할 수 있음.
- 변수명의 맨 앞에는 숫자가 올 수 없음. 영문, $, _만 가능함.
- 변수에 저장되는 데이터 타입
  - 문자형
  - 숫자형
  - 논리형
  - 널형



#### 연산자

##### 산술 연산자

- 더하기 / 빼기 / 곱하기 / 나누기 / 나머지

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>산술 연산자</title>
<script>
	var a = 10, b = 3, sub, mult, divi;
	sub = a - b;
	mult = a * b;
	divi = a / b;
	
	document.write("두수의 합 : " + (a + b) + "<br>");
	document.write("두수의 차 : " + sub + "<br>");
	document.write("두수의 곱 : " + mult + "<br>");
	document.write("두수의 몫 : " + divi + "<br><br>");
</script>
</head>
<body>

</body>
</html>
```

- 12라인의 (a+b)에서 괄호를 생략할 경우 "문자열" + 숫자의 덧셈 연산은 두 데이터를 연결한다는 의미가 되기 때문에 "두 수의 합: 103"으로 출력됨.



##### 관계 연산자

- A==B 와 A!=B 는 A와 B의 데이터 값이 같은지와 그렇지 않은지를 비교하는 연산자.
- A===B와 A!==B는 데이터 형이 같은지와 그렇지 않은지를 비교함.

```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>관계 연산자</title>
<script>
	var a=5;
	rs=a==5; 	document.write(rs + "<br>");
	rs=a>=5;	document.write(rs + "<br>");
	rs=a!=5;	document.write(rs + "<br>");
</script>
</head>
<body>
</body>
</html>
```

- CDATA
  - 유효성 검사의 오류를 막기 위한 스크립트 명령문
  - CDATA를 선언하면 선언 내부에 있는 '태그'는 '태그'로 인식하는 것이 아니라 '문자'로 인식함.
  - 자바스크립트 안에 '태그'가 들어갈 경우 반드시 CDATA 선언 안에 작성해야 함.
  - 자바스크립트를 외부 파일(*.js)을 만들고 불러서 사용할 경우에는 CDATA를 사용하지 않아도 됨.

```HTML
// CDATA 선언 기본 형식
<script>
	//<![CDATA[
    	스크립트 실행문;
    //]]>
</script>
```

- CDATA는 웹 브라우저에서 &, >, <와 같은 특수문자를 HTML이 아닌 문자로 인식해서 그대로 출력함.



##### 대입(할당) 연산자

- +=, -=, *=, /=

```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>대입(할당)연산자</title>
<script>
	var num=5;
	num+=2;	document.write(num + "<br>");
	num*=5;	document.write(num + "<br>");
	num%=3;	document.write(num + "<br>");
</script>
</head>
<body>
</body>
</html>
```



##### 논리 연산자

- A && B : AND
- A || B : OR
- !A : NOT

```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>논리연산자</title>
<script>
	var str="korea";
	document.write(str=="korea" || str=="KOREA");
	document.write("<br>");
	
	var score=57;
	document.write(score>=70 && score<90);
	document.write("<br>");
	
	var sign="true";
	document.write(!sign);
</script>
</head>
<body>

</body>
</html>
```



##### 증감 연산자

- ++A, A++
- --A, A--

```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>증감연산자</title>
<script>
	var k=5;
	document.write(k++ + "<br>");
	document.write(++k + "<br>");
	document.write(k + "<br>");
	document.write(--k + "<br>");
	document.write(k-- + "<br>");
</script>
</head>
<body>

</body>
</html>
```



##### 문자 결합 연산자

- "문자" + "문자"
- "문자" + 숫자

```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문자결합연산자</title>
<script>
	var str1="korea";   var str2=" fighting";
	document.write(str1 + str2 + "<br><br>");
	
	var str="abc";  var num="123";
	document.write(str + num);
</script>
</head>
<body>

</body>
</html>
```



##### 삼항(조건) 연산자

- (조건식)?A:B;  >> 조건식의 결과가 참이면 A를 수행하고 거짓이면 B를 TNGOD.

```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>삼항연산자</title>
<script>
	var score=prompt("점수를 입력하시오","");
	(score>=60)?alert("합격") : alert("불합격");
</script>
</head>
<body>

</body>
</html>
```



#### 제어문

##### 조건문

- if, if~else, if~else if

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>조건연산자</title>
<script>
	var num=35;
	if(num%7==0)
	{
		document.write(num+"은(는) 7의 배수 입니다");
	}
</script>
</head>
<body>

</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>if~else문</title>
<script>
	var score=prompt("점수를 입력하시오","");
	if(score>=60){
		document.write("합격<br>");
		document.write("당신의 점수는 "+score+"점 입니다");
	}else{
		document.write("불합격<br>");
		document.write("당신의 점수는 "+score+"점 이며, 다음 기회에~");
	}
</script>
</head>
<body>

</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>if~else if문</title>
<script>
	var score=95;
	var grade;                
	
	if(score>=90)		grade='A';
	else if(score>=80)	grade='B';
	else if(score>=70)	grade='C';
	else if(score>=60)	grade='D';
	else				grade='F';
	
	document.write("나의 점수는 "+score+"점이고 학점은 " + grade +" 입니다"); 
</script>
</head>
<body>

</body>
</html>
```



##### 선택문

- switch~case

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>switch(선택문)</title>
<script>
	var age=prompt("나이를 입력하시오","");
	switch(true)
	{
		case age>=60: alert("노년입니다");  break;
		case age>=35: alert("중년입니다");  break;
		case age>=20: alert("청년입니다");  break;
		default : alert("어린이 또는 청소년입니다");
	} 
</script>
</head>
<body>

</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>switch(선택문)</title>
<script>
	var code=prompt("부서코드를 입력하시오(dev1,dev2,eng1,eng2)","");
	var dept;
	switch(code)
	{
		case "dev1": 
		case "dev2": dept="개발부" ; break;
		case "eng1": 
		case "eng2": dept="기술부" ; break;
	} 
	document.write("당신은 " + dept+ "에 근무하며, 부서 코드는 " + code +"입니다");
</script>
</head>
<body>

</body>
</html>
```



##### 반복문

- for

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>for(반복문)</title>
<script>
	for(var a=1; a<=10; a++){
		document.write(a+"&nbsp;&nbsp;&nbsp;");
	}
	
	for(var b=1; b<=6; b++){
		document.write("<h"+b+">Hello World<"+"/h"+b+">");
	}
</script>
</head>
<body>

</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>for(반복문)</title>
<script>
	var sum=0, even=0, odd=0;
	for(var i=1; i<=100; i++)
	{
		sum += i;             
		switch(i % 2)
		{
			case 0 : even+=i;   break;   
			case 1 : odd+=i;    break;   
		}
	}
	document.write("1-100까지의 전체합: " + sum + "<br>");
	document.write("1-100까지의 홀수합: " + odd + "<br>");
	document.write("1-100까지의 짝수합: " + even + "<br>");
</script>
</head>
<body>

</body>
</html>
```

- 반복문에서 break를 만나면 해당 반복문의 {}에서 빠져나와 반복 작업을 종료.
- continue를 만나면 다음 실행문을 수행하지 않고 다음 loop를 진행.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>break/continue</title>
<script>
	for(var i=1; i<=10; i++)
	{
		if(i==5)
			break;
		document.write(i+"&nbsp;&nbsp;")
	}
	document.write("<br><br>");
	
	for(var j=1; j<=10; j++)
	{
		if(j==5)
			continue;
		document.write(j+"&nbsp;&nbsp;")
	}
</script>
</head>
<body>

</body>
</html>
```

- while

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>while(반복문)</title>
<script>
	var n=10;
	while(n >= 1)
	{
		document.write(n+"&nbsp;&nbsp;");
		--n;
	}
	document.write("<br><br>");
	
	var m=0;
	while(m < 3)
	{
		document.write("<img src='image/mouse.png'>");
		++m;
	} 
</script>
<body>

</body>
</html>
```

- do~while

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>반복문(do~while문)</title>
<script>
	var kor, eng, sum;
	do{
		kor=prompt("국어 점수를 입력하시오","");
		if(kor<0 || kor>100)
			alert("0-100사이의 숫자를 입력하시오");
	}while(kor<0 || kor>100);
	
	
	do{
		eng=prompt("영어 점수를 입력하시오","");
		if(eng<0 || eng>100)
			alert("0-100사이의 숫자를 입력하시오");
	}while(eng<0 || eng>100);
	
	sum=parseInt(kor)+parseInt(eng);
	document.write("합계 :" +sum+"점");
</script>
<body>

</body>
</html>
```



### 1.4. 함수(Function)

- 코드의 양이 많은 큰 프로그램을 관련 코드끼리 묶어서 분리하는 구조적 프로그램.



#### 선언적 함수

- 함수 선언식(Function Declarations)이라고도 함.
- function이라는 키워드를 사용해서 함수를 선언.
- 이 함수를 사용할 경우에는 호출문이 먼저 나와도 에러 없이 처리함.



##### 함수의 정의와 호출

- 함수는 function이라는 키워드를 쓰고 함수 이름을 정의한 후 블록 안에서 내용을 구현함.
- 함수를 사용할 때는 정의한 함수명으로 호출함.



##### 매개 변수와 리턴값

- 매개 변수(인자)란 호출하는 함수에 값을 전달할 때 () 안에 데이터를 넣어 보내는 변수를 매개 변수라고 함.
- 계산된 결과를 반환하는 값을 리턴값이라고 함.
- 리턴값은 데이터, 변수, 수식 등 3가지 형태로 사용할 수 있음.
- return 값이 없는 경우도 있으며 이때는 return만 쓰거나 생략할 수 있음.
- return 값이 있는 경우에는 반드시 1개만 가능함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>선언적 함수</title>
<script>
	function kindShape(){
		document.write("**도형의 넓이**<br>");
	} 
	function circleSize(r){
		document.write("원의 넓이 = " + (r*r*3.14) +"<br>");
	}
	function getSquareSize(bottom, height){
	   return bottom * height;      
	}
	kindShape();
	circleSize(10);
	document.write("사각형의 넓이 = " + getSquareSize(4,5));
</script>
</head>
<body>

</body>
</html>
```



#### 익명 함수

- 함수 표현식(Function Expressions)이라고도 함.
- function 형태는 함수이지만 이름이 없기 때문에 익명 함수라고 부름.
- 익명 함수는 호출문을 함수 선언 이후에 작성해야 에러가 발생하지 않음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>익명 함수</title>
<script>
	var getTrapezoid = function(bottom,top,height){ 
	   return (bottom + top)* height / 2.0;      
	}
	document.write("사다리꼴의 넓이=" + getTrapezoid(5,4,3));
</script>
</head>
<body>

</body>
</html>
```



#### 내장 함수

- 내장 함수는 자바스크립트가 자체적으로 가지고 있는 함수.
- 네트워크 통신용 함수와 숫자와 관련된 함수가 있음.
- 인코딩
  - 네트워크 통신에 전송할 목적으로 어떤 시스템에서도 읽을 수 있도록 ASCII 문자로 바꾸어 주는 부호화 함수임.
  - `encodeURI()` : 파라미터를 전달하는 URI 전체를 인코딩할 때 사용하며 특수문자를 제외한 문자만 인코딩됨. 주로 인터넷 주소를 인코딩할 때 사용함.
- 디코딩
  - 부호화된 문자를 원래대로 되돌리는 함수임.
  - `decodeURI()` : encodeURI()로 인코딩된 데이터를 다시 되돌림.
- URI(Uniform Resource Identifier)는 하나의 리소스를 가리키는 문자열. 가장 흔한 URI가 인터넷 주소 리소스를 가리키는 url임.

- 숫자와 관련된 함수
  - `isNaN()` : NaN은 Not a Number의 약자. 숫자가 아닌 문자가 포함되면 true 반환.
  - `isFinite()` : 값이 유한수인지를 판단.
  - `parseInt()` : 문자를 정수형으로 변환.
  - `parseFloat()` : 문자를 실수형으로 변환.
  - `eval()` : 문자로 된 수식을 자바스크립트의 수식으로 인식하여 실행시키고 결과 변환.
  - `Number()` : 문자를 숫자형으로 변환.
  - `String()` : 숫자를 문자형으로 변환.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>인코딩과 디코딩</title>
<script>
	function ex1(){
		alert(encodeURI('가나다')+"\n"+ decodeURI('%EA%B0%80%EB%82%98%EB%8B%A4'));
	}
	function ex2(){
		var x=10, y=15;
		alert(String(x)+String(y));
	}
	function ex3(){
		var ob1=eval("var num=5+2");
		var ob2=eval("({'a':1, 'b':2, 'c':3})");
		alert(num +"\n" + ob2.b);
	}
</script>
</head>
<body>
<form name="frm">
	<input type="button" onclick="ex1()" value="연습1">&nbsp;&nbsp;
	<input type="button" onclick="ex2()" value="연습2">&nbsp;&nbsp;
	<input type="button" onclick="ex3()" value="연습3">&nbsp;&nbsp;
</form>	
</body>
</html>
```

- 과거에는 JSON 파일을 불러올 때 문자 그대로 넘어와 eval 함수를 통해 파싱하여 데이터에 접근했었음
- 현재는 eval 함수는 보안상의 이슈로 사용하지 않음.
- JSON 파일 파싱하려면 JSON.parse 함수 사용.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>기타 내장 함수</title>
<script>
function plusfunc(){
	var a=parseInt(frm.a.value);
	var b=parseInt(frm.b.value);
	
	if(isNaN(a) || isNaN(b)){
		alert("a값 또는 b값은 숫자가 아닙니다");
		frm.a.value="";
		frm.b.value="";
		frm.a.focus();
		return;
	}
	frm.rs.value = a+b;
}
function divifunc(){
	var a=Number(frm.a.value);
	var b=Number(frm.b.value);
	rs=a/b;
	
	if(isFinite(rs)==false){
		alert("0으로 나눌수 없습니다");
		frm.a.value="";
		frm.b.value="";
		frm.a.focus();
		return;
	}
	frm.rs.value=rs;
}
</script>
</head>
<body>
<form name="frm">
	A값 : <input type="text" size="7" name="a">&nbsp;&nbsp;
	B값 : <input type="text" size="7" name="b">&nbsp;&nbsp;
	결과 : <input type="text" size="7" name="rs">
	<input type="button" name="+" value="더하기" onclick="plusfunc()">
	<input type="button" name="/" value="나누기" onclick="divifunc()">
</form>
</body>
</html>
```



## 2. JavaScript 객체

### 2.1. 객체 생성

- 객체(Object)는 데이터를 의미하는 프로퍼티(또는 속성)와 데이터 처리 기술(즉, 동작인 메소드(Method))의 조합임.
- 프로퍼티는 프로퍼티 이름과 프로퍼티 값으로 구성됨.
- 메소드는 객체에서 사용하는 함수.
- 자바스크립트에서는 3가지 객체 생성 방법이 존재함.
  - 리터럴을 이용한 객체 생성
  - 생성자 함수를 이용한 객체 생성
  - Object.create()



#### 리터럴을 이용한 객체 생성

- 객체 리터럴 방식은 선언과 동시에 인스턴스를 생성함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>리터럴을 이용한 객체 생성</title>
<script>
	var User={
	  	name: 'kim',
	  	age: 25,
	  	view: function () {
	  		document.write('My name is ' + this.name + "<br>");
	  		document.write('My age is ' + this.age);
	  	}
	};
	document.write(typeof User + "<br>"); 
	User.view();
</script>
</head>
<body>

</body>
</html>
```



#### 생성자 함수를 이용한 객체 생성

- Object로 빈 객체를 생성한 후 프로퍼티와 메소드 추가.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Object로 빈 객체 생성 후 프로퍼티와 메소드 추가</title>
<script>
	var User=new Object();
	User.name='kim';
	User.age=25;
	User.view=function(){
		document.write('My name is ' + this.name + "<br>");
		document.write('My age is ' + this.age);
	};
	User.view();
</script>
</head>
<body>

</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>생성자를 이용한 객체 생성</title>
<script>
	//생성자 함수
	function User(name, age){
		this.name=name;
		this.age=age;
		this.view=function(){
			document.write('나의 이름은 '+this.name+'이고 나이는 '+this.age+"세 입니다<br>");
		};
	}
	//인스턴스 생성
	var ob1=new User('kim',25);
	var ob2=new User('lee',20);
	ob1.view();
	ob2.view();
</script>
</head>
<body>

</body>
</html>
```



### 2.2. 표준 객체

- 표준 객체(Standard Built-in Object)란 자바스크립트에서 자주 사용하는 Number, String, Array, Date, Math 등의 내장 객체를 말함.



#### Number 객체

- 숫자를 다루는 객체.
- `var 변수명 = new Number(숫자);`
- `var 변수명 = 숫자;`

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Number 객체</title>
<script>
	var a=15;
	var b=new Number(15);
	document.write("a와 b를 더하면: " + (a.toString()+b.toString()));
	document.write("<br>15의 2진수 표현: " + a.toString(2));
	document.write("<br>a와 b의 타입: " + typeof a+"&nbsp;&nbsp;"+typeof b);
	document.write("<br>a와 b의 값 비교: " + (a==b));
	document.write("<br>a와 b의 타입 비교: "+ (a===b));
</script>
</head>
<body>

</body>
</html>
```



#### String 객체

- String 객체는 문자열을 다루는 객체.
- 문자열은 단일 따옴표(' ') 또는 이중 따옴표(" ") 안에 작성함.
- 자바스크립트에서 문자열은 주로 리터럴을 사용함.
- new 연산자로 String 객체를 생성해서 사용하기도 함.
- `var 변수명 = new String("문자열");` 또는 `var 변수명 = "문자열";`
- String 속성과 문자 관련 메소드
  - `length` : 문자열의 길이를 구함.
  - `big() / small()` : 문자를 한 단계 크게 / 작게 설정함.
  - `blink()` : 문자를 깜박이게 설정함.
  - `bold()` : 문자를 굵게 설정함.
  - `sub() / sup()` : 아래 첨자 / 위 첨자로 설정함.
  - `fontsize(크기)` : 문자의 크기를 설정함. (범위 : 1-7)
  - `fontcolor(색상)` : 문자의 색상을 설정함.
  - `toLowerCase()` : 문자를 소문자로 변경.
  - `toUpperCase()` : 문자를 대문자로 변경.
  - `anchor('#위치표시문자')` : \<a name=" ">과 같은 효과.
  - `link('링크할 위치')` : \<a href= " ">과 같은 효과.
  - `italics() / strike()` : 이탤릭체 / 취소선 설정.

- String 문자열 관련 메소드
  - `charAt(n)` : n번째 위치의 문자를 반환함.
  - `indexOf("찾을 문자")` : 처음부터 시작해서 최초로 만나는 "찾을 문자"의 위치를 반환함.
  - `lastIndexOf("찾을 문자")` : 끝에서부터 시작해서 최초로 만나는 "찾을 문자"의 위치를 반환함.
  - `substring(n1, n2)` : n1에서 (n2-1) 사이의 문자열을 반환함.
  - `slice(s, e)` : s부터 (e-1)의 문자열을 분리함.
  - `substr(s, 길이)` : s부터 길이만큼 문자열을 추출함.
  - `concat("문자열")` : 두 개의 문자열을 연결함.
  - `split("기준 문자")` : 기준이 되는 문자로 문자열을 잘라서 배열에 저장.
  - replace(s1, s2) : 문자열 중 s1을 s2로 치환함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문자열 함수</title>
<script>
	var data=new String("javascript test");
	document.write(data+"<br>");                
	
	document.write(data.length+"<br>");         
	document.write(data.toLowerCase()+"<br>"); 
	document.write(data.bold()+"<br>");
	document.write("남가람 북스".link('http://www.namgarambooks.co.kr')+"<br><br>"); 
	
	document.write(data.charAt(0)+"<br>");      
	document.write(data.substring(1,3)+"<br>"); 
	document.write(data.substring(4)+"<br>"); 
	document.write(data.replace("test", "sample")+"<br>");
</script>
</head>
<body>

</body>
</html>
```



#### Array 객체

- 배열은 하나의 객체에 여러 개의 데이터 값을 저장할 때 사용함.
- 같은 타입의 자료만 저장해야 하는 다른 프로그래밍 언어와 달리 서로 다른 타입의 데이터를 저장하는 것이 가능함.
- 자바스크립트에서 배열을 선언하는 방식은 리터럴과 배열 객체를 사용하는 2가지가 있음.
- 리터럴 방식은 대괄호를 사용하여 배열 선언과 값을 한 번에 처리함. 가장 일반적인 방식.
- `var 변수명 = [값1, 값2, ... , 값n];`
- Array 관련 메소드
  - `sort()` : 배열 값들을 오름차순으로 정렬함.
  - `reverse()` : 배열 값들을 역순으로 바꿈.
  - `concat(array)` : 두 개의 배열을 합하여 하나의 배열로 만듦.
  - `join([str])` : 배열에 들어 있는 값을 모두 붙여서 하나의 문자열로 만듦.
  - `push()` : 배열의 마지막에 새로운 원소를 추가함.
  - `pop()` : 배열의 마지막 원소를 추출. (가장 최근에 push()한 요소)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>배열 객체</title>
<script>
	var fruits=["Banana", "Orange", "Apple", "Mango"];
	
	var len=fruits.length;
	var text="";
	for (i = 0; i < len; i++) {
		text += fruits[i] + "<br>";
	}
	document.write(text);
</script>
</head>
<body>

</body>
</html>
```

- 배열 객체를 선언하는 방식은 new 키워드를 사용해서 객체 선언과 동시에 값을 인자로 추가함.

- `var 변수명 = new Array (값1, 값2, ... , 값n);`

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>배열 객체</title>
<script>
	var fruits=new Array("Banana", "Orange", "Apple", "Mango");
	fruits.sort();
	document.write(fruits);
	document.write("<br>");
	
	fruits.reverse();
	document.write(fruits);
</script>
</head>
<body>

</body>
</html>
```



#### Date 객체

- Date 객체는 날짜와 시간에 대한 정보를 제공함.
- 객체 생성 시 날짜를 지정하지 않으면 시스템에 설정된 날짜와 시간을 제공함.
- `var 변수명 = new Date();` 또는 `var 변수 = new Date(년, 월, 일, 시, 분, 초);`
- 날짜와 시간 관련 메소드
  - `setFullYear() / getFullYear()` : 연도만 설정하거나 반환.
  - `setMonth() / getMonth()` : 월만 설정하거나 반환.
  - `setDate() / getDate()` : 일(월 기준)을 설정하거나 반환.
  - `setDay() / getDay()` : 일(주 기준)을 설정하거나 반환.
  - `setHour() / getHour()`  : 시간을 설정하거나 반환.
  - `setMinutes() / getMinutes() ` : 분을 설정하거나 반환.
  - `setSeconds() / getSeconds()` : 초를 설정하거나 반환.
  - `setMiliseconds() / getMiliseconds()` : 밀리 초를 설정하거나 반환.
  - `toString()` : 날짜를 문자형식으로 반환.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>날짜 함수</title>
<script>
	var days=new Array("일","월","화","수","목","금","토");
	var date=new Date();
	document.write("오늘은 " 
			+date.getFullYear()+"년"
			+(date.getMonth()+1)+"월"
			+date.getDate()+"일 "
			+days[date.getDay()]+"요일 입니다.<br>");
	
	document.write("현재시간은 "+date.getHours()+":" 
			                +date.getMinutes()+":"
		                    +date.getSeconds()+"입니다.");
</script>
<body>

</body>
</html>
```



#### Math 객체

- Math 객체는 수학에서 자주 사용하는 메소드를 미리 정의해놓은 객체.
- 최대값/최소값을 구하거나 거듭제곱, 제곱근 등의 특정 공식을 사용할 수 있음.
- 단순한 계산이 아닌 경우 Math 객체의 메소드를 사용하면 매우 편리함.
- 수학 관련 메소드
  - `max(n1, n2, ...)` : 가장 큰 값을 반환.
  - `min(n1, n2, ...)` : 가장 작은 값을 반환.
  - `round(n)` : 반올림 값을 반환.
  - `ceil(n)` : 올림 값을 반환.
  - `floor(n)` : 내림 값을 반환.
  - `abs(n)` : 절댓값을 반환.
  - `random(n)` : 0~1 사이의 임의의 수를 반환.
  - `pow()` : 숫자의 거듭제곱을 계산해서 반환.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Math 객체</title>
<script>
	document.write(Math.pow(8, 2)+"<br>");  
	document.write(Math.random()+"<br>");   
	document.write(Math.floor(4.7)+"<br>"); 
	document.write(Math.ceil(4.4)+"<br>");  
	document.write(Math.round(4.7)+"<br>"); 
	document.write(Math.abs(-4.4)+"<br>");  
	document.write(Math.sqrt(64)+"<br>");   
</script>
</head>
<body>

</body>
</html>
```



### 2.3. 브라우저 객체 모델(BOM)

#### window 객체

#### location 객체

#### navigator 객체

#### history 객체

#### screen 객체



### 2.4. 문서 객체 모델(DOM)



### 2.5. 이벤트(Event) 모델

#### 인라인 이벤트 모델

#### 고전 이벤트 모델

#### 인터넷 익스플로러 이벤트 모델

#### 표준 이벤트 모델



### 2.6. JSON

### 2.7. 정규 표현식



## 3. JavaScript와 표준 API 활용







<br>

# References

- [한 권으로 끝내는 실무 Java 웹 개발서 / 김정현, 김계희 / 남가람북스 / 2019](http://www.kyobobook.co.kr/product/detㄴailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025)

<br>