## 데이트 피커를 이용한 알람

---

### Profile

> 작성자 : 양경모
>
> 소속 : Team S.C.P
>
> 발표일 : Mon 14 Jan 2019

---

### Date Picker?

> 데이트 피커라는 것은 우리가 IOS를 사용하면서 볼수있는 날짜를 고르는 라이브러리다
>
> 그림으로 보면 바로 알수 있을 것이다.
>
> ![date_picker](https://user-images.githubusercontent.com/40850499/50961796-076ebf00-150c-11e9-80b2-f8d34ae0b5be.png)
>
> 이렇게 생긴 라이브러리다

### Problem

>![image](https://user-images.githubusercontent.com/40850499/50962040-b3b0a580-150c-11e9-8e38-3cef0b4dc44b.png)
>
>우리에게 주어진 문제이다.

### Process

>우선 전체적인 코드를 보여드리겠습니다.
>
>``` swift
>import UIKit
>
>class ViewController: UIViewController {
>    let timeSelector : Selector = #selector(ViewController.updateTime)
>    let interval = 1.0
>    var count = 0
>    var alarmTime : String?
>    
>    @IBOutlet weak var lblCurrentTime: UILabel!
>    @IBOutlet weak var lblPickerTime: UILabel!
>    
>    override func viewDidLoad() {
>        super.viewDidLoad()
>        // Do any additional setup after loading the view, typically from a nib.
>        
>        Timer.scheduledTimer(timeInterval: interval, target: self, selector: timeSelector, userInfo: nil, repeats: true)
>        // 타이머 설정
>        
>    }
>    
>    @IBAction func changeDatePicker(_ sender: UIDatePicker) {
>        let datePickerView = sender
>        let formatter = DateFormatter()
>       
>        formatter.dateFormat = "yyyy-MM-dd HH:mm EEE"
>        lblPickerTime.text =
>        "선택 시간 : " + formatter.string(from: datePickerView.date)
>        
>        formatter.dateFormat = "hh:mm aaa"
>        alarmTime = formatter.string(from: datePickerView.date)
>    
>    }
>    
>    @objc func updateTime(){
>        let date = NSDate()
>        // 현재 시간을 가져옴
>        
>        let formatter = DateFormatter()
>        // formatter 객체 선언
>        formatter.dateFormat = "yyyy-MM-dd HH:mm:ss EEE"
>        lblCurrentTime.text =
>        "현재 시간 : " + formatter.string(from: date as Date)
>        
>        formatter.dateFormat = "hh:mm aaa"
>        let currentTime = formatter.string(from: date as Date)
>        
>        if (alarmTime == currentTime) {
>            view.backgroundColor = UIColor.red
>        }
>        else {
>            view.backgroundColor = UIColor.white
>        }
>    }
>
>}
>```
>
>그리고 다음은 스토리보드를 보여드리겠습니다.
>
>![image](https://user-images.githubusercontent.com/40850499/50963781-fffde480-1510-11e9-9266-d9e0e8f3bd0c.png)
>
>우선 현재 시간과 선택 시간에서 각각 시간의 데이트 포맷을 따로 빼주고 그것을 비교해서 변경 해주면 된다.
>
>우선 기본적인 적인 아웃렛 변수와 액션 함수를 추가해준다.
>
>그리고 상수와 변수들을 선언해준다. 여기서
>
>`` let timeSelector : Selector = #selector(ViewController.updateTime) ``  
>
>타이머가 구동되면 실행할 함수를 지정해준다. 이 함수를 지정해주면 에러가 뜰텐데 이 에러는 updateTime을 선언하지 않았기 때문이기에 뒤에서 차차 해결될 에러이다.
>
>그리고 viewDidLoad 함수에 
>
>``` swift
>override func viewDidLoad() {
>        super.viewDidLoad()
>        // Do any additional setup after loading the view, typically from a nib.
>        
>        Timer.scheduledTimer(timeInterval: interval, target: self, selector: timeSelector, userInfo: nil, repeats: true)
>        // 타이머 설정
>```
>
>를 추가해준다. 여기서 Timer.scheduledTimer는 타이머를 설정해주는 것이다. interval을 시간 간격, 타켓은 자기 자신, 셀렉터는 선언해준 timeSelector, userInfo는 nil값을 넣어준다.
>
>그 후 선언한 액션 함수 changeDatePicker를 
>
>``` swift
>@IBAction func changeDatePicker(_ sender: UIDatePicker) {
>        let datePickerView = sender
>        let formatter = DateFormatter()
>       
>        formatter.dateFormat = "yyyy-MM-dd HH:mm EEE"
>        lblPickerTime.text =
>        "선택 시간 : " + formatter.string(from: datePickerView.date)
>        
>        formatter.dateFormat = "hh:mm aaa"
>        alarmTime = formatter.string(from: datePickerView.date)
>    
>    }
>```
>
>상수 형태의 변수 datePickerView에 액션함수 매개변수인 sender에서 받아온 값을 넣는다 sender의 자료형은 UIDatePicker이다
>
>그리고 formatter 상수를 선언해주고 DateFormatter( ) 을 넣어준다
>
>그리고 formatter의 데이터 포맷을 정해주고 lblPickerTime에 datePickerView.date로부터 받아온 날짜를 더해서 텍스트를 나오게 해준다. 그 후 시간을 비교하기 위해 다시 또 시간만 형식으로 빼서 alarmTime에 저장해준다.
>
>그 후 updateTime이라는 함수를 선언해준다. 이 함수를 선언해줘야 처음에 선언했던 셀렉터에서의 에러를 해결할 수 있다,
>
>``` swift
>@objc func updateTime(){
>        let date = NSDate()
>        // 현재 시간을 가져옴
>        
>        let formatter = DateFormatter()
>        // formatter 객체 선언
>        formatter.dateFormat = "yyyy-MM-dd HH:mm:ss EEE"
>        lblCurrentTime.text =
>        "현재 시간 : " + formatter.string(from: date as Date)
>        
>        formatter.dateFormat = "hh:mm aaa"
>        let currentTime = formatter.string(from: date as Date)
>        
>        if (alarmTime == currentTime) {
>            view.backgroundColor = UIColor.red
>        }
>        else {
>            view.backgroundColor = UIColor.white
>        }
>    }
>
>
>```
>
>updateTime( ) 함수는 타이머가 구동된 후 정해진 시간이 되었을 때 실행할 함수로써 앞에 @objc가 붙는 이유는 #selector( )의 인자로 사용될 메소드를 선언할 때 Obj-C와의 호환성을 위하여 함수 앞에 반드시 붙여줘야하는 키워드이다.
>
>밑에는 changeDatePicker 함수에서와 비슷하게 작성해주면된다. 그 후 currentTime과 alarmTime을 비교하여 배경화면 색을 바꿔주면 완성된다.