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
  - Servlet mapping
  - web.xml
  - 서블릿 생명주기
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
       └──── GenericServlet ────┘
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

## FirstServlet을 이용한 실습 - 사용자 정의 서블릿을 만들어보기

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
		System.out.println("destory 메서드 호출");
	}
}
{% endhighlight %}

<br>

#### 서블릿 매핑하기

- 브라우저에서 서블릿명으로 요청하는 방법

  - 톰캣에 프로젝트 추가
  
  - 프로젝트명 뒤 패키지명이 포함된 클래스명 전부를 입력
  
  - http://IP주소:port/프로젝트명/패키지명포함클래스명
  
  - http://127.0.0.1:8090/pro05/sec01.ex01.FirstServlet
  
  - 이렇게 되면 입력하기가 불편하고 보안에도 좋지 않음
  <br>서블릿 클래스명에 대응되는 서블릿 매핑명으로 실제 서블릿을 요청 (톰캣에 등록은 해야함)
  
- 서블릿 매핑 과정

  - 각 프로젝트에 있는 web.xml 설정
  
  - \<servlet>태그와 \<servlet-mapping> 태그 이용
  
  - 여러 개의 서블릿 매핑 시 \<servlet> 태그를 먼저 정의 후 \<servlet-mapping> 태그를 정의
  
- 실제 서블릿 매핑의 예

  - \<servlet> \<servlet-mapping> 하위 태그에 공통으로 \<servlet-name>이 들어감
  
  - 공통으로 들어가는 \<servlet-name> 태그의 같은 값(예제에선 aaa)이 \<servlet>과 \<servlet-mapping> 태그를 연결해 줌
  
  - 웹 브라우저에서 \<servlet-mapping>의 \<url-pattern> 태그의 URL(예제에선 /first)로 요청
  
  - \<servlet-mapping>의 \<url-pattern>에 해당하는 \<servlet-name> 값(예제에선 aaa)확인
  
  - 확인한 값(aaa)을 \<servlet>의 \<servlet-name>에서 찾고 태그를 연결시켜 줌
  
  - 브라우져에서 /<url-pattern>의 /first로 요청할 경우, aaa값을 가지는 \<servlet> 태그를 찾아 실제 서블릿인 sec.ex01.FirstServlet을 실행

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

- 서블릿 매핑을 실제 프로젝트에 적용해보기

  - pro05 프로젝트의 WebContent > WEB-INF 폴더 클릭 > web.xml을 엶
  
  - \<web-app> 태그의 하위 태그를 지우고 거기에 서블릿 매핑을 작성
  
{% highlight html %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <servlet>
  	<servlet-name>aaa</servlet-name>
  	<servlet-class>sec01.ex01.FirstServlet</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>aaa</servlet-name>
  	<url-pattern>/first</url-pattern>
  </servlet-mapping>
</web-app>
{% endhighlight %}

<br>

#### 톰캣에 프로젝트 실행

- 새로 만든 서버(pro05)를 톰캣에 등록

- 톰캣 재실행

- 웹 브라우저에서 서블릿 매핑 이름인 /first로 요청

  - http://127.0.0.1:8090/pro05/first

- 이클립스 콘솔에 메시지 찍히는지 확인

  - init 메서드 호출
  
  - doGet 메서드 호출
  
![image](/img/2019-10-23/start-servlet-001-console1.png)

<br>
<br>

## FirstServlet을 이용한 실습 - 사용자 정의 서블릿을 만들어보기

#### 다수의 서블릿 매핑하기

- 일반적인 웹 애플리케이션은 각 기능에 대한 서블릿을 따로 만들어서 관리

  - 즉 프로젝트에서 여러 개의 서블릿을 만들어 사용

<br>

#### 다른 서블릿 SecondServlet.java 추가

- FirstServlet과 동일한 위치에 서블릿 파일 추가

  - 내용

{% highlight java %}
package sec01.ex01;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class SecondServlet extends HttpServlet {

	@Override
	public void init() throws ServletException {
		System.out.println("init 메서드 호출 >>>>");
	}
	
	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("doGet 메서드 호출>>>>");
	}
	
	@Override
	public void destroy() {
		System.out.println("destroy 메서드 호출>>>>");
	}
}
{% endhighlight %}

- web.xml 파일에 매핑

  - 주의 할 점) 여러개 매핑시 \<servlet> 태그와 \<servlet-mapping> 태그를 각각 분리해서(개당 1개씩) 작성해야 함
  
  - \<servlet>태그끼리 \<servlet-mapping>태그끼리 위치(뭉쳐놓기)
  
  - \<servlet-name>태그 값은 유니크해야함
  
  - 내용
  
