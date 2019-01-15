## All Contents

→ Windows Kernel Tutorial<br>
＿ All About Driver<br>
＿ CE Dematerializer<br>
＿ Broken DEMACIA<br>

## Windows Kernel Tutorial

해킹에 대해서 이야기 할 때 커널에 관한 이야기가 엄청 많이 나옵니다.<br>
흥미 있는 주제들에 대해서만 이야기 해보자면..<br>

- 게임 매크로를 만들려면 SSDT 후킹을 할 줄 알아야 한다.
- 보안 모듈을 우회하기 위해선 커널을 잘 알아야 한다.
- PC방에 소켓 프록시를 숨겨 놓기 위해는 커널을 잘 알아야 한다.

이것 외에도 커널 하면 뭔가 멋져보여서 혹은 넘을 수 없을 것만 같은 산이라고 생각하고 있었습니다.<br>
여러 군데에서 잡지식이 쌓이면서 만든 커널 튜토리얼 입니다.<br>

- API
ApplicationProgrammingInterface 줄여서 API라고 부르게 되죠?
어플리케이션을 프로그래밍을 하기 위한 인터페이스입니다.<br>
프로그래머가 자주 하는 작업 혹은 어려운 작업들을 단순히 인자 몇 개와 플래그 셋팅으로 결과를 얻어 올 수 있는 효율적인 수단인 것이죠.<br>
즉, API들은 문서화가 되어있습니다. Windows API들이라면 MSDN에 NaverAPI라면 네이버 개발자 센터에 정해져 있는 것처럼요.<br>

<MSDN에 정의되어 있는 스펙 이미지><br>
<네이버에 정의되어 있는 스펙 이미지><br>

그렇다면 이러한 API를 아는 것이 왜 커널을 이해하는데 도움이 되는 것일까요?




- DLL

- Process

- Map of User and Kernel
  - 익스큐티브
  - 커널
  - OBJECT

- 결론<br>
여기까지 하이레벨에서 로우레벨 까지의 과정을 간략하게나마 살펴 보았습니다.<br>
다음은 드라이버를 만드는 방법과 게임에서 어떻게 우회되어 사용되는지에 대해 이야기 해보도록 하겠습니다.<br>
