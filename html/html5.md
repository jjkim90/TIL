# HTML5



> 이 문서는 [<한 권으로 끝내는 실무 Java 웹 개발서 - 김정현, 김계희>](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025) 책을 보고 정리한 문서입니다.



## 1. HTML5 시작하기

- 2014년 10월 28일에 차세대 웹 표준으로 확정.
- 금융권 액티브엑스 설치 없이 인터넷 서비스 이용할 수 있는 '오픈뱅킹' 도입
- 플래시와 같은 플러그인 없어도 자체적으로 동영상이나 음악을 재생할 수 있게 설계, 제작할 수 있음.



### 1.1. HTML5의 역사

- 1993년 처음 발표.
- 1999년 HTML 4.01버전 발표. 하이터텍스트 형태가 주된 요소.
- HTML(Hyper Text Markup Language) : 인터넷 전용 브라우저에서 그림이나 텍스트의 표현을 가능하게 지원하는 언어.
- 마크업이란 컴퓨터가 문서를 생성할 수 있게 하는 신호 체계를 설정하는 것.
- HTML은 구조(문서), CSS는 표현(디자인), Javascript는 동작(이벤트)을 담당함.



### 1.2. HTML5의 특징

- 웹 표준이란 브라우저에 관계없이 브라우저 간 상호 호환성에 초점을 둔 표준안.

- 웹 표준에 따라 웹 애플리케이션을 개발.
- 인터넷 접속 환경이 다른 사용자들이 동등하게 정보를 이용.



### 1.3. HTML5의 구조

- <! DOCTYPE html>은 HTML5 문서임을 나타냄. 모든 문서 첫 번째 행에 표기.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>첫 번째로 만드는 HTML5 페이지</title>
</head>
<body>
<h2>첫 번째 HTML입니다.</h2>
</body>
</html>
```

- HTML : 문서의 시작과 문서의 종료를 나타냄.
- HEAD : 문서의 머리 부분으로 웹페이지에는 보이지 않는 부분으로 문서 제목과 헤더 정보(즉, 문서의 설명, 키워드, 저자) 등을 표시.
- meta charset="UTF-8" : 전 세계의 문자와 기호를 사용할 수 있는 인코딩 방식.
- TITLE : 문서의 제목을 웹 브라우저 제목 표시줄에 표시.
- BODY : 브라우저에 표시되는 문서 내용으로 웹페이지상에 보이는 모든 내용을 표시.



#### HTML 태그의 구성

- 요소(Element)
- 속성(Attribute)
- 속성값(Argument)



#### HTML 태그 규칙

- 엔터, 스페이스바, 탭은 인식하지 않기 때문에 특정 기호 또는 태그로 표현해야 함.
- 파일이나 폴더명 여백 없는 영문자로 작성.



### 1.4. 실습환경



## 2. HTML5 태그 소개



### 2.1. HTML5 기본 태그

#### 텍스트를 처리하는 태그

##### 주석 태그

- 간단한 설명을 기록해둘 때 사용.
- 코드의 수정이나 보완 시 내용을 쉽게 파악.
- 주석 처리안에 작성한 내용은 없는 것으로 인식되어 실행하지 않음.

```html
<!-- 설명할 내용 -->
```



##### 제목 태그 \<h>

- Heading의 약자. Bold로 포현하며 문장의 제목으로 사용함.
- 자동 줄바꿈 기능을 포함하고 있음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>제목 태그</title>
</head>
<body>
	<h1>HTML5 Web Program</h1>
	<h2>HTML5 Web Program</h2>
	<h3>HTML5 Web Program</h3>
	<h4>HTML5 Web Program</h4>
	<h5>HTML5 Web Program</h5>
	<h6>HTML5 Web Program</h6>
</body>
</html>
```



##### 줄 바꾸기 태그 \<br>

- Line break의 약자. HTML 문서의 어느 곳에서든 강제로 줄바꿈 할 수 있음.
- 종료 태그 없음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>줄 바꾸기 태그</title>
</head>
<body>
	웹문서 구현(클라이언트, front-end)
	<br> &nbsp;&nbsp;&nbsp;&nbsp;html: 문서의 구조를 작성
	<br> &nbsp;&nbsp;&nbsp;&nbsp;css: 문서에 스타일 적용
	<br> &nbsp;&nbsp;&nbsp;&nbsp;javascript: 문서를 동적으로 처리한다.
</body>
</html>
```

- 띄어쓰기와 엔터 줄바꿈은 처리되지 않음. 1칸 띄우려면 특수문자 (\&nbsp;)를 이용해야 함.



##### 입력한 모양대로 출력하는 태그 \<pre>

- Preformatted text의 약자.
- 공백, 탭, 줄바꿈, 띄어쓰기, ASCII 문자 등을 그대로 표현할 수 있음.
- Courier, 굴림체, 바탕체 등 모든 문자의 폭이 동일한 고정 폭 글꼴을 사용해야 웹페이지에서도 같은 표현을 할 수 있음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>입력한 모양대로 출력하는 태그</title>
</head>
<body>
	<pre>
	웹 문서 구현(클라이언트, front-end)
	    html: 문서의 구조를 작성
	    css: 문서에 스타일 적용
	       JavaScript: 문서를 동적으로 처리한다.	
	</pre>	
</body>
</html>
```

- 게시판이나 방명록의 상세보기를 할 때 \<br>이나 \&nbsp;를 사용하지 않아도 입력 내용대로 출력할 수 있음.



