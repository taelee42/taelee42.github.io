---
title: iOS | FileManager로 Swift에서 파일 읽고 쓰기
tags: [Swift, FileManager]
date: 2021-02-10 02:30:00
categories: [iOS]
---

> 👉 swift에서 파일을 읽고 쓰는 등의 작업을 하기 위한 방법을 알아봅니다.
> fileManager의 사용법을 기록했습니다.

### 키워드
- `FileManager`
- `urls`
- `appendingPathComponent`
- `NSString`
- `write`
- `fileExists`
- `createDirectory`
- `NSString`
- `removeItem`
<!--more-->




## 🦊 1 FileManager 불러오기

FileManager : 파일을 다루기 위한 클래스, 인스턴스화해서 사용

```swift
import Foundation
//import UIKit

let fileManager = FileManager.default
```
- FileManager를 사용하려면 Foundation이나 UIKit import하기
  - 정확히는 Foundation을 불러오면 되는데 UIKit을 불러오면 Foundation도 같이 불러온다고 합니다.(공식문서에는 못찾았습니다.)
  - [FileManager 공식문서](https://developer.apple.com/documentation/foundation/filemanager)
  - [UIKit 공식문서](https://developer.apple.com/documentation/uikit)
  - [Foundation 공식문서](https://developer.apple.com/documentation/foundation#//apple_ref/doc/uid/20001091)

- FileManager를 사용하기 위해서 인스턴스를 불러옵니다.
  - 싱글톤으로 불러오기 위해서 `default`를 사용합니다.
    - 다른곳에서 싱글톤을 불러올 때 `클래스(스트럭트).shared`로 봤었는데 어떻게 정의하냐에 따라 불러오는게 다른것 같습니다.(🤔뇌피셜)

- 여기서 불러온 fileManager는 아래에서 계속 사용됩니다.

## 🦊 2 경로 지정하기

```swift
let documentURL = fileManager.urls(for: .documentDirectory, in: .userDomainMask)[0]
```

- `fileManager.urls`를 사용하여 경로를 지정합니다.
  - 여기서 지정된 경로로 파일을 저장하거나 불러오거나 삭제할 수 있습니다.
  - 배열을 리턴하기 때문에 `[0]`으로 첫번째 원소만 가져옵니다.
    - (❓궁금)원소가 하나만 있는데 왜 배열인지는 모르겠습니다. 아마 다르게 사용하면 경로를 여러개 리턴해주는 것 같습니다.
- `urls` 인자 소개(아직도 확실치 않습니다만 실험결과를 알려드리겠습니다)
  - [urls(for:in:)](https://developer.apple.com/documentation/foundation/filemanager/1407726-urls)
    - 공식문서를 아무리 봐도 이해가 안돼서 직접 실험을 해봤습니다.

- 첫번째 인자 `FileManager.SearchPathDirectory` 조사
  - enum으로 이루어져 있고 [공식문서](https://developer.apple.com/documentation/foundation/filemanager/searchpathdirectory)를 가면 어떤 값들이 있는지 볼 수 있습니다.

```swift
print(fileManager.urls(for: .documentDirectory, in: .userDomainMask)[0])
print(fileManager.urls(for: .applicationDirectory, in: .userDomainMask)[0])
print(fileManager.urls(for: .demoApplicationDirectory, in: .userDomainMask)[0])
print(fileManager.urls(for: .developerApplicationDirectory, in: .userDomainMask)[0])
print(fileManager.urls(for: .libraryDirectory, in: .userDomainMask)[0])
print(fileManager.urls(for: .documentationDirectory, in: .userDomainMask)[0])
```

```swift
//안보이는 경우 오른쪽 끝까지 스크롤해주면 됩니다
file:///Users/taelee/Library/Developer/XCPGDevices/6E55D3B6-C54B-4644-829D-A1A70667CD26/data/Containers/Data/Application/0D32CCF2-0C3D-4DB0-851F-2748B49B4651/Documents/
file:///Users/taelee/Library/Developer/XCPGDevices/6E55D3B6-C54B-4644-829D-A1A70667CD26/data/Containers/Data/Application/0D32CCF2-0C3D-4DB0-851F-2748B49B4651/Applications/
file:///Users/taelee/Library/Developer/XCPGDevices/6E55D3B6-C54B-4644-829D-A1A70667CD26/data/Containers/Data/Application/0D32CCF2-0C3D-4DB0-851F-2748B49B4651/Applications/Demos/
file:///Users/taelee/Library/Developer/XCPGDevices/6E55D3B6-C54B-4644-829D-A1A70667CD26/data/Containers/Data/Application/0D32CCF2-0C3D-4DB0-851F-2748B49B4651/Developer/Applications/
file:///Users/taelee/Library/Developer/XCPGDevices/6E55D3B6-C54B-4644-829D-A1A70667CD26/data/Containers/Data/Application/0D32CCF2-0C3D-4DB0-851F-2748B49B4651/Library/
file:///Users/taelee/Library/Developer/XCPGDevices/6E55D3B6-C54B-4644-829D-A1A70667CD26/data/Containers/Data/Application/0D32CCF2-0C3D-4DB0-851F-2748B49B4651/Library/Documentation/
```

뒤에 폴더명만 조금씩 달라지는 것을 확인할 수 있습니다. `directory`로 어떤 폴더를 열어볼지 정하는 것 같습니다.


- 두번째 인자 `domainMask` 조사
  - 이것또한 공식문서에 가면 어떤 값들을 넣을 수 있는지 볼 수 있습니다. [공식문서](https://developer.apple.com/documentation/foundation/filemanager/searchpathdomainmask)

```swift
print(fileManager.urls(for: .applicationDirectory, in: .userDomainMask)[0])
//file:///Users/taelee/Library/Developer/XCPGDevices/6E55D3B6-C54B-4644-829D-A1A70667CD26/data/Containers/Data/Application/0D32CCF2-0C3D-4DB0-851F-2748B49B4651/Applications/

print(fileManager.urls(for: .applicationDirectory, in: .localDomainMask)[0])
//file:///Applications/

print(fileManager.urls(for: .applicationDirectory, in: .networkDomainMask)[0])
//file:///Network/Applications/

print(fileManager.urls(for: .applicationDirectory, in: .systemDomainMask)[0])
//file:///Applications/
```

- for: .documentDirectory로 하니깐 에러가 많이나서 applicationDirectory로 고정하고 in부분을 바꿔봤습니다.
- (🤔뇌피셜) 결과로 추측해봤을 때 도메인이 directory를 찾을 기계(?)를 말하는게 아닐까 싶습니다.(공식 문서에도 비슷한 설명이 있는거 같은데 잘 구분이 안되네요)
  - userDomainMaks: 이 앱이 구동되는 기계
  - localDomainMaks: 현재 이앱을 만들고 있는 컴퓨터
  ...등등

- 몇몇 코드를 봤을때 documentDirectory, userDomainMask로 사용하는걸로 보아 일단 저도 그렇게 사용해보겠습니다.
  - 진행하다 문제가 생기거나 추가사항이 있으면 수정하겠습니다.




## 🦊 3-1 파일 만들기

```swift
// 상세경로 추가(.../Documents -> .../Documents/저장파일.txt)
let fileURL = documentURL.appendingPathComponent("저장파일.txt")

let textString = NSString(string: "저장할 내용입니다")
try? textString.write(to: fileURL, atomically: true, encoding: String.Encoding.utf8.rawValue)
```

- `documentURL.appendingPathComponent`로 상세경로를 지정해줍니다.
  - 경로가 파일을 나타내는 경우 확장자명까지 지정해줍니다.
- NSString(): 파일에 저장할 문자열을 지정합니다. (일반 String으로 하면 에러가 납니다)
- `write` 설명
  - to: 파일을 저장할 경로
  - atomically: true이면 백업파일을 먼저 만들고 에러가 없으면 해당경로의 이름으로 변경,
    false이면 해당 경로 파일에 바로 기록합니다.
  - encoding: 인코딩 종류, 특별한 이유가 없는 경우 utf8을 사용하면 될듯합니다.

- [write 공식문서](https://developer.apple.com/documentation/foundation/nsdata/1408033-write)

## 🦊 3-2 폴더 만들기

```swift
//생성할 폴더 경로 지정(위에서 지정한 documentURL에 추가)
let dirURL = documentURL.appendingPathComponent("나는폴더다")

print(dirURL)  //이대로는 struct여서 출력해보면 아래처럼 나옴
//%E1%84%82%E1%85%A1%E1%84%82%E1%85%B3%E1%86%AB%E1%84%91%E1%85%A9%E1%86%AF%E1%84%83%E1%85%A5%E1%84%83%E1%85%A1

print(dirURL.path) //path로 접근하면 경로가 출력
// /Users/taelee/Library/Developer/XCPGDevices/6E55D3B6-C54B-4644-829D-A1A70667CD26/data/Containers/Data/Application/0D32CCF2-0C3D-4DB0-851F-2748B49B4651/Documents/나는폴더다

if !fileManager.fileExists(atPath: dirURL.path) {
    do {
        try fileManager.createDirectory(atPath: dirURL.path, withIntermediateDirectories: true, attributes: nil)
    } catch {
        NSLog("폴더 생성 실패")
    }
}
```

- `fileExists`로 폴더가 있는지 검사(없으면 생성)
- `createDirectory`로 폴더 생성
  - [createDirectory 공식문서](https://developer.apple.com/documentation/foundation/filemanager/1407884-createdirectory)
  - atPath: 생성할 경로
  - withIntermediateDirectories: true면 중간경로도 같이 생성, false면 중간경로가 하나라도 없으면 fail한다고 하는데 아마 에러를 던지는 듯?
  - attributes: owner, group numbers, file permissions, modification date등의 정보를 세팅할 수 있음. nil로 하면 기본값으로 생성됨

## 🦊 3-3 파일 불러오기
```swift
//위에서 fileURL에 저장한 코드 사용
let fileURL = documentURL.appendingPathComponent("저장파일.txt")
print(fileURL)

let textString = NSString(string: "저장할 내용입니다")
try? textString.write(to: fileURL, atomically: true, encoding: String.Encoding.utf8.rawValue)

//여기서부터 파일 불러오는 코드
do {
    let text = try String(contentsOf: fileURL, encoding: .utf8)
    print(text)
} catch let error {
    print(error.localizedDescription)
}
```


## 🦊 3-4 파일 삭제

```swift
do {
    try fileManager.removeItem(at: fileURL)
} catch let error {
    print(error.localizedDescription)
}
```


## 🦊 4 복습

폴더 생성-> 파일 쓰기-> 파일 읽기 -> 읽은 파일 수정 -> 파일 다시 쓰기 -> 파일 읽기 -> 파일 삭제

```swift
import Foundation
let fileManager = FileManager.default

let documentURL = fileManager.urls(for: .documentDirectory, in: .userDomainMask)[0]

//폴더 생성
let dirURL = documentURL.appendingPathComponent("나는폴더다")
if !fileManager.fileExists(atPath: dirURL.path) {
    do {
        try fileManager.createDirectory(atPath: dirURL.path, withIntermediateDirectories: true, attributes: nil)
    } catch {
        NSLog("폴더 생성 실패")
    }
}

//파일 생성
let fileURL = dirURL.appendingPathComponent("저장파일.txt")
let textString = NSString(string: "피카츄, 이 문장은 처음으로 저장된 문장이야")
try? textString.write(to: fileURL, atomically: true, encoding: String.Encoding.utf8.rawValue)

//파일 읽고, 내용 수정해서 다시 저장(같은 경로)
do {
    let text = try String(contentsOf: fileURL, encoding: .utf8)
    print("--> 첫번째 읽은 문장")
    print(text)
    let newText = NSString(string:text + "\n" + "새로운 문장을 추가했어요")
    try? newText.write(to: fileURL, atomically: true, encoding: String.Encoding.utf8.rawValue)
} catch let error {
    print(error.localizedDescription)
}

//파일 다시 읽기
do {
    let text = try String(contentsOf: fileURL, encoding: .utf8)
    print("--> 두번째 읽은 문장")
    print(text)
} catch let error {
    print(error.localizedDescription)
}

//파일 삭제
do {
    try fileManager.removeItem(at: fileURL)
} catch let error {
    print(error.localizedDescription)
}

```



### 참고 문서
[iOS ) FileManager를 이용해 파일/폴더 만드는 법](https://zeddios.tistory.com/440)

[(Swift) FileManager](https://jiseobkim.github.io/swift/2019/07/11/swift-File-Manager.html)

### 나중에 참고할 문서
[로컬 컴퓨터에 fileManager로 접근하기](https://hcn1519.github.io/articles/2017-07/swift_file_manager)
