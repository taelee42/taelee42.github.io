---

tags: [iOS, navigation controller]

---

화면을 전환하는 방법을 알아보겠습니다.

먼저 **같은 스토리보드** 내에서 화면을 전환하는 방법을 알아보고 
**다른 스토리보드**에 있는 두 화면을 전환하는 방법도 해보려고 합니다.

같은 스토리보드에서 작업 하는게 편하고 간단하지만 스토리보드를 나누면 협업시 **충돌이 적어지고 코드의 구조를 파악하기 쉽다는 장점**이 있습니다.

<!--more-->
---




# 같은 스토리보드내에서 화면전환


<img data-action="zoom" src='{{ "/assets/images/2020-02-13/a.png" | relative_url }}' alt='absolute'>

먼저 2개의 뷰 컨트롤러를 준비합니다.

2개의 뷰 컨트롤러간의 구분을 위해 배경색을 다르게 하고 가운데에 Label을 만들어줬습니다.
또 1번째 화면에서는 2번째 화면을 연결해줄 버튼을 만들었습니다.
(아직 버튼에는 아무 기능을 넣지 않았습니다.)

메인화면인 빨간화면은 프로젝트 생성시 연결된 ViewController.swift에 연결돼있는 상태이고
파란 화면은 아직 뷰컨트롤러에 연결하지 않았습니다.

## 1) navigation controller에 넣기

화면 전환을 하기 위해서는 navigation controller에 뷰 컨트롤러를 넣어줘야(embed in)합니다. embed in을 하기 위해서는 총 2가지 방법이 있습니다.

### 방법1) 뷰 컨트롤러 선택 -> editor menu -> embed in-> navigation controller

<img data-action="zoom" src='{{ "/assets/images/2020-02-13/1.gif" | relative_url }}'   alt='absolute'>

### 방법2) 뷰 컨트롤러 선택 -> 우측 하단에 embed in 아이콘 선택 -> navigation controller

<img data-action="zoom" src='{{ "/assets/images/2020-02-13/2.gif" | relative_url }}'  alt='absolute'>

## 2) 버튼에 파란화면 연결하기

<img data-action="zoom" src='{{ "/assets/images/2020-02-13/3.gif" | relative_url }}' alt='absolute'>



#### 참고
- 연결할 때 여기선 show를 선택했지만 그 이외에도 메뉴들이 몇개 더있습니다.
- 수정하고 싶은 경우 뷰 컨트롤러간의 선(segue)를 누르고 오른쪽 패널에서 kind를 바꿔주면 됩니다.(아래 gif참고)

<img data-action="zoom" src='{{ "/assets/images/2020-02-13/5.gif" | relative_url }}' alt='absolute'>



- 총 메뉴가 4개인데 하나씩 바꿔가며 실행해본결과 1번과 2,3,4번은 차이를 알겠지만 2,3,4번간의 차이는 잘 모르겠습니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-13/b.png"  | relative_url }}' alt='absolute'>



- 참고로 2,3,4번은 아래처럼 modal창으로 뜹니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-13/6.gif" | relative_url }}' width=200 alt='absolute'>



## 3) 1차 완성

<img data-action="zoom" src='{{ "/assets/images/2020-02-13/4.gif"| relative_url }}'  width=300 alt='absolute'>

아주 기본적인 화면 전환 기능을 만들었지만 몇가지 개선사항들이 보입니다.

1. 위의 흰부분(네비게이션 바)이 어색합니다.
2. 백 버튼 없이 원래 화면으로 돌아오기


## 4) 위 흰부분(네비게이션 바) 없애기

네비게이션 관련된 부분은 각각의 뷰 컨트롤러의 Navigation *** 에서 설정할 수 있습니다.

**전체적인 설정**은 Navigation Controller Scene -> Navigation Bar에서 할 수 있고
각 뷰 컨트롤러마다의 설정은 해당 View Controller Scene -> Navigation Item에서 할 수 있습니다.

네비게이션 바의 스타일을 `Large Titles`로 바꾸면 네비게이션바의 흰부분이 사라집니다.


<img data-action="zoom" src='{{ "/assets/images/2020-02-13/7.gif" | relative_url }}' alt='absolute'>



타이틀도 함께 입력해주겠습니다.
타이틀을 입력하는 방법도 2가지인데

### 방법1) storyboard에서 직접 바꾸기


<img data-action="zoom" src='{{ "/assets/images/2020-02-13/10.gif" | relative_url }}' 
alt='absolute'>


### 방법2) ViewController.swift에서 코드로 만들기

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        title = "Title: Screen1" //이 부분을 추가함
    }
}
```

[코드 보러가기(github)](https://github.com/taelee42/swift-experiments/commit/3e405906e42918c31fe7ce463a0b9559b56d61cb#diff-53a8df938c24b22e3c07ffe068d6b57a080513f06e3184947b03360d2370e671)

## 5) 뒤로가기 버튼 색상 변경

이대로 실행하면 뒤로가기 버튼의 기본색이 파란색이라 두번째 화면에서 뒤로가기 버튼이 보이지 않습니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-13/c.png"  | relative_url }}'  width=600 alt='absolute'>

위 부분을 흰색으로 바꿔줬습니다.

## 6) 완성
<img data-action="zoom" src='{{ "/assets/images/2020-02-13/11.gif" | relative_url }}' 
 width=300 alt='absolute'>


---

잘못된 정보가 있으면 댓글로 알려주세요!

