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
>![](https://adminstvo.files.wordpress.com/2011/04/tabbar.png)
>
>탭 바 컨트롤러는 보이는 거와 같이 하단에 있는 탭을 이용해 뷰 컨트롤러를 이동하는 방식의 뷰 컨트롤러다. 탭 바 컨트롤러를 이용하기 위해서는 메뉴 바에서 Editor -> Embed In -> Tab Bar Controller 을 선택하면 된다.
>
>두 번째 연결부터는 탭 바 컨트롤러에서 ⌃ + 드래그로 View Controller를 연결한다.
>
>---

### :question: First Problem

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

### :pushpin: Second Topic 

> 두번째 주제는 동영상 플레이어에 대한 주제이다. 우선 동영상 플레이어를 이용하기 위해서는 AVKit를 import 를 해주어야한다. 
>
> 그리고 자료형들 중에서 전치사로 붙어있는 NS, CG에 대해서도 알아볼 것이다.
>
> 우선 NS에 대해서는 위키피디아에서 찾아보면
>
> ```
> NeXTSTEP is a discontinued object-oriented, multitasking operating system based on UNIX. It was developed by NeXT Computer in the late 1980s and early 1990s and was initially used for its range of proprietary workstation computers such as the NeXTcube. It was later ported to several other computer architectures.
> Although relatively unsuccessful at the time, it attracted interest from computer scientists and researchers. It was used as the original platform for the development of the Electronic AppWrapper, the first commercial electronic software distribution catalog to collectively manage encryption and provide digital rights for application software and digital media, a forerunner of the modern "app store" concept. It was also the platform on which Tim Berners-Lee created the first web browser, and on which id Software developed the video game Doom.
> After the purchase of NeXT by Apple, it became the source of the popular operating systems macOS, iOS, watchOS, tvOS, and audioOS. Many bundled macOS applications, such as TextEdit, Mail, and Chess, are descendants of NeXTSTEP applications.
> 
> 											  - https://en.wikipedia.org/wiki/NeXTSTEP
> ```
>
> 라고 설명되어있다. 해석을 해서 요약해보면 NeXTSTEP이라는 기업을 인수해서 애플에서 응용 프로그램 중에 번들로 제공되는 여러 응용 프로그램들은 NeXTSTEP Application의 자손중에 하나이다. 뒤에서 다룰 CG와 마찬가지로 이미 다른 곳에서 사용되는 일반 자료형들과의 충돌을 막기위해 앞에 접두사 NS를 붙여서 사용하는 것이다.
>
> CG에 대해서는 애플 개발자 사이트에서 찾아보면
>
> ```
> You should try to choose names that clearly associate each symbol with your framework. 
> For example, consider adding a short prefix to all external symbol names. 
> Prefixes help differentiate the symbols in your framework from those in other frameworks and libraries. 
> They also make it clear to other developers which framework is being used. 
> Typical prefixes include the first couple of letters or an acronym of your framework name. 
> For example, functions in the Core Graphics framework use the prefix “CG”.
> 
> -https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/CreationGuidelines.html
> ```
>
> 라고 설명되어있다. 이처럼 CG 또한 기본 함수나 자료형들과의 충돌을 막기위해 Cotre Graphics FrameWrok의 함수들 앞에는 접두사 CG를 붙여서 사용하는 것이다.
>
> ---

### :question: Second Problem

>두번째 주제에 대한 문제이다.
>
>![image](https://user-images.githubusercontent.com/46397818/53105050-c12d6680-3573-11e9-9b1a-1a3fefd3e9ba.png)
>
>문제 전의 실습에서는 저 동영상 썸네일들은 버튼을 이용하고 그 버튼에 이미지를 삽입해서 만든 것이다. 저 버튼을 누르면 앱 내부에서 실행하는 것은 따로 파일 경로를 파일명과 확장자를 지정해주어서 실행해주는 것이며, 외부 링크로는 말그대로 우리가 아는 인터넷 링크를 넣어주어서 그 링크에 해당하는 동영상을 가져와서 실행해주는 것이므로 와이파이나 데이터가 연결 되어있어야 동영상을 실행할 수 있다.

### 🧠 Solution

>이제 전체적인 코드를 보면
>
>```swift
>import UIKit
>import AVKit
>
>class ViewController: UIViewController {
>
>override func viewDidLoad() {
>   super.viewDidLoad()
>   // Do any additional setup after loading the view, typically from a nib.
>}
>
>@IBAction func btnPlayInternalMovie(_ sender: UIButton) {
>   let filePath: String? = Bundle.main.path(forResource: "FastTyping", ofType: "mp4")
>   // 우선 비디오 파일명을 사용하여 비디오가 저장된 앱 내부의 파일 경로를 받아온다.
>   let url = NSURL(fileURLWithPath: filePath!)
>   // 앱 내부의 파일명을 NSURL 형식으로 변경
>   
>   playVideo(url: url)
>   
>   /*
>    let playerController = AVPlayerViewController()
>    //인스턴스 생성
>    let player = AVPlayer(url: url as URL)  // 비디오URL로 초기환 AVPlayer의 인스턴스 생성
>    playerController.player = player    // 컨트롤러의 플레이어 속성에 인스턴스 할당
>    
>    self.present(playerController, animated: true) {
>    player.play()   // 비디오 재생
>    }
>    */
>   
>
>}
>
>@IBAction func btnPlayInternalMov(_ sender: UIButton) {
>   let filePath: String? = Bundle.main.path(forResource: "Mountaineering", ofType: "mov")
>   
>   let url = NSURL(fileURLWithPath: filePath!)
>   
>   playVideo(url: url)
>}
>
>// 외부 링크 비디오 재생
>@IBAction func btnPlayExternalMovie(_ sender: UIButton) {
>   // 외부 파일 mp4
>   let url = NSURL(string: "https://dl.dropboxusercontent.com/s/e38auz050w2mvud/Fireworks.mp4")!
>   
>   playVideo(url: url)
>   
>   
>   /*
>    let playerController = AVPlayerViewController()
>    
>    let player = AVPlayer(url: url as URL)
>    playerController.player = player
>    
>    self.present(playerController, animated: true) {
>    player.play()
>    }
>    */
>
>}
>
>@IBAction func btnPlayExtrnalMov(_ sender: UIButton) {
>   let url = NSURL(string: "https://dl.dropboxusercontent.com/s/ijybpprsmx0bgre/Seascape.mov")!
>   
>   playVideo(url: url)
>}
>
>private func playVideo(url: NSURL) {
>   let playerController = AVPlayerViewController()
>   
>   let player = AVPlayer(url: url as URL)
>   playerController.player = player
>   
>   self.present(playerController, animated: true) {
>       player.play()
>   }
>}
>}
>```
>
>앱 내부 비디오 재생은 InternalMov,Movie인 두 개의 액션 함수를 이용해서 불러오며
>
>외부 링크 비디오 재생은 ExternalMove,Movie인 두 개의 액션 함수를 이용해서 불러오는 것이다.
>
>우선 playVideo라는 함수를 보면
>
>AVPlayerViewController의 인스턴스 상수를 선언해 주는것인데, 이 AVPlayerViewController는 애플 개발자 문서에서 찾아보면 
>
>> displays the video content from a player object along with system-supplied playback controls.
>
>라고 되어있다. 해석해보면 플레이어 객체로부터 받아온 비디오 컨텐츠를 화면에 뿌려주는 것이다.
>
>그 후 AVPlayer (An object that provides the interface to control the player’s transport behavior.)라는 객체를 인스턴스 상수 player로 선언해준다.
>
>그리고 player를 playerController의 player에 넣어준다.
>
>그리고 present 함수를 이용해서 url로 연결될시 플레이러를 플레이 하게 해준다.
>
>---