##### 문자 모양 태그

- 문자에 모양을 주어 의미와 형태를 부여함.
- \<b> : 굵게
- \<del> : 취소선
- \<em> : 강조
- \<i> : 이탤릭체
- \<ins> : 추가한 내용을 밑줄로 표시.
- \<mark> : 음영으로 텍스트 강조.
- \<small> : 작은 텍스트를 정의함.
- \<strong> : 중요하거나 긴급을 요하는 내용 표시. 강력한 텍스트를 정의함.
- \<sub> : 아래 첨자
- \<sup> : 윗첨자

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>문자 모양 태그</title>
</head>
<body>
	<p><b> b 태그는 굵은 글꼴입니다.</b> </p>
	<p><i> i 태그는 기울임 꼴입니다.</i> </p>
	<p><em> em 태그는 강조로 표시합니다.</em> </p>
	<p><small> small 태그는 작은 글자를 표시합니다.</small> </p>
	<p><mark> mark 태그는 음영 </mark>으로 표시합니다.</p>
	<p><strong> strong 태그는 문자를 강력하게 강조합니다.</strong> </p>
	<p>X<sub>2</sub>(아래 첨자) 와  Y<sup>2</sup>(위 첨자)입니다. </p>
	<p>ins 태그는 <ins>추가한 내용을 밑줄</ins>로 표시합니다.</p>
	<p><del>del 태그는 취소선을 표시합니다.</del> </p>
</body>
</html>
```



##### 특수 문자 표기법 

- HTML 문서상에서 직접 쓸 수 없는 특수기호를 표현하기 위해 사용하는 표기법.
- \&nbsp; - 공백
- \&lt; - <
- \&gt; - >
- \&amp; - &
- \&quot; - ''
- \&copy - c
- \&reg; - r

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>특수 문자 표기</title>
</head>
<body>
	<p>space: &nbsp;  (공백)</p>
	<p>less than: &lt;</p>     
	<p>greater than: &gt; </p>  
	<p>ampersand: &amp;</p>    
	<p>cent: &cent; </p>
	<p>pound: &pound; </p>
	<p>yen: &yen;</p>
	<p>euro: &euro; </p>
	<p>section: &sect; </p>
	<p>copyright: &copy; </p>
	<p>registered trademark: &reg; </p>
</body>
</html>
```



#### 공간 분할 태그와 선 태그



##### 단락 태그 \<p>

- 텍스트를 단락(문단)으로 정의할 때 사용함.
- \<br> 태그를 2번 입력한 효과.
- \<p> 태그는 연속으로 사용해도 \<p>를 한 번으로 인식함.
- 전체 텍스트에서 독립된 문단.



##### 블록 형식의 공간 분할 태그 \<div>

- 박스 형태로 블록 영역으로 공간을 분할하는 단락 정렬 태그임.
- 블록 요소는 요소 앞뒤로 줄바꿈이 일어나며 주로 레이아웃에 사용함.
- \<p>와 유사하나 \<div>에는 \<br> 기능이 없음.



##### 인라인 형식의 공간 분할 태그 \<span>

- CSS와 함께 문자를 꾸며줄 때 사용함.
- \<div> 태그와의 차이점은 블록(block)이 아닌 인라인(inline) 형식으로 공간을 분할한다는 점임.
- \<div>는 줄바꿈이 되지만 \<span>은 요소 간에 줄바꿈을 하지 않음.



##### 선 그리기 태그 \<hr>

- Horizontal Rule의 약자.
- 텍스트 사이에 선을 그음. 세로 선은 그을 수 없음.
- 자동 줄바꿈 기능이 있고 종료 태그 없음.



```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>공간 분할 태그와 선 태그</title>
</head>
<body>
	<div>12345</div>
	<div>가나다라</div>
	<div>ABCD</div>
    <hr>
    
	<p>12345</p>
	<p>가나다라</p>
	<p>ABCD</p>
    <hr>
    
	<span>12345</span>
	<span>가나다라</span>
	<span>ABCD</span>
</body>
</html>
```

- \<div> 태그는 블록으로 영역을 설정하기 때문에 한 행의 끝까지 영역으로 확보.
- \<p> 태그는 줄바꿈 기능이 있어 한 줄 띄움.
- \<span> 태그는 폭과 높이를 지정해도 표현되지 않고 글자의 크기만큼 인라인 영역을 확보함.
- \<hr>로 선을 그림.



```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>공간 분할 태그와 선 태그</title>
</head>
<body>
	<div style="border:1px dotted red">
	<h2>백두산 호랑이</h2>
	<hr><br>
	높은 산의 숲이 우거진 곳에서 사는 고양잇과의 포유류로 
	<span style="color: red; font-weight: bold">한국 호랑이</span>라고도 한다.<br><br> 
	</div>
</body>
</html>
```

- \<div> 태그 블록 안의 내용을 1px, dotted, red로 감싸서 영역을 표시함.
- \<span> 태그 안의 CSS 이용해서 문자색은 red, 글꼴은 굵게 표시.



#### 이미지 태그 \<img>

- 웹 문서에 이미지를 표현할 때 사용함.
- img 요소는 반드시 src 속성을 설정해야 함.
- 사용할 수 있는 이미지 파일
  - GIF : 표현 가능 색상 256개.
  - JPEG : 24비트 칼라. 수백만 개의 색상 표현.
  - PNG : GIF, JPEG, BMP 장점 합침.
