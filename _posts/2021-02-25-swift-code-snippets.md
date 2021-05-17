---
title: Swift | Code Snippets
tags: [iOS, swift, closure]
date: 2021-05-15 15:22:00 +09:00

---

스위프트를 공부하면서 수집한 코드 조각들입니다.

<!--more-->
---





# 뒤로가기 버튼을 다른 모양의 버튼으로 만들고 뒤로가기 기능을 추가해주는 코드

  - DesignPatternByTutorials -> 04 Delegation Pattern

```swift
public override func viewDidLoad() {
    super.viewDidLoad()
    setupCancelButton()
}
private func setupCancelButton() {
    let action = #selector(handleCancelPressed(sender:))
    let image = UIImage(named: "ic_menu")
    navigationItem.leftBarButtonItem = UIBarButtonItem(image: image, landscapeImagePhone: nil, style: .plain, target: self, action: action)
}

@objc private func handleCancelPressed(sender: UIBarButtonItem) {
    delegate?.questionViewController(self, didCancel: questionGroup, at: questionIndex)
  }
```

>#selector와 @objc는 알아봐야 될듯









# 특정 셀 선택되었을 때 이거는 여기가 아니라 collectionView protocol로 가야할 듯
```swift
extension PlayViewController: UICollectionViewDelegate {
    //클릭했을 때 동작
    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {
    }
}
```


# awakenib

```swift
override func awakeFromNib() {
        super.awakeFromNib()
    }
```

# 터치된 부분의 좌표를 알려주는 코드
```swift
//viewController.swift
override func touchesBegan(_ touches: Set<UITouch>, with event: UIEvent?) {
    if let touch = touches.first {
        let location = touch.location(in: self.view)
        print(location)
    }

}
```

# 오토 레이아웃 코드로 promatically하게 짜기
```swift
completeMenu.leadingAnchor.constraint(equalTo: view.safeAreaLayoutGuide.leadingAnchor).isActive = true
```



# 위도 경도 처리하는 코드
```swift
import CoreLocation

let location = CLLocation(latitude: 37.498206, longitude: 127.02761)
func printLocation(location: CLLocation) {
    print(location.coordinate.latitude)
    print(location.coordinate.longitude)
}
printLocation(location: location)
```







# Json parsing할 때 Codable스트럭처 예시

- 파싱해야될 json형태
```json
{
   "resultCount":1,
   "results":[
      {
         "trackId":1485324750,
         "artistName":"Hayao Miyazaki",
         "trackName":"My Neighbor Totoro",
         "previewUrl":"https://video-ssl.itunes.apple.com/itunes-assets/Video113/v4/ee/88/5d/ee885d14-75e4-aa89-d38f-f2aada66c05f/mzvf_8063807238545732413.640x478.h264lc.U.p.m4v",
         "artworkUrl30":"https://is3-ssl.mzstatic.com/image/thumb/Video123/v4/c9/3e/0b/c93e0ba6-da8d-ec7e-26f3-01da52579766/source/30x30bb.jpg",
         "artworkUrl60":"https://is3-ssl.mzstatic.com/image/thumb/Video123/v4/c9/3e/0b/c93e0ba6-da8d-ec7e-26f3-01da52579766/source/60x60bb.jpg",
         "artworkUrl100":"https://is3-ssl.mzstatic.com/image/thumb/Video123/v4/c9/3e/0b/c93e0ba6-da8d-ec7e-26f3-01da52579766/source/100x100bb.jpg"
      }
   ]
}
```

- 이름을 같게 추출할때는 구조체에 이름과 형식만 선언하면 된다.
- 이름이 다를 경우 CodingKeys 열거형을 사용해서 추출한다.
- 오브젝트안에 오브젝트가 있는 경우에는 구조체를 중첩해서 사용한다.

```swift
struct Response: Codable {
    let resultCount: Int
    let movies: [Movie]
    
    enum CodingKeys: String, CodingKey {
        case resultCount
        case movies = "results"
    }
}

struct Movie: Codable {
    let title: String
    let director: String
    let thumbnailPath: String
    let previewURL: String
    
    enum CodingKeys: String, CodingKey {
        case title = "trackName"
        case director = "artistName"
        case thumbnailPath = "artworkUrl100"
        case previewURL = "previewUrl"
    }
}

```

# Dispatch Group 사용 예시
```swift
    let apiQueue = DispatchQueue(label: "ApiQueue", attributes: .concurrent)
    
    let group = DispatchGroup()
    
    func fetch(location: CLLocation, completion: @escaping () -> ()) {
        group.enter()
        apiQueue.async {
            self.fetchCurrentWeather(location: location) { (result) in
                switch result {
                case .success(let data):
                    self.summary = data
                default:
                    self.summary = nil
                }
                self.group.leave()
            }
        }
        
        group.enter()
        apiQueue.async {
            self.fetchForecast(location: location) { (result) in
                switch result {
                case .success(let data):
                    self.forecast = data
                default:
                    self.forecast = nil
                }
                self.group.leave()
            }
        }
        group.notify(queue: .main) {
            completion()
        }
    }
```

queue를 만들고 Dispath Group으로 그 큐들을 다루는 논리적인 그룹을 만들었다.
queue별로 group.enter()로 그룹에 진입하고
`self.group.leave()`로 빠져나온다.
모두 빠져나오면 `group.notify(queue: .main)`으로 실행할 코드를 작성한다.
여기에서 두가지 인자 queue와 closure를 받는데 큐에서 해당 클로져가 실행되는 코드이다.

# Singleton Pattern
```swift
class WeatherDataSource {
    static let shared = WeatherDataSource()
    private init() {}
}
```
이렇게 사용한다
생성자가 외부에서 실행되지 않도로고 private으로 선언해줘야한다.


# 특정 뷰에 이미지 넣기

```swift
cell.weatherImageView.image = UIImage(named: weather.icon)
```

# 온도 data formatter
```swift
//Double+Formatter.swift
fileprivate let temperatureFormatter: MeasurementFormatter = {
    let f = MeasurementFormatter()
    f.locale = Locale(identifier: "ko_kr")  //로케일 변경
    f.numberFormatter.maximumFractionDigits = 1 // 소숫점이 최대 1자리까지만, 0이면 안보이게함
    f.unitOptions = .temperatureWithoutUnit // 섭씨나 화씨기호가 안보이게, ° 이거는 보임
    return f
}()

extension Double { //더블에 확장해서 .으로 쉽게 사용하게 만듬
    var temperatureString: String {
        let temp = Measurement<UnitTemperature>(value: self, unit: .celsius) //위의 MesurementFormatter()이기 때문에 Mesurement형식으로 만들어줘야 하는것 같다.
        return temperatureFormatter.string(from: temp)
```

# 코드로 계산한뒤에 inset바꾸기


```swift
    var topInset = CGFloat(0.0)
    
    override func viewDidLayoutSubviews() {
        super.viewDidLayoutSubviews()
        
        if topInset == 0.0 {
            let firstIndexPath = IndexPath(row: 0, section: 0)
            if let cell = listTableView.cellForRow(at: firstIndexPath) {
                topInset = listTableView.frame.height - cell.frame.height
                
                var inset = listTableView.contentInset
                inset.top = topInset
                listTableView.contentInset = inset
            }
        }
    }
```

# 현재 실행되는 부분이 메인쓰레드인지 확인하는 코드

```swift
print(Thread.isMainThread ? "Main Thread": "Background Thread")
```