{% highlight html %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
  <servlet>
  	<servlet-name>aaa</servlet-name>
  	<servlet-class>sec01.ex01.FirstServlet</servlet-class>
  </servlet>
  <servlet>
  	<servlet-name>bbb</servlet-name>
  	<servlet-class>sec01.ex01.SecondServlet</servlet-class>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>aaa</servlet-name>
  	<url-pattern>/first</url-pattern>
  </servlet-mapping>
  <servlet-mapping>
  	<servlet-name>bbb</servlet-name>
  	<url-pattern>/second</url-pattern>
  </servlet-mapping>
</web-app>
{% endhighlight %}

- web.xml 변경사항 반영은 톰캣 재기동 필요 > 톰캣 재시작(정지 후 시작)

- 브라우저에서 각각 요청

  - http://127.0.0.1:8090/pro05/first
  
  - http://127.0.0.1:8090/pro05/second

![image](/img/2019-10-23/start-servlet-002-console2.png)

<br>
<br>

## 서블릿 동작 과정

#### 서블릿 동작 과정

- ① 클라이언트가 http://localhost:8090/proc05/first로 요청 (→ 톰캣)

- ② 톰캣이 web.xml에서 서블릿 확인 → sec01.ex01.FirstServlet.java

- ③ 톰캣은 확인한 서블릿이 메모리에 존재하는지 확인 (존재한다면 ⑥으로 이동)

- ④ FirstServlet을 메모리에 로드

- ⑤ init()을 호출

- ⑥ doGet()또는 doPost()를 호출

- ⑦ 결과를 Client에게 응답

<br>

#### 특징

- 동일한 작업의 경우, 서블릿은 메모리에 존재하는 서블릿을 재사용

  - 훨씬 빠르고 효율적
  
- 서블릿은 스레드 방식으로 동작

<br>

#### 동일한 작업 테스트

- http://localhost:8090/proc05/first로 요청을 3번 연속 해보기

- init(초기화)는 1번 실행, doGet(실행)은 3번 실행을 확인할 수 있음

![image](/img/2019-10-23/start-servlet-003-console3.png)

<br>
<br>

## 애너테이션을 이용한 서블릿 매핑

#### web.xml에 서블릿 매핑 vs 애너테이션에 서블릿 매핑

- web.xml에 여러 서블릿 설정

  - 장점) 한 파일에서 확인 가능
  
  - 단점) 파일이 복잡해짐

- 애너테이션(@)에 서블릿 매핑

  - 소스코드에 직접 기능을 설정
  
  - 톰캣 7 버전부터 사용 가능함

  - 장점) 가독성이 좋음
  
  - 단점) 한 파일에서 확인할 수 없음
  
<br>

#### 애너테이션을 이용한 서비스 매핑

- @WebServlet("url-pattern")을 클래스 위에 붙여줌

  - 예) @WebServlet("/third")
  
- 해당 애너테이션을 사용하려면 꼭 HttpServlet 클래스를 상속받아야 함

  - 클래스 클래스명 extends HttpServlet { }
  
<br>

#### 애너테이션을 이용한 서비스 매핑 실습

- 기존처럼 이클립스에서 서블릿 클래스를 직접 만든 후 자바 코드에 애너테이션을 붙여서 매핑할 수도 있음

- 이클립스에서는 서블릿을 만들면서 애너테이션을 바로 적용할 수 있음 ! ! 

- 실습

  - pro05 프로젝트의 sec01.ex01 패키지를 선택 > 마우스 우클릭 > New > Servlet
  
  - 클래스명 ThirdServlet > Next 
  
  - 하단의 URL mappings: /ThirdServlet 선택 > 우측의 Edit... > 패턴 : /third > OK > Next
  
  - 하단 Which method stubs would you like to create에서 
  <br>\[체크해제] Constructors from superclass
  <br>\[체크-기본값] Inherited abstract methods
  <br>메소드 중 init, destroy, doGet 체크 > Finish
  
- 생성 결과(ThirdServlet.java)

  - `private static final long serialVersionUID = 1L;` → 서비스 직렬화를 위해 이클립스에서 자동으로 생성한 상수
  
  - 자동으로 생성된 주석들은 삭제해도 무관
  
  - `@WebServlet("/third")` 확인 가능
  
  - import도 자동으로 되어있고 HttpServlet 상속도 되어있음

{% highlight java %}
package sec01.ex01;

import java.io.IOException;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ThirdServlet
 */
@WebServlet("/third")
public class ThirdServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException {
		// TODO Auto-generated method stub
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() {
		// TODO Auto-generated method stub
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

}
{% endhighlight %}

- ThirdServlet.java에 기능 추가

  - 메세지 출력 기능 추가
  
{% highlight java %}
package sec01.ex01;

import java.io.IOException;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ThirdServlet
 */
@WebServlet("/third")
public class ThirdServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException {
		System.out.println("ThirdServlet init 메서드 호출");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() {
		System.out.println("ThirdServlet distroy 메서드 호출");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		System.out.println("ThirdServlet doGet 메서드 호출");
	}

}
{% endhighlight %}

- 서블릿 매핑 설정을 했으므로 톰캣 재시작 (정지 > 시작)

- 웹 브라우저에서 서블릿 매핑명(URL패턴)으로 요청

  - http://127.0.0.1:8090/pro05/third
  
- 콘솔 메시지 확인

![image](/img/2019-10-23/start-servlet-004-console4.png)

<br>

#### 애너테이션 서블릿 매핑시 주의점

- 매핑명(URL 패턴)이 이미 사용된(URL 패턴)과 중복되지 않도록 주의해야 함

- 중복되면 톰캣 시작시 에러 메시지 발생하며 서버가 정상적으로 뜨지 못함

  - ThirdServlet을 /first로 매핑명 수정하고 테스트해본 결과

  - Caused by: java.lang.IllegalArgumentException: 이름이 [aaa]과 [sec01.ex01.ThirdServlet]인 두 서블릿들 모두 url-pattern [/first]에 매핑되어 있는데, 이는 허용되지 않습니다.

- 매핑이 동일하게 되더라도 이클립스에서 선경고를 띄워주진 않는 것 같음
