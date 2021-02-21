---
title: Jekyll | Jekyll TeXt Theme에 카테고리 페이지 추가하기
tags: [Jekyll]
date: 2021-02-21 20:08:00 +09:00
category:  [Jekyll]
---

Jekyll TeXt-Theme에 카테고리 페이지 추가하는 방법을 기록했습니다.

<!--more-->
---


>🚨🚨🚨 주의   🚨🚨🚨  
>  
>이 내용은 일반적인 지킬 테마에는 적용이 안될 수 있습니다.  
제가 사용하는 TeXt 테마는 페이지를 추가한느 방법이 다른 지킬 테마와는 달라서 내용을 기록합니다.



## 1 다른 테마와는 방식이 다른 TeXt Theme

다른 테마에는 _page폴더나 _layout폴더에서 파일들을 변경하면 permalink로 외부에서 접속이 가능한데 TeXt테마는 그렇게 해보니 제대로 작동이 안됐습니다.
인터넷에 `jekyll TeXt-theme category`로 많은 검색을 해봤지만 제대로 된 내용이 나오지 않아 기록을 남깁니다.
많은 삽질끝에 2가지 방향이 있다는 것을 깨닫게 됐습니다.

1. 루트 폴더에 about.md 파일이 있는데 About메뉴에 대한 레이아웃 코드가 적혀져 있었습니다.
2. [Jekyll-TeXt-theme 깃헙](https://github.com/kitian616/jekyll-TeXt-theme)에 들어가서 fork한 사람중에 Archive, About이외의 페이지가 있는 사람을 찾고 해당 깃헙의 코드를 참고 했습니다.

제가 참고한 사람의 [깃헙(alinush/alinush.github.io)](https://github.com/alinush/alinush.github.io)과 [페이지(alinush.github.io)](https://alinush.github.io/)입니다.

> 1500개의 포크중 알파벳 a에서 단서를 찾을 수 있어서 다행이었습니다.

<br />

## 2 네비게이션 바 변경

네비게이션 바는 아래 캡처된 부분을 의미합니다.

<img style="border: 5px solid #fda6a7" data-action="zoom" src='{{ "/assets/images/2020-02-21/img1.png" | relative_url }}' alt='absolute'>


TeXt 테마는 Archive와 About페이지만 네비게이션 바에 있는데 Category항목을 추가해줄 예정입니다.

네비게이션 바는 _data/navigation.yml 파일에서 수정할 수 있습니다.
맨 위에 category.html로 가게 하는 Category항목을 추가해줍니다.
중국어, 프랑스어까지 지원할 필요는 없는 거 같아 굳이 추가하지는 않았습니다.


```yml
header:
# 추가된 부분 시작
  - titles:
      # @start locale config
  en      : &EN       Category
  en-GB   : *EN
  en-US   : *EN
  en-CA   : *EN
  en-AU   : *EN
  ko      : &KO       카테고리
  ko-KR   : *KO
      # @end locale config
    url: /category.html
# 추가된 부분 끝

  - titles:
      # @start locale config
      en      : &EN       Archive
      en-GB   : *EN
      en-US   : *EN
      en-CA   : *EN

아래 생략...
```

이제 네비게이션 바에 Category메뉴가 생기고 눌러보면 category.html로 이동이 되지만 아직 연결된 페이지가 없어서 404 페이지가 나오게 됩니다.

<br />

## 3 Category 페이지 만들기

TeXt테마는 _layout 폴더나 _page폴더를 건드리지 않게 더 간편하게 페이지를 추가할 수 있습니다.
루트 폴더의 md파일을 만들고 접속할 페이지를 만들어주면 되는데요

아래 처럼 코드를 치면 됩니다. 위의 yml설정같은 부분은 같이 루트에 있는 about.md와 거의 똑같이 만들었습니다.
여기서 key: page-category가 접속하게 될 주소와 연관되어 있는 것 같습니다. 
이 페이지를 만들고나니 주소/category.html로 접속이 가능하게 되었습니다.

그리고 아래 카테고리별로 포스트가 나오게 되는 Liquid코드는 [ Simple steps to setup Jekyll Categories and Tags](https://blog.webjeda.com/jekyll-categories/) 이 페이지를 참고했고 거기에 제가 날짜 형식과 포스트간의 행간만 수정했습니다.

```md
---
layout: article
titles:
  # @start locale config
  en      : &EN       Category
  en-GB   : *EN
  en-US   : *EN
  en-CA   : *EN
  en-AU   : *EN
  ko      : &KO       카테고리
  ko-KR   : *KO
  # @end locale config
key: page-category
---
{% raw %}
<div id="archives">
{% for category in site.categories %}
  <div class="archive-group">
    {% capture category_name %}{{ category | first }}{% endcapture %}
    <div id="#{{ category_name | slugize }}"></div>
    <p></p>

    <h3 class="category-head">{{ category_name }}</h3>
    <a name="{{ category_name | slugize }}"></a>
    {% for post in site.categories[category_name] %}
    <article class="archive-item">
      <p style="line-height:1rem">
        <a href="{{ site.baseurl }}{{ post.url }}">{{post.title}}</a>
        <span style="font-size: 1rem; font-weight:normal">{{post.date | date: "%b %d %Y"}}</span>
      </p>
    </article>
    {% endfor %}
  </div>
{% endfor %}
</div>
{% endraw %}
```


## 4 완성

아래처럼 카테고리 페이지가 아주 깔끔하게 적용된 것을 볼 수 있습니다.
<img style="border: 5px solid #fda6a7" data-action="zoom" src='{{ "/assets/images/2020-02-21/img2.png" | relative_url }}'  alt='absolute'>