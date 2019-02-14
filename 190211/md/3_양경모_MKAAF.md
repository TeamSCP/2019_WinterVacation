## MapKitView를 이용한 Annotation & Anonymous Funcition

---

### 👦🏻 Profile

> 작성자 : 양경모
>
> 소속 : Team S.C.P
>
> 발표일 : Mon 11 Jan 2019
>
> ---

### 📌 Topic

>오늘은 MapKitView와 MapKitView의 하나의 기능인 Annotation을 살펴보며 그 후 Anonymous Function을 살펴볼 것이다.
>
>MapKitView의 기능은 이름 그대로 Map, 지도에 대한 기능을 사용하는 라이브러리다
>
>그리고  앱에서 위치정보를 사용하기 위해서는 사용자 승인을 얻어햐 한다.
>
>사용자 승인을 얻기 위해서는
>
><img width="444" alt="image" src="https://user-images.githubusercontent.com/46397818/52173561-1c1c3b00-27ca-11e9-9d88-f7987a1f1843.png">
>
>info.plist라는 파일을 열어서 설정을 해주어야한다.
>
>Privacy - Location When In Use Us.. 의 Value 값을 App needs location 으로 변경해준다.
>
>그 후 Import MapKit을 해주고 사용해야한다.
>
>그리고 익명함수인 Anonymous Function은 Closure라고도 불린다. 즉 함수안에서 함수 이름을 따로 지정해주지 않고 일회용 함수를 사용하는거다.
>
>자세한 내용은 뒤에가서 살펴보자
>
>---

### 🥏 Problem

>8장 예제를 보면 이러하다.
>
><img width="504" alt="image" src="https://user-images.githubusercontent.com/46397818/52173839-18d77e00-27cf-11e9-8ef6-c41eed4f8e6c.png">
>
>8장에서 배운 내용을 토대로 세그먼트와 핀을 추가하는 것이다. 생각보다 간단하다.
>
>그냥 8장에서 배웠던 세그먼트 컨트롤에서 하나 추가하고 Annotation 함수를 이용해서 핀을 하나 추가하면 된다.
>
>---

### 🧠 Process 

