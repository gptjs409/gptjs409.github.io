---
layout: post
title:  "GitHub 블로그 만들기"
date:   2019-10-11 23:41:23
author: Choi HyeSun
categories: git
tags:
  - GitHub블로그
  - GitHubBlog
  - Blog
  - GitHub
  - Jekyll
  - Git
  - 블로그
---

 
## 1. 변경할 테마 선택

GitHub 지원 Jeykyll 테마 [Link](https://github.com/topics/jekyll-theme)
- jekyll remote theme
- 원하는 테마 선택
- 이번엔 요 [테마](http://bencentra.com/centrarium/typography/)를 적용해볼 예정
<br>
<br>

## 2. Jekyll 테마 설정

원하는 테마에서 _config.yml 복사하여 Github 최상위 경로에 넣기
- 원하는 테마 (예) https://github.com/bencentra/centrarium/edit/master/_config.yml
  - 위의 파일을 복사
  - raw를 선택하면 전체 복사가 편함
  
<br>

본인 레파지토리 (예)https://github.com/gptjs409/gptjs409.github.io
- 생성한 Github 최상위 경로에 __config.yml 파일 전체 수정

<br>

복사한 __config.yml
- remote_theme 주석 해제 (필수)

  {% highlight markdown %}
  (수정 전)
  # remote_theme           : "mmistakes/minimal-mistakes"
  
  (수정 후)
  remote_theme           : "mmistakes/minimal-mistakes"
  {% endhighlight %}
  
- url 수정 (필수)

  {% highlight markdown %}
  (수정 전)
  baseurl: "" # the subpath of your site, e.g. /blog/
  url: "" # the base hostname & protocol for your site
  
  (수정 후 - url은 본인 github명으로 입력할 것)
  baseurl: "" # the subpath of your site, e.g. /blog/
  url: "https://gptjs409.github.io/" # the base hostname & protocol for your site
  {% endhighlight %}
  
- 기타

  {% highlight markdown %}
  (수정 전)
  title: Centrarium
  subtitle: "A simple yet classy theme for your Jekyll website or blog"
  email: blcentra@gmail.com
  name: Ben Centra
  description: >
    A simple yet classy theme for your Jekyll website or blog.
  
  os_repo: "https://github.com/bencentra/centrarium.com"
  os_rcs_type: "git"
  os_src: "git@github.com:bencentra/centrarium.com.git"
  
  (수정 후)
  title: Sun's Blog
  subtitle: "HyeSun's Blog :)"
  email: gptjs409@gmail.com
  name: Choi HyeSun
  description: >
    This is Blog for Study & Work & Hobby
    
  os_repo: "https://github.com/gptjs409"
  os_rcs_type: "git"
  os_src: "git@github.com:gptjs409/gptjs409.github.io.git"
  {% endhighlight %}

<br>
<br>

## 3. posts.md도 복사
- 해당 테마 테스트 md 파일인듯
- posts.md [LINK](https://github.com/gptjs409/gptjs409.github.io/blob/master/posts.md)

<br>
<br>

## 4. index.html 삽입

index.html
- index.html 또는 index.md 파일이 Jekyll에서 정적 사이트를 생성할 때 가장 먼저 보여주는 페이지
- 원하는 테마 (예) https://github.com/mmistakes/minimal-mistakes/blob/master/index.html
  - 위의 파일을 복사
  - raw를 선택하면 전체 복사가 편함
- 본인 레파지토리 (예)https://github.com/gptjs409/gptjs409.github.io
  - 생성한 Github 최상위 경로에 index.html 파일 넣기
  - Create new file로 생성했음(어떻게 넣던 상관 없음)
  - file name : index.html
  - Edit new file : 복사한 내용 붙여넣기

<br>
<br>

## 5. 신경쓸 부분
안 쓰는 SNS 지우기 및 쓰는 건 본인 것으로 업데이트
- 안쓰는 SNS 설정은 share: false로 변경
- 쓰는 SNS 업데이트(깃헙/링크드인)

<br>
<br>

## 6. 파일 옮기기
대상
- \_includes
- \_layouts
- \_sass
- assets
- css

커버와 로고

{% highlight markdown %}
cover: "/assets/header_image.jpg"
logo: "/assets/logo.png"
{% endhighlight %}

- 커버이미지는 무료 이미지 사이트를 통해 가져옴 [LINK](https://pixabay.com/ko/photos/%EA%B3%A0%EC%96%91%EC%9D%B4-%EB%8B%AC%EC%BD%A4%ED%95%9C-%EA%B7%80%EC%97%AC%EC%9A%B4-%EC%88%99%EC%B7%A8-4436152/)
- 로고 이미지는 무료 로고 만들기 사이트를 통해 만듦, 아이디 필요 [LINK](https://www.miricanvas.com/design)

색상은 /\_sass/base/\_variables.scss에서 액션컬러 변경으로 적용(그레이) 

<br>
<br>

## 7. 하이라이트 종류 변경하기(문자열 하이라이트)

- 하이라이트 종류 찾기 [LINK](https://highlightjs.org/static/demo/)
- 하이라이트 css 찾기 [LINK](https://github.com/highlightjs/highlight.js/tree/master/src/styles)
- 단 지킬 테마 설정때문에 해당 안에 있는 설정만 가능 [Link](https://cdnjs.com/libraries/highlight.js/)
- 고른 테마

{% highlight markdown %}
(_config.yml 수정 전)
highlightjs_theme: "monokai-sublime"

(수정 후)
highlightjs_theme: "a-11-y-light"
{% endhighlight %}

- 테마때문에 하이라이트 표시할 땐 다음 블럭 안에 넣어줄 것

{% highlight javascript %}
{ % highlight 언어명(xml같은거) % }
{ % endhighlight % }
{% endhighlight %}
