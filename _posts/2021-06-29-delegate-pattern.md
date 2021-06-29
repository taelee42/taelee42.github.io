---
title: iOS | iOS에서 Delegate Pattern 만드는 순서 (화면 전환시 데이터 변경을 알려주고 싶을 때)
tags: [iOS, swift]
date: 2021-06-29 04:11:00 +09:00
categories: [iOS]

---

Closure가 무엇이고 어떻게 사용하는지 알아보도록 하겠습니다.

<!--more-->
---

## 델리게이트 패턴을 쓰는 이유
앱에서 여러 화면들간의 데이터 변경을 알려주는 용도로 델리게이트 패턴을 사용하는 것 같습니다.
이외에 KVO라던가 Notification Center등을 이용할 수도 있습니다.


## 델리게이트 패턴을 적용할 상황 설명
메인 화면(1)과 정보를 수정하는 화면(2)이 있다고 가정해 볼게요
수정하는 화면에서 정보를 수정하고 메인화면으로 돌아오면 데이터가 변경되어 있게끔 구현하려고 합니다.

이때 델리게이트 패턴을 쓰게 되는데요

아래처럼 쥬스를 주문하는 화면을 메인화면(=첫번째 화면)이라고 하겠습니다.

<img data-action="zoom" src="https://images.velog.io/images/taelee/post/cd5ee310-3266-4033-a88c-005680d90d37/image.png" width=500>

그리고 정보를 수정하는 화면을 재고수정화면(=두번째 화면)이라고 하겠습니다.

<img data-action="zoom" src="https://images.velog.io/images/taelee/post/c3e8d2b7-0948-4290-ad48-aec217cdf6f1/image.png" width=500>



## 델리게이트 패턴 구현하기

### 1. 델리게이트 프로토콜 구현하기
```swift
//StockDelegate.swift
protocol StockDelegate {
    func stockDidChange(_ vc: UIViewController)
}
```
- 새로운 파일을 하나 생성해서 델리게이트로 맡길 함수를 하나 작성합니다.
이 경우에는 재고 추가를 완료하고 재고(stock)이 바뀌것을 알려줄 예정이니 stockDidChange()라는 함수를 만들었습니다.

### 2. 2번째 화면(재고 수정화면)에 해줘야 할 일

```swift
//ChangeStockViewController.swift
//재고 추가 화면에 관한 뷰컨트롤러입니다.
class ChangeStockViewController: UIViewController {
  ...
    var delegate: StockDelegate?
  ...
    @IBAction func touchUpCloseButton(_ sender: UIBarButtonItem) {
        dismiss(animated: true, completion: nil)
        delegate?.stockDidChange(self)
    }
  ...
}
```

2번째 화면에 델리게이트를 장착해서 그 델리게이트에게 데이터가 바뀌엇음을 알려줄 예정입니다.
- 1) 장착하기 위해서 `var delegate: StockDelegate?`아까 만든 프로토콜을 채용하는 변수자리를 하나 만들어줍니다.
- 2) 닫기 버튼을 눌렀을때 장착될 델리게이트에게 현재 데이터수정이 완료되었음을 알려줍니다.


### 3. 첫번째 화면(메인화면)에서 해야할 일
- 여기서는 2가지 일을 처리해야합니다.
  1. 첫번째 화면(메인화면)에서 두번째 화면(재고수정화면)으로 넘어갈 때 델리게이트 장착시키기
  2. 델리게이트가 실제 구현해야할 사항 만들기

요렇게 2가지를 구현하겠습니다.

```swift
class ViewController: UIViewController {
...
    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        guard let vc = segue.destination.children.first as? ChangeStockViewController else {
            return
        }
        vc.delegate = self
    }
}

extension ViewController: StockDelegate {
    func stockDidChange(_ vc: UIViewController) {
        updateStockLabels()
    }
}
```

#### 1) 첫번째 화면(메인화면)에서 두번째 화면(재고수정화면)으로 넘어갈 때 델리게이트 장착시키기

저는 첫번째 화면에서 segue를 이용해서 두번째 화면으로 넘어가는데요
이때 prepare라는 메서드가 segue로 넘어가기 직전에 불려진다고 합니다. 그래서 이곳에 2번째 화면의 델리게이트를 현재 뷰컨트롤러(메인화면의 뷰컨트롤러)로 장착했습니다.
`vc.delegate = self`입니다. vc가 segue가 향하는 첫번째 목적지이고 self는 현재 뷰컨트롤러인 메인 화면의 뷰컨트롤러입니다.

##### 참고사항
만약 segue로 화면을 전환하지 않고 스토리보드를 찾아 그 스토리보드내의 뷰컨트롤러를 인스턴스화해서 화면을 바꾸는 경우라면 아래와 같은 코드를 사용하시면 됩니다.

```swift
guard let controller = self.storyboard?.instantiateViewController(identifier: "controller") else { return }
guard let view = controller.children.first as? ChangeStockViewController else { return }
view.delegate = self
self.present(controller, animated: true, completion: nil)
```

#### 2) 델리게이트가 실제 구현해야할 사항 만들기
- 위코드의 익스텐션 부분에 작성했는데요.
```swift
extension ViewController: StockDelegate {
    func stockDidChange(_ vc: UIViewController) {
        updateStockLabels()
    }
}
```
프로토콜에서 이미 선언했던 함수인 stockDidChange를 사용해서 실제 이 함수가 불러질 때(2번째 화면에서 닫기버튼을 눌렀을때) 실행할 구현사항을 적습니다.
저는 이때 화면의 label들이 다시 불러와지는 updateStockLabels()를 만들어서 이 함수가 구현되도록 실행했습니다.



## 구현한 사항 복습하기

1. 델리게이트 프로토콜을 파일로 작성합니다.

   여기에는 2번째 화면인 재고수정화면에서 데이터 수정이 완료되었음을 알려주는 함수를 선언해둡니다.

2. 2번째 화면에서 해야할 일

   1. 2번재 화면은 델리게이트에게 일을 맡길 예정이기 때문에 델리게이트가 장착될 변수를 만듭니다.

   2. 그리고 데이터 수정이 완료되면(여기서는 닫기버튼을 누르면) 델리게이트의 함수(위의 프로토콜에서 선언한)를 실행합니다.

      (이 함수가 어떤 일을 할 지는 다음스텝에서 작성합니다.)

3. 1번재 화면에서 해야할 일

   1. 첫번재 화면에서 두번재 화면으로 넘어갈 때 두번째 화면에게 델리게이트를 작창시킵니다.

      첫번째 화면이 두번째 화면의 델리게이트가 될 예정이기 때문입니다.

   2. 델리게이트의 구현된 함수가 정확히 어떤 일을 할지 구현합니다.