- HTML에서는 align, border, hspace, vspace 속성 지원하지 않고 width, height도 스타일 시트에서 설정이 가능하기 때문에 HTML에서는 잘 사용하지 않음.



```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>이미지 태그</title>
</head>
<body>
	<img src="image/mouse.png">
	<img src="image/mouse.png" width="80px" height="80px">
	<hr> 
	<img src="image/mouse1.png" alt="미키 마우스">
</body>
</html>
```

- alt는 이미지 파일 불러오기 실패했을 때 표시되는 텍스트.

- figure는 독립된 콘텐츠를 표현할 때 사용. 이미지와 제목을 그룹으로 묶을 수 있음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>figure 태그</title>
</head>
<body>
<figure>
	<img src="image/w3c.png" alt="w3c사이트" width="75">
	<figcaption>[그림 1] w3c사이트</figcaption>
</figure>
 <br>
</body>
</html>
```

- \<figure> \</figure> 태그 안에서 이미지(img)와 이미지의 제목(figcaption)을 그룹으로 묶어서 사용함.



#### 목록 태그

- 메뉴를 작성하거나 데이터를 나열할 때 사용함.
- 비 순차적 목록, 순차적 목록, 정의 목록이 있음.



##### 비 순차적 목록 \<ul>

- Unordered List의 약자.
- 세부 리스트 항목은 \<li>를 사용함.
- 기본값은 채워진 원(disc). circle, disc, square, none을 쓸 수 있음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>목록 태그</title>
</head>
<body>
	<ul type="disc">
		<li>Caffe latte</li>
		<li>Americano</li>
		<li>Espresso</li>
	</ul>
</body>
</html>
```



##### 순차적 목록 \<ol>

- Ordered List의 약자.
- 세부 리스트 항목은 \<li>를 사용함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>목록 태그</title>
</head>
<body>
	<ol type="A">
		<li>Caffe latte</li>
		<li>Americano</li>
		<li>Espresso</li>
	</ol>

	<ol start="50">
		<li>Caffe latte</li>
		<li>Americano</li>
		<li>Espresso</li>
	</ol>
</body>
</html>
```

- \<ol> 태그의 type 속성 1, a, A, i, I 쓸 수 있음.
- start 속성은 임의의 숫자부터 시작하고 싶을 때 쓸 수 있음.



##### 정의 목록 \<dl>

- Definition List의 약자.
- DT(Definition Term), DD(Definition Description)와 함께 사용함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>목록 태그</title>
</head>
<body>
	<dl>
		<dt>HTML</dt>
		<dd>HyperText Markup Language</dd>
		<dt>CSS</dt>
		<dd>Cascading Style Sheets</dd>
	</dl>
</body>
</html>
```

- \<dl> 태그 사이에 \<dt>는 정의할 용어, \<dd>는 용어 설명.
- \<dd>는 들여쓰기됨.



#### 하이퍼링크 태그 \<a>

- Anchor의 약자.
- 텍스트나 이미지 콘텐츠에 링크를 걸어서 다른 페이지로 이동하는 하이퍼링크.

```html
<!-- 기본 형식 -->
<a href= " " target =" ">텍스트나 이미지</a>

href="이동할 페이지 또는 링크할 URL"
target="_blank | _top | _self | _parent"
```

- \<a> 태그의 href 속성은 링크의 목적지를 나타냄.
- target 속성
  - _blank는 새창 또는 새탭에서 연결된 문서를 오픈함.
  - _top은 현재창에서 연결된 문서를 오픈함.
  - _self는 연결된 문서를 동일한 프레임에서 오픈함.(기본값)
  - _parent는 상위 프레임에서 연결된 문서를 오픈함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>하이퍼링크 태그</title>
</head>
<body>
	<a href="http://www.w3c.org" target="_top">W3C</a><br>
	<a href="ex01_01.html" target="_blank">1페이지로 이동</a><br><br>

	<a href="ex01_07.html" title="ex01_07.html페이지로 이동"> <img
		src="image/mouse.png">
	</a>
</body>
</html>
```



#### 테이블 태그 \<table>

- 표를 만드는 태그. 데이터를 보여주는 표를 만들 때 사용함.
- HTML5부터 테이블로 레이아웃을 설정하지 않음.
- 웹페이지의 레이아웃을 설정할 때는 \<div> 태그나 시멘틱 태그를 사용함.
- 웹 표준에서 테이블 태그를 사용하는 경우
  - 스프레드시트
  - 차트
  - 스케줄
  - 달력
- 테이블 태그
  - \<table> - 2차원 격자 모양의 표를 표시.
  - \<tr> - Table Row의 약자. 행 구분.
  - \<th> - Table Header의 약자. 표 내부의 제목. 생략 가능. 진한 문자색과 가운데 정렬이 기본.
  - \<td> - Table Data의 약자. 열 구분.
  - \<caption> - 표의 상단/하단에 제목을 적을 때 사용함. 생략 가능.
  - \<thead> - 행 그룹에서 제목 그룹을 설정.
  - \<tbody> - 행 그룹에서 내용 그룹을 설정.
  - \<tfoot> - 행 그룹에서 꼬리 그룹을 설정.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>테이블 태그</title>
</head>
<body>
	<table border="1">
		<caption>주소록</caption>
		<tr>
			<th>이름</th>
			<th>주소</th>
			<th>전화</th>
		</tr>
		<tr>
			<td>김자바</td>
			<td>서울</td>
			<td>010-111-1111</td>
		</tr>
		<tr>
			<td>박자바</td>
			<td>수원</td>
			<td>010-222-2222</td>
		</tr>
	</table>
</body>
</html>
```

