---
title: iOS | 화면전환하기2 (다른 스토리보드, navigtaion controller)
tags: [iOS, navigation controller]

---

이전 포스트에서 **동일 스토리보드**에서 화면전환하는 법을 해봤고 이 포스트에서는 스토리보드를 나눠서 화면전환하는 법을 진행하겠습니다.

<!--more-->
---

# 🦊 다른 스토리보드에 있는 뷰컨트롤러간의 화면 전환하기

## 1. 2개의 스토리보드, 2개의 뷰컨트롤러 생성하기

프로젝트를 생성하면 이미 한쌍의 Main.storyboard와 ViewController.swift파일이 생성되어 있습니다.

한쌍은 더 생성하고 보기 좋게 각각 그룹화를 진행하겠습니다.

- CMD+N을 눌러서 Second 스토리보드를 하나 생성합니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/a.png" | relative_url }}'  alt='absolute'>

- 똑같은 방법으로 이번에는 CoCoa Touch Class를 선택해서 SecondViewController.swift를 생성합니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/b.png" | relative_url }}' alt='absolute'>

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/c.png" | relative_url }}'  alt='absolute'>

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/d.png" | relative_url }}' width=200 alt='absolute'>

요렇게 파일2개가 추가된것을 볼 수 있습니다. 
파일 구조가 파악되기 쉽게 하기 위해 그룹화를 진행하겠습니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/1.gif" | relative_url }}' width=200 alt='absolute'>

- 결과

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/e.png" | relative_url }}' width=200 alt='absolute'>

## 2. 두번째 스토리보드에 뷰컨트롤러 추가하기

- 먼저 cmd + shift + L 을 눌러서 view controller를 가져와줍니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/2.gif" | relative_url }}' alt='absolute'>


- 스토리보드와 SecondViewController.swift를 연결해줍니다.
  뷰컨트롤러 파일이 제대로 생성되어 있다면 몇글자만 쳐도 자동완성이 됩니다.
  

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/f.png" | relative_url }}' width=500 alt='absolute'>

- 스토리보드 ID도 정해줍니다.(Second라는 이름이 너무 여러곳에서 쓰이기 때문에 구분을 위해 SecondSB라고 정해줬습니다.)
  - 이 이름은 추후에 첫번째 스토리보드에서 두번재 스토리보드를 찾을때 사용됩니다.
  

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/g.png" | relative_url }}' width=500 alt='absolute'>



## 3. 2개의 스토리보드에 버튼을 만들고 배경, 버튼색 넣기

- 첫번째 스토리보드의 뷰 컨트롤러와 두번째 스토리보드의 뷰 컨트롤러 둘다 버튼을 만들어주고 배경색을 지정해주었습니다. 

- 위에서 만들었던 두번째 스토리보드부터 작업하겠습니다.(Second.storyboard)
- 버튼은 cmd + Shift + L을 눌러서 button을 가져옵니다. 

- 버튼의 내용을 바꿉니다.
  - 오타가 있습니다. Go To 1st Screen으로 해주세요
<img data-action="zoom" src='{{ "/assets/images/2020-02-14/4.gif" | relative_url }}' alt='absolute'>


- 버튼 배경색, 글자색, 글자 크기등을 설정합니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/img7.png" | relative_url }}'  alt='absolute'>

- 버튼의 가로와 세로를 정해줍니다.  왼쪽패널에서 컨트롤르 누른책 버튼을 클릭하고 다시 버튼에 내려두면 Width와 Height를 설정할 수 있습니다. (shift를 누르면 여러개를 선택할 수 있어요)
  - width는 300, height는 70으로 설정했습니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/6.gif" | relative_url }}' alt='absolute'>

- 오토레이아웃을 설정합니다. 버튼에 CTRL을 누른책 클릭하여 View에 끌어다 놓습니다.
  - Center Horizontally in Safe Area와 Center Vertically in Safe Area 를 선택합니다.
  - 역시 shift를 클릭하면 여러개를 선택할 수 있습니다.
<img data-action="zoom" src='{{ "/assets/images/2020-02-14/7.gif" | relative_url }}' alt='absolute'>

- 배경색은 뷰를 클릭하고 아래 부분에서 지정합니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/3.gif" | relative_url }}' alt='absolute'>

- Main storyboard에 있는 뷰컨트롤도 마찬가지로 꾸며줍니다.
- 결과물 (빨간배경 -> Main, 파란 배경 -> Second)

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/img1.png" | relative_url }}' height=500 alt='absolute'>
<img data-action="zoom" src='{{ "/assets/images/2020-02-14/img2.png" | relative_url }}' height=500 alt='absolute'>

## 3. 버튼을 뷰 컨트롤러의 코드와 연결하기

- 스토리보드창에서 Assistant 창을 엽니다.
  - 이때 스토리보드와 뷰 컨트롤러가 잘 연결되어 있으면 자동으로 연결된 뷰 컨트롤러가 나타납니다.
