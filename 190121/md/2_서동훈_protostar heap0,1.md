##protostar heap0,1

protostar heap0,1 은 heap도 overflow가 된다 정도? 를 알수있는 문제이기에 사실상 버퍼오버플로우 문제이다. UAF나 double-free attack 같은 문제가아님.

---
###heap0

아래 그림은 heap0의 소스코드이다.
![Alt text](https://user-images.githubusercontent.com/37978105/51453653-03506600-1d84-11e9-910f-3e1c622b01cc.png)

코드를 보면 data와 fp 구조체를 선언하였고 구조체 포인터 d,f에 각각의 구조체 크기만큼의 메모리를 할당하고있다.
fp구조체 fp함수포인터에 nowinner라는 함수의 주소를 넣어주고
data구조체 name변수에 strcpy로 argv[1]의 값을 복사시킨후 nowinner함수를 실행시키며 종료가 된다.

우선 28번쨰 줄에서 strcpy함수로인한 heap overflow가 일어난다는것은 간단히 알수있다.

이로서 이 문제는 strcpy로 오버플로우를 일으켜 winner함수를 실행시키면 된다는 것 을 알수있다.

더 자세히 알아보기위해 gdb를 이용해보자.

아래 그림은 main의 어셈블리이다.

![Alt text](https://user-images.githubusercontent.com/37978105/51453675-24b15200-1d84-11e9-8ec0-7bd0f959f093.png)


argv[1]에 값을넣고 이후의 메모리 상황을 보기위해 strcpy함수 이후인 main+111에 브레이크 포인트를 잡고 AAAA넣고 실행을하였다.
![Alt text](https://user-images.githubusercontent.com/37978105/51453693-3eeb3000-1d84-11e9-84d9-6f4803f24ed8.png)

아래그림은 이후 메모리 상황이다.
![Alt text](https://user-images.githubusercontent.com/37978105/51453696-427eb700-1d84-11e9-9472-2bcb49910246.png)

(**p_flag를 제외한값이 실제 크기**)

첫 chunk의 크기가 0x49인것 을 볼수있다.
>0x49=73은 8(chunk header)+1(p_flag)+64(mem)=73(0x49) 

argv[1]에 AAAA도 메모리 안에 있는것을 볼수있다.

다음 chunk 0x11
>0x11=17은 8(chunk header)+1(p_flag)+8(mem)=17(0x11)

**fp구조체가 4byte지만 32비트에서 chunk size는 8byte단위이므로16byte가 할당된것이다.**

알기쉽게 위 두chunk를 각 각 d_chunk,p_chunk라 하 

0x08048478의주소는 nowinner의 함수이다.

아래그림은자 페이로드에사용할 winner 함수의 주소

![Alt text](https://user-images.githubusercontent.com/37978105/51453714-5a563b00-1d84-11e9-8b1f-a1b4b5b59fca.png)


자! 이제 페이로드를 작성해보자

64(d_chunk mem)+8(p_chunk header)+winner_addr
즉 72byte(dummy)+winner_addr이면 될것이다.

![Alt text](https://user-images.githubusercontent.com/37978105/51453724-76f27300-1d84-11e9-96ab-ea6e6965db8a.png)
와! 성공

---
###heap1

다음은 heap1의 문제이다 아래는 heap1의 소스코드이다.

![Alt text](https://user-images.githubusercontent.com/37978105/51453773-ac975c00-1d84-11e9-91f6-da1c8d3f4d4c.png)

internet이라는 구조체를 선언하고 i1,i2 구조체 포인터에 internet구조체의 크기만큼 메모리를 할당하고있다.
그리고 각 각의 internet구조체의 name멤버변수에는 8byte만큼 메모리를 할당하고있다. 그후 strcpy함수를이용해 각 각의 name 변수에 argv[1],argv[2]의 값을 복사해주고 있다.

gdb로 자세히 알아보자

아래 그림은 main의 어셈블리이다.

![Alt text](https://user-images.githubusercontent.com/37978105/51453800-bfaa2c00-1d84-11e9-9f3d-a6d0472d5e83.png)


이번에도 strcpy이후의 메모리를 보기위해 main+156이후로 브레이크 포인터를 잡고 argv[1]에는 AAAA argv[2]에는 BBBB를 넣어 실행하였다.

![Alt text](https://user-images.githubusercontent.com/37978105/51453837-0a2ba880-1d85-11e9-8e0d-accb66dea63e.png)

아래의 그림을보면 얼추 감이 잡혔을것이다. argv[1]에서 오버플로우 시켜 0x0804a038을 변조시키고 argv[2]에 넣은값이  그 변조된 주소에 저장이 된다.
![Alt text](https://user-images.githubusercontent.com/37978105/51453853-20d1ff80-1d85-11e9-82fe-a7a17d581a97.png)

main+168에서 puts 함수를 사용하고있는대 이 puts_got를 winner함수로 overwrite시키면 끝이다.

![Alt text](https://user-images.githubusercontent.com/37978105/51453880-42cb8200-1d85-11e9-8f65-166762e99761.png)

puts_got=0x8049774,winner_addr=0x8048494
페이로드를 짜보면  8byte(i1->name mem)+ 8byte(i2_chunk_header)+4byte(i2_priority)+puts_got+  +winner_addr이면 끝이다.

![Alt text](https://user-images.githubusercontent.com/37978105/51453951-a48bec00-1d85-11e9-804d-39f21785fccc.png)

![Alt text](https://user-images.githubusercontent.com/37978105/51453960-aeadea80-1d85-11e9-86c5-b830d0896911.png)

끄읕.
