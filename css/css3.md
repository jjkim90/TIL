# CSS3

<br>

> 이 문서는 [<한 권으로 끝내는 실무 Java 웹 개발서 - 김정현, 김계희>](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025) 책을 보고 정리한 문서입니다.

<br>

## 1. CSS3의 개요

- CSS(Cascading Style Sheets)
- 텍스트의 색상이나 크기 또는 이미지의 크기나 위치 등을 처리하는 웹 문서에서 디자인 요소를 담당하는 언어.
- 자주 사용하는 디자인 부분들을 미리 조합해서 만들어 놓고 HTML 페이지 내부에서 필요할 때마다 적용함.

<br>

### 1.1. CSS3의 역사와 특징

- 1996년 W3C가 표준안 CSS1 제정.
- 1998년 CSS2
- 2005년 CSS3
- HTML로는 부족한 레이아웃이나 폰트 등에 다양성을 부여함.
- 폰트 크기, 자간 설정, 행간의 높이, 페이지 여백을 마음대로 조절할 수 있음.
- 웹페이지의 레이아웃을 자유롭게 설계하여 배치할 수 있음.
- 링크할 때 밑줄의 변형도 가능해서 가독성을 높일 수 있음.

<br>

### 1.2. CSS 작성 방법

```html
<style type="text/css">
p { color: red; padding: 7px;}
</style>
```

- 스타일 시트 선언은 \<STYLE>\</STYLE> 사이에 기술함.
- type="text/css"는 스타일의 유형이 텍스트이고 파일은 CSS라는 뜻.
- type="text/css"는 생략이 가능함.
- \<STYLE> 태그 내부에 작성하는 CSS의 기본 문법은 선택자 { }로 구성함.
- 선택자 안의 선언문(속성과 속성값) 간의 구분은 ;(세미콜론)으로 함.
- 외부 문서로 만들 경우 CSS로 파일을 따로 저장하고 @import나 link를 사용하여 HTML 태그와 연결하여 스타일을 적용함.
- 주석 처리는 /*와 */ 사이에 작성함.
- CSS 코드 만드는 방법 3가지
  - 1. 내부 스타일 시트는 HTML 문서 안의 \<STYLE>과 \</STYLE> 안에 CSS 코드를 추가하는 방법.
    2. 외부 스타일 시트는 별도의 CSS 파일을 만들어 놓고 link나 @import를 이용하여 HTML 문서와 연결하는 방법.
    3. HTML 태그의 style 속성에 CSS 코드를 추가하는 방법.
- 외부 스타일 시트는 여러 HTML 문서에 재활용 가능하고 관리 측면에서 볼 때 사용하기에 편리함.
- @import url(" ")은 여러 개의 CSS 파일을 한 번에 가져올 수 있음.

```HTML
<style>
    @import url("style1.css");
    @import url("style2.css");
    @import url("style3.css");
</style>
```

- link 태그와 @import는 비슷하게 사용하지만 성능(속도) 면에서는 link가 더 빠름.

```html
<head>
    ...
	<link rel="stylesheet" type="text/css" href="style.css">    
</head>
<body>
    <span>Hello World</span>
</body>
```

```css
span{ color: red; }
```

<br>

## 2. 기본적인 CSS3 선택자

- 선택자(Selector)란 특정 태그나 요소를 지칭함.
- 지정한 선택자에 사용자가 의도하는 디자인을 적용함.

<br>

### 2.1. 전체 선택자

- 전체 선택자(Universal Selector)는 *(별표) 선택자임.
- 와일드(wild) 선택자라고도 하며 모든 HTML 태그를 선택함.

```css
*{속성: 값;}
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Universal Selector</title>
<style>
	*{ font-family:"Comic Sans MS"; }
</style>
</head>
<body>
	<h1>Web Programming</h1>
	<ul>
		<li>HTML</li>
		<li>CSS</li>
		<li>JavaScript</li>
	</ul>
</body>
</html>
```

<br>

### 2.2. 타입(태그) 선택자

- 타입 선택자(Type Selector)는 p, div, span 등 HTML의 특정 태그를 선택하여 스타일을 적용함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Type Selector</title>
<style>
	h1{	font-family:"Comic Sans MS"; }
</style>
</head>
<body>
	<h1>Web Programming</h1>
	<ul>
		<li>HTML</li>
		<li>CSS</li>
		<li>JavaScript</li>
	</ul>
</body>
</html>
```

<br>

### 2.3. 아이디(ID) 선택자

- 아이디 선택자(ID Selector)는 특정 id 속성값을 가지고 있는 태그를 선택함.
- id 속성값 앞에 #을 붙여서 아이디 선택자임을 표시함.
- `#ID명{속성: 값;}`

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>ID Selector</title>
<style>
	#t1{ font-weight: bold; }
</style>
</head>
<body>
	<span id="t1">Have a nice day</span>
</body>
</html>
```

<br>

### 2.4. 클래스(Class) 선택자

- 특정 class 속성값으로 가지고 있는 태그를 선택함.
- class 속성값 앞에 .(점)을 붙여서 클래스 선택자임을 나타냄.
- `.클래스명{속성: 값;}`
- `태그명.클래스명{속성: 값;}`

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>class가 t2인 요소에만 스타일 적용</title>
<style>
	.t2{ font-weight: bold; }
</style>
</head>
<body>
	<p class="t2">Have a nice day</p>
	<span class="t2">좋은 하루 되세요</span>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>class가 p.t2인 요소에만 스타일 적용</title>
<style>
	p.t2{ font-weight: bold;}
</style>
</head>
<body>
	<p class="t2">Have a nice day</p>
	<span class="t2">좋은 하루 되세요</span>
</body>
</html>
```

<br>

#### ID와 CLASS의 차이점

- ID
  - ID는 유일한 요소에 스타일을 적용함.
  - 한 문서에서 ID는 한 번만 사용 가능함.
  - 상단 메뉴바, 하단 정보, 홈페이지 로고처럼 유일한 것들을 선택할 때 사용함.
- CLASS
  - CLASS는 '복수' 요소에 스타일을 적용함.
  - 한 문서에서 여러번 사용 가능함.
  - 소제목처럼 반복되는 요소에 사용함.

<br>

### 2.5. 속성 선택자

