---
title: Swift | Dispatch 큐로 동기, 비동기 실현해보기
tags: [iOS, swift]
date: 2021-04-29 12:13:00 +09:00
categories: [Swift]

---

Dispatch 큐를 이용해서 동기, 비동기를 실현해보겠습니다.

<!--more-->
---



## 0. 실행할 함수들 작성



- 동기, 비동기 실행으로 인한 시간차이를 알기 위해 sleep() 함수를 사용해서 작업이 오래걸리도록 만들었습니다.
- DispatchQueue를 이용해 여러 쓰레드로 분산 시킬 준비를 하였습니다.

```swift

import Foundation

let queue = DispatchQueue.global()

func task1() {
    print("Task 1 시작")
    sleep(1)
    print("Task 1 완료★")
}

func task2() {
    print("Task 2 시작")
    print("Task 2 완료★")
}

func task3() {
    print("Task 3 시작")
    sleep(4)
    print("Task 3 완료★")
}

func task4() {
    print("Task 4 시작")
    sleep(3)
    print("Task 4 완료★")
}
```







## 1. Dispatch 큐 없이 함수들 실행해보기




```swift
let start1 = Date()

print("큐사용 X")
print("====================")
task1()
task2()
task3()
task4()
print("====================")

print(Date().timeIntervalSince(start1))
```

```swift
//결과
큐사용 X
====================
Task 1 시작
Task 1 완료★
Task 2 시작
Task 2 완료★
Task 3 시작
Task 3 완료★
Task 4 시작
Task 4 완료★
====================
8.001609921455383
```



- task 1, 2, 3, 4가 모두 동기적으로 실행되었으며 총 시간이 8초가 걸렸습니다.





## 2. Dispatch 큐를 사용하여 동기적으로 실행해보기



```swift
let start2 = Date()
print("Dispatch 큐를 사용해서 동기적으로 실행")
print("====================")

queue.sync {
    task1()
}

queue.sync {
    task2()
}

queue.sync {
    task3()
}

queue.sync {
    task4()
}

print("====================")
print(Date().timeIntervalSince(start2))
```

```swift
//결과
Dispatch 큐를 사용해서 동기적으로 실행
====================
Task 1 시작
Task 1 완료★
Task 2 시작
Task 2 완료★
Task 3 시작
Task 3 완료★
Task 4 시작
Task 4 완료★
====================
8.007328987121582
```

- 동기적으로 실행했을 때와 동일한 결과를 얻을 수 있습니다.
- 디스패치 큐에 넣어서 다른 쓰레드로 분배했지만 task가 완료될때까지 다음 작업을 쓰레드에 분배하지 않기때문에 작업이 순서대로 진행됩니다.
- 여러 쓰레드에 알아서 분배해주는 역할을 하는 디스패치에 task들을 넣었지만 내부적으로는 메인쓰레드에서 쭈욱 작업이 실행된다고 합니다.

## 2. Dispatch 큐를 사용하여 비동기적으로 실행해보기

```swift
let start3 = Date()
print("Dispatch 큐를 사용해서 비동기적으로 실행")
print("====================")


print("비동기")
queue.async {
    task1()
}

queue.async {
    task2()
}

queue.async {
    task3()
}

print("====================")
print(Date().timeIntervalSince(start3))
```
```swift
//결과
Dispatch 큐를 사용해서 비동기적으로 실행
====================
비동기
====================
0.0002989768981933594
Task 1 시작
Task 2 시작
Task 2 완료★
Task 3 시작
```

- 메인쓰레드가 종료되면 다른 쓰레드의 작업결과를 돌려받지 못하고 종료되어서 task들이 글로벌 큐에 담기기만하고 종료된다.
- 담은뒤에 그 결과를 기다리지 않고 다음 작업(다음 task를 큐에 담기)를 실행한다는 사실을 알 수 있다.
- 또한 기다리지 않기 때문에 작업시간이 매우 적게 걸리는 것도 볼 수 있다.