---
layout: post
title:  "[Java] 서블릿 이해하기"
date:   2019-10-23 22:10:14
author: Choi HyeSun
categories: java
tags:
  - Java
  - 서블릿
  - Servlet
  - Servlet API
  - Servlet API 계층구조
---

## 서블릿(Servlet)이란?

#### 배우는 이유

- 정적 웹 페이지의 문제점을 보완하여 나온 것이 동적 웹페이지를 구현하는 것

  - 초기 동적 웹 페이지를 서블릿(Servlet, 자바로 만든 CGI 프로그램)을 이용해서 구현
  
- 서블릿의 문제점을 보완하여 나온것이 JSP

  - JSP의 많은 기능은 Servlet을 따름
  
- Servlet을 먼저 이해하고 나면 JSP를 조금 더 쉽게 이해할 수 있음
  
  - 실제로 웹 애플리케이션 개발시 JSP와 서블릿이 각자의 고유한 

<br>

#### 서블릿이란?

- 서버 쪽에서 실행되면서 클라이언트의 요청에 따라 동적으로 서비스를 제공하는 자바 클래스

- 자바로 작성되어 있으므로 자바의 일반적인 특징을 모두 가짐

- 일반 자바 프로그램과 다르게 독자적으로 실행되지 못하고 톰캣과 같은 JSP/Servlet 컨테이너에서 실행되는 차이가 있음

  - JSP/Servlet 컨테이너는 톰캣 뿐만 아니라 JEUS, Web Logic, JBOSS, 기업에서 제작하는 유료 컨테이너 등이 있음
  
- 서버에서 실행되다가 웹브라우저에서 요청을 하면 해당 기능을 수행한 후 웹 브라우저에 결과를 전송

- 서버에서 실행되므로 보안과 관련된 기능도 훨씬 안전하게 수행 가능

<br>

#### 서블릿 동작 과정

1. Client → Web Server 요청

2. Web Server → Was Server 위임

3. Was Server → Servlet 호출

4. Servlet 실행 (요청에 대한 기능들 수행)

5. Servlet → Was Server 결과 반환

6. Was Server → Web Server 결과 반환

7. Web Server → 클라이언트 결과 반환

<br>

#### 서블릿의 기능

- 단순히 고정된 정보를 브라우저에 보여주는 것 → 웹 서버

- 쇼핑몰 화면에 실시간으로 변하는 상품의 할인 가격을 보여주려면 상품의 할인 가격을 데이터 베이스에서 가져오는 기능이나 직접 계산하는 기능이 필요

  - 이런 기능을 서버쪽에서 서블릿이 처리
  
  - 상품 할인 가격 표시처럼 웹 페이지에서 동적으로 변하는 정보를 효과적으로 다룰 수 있음
  
<br>

#### 그 외 서블릿 특징

- 서버 쪽에서 실행되면서 기능을 수행함

- 기존의 정적인 웹 프로그램의 문제점을 보완하여 동적인 여러가지 기능을 제공

- 스레드 방식으로 실행

- 자바로 만들어져 자바의 특징(객체지향)을 지님

- 컨테이너에서 실행

- 컨테이너 종류에 상관 없이 실행(플랫폼 독립적)

- 보안 기능 적용 용이

- 웹 브라우저에서 요청시 기능을 수행

<br>
<br>

## 서블릿 API 계층 구조와 기능

#### 서블릿의 계층 구조

- 서블릿은 자바로 만들어졌으므로 클래스들 간의 계층 구조를 가짐

- 서블릿 API 계층 구조

  - Servlet과 ServletConfig 인터페이스를 구현해 제공
  
  - GenericServlet 추상클래스가 위 두 인터페이스의 추상 메서드를 구현
  
  - HttpServlet이 GenericServlet을 상속받음
  
{% highlight bash %}
Servlet(Interface)        ServletConfig(Interface)
       │                              │
       │                              │
       └─────── GenericServlet ───────┘
               (Abstract Class)
                      ┃
                      ┃
                 HttpServlet
                   (Class)
{% endhighlight %}

<br>

#### 