>우선 전체적인 코드를 보자
>
>``` swift
>import UIKit
>import MapKit
>
>class ViewController: UIViewController, CLLocationManagerDelegate {
>   let locationManager = CLLocationManager()
>    
>    @IBOutlet var myMap: MKMapView!
>    @IBOutlet var lblLocationInfo1: UILabel!
>    @IBOutlet var lblLocationInfo2: UILabel!
>    
>    override func viewDidLoad() {
>        super.viewDidLoad()
>        // Do any additional setup after loading the view, typically from a nib.
>        
>        lblLocationInfo1.text = ""
>        lblLocationInfo2.text = ""
>        locationManager.delegate = self
>        locationManager.desiredAccuracy = kCLLocationAccuracyBest
>        locationManager.requestWhenInUseAuthorization()
>        locationManager.startUpdatingLocation()
>        myMap.showsUserLocation = true
>    }
>
>    // 위도, 경도, 범위의 크기를 지정하면 지도에 원하는 위치 표시해주는 함수
>    func goLocation(latitudeValue : CLLocationDegrees, longitudeValue: CLLocationDegrees, delta span: Double) -> CLLocationCoordinate2D {
>        let pLocation = CLLocationCoordinate2DMake(latitudeValue, longitudeValue)
>        let spanValue = MKCoordinateSpan(latitudeDelta: span, longitudeDelta: span)
>        let pRegion = MKCoordinateRegion(center: pLocation, span: spanValue)
>        myMap.setRegion(pRegion, animated: true)
>        return pLocation
>    }
>    
>    // 위치가 업데이트 되었을때 지도에 위치를 나타내는 함수
>    func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
>        let pLocation = locations.last
>        _ = goLocation(latitudeValue: (pLocation?.coordinate.latitude)!, longitudeValue: (pLocation?.coordinate.longitude)!, delta: 0.01)
>        // 위도와 경도 값을 가지고 역으로 주소 찾아주는 함수
>        CLGeocoder().reverseGeocodeLocation(pLocation!, completionHandler: {
>            (placemarks, error) in
>            let pm = placemarks!.first
>            let country = pm!.country
>            var address:String = country!
>            if pm!.locality != nil {
>                address += " "
>                address += pm!.locality!
>            }
>            else if pm!.thoroughfare != nil {
>                address += " "
>                address += pm!.thoroughfare!
>            }
>            
>            self.lblLocationInfo1.text = "현재 위치"
>            self.lblLocationInfo2.text = address
>            
>        })
>        
>        locationManager.stopUpdatingLocation()
>    }
>    
>    func setAnnotation(latitudeValue: CLLocationDegrees, longitudeValue: CLLocationDegrees, delta span: Double, title strTitle: String, subtitle strSubtitle:String) {
>        let annotation = MKPointAnnotation()
>        annotation.coordinate = goLocation(latitudeValue: latitudeValue, longitudeValue: longitudeValue, delta: span)
>        annotation.title = strTitle
>        annotation.subtitle = strSubtitle
>        myMap.addAnnotation(annotation)
>    }
>    @IBAction func sgChangeLocation(_ sender: UISegmentedControl) {
>        if sender.selectedSegmentIndex == 0 {
>            
>        }
>        else if sender.selectedSegmentIndex == 1 {
>            setAnnotation(latitudeValue: 37.751853, longitudeValue: 128.87605740000004, delta: 1, title: "한국 폴리텍 대학 강릉 캠퍼스", subtitle: "강원도 강릉시 남산초교길 121")
>            self.lblLocationInfo1.text = "보고 계신 위치"
>            self.lblLocationInfo2.text = "한국 폴리텍 대학 강릉 캠퍼스"
>        }
>        else if sender.selectedSegmentIndex == 2 {
>            setAnnotation(latitudeVale: 37.5307871, longitudeValue: 126.8981, delta: 0.1, title: "이지스 퍼블리싱", subtitle: "서울특별시 영등포구 당산로 41길 11")
>            self.lblLocationInfo1.text = "보고 계신 위치"
>            self.lblLocationInfo2.text = "이지스 퍼블리싱 출판사"
>        }
>        else if sender.selectedSegmentIndex == 3 {
>            setAnnotation(latitudeValue: 37.642705, longitudeValue: 126.918033, delta: 0.1, title: "우리집", subtitle: "서울특별시 은평구 진관3로 15-35")
>        }
>    }
>    
>}
>```
>
>코드를 보면은 아웃렛 변수로는
>
>``` swift
>	@IBOutlet var myMap: MKMapView!
>    @IBOutlet var lblLocationInfo1: UILabel!
>    @IBOutlet var lblLocationInfo2: UILabel!
>```
>
>이렇게 세가지가 선언되어있다. 여기서 세가지는 위치 내용을 출력하기 위해 레이블 2개를 아웃렛 변수로 선언해주고 지도를 출력하기 위해 MapKitView 라이브러리를 아웃렛 변수로 선언해준다.
>
>그 후 선언된 함수를 보면
>
>goLocation 함수는 구조체인 CLLocationCoordinate2D을 자료형으로 반환하는 함수이며 매개변수로 CLLocationDegrees형인 latitudeValue, CLLocationDegrees형인 longitudeValue, Double형인 delta span을 받는다. 상수들로 pLocation, spanValue, pRegion을 선언해주며 pLocation을 반환해주며 pLocation의 자료형은 CLLocationCoordinate2D이다.
>
>``` swift
> func goLocation(latitudeValue : CLLocationDegrees, longitudeValue: CLLocationDegrees, delta span: Double) -> CLLocationCoordinate2D {
>        let pLocation = CLLocationCoordinate2DMake(latitudeValue, longitudeValue)
>        let spanValue = MKCoordinateSpan(latitudeDelta: span, longitudeDelta: span)
>        let pRegion = MKCoordinateRegion(center: pLocation, span: spanValue)
>        myMap.setRegion(pRegion, animated: true)
>        return pLocation
>    }
>```
>
>locationManager 함수 선언문을 보면 매개변수로 CLLocationManger형의 manager와 CLLocation형의 배열을 받고 상수 pLocation 값을 locations 배열의 마지막 값을 받아 저장하고, 마지막 위치의 위도와 경도 값을 가지고 앞에서 만든 goLocation 함수를 호출할 때 delta 값은 지도의 크기를 결정하는데, 값이 작을수록 확대되는 효과가 나타난다. 0.01로 하였으니 100배 확대된다.
>
>위도와 경도 값을 가지고 역으로 위치의 정보, 즉 주소를 찾아보겠다. 핸들러의 익명 함수를 추가로 준비한다. 쉽게 말해 reverse Geocode Location 함수에서 내부 파라미터인 핸들러를 익명 함수로 처리하기 위해 중괄호를 넣는다.
>
>여기서 핸들러의 추가 함수를 작성하는데 placemarks 값의 첫 부분만 pm 상수로 받는다. 이 pm 상수에서 나라 값을 추출하고, 지역과 도로는 분기문에서 존재할 경우에 출력하도록 하겠다. 각각의 값 사이에는 공백을 넣어줘서 가독성을 높여주었다.
>
>```swift
>    func locationManager(_ manager: CLLocationManager, didUpdateLocations locations: [CLLocation]) {
>        let pLocation = locations.last
>        _ = goLocation(latitudeValue: (pLocation?.coordinate.latitude)!, longitudeValue: (pLocation?.coordinate.longitude)!, delta: 0.01)
>        // 위도와 경도 값을 가지고 역으로 주소 찾아주는 함수
>        CLGeocoder().reverseGeocodeLocation(pLocation!, completionHandler: {
>            (placemarks, error) in
>            let pm = placemarks!.first
>            let country = pm!.country
>            var address:String = country!
>            if pm!.locality != nil {
>                address += " "
>                address += pm!.locality!
>            }
>            else if pm!.thoroughfare != nil {
>                address += " "
>                address += pm!.thoroughfare!
>            }
>            
>            self.lblLocationInfo1.text = "현재 위치"
>            self.lblLocationInfo2.text = address
>            
>        })
>        
>        locationManager.stopUpdatingLocation()
>    }
>```
>
>setAnnotation 함수의 선언문을 살펴보면 위,경도 값을 받고 delta값과 title, subtitle값을 매개변수로 받는다. 상수 annotation 객체를 선언해주고, annotation.coordinate의 값을 goLocation의 함수로 부터 받아온다. 그리고 제목과 부제를 받아주고 지도에 추가해준다.
>
>```swift
>func setAnnotation(latitudeValue: CLLocationDegrees, longitudeValue: CLLocationDegrees, delta span: Double, title strTitle: String, subtitle strSubtitle:String) {
>        let annotation = MKPointAnnotation()
>        annotation.coordinate = goLocation(latitudeValue: latitudeValue, longitudeValue: longitudeValue, delta: span)
>        annotation.title = strTitle
>        annotation.subtitle = strSubtitle
>        myMap.addAnnotation(annotation)
>    }
>```
>
>Segment Control 라이브러리 액션 함수인 sgChangeLocation을 살펴보면 segmentIndex를 확인해서 각각 알맞는 값을 뿌려준다.
>
>``` swift 
> @IBAction func sgChangeLocation(_ sender: UISegmentedControl) {
>        if sender.selectedSegmentIndex == 0 {
>            
>        }
>        else if sender.selectedSegmentIndex == 1 {
>            setAnnotation(latitudeValue: 37.751853, longitudeValue: 128.87605740000004, delta: 1, title: "한국 폴리텍 대학 강릉 캠퍼스", subtitle: "강원도 강릉시 남산초교길 121")
>            self.lblLocationInfo1.text = "보고 계신 위치"
>            self.lblLocationInfo2.text = "한국 폴리텍 대학 강릉 캠퍼스"
>        }
>        else if sender.selectedSegmentIndex == 2 {
>            setAnnotation(latitudeVale: 37.5307871, longitudeValue: 126.8981, delta: 0.1, title: "이지스 퍼블리싱", subtitle: "서울특별시 영등포구 당산로 41길 11")
>            self.lblLocationInfo1.text = "보고 계신 위치"
>            self.lblLocationInfo2.text = "이지스 퍼블리싱 출판사"
>        }
>        else if sender.selectedSegmentIndex == 3 {
>            setAnnotation(latitudeValue: 37.642705, longitudeValue: 126.918033, delta: 0.1, title: "우리집", subtitle: "서울특별시 은평구 진관3로 15-35")
>        }
>    }
>```
>
>여기까지 함수들을 확인해보았다.
>
>이렇게 함수를 만들어주고 적용하면 발표의 내용처럼 나올 것이다.
>
>---

