## Picker View를 이용한 Gallery

---

### Profile

> 발표자 : 양경모
>
> 소속 :   Team S.C.P
>
> 발표일 : Jan 21 2019

### Problem

> <img width="600" alt="image" src="https://user-images.githubusercontent.com/46397818/51454889-6513ce80-1d8a-11e9-890f-1a48450f8721.png">
>
> 문제는 2가지인데 여기서 밑에꺼만 해보기로 합니다
>
> 우선 2열로 나오기 위해서는 힌트에서 처럼 PICKER_VIEW_COLUMN이라는 변수의 값을 2로 선언해주면 pickerView 메소드에서 값을 받아서 피커 뷰를 2열로 출력하게 한다.
>
> 그리고 두번째 힌트처럼 component를 이용해서 구분할 수 있다. 왼쪽이 0 오른쪽이 1 여러개로 된다면 0~n 순으로 지정해주면 된다.

### Process

>우선 스토리보드를 만들어 보자
>
>![image](https://user-images.githubusercontent.com/46397818/51455179-af497f80-1d8b-11e9-85ef-143a81d3e088.png)
>
>이런식으로 피커뷰, 레이블 그리고 이미지 뷰를 만들어준다.
>
>그리고 결과 코드를 보면
>
>``` swift
>import UIKit
>
>class ViewController: UIViewController, UIPickerViewDelegate, UIPickerViewDataSource {
>    let MAX_ARRAY_NUM = 10
>    let PICKER_VIEW_COLUMN = 2
>    let PICKER_VIEW_HEIGHT:CGFloat = 80
>    var imageArray = [UIImage?]()
>    var imageFileName = [ "1.jpg", "2.jpg", "3.jpg", "4.jpg", "5.jpg", "6.jpg", "7.jpg", "8.jpg", "9.jpg", "10.jpg"]
>    
>    @IBOutlet weak var pickerImage: UIPickerView!
>    @IBOutlet weak var lblImageFileName: UILabel!
>    @IBOutlet weak var imageView: UIImageView!
>    
>    override func viewDidLoad() {
>        super.viewDidLoad()
>        // Do any additional setup after loading the view, typically from a nib.
>        
>        for i in 0 ..< MAX_ARRAY_NUM {
>            let image = UIImage(named : imageFileName[i])
>            imageArray.append(image)
>        }
>        
>        lblImageFileName.text = imageFileName[0]
>        imageView.image = imageArray[0]
>    }
>    
>    // 피커 뷰의 컴포넌트 수 설정 (델리케이트 메소드) 피커 뷰에 표시되는 열의 개수 의미
>    func numberOfComponents(in pickerView: UIPickerView) -> Int {
>        return PICKER_VIEW_COLUMN
>    }
>    
>    // 피커 뷰 사진 높이 지정
>    func pickerView(_ pickerView: UIPickerView, rowHeightForComponent component: Int) -> CGFloat {
>        return PICKER_VIEW_HEIGHT
>    }
>    
>    // 위에있는 메소드 인수를 가지는 델리게이트 메소드. 피커 뷰에게 컴포넌트의 열의 개수를 정수 값으로 넘겨줌.
>    func pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
>        return imageFileName.count
>    }
>    
>    // 피커 뷰에서 컴포넌트의 각 열의 타이틀을 문자열 값으로 넘겨줌
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
>        // lblImageFileName.text = imageFileName[row]
>        // imageView.image = imageArray[row]
>        
>        if(component==0) {
>            lblImageFileName.text = imageFileName[row]
>        }
>        else {
>            imageView.image = imageArray[row]
>        }
>    }
>}
>```
>
>이런식으로 이루어진다. 우선 변수들을 살펴보면
>
>````swift
>	let MAX_ARRAY_NUM = 10
>    let PICKER_VIEW_COLUMN = 2
>    let PICKER_VIEW_HEIGHT:CGFloat = 80
>    var imageArray = [UIImage?]()
>    var imageFileName = [ "1.jpg", "2.jpg", "3.jpg", "4.jpg", "5.jpg", "6.jpg", "7.jpg", "8.jpg", "9.jpg", "10.jpg"]
>    
>    @IBOutlet weak var pickerImage: UIPickerView!
>    @IBOutlet weak var lblImageFileName: UILabel!
>    @IBOutlet weak var imageView: UIImageView!
>````
>
>변수들은 8가지 있는데 밑에 3개는 아웃렛 변수이고 나머지 5가지는 피커뷰 길이 조정 변수, 배열 최대 수 변수이다. 나머지는 이미지 출력을 위하 변수다.
>
>```swift
>// 피커 뷰의 컴포넌트 수 설정 (델리케이트 메소드) 피커 뷰에 표시되는 열의 개수 의미
>    func numberOfComponents(in pickerView: UIPickerView) -> Int {
>        return PICKER_VIEW_COLUMN
>    }
>    
>    // 피커 뷰 사진 높이 지정
>    func pickerView(_ pickerView: UIPickerView, rowHeightForComponent component: Int) -> CGFloat {
>        return PICKER_VIEW_HEIGHT
>    }
>    
>    // 위에있는 메소드 인수를 가지는 델리게이트 메소드. 피커 뷰에게 컴포넌트의 열의 개수를 정수 값으로 넘겨줌.
>    func pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
>        return imageFileName.count
>    }
>    
>	// 이미지 크기 지정
>    func pickerView(_ pickerView: UIPickerView, viewForRow row: Int, forComponent component: Int, reusing view: UIView?) -> UIView {
>        let imageView = UIImageView(image:imageArray[row])
>        imageView.frame = CGRect(x: 0, y: 0, width: 100, height: 150)
>        
>        return imageView
>    }
>    
>	//
>    func pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int) {
>        
>        if(component==0) {
>            lblImageFileName.text = imageFileName[row]
>        }
>        else {
>            imageView.image = imageArray[row]
>        }
>    }
>}
>```
>
>함수부분이다. 주석을 읽어보면 이해할 수 있을 것이다.

