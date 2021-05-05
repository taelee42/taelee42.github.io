---
title: Swift | View Controller의 생명주기(Life Cycle)
tags: [iOS, swift]
date: 2021-05-05 13:16:00 +09:00
categories: [iOS]

---

View Controller에서 자동으로 수행되는 viewDidLoad, ViewWillAppear등에 대해 살펴보겠습니다.

<!--more-->
---

## 0. 전체 메소드
먼저 뷰 컨트롤러의 생명주기와 관계된 method들을 살펴보겠습니다.

![](https://images.velog.io/images/taelee/post/3bb0c5fb-2e0e-4744-8a7a-7a5fae115cff/image.png)

함수명들이 직관적으로 짜여져있는데요.
did의 경우는 완료되었음을 의미하고 will을 실행되기 직전을 의미합니다.
예를들어 viewWillAppear는 뷰가 화면에 나타나기 직전을 의미하죠

## 1. viewDidLoad()
>Called after the controller's view is loaded into memory. (Apple docs)
컨트롤러의 뷰가 메모리에 로드된 후에 호출된다.

view가 메모리에 로드되는 시점에 처음 한번만 실행됨

## 2. viewWillAppear(_:)
>Notifies the view controller that its view is about to be added to a view hierarchy. (Apple docs)
뷰가 화면(뷰계층)에 추가되기 직전에 호출된다.

### viewDidLoad와 viewWillAppear의 차이

뷰가 하나일 때는 둘의 역할이 비슷하지만 뷰가 여러개일 때는 확연한 차이를 보입니다.
예를들어 뷰가 처음 보여졌을 때는 viewDidLoad와 ViewWillAppear가 둘다 호출되지만 다른 화면을 갔다가 돌아왔을때는 viewWillAppear만 호출된다.
뷰가 이미 메모리에 로드외어 있기 때문에 viewDidLoad는 실행되지 않습니다.

## 3. viewDidAppear(_:)
>Notifies the view controller that its view was added to a view hierarchy. (Apple docs)
뷰가 나타난 뒤에 호출된다.

## 4. viewWillDisappear(_:)
>Notifies the view controller that its view is about to be removed from a view hierarchy. (Apple docs)
뷰가 사라지기 직전에 호출된다.

## 5. viewDidDisappear(_:)
>Notifies the view controller that its view was removed from a view hierarchy. (Apple docs)
뷰가 사리지고 난 뒤에 호출된다.


## 6. 실제 구동결과 살펴보기

뷰를 2개 만들어서 각각의 뷰 컨트롤러마다 위의 메소드 들을 전부 작성했습니다.

### 1. 첫번째 화면이 보일 때
![](https://images.velog.io/images/taelee/post/4e1c5846-1bb7-46ef-ae1e-7fa82dd99ff9/image.png)

### 2. 두번째 화면으로 넘어갔을 때
![](https://images.velog.io/images/taelee/post/043b6eaf-3810-4c68-bcd7-1693be808ca4/image.png)

### 3. 첫번째 화면으로 돌아왔을 때
![](https://images.velog.io/images/taelee/post/de9a7bf0-ad59-4ed5-9376-9b9cf30b9feb/image.png)

- 위에서 언급한 것처럼 viewDidLoad는 뷰가 처음 생성될 때 한번만 호출됩니다.

- 다른 화면으로 이동시   
1. 이전화면 viewWillDisappear  
2. 현재화면 viewWillAppear  
3. 이전화면 viewDidDisappear  
4. 현재화면 viewDidAppear  
이 순서로 함수들이 호출됩니다.

## 참고
- [iOS ) View Controller의 생명주기(Life-Cycle)[ZeddiOS]](https://zeddios.tistory.com/43)