- colspan과 rowspan 속성을 적용하여 행 또는 열을 병합할 수 있음.
- colspan은 열 병합하여 하나의 행으로 만듦.
- rowspan은 행 병합하여 하나의 열로 만듦.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>테이블 태그</title>
</head>
<body>
	<table border="1">
		<tr>
			<td colspan="4">지상파 방송국</td>
		</tr>
		<tr>
			<td rowspan="2">채널</td>
			<td>MBC</td>
			<td>KBS</td>
			<td>SBS</td>
		</tr>
		<tr>
			<td>11</td>
			<td>9</td>
			<td>5</td>
            <td>10</td>
		</tr>
	</table>
</body>
</html>
```

- 테이블 머리인 thead, 테이블의 본문인 tbody, 테이블의 꼬리인 tfoot를 이용해서 테이블 안에서 태그의 위치를 재배치할 수 있음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>테이블 태그</title>
</head>
<body>
	<table border="1">
		<caption align="top">교재 가격표</caption>
		<thead>
			<tr>
				<th>서적명</th>
				<th>출판사</th>
				<th>가격</th>
				<th>기타</th>
			</tr>
		</thead>
		<tfoot>
			<tr>
				<td colspan="2">합계</td>
				<td>107,000원</td>
				<td>&nbsp;</td>
			</tr>
		</tfoot>
		<tbody>
			<tr>
				<td>HTML5</td>
				<td>공갈닷컴</td>
				<td>55,000원</td>
				<td rowspan="3">&nbsp;</td>
			</tr>
			<tr>
				<td>CSS3</td>
				<td>남가람북스</td>
				<td>32,000원</td>
			</tr>
			<tr>
				<td>JavaScript</td>
				<td>야메루 출판사</td>
				<td>20,000원</td>
			</tr>
		</tbody>
	</table>
</body>
</html>
```



#### 프레임 태그 \<iframe>

- Internet Frame의 약자.
- 현재 HTML 문서 내에 일정한 크기의 영역을 할당해서 삽입하는 인라인 프레임.
- 속성
  - sandbox : 플러그인 실행 금지, JavaScript 사용 금지, 폼 요소에 의한 페이지 호출 금지 등의 제한을 할 수 있음.
  - srcdoc : iframe 태그에 표시할 페이지의 내용을 작성.
    - srcdoc를 사용할 경우 src 속성보다 우선순위가 높기 때문에 src의 내용은 무시함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>프레임 태그</title>
</head>
<body>
	<iframe src="http://www.namgarambooks.co.kr">
		<p>이 브라우저는 iframe을 지원하지 않습니다</p>
	</iframe>
</body>
</html>
```

- \<iframe> 태그 안에 src 주소의 웹사이트를 오픈함.
- \<p> 태그는 브라우저가 \<iframe> 태그를 지원하지 않을 경우 출력함.



#### 입력양식 태그

- 로그인, 회원가입, 게시판, 설문조사 등의 데이터를 입력받을 수 있게 만드는 양식 태그.
- \<form> 태그와 자식 태그인 \<input>, \<select>, \<textarea>가 있음.



##### \<form> 태그

- 정보를 입력 또는 선택하고 버튼을 클릭할 때 정보를 서버에 전달하는 CGI 기능.
  - CGI(Common Gateway Interface) : 웹 서버와 클라이언트 사이에서 데이터를 주고받기 위한 표준화된 인터페이스. C, Visual Basic, Perl, PHP, ASP 등을 이용하여 작성. \<form>의 "name" 속성과 사용자가 입력한 데이터를 한 쌍으로 서버에 전송함.
- 응답받는 양방향의 의사소통을 지원하는 입력 양식.
- 웹 서버와 클라이언트가 서로 상호작용하여 통신할 수 있도록 하는 역할을 담당.
- \<form> 태그 속성
  - action="URL" : form의 내용을 처리하기 위해 서버 쪽 URL을 지정.
  - method="get/post" : 클라이언트의 데이터를 서버로 보내기 위한 방법을 설정.
    - get(기본값) : 서버로 데이터를 전송할 때 길이의 제한이 있음. 또한 주소 표시줄에 전송값을 출력함.
    - post : 대용량이거나 보안이 필요한 데이터를 서버로 전송할 때 적합함.
  - enctype=" " : 폼 데이터 제출 시 인코딩 타입 결정.
    - application/x-www-form-urlencoded :  key=value의 Map 형태로 표현. 기본값임. 예를 들어, name=kim&age=20&phone=010-111-1111로 전송함.
    - multipart/form-data : 모든문자 인코딩하지 않음. 첨부파일을 전송할 때 사용함.
    - text/plain : 공백 문자는 "+" 기호로 변경되지만, 나머지 문자는 모두 인코딩하지 않음.
  - target="윈도우명" : form에서 입력 데이터의 처리된 결과를 표시할 윈도우.
  - name=" " : form을 식별하기 위해 이름을 지정함.
  - accept-charset=" " : 전송에 사용할 문자 인코딩을 지정함.
- \<form> 태그의 자식 태그
  - input
    - text
    - password
    - checkbox
    - radio
    - submit
    - reset
    - button
    - image
    - file
    - hidden
  - textarea
    - cols
    - rows
  - select
  - optgroup
  - option
  - fieldset
  - legend
- input 태그 속성
  - type="입력 타입"
  - name="변수 명"
  - value="기본입력 값"
  - size="입력양식길이"
  - maxlength="입력문자수"
  - src="URL"
  - checked
  - readonly
  - accept
  - alt

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>입력양식</title>
</head>
<body>
	<form action="ex01_01.html" method="get">
		<table border="0">
			<tr>
				<td>아이디</td>
				<td><input type="text" name="id" size="20"></td>
			</tr>
			<tr>
				<td>비밀번호</td>
				<td><input type="password" name="pwd" size="20"></td>
			</tr>
			<tr>
				<td colspan="2" align="center">
					<input type="button" value="목록">
					<input type="submit" value="전송">
					<input type="reset" value="취소">
				</td>
			</tr>
		</table>
	</form>
</body>
</html>
```

