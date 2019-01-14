#  Root me Javascript - Obfuscation 1, 2, 3

 **Wargame site**  : [Root me](https://www.root-me.org/?page=news&lang=en)



## 1.  Javascript - Obfuscation 1

문제를 풀어 봅시다. 시이이작!

![1-1](https://user-images.githubusercontent.com/40850499/51096786-8f054800-1802-11e9-944d-3ba9281b347a.PNG)

문제를 들어가는 순간 위에 있는 그림과 같이 패스워드를 쓸 수 있는 창이 뜬다.

일단 사이트에는 알 수 있는게 하나도 없으니 바로 이 사이트의 소스를 보러 가야겠죠?

![1-2](https://user-images.githubusercontent.com/40850499/51096793-9c223700-1802-11e9-855c-36a9d35b5153.PNG)


해당 페이지 소스를 자세히 보면 이 두 부분이 중요하다는 것을 알 수 있다.

```http
pass = '%63%70%61%73%62%69%65%6e%64%75%72%70%61%73%73%77%6f%72%64';
```

```c
if(h == unescape(pass)) {
                  alert('Password accepté, vous pouvez valider le challenge avec ce mot de passe.\nYou an validate the challenge using this pass.');
              } else {
                  alert('Mauvais mot de passe / wrong password');
              }

```

먼저 간단하게 소스 해석을 해보자면 pass의 값은 url encoding 이 되어있다는 것을 알 수 있고,

만약 unescape(pass)가 h 와 같다면 alert 창으로 맞다고 알려주고 다르면 아니라고 알려주는 소스이다.

**이 문제를 해결 하려면 pass 변수에 url encoding 된 문자열을 넣어주고 unescape 함수로 decode을 해주면 된다**

pass의 값이 인코딩이 되어있으니 url decode을 해보면,

![1-3](https://user-images.githubusercontent.com/40850499/51096797-a80df900-1802-11e9-9711-265eac35fcdf.PNG)


이렇게 패스워드 값이 나온다. 

![1-5](https://user-images.githubusercontent.com/40850499/51096837-e86d7700-1802-11e9-8b08-efb5e6e9476d.PNG)

너무나 간단하게 성공 ^_____________________________________________^





------



## 2.  Javascript - Obfuscation 2

Obfuscation 2 문제를 들어가게 되면 그냥 흰 화면만 나온다.

그래서 바로 해당 페이지 소스를 보면,

![2-1](https://user-images.githubusercontent.com/40850499/51096838-f4593900-1802-11e9-90cf-3383d5405567.PNG)

```c#
var pass = unescape("unescape%28%22String.fromCharCode%2528104%252C68%252C117%252C102%252C106%252C100%252C107%252C105%252C49%252C53%252C54%2529%22%29");
```

Obfuscation 1번과 비슷하게 url encoding 이 되어있는 것을 볼 수 있다. 

**이 문제를 해결 하려면  콘솔 창을 이용해서 decode해주면 된다.**

콘솔 창을 이용해서 디코드를 해주게 되면 다음과 같이 나온다.

![2-2](https://user-images.githubusercontent.com/40850499/51096841-fd4a0a80-1802-11e9-9657-257448438a07.PNG)
```
"unescape("String.fromCharCode%28104%2C68%2C117%2C102%2C106%2C100%2C107%2C105%2C49%2C53%2C54%29")"
```

디코드를 해줘도 인코딩 된 값이 나오니 한번 더 해줘야한다.

한번 더 해주게 되면,

![2-3](https://user-images.githubusercontent.com/40850499/51096846-04711880-1803-11e9-8202-d4ec576a1342.PNG)


왠지 답일 것 같은 게 나온다. 처음엔 이게 답이겠거니 하고 신나서 바로 확인을 해봤는데 아니라고 떴다.

혹시 몰라서 한번 더 콘솔 창을 이용해 확인을 해봤더니,

![2-4](https://user-images.githubusercontent.com/40850499/51096850-0c30bd00-1803-11e9-9112-3972d101c389.PNG)

짜란 !!! 이번엔 진짜 답이 나왔다.

![2-5](https://user-images.githubusercontent.com/40850499/51096903-5e71de00-1803-11e9-86a1-74d4e29584ea.PNG)


이렇게 Obfuscation 2 해결!



------



## 3.  Javascript - Obfuscation 3

문제를 보면
![3-1](https://user-images.githubusercontent.com/40850499/51096862-1eaaf680-1803-11e9-9b85-5fd82489369e.PNG)

바로 패스워드 값을 제출하는 창이 뜬다.

그럼 해당 페이지의 소스를 보자요..

![3-2](https://user-images.githubusercontent.com/40850499/51096865-24a0d780-1803-11e9-8b70-06bd58a31f32.PNG)


```c
 var pass = "70,65,85,88,32,80,65,83,83,87,79,82,68,32,72,65,72,65";
        var tab  = pass_enc.split(',');
                var tab2 = pass.split(',');var i,j,k,l=0,m,n,o,p = "";i = 0;j = tab.length;
                        k = j + (l) + (n=0);
                        n = tab2.length;
                        for(i = (o=0); i < (k = j = n); i++ ){o = tab[i-l];p += String.fromCharCode((o = tab2[i]));
                                if(i == 5)break;}
                        for(i = (o=0); i < (k = j = n); i++ ){
                        o = tab[i-l]; 
                                if(i > 5 && i < k-1)
                                        p += String.fromCharCode((o = tab2[i]));
                        }
        p += String.fromCharCode(tab2[17]);
        pass = p;return pass;
    }
String["fromCharCode"](dechiffre("\x35\x35\x2c\x35\x36\x2c\x35\x34\x2c\x37\x39\x2c\x31\x31\x35\x2c\x36\x39\x2c\x31\x31\x34\x2c\x31\x31\x36\x2c\x31\x30\x37\x2c\x34\x39\x2c\x35\x30"));
 h = window.prompt('Entrez le mot de passe / Enter password');
    alert( dechiffre(h) );
    
```

이렇게 소스를 보면 var pass 는 70,65,85,88,32,80,65,83,83,87,79,82,68,32,72,65,72,65 라고 나와있다. 이걸 보고 **아스키 코드**가 생각이 났다.

저렇게 긴 소스는 다 볼 필요 없고, 이문제에서 중요한 부분은 이 부분이다.

```c
String["fromCharCode"](dechiffre("\x35\x35\x2c\x35\x36\x2c\x35\x34\x2c\x37\x39\x2c\x31\x31\x35\x2c\x36\x39\x2c\x31\x31\x34\x2c\x31\x31\x36\x2c\x31\x30\x37\x2c\x34\x39\x2c\x35\x30"));
    
```

저 소스를 보고 바로 16진수가 떠올랐다. 그래서 16진수 변환기로 이것 저것 다 해봤는데 실패,,,,

계속 이상한 삽질만 하다가 위에서 본 아스키코드가 생각이나서 바로 아스키코드 변환기를 사용해 변환을 해보았더니 다음과 같은 숫자들이 나왔다.

```c
55,56,54,79,115,69,114,116,107,49,50
```

위 소스를 다시 보면,  alert( dechiffre(h) ); 가 있다.

그럼 h =55,56,54,79,115,69,114,116,107,49,50 이니까 

String.fromCharCode(55,56,54,79,115,69,114,116,107,49,50); 이 된다.

콘솔 창을 이용해 확인을 해보면

![3-3](https://user-images.githubusercontent.com/40850499/51096868-2d91a900-1803-11e9-85c6-6949c95cb8c9.PNG)

짜란 답이 나왔다.!
![3-4](https://user-images.githubusercontent.com/40850499/51096875-35514d80-1803-11e9-8671-0a869fa7cc7d.PNG)



삽질 많이 했던 Obfuscation 3 해결!
