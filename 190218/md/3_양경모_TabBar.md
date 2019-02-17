## Tab Bar Controller & Navigation Controller

---

### :kr: Profile

>발표자 : 양경모
>
>소속 :  Team S.C.P
>
>발표일 : 18 Feb 2019
>
>---

### :pushpin: First Topic 

>Tab Bar Controller
>
>탭 바 컨트롤러는 다음 사진부터 보면서 설명하겠다.
>
>​![](https://adminstvo.files.wordpress.com/2011/04/tabbar.png)
>
>탭 바 컨트롤러는 보이는 거와 같이 하단에 있는 탭을 이용해 뷰 컨트롤러를 이동하는 방식의 뷰 컨트롤러다. 탭 바 컨트롤러를 이용하기 위해서는 메뉴 바에서 Editor -> Embed In -> Tab Bar Controller 을 선택하면 된다.
>
>두 번째 연결부터는 탭 바 컨트롤러에서 ⌃ + 드래그로 View Controller를 연결한다.
>
>---

###:question: First Problem

> ![image](https://user-images.githubusercontent.com/46397818/52481851-12903a00-2bf3-11e9-929c-ef62f99b9be7.png)
>
> 문제는 10장에서 공부했던 것에서 새로운 탭을 추가하는 것이다.
>
> 생각보다 어렵지 않은 문제이다.
>
> ---

### 🧠 Solution

>우선 스토리보드에서 피커 뷰 예제의 스토리보드를 복제하여 추가한다.
>
>그 후 Ctrl + Drag 를 하여 연결해준다!
>
>![image](https://user-images.githubusercontent.com/46397818/52482204-022c8f00-2bf4-11e9-9146-a981c53d092f.png)
>
>그리고 피커 뷰 예제의 ViewController.swift 파일을 복사하여 이름을 PickerViewController.swift로 변경하고 현재 프로젝트 폴더에 집어 넣어준다.
>
>![image](https://user-images.githubusercontent.com/46397818/52482301-3ef88600-2bf4-11e9-8b6b-e6fc64091f82.png)
>
>이렇게 간단하다... 그리고 현재 프로젝트의 ViewConrtroller.swift 에서 처음 보여지는 뷰에서 추가한 버튼의 액션함수를 추가한 후 tabBarController?.selectedIndex = 3을 지정해준다. 여기서 3이 새로 생성한 PickerViewController의 스토리보드이다.
>
>``` swift
>import UIKit
>
>class ViewController: UIViewController {
>
>    override func viewDidLoad() {
>        super.viewDidLoad()
>        // Do any additional setup after loading the view, typically from a nib.
>    }
>
>    @IBAction func btnMoveImageView(_ sender: UIButton) {
>        tabBarController?.selectedIndex = 1
>    }
>    
>    @IBAction func btnMoveDatePicker(_ sender: UIButton) {
>        tabBarController?.selectedIndex = 2
>    }
>    
>    @IBAction func btnMovePickerView(_ sender: UIButton) {
>        tabBarController?.selectedIndex = 3
>    }
>}
>
>```
>
>이렇게 해주면 끝난다!
>
>---





​