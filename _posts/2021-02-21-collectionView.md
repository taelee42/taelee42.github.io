---
title: iOS | Collection View의 Protocol들(DataSource, Delegate, DelegateFlowLayout)
tags: [iOS, collection view, protocol]
date: 2021-02-21 05:37:24 +09:00

---

컬렉션 뷰를 생성하고 여러 프로토콜을 통해 컬렉션 뷰의 동작이나 모양을 바꿀 수 있습니다.
각 프로토콜별로 어떤 메서드들이 있는지 알아보겠습니다.

<!--more-->
---


## 1. 프로토콜 요약
>제가 다뤘던 주요 프로토콜은 다음과 같습니다.  
추후에 중요하다고 생각되는 프로토콜이나 메서드를 다루면 추가하도록 하겠습니다.


- UICollectionViewDataSource  
셀이 담을 내용과 관련되어 있습니다. (몇개의 셀을 보여줄지, 섹션을 몇개 보여줄지)

- UICollectionViewDelegate  
셀의 동작관 관련되어 있습니다.(셀 선택, Context Menu, Action Menu등이 실행되었을 때 어떻게 동잘할지)

- UICollectionViewDelegateFlowLayout:  
셀의 외관과 관련되어 있습니다. (셀 혹은 섹션의 크기나 여백등을 어떻게 표시할지)

위의 프로토콜 중 꼭 구현해야하는 메서드를 갖고 있는 프로토콜은 UICollectionViewDataSource의 2개뿐입니다.
그 이외에는 메서드는 필요에 따라 모두 선택적으로 구현하면 됩니다.

## 2. 필수 메서드

<span style="color:#FF6868; font-size: 1.5rem" >DataSource 프로토콜</span>


```swift
//한 섹션에 몇개의 컬렉션 셀을 보여줄까?
func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int

//셀을 어떻게 보여줄까?(dequeueReusableCellWithReuseIdentifier와 함께 사용)
func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell
```

## 3. 선택 메서드

<span style="color:#FF6868; font-size: 1.5rem" >DataSource 프로토콜</span>

#### 사용해본 것

```swift
//섹션의 갯수를 몇개로 할까? (기본값 1개)
func numberOfSections(in collectionView: UICollectionView) -> Int

// The view that is returned must be retrieved from a call to -dequeueReusableSupplementaryViewOfKind:withReuseIdentifier:forIndexPath:
// 헤더나 푸터에 사용되는 supplementaryElement를 어떻게 보여줄까?
func collectionView(_ collectionView: UICollectionView, viewForSupplementaryElementOfKind kind: String, at indexPath: IndexPath) -> UICollectionReusableView

```

#### 그외 사용해보지 않은 것
> 사용해보지 않아 Quick Help에 있는 내용만 보고 작성했습니다. \
공부용으로 작성된 내용이라 내용이 틀릴 수도 있습니다. \
잘못된 정보가 있다면 댓글로 알려주세요!

```swift
//해당 셀이 이동될 수 있을까?
func collectionView(_ collectionView: UICollectionView, canMoveItemAt indexPath: IndexPath) -> Bool

//해당 셀을 새로운 위치로 이동
func collectionView(_ collectionView: UICollectionView, moveItemAt sourceIndexPath: IndexPath, to destinationIndexPath: IndexPath)

//컬렉션뷰에 모든 셀들의 타이틀 추출 
func indexTitles(for collectionView: UICollectionView) -> [String]?

//위에서 알게된 타이틀 이름으로 해당 셀이 몇번째 셀인지 물어보기
func collectionView(_ collectionView: UICollectionView, indexPathForIndexTitle title: String, at index: Int) -> IndexPath
```

  
<br/>

<span style="color:#FF6868; font-size: 1.5rem" >Delegate 프로토콜</span>

#### 사용해본 것
```swift
//셀이 select되었을때 어떻게 할까?
func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath)
```


#### 그외 사용해보지 않은 것

너무 많아서 (32개) 어떤 기능과 관련되어 있는지 요약해서 적겠습니다.

- 셀이 hightlight되거나 해제됐을 때 알려주기
- 셀이 select되거나 해제됐을 때 알려주기
- 셀이 보여지거나 사라졌을 때 알려주기
- SpplementaryView(헤더, 푸터)가 보여지거나 사라졌을 때 알려주기
- ActionMenu, Context Menu 관련
- Focus 관련
- Custom Transition 관련
- 셀이 적절한 곳으로 잘 이동되었는지
- 셀이 수정가능한지
- 두 손가락으로 동시의 여러셀을 선택할 수 있는지

> hightlight, select 모두 셀에 터치를 했을 때의 상태변화입니다.  
hightlight: 터치가 시작되자마자  
select: 터치가 끝난 후

<br/>

<span style="color:#FF6868; font-size: 1.5rem" >DelegateFlowLayout 프로토콜</span>


#### 사용해본 것
```swift
// cell의 사이즈를 어떻게 할까?
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize
```

#### 그외 사용해보지 않은 것

```swift
//섹션의 inset(margin)을 어떻게 할까?
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, insetForSectionAt section: Int) -> UIEdgeInsets

//특정 섹션의 row나 column들간의 spacing을 어떻게 할까?
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumLineSpacingForSectionAt section: Int) -> CGFloat

//특정 섹션의 item들간의 spacing을 어떻게 할까?
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, minimumInteritemSpacingForSectionAt section: Int) -> CGFloat

//특정 섹션의 Header View의 사이즈를 어떻게 할까?
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, referenceSizeForHeaderInSection section: Int) -> CGSize

//특정 섹션의 Footer View의 사이즈를 어떻게 할까?
func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout: UICollectionViewLayout, referenceSizeForFooterInSection section: Int) -> CGSize
```