- 속성 선택자(Attribute Selector)는 특정 속성을 가지고 있거나 특정 값을 가지고 있는 요소를 선택함.
- `태그명[속성명]{속성: 값;}`
- 속성 선택자 형태 7가지
  - Tag[attr]{속성: 값;} : 속성 포함된 태그 선택
  - Tag[attr="val"]{속성: 값;} : 속성값이 'val'과 일치하는 태그 선택
  - Tag[attr~="val"]{속성: 값;} : 속성값이 'val'을 단어로 포함하는 태그를 선택
  - Tag[attr|="val"]{속성: 값;} : 속성값이 'val'이거나 시작하는 태그를 선택
  - Tag[attr^="val"]{속성: 값;} : 속성값이 'val'로 시작하는 태그를 선택
  - Tag[attr*="val"]{속성: 값;} : 속성값이 'val'을 포함하는 태그를 선택
  - Tag[attr$="val"]{속성: 값;} : 속성값이 'val'로 끝나는 태그를 선택

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>속성 선택자로 만든 로그인</title>
<style>
 	a[target=_blank] { color: green; }  
	[class^="first"] { color: red; } 
	input[type=text] { width: 100px; margin-bottom: 10px; background-color: #D5D5D5; } 
	input[type=button] { color:white; font-weight:bold; background-color: #000; }
</style>
</head>
<body>
	<div class="first_test">[로그인 화면]</div> 
	<div class="second_test">아이디와 비밀번호를 입력하시오</div><br>
	<form name="input" action="" method="get"> 
		아이디<input type="text" name="id" size="20"> 
		비밀번호<input type="text" name="pw" size="20"> 
		<input type="button" value="로그인"> 
		<a href="register.html" target="_blank">회원가입</a><br><br> 
	</form>
</body>
</html>
```

<br>

### 2.6. 가상 클래스 선택자

- 웹 문서의 소스에는 실제로 존재하지 않지만 필요에 의해 가상의 선택자를 지정해서 사용하는 것.
- 문자 블록
  - `tag:first-line{속성: 값;}` : 태그의 첫 번째 라인을 선택
  - `tag:first-letter{속성: 값;}` : 태그의 첫 번째 문자를 선택
  - `tag:first-child{속성: 값;}` : 태그의 첫 번째 자식 요소를 선택
  - `tag:nth-child(e){속성: 값;}` : 태그의 odd/even/n번째 요소를 선택
- 콘텐츠
  - `tag:before{content: 값;}` : 선택한 태그 앞에 지정한 콘텐츠를 삽입
  - `tag:after{content: 값;}` : 선택한 태그 뒤에 지정한 콘텐츠를 삽입
- 하이퍼링크
  - `tag:link{속성: 값;}` : 하이퍼링크 태그에 방문하지 않았을 경우
  - `tag:visited{속성: 값;}` : 하이퍼링크 태그에 방문했을 경우
- 마우스
  - `tag:hover{속성: 값;}` : 마우스 커서가 태그에 올라와 있는 동안
  - `tag:active{속성: 값;}` : 태그가 활성화되었을 경우
  - `tag:focus{속성: 값;}` : 태그에 포커스가 머물러 있는 경우

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>가상 클래스 선택자로 제목 꾸미기</title>
</head>
<style>
	p:first-letter{
		font-size: 25px;
		background-color: gray;
		color:white;
		font-weight:bold;
	}
	h1:before{
		content: "☆";
		color: blue;
	}
	h1:after{
		content: "★";
		color: blue;
	}
</style>
<body>
	<h1>Famous Saying</h1>
	<p>The world is a beautiful book, but of little use to him who cannot read it.</p>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>가상 클래스 선택자로 방문 사이트 색상 바꾸기</title>
<style>
	a:link{color:blue;}
	a:visited{color:red;}
	a:hover{color:green;}
	a:active{color:yellow;}
</style>
</head>

<body>
	<a href="http://http://www.namgarambooks.co.kr/" target="_top">남가람북스</a>를 클릭하세요
</body>
</html>
```

<br>

### 2.7. 후손(하위) 선택자

- 후손(하위) 선택자(Descendant Selector)는 특정 태그의 후손 태그를 선택함.
- `선택자 선택자{속성: 값;}`
- 후손(하위) 선택자는 자식과 손자 모두 해당함.
- 선택자와 선택자 사이에 공백(space)이 포함되어야 함.
- `Tag1 Tag2` : Ta1 태그의 후손인 Tag2 태그를 선택함.

<br>

### 2.8. 자식 선택자

- 자식 선택자(Child Selector)는 특정 태그의 자식 태그를 선택함.
- `선택자 > 선택자{속성: 값;}`
- 자식 태그 아래인 손자 태그에는 해당되지 않음.
- `Tag1 > Tag2` : Tag1 태그의 자식인 Tag2 태그를 선택함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>후손 선택자와 자식 선택자의 이해</title>
<style>
	.header h3{ color: red; }
	.footer > h3{ color: blue; }
</style>

</head>
<body>
	<div class="header">
		<h3>test1</h3>
		<div class="hea">
			<h3>test2</h3>
		</div>
	</div>
	
	<div class="footer">
		<h3>test3</h3>
		<div class="foo">
			<h3>test4</h3>
		</div>
	</div>
</body>
</html>
```

<br>

### 2.9. 형제 선택자

- 형제 선택자(Sibling Selector)는 특정 태그의 형제 태그를 선택함.
- `선택자 ~ 선택자{속성: 값;}`

<br>

### 2.10. 인접 형제 선택자

- 인접 형제 선택자(Adjacent Sibling Selector)는 특정 태그의 형제 태그 중 첫 번째 태그를 선택함.
- `선택자 + 선택자{속성: 값;}`

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>형제 선택자와 인접 형제 선택자의 이해</title>
<style>
	h1+h2{ color: red;} 
	h1~h2{ background-color: yellow;}
	h1~h2:hover{ color: blue;} 
</style>
</head>
<body>
	<h1>Have a nice day</h1>
	<h2>Have a nice day</h2>
	<h2>Have a nice day</h2>
	<h3>Have a nice day</h3>
	<h2>Have a nice day</h2>
</body>
</html>
```

- 아래는 지금까지 배운 내용의 응용 코드

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>가상 선택자</title>
<style>
	hr{background-color: gray;}
	ol#coun li{color:green;}
	ol#coun li:hover{color:orange; font-weight:bold;}
</style>
</head>
<body>
	** 여행하고 싶은 나라 **
	<hr>
	<ol id="coun">
		<li>중국</li>
		<li>일본</li>
		<li>미국</li>
		<li>호주</li>
	</ol>
	** 여행하고 싶은 섬 **
	<hr>
	<ol id="island">
		<li>제주도</li>
		<li>하와이</li>
		<li>오키나와</li>
		<li>발리</li>
	</ol>
</body>
</html>
```

<br>

### 2.11. 그룹 선택자

- 그룹 선택자는 콤마(,)를 이용해서 여러 선택자들을 묶어서 선택함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>그룹으로 묶은 태그에 스타일 적용</title>
<style>
	h1, div, p {
	  color: green;
	  font-size:25px;
	  font-weight: bold;
	  font-family: "나눔고딕";
	}
</style>
</head>
<body>
	<h1>123</h1>
	<div>가나다</div>
	<p>ABC</p>
</body>
</html>
```

- 선택자 우선순위(위쪽이 우선순위 높음)
  - 인라인 선택자
  - 아이디 선택자
  - 클래스 선택자
  - 태그 선택자

- 스타일이 충돌하는 경우 우선순위에 따라 적용됨.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>선택자의 우선순위</title>
<style type="text/css">
	p{font-size: 25px; font-weight: bold; color:#8041D9;}
	.t1{font-size: 15px; font-family: 휴먼옛체,맑은고딕; color:rgb(255,0,221);}
	#t2{font-size: 35px; font-family: "Times New Roman","Comic Sans MS"; color:gray;}
</style>
</head>
<body>
	<h1>[CSS를 이용한 폰트]</h1>
	<p class="t1"  id="t2">Good Morning</p>
	<p class="t1">안녕하세요</p>
	<p class="t1" id="t2" style="color:red; font-size:50px;">Good Morning</p>
	<p>안녕하세요</p>
	<strong class="t1" id="t2">안녕하세요</strong>
</body>
</html>
```

<br>

## 3. CSS3 주요 스타일 속성

### 3.1. 문자 스타일 속성

- 문자(font)에 관련된 스타일을 지정할 때 사용하는 속성.
- 글꼴의 종류, 크기, 굵기, 색상, 대문자, 소문자 사용 여부 등이 있음.
- 문자 스타일 속성 종류
  - font-family : 글꼴 지정.
  - font-size : 문자 크기 설정.
  - font-style : 문자 기울기 지정.
    - font-style : normal | italic | oblique | initial | inherit
  - font-weight : 문자 굵기 설정.
    - font-weight : normal | bold | bolder | lighter | initial | inherit
  - font-variant : 작은 대문자 표시.
    - font-variant : normal | small-caps | initial | inherit
  - line-height : 줄 간격 조절
  - font : font 속성을 한 번에 기술

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문자 스타일 속성</title>
<style>
	.fs{ 
		font-size: 25px ;
		line-height:50px;
	}
	#f1{ font-variant: small-caps; }
	#f2{ font-style: italic; }
	#f3{ font-family: "Comic Sans MS" ; }
	#f4{ font-weight: bold; }
</style>
</head>
<body>
	<div class="fs">Rather be dead than cool.</div>
	<div class="fs" id="f1">Rather be dead than cool.</div>
	<div class="fs" id="f2">Rather be dead than cool.</div>
	<div class="fs" id="f3">Rather be dead than cool.</div>
	<div class="fs" id="f4">Rather be dead than cool.</div>
</body>
</html>
```

<br>

#### 절대 크기

- 절대 크기는 고정된 크기로 장치에 따라 크기를 조절할 수 없음.
- pixel(px)에서 1px은 컴퓨터의 화소(점 1개)와 같으며, 주로 컴퓨터 화면에서 사용함.
- point(pt)는 주로 인쇄 매체에서 사용함.
- medium이 기본값으로 12pt=19px=13m=100%
- 절대(고정) 크기의 단위 종류
  - in
  - cm
  - mm
  - pt : 1포인트=1/72인치
  - pc : 1파이카=1/12포인트
  - medium : 16px=12pt=1em
  - large : 18px=1.125em
  - small : 13px = 0.812em
  - x-large : 24px = 1.5em
  - xx-large : 32px = 2em
  - x-small : 10px = 0.625em
  - xx-small : 9px = 0.5625em

<br>

#### 상대 크기

- 상대 크기는 앞 요소에 대한 상대적인 크기로 장치에 따라 크기 조절이 가능함.
- em은 웹 문서에서 사용하는 단위. 1em=12pt=16px=100%
- percent(%)는 em처럼 모바일과 태블릿 등 다른 기기에서 상대적으로 크기를 조절할 수 있음.
- 상대(비례) 크기의 단위 종류
  - em : 기준 문자의 높이
  - % : 기준 글꼴 크기의 백분율 크기
  - larger : 앞에 입력한 문자보다 크게
  - ex : 기준 문자의 소문자 높이
  - px : 1픽셀을 1로 하는 단위
  - smaller : 앞에 입력한 문자보다 작게

<br>

### 3.2. 문단 스타일 속성

- 문단(Paragraph)에 관련된 스타일을 지정할 때 사용하는 속성.

- 문단 스타일 속성 종류

  - text-decoration : 문자에 밑줄 또는 취소선 등을 넣음.
    - none | underline | overline | line-through | inherit
  - text-align : 문단 정렬.
    - center | left | right | justify
  - text-indent : 문단 들여쓰기 또는 내어 쓰기 지정
  - text-transform : 영문자 모두 대문자로 또는 소문자로 변경
    - none | capitalize | uppercase | lowercase | inherit
  - letter-spacing : 글자 간의 간격 조절
  - word-spacing : 단어 간의 간격 조절
  - vertical-align : 문자와 문자간의 수직 정렬을 조절.
    - baseline | top | middle | bottom
  - white-space : 줄바꿈에 대한 설정
    - normal | nowrap | pre | pre-line | pre-wrap | inherit

  ```html
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>문단 스타일 속성</title>
  <style>
  	div{ height: 40px; border: 1px dotted #d5d5d5; }
  	div.a{ text-transform: capitalize; }
  	div.b{ text-align: right;}
  	div.c{ text-decoration: underline overline;
  		   text-decoration-color: red;
  		   text-decoration-style: dotted;
  	}
  	div.d{ text-indent: 50px;}
  	div.e{ letter-spacing:10px; }
  	div.f{ word-spacing: 30px; }
  </style>
  </head>
  <body>
  	<div class="a">text-transform: capitalize</div>
  	<div class="b">text-align: right</div>
  	<div class="c">text-decoration color style</div>
  	<div class="d">text-indent: 50px</div>
  	<div class="e">letter-spacing:10px</div>
  	<div class="f">word spancing 30px</div>
  </body>
  </html>
  ```

<br>

### 3.3. 테두리 스타일 속성

- 테두리(Border)에 관련된 스타일을 지정할 때 사용하는 속성.
- 테두리 스타일 속성 종류
  - border-width : 선의 굵기를 지정. px, em 등 단위 사용. px 주로 사용.
  - border-style : 선의 형태 지정.
    - none | solid | dotted | dashed | double | groove 등
  - border-color : 선의 색상을 지정. 색상 이름이나 색상 코드 들어갈 수 있음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>테두리 스타일 속성</title>
<style>
	body > div{
		position:relative; 
		text-align:center;
		overflow:hidden;	
	}
	.box{width:70px; height:70px; float:left; color:black; position:relative; background-color:white;}
	.box:nth-child(1){left:10px; border-style : none;}
	.box:nth-child(2){left:20px; border-style : solid;}
	.box:nth-child(3){left:30px; border-style : dotted;}
	.box:nth-child(4){left:40px; border-style : dashed;}
	.box:nth-child(5){left:50px; border-style : double;}
	.box:nth-child(6){left:60px; border-style : groove;}
</style>
</head>
<body style=" background-color:#FAED7D;">
	<div class="box">none</div>
	<div class="box">solid</div>
	<div class="box">dotted</div>
	<div class="box">dashed</div>
	<div class="box">double</div>
	<div class="box">groove</div>
</body>
</html>
```

- 각각 순서대로 선 없음, 실선, 점선, 굵은 점선, 이중 실선, 패인 실선을 표현함.

<br>

### 3.4. 리스트 스타일 속성

- 리스트 스타일은 ol, ul 요소에 스타일을 설정하는 속성.
- 리스트 스타일은 한계가 있기 때문에 많이 사용하지 않음.
- 리스트 속성 종류
  - list-style-type : 리스트 앞에 오는 불릿의 타입을 결정하는 속성.
    - none | disc | circle | square | decimal | lower-alpha | upper-alpha | lower-roman | upper-roman
  - list-style-image : 리스트 불릿을 이미지로 사용하고자 할 때 사용함.
  - list-style-position : 리스트의 불릿이 밖에 있을지 안에 있을지를 결정함.
    - outside | inside | inherit
  - list-style : 리스트 속성들을 한 번에 사용할 수 있음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>리스트 스타일 속성</title>
<style>
	ol{
		list-style-type: upper-alpha;
		list-style-position:inside;
	}
	ul{
		/* list-style-type: circle; */
		list-style-image: url("image/icon.png");
	}
</style>
</head>
<body>
	<h2>가지고 있는 물건</h2>
	<ul>
		<li>시계
		<li>가방
		<li>핸드폰
	</ul>
	
	<h2>가지고 싶은 물건</h2>
	<ol>
		<li>타블렛
		<li>명품가방
		<li>보석
	</ol>
</body>
</html>
```

<br>

### 3.5. 배경 스타일 속성

- 배경 스타일은 영역에 색상 또는 이미지를 배경으로 설정함.
- 배경 속성 종류
  - background-color : 배경에 색상을 넣는 속성
  - background-image : 배경에 이미지를 넣는 속성
    - background-image : url('경로')
  - background-repeat : 배경 이미지 반복 여부 설정하는 속성.
    - repeat | repeat-x | repeat-y | no-repeat | inherit
  - background-position : 이미지의 위치를 설정하는 속성.
    - px | % | left, right | top, bottom | center | inherit
  - background-attachment : 화면을 스크롤 할 때 배경 이미지가 어떻게 동작하는지를 설정하는 속성
    - scroll | local | fixed | inherit

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>배경 스타일 속성</title>
<style>
	div{
	   width:300px;
	   height:300px;
	   border:1px solid black;
	   background-color:#d5d5d5;
	   background-image:url("image/cat.png");
	   background-repeat: no-repeat;
	   background-position: bottom center;
	}
</style>
</head>
<body>
	<div></div>
</body>
</html>
```

<br>

### 3.6. 테이블 스타일 속성

- 테이블 스타일 속성은 테이블에 관련 속성을 지정할 때 사용함.
- 테이블의 속성 종류
  - border-collapse : 테이블 셀 간의 간격 여부를 설정
    - separate | collapse | inherit
  - border-spacing : 테이블 셀 간의 공백 크기를 조절해 주는 속성
    - separate인 상태에서만 가능.
  - empty-cells : 테이블에서 빈 셀에 대해 그 셀의 테두리나 배경을 브라우저에 보여줄지 보여주지 않을지를 결정.
    - separate인 상태에서만 가능. hide | show | inherit
  - table-layout : 셀의 너비를 고정 또는 가변으로 할지를 설정함.
    - auto | fixed | inherit
  - vertical-align : 셀 안에서 수직 정렬함
    - top | middle | bottom
  - caption-side : 캡션의 위치를 정하는 속성임
    - top | bottom | initial | inherit

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>미세먼지 농도 테이블 만들기</title>
<style>
	table{
	    width: 100%;
	    border-collapse: collapse;
	    font-size: 14px;
	    text-align: center;
	}
	 table > caption{
	    font-size: 20px;
	    font-weight:700;
	    margin-bottom: 10px;
	}
	table td, table th{
	    padding: 8px 5px;
	    border: 1px solid #ccc;
	}
	.blue{
	    color: white;
	    background: #43cbff;
	}
	.green{
	    color: white;
	    background: #81d733;
	}
	.yellow{
	    background: yellow;
	}
	.orange{
	    background: orange;
	}
	.red{
	    color: white;
	    background: red;
	}
</style>
</head>
<body>
<table>
	<caption>미세먼지 농도</caption>
	<thead>
		<tr>
			<th colspan="2">예보구간</th>
			<th class="blue">좋음</th>
			<th class="green">보통</th>
			<th class="yellow">약간나쁨</th>
			<th class="orange">나쁨</th>
			<th class="red" colspan="2">매우나쁨</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td colspan="2">예측 농도</td>
			<td>0~30</td>
			<td>31~80</td>
			<td>81~120</td>
			<td>121~200</td>
			<td>201~300</td>
			<td>301 ~</td>
		</tr>
		<tr>
			<td rowspan="2">행동 요령</td>
			<td>노약자</td>
			<td></td>
			<td></td>
			<td>장시간 실외 활동 가급적 자제</td>
			<td>무리한 실외활동 자제요청 (특히 호흡기, 심질환자, 노약자)</td>
			<td>실외활동 제한</td>
			<td>실내생활</td>
		</tr>
		<tr>
			<td>일반</td>
			<td></td>
			<td></td>
			<td></td>
			<td>장시간 무리한 실외활동 자제</td>
			<td>실외활동 자제</td>
			<td>실외활동 자제</td>
		</tr>
	</tbody>
</table>
</body>
</html>
```

<br>

### 3.7. 박스 모델 속성

- 박스 모델(Box Model)이란 웹페이지에서 텍스트 문장이나 이미지 등의 콘텐츠를 만들 때 사용함.
- 네모난 박스의 형태로 4개의 영역을 구성하고 각각 패딩(padding), 테두리(border), 여백(margin), 콘텐츠(content)로 구성함.
- 박스 모델은 CSS의 레이아웃을 만들 때 가장 기본이 됨.
- 여백(margin) 속성은 테두리와 이웃하는 다른 요소 사이의 간격의 크기를 설정함.
- 여백은 패딩 영역처럼 background-color 속성으로 배경색의 영향을 받지 않음.
- 여백의 속성 종류
  - margin
  - margin-top, right, bottom, left
- margin 속성값을 auto로 설정하면 웹브라우저에서 오른쪽과 왼쪽 등 수평 방향의 여백 값을 자동으로 설정함.
- 패딩(padding)은 내용과 테두리 사이의 영역을 설정함.
- 패딩 영역은 background-color 속성으로 설정하는 배경색으로도 가능함.
- 패딩의 속성 종류
  - padding
  - padding-top, right, bottom, left
- padding 속성을 사용하면 top을 기준으로 시계방향으로 값이 설정됨.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문단에 여백 주기</title>
<style>
	span {
		border: 2px solid teal;
	}
	span.pad {
		padding:50px 20px 50px 20px;
 		/*padding-top: 50px;
		padding-right: 20px;
		padding-bottom: 50px;
		padding-left: 20px;
		 */
	}
</style>
</head>
<body>
	<span>padding 속성을 설정하지 않은 요소</span><br><br><br><br>
	<span class="pad">padding 속성값을 방향별로 따로 설정한 요소</span>
</body>
</html>
```

- 테두리(border) 속성은 내용과 패딩 영역을 둘러싸는 테두리의 스타일을 설정함.
- 테두리의 속성 종류
  - border
    - 태그의 테두리를 설정하는 속성.
    - 세부적인 속성들을 한 번에 쓸 수 있음.
    - (border: border-width border-style border-color)
  - border-style
  - border-width
  - border-color
  - border-top
  - border-right
  - border-bottom
  - border-left

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>테두리 꾸미기</title>
<style>
	div {
		background-color: darkgray;
		color: white;
		width: 450px;
		padding: 30px;
		border: 20px solid teal;
		margin: 50px;
	}
</style>
</head>
<body>
	<div>Alice in Wonderland<br><br>
		When Alice follows the White Rabbit down a rabbit hole, she finds herself in a land where she can change her size. She meets a Duchess with a pig for a baby, a Cook that throws spoons, a wise but mad Cheshire cat, the March Hare, Mad Hatter and a Dormouse. A Caterpillar shows her how to adjust her height by eating from the right and left sides of a mushroom.
	</div>
</body>
</html>
```

<br>

### 3.8. CSS 디버깅과 개발자 도구

- 의도치 않은 형태로 화면이 출력될 경우 코드의 내용만 보고 문제를 찾아내기 어려움. 웹브라우저들은 디버깅을 위한 '개발자 도구'를 내장하고 있음.
- 크롬 브라우저는 F12

<br>

## 4. 디자인을 위한 고급 스타일 효과

- CSS는 화면을 고급스럽게 꾸밀 수 있게 고급 스타일을 지원함. 그림자 효과, 그래디언트, 내비게이션, 드롭 박스, 갤러리, 썸네일, 툴팁 등을 사용할 수 있음.

<br>

### 4.1. Shadow와 Gradient

- 이미지나 텍스트에 그림자(Shadow) 효과를 주고 싶을 때 CSS3의 text-shadow 속성 또는 box-shadow 속성을 사용함.
- text-shadow와 box-shadow의 속성값
  - h-shadow : 수평 그림자 위치. 값이 +이면 오른쪽으로 생김.
  - v-shadow : 수직 그림자 위치. 값이 +이면 아래쪽으로 생김.
  - blur-radius : 흐림 반경. 0이 가장 진하고 숫자 높아질수록 점점 흐려짐.
  - spread-distance : 확산 거리. 값이 +이면 그림자의 크기가 확대됨.
  - color : 컬러

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>그림자 만들기</title>
<style>
	.shadow1 { text-shadow: 2px 2px; }
	.shadow2 { text-shadow: 2px 2px 10px ; }
	.shadow3 { text-shadow: 0 0 3px red; }
	.shadow4 { text-shadow: 2px 2px 10px green; }
	.shadow5 { color: white; text-shadow: 1px 1px 2px black, 0 0 20px purple, 0 0 5px maroon; }
	</style>
</head>
<body>
	<div class="shadow1">Black Shadow Effect</div><br>
	<div class="shadow2">Cloudy Shadow Effect</div><br>
	<div class="shadow3">Neon Effect</div><br>
	<div class="shadow4">Color Shadow Effect</div><br>
	<div class="shadow5">Shadow Overlay Effect</div>
</body>
</html>
```

- 그래디언트(Gradient)는 2가지 이상의 색상을 혼합하여 부드러운 색감으로 배경 등을 표현하는 것.
- 이미지를 사용하지 않고 div 또는 특정 태그의 배경에 그래디언트 효과를 넣을 수 있음.
- 그래디언트의 종류
  - 선형 그래디언트(linear gradients)
    - 선형으로 그래디언트 효과를 나타냄.
    - 최소한 두 개 이상의 색상 지정점이 필요함.
    - background : linear-gradient (direction, color1, color2 ...);
    - background : linear-gradient (angle, color1, color2 ...);
  - 원형 그래디언트(radial gradients)
    - 원 또는 타원의 중심에서부터 바깥쪽으로 색상이 바뀜.
    - background : radial-gradient(모양/크기 및 위치, 출발색, 중간색 ... 종료색)
  - direction은 방향, to bottom은 위에서 아래로(기본값) 그래디언트를 만듦.
  - angle은 각도(0deg, 180deg 등)로 방향을 설정해서 그래디언트를 만듦.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>선형 그래디언트</title>
<style>
	.grad{
	  width: 200px;
	  height: 100px;
	  background: linear-gradient( to bottom, yellow, red );
	}
</style>
</head>
<body>
	<div class="grad"></div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>원형 그래디언트</title>
<style>
	.circle {
	  width: 200px;
	  height: 200px;
	  background: radial-gradient(circle, yellow, green);
	}                              /*원모양    출발색      끝색*/
	.ellipse {
	  width: 300px;
	  height: 100px;
	  background: radial-gradient(ellipse,yellow, green);
	}                              /*타원모양   출발색      끝색*/
</style>
</head>
<body>
	<div class="circle"></div><br>
	<div class="ellipse"></div>
</body>
</html>
```

<br>

### 4.2. Navigation과 Dropdown

- Navigation과 Dropdown은 웹페이지의 메뉴바를 만들 때 주로 이용함.
- 메뉴바는 상단 또는 왼쪽에 만듦.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>메뉴 바 만들기</title>
<style>
	body {
	    margin: 0;
	}
	.header {
	    padding: 5px;
	    text-align: center;
	    background-image:url("image/back.png");
	    border-bottom: 1px solid #bdbdbd;
	    color:navy;
	}
	ul {
	    list-style-type: none;
	    margin: 0;
	    padding: 0;
	    width: 100px;
	    background-color: white;
	    position: fixed;
	    height: 100%;
	    overflow: auto;
	}
	li a {
	    display: block;
	    color: #000;
	    padding: 8px 16px;
	    text-decoration: none;
	}
	li a:hover{
	    background-color: #555;
	    color: white;
	}
</style>
</head>

<body>
	<div class="header">
		<h1>Web Site Name</h1>
	</div>
	<ul>
		<li><a href="#main">메일</a></li>
		<li><a href="#cafe">카페</a></li>
		<li><a href="#blog">블로그</a></li>
		<li><a href="#shopping">쇼핑</a></li>
	</ul>
</body>
</html>
```

- 28라인에서 display를 블록으로 처리. 요소 하나하나를 블록으로 처리하기 때문에 줄을 바꿔서 다음 요소를 출력함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>드롭 다운 메뉴바 만들기</title>
<style>
	ul {
	    list-style-type: none;
	    margin: 0;
	    padding: 0;
	    overflow: hidden;
	    background-color: rgb(53,94,169);
	    /* background-color:  #999 ; */
	}
	li {
	    float: left;
	}
	li a, .dropbtn {
	    display: inline-block;
	    color: white;
	    text-align: center;
	    padding: 14px 16px;
	    text-decoration: none;
	}
	li a:hover, .dropdown:hover .dropbtn {
	    /* background-color: red; */
	    background-color: #6799FF;
	}
	li.dropdown {
	    display: inline-block;
	}
	li a.active, a.active:hover{
	    /* background-color: #4CAF50; */
	    background-color: #6799FF;
	}
	.dropdown-content {
	    display: none;
	    position: absolute;
	    background-color: #f9f9f9;
	    min-width: 160px;
	    box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
	    z-index: 1;
	}
	.dropdown-content a {
	    color: black;
	    padding: 12px 16px;
	    text-decoration: none;
	    display: block;
	    text-align: left;
	}
	.dropdown-content a:hover {background-color: #f1f1f1}
	.dropdown:hover .dropdown-content {
	    display: block;
	}
</style>
</head>
<body>
	<ul>
		  <li><a href="#home">Home</a></li>
		  <li><a href="#mail">메일</a></li>
		  <li><a href="#cafe">카페</a></li>
		  <li><a href="#blog">블로그</a></li>
		  <li class="dropdown">
			    <a href="#shopping" class="dropbtn">쇼핑</a>
			    <div class="dropdown-content">
				      <a href="#">가전제품</a>
				      <a href="#">의류/악세사리</a>
				      <a href="#">가방/신발</a>
			    </div>
		  </li>
		  <li class="dropdown">
			    <a href="#new" class="dropbtn">뉴스</a>
			    <div class="dropdown-content">
			      <a href="#">정치</a>
				      <a href="#">경제</a>
				      <a href="#">생활/문화</a>
			    </div>
		  </li>
		  <li style="float:right"><a class="active" href="#login">로그인</a></li>
	</ul>
</body>
</html>
```

<br>

### 4.3. Gallery

- 갤러리(Gallery)는 이미지와 그림자를 이용하여 폴라로이드 효과를 만듦.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>이미지를 이용한 폴라로이드 효과 주기</title>
<style>
	body {margin:25px;}
	div.title{
		width: 300px;
		text-align:center;
	}
	div.polaroid {
	  width: 300px;
	  height: 300px;
	  background-color: white;
	  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.3), 0 6px 20px 0 rgba(0, 0, 0, 0.15);
	  margin-bottom: 15px;
	}
	div.container {
	  text-align: center;
	  padding: 5px 5px;
	}
	img{
		width:100%;
		height:230px;
	}
</style>
</head>
<body>
	<div class="title"><h2>따라 그리는 명화</h2></div>
	<div class="polaroid">
		<img src="image/pic1.jpg" alt="WorkOfArt_1">
		<div class="container">
			<p>카드놀이 하는 사람들 (미니 作)</p>
		</div>
	</div>
	  
	<div class="polaroid">
		<img src="image/pic2.jpg" alt="WorkOfArt_2">
		<div class="container">
			<p>길가의 집 (미니 作)</p>
		</div>
	</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>이미지 갤러리 만들기</title>
<style>
	div.gallery {
	    margin: 5px;
	    border: 1px solid #ccc;
	    float: left;
	    width: 180px;
	}
	
	div.gallery:hover {
	    border: 1px solid #777;
	    opacity: 0.3;
	}
	
	div.gallery img {
	    width: 100%;
	    height: auto;
	}
	
	div.desc {
	    padding: 15px;
	    text-align: center;
	}
</style>
</head>
<body>
	<div class="gallery">
	  <a target="_blank" href="image/01.jpg">
	    <img src="image/01.jpg" alt="evening glow" width="300" height="200">
	  </a>
	  <div class="desc">Evening Glow</div>
	</div>
	
	<div class="gallery">
	  <a target="_blank" href="image/02.jpg">
	    <img src="image/02.jpg" alt="lakeside" width="600" height="400">
	  </a>
	  <div class="desc">Lake Side</div>
	</div>
	
	<div class="gallery">
	  <a target="_blank" href="image/03.jpg">
	    <img src="image/03.jpg" alt="sea" width="600" height="400">
	  </a>
	  <div class="desc">Southern Sea</div>
	</div>
	
	<div class="gallery">
	  <a target="_blank" href="image/04.jpg">
	    <img src="image/04.jpg" alt="coast road" width="600" height="400">
	  </a>
	  <div class="desc">Coast Road</div>
	</div>
</body>
</html>
```

<br>

## 5. CSS3 레이아웃

- 레이아웃이란 웹사이트에서 정보와 내용을 보기 편하게 한 화면에 영역을 나누고 디자인하는 것임.
- 다양한 해상도에 대응할 수 있는 레이아웃이 필요함.

<br>

### 5.1. 크로스 브라우징

- 크로스 브라우징(Cross Browsing)이란 사용자의 기기나 시스템이 각각 달라도 최대한 비슷하게 표현하도록 공통 요소를 사용해서 웹페이지를 개발하는 기법.
- 웹 표준 기술을 이용해서 특정 기종(또는 특정 브라우저)에만 최적화되지 않도록 하는 것.

- 모든 사용자가 인터넷의 정보로부터 소외되지 않도록 웹 표준 기술로 개발해야 함.

<br>

### 5.2. 레이아웃의 종류

- 레이아웃은 웹페이지의 영역을 Header, Footer, Menu, Content로 나누는 것.
- 고정형 레이아웃, 유동형 레이아웃, 반응형 레이아웃이 있음.

<br>

#### 고정형 레이아웃

- 고정형 레이아웃이란 전체 페이지의 너비를 정하고 콘텐츠에 따라 단을 나누어 정렬하는 방식.
- 데스크톱 위주의 웹페이지에서 모니터 해상도를 고려해 너비가 고정된 경우에 사용함.
- 고정형 레이아웃은 고정된 너비에 콘텐츠를 가운데 배치함.

- 고정형 레이아웃은 텍스트의 영역을 좁게 만들어서 가독성을 좋게 함.
- 여백을 짜임새 있게 구성하고 콘텐츠에 포함할 내용을 표현하기 때문에 디자인하기 쉬움.
- 창 너비를 고정 너비보다 크게 조정하면 웹페이지는 가운데 정렬되기 때문에 양쪽에 빈 공백이 생기고 공간을 낭비할 수 있음.

<br>

#### 유동형 레이아웃

- 어떤 해상도의 모니터라도 좌우 공백 없이 콘텐츠 영역이 모니터 화면을 다 채우도록 하는 방식.
- 너비를 비율로 설정하여 창 너비에 따라 콘텐츠 크기가 늘어나거나 줄어듦.
- 이미지가 없는 페이지에서 주로 사용함.
- 요즘은 일정 범위까지만 유동형 레이아웃으로 줄어들고 설정 범위 이내로 줄어들면 고정형 레이아웃으로 전환하도록 사용함.

<br>

#### 반응형 레이아웃

- 기기에 따라 사용자 화면에 맞게 레이아웃이 변하는 것을 말함.
- 규모가 작은 웹사이트를 만들 때 효율적.
- 고정적인 형태의 콘텐츠를 갖고 있는 정적인 사이트에 적합함.

<br>

### 5.3. CSS 레이아웃 속성

- 레이아웃에는 요소들의 출력 형태나 위치를 설정하는 속성들이 있음.
- 속성의 종류는 overflow, display, visibility, position, z-index, float, clear가 있음.

<br>

#### overflow

- overflow 속성은 박스 모델 안의 내용이 박스의 범위를 초과할 때 어떤 형태로 출력할지를 결정함.
  - visible : 범위 초과해도 그대로 표현 (기본값)
  - hidden : 내용 숨김
  - scroll : 스크롤바 생성
  - auto : 내용이 박스 벗어날 때만 스크롤 생김
  - inherit

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>레이아웃 속성</title>
<style>
	#visible, #hidden, #scroll, #auto
	{
		width:400px;
		height:150px;
		background-color:#FAF4C0;
		border: 1px solid black;
		margin-bottom: 100px;
	}
	#visible{overflow: visible; }
	#hidden{overflow: hidden; }
	#scroll{overflow: scroll; }
	#auto{overflow: auto;}
</style>
</head>
<body>
	<div id="visible">
		<h2>1.overflow:visible</h2>
		When Alice follows the White Rabbit down a rabbit hole, she finds herself in a land where she can change her size. She meets a Duchess with a pig for a baby, a Cook that throws spoons, a wise but mad Cheshire cat, the March Hare, Mad Hatter and a Dormouse. A Caterpillar shows her how to adjust her height by eating from the right and left sides of a mushroom. With that knowledge,
	</div>
	
	<div id="hidden">
		<h2>2.overflow:hidden</h2>
		When Alice follows the White Rabbit down a rabbit hole, she finds herself in a land where she can change her size. She meets a Duchess with a pig for a baby, a Cook that throws spoons, a wise but mad Cheshire cat, the March Hare, Mad Hatter and a Dormouse. A Caterpillar shows her how to adjust her height by eating from the right and left sides of a mushroom. With that knowledge,
	</div>
	
	<div id="scroll">
		<h2>3.overflow:scroll</h2>
	</div>
	
	<div id="auto">
		<h2>4.overflow:auto</h2>
		When Alice follows the White Rabbit down a rabbit hole, she finds herself in a land where she can change her size. She meets a Duchess with a pig for a baby, a Cook that throws spoons, a wise but mad Cheshire cat, the March Hare, Mad Hatter and a Dormouse. A Caterpillar shows her how to adjust her height by eating from the right and left sides of a mushroom. With that knowledge,
	</div>
</body>
</html>
```

<br>

#### display와 visibility

- HTML은 \<div>\<h>와 같은 블록 요소와 \<span>\<a>와 같은 인라인 요소가 있음.
- 이 요소들은 각각 display 속성을 가지고 있으며 inline 또는 block으로 변경이 가능함.
- visibility는 요소가 보이거나 안보이게 하는 속성임.
  - display: none은 메모리에 존재하지 않기 때문에 보이지 않음.
  - visibility: hidden은 메모리에 존재는 하지만 hidden으로 처리는 것.
- 레이아웃의 속성
  - display : inline | block | none | inline-block
  - visibility : visible | hidden | collapse
- display는 해당 HTML 요소가 웹브라우저에서 어떻게 표현할지를 결정함.
  - block은 줄바꿈 한 후 새로운 라인의 전체 영역(너비)를 확보함.
  - inline은 새로운 라인에서 시작하지 않고 너비도 해당 내용만큼만 영역을 확보함.
  - none은 박스를 생성하지 않음.
  - inline-block은 inline을 block처럼 만들어서 바로 옆에 나열함.
- block이 기본인 요소 : div, h, p, ul, ol, form
- inline이 기본인 요소 : span, a, img
- visibility
  - visible(기본값)은 요소를 보여줌.
  - hidden은 요소를 보여주지 않음.
  - collapse는 테이블 요소에서만 쓸 수 있음. 필요에 따라서 공백을 없애고 테이블 레이아웃을 자연스럽게 보여줌.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>블록과 인라인</title>
<style>
	span
	{
		width: 100px;
		height: 100px;
		display: none;
	}
	#x {
		background-color:#FFE400;
	}
	#y {
		background-color:#B5B2FF;
		/* visibility: hidden; */
	}
	#z {
		background-color:#D9418C;
	}
