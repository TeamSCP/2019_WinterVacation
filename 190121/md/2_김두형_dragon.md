## Pwnable.kr - Dragon

------

2019.01.21 @ Plit00

이문제는 4일동안 갈렸다..

먼저 이 문제의 핵심을 말하자면

- Integer overflow
- UAF 

```
integer overflow
-정수형의 크기는 고정되어 있는데 정수형이 저장할 수 있는 값의 크기보다 큰 값을 저장하려 할때 예상치 않은 문제가 발생하는것 

UAF
-heap 영역에서 할당된 malloc공간을 free하고 reuse할때 생기는 취약점(이는 heap 메모리를 좀 더 효율적으로 사용하기 위해 반환된 heap영역의 크기를 기억해놨다가 같은 크기의 할당 요청이 들어오면 이전 영역을 재사용하게 합니다.

```

자 먼저 문제를 보자!

<img width="328" alt="1" src="https://user-images.githubusercontent.com/40850499/51453776-b1f4a680-1d84-11e9-8dd8-fd4fe4bd7795.png">



ida를 까서 분석을 해보던 도중 문제를 풀게되면 직업 선택에서 1,2를 선택할 수 있게되는데
이외의 레벨이 있다는사실을 깨닫고 다시 문제에 접속해보니 3을 치면 secretlevel에 접속할 수 있다.

자 새로운 레벨이 있다는것은 저것을 이용해서 문제를 풀라는 것으로 생각하고 
gdb로 넘어가서 함수마다의 분석을 시작했다.

<img width="387" alt="2" src="https://user-images.githubusercontent.com/40850499/51453777-b1f4a680-1d84-11e9-8eef-58caaf5181f3.png">

함수를 보니 ida에서 봤던 것처럼 SecretLevel이 있다.
그렇다면 이것에서는 무엇을 하는지 알아보자.

<img width="456" alt="3" src="https://user-images.githubusercontent.com/40850499/51453778-b1f4a680-1d84-11e9-880e-2f145f3669d5.png">



여기서는 비밀번호를 입력받고 문자열과 비교해서 일치한다면 쉘을 던져준다.
그래서 입력 문자열포맷과 비밀번호를 비교해보았더니 



<img width="500" alt="4" src="https://user-images.githubusercontent.com/40850499/51453779-b28d3d00-1d84-11e9-9a00-a335e37b1c82.png">

scanf로 10글자를 봤는데 입력해야할 문자열이 10글자가 넘는다...
여기서 또 다시 고민을 하게됐는데.

저 문자열을 쓸 수 있는 방법은 system("/bin/sh")이 실행되도록 0x08048dbf 이 주소로 실행흐름을 바꾸면 된다.



<img width="500" alt="5" src="https://user-images.githubusercontent.com/40850499/51453780-b28d3d00-1d84-11e9-95bd-a2b009715ca8.png">



FightDragon 함수에서 찾은것인데 이게 우리가 풀 수 있는 길을 알려주는것이였다.
이분의 scanf는 특별했는데 위에서 malloc을 해주고 그곳에서 입력을 받는다 
또한 

<img width="400" alt="6" src="https://user-images.githubusercontent.com/40850499/51453781-b28d3d00-1d84-11e9-81cc-3e0e1c7a06cf.png">



16글자를 입력받는다 아까와의 10글자와 다르게 말이야
이것을 보는순간 여기에 payload를 넣을 수 있다는 생각이 들었다.

<img width="490" alt="7" src="https://user-images.githubusercontent.com/40850499/51453782-b325d380-1d84-11e9-97e9-717af1a72f6e.png">

칼공격함수가 끝난후에 eax값에 ebp-0x18이라는 값을 넣고 
이 값이 0이 아니어야 넣을 수 있는 것이 생기는데,이것은 즉 용을 죽일 수 있다는 말을 뜻한다.

<img width="510" alt="8" src="https://user-images.githubusercontent.com/40850499/51453783-b325d380-1d84-11e9-8a39-98dd765e135e.png">

dragon의 구조체를 보면 보면
첫번째는 몬스터 정보를 출력하는 함수 주소이고 , 두번째는 몬스터 체력과 그옆에 5가 체력회복량을 나타낸다. 이 정보를 보면 몬스터의 체력이 바이트단위로 설정되있다는것을 알 수 있다.
범위는 -127~127 이다.

<img width="450" alt="9" src="https://user-images.githubusercontent.com/40850499/51453784-b325d380-1d84-11e9-84b7-6c16a729022a.png">



테스트를 해본결과 가정사실이 맞았다.
이제 우리는 위와 같이 16글자 16바이트를 넣을 수 있다.

취약점을 생각해보면 
scanf로 입력받고 아래 코드를 보면 call eax가 보인다. 

<img width="500" alt="10" src="https://user-images.githubusercontent.com/40850499/51453785-b325d380-1d84-11e9-876e-4d6c335cf62f.png">

이것은 첫번째 인자인 몬스터정보 출력 함수를 호출하고 있다. 16바이트의 값을 입력한 부분이 malloc으로 받은 부분이니 이주소가 전에 free가 되었던 heap address이다. 16바이트 주소에 AAAA를 입력하였다 그럼 call eax에서 이주소가 몬스터 정보 출력 함수인줄 알고 이주소를 호출하려 할 것이다

<img width="500" alt="11" src="https://user-images.githubusercontent.com/40850499/51453786-b3be6a00-1d84-11e9-96c3-ea911d7908fd.png">



마지막으로는 입력할때 secretlevel에서의 쉘을 실행시키는 주소를 입력해주고 pwn을 이용해 익스를 짜면 쉘을 획득할 수 있다.

```python
from pwn import *
p = remote('pwnable.kr', 9004) 
p.recv() 
p.send('1\n' + ('3\n3\n2\n' * 2) + '1\n' + ('3\n3\n2\n' * 4)) 

p.recvuntil('As:\n') p.sendline(p32(0x08048dbf)) p.interactive()
```







