## 기술문서 1주차

#### wargame.kr의 login filtering 문제 풀이



![img](http://cfile9.uf.tistory.com/image/9916F8475C2EF32429AF90)

문제를 보고 계정이 잠겨있고 이것을 우회하여 풀어내야한다는 것을 예상할 수  있다.







![img](http://cfile6.uf.tistory.com/image/994A49475C2EF324269177)



ID와 PW를 입력할 수 있는 로그인 폼이 보인다.

이걸로는 힌트도 얻지 못하므로 get source를 눌러보았다.

그러자 소스가 나타났다. 쭉 살펴보니 신경이 쓰이는 부분들이 나타났다.







![img](http://cfile25.uf.tistory.com/image/998AEB4C5C2EF40E284667)



그래서 바로 mysql_real_escape_string이 무엇인지를 구글신에 검색해보았다.

그러자 구글신은 mysql_real_escape_string은 mysql_query에서 특수 문자열을 이스케이프하기 위해 사용되는 함수라고 말해주었다.

쉽게 말해 sql injection 공격법을 방어하기 위한 함수라는 것을 알 수 있었다.

그래서 mysql_real_escape_string을 우회하여 sql injection을 사용하는 방법에 대해 찾아보았다.



> mysql_real_escape_string 우회법
>
>  - 특정조건 하에서 가능 (멀티바이트를 사용하는 언어로 인코딩 할 시)
>  - \ 앞에 &a1_%fe 의 값이 들어가면 해당 값과 \에 해당하는 %5c가 합쳐져서 하나의 문자를 표현하게 됨
>  - sql injection 방어를 위해 생성한 \가 사라져서 공격이 가능해짐



공부를 하고 문제 풀이에 적용을 해보았지만 계속해서 답은 틀렸다고 나왔다.

그래서 sql injection 문제가 아니라는 생각이 들었다.







![img](http://cfile7.uf.tistory.com/image/99E61F475C2EF560304B50)

그래서 아래 부분을 보니 $row에 id 변수값이 존재할 경우

 id가 guest이거나 blueh4g일 때 계정이 잠겨있다는 메세지가 출력되도록 설정되어있고

 그것이 아니라면 key 값을 보여달라고 설정되어있다.

그리고 $row에 id 변수 값이 존재하지 않을 경우 틀렸다는 메세지가 출력되도록 설정되어있다.









![img](http://cfile22.uf.tistory.com/image/991368425C2EF6F62AF113)





그리고 마지막 부분 잠겨있는 계정의 id와 pw값이 제공되어있었다.



그렇다면 $row에 존재하는 id이면서 guest와 blueh4g는 아닌 계정을 찾아야한다는 것이 이 문제의 정체라는 것을 알게 되었다.

그리고 곧 php는 대,소문자를 구분하지만 쿼리는 대,소문자를 구분하지 않는다는 것을 깨닫게 되었고 이 문제를 해결하기 위한 방법을 찾았다.

바로 id 입력시 대,소문자를 섞어줘야 한다는 것이였다.



그렇게 id에 guesT pw에 guest를 입력해주자 바로 FLAG 값이 나오게 되었다.



------





![img](http://cfile8.uf.tistory.com/image/999F1B3F5C2EF89C33BD2B)



이렇게 4번 문제도 해결할 수 있었다!







<4번 문제를 통해 공부한 것>



> mysql_real_escape_string은 mysql_query에서 특수 문자열을 이스케이프하기 위해 사용되는 함수

>  php - 대,소문자 구분 O
>
>  쿼리 - 대,소문자 구분 X





------

#### 웹해킹 공부 - 난독화



해킹의 기본 - IDS/IPS 탐지 우회하는 것

> IDS (Instrusion Detection System)
>
> ​	- 침입 탐지 시스템. 시스템에 대한 원치않는 조작 탐지

> IPS (Intrusion Prevention System)
>
>  - 침입 차단 시스템. 외부 네트워크로부터 내부 네트워크로 침입하는 네트워크 패킷을 찾아  제어하는 기능을 하진 소프트웨어&하드웨어



난독화 : 우회 기법을 통해 탐지를 어렵게 함

난독화의 종류 - 1. 소스코드 난독화 

​	                    2. 바이너리 난독화



![](C:\Users\박지윤\Desktop\캡처.PNG)

​		보호 기능 												해킹 기능	