- type="button" : 이벤트와 연결되어 JavaScript의 함수를 호출함. 현재는 연결되어 있지 않기 때문에 어떤 동작도 하지 않음.
- type="submit" : form 태그의 action="url" 페이지로 이동함. method="get" 방식이므로 "ex01_01.html?id=abcd&pwd=1234"의 형태로 데이터를 서버로 전송함.
- type="reset"은 form 태그 안의 입력된 모든 데이터를 초기화함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>radio와 checkbox 태그</title>
</head>
<body>
	<form>
		<table border="0">
			<tr>
				<td>성별<br>
				<td><input type="radio" name="gender" value="man" checked>남자
					<input type="radio" name="gender" value="woman">여자</td>
			</tr>
			<tr>
				<td>취미</td>
				<td><input type="checkbox" name="hobby" value="climbing">등산
					<input type="checkbox" name="hobby" value="traveling">여행 <input
					type="checkbox" name="hobby" value="fishing">낚시</td>
			</tr>
		</table>
	</form>
</body>
</html>
```

- type="radio"는 여러 개의 radio 버튼 중 1개만 선택 가능함.
- type="checkbox"는 요소를 중복하여 선택 가능함.

- textarea 태그 속성
  - cols : 칼럼 수
  - rows : 행 수
  - name : 요소 이름 지정
  - readonly : 읽기 전용으로 설정
  - disabled : 클릭할 수도 없고 수정도 불가능함.
- select 와 option 태그 속성
  - name="입력변수 명" : 서버에 전송할 양식의 이름.
  - size="보일 항목 개수" : 드롭 다운 메뉴의 줄 수를 지정함.
  - multiple : 메뉴를 다중으로 선택하는 경우에 사용함.
  - value="데이터" : \<option>에서 사용하는 데이터.
  - selected : 기본값으로 선택할 데이터를 지정함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>select와 textarea 태그</title>
</head>
<body>
	<form>
		<table border="0">
			<tr>
				<td>컴퓨터 운영체제</td>
				<td><select name="OS" size="3" multiple>
						<option value="windows">윈도우7</option>
						<option value="linux">리눅스</option>
						<option value="solrais">솔라리스</option>
						<option value="mac">맥</option>
				</select></td>
			<tr>
			<tr>
				<td>기타사항</td>
				<td><textarea rows="7" cols="30" name="exam"></textarea></td>
			</tr>
		</table>
	</form>
</body>
</html>
```

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>select 태그</title>
</head>
<body>
	<select>
		<option>JAVA</option>
		<option>JSP</option>
		<option>HTML5</option>
		<option>jQuery</option>
	</select>
</body>
</html>
```

- 리스트 박스는 지정한 size만큼 데이터를 화면에 보여주지만, size가 없으면 아래화살표 모양을 표시하는 콤보 박스가 됨. 화살표 누르면 박스가 열리면서 데이터를 보여줌.
- optgroup 태그 속성
  - label="타이틀" : 그룹의 타이틀

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>optgroup과 option 태그</title>
</head>
<body>
	<select>
		<optgroup label="html5">
			<option>html</option>
			<option>css</option>
			<option>javascipt</option>
		</optgroup>
		<optgroup label="웹 표준">
			<option>구조</option>
			<option>동작</option>
			<option>표현</option>
		</optgroup>
	</select>
</body>
</html>
```



##### \<label> 태그와 for 속성

- \<label> 태그에서 for 속성은 입력 박스(input)와 레이블(label)을 연결함.
- label 태그의 for 속성이 input과 연결되어 있을 경우 label을 클릭하면 자동으로 연결된 input이 활성화됨.
- for와 id의 이름이 같아야 두 태그가 연결됨.
- 연결 방법 2가지
  - label 태그 안에 input 태그를 위치시키기.
  - label 태그의 for 속성 값과 input 태그의 id 속성 값 일치시키기.


```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>label 태그와 for 속성</title>
</head>
<body>
<form>
   <label>이름 <input type="text" name="name"></label><br><br>

   <label for="myName">이름</label>
   <input type="text" name="name" id="myName">
</form>
</body>
</html>
```



##### fieldset 그룹 태그와 legend

- \<fieldset> 태그는 같은 목적을 가진 태그들을 그룹화함.
- \<legend>는 \<fieldset> 안에 사용하며 \<fieldset>으로 묶은 영역의 제목으로 사용함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>fieldset 그룹 태그와 legend</title>
</head>
<body>
<form>
  <fieldset>
    <legend>커피 사이즈는 어떤걸로 할까요?</legend>
    <p>
      <input type="radio" name="size" id="size1" value="small" />
      <label for="size1">Small</label>
    </p>
    <p>
      <input type="radio" name="size" id="size2" value="medium" />
      <label for="size2">Medium</label>
    </p>
    <p>
      <input type="radio" name="size" id="size3" value="large" />
      <label for="size3">Large</label>
    </p>
  </fieldset>
