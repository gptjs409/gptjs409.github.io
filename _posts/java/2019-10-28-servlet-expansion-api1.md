---
layout: post
title:  "[Java] 서블릿 확장 API 사용하기-(1)"
date:   2019-10-28 13:27:91
author: Choi HyeSun
categories: java
tags:
  - Java
  - Servlet
  - forward
  - redirect
  - location
  - dispatcher
  - refresh
---

## 서블릿 포워드 기능 사용하기

- 웹 프로그래밍 개발 초기 - 기본적인 서블릿 기능을 이용해 실제 웹 사이트의 기능 구현

  - 서블릿 요청
  
  - 비즈니스 로직 처리
  
  - 웹 브라우저의 화면 표시 응답
  
- 이 외 서블릿 프로그래밍을 개발할 떄 사용하는 기능들

  - 포워딩
  
  - 바인딩
  
  - 애너테이션 
  
  - 등등
  
<br>

#### 포워드 기능

- 포워드(forward)

  - 하나의 서블릿에서 다른 서블릿이나 JSP와 연동하는 방법
  
  - 서블릿에서 다른 서블릿이나 JSP로 요청을 전달하는 역할
  
  - 요청(request) 전달 때 추가 데이터를 포함시켜서 전달할 수도 있음
  
  - 모델2 개발방식으로 웹 애플리케이션 개발시 서블릿에서 JSP로 데이터를 전달할 떄 주로 사용
  
- 포워드 기능이 사용되는 용도

  - 요청(request)에 대한 추가 작업을 다른 서블릿에게 수행하게 함
  
  - 요청에 포함된 정보를 다른 서블릿이나 JSP와 공유
  
  - 요청에 정보를 포함시켜 다른 서블릿에 전달할 수 있음
  
  - 모델2 개발시 서블릿에서 JSP로 데이터를 전달하는 데 사용
<br>
<br>

## 서블릿의 여러가지 포워드 방법

#### 서블릿에서 사용되는 포워드 방법 네 가지

- redirect 방법

  - HttpServletResponse 객체의 sendRedirect() 메서드 이용
  
  - 웹 브라우저에 재요청하는 방식
  
  - 형식 : sendRedirect("포워드할 Servlet or JSP");

- refresh 방법

  - HttpServletResponse 객체의 addHeader() 메서드 이용
  
  - 웹 브라우저에 재요청하는 방식
  
  - 형식 : response.addHeader("Refresh", 경과시간(초);url=요청할 Servlet or JSP");
  
- location 방법

  - 자바스크립트 location 객체의 href 속성 이용
  
  - 자바스크립트에서 재요청하는 방식
  
  - 형식 : location.href='요청할 Servlet or JSP'
 
- dispatch 방법

  - 일반적으로 포워딩 기능을 지칭
  
  - 서블릿이 직접 요청하는 방식
  
  - RequestDispatcher 클래스의 forward() 메서드 이용
  
  - 형식 : RequestDispatcher dis = request.getRequestDispatcher("포워드할 Servlet or JSP");
  <br>dis.forward(request, response);
  
- 특징

  - redirect, resfresh, location : Servlet이 웹 브라우저를 거쳐 다른 Servlet or JSP에 요청하는 방법
  
  - dispatch : Servlet에서 Client를 거치지 않고 바로 다른 Servlet에 요청하는 방법
  
  - 네 가지 모두 자주 사용함
  
<br>

#### redirect를 이용한 포워딩

- 서블릿의 요청이 클라이언트의 웹 브라우저를 다시 거쳐 요청되는 방식

- 순서

  - 클라이언트의 웹 브라우저에서 첫 번째 서블릿에 요청
  
  - 첫 번째 서블릿은 sendRedirect() 메서드를 이용해 두 번째 서블릿을 웹 브라우저를 통해 요청
  
  - 웹 브라우저는 sendRedirect() 메서드가 지정한 두 번째 서블릿을 다시 요청
  
- 실습1. pro08프로젝트를 만들고 sec01.ex01 패키지를 추가
  
- 실습2. FirstServlet.java 생성

{% highlight java %}
package sec01.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class FirstServlet
 */
@WebServlet("/firstRedirect")
public class FirstServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		response.sendRedirect("secondRedirect"); // sendRedirect()를 이용해 웹 브라우저에게 다른 서블릿인 secondRedirect로 재요청
	}

}
{% endhighlight %}

- 실습3. SecondServlet.java 생성

{% highlight java %}
package sec01.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SecondServlet
 */
@WebServlet("/secondRedirect")
public class SecondServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		out.println("<html><body>");
		out.println("sendRedirect를 이용한 redirect 실습입니다.");
		out.println("</body></html>");
	}

}
{% endhighlight %}

- 실습 4. http://localhost:8090/pro08/firstRedirect로 요청

  - http://localhost:8090/pro08/secondRedirect로 넘어감
  
  ![image](/img/2019-10-28/servlet-expansion-api1-001-web1.png)

<br>

#### refresh를 이용한 포워딩

- redirect처럼 웹 브라우저를 거쳐 요청을 수행

