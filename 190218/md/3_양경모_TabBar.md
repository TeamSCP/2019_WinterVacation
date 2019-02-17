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

### :pushpin: Topic 

>Tab Bar Controller
>
>탭 바 컨트롤러는 다음 사진부터 보면서 설명하겠다.
>
>![](https://adminstvo.files.wordpress.com/2011/04/tabbar.png)
>
>탭 바 컨트롤러는 보이는 거와 같이 하단에 있는 탭을 이용해 뷰 컨트롤러를 이동하는 방식의 뷰 컨트롤러다. 탭 바 컨트롤러를 이용하기 위해서는 메뉴 바에서 Editor -> Embed In -> Tab Bar Controller 을 선택하면 된다.
>
>두 번째 연결부터는 탭 바 컨트롤러에서 ⌃ + 드래그로 View Controller를 연결한다.
>
>---

### :question: Problem

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
>```swift
>import UIKit
>
>class PickerViewController: UIViewController, UIPickerViewDelegate, UIPickerViewDataSource {
>    let MAX_ARRAY_NUM = 10
>    let PICKER_VIEW_COLUMN = 1
>    let PICKER_VIEW_HEIGHT:CGFloat = 80
>    var imageArray = [UIImage?]()
>    var imageFileName = [ "1.jpg", "2.jpg", "3.jpg", "4.jpg", "5.jpg",
>                          "6.jpg", "7.jpg", "8.jpg", "9.jpg", "10.jpg" ]
>
>    @IBOutlet var pickerImage: UIPickerView!
>    @IBOutlet var lblImageFileName: UILabel!
>    @IBOutlet var imageView: UIImageView!
>    
>    override func viewDidLoad() {
>        super.viewDidLoad()
>        // Do any additional setup after loading the view, typically from a nib.
>        
>        for i in 0 ..< MAX_ARRAY_NUM {
>            let image = UIImage(named: imageFileName[i])
>            imageArray.append(image)
>        }
>        
>        lblImageFileName.text = imageFileName[0]
>        imageView.image = imageArray[0]
>    }
>
>    override func didReceiveMemoryWarning() {
>        super.didReceiveMemoryWarning()
>        // Dispose of any resources that can be recreated.
>    }
>
>    // returns the number of 'columns' to display.
>    func numberOfComponents(in pickerView: UIPickerView) -> Int {
>        return PICKER_VIEW_COLUMN
>    }
>    
>    // returns height of row for each component.
>    func pickerView(_ pickerView: UIPickerView, rowHeightForComponent component: Int) -> CGFloat {
>        return PICKER_VIEW_HEIGHT
>    }
>    
>    // returns the # of rows in each component..
>    func pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
>        return imageFileName.count
>    }
>    
>//    func pickerView(_ pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int) -> String? {
>//        return imageFileName[row]
>//    }
>    
>    func pickerView(_ pickerView: UIPickerView, viewForRow row: Int, forComponent component: Int, reusing view: UIView?) -> UIView {
>        let imageView = UIImageView(image:imageArray[row])
>        imageView.frame = CGRect(x: 0, y: 0, width: 100, height: 150)
>        
>        return imageView
>    }
>    
>    func pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int) {
>        lblImageFileName.text = imageFileName[row]
>        imageView.image = imageArray[row]
>    }
>}
>```
>
>이미지는 예제에서 받아서 가져오며 5장에서 배웠던 피커뷰 즉 내가 원하는 것을 골라서 출력하는 코드를 작성해준다.
>
>그리고 클래스 이름은 바꿔주어야한다. 그 후 스토리보드에서 데이트 피커에 해당하는 클래스를 연결해준다.
>
>``` swift
>import UIKit
>
>class ViewController: UIViewController {
>
>override func viewDidLoad() {
>   super.viewDidLoad()
>   // Do any additional setup after loading the view, typically from a nib.
>}
>
>@IBAction func btnMoveImageView(_ sender: UIButton) {
>   tabBarController?.selectedIndex = 1
>}
>
>@IBAction func btnMoveDatePicker(_ sender: UIButton) {
>   tabBarController?.selectedIndex = 2
>}
>
>@IBAction func btnMovePickerView(_ sender: UIButton) {
>   tabBarController?.selectedIndex = 3
>}
>}
>
>```
>
>이렇게 해주면 끝난다!
>
>---