</form>
</body>
</html>
```



### 2.2. HTML5 멀티미디어 태그

- 멀티미디어 태그인 video와 audio는 브라우저 자체에서 동영상을 재생하도록 기능이 추가됨.
- 브라우저 별로 지원하지 않는 동영상 파일도 있으니 지원 현황을 확인하고 사용해야 함.



#### 음악 태그 \<audio>

- 음악 파일을 실행할 때 사용함.
- audio의 속성값
  - src=" " : 오디오 파일의 경로 지정.
  - controls = "controls" : 오디오 컨트롤을 표시할지 여부를 지정.
  - autoplay = "autoplay" : 파일이 로드되자마자 자동 재생.
  - loop : 사운드를 반복 재생하도록 설정.
  - type=" " : 오디오 파일의 MIME 형식을 지정함.
- 오디오 파일의 종류
  - MP3(*.mp3)
  - WAVE(*.wav, *.wave) : MS와 IBM 개발. 비압축 방식의 표준 오디오 파일 포맷.
  - OGG(*.ogg, *.ogv) : 공개 소스 기반의 다양한 코덱 지원하는 오디오 파일.



#### 동영상 태그 \<video>

- 동영상 파일을 재생할 때 사용함.
- video의 속성값
  - src=" " : 파일 경로
  - controls = "controls" : 동영상 화면에 컨트롤을 표시할지 여부를 지정함.
  - width=" ", height=" " : 화면에서 비디오가 표시될 영역의 크기를 설정.
  - poster= " " : 동영상이 화면에 나타나지 않을 때 대신 표시할 그림. 일종의 썸네일.
  - preload=" "
    - auto(기본값) : 페이지를 로드하고 바로 비디오 파일을 다운로드함.
    - metadata : 사용자가 재생시키기 전 까지는 비디오의 크기, 첫 프레임, 비디오 관련정보 등과 같은 메타 데이터만 다운로드함.
    - none : 재생 시작 전까지 비디오 파일을 다운로드 하지 않음.
- 비디오 파일의 종류
  - MPEG4(*.mp4, *.m4v) : 일반적으로 웹에서 비디오 파일 공유할 때 사용.
  - WebM(*.webm) : 구글 개발.
  - avi : 윈도우 지원. 실시간 웹 영상에 적합하지 않음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>오디오와 비디오</title>
</head>
<body>
	<audio controls="controls">
		<source src="audio/ladymov.mp3"  type="audio/mp3">
		<source src="audio/ladymov.ogg"  type="audio/ogg">
	</audio><br><br>
	
	<video width="640px" height="360px"  controls="controls">
		<source src="video/tiger.mp4" type="video/mp4">
		<source src="video/tiger.webm" type="video/webm">
	</video>
</body>
</html>
```



### 2.3. HTML5 태그의 변화

- HTML5부터 기존에 사용하던 태그에 기능을 추가한 향상 태그와 새롭게 추가한 태그 있음.
- CSS에서 기능을 대체할 수 있거나 사용상 문제가 있는 태그는 삭제함.



#### HTML5에서 향상된 태그

- 향상된 input 태그 속성
  - date
  - month
  - week
  - time
  - datetime-local
  - email
  - url
  - tel
  - number -> min, max, step(간격), value(초기값)
  - range -> min, max, step(간격), value(초기값)
  - color
  - search

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>향상된 input 태그</title>
</head>
<body>
	<form>
		이메일: <input type="email" name=" email"><br> 
		URL : <input type="url" name="url"><br> 
		전화번호: <input type="tel" name="tel"><br> 
		색상: <input type="color" name="color"><br> 
		월: <input type="month" name="month"><br> 
		날짜: <input type="date"name="date"><br> 
		주: <input type="week" name="week"><br> 
		시간: <input type="time" name="time"><br> 
		지역시간: <input type="datetime-local" name="datetime-local"><br> 
		숫자: <input type="number" name="number" min="1" max="10" step="2"><br>
		범위: <input type="range" name=" range" min="1" max="10" step="2"><br>
		Search: <input type="search" name="search"><br>
		<input type="submit" value="제출">
	</form>
