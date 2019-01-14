# HTML

* 웹페이지를 대표하는 html을 만든 후 링크를 걸어 다른 html 페이지와 연결 시키기
	+ test.html 대표 페이지
	+ index.html web 설명 페이지
	+ html.html html 설명 페이지
	+ css.html css 설명 페이지
	+ javascript javascript 설명 페이지

-test.html에서 dod?를 누르면 https://www.google.co.kr/의 링크로 들어가게 되는데 target="blank"를 해주어 새창으로 뜬다.

<hr/>

> test.html

<!DOCTYPE html>
<html>
<head>
	<title> test </title>
	<meta charset="utf-8">
</head>
<body>
	<h1> <a href="test.html"><u>Test</u> </a></h1>
	<a href = "https://www.google.co.kr/" target="_blank" title = "google" >d0d? </a>
	<h2> <a href="index.html"  title="WEB^__^"> WEB </a> </h2>
	<ol>
		<li><a href="HTML.html" title="HTML'^'">HTML </a> </li>
		<li><a href="CSS.html"  title="CSS^3^">CSS </a> </li>
		<li><a href="JAVASCRIPT.html"  title="JoJ">JAVASCRIPT </a> </li>
	</ol>
<p> 
	<img src = "http://cafefiles.naver.net/MjAxODA4MTdfMTgy/MDAxNTM0NDg1MzM0MzAx.CAv19LsXHAS364oYvMHPBHv-dX6s20qUCA5T-fgNsJgg.m953UNrkbxgDMbbeQ91woL2fb_WJOpA2oaEbnrjoEDcg.PNG.rg_rg/%C1%A4%BA%B8%BA%B8%C8%A3%BF%EE%BF%B50.PNG">
	안뇽^___^!!
</p>
</body>

</html>

<hr/><hr/>

>index.html

<!DOCTYPE html>
<html>
<head>
	<title> index </title>
	<meta charset="utf-8">
</head>
<body>
	<h1> <a href="test.html"><u>Test</u> </a></h1>
	<a href = "https://www.google.co.kr/" target="_blank" title="google">d0d? </a>
	<h2> <a href="index.html"  title="WEB^__^"> WEB </a> </h2>
	
<p> 
	월드 와이드 웹(World Wide Web, WWW, W3)은 인터넷에 연결된 컴퓨터들을 통해 사람들이 정보를 공유할 수 있는 전 세계적인 정보 공간을 말한다. 간단히 웹(Web)이라 부르는 경우가 많다. 이 용어는 인터넷과 동의어로 쓰이는 경우가 많으나 엄격히 말해 서로 다른 개념이다. 웹은 전자 메일과 같이 인터넷 상에서 동작하는 하나의 서비스일 뿐이다. 그러나 1993년 이래로 웹은 인터넷 구조의 절대적 위치를 차지하고 있다.
</p>
</body>

</html>

<hr/><hr/>

> html.html

<!DOCTYPE html>
<html>
<head>
	<title> HTML </title>
	<meta charset="utf-8">
</head>
<body>
	<h1> <a href="test.html"><u>Test</u> </a></h1>
	<a href = "https://www.google.co.kr/" target="_blank" >d0d? </a>
	<ul>
		<li><a href="HTML.html" title="HTML'^'">HTML </a> </li>
	</ul>
<p> 
	<strong>HTML</strong>은 하이퍼텍스트 마크업 언어(HyperText Markup Language, 문화어: 초본문표식달기언어, 하이퍼본문표식달기언어)라는 의미의 웹 페이지를 위한 지배적인 마크업 언어다.
</p>
</body>

</html>

<hr/><hr/>
> css.html

<!DOCTYPE html>
<html>
<head>
	<title> CSS </title>
	<meta charset="utf-8">
</head>
<body>
	<h1> <a href="test.html"><u>Test</u> </a></h1>
	<a href = "https://www.google.co.kr/" target="_blank" >d0d? </a>
	<ul>
		<li><a href="CSS.html"  title="CSS^3^">CSS </a> </li>
	</ul>
<p> 
	종속형 시트 또는 캐스케이딩 스타일 시트(Cascading Style Sheets, CSS)는 마크업 언어가 실제 표시되는 방법을 기술하는 언어로[1], HTML과 XHTML에 주로 쓰이며, XML에서도 사용할 수 있다. W3C의 표준이며, 레이아웃과 스타일을 정의할 때의 자유도가 높다.

마크업 언어가 웹사이트의 몸체를 담당한다면 CSS는 옷과 액세서리 같은 꾸미는 역할을 담당한다고 할 수 있다. 즉, HTML 구조는 그대로 두고 CSS 파일만 변경해도 전혀 다른 웹사이트처럼 꾸밀 수 있다.

현재 개발 중인 CSS3의 경우 그림자 효과, 그라데이션, 변형 등 그래픽 편집 프로그램으로 제작한 이미지를 대체할 수 있는 기능이 추가되었다. 또한 다양한 애니메이션 기능이 추가되어 어도비 플래시를 어느 정도 대체하고 있다.
</p>
</body>

</html>

<hr/><hr/>

> javascript.html

<!DOCTYPE html>
<html>
<head>
	<title> Javascript </title>
	<meta charset="utf-8">
</head>
<body>
	<h1> <a href="test.html"><u>Test</u> </a></h1>
	<a href = "https://www.google.co.kr/" target="_blank" >d0d? </a>
	<ul>
		<li><a href="JAVASCRIPT.html"  title="JoJ">JAVASCRIPT </a> </li>
	</ul>
<p> 
	자바스크립트(영어: JavaScript)는 객체 기반의 스크립트 프로그래밍 언어이다. 이 언어는 웹 브라우저 내에서 주로 사용하며, 다른 응용 프로그램의 내장 객체에도 접근할 수 있는 기능을 가지고 있다. 또한 Node.js와 같은 런타임 환경과 같이 서버 사이드 네트워크 프로그래밍에도 사용되고 있다. 자바스크립트는 본래 넷스케이프 커뮤니케이션즈 코퍼레이션의 브렌던 아이크(Brendan Eich)가 처음에는 모카(Mocha)라는 이름으로, 나중에는 라이브스크립트(LiveScript)라는 이름으로 개발하였으며, 최종적으로 자바스크립트가 되었다. 자바스크립트가 썬 마이크로시스템즈의 자바와 구문(syntax)이 유사한 점도 있지만, 이는 사실 두 언어 모두 C 언어의 기본 구문에 바탕을 뒀기 때문이고, 자바와 자바스크립트는 직접적인 관련성이 없다. 이름과 구문 외에는 자바보다 셀프와 유사성이 많다.
</p>
</body>

</html>