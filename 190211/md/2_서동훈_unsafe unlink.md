





##unsafe_unlink!

unsafe_unlink에 대해 알아보자.

##unlink?

unlink를 이용하기전 unlink에 대해 간단히 알아보자.

다음과 같이 더블 링크드 리스트가 있다고할떄.

<img src="https://user-images.githubusercontent.com/37978105/52841552-f4b95c80-313f-11e9-980e-7d7dde97ddfe.png" width=400>

p가 해제가 되면서 BK와 FD를 병합하는 과정을 하는것이 unlink함수이다.

<img src="https://user-images.githubusercontent.com/37978105/52845302-1c152700-314a-11e9-8850-156af88431aa.png" width=400>

이 unlink함수는 2가지 security check를 진행 하는대 이 2가지를 우회해야 unlink를 이용할수 있다. 

<img src="https://user-images.githubusercontent.com/37978105/52845403-65fe0d00-314a-11e9-80ee-894b97608aa1.png" width=400>

##exploit plan

실습환경
>ubuntu 14.04.5 LTS

>glibc 2.19

>64bit

소스코드는 다음과 같다

이 실습에는 몇가지 조건이 있다.
공격자에 의해 생성된 heap영역이 전역변수에서 관리 되어야 하며
0x80이상을 할당해야한다(fastbin size면 x) 첫번쨰 chunk에 fake chunk를 저장할수 있어야 하며 두번쨰 chunk의 header를 덮어쓰고 해제 할수 있어야한다. 

![k-012](https://user-images.githubusercontent.com/37978105/52845886-9003ff00-314b-11e9-8607-7e9ceb4a03f9.png)


위에 부터 전역변수 buf1 첫번쨰,chunk 두번쨰chunk ,top_chunk

<img src="https://user-images.githubusercontent.com/37978105/52846203-61d2ef00-314c-11e9-8d68-166ee3f06acb.png" width=200>

다음과 같이 fake chunk를 만들어 준다. fake chunk는 unlink의 security check를 우회하기 위해 만들어 주는 것이다. fake chunk에 자세한 설명은 뒤에.

<img src="https://user-images.githubusercontent.com/37978105/52846980-5e406780-314e-11e9-8933-4d9ac988ed5d.png" width=200 >

그리고 두번쨰 chunk의 prev_size와 size를 다음과 같이 덮어씌어 준다 

prev_size=0x80
>으로 한것은 한 chunk가 이전 chunk를 찾을떄 prev_size를 자신의 주소에서 빼는것으로 찾기때문에 0x80으로 해서 이전 청크의 시작이 0x602010으로  만들어지게끔 한것

size=0x90
>p플래그 해제로 앞 chunk가 해제된것으로 만들기위해 

<img src="https://user-images.githubusercontent.com/37978105/52852295-84b8cf80-315b-11e9-9f96-772ea240847d.png" width=200>

이후 free(buf2)가 진행되며 chunk 병합이 일어나고 unlink 호출로 인해  전역변수 buf1 의 0x602010-->fake chunk(fd) 로 바뀌어 진다.

<img src="https://user-images.githubusercontent.com/37978105/52852299-87b3c000-315b-11e9-9b2c-a6615eb18303.png"width=200>

fake chunk의 fd,bk를 각 각 chunk의 시작인 prev_size의 주소라고 할때 fake chunk의 FD->bk 와 BK->fd가 같은 장소를 가르키고 있다 이것으로 1번쨰 security check를 우회할수있다.

fake chunk의 prev_ size,size = 0인것은 2번쨰 security check(chunk size!=next chunk prev_size)를 우회해야 된다 이것은 fake chunk prev_size=size=0x0으로 할떄 **size=0x0일때 다음 chunk를 가리키려할때 0x0에 다음 chunk는 fake chunk가 되어서 fake chunk의 prev_size=0x0임으로 2번쨰 security chunk 우회 된것이다.**
 
free 함수에서 fake chunk bk영역을 다음 chunk가 존재 한다고 판단해서 전역변수에 fake chunk의 fd값이 들어간것이다.

![8](https://user-images.githubusercontent.com/37978105/52860828-f0a73200-3173-11e9-839d-bc82f8acafb6.png)

buf1에 0x601060-0x601048이 되면서 0x601060 값을 덮어 쓸수 있게된것이다. 즉 다음과 같이 스택 영역 주소로 덮어주면 스택 영역을  사용할수 있는 것이다.

<img src="https://user-images.githubusercontent.com/37978105/52863021-fc95f280-3179-11e9-9450-074577430a98.png" width=200>

다음과 같이 스택영역에 원하는 값을 저장할수 있다.

<img src="https://user-images.githubusercontent.com/37978105/52863145-6c0be200-317a-11e9-80b5-dbb0edcc1703.png" width=400>