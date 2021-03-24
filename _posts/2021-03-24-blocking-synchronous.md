---
title: OS | 블로킹 == 동기, 논블로킹 == 비동기 인가요?
tags: [OS, Computer Science]
date: 2021-03-24 04:11:00 +09:00
categories: [OS]

---

Blocking vs Non-Blocking, Synchronous vs Asynchronous에 대해 각각 비교하고
Blocking과 Synchronous는 완전히 같인지 아닌지를 이야기해보겠습니다.


<!--more-->
---


>운영체제를 공부하면서 제가 조사해보고 적은 내용입니다.
>제 생각이 중간중간 들어가 있을 수 있기 때문에 틀린부분이 있다면 댓글로 알려주세요



## Blocking vs Non-Blocking

> 요약: 한 프로세스가 CPU의 제어권을 뺏기면 **Blocking**, 안 뺏기면 **Non-Blocking**


프로세스(=실행중인 프로그램)는 할당받은 CPU를 통해서 작업을 수행할 수 있습니다. CPU를 할당받지 않으면 아무런 연산을 할 수 없죠. 

CPU가 하나라고 가정하고 그 CPU를 여러 프로세스가 돌아가면서 쓴다고 하겠습니다. (이를 멀티 프로그래밍이라고 합니다. 여러 CPU를 사용하는 멀티 프로세싱과는 다릅니다.)

CPU를 돌아가면서 쓰기 때문에 CPU를 정해진 룰에 따라 잘 나눠쓰는 문제가 생깁니다. 특히 프로세스가 CPU를 쓰다가 언제 다음 프로세스에게 넘겨줄지가 상당히 중요합니다.

여러가지 룰이 있겠지만, 프로세스가 CPU를 갖고 있는데 사용하지 않는 일은 발생하면 안되겠죠.

그런 일이 있을까 싶지만, 충분히 있을 수 있습니다.



프로세스의 작업에는 연산만이 있지 않습니다. 컴퓨터의 파일을 읽어와야 할 수도 있고 인터넷으로부터 정보를 받아와야 작업을 할 수도 있죠.

이런 작업을 입출력 작업(I/O, Input/Output)이라고 하며 이 작업은 CPU가 하는 연산에 비해 매우 매우 느립니다. 

(컴퓨터가 작업중임에도 기다려야 할 때를 생각해보면 보통 파일을 옮기거나, 다운받고 있을 때가 많죠)

CPU가 열심히 연산을 하다가 파일을 하나 읽어와서 작업을 해야 하는데 그 파일이 없으면 다음 작업을 할 수 없습니다.

읽어온 파일을 바탕으로 CPU가 작업을 할 수 있는데 시간이 오래걸리는 파일로딩 작업을 마냥 기다릴 수 없습니다. CPU를 기다리는 프로세스들이 많거든요

이 때가 프로세스로부터 CPU를 뺏어가기 딱 좋은 타이밍이죠. 이렇게 CPU를 빼앗겨 아무런 일을 할 수 없는 프로세스를 Blocked 상태라고 합니다.

이 Blocked 상태의 프로세스는 외부작업이 끝났다는 신호를 받고 나서야 입출력작업의 결과와 함께 CPU의 할당을 기다리는 줄에 가서 대기할 수 있습니다.



이제 Blocking과 Non-Blocking을 설명할 차례가 왔습니다.

**Blocking**이란 프로세스가 외부작업(I/O작업 등)을 할 때 CPU의 제어권이 뺏길 수 있음을 의미합니다.

만약 프로세스가 I/O작업을 하러 가서 CPU를 사용하지 않음에도 CPU를 반환하지 않으면 **Non-Blocking**이라고 하죠



즉 **Blocking / Non-Blocking**은 어떤 **하나의 프로세스**(혹은 쓰레드)가 외부작업을 할 때 **CPU의 제어권을 내어주게 설계되어 있는지 그렇지 않은지를 의미**합니다.



Blocking/Non-Blocking은 프로세스 **하나**의 관점에서 바라보는 것이 밑의 Synchronous/Asynchronous와의 큰 차이점입니다.





## Synchronous vs Asynchronous

>요약: 한 프로세스에 다른 프로세스에게 요청을 보내고 기다리면 **Synchronous**, 요청의 응답은 안기다리고 다음 코드를 수행하면 **Asynchronous**

Synchronous(동기)는 위의 Blocking/Non-Blocking과 달리 2개의 프로세스(혹은 쓰레드)의 관점에서 바라봅니다.

한국말로 동기화라고 불리는 Synchronous는 두 작업의 시작과 끝이 일치해야합니다.

