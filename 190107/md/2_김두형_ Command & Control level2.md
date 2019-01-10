## Root-ME [Command & Control level2]

------

2019.01.07@Plit00
Root-me에서 포렌식에 분류되는 문제입니다.

```
with your help the computer have been identified. You have requested a memory dump and before starting your analysis you want to take a look at the antivirus logs. Unfortunately you forget to write down the workstation hostname. But it’s not a problem to get it back since you have a memory dump. The validation flag is the workstation hostname. The uncompressed memory dump md5 hash is e3a902d4d44e0f7bd9cb29865e0a15de.
```

###   설명

- Byte

- imaginfo
- workstation-hostname
- hivelist
  - Python 기반으로 만든 Memory Forensic Tool
  - Windows, Linux, Mac OS 실행 가능
  - 오픈소스 형태이며, 플러그인으로 여러 기능을 사용
  - 실행 가능 파일
    - 메모리 덤프 파일(.img , .raw , .dmp)
    - 하이버네이션 파일(.hider)
    - 가상 머신 메모리(.vmem)



#### 파일을 뜯어보자.

<img width="500" alt="memory root me command control level 2 digital forensics 2019-01-07 00-12-01" src="https://user-images.githubusercontent.com/13433722/50755514-98d7fa00-129c-11e9-82be-11b2f2825f70.png">

여기서 알 수 있는것은 imaginfo 플러그인을 이용하여 운영체제 정보 확인결과
Win7SP0x86, Win7SP1x86임을 알 수 있었고 해당 문제에서 요구하는 것은 workstation의  hostname이므로  해당 정보가 저장되어 있는 곳인 레지스트리를 살펴볼 것이다.

```
포렌식에서 레지스트리를 분석해본 경험이 있다면 레지스트리 hive 중 system에
가장 많은 시스템 정보가 담겨 있음을 알것이다.
```

![image](https://user-images.githubusercontent.com/40850499/50952265-33ca1180-14f3-11e9-887c-e4f9c6246f97.png)

해당 주소인 /REGISTRY/MACHINE/SYSTEM에 메모리의 레지스트리 하이브 시스템 주소를 확인한다



![image](https://user-images.githubusercontent.com/40850499/50952877-16964280-14f5-11e9-81d3-cbbeb1e0f869.png)

확인한 메모리 상의 가상주소를 HIVE OFFSET을 의미하는 -o 옵션 및 레지스트리 키 값을 출력하는 -K 옵션을 통해 hostname값이 저장되어 있는 경로인ControlSet001\Control\ComputerName\ComputerName을 접속하면 

```
The validation flag is the workstation hostname.
```

이라는 말을 다시 생각해보자.