- 순서

  - 클라이언트의 웹 브라우저에서 첫 번째 서블릿에 요청
  
  - 첫 번째 서블릿은 addHeader() 메서드를 이용해 두 번째 서블릿을 웹 브라우저를 통해 요청
  
  - 웹 브라우저는 addHeader() 메서드가 지정한 두 번째 서블릿을 재요청
  
- 실습1. sec01.ex02 패키지를 추가

- 실습2. FirstServlet.java 생성

{% highlight java %}
package sec01.ex02;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class FirstServlet
 */
@WebServlet("/firstRefresh")
public class FirstServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		response.addHeader("Refresh",  "1;url=secondRefresh"); // 웹 브라우저에 1초 후 서블릿 secondRefresh로 재 요청
	}

}
{% endhighlight %}

- 실습3. SecondServlet.java 생성

{% highlight java %}
package sec01.ex02;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SecondServlet
 */
@WebServlet("/secondRefresh")
public class SecondServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		out.println("<html><body>");
		out.println("refresh를 이용한 redirect 실습입니다.");
		out.println("</body></html>");
	}

}
{% endhighlight %}

- 실습4. http://localhost:8090/pro08/firstRefresh 호출

  - 1초 후 http://localhost:8090/pro08/secondRefresh로 넘어감
  
  ![image](/img/2019-10-28/servlet-expansion-api1-002-web2.png)
  
  ![image](/img/2019-10-28/servlet-expansion-api1-003-web3.png)

<br>

#### location을 이용한 포워딩

- 실습1. sec01.ex02 패키지를 추가

- 실습2. FirstServlet.java 생성

{% highlight java %}
package sec01.ex03;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class FirstServlet
 */
@WebServlet("/firstLocation")
public class FirstServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		out.print("<script type='text/javascript'>");	// 자바스크립트 location의 href 속성에 서블릿 secondLocation을 설정해 재요청
		out.print("location.href='secondLocation';");
		out.print("</script>");
	}

}
{% endhighlight %}

- 실습3. SecondServlet.java 생성

{% highlight java %}
package sec01.ex03;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SecondServlet
 */
@WebServlet("/secondLocation")
public class SecondServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		out.println("<html><body>");
		out.println("Location를 이용한 redirect 실습입니다.");
		out.println("</body></html>");
	}

}
{% endhighlight %}

- 실습 4. http://localhost:8090/pro08/firstLocation로 요청

  - http://localhost:8090/pro08/secondLocation로 이동
  
  ![image](/img/2019-10-28/servlet-expansion-api1-004-web4.png)

<br>

#### redirect 방식으로 다른 서블릿에 데이터 전달하기

- redirect 방식을 이용하면 웹 브라우저를 통해 다른 서블릿을 호출하면서 원하는 데이터를 전달할 수도 있음

  - GET방식(URL + 파라미터)으로 데이터를 전달
  
  - refresh나 location도 동일하게 할 수 있음

  - 예) "secondRedirect?name=sun"
  <br>String name=request.getParameter("name");

<br>
<br>

## dispatch를 이용한 포워드 방법

- 모델2 방식이나 스트럿츠(struts), 스프링(spring) 프레임에서 포워딩시 사용

<br>

#### dispatch를 이용한 포워딩 과정

- 클라이언트의 웹 브라우저를 거치지 않고 바로 서버에서 포워딩

  - 웹 브라우저 주소창의 URL이 변경되지 않음
  
  - 클라이언트 측에서는 포워드가 진행되었는지 알 수 없음
  
- 순서

  - 클라이언트의 웹 브라우저에서 첫 번째 서블릿에 요청
  
  - 첫 번째 서블릿은 RequestDispatcher를 이용해 두 번째 서블릿으로 포워드
  
- 실습 1. sec03.ex01 패키지 생성
  
- 실습 2. FirstServlet.java 서블릿 생성

{% highlight java %}
package sec03.ex01;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class FirstServlet
 */
@WebServlet("/firstDispatcher")
public class FirstServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset-utf-8");
		RequestDispatcher dispatch = request.getRequestDispatcher("secondDispatcher");
		dispatch.forward(request, response);
	}

}
{% endhighlight %}

- 실습 3. SecondServlet.java 서블릿 생성

{% highlight java %}
package sec03.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SecondServlet
 */
@WebServlet("/secondDispatcher")
public class SecondServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		out.println("<html><body>");
		out.println("dispatch를 이용한 forword 실습");
		out.println("</body></html>");
	}

}
{% endhighlight %}

- 실습 4. http://localhost:8090/pro08/firstDispatcher 브라우저에서 실행

  - 브라우저 url은 바뀌지 않고 SecondSerlvet이 실행됨
  
  - 서블릿의 포워드가 서버단에서 수행되었기 때문

  ![image](/img/2019-10-28/servlet-expansion-api1-004-web4.png)

- get 방식으로 파라미터를 붙여서 데이터를 전달할 수 있음

  - 기존 : RequestDispatcher dispatch = request.getRequestDispatcher("secondDispatcher");

  - 데이터추가 : RequestDispatcher dispatch = request.getRequestDispatcher("secondDispatcher?abc=def");
  
  - 데이터사용 : String alpha = request.getParameter("abc");
