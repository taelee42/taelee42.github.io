---
title: Swift | Optional 추출 방법
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

방법2부터는 방법1과 달리 옵셔널의 값이 nil이여도 에러를 만들지 않습니다.
옵셔널의 값에 접근하기 전에 nil인지 확인하는 절차가 있고 nil인 경우와 nil이 아닌 경우를 나눠서 처리합니다.

### 옵셔널의 값이 nil이 아닌 경우
```swift
var name:String?
name = "IronMan"

if let newName = name {
    print(newName)
} else {
    print("Failed to Unwrapping")
}

// 결과 
// "IronMan" 출력
```
옵셔널 타입인 name을 newName으로 값을 접근해서 추출합니다. 
이때 nil이 아니면 바로 밑에 `print(newName)`로 넘어가고 
nil인 경우는 else 구문으로 넘어갑니다.

이때 옵셔널 값의 접근한 변수 newName의 타입은 String으로 옵셔널이 아니게 됩니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-15/img6.png" | relative_url }}' width=500 alt='absolute'>


### 옵셔널의 값이 nil인경우

위에서 설명한대로 nil인 경우에는 else문으로 들어가서 동작합니다.

```swift
var name:String?
name = nil

if let newName = name {
    print(newName)
} else {
    print("Failed to Unwrapping")
}
// 결과
// "Failed to Unwrapping" 출력
```


## 방법3) Optional binding2 (guard let)

방법2와 상당히 유사하지만 조금 다른 두번째 Optional binding입니다.
`if let`이 아닌 `guard let`을 사용하는데요

if let과의 차이점은 else구문만 있고 nil이 아닌 경우에 처리하는 부분이 없습니다.

옵셔널의 값을 추출하기만 하고 그이외에 다른 동작이 필요하지 않을 때 if let보다 좀더 간결하게 쓸 수 있는 문법입니다.

```swift
var name:String?
name = "IronMan"

func printAndParse(name: String?) {
    guard let newName = name else {
        print("Failed to Unwrapping")
        return
    }
    print(newName)
}

printAndParse(name: name)

// 결과
// "IronMan" 출력
```

if let과 다르게 guard let 은 return과 함께 사용되어야 해서 함수안에 넣어주었습니다.

## 방법4) Nil Coalescing

마지막 방법입니다.
Nil Coalescing은 닐이 아닌경우에는 옵셔널의 값을 추출하고 nil인 경우에는 미리 정해둔 **기본값**으로 대체해줍니다.

```swift
var name:String?
name = "Hulk"

let newName = name ?? "noName"

print(newName)
```

위의 코드에서 `let newName = name ?? "noName"` 이부분에서 Nil Coalescing이 일어나는데요
name이 nil이 아니면 옵셔널의 값인 "Hulk"가 newName으로 잘 전달되는데
만약 name이 nil이라면 옆에 지정해둔 "noName"이 newName의 값으로 전달됩니다.


## 더 알아볼 사항

optional 추출방법에 Optional Chaining이라는 방법도 있는데 좀더 조사하고 다른 포스트에서 다뤄보도록 하겠습니다.
[OptionalChaining 공식문서](https://docs.swift.org/swift-book/LanguageGuide/OptionalChaining.html)

---

잘못된 정보가 있다면 댓글로 알려주세요!
