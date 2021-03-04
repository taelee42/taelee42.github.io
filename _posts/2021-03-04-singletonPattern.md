---
title: 디자인 패턴 | Singleton Pattern(싱글톤 패턴)
tags: [iOS, swift, Design Pattern]
date: 2021-03-04 10:11:00 +09:00
categories: [Design Pattern]
---

iOS개발시 자주 사용하게 되는 싱글톤 패턴에 대해 알아보겠습니다.

<!--more-->
---


## 싱글톤을 언제 쓰나요?

스위프트에서 **싱글톤 패턴**은 2가지 이유로 사용됩니다.

1. 클래스의 **인스턴스를 하나**만 만들고 싶을 때 
2. 전역 변수처럼 인스턴스를 **어디서든 접근**하고 싶을 때


전역 변수처럼 어디서든 접근하고 싶은데 인스턴스로 2개 이상을 만들고 싶을 때는 **싱글톤 플러스 패턴**을 사용한다고 합니다.

## 사용 코드

```swift
//클래스 선언 시
public class MySingleton {
  //1
  static let shared = MySingleton()
  //2
  private init() { }
}

// 클래스 사용할 때
let mySingleton = MySingleton.shared
//let mySingleton2 = MySingleton() <-- 컴파일 에러
```


1. 클래스 내에 shared라는 변수를 만들어서 인스턴스화해서 접근하는 것이 아닌 클래스에 직접 접근합니다.
2. init 생성자를 private으로 제한시켜 클래스의 인스턴스화를 막습니다. 마지막줄처럼 인스턴스화를 시도하면 컴파일 에러가 뜹니다.

## 인스턴스를 또 만들어도 같은 클래스 맞나요?

```swift

public class MySingleton {
  static let shared = MySingleton()
    var name = "Hulk"
  private init() { }
}
let mySingleton = MySingleton.shared
mySingleton.name = "Iron Man"
let mySingleton2 = MySingleton.shared
print(mySingleton.name) //Iron Man
```

이렇게 코드를 쳐보면 mySingleton2가 두번째에 새로 생겼음에도 불구하고 name이 처음 초기화된 "Hulk"가 아니라 "Iron Man"임을 알 수 있다.
즉 mySingleton2는 새로 생긴 인스턴스가 아니라 mySingleton과 같은 클래스임을 알 수 있습니다.