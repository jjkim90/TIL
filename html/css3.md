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
<title>Insert title here</title>
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
<title>Insert title here</title>
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









### 2.4. 클래스(Class) 선택자







### 2.5. 속성 선택자







### 2.6. 가상 클래스 선택자







### 2.7. 후손(하위) 선택자







### 2.8. 자식 선택자







### 2.9. 형제 선택자







### 2.10. 인접 형제 선택자







### 2.11. 그룹 선택자







## 3. CSS3 주요 스타일 속성





### 3.1. 문자 스타일 속성







### 3.2. 문단 스타일 속성







### 3.3. 테두리 스타일 속성







### 3.4. 리스트 스타일 속성







### 3.5. 배경 스타일 속성







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

- [<한 권으로 끝내는 실무 Java 웹 개발서 - 김정현, 김계희>](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025)