</style>
</head>
<body>
	웹 표준은 <span id="x">HTML</span>구조,
	<span id="y">CSS</span>디자인,
	<span id="z">JavaScript</span>동작이 있습니다.
</body>
</html>
```

- display 값을 none, inline, block, inline-block 바꿔가며 변화하는 모습을 확인하기.

<br>

#### position과 z-index

- position은 CSS에서 특정 요소의 위치를 지정하는 속성.
  - static : (기본값)무조건 요소를 순서대로 배치. 위치 지정해도 적용되지 않음.
  - relative : 직전 요소의 상대 위치에 배치
  - absolute : 문서(또는 상위 요소) 내에서 절대 위치에 배치
  - fixed : 현재 보이는 브라우저 화면 내에서 절대 위치에 배치
- z-index를 이용해서 요소를 앞쪽으로 배치하거나 뒤쪽으로 배치할 수 있음.
- z-index는 position 속성을 적용한 요소에서만 동작함.
- z-index는 정수값을 사용해서 배치 순서를 정함. 값이 클수록 앞쪽에, 작을수록 뒤쪽에 배치함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>z-index로 요소의 우선순위 설정</title>
<style>
	.box{
		width:100px; height:100px;
		position:fixed;     
	}
	.box:nth-child(1) {
		background-color:red;
		left:10px;  top:10px;	
		z-index:1;
	}
	.box:nth-child(2) {
		background-color:green;
		left:50px;  top:50px; 	
		z-index:2;
	}
	.box:nth-child(3) {
		background-color:blue;
		left:90px;  top:90px;
		
		z-index:3;
	}
</style>
</head>
<body>
<div>
	<div class="box"></div>
	<div class="box"></div>
	<div class="box"></div>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>position으로 요소의 위치 설정</title>
<style>
	body > div{	
		position:absolute;  
		top:150px;
		left:150px;
		height: 300px;
		width: 300px;
		border: 1px dotted black;
		overflow:hidden;	
	}
	.box{
		width:100px; height:100px;
		position:absolute;
	}
	.box:nth-child(1) {
		background-color:red;
	}
	.box:nth-child(2) {
		background-color:green;
		left:100px;
		top:100px;
	}
	.box:nth-child(3) {
		background-color:blue;
		left:200px;
		top:200px;
	}
</style>
</head>

<body>
<div>
	<div class="box"></div>
	<div class="box"></div>
	<div class="box"></div>
</div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>position으로 요소 배치</title>
<style>
	.t1{position: static;}
	.t2{position: static; top:100px; left: 150px;}
	.t3{position: relative; top:100px; left: 100px;}
	.t4{position: absolute; top:200px; left: 200px;}
	.t5{position: fixed; top:30px; right: 50px;}
</style>
</head>
<body>
	<img src="image/americano.jpg" class="t1" width="100" height="100">
	<img src="image/cafelatte.jpg" class="t2" width="100" height="100">
	<img src="image/americano.jpg" class="t3" width="100" height="100">
	<img src="image/cafelatte.jpg" class="t4" width="100" height="100">
	<img src="image/americano.jpg" class="t5" width="100" height="100">
</body>
</html>
```