###👁‍🗨 Closure (Anonymous Function)

>클로저는 함수형 프로그래밍 언어의 가장 큰 특징 중 하나이다.
>
>클로저에 대해 설명하기 전에 다음의 코드를 보자
>
>``` swift
> let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
>
> func backward(s1: String, s2: String) -> Bool {
>   return s1 > s2;
> }
> // [Chris, Alex, Ewa, Barry, Daniella]
>
> println("정렬 전 names: \(names)")
>
> var reversed = sort(names, backward)
> // [Ewa, Daniella, Chris, Barry, Alex]
>
> println("정렬 후 names: \(names)")
>									   	// [출처] [Swift] 클로저 (Closure)|작성자 조원호
>```
>
>위 코드의 sort() 함수에서 파라미터로 사용한 정렬 방식이 바로 Swift에서의 클로저이다. 바로 파라미터로 사용하는 것이다. Swift에서 사용하는 클로저는 위 sort 함수처럼 함수 이름을 파라미터로 사용할 수도 있지만, 함수 이름 대신 아예 함수 본문을 사용할 수도 있다.
>
>다음은 애플에서 제공하는 "Swift Programming Language" 문성세서 다음과 같이 클로저 표현식이 정의되어 있다.
>
>``` swift
> { (파라미터들) -> 리턴_타입 in
>
>   문장들
>
> }
>```
>
>기존의 함수 타입과 클로저의 차이점은 in 이라는 키워드를 사용했다는 점이다.
>
>위 코드를 클로저를 적용한 예제를 보자
>
>``` swift
>   let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
> // [Chris, Alex, Ewa, Barry, Daniella]
>
> println("정렬 전 names: \(names)")
>
> var reversed = sort(names, 
>   {
>     (s1: String, s2: String) -> Bool in
>      return s1 > s2
>   }
> )
> // [Ewa, Daniella, Chris, Barry, Alex]
>
> println("정렬 후 names: \(names)")
>										// [출처] [Swift] 클로저 (Closure)|작성자 조원호
>```
>
>위의 클로저 형태는 기존의 함수에 비해 함수 이름이 줄고, 클로저를 사용하는 함수의 파라미터 부분에 직접 코드 블록을 작성할 수 있다.
>
>---



