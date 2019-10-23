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
       └── GenericServlet ──┘
               (Abstract Class)
                      ┃
                      ┃
                 HttpServlet
                   (Class)
{% endhighlight %}

<br>

#### 서블릿 API 기능

- 서블릿 API를 구현하는 여러 구성 요소들의 특징

|**서블릿 구성 요소**|기능|
|:-:|---|
|**Servlet 인터페이스**|- javax.servlet 패키지에 선언<br>- Servlet 관련 추상 메서드를 선언<br>- init(), service(), getServletInfo(), getServletConfig()를 선언|
|**ServletConfig 인터페이스**|- javax.servlet 패키지에 선언<br>- Servlet 기능 관련 추상 메서드가 선언<br>- getInitParameter(), getInitParameterNames(), getServletContext(), getServletName()이 선언|
|**GenericServlet (추상)클래스**|- javax.servlet 패키지에 선언<br>- 상위 두 인터페이스를 구현하여 일반적인 서블릿 기능을 구현한 클래스<br>- GenericServlet을 상속받아 구현한 사용자 서블릿은 프로토콜에 따라 각각 Service()를 오버라이딩해서 구현|
|**HttpServlet 클래스**|- javax.servlet.http 패키지에 선언<br>- GenericServlet을 상속받아 HTTP 프로토콜을 사용하는 웹 브라우저에서 서블릿 기능을 수행<br>- 웹 브라우저 기반 서비스를 제공하는 서블릿을 만들 때 상속받아 사용<br>- 요청시 service()가 호출되면서 요청 방식에 따라 doGet()이나 doPost()가 차례대로 호출됨|

<br>

- GenericServlet

  - 일반적인 여러 통신 프로토콜에 대한 클라이언트/서버 프로그램에서 서블릿 기능을 구현할 수 있는 클래스
  
- HttpServlet

  - GenericServlet을 상속
  
  - HTTP 프로토콜을 사용하는 서블릿 기능을 구현하는 클래스
  
  - HttpServlet을 상속받아 HTTP 프로토콜로 동작하는 웹 브라우저의 요청을 처리하는 서블릿을 만들어 볼 것
  
  - 그 외 다른 서블릿 구성요소들의 기능은 API 문서를 참고
  
  - 주요 메서드와 기능
  
|**메서드**|기능|
|--:|---|
|**protected doDelete(HttpServletRequest req,<br>HttpServletResponse resp)**|서블릿이 DELETE request를 수행하기 위해 service()를 통해서 호출|
|**protected doGet(HttpServletRequest req,<br>HttpServletResponse resp)**|서블릿이 GET request를 수행하기 위해 service()를 통해서 호출|
|**protected doHead(HttpServletRequest req,<br>HttpServletResponse resp)**|서블릿이 HEAD request를 수행하기 위해 service()를 통해서 호출|
|**protected doPost(HttpServletRequest req,<br>HttpServletResponse resp)**|서블릿이 POST request를 수행하기 위해 service()를 통해서 호출|
|**protected service(HttpServletRequest req,<br>HttpServletResponse resp)**|표준 HTTP request를 public service()에서 전달받아 doXXX() 메서드를 호출|
|**public service(HttpServletRequest req, <br>HttpServletRequest resp)**|클라이언트의 request를 protected service()에게 전달|

<br>

- 클라이언트 요청시 public service() 메서드를 먼저 호출 후 다시 protected service() 메서드를 호출, 그 다음 request에 따라 doXXX() 메서드를 호출

<br>
<br>

## 서블릿의 생명주기 메서드

- 서블릿도 자바 클래스이므로 실행하면 당연히 초기화 과정 메모리에 인스턴스를 생성하여 서비스를 수행 다시 소멸하는 과정을 거침

  - 이런 단계를 거칠 때 마다 서블릿 클래스의 메서드가 호출되어 초기화, 데이터베이스 연동, 마무리 작업을 수행
  
  - 각 과정에서 호출되어 기능을 수행하는 메서드 : 서블릿 새생명주기 메서드
  
- 서블릿 생명주기(Life Cycle) 메서드

  - 서블릿 실행마다 호출되어 기능을 수행하는 콜백 메서드
  
- 서블릿의 생명주기 메서드 기능과 특징

  - init() 실행 초기에 서블릿 기능 수행과 관련된 기능을 설정하는 용도로 많이 사용, 생략 가능
  - destroy() 서블릿이 메모리에서 소멸될 때 여러가지 종료 작업 수행, 생략 가능
  - doGet(), doPost() do로 시작하는 메서드는 서블릿의 핵심 기능을 처리하므로 반드시 구현