</body>
</html>
```

- 웹브라우저별로 결과가 다르게 나타남.
- input 태그에 추가된 속성
  - autocomplete : 자동 입력완성 (기본값 : on)
  - autofocus : 자동으로 입력 포커스를 표시
  - pattern : 입력 가능한 형태의 정규 표현식 패턴
  - required : 반드시 채워야 하는 필드. 입력이 안되면 오류 메시지를 표시함.
  - placeholder : 입력할 내용을 흐린 색의 Hint로 보여줌.
  - readonly : 읽기 전용 (boolean 속성)
  - form : form이 하나 이상인 경우 input 요소가 속한 form의 id임.
  - formaction : form을 전송할 때 입력 데이터를 처리할 파일의 URL을 지정함. form 요소의 action 속성을 새롭게 지정함. type="submit"과 type="image"와 함께 사용됨.
  - formenctype : 서버로 제출 시 form-data의 encode를 지정. method="post"인 경우에만 해당함.
  - formmethod : action="URL"로 form-data를 제출할 때 사용할 메서드를 정의함.
  - formnovalidate : 서버로 전송할 때 input 요소의 유효성을 검사하지 않음 (boolean 속성)
  - formtarget : 서버로 전송한 후 응답받은 데이터를 표시할 곳의 이름을 지정함.
  - height/width : 높이와 너비를 지정. \<input type="image"> 요소에만 사용함.
  - list : input 요소가 사용되기 위해서 미리 만들어 놓은 datalist 요소를 가리킴.
  - min/max : input 요소의 최소값, 최대값 지정.
  - multiple : 사용자가 input 요소에 하나 이상의 값을 입력할 수 있음.
  - step : 숫자의 단계를 지정함.  step="3"이면 0, 3, 6, ...으로 증가함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>form 태그로 만든 개인 정보 입력</title>
</head>
<body>
	<form>
		<fieldset>
			<legend>개인 정보 입력</legend>
			<table>
				<tr>
					<td><label for="name">이름</label></td>
					<td><input type="text" name="name" 
					       placeholder="이름을 입력하시오"
						   required="required" autofocus="autofocus" id=”name”></td>
				</tr>
				<tr>
					<td><label for="email">이메일</label></td>
					<td><input type="email" name="mail" placeholder="이메일을 입력하시오" id=”email”></td>
				</tr>
				<tr>
					<td><label for="phone">전화</label></td>
					<td><input type="text" name="tel"
						pattern="[0-1]{3}-[0-9]{4}-[0-9]{4}" 
						title="###-####-####"
						placeholder="전화번호를 입력하시오" id=”phone”></td>
				</tr>
				<tr>
					<td><label for="photo">파일선택</label></td>
					<td><input type="file" accept="image/jpg, image/gif" id=”phone”></td>
				</tr>
			</table>
			<input type="submit" value="제출"> 
			<input type="reset"  value="취소">
		</fieldset>
	</form>
</body>
</html>
```



#### HTML5에서 추가된 태그

- HTML5에 새로 추가된 태그들
  - \<a> - download속성, ping속성
  - \<bdi> - 텍스트의 출력 방향을 설정.
  - \<command> - 사용자가 호출할 수 있는 명령어를 나타냄.
  - \<details> - 사용자 요청에 따라 얻은 컨트롤이나 추가적인 정보를 표시함.
  - \<datalist> - input의 list 속성과 함께 콤보 박스를 만드는 태그.
  - \<dialog> - 대화를 의미 있는 콘텐츠로 만들고자 할 때 사용하며 대화상자 또는 창을 나타냄.
  - \<mark> - 특정 텍스트의 강조 효과를 형광펜으로 칠한 것과 같은 효과.
  - \<meter> - 디스크 사용량 등과 같은 측정치를 표시하는 태그.
  - \<menu> - 명령어 정의 또는 단축 메뉴들을 목록화함.
  - \<output> - 스크립트 등을 통해서 결과를 나타내는 태그
  - \<progress> - 다운로드할 때 얼마나 진행하였는지 보여주는 태그.
  - \<ruby>\<rt>\<rp>\<rb>\<rtc> - 루비는 아시아권 언어(한자, 일본어, 중국어)에서 발음이나 주석을 표시하기 위한 텍스트임.
  - \<summary> - details 요소의 하위 요소로써 머리글을 나타냄.
  - \<time> - 날짜와 시간을 나타내는 태그.
  - \<wbr> - 줄바꿈 지점을 나타낼 때 사용함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>HTML5에 새로 추가된 태그들</title>
</head>
<body>
<mark>Mickey Mouse</mark>
<details>
  <summary>이미지 정보</summary>
  <ul>
  	<li>파일: mouse.png</li>
    <li>크기: 36KB</li>
    <li>위치: D:\image</li>
    <li><a href="image/mouse.png" download>파일 다운로드</a></li>
  </ul>
</details><br>

<ruby>
	<rtc>大韓民國</rtc>
	<rt>대한민국</rt>
</ruby><br><br>

<meter value="0.75">75%</meter><br><br>
<progress value="30" max="100">Progress:10%</progress><br><br>

<menu label="커피종류">
  <li style="list-style:none;"><input type="radio" name="tea">아메리카노</li>
  <li style="list-style:none;"><input type="radio" name="tea">카페라떼</li>
  <li style="list-style:none;"><input type="radio" name="tea">콜드블루</li>