<br>

#### float와 clear

- float 속성은 본문 중간에 이미지를 넣고자 할 때 주로 사용함.
- 이미지가 본문 내용 중간에서 왼쪽이나 오른쪽으로 위치하고 텍스트가 자연스럽게 그 옆에 어우러지도록 배치할 수 있음.
- clear는 float를 사용해서 이미지를 넣었다가 이미지 옆에 아닌 아래에서 새로 시작하고 싶을 때 사용함.
- float 종류
  - none : (기본값) 플로팅 박스 적용 없이 순서대로 배치함.
  - left : 플로팅 박스를 왼쪽 경계에 배치하며 주변 콘텐츠가 오른쪽에 나타남.
  - right : 플로팅 박스를 오른쪽 경계에 배치하며 주변 콘텐츠가 왼쪽에 나타남.
  - initial
  - inherit
- clear 종류
  - none
  - left : 왼쪽 요소 취소
  - right : 오른쪽 요소 취소
  - both : 모든 float(left, right) 취소

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>float를 이용한 배치</title>
<style>
	img {
	    float: left;
	}
</style>
</head>
<body>
	<p><img src="image/house.jpg" alt="house" style="width:170px;height:170px;margin-right:15px;">
	When Alice follows the White Rabbit down a rabbit hole, she finds herself in a land where she can change her size. She meets a Duchess with a pig for a baby, a Cook that throws spoons, a wise but mad Cheshire cat, the March Hare, Mad Hatter and a Dormouse. A Caterpillar shows her how to adjust her height by eating from the right and left sides of a mushroom. With that knowledge, Alice is able to enter a beautiful garden to play croquet with the Queen of Hearts. This game of croquet is played with flamingos (mallets), balls (curled-up hedgehogs) and an army of bent-over cards (arches). As the living croquet game continually changes, the Queen has most of her guests arrested, but introduces Alice to a Gryphon and Mock Turtle. 
	By the end, almost everyone she meets ends up in a courtroom where the King of Hearts wants a verdict before the evidence, jurors pay attention to all the wrong information, witnesses are threatened with punishment and Alice grows tall enough not to be intimidated by a pack of cards, which is what the court and soldiers are. Suddenly Alice wakes up and realizes that her entire adventure was an amazing dream.</p> 
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>float와 clear를 이용한 레이아웃 설정</title>
<style>
	*{
		margin: 0;  padding: 0;  border: 0;
	}
	div.aa{
		width: 80px;
		height: 100px;
		border-style: solid;
		background-color: #FFE400;
		padding: 0px;  
		float: left;
		margin-right: 10px;
	}
	
	div.bb{
		width: 98px;
		background-color: #47C83E;
		clear:both;
	}
	
	div.cc{
		width: 40px;
		height:150px; 
		background-color: #D1B2FF;
		float: left;
	}
</style>
</head>

<body>
	<div class="aa">
		AA
	</div>
	<div class="cc">
		CC
	</div>
	<div class="bb">
		BB
	</div>
</body>
</html>
```

<br>

### 5.4. 웹 표준 검증

- 웹 표준 검증이란 HTML, XML과 같은 웹 문서의 표준 유효성을 검증하는 서비스.
- W3C에서 마크업의 웹 표준검사를 통해서 모든 브라우저가 제대로 표현되는지를 검사함.
- 웹 표준 검증 사이트는 https://validator.kldp.org/
- 검증 방법 3가지
  - URL 입력
  - 파일 업로드
  - 직접 입력
- 웹 표준 검사를 통과하지 못한 경우 빨간 표시줄의 경고 메시지가 출력됨.
- 웹 표준 검사를 통과한 경우 녹색 표시줄에 '이 문서는 HTML5입니다'라는 메시지가 출력됨.

<br>

# References

- [한 권으로 끝내는 실무 Java 웹 개발서 / 김정현, 김계희 / 남가람북스 / 2019](http://www.kyobobook.co.kr/product/detㄴailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025)

<br>