|생명주기 단계|**호출 메서드**|특징|
|---|---|---|
|초기화|**init()**|- 서블릿 요청시 맨 처음 한 번만 호출<br>- 서블릿 생성시 초기화 작업을 주로 수행|
|작업 수행|**doGet()<br>doPost()**|- 서블릿 요청시 매번 호출<br>- 실제로 클라이언트가 요청하는 작업을 수행|
|종료|**destroy()**|- 서블릿이 기능을 수행하고 메모리에서 소멸될 때 호출<br>- 서블릿의 마무리 작업을 주로 수행|


<br>
<br>

## FristSerlvet을 이용한 실습 - 사용자 정의 서블릿을 만들어보기

#### 이클립스에서 서블릿을 만들고 실행하는 과정

1. 사용자 정의 서블릿 클래스 만들기

2. 서블릿 생명주기 메서드 구현

3. 서블릿 매핑 작업

4. 웹 브라우저에서 서블릿 매핑명으로 요청하기

<br>

#### 사용자 정의 서블릿 만들기

- 웹 프로그래밍에서 사용되는 사용자 정의 서블릿

  - HttpServlet 클래스를 상속받아서 만듦
  
  - 3개의 생명주기 메서드를 오버라이딩해서 기능을 구현(init(), doGet, destroy())
  
- 형식

{% highlight java %}
public class FirstServlet extends HttpServlet {
    @Override
    public void init() {
       ...
    }
    
    @Override
    public void doGet(HttpServletRequest req, HttpServletResponse resp) {
       ...
    }
    
    @Override
    public void destory() {
       ...
    }
}
{% endhighlight %}

<br>

#### 톰캣의 servlet-api.jar 클래스 패스 설정하기

- 서블릿 API들은 톰캣의 servlet-api.jar 라이브러리 제공

  - 이클립스의 프로젝트에선 서블릿을 사용할면 반드시 클래스 패스를 설정해야 함
  
- 설정 방법

  - 이클립스 상단의 New > Dynamic Web Project
  
  - 프로젝트명 : pro05, 경로 : D:\\git\\sunny\\pro05 > Next > Next
  
  - \[체크] Generate web.xml deployment descriptor > Finish
  
  - 생성된 프로젝트 pro05를 선택 > 마우스 우클릭 > Build Path > Configure Build Path...
  
  - Java Build Path 창에서 Libraries 탭 > Add Extends JARs...(우측 2번째 button)
  
  - \%CATALINA_HOME\% (D:\\tomcat9)의 lib 디렉터리의 servlet-api.jar를 선택 후 열기 > Apply and Close
  
<br>

#### 첫 번째 서블릿 만들기 (FirstSerlvet.java)

- pro05프로젝트의 Java Resource 디렉터리 하위의 src 선택 > 마우스 우클릭 > New > Package

- Name : sec01.ex01 > Finish > 패키지 생성 확인

- 생성된 패키지 선택 > 마우스 우클릭 > New > Class

- Name : FirstServlet > Finish > 생성됨 확인

{% highlight java %}
package sec01.ex01;

public class FirstServlet {

}
{% endhighlight %}

- 자바 코드 작성(FirstServlet.java)

  - HttpServlet 상속
  
  - 3개의 생명주기 메서드를 차례로 구현
  
  - 각 메서드는 호출시 메시지만 출력

{% highlight java %}
package sec01.ex01;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class FirstServlet extends HttpServlet {

	@Override
	public void init() throws ServletException {
		System.out.println("init 메서드 호출");
	}
	
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("doGet 메서드 호출");
	}
	
	@Override
	public void destroy() {
		System.out.println("destory 메서드 호출")
	}
}
{% endhighlight %}

<br>

#### 서블릿 매핑하기

- 브라우저에서 서블릿명으로 요청하는 방법

  - 톰캣에 프로젝트 추가
  
  - 프로젝트명 뒤 패키지명이 포함된 클래스명 전부를 입력
  
  - http:\//IP주소:port/프로젝트명/패키지명포함클래스명
  
  - http:\//127.0.0.1:8090/pro05/sec01.ex01.FirstServlet
  
  - 이렇게 되면 입력하기가 불편하고 보안에도 좋지 않음
  <br>서블릿 클래스명에 대응되는 서블릿 매핑명으로 실제 서블릿을 요청 (톰캣에 등록은 해야함)
  
- 서블릿 매핑 과정

  - 각 프로젝트에 있는 web.xml 설정
  
  - \<servlet>태그와 \<servlet-mapping> 태그 이용
  
  - 여러 개의 서블릿 매핑 시 \<servlet> 태그를 먼저 정의 후 \<servlet-mapping> 태그를 정의
  
- 실제 서블릿 매핑의 예

{% highlight html %}
<servlet>
  <servlet-name>aaa</servlet-name>
  <servlet-class>sec01.ex01.FirstServlet</servlet-class>
</servlet>

<servlet-mapping>
  <servlet-name>aaa</servlet-name>
  <url-pattern>first</url-pattern>
</servlet-mapping>
{% endhighlight %}
