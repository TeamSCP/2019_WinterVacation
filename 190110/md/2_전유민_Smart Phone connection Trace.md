## 스마트폰 연결 흔적 (Smart Phone connection Trace)

**준비물**

- FTK Imager : 포렌식에서 파일을 복구할 때 가장 많이 쓰이는 툴
- Registry Explorer : 여러가지 레지스트리를 볼 수 있거나 분석할 수 있는 툴 (레지스트리 탐색기) 
- USB 케이블
- 스마트폰 (삼성 갤럭시 & 애플 아이폰)

1. **FTK Imager 실행 -> SYSTEM 파일 찾기 -> 추출 **

FTK Imager 툴을 실행시키고, Logical Drive에서 C를 선택한 다음 SYSTEM 파일을 찾기 위해 다음 경로와 같이 이동하면 SYSTEM 파일이 나온다. 

![image](https://user-images.githubusercontent.com/40850499/51129196-2d2df800-186d-11e9-989a-2b45c6ab5752.png) 

여기서 SYSTEM 파일은 운영체제에 중요한 컴퓨터 파일, 하드웨어를 직접 작동시키는 장치 제어기 역할을 한다.

![image](https://user-images.githubusercontent.com/40850499/51129259-58184c00-186d-11e9-9eda-889f69ed7f15.png) 

### 경로 : C: - Windows – System32 – config - SYSTEM

위 경로대로 이동하면 다음 화면과 같이 SYSTEM 파일이 나오고 오른쪽 마우스를 누르면 Export Files...가 나오고 누르면 추출이 된다. (저장은 바탕화면이든 아무 곳이나 해도 상관없다) 

2. **Registry Explorer 실행 -> USB 연결 흔적 확인**

Registry Explorer를 실행하고 파일 찾기를 누르고 방금 찾은 SYSTEM 파일을 선택하면 여러가지 폴더들이 뜨는데 USB 흔적을 확인하기 위해 다음 경로로 이동한다.

### **경로 : ControlSet001 – Enum - USB** 

![image](https://user-images.githubusercontent.com/40850499/51129285-67979500-186d-11e9-82dd-c8ebe8c2b7b1.png) 

화면과 같이 VID_OOOO & PID_OOOO 로 쓰여있는 많은 파일들이 나온다. (VID & PID - 내가 지금까지 연결시켰던 USB)

3. **USB VID & PID 설명**

- VID (Vender ID) : 제조사 ID 
- PID (Product ID) : 그 제조사의 제품 고유번호 
- VID & PID : 다른 USB와 겹치지 않는 유일한 식별번호 

4. **연결 되어있는 (연결 되었던) VID & PID 확인**

![image](https://user-images.githubusercontent.com/40850499/51129296-71b99380-186d-11e9-87b2-358c68c88e59.png) 

연결 되어 있는 USB 디바이스의 VID와 PID를 확인하기 위해 검색에서 장치관리자를 검색하면 여러가지 장치들이 나오고 밑에 휴대용 장치를 보면 현재 연결이 되어 있거나 연결 되었던 스마트폰이 나오고 자세히를 누르면 연결되었던 USB 디바이스의 VID와 PID 번호가 나온다. 

![image](https://user-images.githubusercontent.com/40850499/51129320-81d17300-186d-11e9-81c6-1d7b23bf5905.png) 

5. **연결 흔적 확인**

![image](https://user-images.githubusercontent.com/40850499/51129331-8a29ae00-186d-11e9-8ae0-46d96eecbf55.png) 

실행 시킨 Registry Explorer를 보면 방금 확인한 VID와 PID번호가 나오고 마지막으로 연결되었던 시간, 여러가지 디바이스의 정보가 나온다. 포렌식에서 원래는 USB 연결 흔적을 찾기 위해 많이 쓰이지만, USB 케이블을 이용하여  스마트폰을 연결하는 경우도 많기 때문에 Registry Explorer 나 viewer와 같은 이런 종류의 툴을 이용하여 연결했던 스마트폰의 연결 흔적을 볼 수도 있다.

6. **SAMSUNG & APPLE**

![image](https://user-images.githubusercontent.com/40850499/51129344-91e95280-186d-11e9-9073-65a0b489c8e4.png) 

위에는 삼성 갤럭시 스마트폰을 연결 시켰지만, 혹시 아이폰은 뭐가 다를까해서 아이폰도 연결 시켜 삼성과 아이폰의 연결 흔적을 비교해보았다. 삼성은 기기 모델명과 어떤 기기인지 나오는 반면, 아이폰은 Apple IPhone이라고만 나온다. (이 것으로 아이폰은 쓰.레.기라는 것을 알 수 있다..)

![image](https://user-images.githubusercontent.com/40850499/51129359-9a418d80-186d-11e9-8fa1-c16d48a0bb0a.png) 

USB의 추가 정보를 보려면 Device Hunt라는 사이트에 들어가면 볼 수 있다. 주로 VID와 PID 같은 장치의 ID를 사용하여 내가 찾고 싶은 장치를 찾을 수 있다. (주로 USB나 PCI를 찾을 때 사용)

끝이다.



