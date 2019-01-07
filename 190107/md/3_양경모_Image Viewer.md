## SWIFT를 이용한 아이폰 앱 만들기

***

### Profile

> 작성자 : 양경모
>
> 소속 : Tema S.C.P
>
> 발표일 : 2019 01 06 월요일

***

### What is SWIFT ?

> 스위프트는 2014년에 공개된 애플이 개발한 프로그래밍 언어이다.
>
> 기존 Objective-C의 단점을 보완하고, LLVM/Clang 컴파일러로 빌드되는 언어이다.
>
> IOS, macOS를 대상으로 한다.

***

### What are we learning?

> 필자는 문법만 공부하는 공부법을 별로 좋아하지 않는 사람이여서, 뷰 기반으로 프로그래밍하며 문법 공부를 겸하였다. 그래야 만들어가면서 배우는 재미가 있으므로 그러므로 본 문서에서도 뷰 기반 프로그래밍으로 진행하며 중간 중간에 나오는 용어만 설명하는 방식으로 진행하겠다. 물론 인터페이스 빌더를 사용하지않고 앱을 작성하는 것은 가능하지만 있는 기능을 사용하지 않는다면 흠.. 필자는 사용할 것 이다.
>
> 책은  <a href="https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=125264941"> Do It 스위프트로 아이폰 앱 만들기 입문 </a> 을 사용 하였으며, 미션 파일들은 <a href="https://github.com/doitswift/example">본 링크</a> 로 가면 확인할 수 있다.
>
> 또 가장 베이스를 끌고 가는 애플에서 만든 <a href="http://seoh.github.io/Swift-Korean/">Swift Programming Language</a> (이 문서는 애플 홈페이지에 있는 원서를 번역한 것) 를 확인하면서 공부하면 더욱더 도움이 될 것이다. 

***

###  Adapted to Xcode

> Xcode는 애플에서 개발한 macOS, IOS, tvOS 및 watchOS 개발 전용 IDE이다. Windows에서 보자면 Visual Studio같은 프로그램이다. Xcode에서 지원 언어는 C, C++, Objective-C, Swift, AppleScript, Java, Python, Ruby이며, 써드파티 툴을 이용하면 파스칼, 에이다, C#, Perl, D도 사용 가능하다. 여기서 우리는 Swift를 이용할 것이다.
>
> 당신이 애플 생태계 사용자(~~소위 말하는 앱등이~~)라면 Xcode를 써보는것도 나쁘지 않다. 예전에는 유료였지만 Xcode 4부터 무료로 변경되었으므로 당신이 OS X를 사용중이라면 설치해두는 것도 좋을 것이다. 물론 용량이 부족하다면 설치하지 않도록!
>
> 여기서 Xcode에 대해서 많은 것을 설명할 수 없다... 그렇다면 하루 발표분량으로도 충분하기 때문에 사용하면서 어려운것은 구글링을 하면서 알아가자. 구글링이 있으니까

***

### View based Programming

> 책에서 2장은 기본적인 뷰 기반 프로그래밍 소개이기도 하고 도전이 없으므로 3장부터 시작하겠다.
>
> 3장에서의 도전!은 이미지 뷰어를 만드는 것이다.
>
> ![3장_도전](/Users/yanggaeng/Downloads/3장_도전.jpeg)
>
> 이러한 문제이다. 이미지 뷰어를 만들기 위해서는 이전, 다음으로 가는 버튼과 이미지가 보여지는 이미지뷰 사용해야한다.
>
> !image-20190105175942508](/Users/yanggaeng/Library/Application Support/typora-user-images/image-20190105175942508.png)

***

## Process

