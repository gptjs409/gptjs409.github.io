---
title:  "GitHub 블로그 검색되도록 설정하기"
permalink: /git/blog/20191010210102
last_modified_at: 2019-10-10T21:01:02+09:00
tags:
  - GitHub블로그
  - GitHubBlog
  - Blog
  - GitHub
  - Jekyll
  - Git
  - 블로그
  - GitHub검색설정
  - 검색유입
  - Google검색
  - Naver검색
  - Daum

---

## 1. sitemap 생성하기(구글 등록 준비)

sitemap이란?
- 사이트맵은 사이트에 있는 페이지, 동영상 및 기타 파일과 각 관계에 관한 정보를 제공하는 파일
- Google같은 검색엔진에서는 해당 파일을 읽고 사이트를 더 지능적으로 크롤링할 수 있음
- 크롤러에게 내가 사이트에서 중요하다고 생각하는 파일을 알리고, 해당 파일에 관한 중요한 정보를 제공함

<br>

sitemap.xml
- GitHub Pages 즉, 깃헙 블로그에서는 Plug-in(플러그인)을 사용할 수 없음
- 최상위 디렉토리에 sitemap.xml을 직접 생성해줄 것
- 다음과 같이 셋팅

```html
{% raw %}
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    <url>
      <loc>{{ site.url }}{{ post.url }}</loc>
      {% if post.lastmod == null %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
      {% endif %}
      {% if post.sitemap.changefreq == null %}
        <changefreq>weekly</changefreq>
      {% else %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
      {% endif %}
      {% if post.sitemap.priority == null %}
          <priority>0.5</priority>
      {% else %}
        <priority>{{ post.sitemap.priority }}</priority>
      {% endif %}
    </url>
  {% endfor %}
</urlset>
{% endraw %}

```

