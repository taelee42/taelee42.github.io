---
title: iOS | 스토리보드 이름 바꾸기, 시작 스토리보드 바꾸기
tags: [iOS, swift]
date: 2021-03-10 16:50:00 +09:00
categories: [iOS]

---

스토리보드의 이름을 바꾸거나 여러 스토리보드중 앱 시작화면의 스토리보드를 변경해야 할 때 수정해야하는 부분을 포스팅합니다.

<!--more-->
---

## 앱의 시작 스토리보드 바꾸기

스토리보드의 이름을 바꿨을 때 앱이 제대로 실행되지 않고 아래와 같은 에러 메세지가 나타납니다.
```plain
Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: 'Could not find a storyboard named 'Hi' in bundle NSBundle
```
이 때 앱의 설정파일인 Info.plist에 앱이 어떤 스토리보드를 첫 스토리보드로 인식할지 알려줘야 하는데요.   
이를 위해서 아래의 표시된 2곳의 이름을 새로운 스토리보드의 이름으로 바꿔줍니다.  ㅁㅁ.stoyboard의 ㅁㅁ부분의 이름을 입력해주면 됩니다.

<img data-action="zoom" src='{{ "/assets/images/2021-03-10-change-initial-storyboard/1plist 바꿔야 하는거 네모치기.png" | relative_url }}'  alt='absolute'>

단순히 스토리보드의 이름만 바꾸는 경우는 여기까지만 하면 앱이 잘 실행됩니다.

만약 에러는 안나지만 앱에 검은화면만 나오는 경우는 아래 항목을 참고하시면 됩니다.



<span style="color:red; font-size: 1.5rem" >👇👇👇👇 에러는 안나지만 앱에 검은화면만 나오는 경우👇👇👇👇</span>
## 스토리보드의 시작 뷰컨트롤러 설정하기
새로운 스토리보드를 만들어서 시작 스토리보드를 바꾼 경우 스토리보드의 어떤 뷰컨트롤러를 처음으로 보여줄지 정해줘야 합니다.
처음으로 보여줄 뷰컨트롤러를 클릭후  Attributes Inspector에서 `Is Initial View Controller`항목을 체크해주면 됩니다.

<img data-action="zoom" src='{{ "/assets/images/2021-03-10-change-initial-storyboard/2스토리보드 이니셜설정 뷰 컨트롤러 설정.png" | relative_url }}' width=250 alt='absolute'>

이제 정상적으로 새로운 스토리보드에서 앱이 잘 실행됩니다.

