---
title:  "GitHub 블로그 만들기"
permalink: /git/blog/20191011234123
last_modified_at: 2019-10-11T23:41:23+09:00
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
  ```yml
  (수정 전)
  # remote_theme           : "mmistakes/minimal-mistakes"
  
  (수정 후)
  remote_theme           : "mmistakes/minimal-mistakes"
  ```
- url 수정 (필수)
  ```yml
  (수정 전)
  baseurl: "" # the subpath of your site, e.g. /blog/
  url: "" # the base hostname & protocol for your site
  
  (수정 후 - url은 본인 github명으로 입력할 것)
  baseurl: "" # the subpath of your site, e.g. /blog/
  url: "https://gptjs409.github.io/" # the base hostname & protocol for your site
  ```
- 기타
  ```yml
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
  ```

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

커버와 로고
```
cover: "/assets/header_image.jpg"
logo: "/assets/logo.png"
```
