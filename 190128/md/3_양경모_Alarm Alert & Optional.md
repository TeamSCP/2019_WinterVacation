##  Alert을 이용한 Alarm

---

### 📃 Profile

> 발표자 : 양경모
>
> 소속 : Team S.C.P
>
> 발표일 : Jan 28 2019	

###  :pencil: Problem

>![image](https://user-images.githubusercontent.com/40850499/51436961-01ca6380-1cda-11e9-9e12-d6395afef103.png)

### 🧠 Process

>우선 스토리 보드를 만들어보자
>
>![image](https://user-images.githubusercontent.com/40850499/51437058-c6309900-1cdb-11e9-9841-5c78ce11a380.png)
>
>2개의 레이블과 1개의 데이트 피커를 만들어놓는다.
>
>그리고 완성된 코드를 보면서 얘기하겠다.
>
>``` swift
>import UIKit
>
>class ViewController: UIViewController {
>let timeSelector: Selector = #selector(ViewController.updateTime)
>let interval = 1.0
>var count = 0
>var alarmTime: String?
>var alertFlag = false
>
>@IBOutlet var lblCurrentTime: UILabel!
>@IBOutlet var lblPickerTime: UILabel!
>
>override func viewDidLoad() {
>   super.viewDidLoad()
>   // Do any additional setup after loading the view, typically from a nib.
>   
>   Timer.scheduledTimer(timeInterval: interval, target: self, selector: timeSelector, userInfo: nil, repeats: true)
>}
>
>@IBAction func changeDatePicker(_ sender: UIDatePicker) {
>   let datePickerView = sender
>   
>   let formatter = DateFormatter()
>   formatter.dateFormat = "yyyy-MM-dd HH:mm:ss EEE"
>   
>   lblPickerTime.text = "선택 시간 : " + formatter.string(from: datePickerView.date)
>   
>   formatter.dateFormat = "hh:mm aaa"
>   alarmTime = formatter.string(from: datePickerView.date)
>}
>
>@objc func updateTime(){
>   let date = NSDate()
>   
>   let formatter = DateFormatter()
>   formatter.dateFormat = "yyyy-MM-dd HH:mm:ss EEE"
>   
>   lblCurrentTime.text = "현재 시간 : " + formatter.string(from :date as Date)
>   
>   formatter.dateFormat = "hh:mm aaa"
>   let currentTime = formatter.string(from: date as Date)
>   
>   if alarmTime==currentTime {
>       if !alertFlag{
>           let alarmAlert = UIAlertController(title: "알림", message: "설정된 시간입니다", preferredStyle: UIAlertController.Style.alert)
>           let confirmAction = UIAlertAction(title: "네, 확인", style: UIAlertAction.Style.default, handler: nil)
>           
>           alarmAlert.addAction(confirmAction)
>           present(alarmAlert, animated: true, completion: nil)
>           alertFlag = true
>       }
>       else {
>           alertFlag = false
>       }
>   }
>}
>}
>
>```
>
>여기서 기본적인 기능은 2주전에 발표했던 시간이 같을 경우 배경 화면을 빨간색으로 바꾸는 앱과 베이스는 똑같다.
>
>그리고 alarmTime과 currentTime을 비교해서 같을경우 alarmFlag가 true면 alertcontroller를 만들고 밑에 얼럿액션을 만들어준다. 
>
>```swift
>if alarmTime==currentTime {
>       if !alertFlag{
>           let alarmAlert = UIAlertController(title: "알림", message: "설정된 시간입니다", preferredStyle: UIAlertController.Style.alert)
>           let confirmAction = UIAlertAction(title: "네, 확인", style: UIAlertAction.Style.default, handler: nil)
>           
>           alarmAlert.addAction(confirmAction)
>           present(alarmAlert, animated: true, completion: nil)
>           alertFlag = true
>       }
>       else {
>           alertFlag = false
>       }
>   }
>```
>
>여기서 UIAlertController는 Alert 창을 띄우게하는 메소드이며, UIAlertAction은 얼럿창이 뜰 경우 그 밑에 누르는 액션 버튼을 만들어 주는 메소드이다. 
>
>변하지 않는 상수로 alarmAlert과 confirmAction 을 선언해준뒤 alarmAlert에 confirmAction을 추가해주고 present를 이용해서 얼럿을 활성화 시켜준다. 그리고 alertFlag를 true로 바꿔준다
>
>여기까지가 문제 해결 방법이였고 오늘의 내용은 너무 짧아서 옵셔널에 대해 설명하겠다.

### :smile: Optional

> 옵서녈이라는 말은 직역하면 "선택적인"이라는 뜻이다. 값이 있을 수도 있고 없을 수도 있는 것을 나타낸다.
>
> 예를 들어 문자열의 값이 있으면 "가나다"가 될 것이고, 없다면 "" 일까? 아니다. ""도 엄연히 값이 있는 문자열이다. 정확히 값이 없다는 것을 표현하려면 nil 이다. 그리고 옵셔널에 초깃값을 지정하지 않으면 기본값은 nil이다.
>
> 예를 들면
>
> ``` swift
> var name: String = "양경모"
> name = nil // 컴파일 에러
> 
> var email: String?
> print(email) // nil
> 
> email = "fover32@gmail.com"
> print(email) // Optinal("fover32@gmail.com")
> 
> ```
>
> 이런 식으로 코드가 있다면 첫 두줄에서 보면 String 형식으로 "양경모"라는 값을 넣어주고 name에 nil 값을 넣어주면 에러로 출력된다. 이유는 값이 있는 변수에 nil을 넣어주면 값이 이미 있는데 없다고 바꿔줄 수 없기 때문이다. 
>
> 그리고 4번째 라인에서 마지막 라인까지는 기본적인 예이다. 위에서 말했던 것처럼 초깃값을 지정하지 않으면 nil값이 들어가고 그 후에 따로 값을 넣어주면 출력할 경우 앞에 Optional이 붙는다.
>
> #### Optional Binding
>
> 옵셔널 바인딩이란 옵셔널의 값을 가져오고 싶을 때 사용하는 것이다. 옵셔널의 값이 존재하는지를 검사한 뒤, 존재한다면 그 값을 다른 변수에 대입해준다. 
>
> 예를 들어, 아래의 코드에서 optionalEmail에 값이 존재한면 email 이라는 변수 안에 실제 값이 저장되고, if문 내에서 그 값을 사용할 수 있다. 만약 optionalEmail 값이 nil이라면 if 문이 실행되지 않고 넘어간다.
>
> ``` swift
> if let email = optionalEmail {
>   print(email) // optionalEmail의 값이 존재한다면 해당 값이 출력됩니다.
> }
> // optionalEmail의 값이 존재하지 않는다면 if문을 그냥 지나칩니다.
> ```
>
> #### Optional Chaining
>
> 설명보다는 코드를 보는 편이 좋다. 예컨대 옵셔널로 선언된 어떤 배열을 떠올려 봅시다. 이 배열이 "빈 배열"인지를 검사하려면 어떻게 해야 할까요? nil 이 아니면서 빈 배열인지를 확인해보면 될 것입니다.
>
> ``` swift
> let array: [String]? = []
> var isEmptyArray = false
> 
> if let array = array, array.isEmpty {
>   isEmptyArray = true
> } else {
>   isEmptyArray = false
> }
> 
> isEmptyArray
> ```
>
> 옵셔널 체이닝을 사용하면 이 코드를 더 간결하게 쓸 수 있습니다.
>
> `let isEmptyArray = array?.isEmpty == true`
>
> 3가지 경우로
>
> - array가 nil 인 경우
>
>   array?.isEmpty
>
>   ———— 
>
>   여기까지 실행되고 nil을 반환한다.
>
>   
>
> - array가 빈 배열인 경우
>
>   array?.isEmpty
>
>   ————
>
>   여기까지 실행되고 true를 반환한다.
>
>   
>
> - array에 요소가 있는 경우
>
>   array?.isEmpty
>
>   ————
>
>   여기까지 실행되고 false를 반환한다.
>
> array?.isEmpty의 결과로 나올 수 있는 값은 nil, true, false 가 됩니다. isEmpty의 반환값은 Bool인데, 옵셔널 체이닝으로 인해 Bool?을 반환하도록 바뀐 것이다. 따라서 값이 실제로 true 인지를 확인하려면, == true를 해주어야 한다.
>
> #### Optional Unwrapping
>
> 옵셔널을 사용할 때마다 옵셔널 바인딩을 하는 것이 가장 바람직하다. 하지만, 개발을 하다보면 분명히 값이 존재할 것임에도 불구하고 옵셔널로 사용해야 하는 경우가 종종 있다. 이럴 때에는 옵셔널에 값이 있다고 가정하고 값에 바로 접근할 수 있도록 도와주는 키워드인 ! 를 붙여서 사용하면 된다.
>
> ``` swift 
> print(optionalEmail) // Optional("fover32@gmail.com")
> print(optionalEmail!) // fover32@gmail.com
> ```
>
> ! 을 사용할 때에는 주의할 점이 있는데, 옵셔널의 값이 nil 인 경우에는 런타임 에러가 발생하는 것입니다. Java의 NullPointerException과 비슷하다고 생각하면 될 것이다.
>
> ``` swift
> var optionalEmail: String?
> print(optionalEmail!) // 런타임 에러!
> ```
>
> 
>
> #### Implicitly Unwrapped Optional
>
> 만약, 옵셔널을 정의할 때 ? 대신 ! 를 붙이면 ImplicitlyUnwrappedOptional 이라는 옵셔널로 정의된다.
>
> 직역하면 암묵적으로 벗겨진 옵셔널이다.
>
> ``` swift
> var email: String! = "fover32@gmail.com"
> print(email) // fover32@gamil.com
> ```
>
> 여기서도 마찬가지로 nil 값을 넣을 때 ! 를 넣으면 런타임 에러가 난다.

