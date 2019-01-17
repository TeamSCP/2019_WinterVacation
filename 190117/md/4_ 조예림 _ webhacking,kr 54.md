# 19.01.17 webhacking,kr 54

바로 문제를 보면


![1](https://user-images.githubusercontent.com/40850499/51298331-7c3f7d00-1a67-11e9-9d21-78999918ee09.PNG)
![1-1](https://user-images.githubusercontent.com/40850499/51298332-7e094080-1a67-11e9-9ac3-f2f1305837fc.PNG)
![1-2](https://user-images.githubusercontent.com/40850499/51298333-7f3a6d80-1a67-11e9-93b8-da9431d80022.PNG)


이렇게 Password is 뒤에 값이 바뀌는 것을 알 수 가 있습니다.

제가 문제를 풀어 본 결과 이 문제의 풀이 방법은 세가지입니다.

1. 패스워드 값이 한글자 한글자 뜨면 값을 외워서 풀기
2. Javascript 수정해서 풀기
3. 개발자 도구 Network 사용해 풀기



------

## 1. 한글자 한글자 외워서 풀기

이건 말했다 싶이 옆에 메모장을 켜놓고 패스워드 값이 뜰 때마다 외워서 푸는 방법입니다.

이 방법은 굉장히 쉬운 동시에 귀찮다는 점이 있죠.



## 2. Javascript 수정해서 풀기

소스를 봅시다.

![2](https://user-images.githubusercontent.com/40850499/51298345-937e6a80-1a67-11e9-9d9a-b18f3414d725.PNG)


저 많은 소스들 중에 가장 중요시하게 볼 부분은 다음과 같습니다.

```javascript
     function answer(i)
{
x.open('GET','?m='+i,false);
x.send(null);
aview.innerHTML=x.responseText;
i++;
if(x.responseText) setTimeout("answer("+i+")",100);
if(x.responseText=="") aview.innerHTML="?";
}

setTimeout("answer(0)",10000);
```

setTimeout 함수 : 일정 시간 후에 특정 코드나 함수를 의도적으로 지연한 뒤 실행하고 싶을 때 사용하는 함수이다.

```javascript
setTimeout("answer(0)",10000);  // 10000ms , 즉 10초 뒤부터 answer 시작
```

소스를 더 자세히 살펴보면 다음과 같다.

```javascript
     function answer(i)
{
x.open('GET','?m='+i,false);   //get방식으로 m에 i를 넣어 오픈 
x.send(null);			 // get 방식으로 오픈 한 것을 널 값에 보냄
aview.innerHTML=x.responseText; //html로 x.responseText값을 보여줌
i++;					
if(x.responseText) setTimeout("answer("+i+")",100);  
if(x.responseText=="") aview.innerHTML="?"; //만약 x.responseText값이 없으면 "?" 보여줌
}					

setTimeout("answer(0)",10000);

```

x.responseText 값이 하나씩 대입되어서 aview.innerHTML로 보여준다.

그럼 하나씩 대입 되는 것을 하나씩 추가하는 것으로 바꾸면 패스워드 값을 다 얻을 수 있을 것이다.

aview.innerHTML=x.responseText;   -------> aview.innerHTML+=x.responseText;

구문을 이렇게 수정을 해주고,

if(x.responseText=="") aview.innerHTML="?"; 구문은 x.responseText값이 없으면 ? 값을 보여주니까 이 구문을 삭제 시키고 콘솔 창으로 돌리게 되면
![3](https://user-images.githubusercontent.com/40850499/51298360-a1cc8680-1a67-11e9-9415-d24379a245dc.PNG)

패스워드 값이 뜨게 된다.

이렇게 javascript를 수정시켜서 문제를 해결하는 방법도 있다.



## 3. 개발자 도구 Network 사용해서 풀기

패스워드가 뜨는 순간 개발자 도구로 들어서 network를 확인해보면,
![4](https://user-images.githubusercontent.com/40850499/51298382-ac871b80-1a67-11e9-8044-62de26c5e8ee.PNG)


이렇게 ?m=0 , ?m=1 등등 생기게 된다. 

m=0일때 값을 프리뷰로 볼 수 있으니 m 값을 하나 하나 살펴 보면 패스워드 값을 얻을 수 있다.