- 셋팅 후 블로그주소/sitemap.xml 접속해보면 잘 나옴을 확인할 수 있음 [LINK](https://gptjs409.github.io/sitemap.xml)

``` xml
This XML file does not appear to have any style information associated with it. The document tree is shown below.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd">
  <url>
    <loc>https://gptjs409.github.io/git/blog</loc>
    <lastmod>2019-10-10T10:06:34+00:00</lastmod>
  </url>
  <url>
    <loc>https://gptjs409.github.io/</loc>
  </url>
</urlset>
```

<br>
<br>

# 2. Robots.txt 생성하기(크롤링 가능하게끔 설정)
Robots.txt란?
- 검색엔진 로봇(혹은 크롤러)들이 자료를 수집할 수 있게끔 설정해두는 정책
- 어떤 정보들을 로봇이 수집해가야할지 미리 정의해 놓는 지침
- 사이트맵이 어떤 것들이 있는지 알려준다면, 로봇츠는 수집해야 하는 것들이 무엇인지 알려주는 느낌

<br>
Robots.txt 등록하기(사이트맵 필요!)
- 최상위 디렉토리에 robots.xml을 직접 생성해줄 것
- 다음과 같이 셋팅

``` xml
User-agent: *
Allow: /

Sitemap: https://gptjs409.github.io/sitemap.xml
```
- 등록하면 다음과 같이 나옴 [LINK](https://gptjs409.github.io/robots.txt)

``` html
User-agent: *
Allow: /

Sitemap: https://gptjs409.github.io/sitemap.xml
```

<br>
<br>

## 3. 구글에서 검색되도록 등록하기
GOOGLE SEARCH CONSOLE [LINK](https://www.google.com/webmasters/#?modal_active=none)
- 구글에서 검색되도록 등록하려면 써치 콘솔에 등록해야 함(구글 계정 필요)
- 일단 주소를 Search Console에 등록해보기(소유권 확인까지)
  1. SEARCH CONSOLE [→]
  2. Google 검색 실적 개선하기 [시작하기]
  3. (로그인 안했으면 로그인)
  4. (URL접두어) URL : (블로그URL) https://gptjs409.github.io
  5. HTML 등록하라고 뜨는데, 다운받아서 GitHub Repo 최상위루트에 넣어두기[예제LINK](https://github.com/gptjs409/gptjs409.github.io/blob/master/google5f4ed065a298fe81.html)
  6. 그리고 등록하고 30초정도 대기했다가 등록완료하면 소유권이 확인됨!이 뜸
  7. 속성으로 이동
- 사이트맵 적용해보기
  1. 우측의 메뉴 네비게이션 상단 색인 > Sitemaps의 사이트맵스 선택
  2. 새 사이트맵 추가에 sitemap.xml 입력 후 제출
  3. 제출된 사이트맵으로 (바로 아래) 이동되며 상태가 성공!이라고 뜨면 됨
- Robots.xml 제출하기
  1. 로그인된 상태에서
  2. 해당 링크 접속[LINK](https://www.google.com/webmasters/tools/robots-testing-tool)
  3. 주소가 잘 입력되어 있으면 4번으로, 주소가 다른 기존 등록 주소라면 우측 상단 새로운 Search Console 사용
  4. robots.txt가 잘 나와있는지 확인하고 [제출]
  5. 1번 2번은 희망자만 선택하고, 다 되고나서 3번의 [제출] 끝
- 통계는 반영되고 조금 지나고부터 확인되므로 일단 PASS

<br>
<br>

## 4. RSS Feed 생성하기(네이버/다음 등록 준비)
RSS란?
- Really Simple Syndication 또는 Rich Site Summary의 약자
- 뉴스나 블로그 사이트에서 주로 사용하는 콘텐츠 표현 방식
- 웹 관리자는 RSS 형식으로 웹 사이트 내용을 보여줌
- 넷스케이프를 통해 등장

<br>

RSS피드란?
- 정기적으로 업데이트되는 웹 콘텐츠를 전달해주는 형태
- 글의 전체 또는 요약 정보 및 작성자 등의 정보가 포함

<br>

RSS 피드 등록하기
- GitHub Pages는 Plugin 지원이 안되므로 feed.xml 파일을 직접 만들어야 함
- 최상위 루트 아래 /feed.xml을 생성하면 끝!

``` xml
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.title | xml_escape }}</title>
    <description>{{ site.description | xml_escape }}</description>
    <link>{{ site.url }}{{ site.baseurl }}/</link>
    <atom:link href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" rel="self" type="application/rss+xml"/>
    <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
    <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
    <generator>Jekyll v{{ jekyll.version }}</generator>
    {% for post in site.posts limit:30 %}
      <item>
        <title>{{ post.title | xml_escape }}</title>
        <description>{{ post.content | xml_escape }}</description>
        <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
        <link>{{ post.url | prepend: site.baseurl | prepend: site.url }}</link>
        <guid isPermaLink="true">{{ post.url | prepend: site.baseurl | prepend: site.url }}</guid>
        {% for tag in post.tags %}
        <category>{{ tag | xml_escape }}</category>
        {% endfor %}
        {% for cat in post.categories %}
        <category>{{ cat | xml_escape }}</category>
        {% endfor %}
      </item>
    {% endfor %}
  </channel>
</rss>
---
``` 

<br>
<br>

## 5. 네이버/다음에서 검색되도록 등록하기
네이버(NAVER) : 로그인 필요
- 검색등록 [LINK](https://webmastertool.naver.com/)
  - 로그인
  - 사이트 간단 체크(ID당 하루 최대 10번) : (블로그URL) https://gptjs409.github.io [조회]
  - 자동등록 보안절차(글자숫자입력) [확인]
  - Robots가 없다는데... 나중에 확인할 것
  - 맨 아래 조회한 사이트 소유확인하기 클릭
- 소유확인
  - HTML 파일 업로드 선택
  - HTML 파일 다운로드
  - GITHUB 최상단에 업로드
  - HTML 반영 확인(참고 생각보다 꽤 반영 시간 걸림)
  - 완료
- 웹마스터도구 > 등록한 도메인 선택
  - 요청 > RSS 제출 : Feed URL 입력 후 제출
  - 요청 > 사이트맵 제출 : sitemap.xml 입력 후 제출
  - 웹 페이지 수집 : 확인, 및 등록하고 싶은 것 등록(robots.xml도 한 번 해줌)
- robots 등록안되었다던거
  - 검증 > robots.txt 들어가면 잘 적용 된 것으로 확인됨
  - 검증 > 웹 페이지 최적화에서 재검증 끝!
  
<br>

다음(DAUM) : 로그인 불필요
- 검색등록 [LINK](https://register.search.daum.net/searchForm.daum?act=insert)
  - 검색등록 선택 : 블로그 등록
  - 블로그 URL : (본인 URL) gptjs409.github.io [확인]
  - 개인정보 수집 동의, 개인정보 취급 위탁 동의 [확인]
  - 이메일 입력 (앗! 다음이 아니어도 상관 없음) 입력 후 [확인]
  - 완료
- 완료 메시지

``` text
gptjs409@(메일) 님의 블로그 등록신청이 완료되었습니다.

블로그 URL	http://gptjs409.github.io

입력하신 블로그는 정상적으로 등록신청 되었습니다.
해당 블로그의 글은 심사를 거친 후 블로그 검색에 노출 될 것입니다.
등록 이후 검색 노출은 최대 5일 정도 소요되며 처리 결과는 별도로 알려드리지 않습니다.
블로그 내용이 블로그 검색 기준에 맞지 않다면 거부 될 수 있습니다.
```
- 처리 결과는 아마도 이메일로 오는 듯 함

<br>
<br>


## 삽질ING
깃헙 블로그 쉽다고 누가 그랬는데..
익숙해질 날이 빨리 다가왔으면!
<br>
``` html
{% raw %}{%{% endraw %} raw {% raw %}%}{% endraw %}
{% raw %}{%{% endraw %} endraw {% raw %}%}{% endraw %}
```이거 추가해야 제대로 { } HTML 파일이 보인다는데..
