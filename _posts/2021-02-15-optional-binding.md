---
title: swift | Optional 추출 방법(작성중)
tags: [swift, optional]
date: 2021-02-15 23:05:00 +09:00


---

Force Unwarpping, Optional Binding, Nil Coalescing과 같은 Optional의 추출 방법들에 대해 알아보겠습니다.



<!--more-->
---

# Optional 추출방법
[이전 포스트](https://taelee42.github.io/2021/02/15/optional.html)에서 옵셔널에 대해서 알아보았습니다.

간단 요약하면 **옵셔널**은 nil이 될 수도 있는 자료형입니다.
옵셔널 타입의 변수는 값에 바로 접근할 수 없고 접근하기 위해서는 아래와 같은 방법들이 사용됩니다.

## 방법1) Forced Unwrapping

첫번째 방법은 이름처럼 강제로 옵셔널 상자를 까보기입니다.
옵셔널에 nil이 아닌 값이 담겨있을때는 아래처럼 정상적으로 출력됩니다.

```swift
// Forced unwrapping
var name:String?
name = "pikachu"
print(name!)  //pikachu 출력
```

하지만 옵셔널에 nil이 담겨있는 경우 강제로 추출하게 되면 아래와 같은 에러메세지를 보여줍니다.

```swift
var name:String?
name = nil
print(name!)
```
<img data-action="zoom" src='{{ "/assets/images/2020-02-15/img5.png" | relative_url }}' alt='absolute'>

Forced Unwrapping의 경우 옵셔널에 nil이 담겨져 있지 않다는 확신이 있는 경우에만 사용해줘야 한다고 합니다.


## 방법2) Optional binding1 (if let)
## 방법3) Optional binding2 (guard let)
## 방법4) Nil Coalescing


# 옵셔널

예제1)
옵셔널에는 nil을 담을 수 있다. 
옵셔널이 아니면 nil을 담을 수 없음

let num = Int("10")

Optional의 디폴트 값은 nil로 name은 nil을 갖게 됩니다.


```swift
// 고급 기능 4가지

// Forced unwrapping
// Optional binding (if let)
// Optional binding (guard)
// Nil coalescing

// Forced unwrapping > 억지로 박스를 까보기
// Optional binding (if let) > 부드럽게 박스를 까보자 1
// Optional binding (guard) > 부드럽게 박스를 까보자 2
// Nil coalescing > 박스를 까봤더니, 값이 없으면 디폴트 값을 줘보자
```







# 후기

https://docs.swift.org/swift-book/LanguageGuide/OptionalChaining.html

