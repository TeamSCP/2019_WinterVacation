# algo_auth

2019/01/28

Team S.C.P

정재훈

codegate 2019 문제 algo_auth 의 writeup이다.

> I like an algorithm
>
> nc 110.10.147.104 15712
>
> nc 110.10.147.109 15712

우선 접속해보면

![image](https://user-images.githubusercontent.com/40850499/51808097-d398e500-22d2-11e9-9aa7-72bfb4dace53.png)

위와 같다. 최소 비용 경로 알고리즘을 이용해 100스테이지를 깨야한다. 다익스트라 알고리즘을 이용하여 푸는게 일반적이지만, 알고리즘을 이용하는게 귀찮은 나머지....

Brute Forcing을 먼저 사용해 보았다.

![image](https://user-images.githubusercontent.com/40850499/51808154-8e28e780-22d3-11e9-8291-598db992ddd1.png)

0부터 +1하여 추측해 가면서 맞으면 arr 배열에 추가해 나가는 식으로 짰다.

**너.무. 오래걸린다.**

조금 더 생각을 해보면, 최단 거리의 비용이 최소 비용보다 크거나 같다는 것을 알 수 있다. 따라서, 각 줄의 총 합(최단 거리의 비용들) 중 가장 작은 비용이 드는게 최소 비용과 가깝다는 것을 알 수 있다. 그러므로 각 줄의 총 합 중 최솟값에서 하나 씩 줄여나가면서 찾는 것이 효율이 좋다고 생각했다.

그렇게 코드를 다시 짰다...

![image](https://user-images.githubusercontent.com/40850499/51808232-9afa0b00-22d4-11e9-97e9-a875e3eec77c.png)

이렇게 하니 9분만에 100스테이지를 깰 수 있었다. 이제 각 스테이지의 답은 알았으니 다 넣으면 뭐가 나오는지 알아보자.

![image](https://user-images.githubusercontent.com/40850499/51808258-f7f5c100-22d4-11e9-9169-3e5bd3e02d1e.png)

![image](https://user-images.githubusercontent.com/40850499/51808262-0643dd00-22d5-11e9-81fd-f0027f77cf2a.png)

번역 : 축하한다!  니 답이 답이다

????

누군가에게 각 스테이지의 답을 잘 기억해 두라는 힌트를 받았다.

자세히 보니 human-readable한 값이 octal 또는 decimal로  되어 있는 것 같은 느낌이라 decimal to ascii 를 바로 구글에 검색해보았다.

![image](https://user-images.githubusercontent.com/40850499/51808290-6175cf80-22d5-11e9-8558-c739d1e3e97a.png)

넣어 보니 ASCII text에 있는 값이 base64같아 보인다. 패딩은 없어서 확신은 없지만 뭔가 그래보이는 것이 있지 않은가!

![image](https://user-images.githubusercontent.com/40850499/51808311-997d1280-22d5-11e9-9036-4831451b82a7.png)

☆성★공☆