> 우선 출력할 이미지를 프로젝트 폴더에 넣어준다.
>
> ![image-20190105180932858](/Users/yanggaeng/Library/Application Support/typora-user-images/image-20190105180932858.png)
>
> 파일을 프로젝트에 끌어서 넣으면 설정 창이 나오는데 그 부분에서 Destination : Copy items if needed를 체크해준다. 
> 이 설정은 이미지를 추가했을 때 이미지를 프로젝터 폴더로 복사할지 말지 선택하는 옵션이다.
>
> 그 다음 스토리보드에 Image View 객체와 Button 객체를 추가해준다.
>
> 여기서 Image View는 이미지를 출력해주는 객체이며 Button은 모두가 아는 그 버튼이다! 
>
> ![image-20190105180712014](/Users/yanggaeng/Library/Application Support/typora-user-images/image-20190105180712014.png)
>
> 우선 완성된 코드를 보여주고 설명하겠다.
>
> ~~~swift
> import UIKit
> 
> var numImage = 0
> 
> class ViewController: UIViewController {
>     @IBOutlet weak var imgViewer: UIImageView!
>     
>     var imageName = [ "01.png", "02.png", "03.png", "04.png", "05.png", "06.png"]
>     
>     override func viewDidLoad() {
>         super.viewDidLoad()
>         // Do any additional setup after loading the view, typically from a nib.
>         
>         imgViewer.image = UIImage(named: imageName[0])
>     }
> 
>     @IBAction func btnPrevImg(_ sender: UIButton) {
>         numImage -= 1
>         if (numImage < 0) {
>             numImage = imageName.count - 1
>         }
>         
>         imgViewer.image=UIImage(named: imageName[numImage])
>     }
>     
>     @IBAction func btnNextImg(_ sender: UIButton) {
>         numImage += 1
>         if (numImage >= imageName.count) {
>             numImage = 0
>         }
>         
>         imgViewer.image = UIImage(named: imageName[numImage])
>     }
> }
> 
> 
> ~~~
>
> 우선 `` var numImage = 0 `` 을 사용하여 배열에 사용할 숫자를 전역 변수로 선언
>
>
>
> #### Outlet Variable
>
> ---
>
> `` @IBOutlet weak var imgViewer: UIImageView! `` 는 아웃렛 변수 imgViewer를 추가한 것이다.
>
> 여기서 @IBOutlet 으로 시작하는 코드가 있다. 이것이 아웃렛 변수를 추가한 코드인데 여기서 @IB는 Interface Builder의 약자로 @IB로 시작하는 변수나 함수는 인터페이스 빌더에 관련된 변수나 함수라는 것을 의미한다. 
>
> 여기서 Interface Builder는 
>
> ```
> Xcode 내의 Interface Builder 편집기는 코드를 작성하지 않고 전체 사용자 인터페이스를 간단하게 디자인할 수 있도록 해 줍니다. 간단하게 윈도우, 단추, 텍스트 필드 및 기타 대상체를 디자인 캔버스로 드래그 앤 드롭하여 작동하는 사용자 인터페이스를 만들 수 있습니다.
> ```
>
> 라고 애플 홈페이지에 자세하게 서술되어있다.
>
> 다시 돌아가서 이제 아웃렛 변수를 설명하자면 아웃렛 변수란 객체에 대한 속성을 지정할때 연결한다. 영어 그대로 출구라는 의미로 무엇인가를 출력할때 사용하는 변수이다.
>
> 그리고 변수명 앞에 var를 볼 수 있다. 변수 선언시 종류를 정하는 것인데 종류는 var와 let이 있다.
> var는 변경이 가능한 변수이며 let은 변경이 불가능한 상수다. 이것은 파이썬에서 배열과 튜플과도 같은 차이라고 볼 수 있다
>
> ~~~ swift
> var someVarible = 3
> someVarialbe = 4	// 변경 가능
> 
> let someConstant = 4
> // someConstant = 7 // 에러 발생  
> ~~~
>
> 이런 식으로 사용한다.
>
> 그 다음 `` var imageName = [ "01.png", "02.png", "03.png", "04.png", "05.png", "06.png"] `` 로 파일들을 배열에 넣어준다.
>
> 그리고 그 밑에 함수인 viewDidLoad() 함수는 간단하게 말해서 스토리보드가 실행될때 실행되는 함수이다. main 함수라고도 생각할 수 있다.
> 그리고 그 안에 `` imgViewer.image = UIImage(named: imageName[0]) `` 를 선언하여 이미지 뷰 객체에 처음에 실행했을 때 나올 이미지를 넣어준다.
>
> 이제 액션 함수를 추가한 부분이다 
>
> ```` swift
> @IBAction func btnPrevImg(_ sender: UIButton){
>     
> }
> @IBAction func btnNextImg(_ sender: UIButton){
>     
> }
> ````
>
> 추가하면 이런식으로 만들어 질 것이다. 추가하는 방법은 발표시 설명하겠다. 앞에서 아웃렛 변수에서 설명했던 것 처럼 @IB가 붙어있으면 인터페이스 빌더에서 사용하는  그 뒤에 Action func 를 붙여주면 인터페이스 빌더에서 사용하는 액션 함수를 선언한 것이며 그 뒤에 함수명을 입력해주며 뒤에 (_ sender: UIButton) 은 액션 함수가 실행되도록 이벤트를 보내는 객체, 즉 여기서는 버튼 객체에서 이벤트가 발생했을 때 해당 액션 함수를 실행시킬 것 이므로 UIButton 클래스 타입을 선택한다. 
>
>
>
> #### Function 
>
> ---
>
>  btnPrevImg 함수 실행문을 보면 
>
> ``` swift
> numImage -= 1 
> // 1 빼준다
> 	if (numImage < 0){
>   	  numImage = imageName.count - 1
> // numImage가 0보다 작다면 배열에 인덱스에러가 나기때문에 image.count = 6인데 맨 마지막 인덱스로 가		기 위해 -1을 해준다
> 	}
> imgViewer.image=UIImage(named: imageName[numImage]) 	// imgViewer 객체에 사진을 변경
> ```
>
> btnNextImg 함수 실행문을 보면
>
> ``` swift
>  numImage += 1
> // 1 더해준다
>         if (numImage >= imageName.count) {
>             numImage = 0
>         }
> // numImage가 6보다 크거나 같으면 인덱스 에러가 난다 따라서 0으로 초기화 시켜준다.
>         
>  imgViewer.image = UIImage(named: imageName[numImage])
> ```
>
> 이렇게 작성하고 실행하면 주어진 예제처럼 만들어진다.