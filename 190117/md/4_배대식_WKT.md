## All Contents

→ Windows Kernel Tutorial<br>
__ All About DLL<br>
＿ All About Driver<br>
＿ CE Dematerializer<br>
＿ Broken DEMACIA<br>

## Windows Kernel Tutorial

해킹에 대해서 이야기 할 때 커널에 관한 이야기가 엄청 많이 나옵니다.<br>
흥미 있는 주제들에 대해서만 이야기 해보자면..<br>

- 게임 매크로를 만들려면 SSDT 후킹을 할 줄 알아야 한다.
- 보안 모듈을 우회하기 위해선 커널을 잘 알아야 한다.
- PC방에 소켓 프록시를 숨겨 놓기 위해는 커널을 잘 알아야 한다.

커널이 무엇이고 왜 나쁜 짓을 방어하는 기술들은 커널에 치중되어 있을까요?<br>
또한 앞으로 연재 할 컨텐츠를 이해 혹은 학습 하기 위해서 필요한 지식들에 대해서 이야기 해 보도록 하겠습니다.<br>

## DLL
Dynamic Linked Library, 동적으로 연결되는 라이브러리라는 의미입니다.<br>
프로그램이 메모리에 올라오는 시점에 라이브러리의 참조가 이루어 지므로 유지보수를 따로 할 수 있다는 점이 있습니다.<br>
윈도우에서 기본적으로 제공하는 여러 API들 또한 DLL로 이루어진 경우가 많으며 DLL의 EAT를 참조하여, PE 로더가 IAT를 채워주는 방식으로 구성되어 있습니다.<br>
프로그래머는 이러한 API를 사용 하는데 있어서 문서를 참조하여 인자만 채워주면 그 기능을 사용 할 수 있게 됩니다.<br> 

## API
Application Programming Interface 줄여서 API라고 불리웁니다.<br>
프로그래머가 자주 하는 작업 혹은 어려운 작업들을 단순히 인자 몇 개와 플래그 셋팅으로 결과를 얻어 올 수 있는 효율적인 수단인 것이죠.<br>
즉, API들은 문서화가 되어 있습니다. Windows API들이라면 MSDN에 NaverAPI라면 네이버 개발자 센터에 문서화가 되어 있는 것처럼요.<br>

<MSDN에 정의되어 있는 스펙 이미지><br>
<네이버에 정의되어 있는 스펙 이미지><br>

## SYS


그렇다면 API의 동작 방식을 아는 것이 왜 커널을 이해 하는데 도움이 되는 것일까요?<br>

## Map of User and Kernel
<OS Internal Kernel Map>
  - 익스큐티브
  - 커널

## Process

=> 커널
=> 시스템 프로세스<br>
=> 서비스 프로세스<br>
=> 유저 프로세스<br>



## 결론
여기까지 하이레벨에서 로우레벨 까지의 과정을 간략하게나마 살펴 보았습니다.<br>
다음은 드라이버를 만드는 방법과 게임에서 어떻게 우회되어 사용되는지에 대해 이야기 해보도록 하겠습니다.<br>
