---
title: Git | 다른 브랜치에 잘못 커밋했을 때 해당 커밋 가져오기 (Cherry-Pick)
tags: [Git, cherry-pick]
date: 2021-02-23 16:24:00 +09:00
categories: [Git]

---

Repository를 clone받고 실수로 자신의 브랜치로 바꾸지 않고 작업 후 커밋했을 때 해당 커밋을 가져오는 방법을 알아보겠습니다!

<!--more-->
---


<span style="color:red; font-size: 2rem" >UICollectionViewDataSource</span>


현재 다른 리포지토리를 fork하고 열심히 작업하고 커밋한 뒤에 push가 안된다는 사실을 알았습니다.  
다른 브랜치에 작업하면서 만들어놓은 커밋 2개를 제 브랜치를 만들어서 옮겨보겠습니다.


## 1 현재 작업중이 내용 저장하기

먼저 현재 작업중이 내용을 저장합니다.

모든 내용이 커밋되어 있지 않으면 cherry-pick이 되지 않기에 현재 커밋되지 않은 내용이 있다면 커밋을 하거나 `git stash`로 임시저장합니다.
저는 `git stash`하겠습니다.

<img data-action="zoom" src='{{ "/assets/images/2020-02-23/img1.png" | relative_url }}' width=500 alt='absolute'>

위 내용은 `git stash list`로 어떤 내용들이 임시저장되어 있는지 확인할 수 있습니다.

<br/>


## 2 가져올 커밋ID를 적어놓습니다.

추후에 cherry-pick할 때 가져올 커밋들의 커밋ID가 필요합니다.
`git log`를 통해 가져올 커밋 아이디를 적어놓습니다.
(git log 목록은 위로 갈수록 최신입니다.)

<img data-action="zoom" src='{{ "/assets/images/2020-02-23/img2.png" | relative_url }}' width=500 alt='absolute'>

제 경우 2개의 커밋을 잘못 쌓았기 때문에 두 커밋 아이디를 적어놓겠습니다.
이때 순서가 중요합니다. (이따가 Cherry Pick 할때 과거부터 현재로 하나씩 해주기 때문에 저 또한 과거부터 현재순으로 적어두었습니다.)

`d16b068050682c7ab2ac481842be72db32a145ad`

`1b6114b6276504c30004d39d548ecfc3bf9855e5`

<br/>


## 3 과거 커밋으로 돌아가서 브랜치 만들기

- `git checkout HEAD~2` : 잘못된 커밋을 만들기 전인 HEAD에서 2개 전 커밋으로 돌아가기  
  - (잘못되서 현재 커밋으로 돌아오고 싶다면 `git checkout [브랜치 이름]`을 하면 됩니다.)

<br/>

- `git checkout -b [새로운 브랜치 이름]` : 새로운 브랜치 만들면서 해당 브랜치로 이동하기

  - 위 과정은 아래처럼 쪼갤 수 있습니다.
    - `git brach [새로운 브랜치 이름]` : 새로운 브랜치 만들기
    - `git checkout [새로운 브랜치 이름]` : 해당 브랜치로 이동하기

<br/>

> git checkout은 위처럼 2가지 용도로 사용될 수 있습니다.  
1. 원하는 커밋으로 이동하기  
2. 원하는 브랜치로 이동하기




<br/>


## 4 새로운 브랜치로 커밋 가져오기

이제 아까 적어놓은 커밋들을 하나씩 가져오면 됩니다. 과거커밋부터 하나씩 가져오겠습니다.

- `git cherry-pick d16b068050682c7ab2ac481842be72db32a145ad`
- `git cherry-pick 1b6114b6276504c30004d39d548ecfc3bf9855e5`

<img data-action="zoom" src='{{ "/assets/images/2020-02-23/img3.png" | relative_url }}' width=500 alt='absolute'>

혹시 cherry-pick을 하다가 문제가 생긴경우 체리픽 하기 직전으로 되돌리고 싶으면
`git reset --hard HEAD`를 해주면 됩니다. 제일 최근 커밋(HEAD)이후로 모든 수정사항을 없애는 명령어 입니다.

`--hard`: 모든 수정사항 없애기

`--mixed(default)`:  add취소, 수정사항은 살려둠

`--soft`:  add 안취소, 당연히 수정사항도 살아있음

## 5 잠시 저장해둔 수정사항 적용하기

아까 잠시 stash에 저장해둔 수정사항을 적용합니다.

`git stash apply`

## 5 다른 브랜치의 잘못 만들었던 커밋들 지우기

>  🚨🚨🚨 혹시 협업하는 repository이면서 원격 저장소(github)에 push를 했다면 이 방법을 쓰면 안됩니다.  
협업자와 충돌이 날 수 있기 때문에 이때는 revert로 되돌려서 지운기록을 남겨야합니다.

> 혼자 작업하는 저장소의 경우 원격 저장소에 push했더라도 `git push -f origin master`처럼 -f 옵션으로 강제 푸쉬할 수 있습니다.

원래 브랜치로 돌아갑니다. 
저는 원래 브랜치 이름이 editions/3.0인데 이걸 보시는 분들은 이름이 다를거에요. 보통 master인 경우가 많겠지만 기억이 안나는 경우 
`git branch`를 사용해서 브랜치 목록을 보면 됩니다.

저는 2개를 잘못 쌓았기 때문에 2개만 지우면 됩니다. 이 숫자를 잘 조정해서 지우면 됩니다.
`git reset --hard HEAD~2`

- reset 전

<img data-action="zoom" src='{{ "/assets/images/2020-02-23/img4.png" | relative_url }}' width=500 alt='absolute'>

- reset 후

<img data-action="zoom" src='{{ "/assets/images/2020-02-23/img2.png" | relative_url }}' width=500 alt='absolute'>

---
틀린 정보가 있다면 댓글로 알려주세요!

