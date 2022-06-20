# CSS3



> 이 문서는 [<한 권으로 끝내는 실무 Java 웹 개발서 - 김정현, 김계희>](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025) 책을 보고 정리한 문서입니다.



## 1. CSS3의 개요

- CSS(Cascading Style Sheets)
- 텍스트의 색상이나 크기 또는 이미지의 크기나 위치 등을 처리하는 웹 문서에서 디자인 요소를 담당하는 언어.
- 자주 사용하는 디자인 부분들을 미리 조합해서 만들어 놓고 HTML 페이지 내부에서 필요할 때마다 적용함.



### 1.1. CSS3의 역사와 특징

- 1996년 W3C가 표준안 CSS1 제정.
- 1998년 CSS2
- 2005년 CSS3
- HTML로는 부족한 레이아웃이나 폰트 등에 다양성을 부여함.
- 폰트 크기, 자간 설정, 행간의 높이, 페이지 여백을 마음대로 조절할 수 있음.
- 웹페이지의 레이아웃을 자유롭게 설계하여 배치할 수 있음.
- 링크할 때 밑줄의 변형도 가능해서 가독성을 높일 수 있음.



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



## 2. 기본적인 CSS3 선택자

- 선택자(Selector)란 특정 태그나 요소를 지칭함.
- 지정한 선택자에 사용자가 의도하는 디자인을 적용함.



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



#### ID와 CLASS의 차이점

- ID
  - ID는 유일한 요소에 스타일을 적용함.
  - 한 문서에서 ID는 한 번만 사용 가능함.
  - 상단 메뉴바, 하단 정보, 홈페이지 로고처럼 유일한 것들을 선택할 때 사용함.
- CLASS
  - CLASS는 '복수' 요소에 스타일을 적용함.
  - 한 문서에서 여러번 사용 가능함.
  - 소제목처럼 반복되는 요소에 사용함.



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



### 2.7. 후손(하위) 선택자

- 후손(하위) 선택자(Descendant Selector)는 특정 태그의 후손 태그를 선택함.
- `선택자 선택자{속성: 값;}`
- 후손(하위) 선택자는 자식과 손자 모두 해당함.
- 선택자와 선택자 사이에 공백(space)이 포함되어야 함.
- `Tag1 Tag2` : Ta1 태그의 후손인 Tag2 태그를 선택함.



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



### 2.9. 형제 선택자

- 형제 선택자(Sibling Selector)는 특정 태그의 형제 태그를 선택함.
- `선택자 ~ 선택자{속성: 값;}`



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





### 3.6. 테이블 스타일 속성







### 3.7. 박스 모델 속성







### 3.8. CSS 디버깅과 개발자 도구









## 4. 디자인을 위한 고급 스타일 효과







### 4.1. Shadow와 Gradient







### 4.2. Navigation과 Dropdown







### 4.3. Gallery





## 5. CSS3 레이아웃







### 5.1. 크로스 브라우징







### 5.2. 레이아웃의 종류







### 5.3. CSS 레이아웃 속성







### 5.4. 웹 표준 검증











































# References

- [한 권으로 끝내는 실무 Java 웹 개발서 / 김정현, 김계희 / 남가람북스 / 2019](http://www.kyobobook.co.kr/product/detㄴailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025)