<img data-action="zoom" src='{{ "/assets/images/2020-02-14/img3.png" | relative_url }}'  alt='absolute'>

- 버튼을 컨트롤 클릭하여 viewDidLoad위쪽으로 드래그합니다.
  - 사실 이부분은 하지 않아도 기능의 이상은 없어서 굳이 해야되나 싶습니다.
  - (뇌피셜) 제 생각에 스토리보드의 어떤 UI들이 있는지 알려주는 기능이지 않을까 싶기도 합니다.
  - 아직 배우고 있는중이라 혹시 알고 계신다면 댓글 부탁드립니다. (알게되면 제가 수정해두겠습니다.)
<img data-action="zoom" src='{{ "/assets/images/2020-02-14/10.gif" | relative_url }}'  alt='absolute'>


- 버튼이 어떤기능을 할 지 코딩하기 위해 함수를 만들어줍니다.
  - 위와 달리 이부분은 꼭 수행해줘야 합니다.
  - 동일 스토리보드에서는 이 부분없이 바로 버튼을 컨트롤 클릭하여 옆에 뷰컨트롤러와 연결지었는데 지금 상황에서는 옆 스토리보드에 드래그앤 드롭을 할 수 없기에 어떤 스토리보드인지 알려줘야 하기 때문에 이 부분이 추가됐습니다.
<img data-action="zoom" src='{{ "/assets/images/2020-02-14/11.gif" | relative_url }}'  alt='absolute'>

## 4. 두 스토리보드의 화면을 modal창으로 연결하기

- 위에서 만들어놓은 함수에 아래와 같은 코드를 넣습니다.

```swift
@IBAction func goToSecondScreen(_ sender: UIButton) {
    //1
    let secondStoryboard = UIStoryboard.init(name: "Second", bundle: nil)
    //2
    guard let second = secondStoryboard.instantiateViewController(identifier: "SecondSB") as? SecondViewController else {return}
    //3
    present(nextVC, animated: true, completion: nil)
 
}
```
1. 어떤 스토리보드에서 가져올지 정합니다. `name: "Second"` 이 부분에 스토리보드 이름을 적습니다.
<img data-action="zoom" src='{{ "/assets/images/2020-02-14/img4.png" | relative_url }}' width=200 alt='absolute'>
2. 스토리보드에서 어던 뷰컨트롤러를 가져올지 정합니다. 아까 정해두었던 "SecondSB"를 적어놓습니다. 
<img data-action="zoom" src='{{ "/assets/images/2020-02-14/img5.png" | relative_url }}' width=200 alt='absolute'>
3. present로 연결된 화면을 띄웁니다. 

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/12.gif" | relative_url }}' width=300 alt='absolute'>

아주 잘 실행되지만 되돌아가지를 못합니다. 되돌아 가는 코드를 만들어줍니다.

- SecondViewController.swift에도 위와같은 방식으로 UI버튼과 버튼의 동작(함수)를 뷰컨트롤러에 넣어줍니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/img6.png" | relative_url }}' alt='absolute'>

```swift
@IBAction func goToFirstScreen(_ sender: Any) {
    dismiss(animated: true, completion: nil)
}
```
- 이곳에서 dismiss를 이용하여 modal창이 사라지는 코드를 넣어줍니다.
- 완성

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/13.gif" | relative_url }}' width=200 alt='absolute'>

## 5. modal창이 아닌 navigation controller로 화면 전환하기

- 먼저 navigation controller를 적용하기 위해서는 첫번째 뷰컨트롤러를 navigation controller에 embed in해줍니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/14.gif" | relative_url }}' alt='absolute'>

- 그리고 코드를 수정해줍니다.

```swift
//ViewController.swift
@IBAction func goToSecondScreen(_ sender: UIButton) {
        let secondStoryboard = UIStoryboard.init(name: "Second", bundle: nil)
        guard let secondVC = secondStoryboard.instantiateViewController(identifier: "SecondSB") as? SecondViewController else {return}
        self.navigationController?.pushViewController(secondVC, animated: true)
    }
```
present가 아닌 navigationController의 push를 사용하여 secondVC화면을 불러옵니다.

```swift
//SecondViewController.swift
@IBAction func goToFirstScreen(_ sender: Any) {
        self.navigationController?.popViewController(animated: true)
    }
```
위에서 쌓아놓은(push) 화면을 pop으로 업애버립니다.


[깃헙에서 수정된 코드 확인하기(클릭 후 밑의 2파일만 보면 됩니다)](https://github.com/taelee42/swift-experiments/commit/2268c1d56eeb550e0b78a8eafa8043f43aac0261)

완성

<img data-action="zoom" src='{{ "/assets/images/2020-02-14/15.gif" | relative_url }}' width=200 alt='absolute'>