</menu>
</body>
</html>
```



#### HTML5에서 삭제된 태그

- HTML5에서 삭제된 태그
  - \<font>
  - \<frame>
  - \<frameset>
  - \<noframe>
  - \<center>
  - \<basefont>
  - \<big>
  - \<isindex>
  - \<strike>
  - \<u>
  - \<xmp>
  - \<acronym>
  - \<applet>
  - \<dir>
  - \<tt>
- https://www.w3.org/TR/html5-diff/ 에서 변경점 확인할 수 있음.



### 2.4. HTML5 시맨틱 태그

- 시맨틱 태그(Semantic Tag)는 인터넷상의 각종 자원(웹 문서, 파일, 서비스 등)을 표준 규격으로 표현하는 태그임.
- 검색 로봇 또는 스크린 리더 등의 기계가 쉽게 해석하고 분석할 수 있도록 만들어진 문서를 의미함.
- HTML5에서는 기기가 쉽게 분석하고 해석할 수 있는 문서를 구현하기 위해 Mark-Up 요소들이 새로 추가됨.
- 시맨틱 태그의 구조 요소
  - \<header> : 문서의 제목 요소 또는 로고/아이콘, 저자 정보를 표시함.
  - \<nav> : 문서의 메뉴 모음을 정의함.
  - \<section> : 문서에서 콘텐츠의 주제 그룹. h1~h6와 함께 사용함.
  - \<article> : 독립적인 콘텐츠.
  - \<aside> : 문서의 주요 부분을 제외한 남은 부분의 콘텐츠(보충 정보).
  - \<footer> : 문서의 꼬리말, 저자, 저작권 정보를 표시함.
  - \<hgroup> : 제목과 부제목을 묶어주는 역할을 함.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>div 태그를 이용한 구조화</title>
</head>
<body>
	<div id="header">
		<h1>Web Publisher</h1>
	</div>
	<div id="nav">
		<ul>
			<li><a href="#"> HTML5 </a>
			<li><a href="#"> CSS3 </a>
			<li><a href="#"> JAVASCRPT </a>
			<li><a href="#"> jQuery </a>
		</ul>
	</div>
	<div id="section">
		<h2>HTML5이란?</h2>
		<div id="article">1. HTML개요</div>
		<div id="article">2. 문서구조</div>
	</div>
	<div id="aside">
		<h3>보충설명</h3>
		<p>
			HTML5, CSS3, JavaScript는 front-end입니다<br> PHP, JSP, ASP는
			back-end입니다
		</p>
	</div>
	<div id="footer">
		<p>address and @copyright</p>
	</div>

</body>
</html>
```

- 배치는 CSS의 레이아웃 기능을 이용해서 배치할 수 있음.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>시맨틱 태그를 이용한 구조화</title>
</head>
<body>
	<header>
		<h1>Web Publisher</h1>
	</header>
	<nav>
		<ul>
			<li><a href="#"> HTML5 </a>
			<li><a href="#"> CSS3 </a>
			<li><a href="#"> JAVASCRPT </a>
			<li><a href="#"> jQuery </a>
		</ul>
	</nav>
	<section>
		<h2>HTML5이란?</h2>
		<div id="article">1. HTML개요</div>
		<div id="article">2. 문서구조</div>
	</section>
	<aside>
		<h3>보충설명</h3>
		<p>
			HTML5, CSS3, JavaScript는 front-end입니다<br> PHP, JSP, ASP는
			back-end입니다
		</p>
	</aside>
	<footer>
		<p>address and @copyright</p>
	</footer>
</body>
</html>
```

- 검색 로봇이나 스크린 리더와 같은 기계들이 해석할 수 있도록 만드는데 의미가 있음.
- SEO 향상됨.



## 3. HTML5 표준 API 소개

- HTML5 API(Application Programming Interface)는 소프트웨어 애플리케이션을 액세스하기 위한 프로그래밍 명령어 및 표준화 컬렉션을 뜻함.
- 응용프로그램과 시스템 사이의 중간 역할을 함.
- 개발자 입장에서 보면 API는 클래스나 함수라고 설명할 수 있음.
- API가 제공하는 서비스로 원하는 프로그램을 구현할 수 있음.



### 3.1. Canvas API

- 그래픽 기반의 게임, 이미지 합성과 변형, 드로잉 애플리케이션 등 다양한 콘텐츠를 제공함.
- Canvas는 JavaScript를 통해 그래픽을 만들 수 있는 컨테이너임.
- 그림을 그리기 위해서는 JavaScript를 사용해서 선, 사각형, 원, 문자, 이미지를 추가할 수 있음.



### 3.2. Drag&Drop API

- 웹페이지 내에서 사용자가 요소를 자유롭게 이동시킬 수 있는 기능.
- 요소를 마우스로 선택에서 원하는 곳으로 드래그 하는 동안 선택한 요소는 반투명하게 마우스 포인터를 따라 움직임.
- 마우스에서 손을 떼면 요소는 드롭함.
- 모든 브라우저가 이 기능을 사용할 수 있음.



### 3.3. Web Storage API

- 클라이언트에 웹의 정보를 저장함.
- 웹 스토리지는 웹사이트에 영향을 끼칠 정도로 큰 용량을 허용함.
- 사용자 측에서 좀 더 많은 양의 정보를 안전하게 저장할 수 있도록 지원함.
- 웹 스토리지의 정보는 서버로 전송되지 않고 클라이언트에만 저장함.
- 키(key)와 값(value)을 쌍으로 저장하고 키를 기반으로 데이터를 조회하는 방식.
  - localStorage : 데이터를 저장할 때 영구 보존하는 저장소.
  - sessionStorage : 하나의 세션에 대해서만 데이터를 저장함.



### 3.4. Web Worker API

- 멀티 스레드 구동이 가능함.
- 스크립트와 독립적으로 실행하며 백그라운드에서 실행이 가능함.



### 3.5. Web Socket API

- 웹 브라우저와 웹 서버에서 실시간 양방향 통신 환경을 제공함.
- 웹 소켓의 기술을 사용하면 아무런 제약 없이 웹 브라우저와 웹 서버가 통신할 수 있음.
- 추가로 브라우저를 오픈하거나 스마트폰에서 별도의 플러그인을 설치하지 않아도 순수한 웹 기반의 채팅이 가능함.



### 3.6. Geolocation API

- 사용자의 현재 위치의 좌표(위도와 경도)를 가져오거나 원하는 좌표의 지도를 그릴 수 있음.



# References

- [<한 권으로 끝내는 실무 Java 웹 개발서 - 김정현, 김계희>](http://www.kyobobook.co.kr/product/detailViewKor.laf?mallGb=KOR&ejkGb=KOR&barcode=9791189184025)