다시말해 두개의 프로세스(A,B)에서 한쪽(A)이 요청을 보냈을 때 다른쪽(B)이 응답을 준비할 동안 A가 다음 작업을 하지 않는 것을 의미합니다.



예를 들어, 

마트에서 계산할 때 바코드를 찍는 작업과 돈을 지불하는 작업은 동기입니다. 바코드를 다 찍지 않으면 지불할 수 없습니다.

밥을 먹는 작업과 설거지는 동기입니다. 식사를 마칠 때까지 설거지를 하면 안됩니다.


이에 반해 Asynchronous(비동기)는 프로세스 A가 프로세스 B에게 요청을 보내놓고 응답이 오는것과 관계없이 프로세스A의 할 일을 진행합니다.

예를 들어, 마트에서 계산을 요청하고 요청하는 사이 물을 하나 가지러 빨리 갔다오는 행위는 비동기입니다.

계산을 맡겨놓고 기다리지 않고 바로 물을 가지러 갔기 때문이죠

뭔가 위에서 배운 Blocking과 Synchronous가 비슷하고 Non-Blocking과 Asynchronous가 비슷합니다.
Blocking과 Synchronous 모두 다른 작업이 끝나기 전까지 기다립니다.
정확히 말하면 Blocking 관점에서는 CPU의 제어권이 뺏겨서 강제로 기다리게 되고 Synchronous 관점에서는 다른 프로세스를 자발적으로 기다립니다.

이제 둘의 공통점을 알았으니 차이점을 알아보겠습니다.

## Blocking == Synchronous, Non-Blocking == Asynchronous?

대개의 경우 Blocking과 Synchronous는 같은 의미입니다. Non-Blocking과 Asynchronous도 같은 의미로 쓰이고요. 하지만 둘의 의미가 다른 경우도 있긴합니다. 

### Non-Blocking이면서 Synchronous한 상황
예를들어 Non-Blocking인데 Synchronous한 상황을 가져와보겠습니다.
위에 설명대로라면 CPU의 제어권을 뺏기지 않으면서(Non-Blocking) 다른 프로세스의 응답을 기다려야(Synchronous)합니다.
예를들어 프로세스 A가 프로세스B에게 작업을 하나 요청합니다. 프로세스B는 요청받은 작업을 시작하게 되고 프로세스 A입장에서 다음 코드로 넘어가지 않고 프로세스 B를 기다리면 Synchronous가 성립하게 됩니다.
이제 프로세스 A는 기다리는 입장이기때문에 CPU를 뺏기는게 합당합니다. (기다리고 있는 프로세스들이 많거든요)
하지만 프로세스 A는 CPU를 뺏기지 않기 위해 일을 합니다. 프로세스 B의 요청이 계속 확인하는 반복문을 만들어놓습니다. 
그러면 프로세스 B는 CPU를 뺏기지 않으면서(Non-Blocking) 다음 작업으로 넘어가지는 않는(Synchronous)한 상황이 만들어지게 됩니다.

다시말해, CPU의 제어권은 갖고 있으면서(Non-Blocking) 외부 작업을 기다리는 (Synchronous)상태인거죠
```
//프로세스 A
...
프로세스 B에게 요청
while(not 프로세스 B작업 완료) {
  대기
}
프로세스의 A의 다음 작업 실행
...

```
>참고: 운영체제의 동기화 문제에 등장하는 Mutext, Semaphore에서 Busy Waiting(바쁜 대기)가 Non-Blocking이면서 Synchronous한 상태입니다.

그러면 Blocking이면서 Asynchronous한 상황은 없는지 궁금하실텐데요
이런 상황은 의도적으로 만들진 않는다고 합니다.
생각해보면 CPU의 제어권은 빼앗긴 상태인데 다음 코드로 넘어갈수는 있게 코드를 짠건데요.
CPU의 제어권이 없는 상태에서 다음 코드로 넘어갈 수가 없는거죠


## 결론

- 한 프로세스가 CPU의 제어권을 빼앗기는지 아닌지로 Blocking, Non-Blocking을 판단
- 프로세스가 다른 프로세스에게 보낸 요청을 기다리는지 안기다리는지로 Synchronous, Asynchronous판단
- 보통의 경우 Blocking == Synchronous, Non-Blocking == Asynchronous
- 하지만 Non-Blocking하면서 Synchronous한 상황도 나올 수 있다. 









### 참고

운영체제와 정보기술의 원리(반효경)
https://stackoverflow.com/questions/2625493/asynchronous-vs-non-blocking
https://velog.io/@codemcd/Sync-VS-Async-Blocking-VS-Non-Blocking-sak6d01fhx


