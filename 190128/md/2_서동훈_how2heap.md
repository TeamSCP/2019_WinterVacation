## How2heap 

####how2heap중(first fit,fastbin dup,fastbin dup into stack,fastbin dup consolidate)에 관한 문서이다.

>glibc_version 2.11.2 

>소스코드는 실제와 다를수있음
>
>fastbin dup는 into stack 과 거의 유사함으로 따로 없음


###firtst_fit
---
아래는 first fit의 소스코드이다

![k-20190131-090819](https://user-images.githubusercontent.com/37978105/52280442-4115d700-299f-11e9-9d6f-17e49518983e.png)


아래 사진은  힙 상황을 간단히 표현을 한것이다.

![k-20190206-017600](https://user-images.githubusercontent.com/37978105/52283693-4c203580-29a6-11e9-8abf-610ad111e86d.png)

14번쨰 에 this is A가 들어간 이후의 메모리 이다.
여기서 0x209는 chunk A의 사이즈 이다.(정확히는 p flag를 뺸 0x208) 그리고 mem에 this is A가 들어간 모습을 볼 수 있다.

![k-20190131-015242_a](https://user-images.githubusercontent.com/37978105/52283957-d8caf380-29a6-11e9-95a6-8c3dad843193.png)

a가 free 된 이후 fd,bk에 unsorted bin 주소가 들어간 모습이다.

![k-20190131-015306](https://user-images.githubusercontent.com/37978105/52284228-53940e80-29a7-11e9-8a76-dc9ad9e090d2.png)


c 가 malloc 된 이후 이며, size를 보면 c에 size에 맞게 바뀌었으며  fd,bk에 unsorted bin주소는 다른 주소로 바뀌었고 fd_nextsize와 bk_nextsize가 생긴것을 볼 수 있다.
![k-20190131-015399](https://user-images.githubusercontent.com/37978105/52299813-bac2ba80-29c9-11e9-8267-c685245b915a.png)

아래는 c가 a자리를 사용하게 되면서 원래 a크기의 c의 크기가 차지했을떄 남은 크기가 (512-500=12 지만 청크의 크기는 8바이트 단위 이므로 16이 된것을 볼수있다) 그리고 fd,bk는 unsorted bin을 가리키고 있다.

![k-20190131-01539889](https://user-images.githubusercontent.com/37978105/52300127-6a982800-29ca-11e9-8f5c-07b937fe4266.png)

c에 this is C가 들어간 모습이다.

![k-20190131-202120](https://user-images.githubusercontent.com/37978105/52300331-f01bd800-29ca-11e9-9cd8-05eb75469fd4.png)

아래는 firtst fit의 실행결과.

![k-20190131-202547](https://user-images.githubusercontent.com/37978105/52300417-1ccfef80-29cb-11e9-897a-c9201ee543b0.png)


###fastbin_ dup_ into_stack
---

fastbin _dup _into _stack 소스코드 이다.
 
![k-20190131-091028](https://user-images.githubusercontent.com/37978105/52300586-95cf4700-29cb-11e9-9c6e-4bb0d3743721.png)

free(a)가 되기 전까지의 heap 상황

![k-20190131-037896](https://user-images.githubusercontent.com/37978105/52300925-78e74380-29cc-11e9-92ea-18541af6a3c1.png)

free(a)가 되어 fastbin에 a의 주소가 들어간것을 볼수있다.

>fastbin에는 가장 최근에 free된 주소가 들어간다.


![k-20190131-037993](https://user-images.githubusercontent.com/37978105/52300970-987e6c00-29cc-11e9-831f-98c53c0cf573.png)

free(b)가 된후 b chunk의 fd를 보면 a chunk 의 주소를 가리키고 있고 fastbin에는 b 주소가 들어갔다.

![k-20190131-038271](https://user-images.githubusercontent.com/37978105/52301025-bea40c00-29cc-11e9-9cc9-b0a03d473351.png)

free(a)가 되고 fastbin에는 a의 주소 a chunk의 fd에는 b의 주소가 저장이 되었다.

이렇게 되면 fastbin은 0x804a000->0x804a010->0x804a000->0x804a010 와 같이 진행이 된다.

fastbin 은 fast chunk가 여러개 해제가 되었을때  chunk의 fd를 보고 관리를 하게 되는대 이떄 double free 로 인해 fd가 서로를 가리켜 이러한 현상이 일어나게 되는것이다.

![k-20190131-038350](https://user-images.githubusercontent.com/37978105/52302527-da111600-29d0-11e9-945a-28ae5d1ec069.png)

동일한 크기의 메모리가 호출이 되어 fastbin(0x804a000->0x804a010)이 되었다.
![k-20190131-038449](https://user-images.githubusercontent.com/37978105/52302925-f2356500-29d1-11e9-9537-1495d0f5b59c.png)

아래는 소스코드 22번이 진행된 이후에 모습이다. 우선 fd,bk에 변화를 먼저 말하자면 16번에서 할당된 d는 a chunk에 위치를 하게되는대 이때 d에 userdata는 free a chunk의 fd,bk와 같은 위치 이므로 fd를 스택영역의 주소로 바꿔주면 스택 영역도 할당할 수 있게된다.

예상을 해보자면  0x804a000가 할당-> 0x804a000의 fd(0xbffffc84) 가 다음 fastbin에 저장 -> 0xbffffc84가 할당 이 될것이다. 
![k-20190131-038659](https://user-images.githubusercontent.com/37978105/52303063-59531980-29d2-11e9-8b85-da6cd678026d.png)

0x804a000이 할당되어 fd인 0xbffffc84가 fastbin에 들어간 것을볼 수 있다.

![k-20190131-038763](https://user-images.githubusercontent.com/37978105/52303308-fe6df200-29d2-11e9-8153-921dcce14a68.png)

이후 스택 영역을 할당 이 되야 하지만 오류로 진행x

###fastbin _dup _consolidate
---

아래는 소스코드.

![k-20190131-091298](https://user-images.githubusercontent.com/37978105/52303802-47727600-29d4-11e9-9c39-fbf5799cac3b.png)

이 문제는 malloc_consolidate()라는 특정 함수로 인해 일어나는 현상이다.

free(p1)이 진행된후 메모리 이다. fastbin에는 p1 chunk의 주소가 들어간것을 볼 수 있다.

![k-20190131-068427](https://user-images.githubusercontent.com/37978105/52304645-8e616b00-29d6-11e9-9e6c-5e6446dddc6a.png)

largebin(p3)이 할당되면서 malloc_consolidate()호출이 된다 이후 p1이 unsortedbin으로 들어가게된다.
 
>malloc_consolidate()는 large bin 이상의 크기로 동적할당 요청이 들어오면 다음 chunk가 top chunk인지 확인후 fastbin을 검사하여 small bin으로 병합을 한다.

![k-20190131-068647](https://user-images.githubusercontent.com/37978105/52304807-e1d3b900-29d6-11e9-9dac-ef113d478e49.png)

이후 free(p1)이 되면 small bin에 p1이 저장되며 

![inkedk-20190131-069232_li](https://user-images.githubusercontent.com/37978105/52305098-a5548d00-29d7-11e9-85c0-ef5a9f0113b1.jpg)

fastbin 또한 p1의 주소가 들어가있는 것 을 볼 수 있다.

![k-20190131-070591](https://user-images.githubusercontent.com/37978105/52305387-50654680-29d8-11e9-942d-5ab972edf331.png)

fastbin,smallbin 두곳에 p1이 있으므로 할당한 두 메모리의 주소가 같게 된것이다.

![inkedk-20190131-203196_li](https://user-images.githubusercontent.com/37978105/52305482-8dc9d400-29d8-11e9-906d-7f4d11da67af.jpg)
