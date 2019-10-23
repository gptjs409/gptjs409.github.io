---
layout: post
title:  "[Java] 웹 애플리케이션 이해하기"
date:   2019-10-23 19:54:42
author: Choi HyeSun
categories: java
tags:
  - Java
  - 웹 애플리케이션
  - 웹 애플리케이션 정의
  - 웹 애플리케이션 기본 구조
---

## 웹 애플리케이션

#### 웹 애플리케이션의 정의

- 정적인 웹 애플리케이션 + 동적인 서비스

- 웹 컨테이너에서 실행되는 JSP, 서블릿, 자바 클래스들을 사용해 정적 웹 프로그래밍 방식의 단점을 보완하여 서비스를 제공하는 서버 프로그램

  - 정적 웹 애플리케이션의 기능인 HTML(HTML5), 자바스크립트, CSS 등도 그대로 사용할 수 있음
  
<br>
<br>

## 웹 애플리케이션 기본 구조

{% highlight bash %}

Web Application Name
        │
        └─────── WEB-INF
                   │
                   ├─────── classes
                   │
                   ├─────── lib 
                   │
                   └───────web.xml
{% endhighlight %}

- 웹 컨테이너(톰캣 등)에서 실행하는 웹 애플리케이션의 기본 디렉터리 구조
