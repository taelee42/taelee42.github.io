---
title: OS | Stack영역과 Heap 영역
tags: [OS, stack, heap]
date: 2021-03-04 08:11:00 +09:00
categories: [OS]
---

메모리의 Stack 영역과 Heap 영역의 차이점을 알아보겠습니다.

<!--more-->
---




## 메모리의 영역(Segment)들

개발자가 만든 프로그램이 실행되기 위해서는 메모리(RAM)위에 올라가야 합니다.  
이때 코드들이 각각의 역할에 따라 메모리에 올라가는 영역이 나뉩니다.


전체적인 코드가 CPU가 이해할 수 있는 기계어로 번역되어 **코드영역**에  
전역 변수와 static 변수는 **데이터 영역**에  
전역 변수와 static 변수중 초기화가 안되어 있거나 0으로 초기화 되어 있으면 **BSS영역**에  
마지막으로 그이외 나머지 변수들이 저장되는 **Heap영역**과 **Stack영역**이 있습니다.

Heap 영역과 Stack 영역은 둘다 변수를 저장하는 메모리 영역이라는 공통점 이외에는 많은 차이점을 갖고 있습니다. 그 차이점에 대해 알아보겠습니다.

## Stack 영역이란?

프로그램에서 **함수에 의해 생성되었다가 사라지는 변수**들을 저장하는 메모리 영역입니다.
해당 함수가 일을 다 마치고 반환하게 되면 할당되었던 변수의 메모리 영역이 자동적으로 해제됩니다.

메서드, 지역변수, 참조 변수(클래스 등의 주소를 갖고 있는 변수)이 여기에 속합니다.
메모리 영역이 OS에 의해 자동으로 효율적으로 관리됩니다.



## Heap 영역이란?

사용자에 의해 메모리가 할당되어서 선언된 변수를 말합니다.
고급언어에서는 사용자가 직접 할당한 변수 이외에도 여러가지 변수들이 자동으로 Heap 영역에 저장하고 또 관리됩니다.
프로그램이 실행되고 있는 런타임에도 Heap 영역에 저장된 공간이 유동적으로 변할 수 있는 동적 메모리 할당을 지원합니다. 
>배열을 늘렸다가 줄였다가 하는 것을 상상하면 됩니다. 고급 언어를 쓰시는 분들은 배열의 크기변경이 당연하다고 생각할 수 있겠지만, C에서는 배열을 늘렸다가 줄이려면 malloc이라는 명령어로 선언하지 않으면 배열의 크기조절이 불가능합니다.

메모리가 자동으로 관리되지 않기때문에 사용자 혹은 언어가 지원하는 기능으로 관리해줘야 합니다. 제대로 관리되지 않는다면 메모리누수가 발생할 수 있습니다.


## Stack영역 과 Heap영역의 차이점



|                  | Stack                                                      | Heap                                                        |
| ---------------- | ---------------------------------------------------------- | ----------------------------------------------------------- |
| 데이터 타입      | Stack 자료구조(선형 타입)                                  | Heap 자료구조(비선형 계층적 타입, tree에 기반)              |
| Access 속도      | 매우 빠름                                                  | Stack에 비해 느림                                           |
| 메모리 공간 관리 | OS에 의해 아주 효율적으로 관리되고<br />조각이 생기지 않음 | 효율적으로 관리되지 않음.<br />메모리의 조각이 생길 수 있음 |
| 접근범위         | 변수가 선언된 지역 한정                                    | 글로벌하게 어디서든 접근가능                                |
| 사이즈 제한      | OS에 따라 지정된 사이즈가 있음                             | 무제한                                                      |
| 사이즈 변경      | 불가능                                                     | 가능                                                        |
| 메모리 할당      | stack영역에 연속적으로 메모리가 할당됨                     | 프로그래머에 의해 순서가 바뀔 수 있음                       |
| 생성과 소멸      | 컴파일러에 의해 자동적으로 실행                            | 프로그래머가 수동적으로 실행                                |
| 소멸             | 따로 소멸을 관리할 필요가 없음                             | 프로그래머가 꼭 소멸해줘야함                                |
| Cost             | 작음                                                       | 큼                                                          |
| 메인 이슈        | 메모리 부족                                                | 메모리 조각문제                                             |


## 마치며

스위프트를 공부하는데 클래스는 Heap영역에 그리고 클래스의 주소를 갖고 있는 참조변수는 Stack 영역에 저장된다는 말을 보고 예전에 공부했던 Stack과 Heap영역을 다시 한번 정리해봤습니다.
<figure>
<img data-action="zoom" src='{{ "/assets/images/2021-03-04-stackVSheap/2stackAndHeapExample.jpeg" | relative_url }}' width=400 alt='absolute'>
<figcaption>
클래스의 인스턴스는 Heap에 그리고 그 주소를 갖고 있는 참조변수는 Stack에 저장된다
</figcaption>
</figure>

#### 참고

[Stack vs Heap: Know the Difference(Guru99)](https://www.guru99.com/stack-vs-heap.html)
[What and where are the stack and heap?(stack overflow)](https://stackoverflow.com/questions/79923/what-and-where-are-the-stack-and-heap)

---

## 더 알아보기

#### 코드(텍스트) 영역, 데이터 영역, BSS영역, 힙 영역, 스택 영역
[https://en.wikipedia.org/wiki/Data_segment](https://en.wikipedia.org/wiki/Data_segment)

[https://www.geeksforgeeks.org/memory-layout-of-c-program/](https://www.geeksforgeeks.org/memory-layout-of-c-program/)

[https://stackoverflow.com/questions/7718299/whats-an-object-file-in-c#:~:text=An%20Object%20file%20is%20the%20compiled%20file%20itself.&text=An%20executable%20file%20is%20formed,is%20also%20called%20machine%20code.](https://stackoverflow.com/questions/7718299/whats-an-object-file-in-c#:~:text=An%20Object%20file%20is%20the%20compiled%20file%20itself.&text=An%20executable%20file%20is%20formed,is%20also%20called%20machine%20code.)


