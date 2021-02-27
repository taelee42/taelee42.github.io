---
title: Swift | Delegation Pattern
tags: [iOS, swift, Design Pattern]
date: 2021-02-27 12:11:00 +09:00

---

Closure가 무엇이고 어떻게 사용하는지 알아보도록 하겠습니다.

<!--more-->
---



## Delegation Pattern 이란?

<figure>
<img data-action="zoom" src='{{ "/assets/images/2021-02-27/1delegation pattern 그림.png" | relative_url }}' alt='absolute'>
<figcaption>
</figcaption>
</figure>

- Delegation Pattern은 한 객체가 다른 객체에게 data나 동작을 위임(delegate)하는 패턴입니다.
- Delegation Pattern은 **총 3파트**로 이루어져 있습니다.
1. **Delegating Object**: 위임을 필요로 하는 객체입니다. 데이터나 동작을 다른 객체에게 맡길 객체입니다.
2. **Delegate Protocol**: delegate가 구현해야 하는 메서드가 정의되어 프로토콜입니다.
3. **Delegate**: 위의 프로토콜에 따라 어떤 정보를 주거나 어떻게 동작할지 구체적으로 개발자가 구현하는 객체입니다.

---

예를 들어 **컬렉션 뷰**를 만들 때 컬렉션 뷰는 **뷰 컨트롤러**에게 **data source**와 **delegate**을 위임(delegate)합니다.

이때 컬렉션 뷰에서 필요한 정보나 수행할 동작을 직접 구현하는 것이 아니라 뷰 컨트롤러에게 위임(delegate)합니다.  여기서 **컬렉션뷰**가 delegate 필요로 하는 **Delegating Object**입니다.

**뷰 컨트롤러**는 컬렉션뷰가 필요로하는 정보(셀과 섹션은 몇개 만들건지, 사이즈는 어떻게 할건지 등등)와 수행할 동작(셀이 선택되었을 때 등등)을 구체적으로 코드에 구현합니다.  여기서 뷰 컨트롤러가 컬렉션뷰의 정보와 동작에 대한 코드를 대신 구현해주는 **Delegate**입니다.

이때 뷰컨트롤러는 **UICollectionViewDataSource**, **UICollectionViewDelegate**, **UICollectionViewDelegateFlowLayout**등등의 프로토콜을 준수하면서 구현하게 됩니다. 여기서 나열된 것들이 구현해야될 메서드들이 명시되어 있는 **Delegate Protocol**입니다.

프로토콜에 어떤 메서드들을 구현해야하는지 궁금하다면 제 다른 글을 참조하셔도 좋습니다.  
[iOS | Collection View의 Protocol들(DataSource, Delegate, DelegateFlowLayout)](https://taelee42.github.io/ios/2021/02/21/collectionView.html)