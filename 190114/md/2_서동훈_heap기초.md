##What is Heap?
2019-01-14 서동훈

heap관련문제를 풀기위해 heap공부를 해보자 

와! <u>heap</u> <del>ppap

----
###Heap and Stack
힙(heap)이란? 간단하게 프로그램이 실행되는 도중에 동적으로 할당하고 해제하는 메모리 영역이라고 보면 된다. 
지금까지 사용했던 스택과는 어떤 차이가 있을까?

다음 그림은 메모리 공간을 표현한 것이다.


![Alt text](http://tcpschool.com/lectures/img_c_memory_structure.png)

힙영역은 스택 영역보다 낮은 주소에 위치하며 메모리를 할당할때 낮은 주소에서 높은 주소의 방향으로 할당이되지만 스택은 높은주소에서 낮은 주소의 방향으로 할당된다. 또한 스택은 엄격히 후입선출(LIFO, Last-In First-Out)방식에 따라 동작하지만 힙은 프로그램이 요구하는 일정한 규칙이 없다. 메모리 할당 또한 차이가 있다 힙은 사용자에 의해 메모리 공간이 동적으로 할당되고 해제되지만 스택은 함수의 호출과 할당되며,호출이 완료되면 소멸한다.

---
###heeeeap
이후로는 
 GNU C library(glibc) 에서 사용되는 ptmalloc2(dlmalloc에서 스레딩 지원 기능 추가가 된것)의 메모리 관리 방식이다. 

heap은 동적 메모리 할당과 해제는 어떻게 하는것일까?

> char *p = malloc(size) :원하는 size만큼 malloc을 호출하여 동적 메모리 할당 
>	free(p)  :사용한 메모리를 free하여 반환

이렇게 할당된 메모리는 peda - vmmap으로 확인을 할 수 있다.

아래 그림은 할당 전과 후로 나누어진다.

![k-20190114-055774](https://user-images.githubusercontent.com/37978105/51090033-44131280-17b9-11e9-8573-2acac57606f2.png)

![k-20190114-056050](https://user-images.githubusercontent.com/37978105/51090035-483f3000-17b9-11e9-850a-6f1800e18380.png)


사진을 보면 알겠지만 특정 크기의 메모리 영역을 미리 할당한 뒤 이 영역을 사용하는 방식이다.

---
###chunk 구조
chunk(동적 메모리로 할당되는 영역)

다음 그림은 chunk의 자료구조


![c0098335_4b34adcad42cd_1](https://user-images.githubusercontent.com/37978105/51085151-9122c480-1778-11e9-8494-87f5550900b2.png)

>**prev_size**=이전 chunk의 크기 이전 chunk가 free() 되면 초기화된다

>**size**=현 chunk의 크기
>>32bit 8바이트,64bit는 16바이트 단위로 정렬

>>**N = NON_ MAIN_ARENA**플래그는 muti-thread application에서 각 thread마다 다른 heap 영역을 사용하는 경우,현재 chunk가 main heap(arena)에 속하는지 여부를 나타낸다.

>>**M = IS_MMAPPED**플래그는 해당 필드가 mmap()시스템 콜을 통해 할당된 것인지를 나타낸다.
>
>>**P = PREV_INUSE**플래그는 이전 chunk가 사용 중인지 여부를 나타내는 것이다.
>N은 속할떄  M,P는 할당 될떄  1 아니면 0
>

>**mem**= 실제 우리가 사용하는 메모리영역
>
>**actual chucnk** 다음 chunk의 prev_size 도 현재 chunk의 데이터 영역으로 사용된다 개념적으로 현재 chunk의 속한다함
>>free()시 현재 chunk의 크기를 (중복하여)저장하는 용도로 사용되는대(플래그 제외)이를 boundary tag 기법이라한다.

>**chunk2**는 free()된 후의 모습이다

>**fd = forward pointer**

>**bk = backward pointer**

> fd_ nextsize, bk_ nextsize 도 동일하지만 큰 크기의 chunk에서 사용된다.(그림에 bk,prev_size 사이에 위치)

>free() 시에는 다음 chunk의 p플래그를 지워야 하며 prev_size는 오직 P플래그가 지워진 경우에만 사용해야한다.
>
>

할당된 chunk들이 free()로 해제가되면 bin이라는 구조로 관리된다.

---
###Top chunk(wildness chunk)

>Allocated chunk는 malloc된 chunk라고 생각하면된다
>처음 Top chunk에서 0x22000 -> 0x21000

힙 영역의 가장 마지막에 위치하며 새롭게 할당(malloc)되면 top chunk에서 분리해서 반환하고 top chunk 에서 인접한 chunk가 free되면 병합한다

top chunk가 새로운 malloc이 요청되어 분리될때는 재사용가능한 chunk가 없으며 top chunk에서 분리하여 반환할 크기가 있을 떄 분리가 된다. 

아래 그림은 top chunk에서 새로운 malloc이 요청되어 분리되는 장면이다.
![Alt text](https://cdn.discordapp.com/attachments/533933042868420616/534125675116756992/K-007.png)


top chunk에 인접한 chunk가 free되면 병합(fastbin)제외

아래 그림은 free된 chunk가 합쳐지는 그림이다.

![Alt text](https://cdn.discordapp.com/attachments/533933042868420616/534127580027289640/K-009.png)


실습? 쪽은 pdf에만 있습니다.

우선 topchunk까지 내용이고 다음주에는 bin에대한 정리,
부족한 부분이 많아습니다요.