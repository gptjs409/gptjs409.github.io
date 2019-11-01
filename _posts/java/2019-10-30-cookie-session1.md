---
layout: post
title:  "[Java] 쿠키와 세션-(1)"
date:   2019-10-30 21:29:32
author: Choi HyeSun
categories: java
tags:
  - Java
  - 쿠키
  - Cookie
  - 세션
  - Session
---

## 웹 페이지 연결 기능

- 보통 웹 프로그램에서 사용되는 정보는 서블릿의 비즈니스 로직 처리 기능을 이용해 DB에서 가져옴

- 동시 사용자 수가 많아지면 DB 연동 속도도 영향 → 정보의 종류에 따라 어떤 정보들은 Client PC나 Server의 메모리에 저장해 두고 사용하면 좀 더 프로그램을 빠르게 실행시킬 수 있음

- 서블릿이 로그인 시 사용자의 로그인 상태를 일정하게 유지시키는 기능

<br>

#### 세션 트래킹

- 웹 페이지에서 로그인 등의 정보를 공유하려면?

  - 실제로 HTTP 프로토콜 방식으로 통신하는 웹 페이지들은 서로 어떤 정보도 공유하지 않음

  - CLIENT ↔ 웹 페이지 작업이 제각기 이루어지기 때문

- 세션 트래킹(Session Tracking)

  - 웹 페이지 사이의 상태나 정보를 공유하려면 프로그래머가 웹 페이지 연결 기능을 구현해야 하는데, 이 웹 페이지 연결 기능을 세션 트래킹이라 함

- HTTP 프로토콜

  - 서버-클라이언트 통신 시 (각 웹 페이지의 상태나 정보를 다른 페이지들과 공유하지 않는) stateless 방식 통신
  
  - 브라우저에서 새 웹 페이지를 열면 기존의 웹 페이지나 서블릿에 관한 어떤 연결 정보도 새 웹 페이지에서는 알 수 없음
  
  - 웹 페이지나 서블릿끼리 상태나 정보를 공유하려면 웹 페이지 연결 기능, 즉 세션 트래킹을 이용해야 함
  
- 웹 페이지를 연동하는 방법

  - **\<hidden> 태그** : HTML의 \<hidden> 태그를 이용해 웹 페이지들 사이의 정보를 공유
  
  - **URL Rewriting** : GET 방식으로 URL 뒤에 정보를 붙여서 다른 페이지로 전송
  
  - **쿠키** : 클라이언트 PC의 Cookie 파일에 정보를 저장한 후 웹 페이지들이 공유
  
  - **세션** : 서버 메모리에 정보를 저장한 후 웹 페이지들이 공유

<br>
<br>

## \<hidden> 태그와 URL Rewriting 이용해 웹 페이지 연동하기

- \<hidden> 태그 : 브라우저에 표시되지 않지만 미리 저장된 정보를 서블릿으로 전송할 수 있음

<br>

#### \<hidden> 태그를 이용한 세션 트래킹 실습

- 실습1. pro09 프로젝트 생성, sec01.ex01 패키지 생성

- 실습2. login.html 생성

  - 로그인창에서 ID와 비밀번호를 입력하면 미리 \<hidden> 태그에 저장된 주소, 이메일, 휴대폰 번호를 서블릿으로 전송
  
{% highlight html %}
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>로그인 창</title>
	</head>
	<body>
		<form name="frmLogin" method="post" action="login" encType="UTF-8">
			아이디 :<input type="text" name="user_id"><br>
			비밀번호:<input type="password" name="user_pw"><br>
			<input type="submit" value="로그인">
			<input type="reset" value="다시 입력">
			<input type="hidden" name="user_address" value="서울시 성북구" />	<!-- <hidden> 태그의 value 속성애 주소, 이메일, 전화번호 저장 후 서블릿으로 전송 -->
			<input type="hidden" name="user_email" value="test@gmail.com" />
			<input type="hidden" name="user_hp" value="010-1111-2222" />
		</form>
	</body>
</html>
{% endhighlight %}

- 실습3. LoginServlet.java 생성

  - getParameter() 메서드를 이용해 전송된 회원 정보를 가져온 후 브라우저로 다시 출력

{% highlight java %}
package sec01.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoginServlet
 */
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init 메서드 호출");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		String user_id = request.getParameter("user_id");
		String user_pw = request.getParameter("user_pw");
		String user_address = request.getParameter("user_address"); // <hidden> 태그로 전송된 값을 getParameter() 메서드를 이용해 가져옴
		String user_email = request.getParameter("user_email");
		String user_hp = request.getParameter("user_hp");
		
		String data="안녕하세요!<br> 로그인하셨습니다.<br><br>";
		data += "<html><body>";
		data += "아이디 : " + user_id;
		data += "<br>";
		data += "비밀번호 : " + user_pw;
		data += "<br>";
		data += "주소 : " + user_address;
		data += "<br>";
		data += "email : " + user_email;
		data += "<br>";
		data += "휴대전화 : " + user_hp;
		data += "</body></html>";
		out.print(data);
	}

	
	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() {
		System.out.println("destroy 메서드 호출");
	}

}
{% endhighlight %}

- 실습 4. http://localhost:8090/pro09/login 브라우저 호출

  ![image](/img/2019-10-30/cookie-session1-001-web1.png)

  ![image](/img/2019-10-30/cookie-session1-002-web2.png)

<br>

#### URL Rewriting을 이용한 세션 트래킹 실습

- URL Rewriting을 이용해 로그인창에서 입력받은 ID와 비밀번호를 다른 서블릿으로 전송하여 로그인 상태를 확인

- 실습1. sec01.ex02 패키지 생성

- 실습2. login2.html 생성

  - login.html copy

- 실습3. LoginServlet 클래스 생성

{% highlight java %}
package sec01.ex02;

import java.io.IOException;
import java.io.PrintWriter;
import java.net.URLEncoder;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoginServlet
 */
@WebServlet("/login2")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init 메서드 호출");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		String user_id = request.getParameter("user_id");
		String user_pw = request.getParameter("user_pw");
		String user_address = request.getParameter("user_address");
		String user_email = request.getParameter("user_email");
		String user_hp = request.getParameter("user_hp");
		
		String data="안녕하세요!<br> 로그인하셨습니다.<br><br>";
		data += "<html><body>";
		data += "아이디 : " + user_id;
		data += "<br>";
		data += "비밀번호 : " + user_pw;
		data += "<br>";
		data += "주소 : " + user_address;
		data += "<br>";
		data += "email : " + user_email;
		data += "<br>";
		data += "휴대전화 : " + user_hp;
		data += "<br><br>";
		
		user_address = URLEncoder.encode(user_address, "utf-8");
		data += ("<a href='/pro09/second2?user_id=" + user_id + "&user_pw=" + user_pw + "&user_address=" + user_address +"'>두 번째 서블릿으로 보내기</a>");
		data += "</body></html>";
		out.print(data);
	}
	
	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() {
		System.out.println("destroy 메서드 호출");
	}

}
{% endhighlight %}

- 실습4. SecondServlet 클래스 생성

{% highlight java %}
package sec01.ex02;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SecondServlet
 */
@WebServlet("/second2")
public class SecondServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init 메서드 호출");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() {
		System.out.println("destroy 메서드 호출");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		String user_id = request.getParameter("user_id");
		String user_pw = request.getParameter("user_pw");
		String user_address = request.getParameter("user_address");
		
		out.println("<html><body>");
		if (user_id != null && user_id.length() != 0) {
			out.println("이미 로그인 상태입니다!<br><br>");
			out.println("첫 번째 서블릿에서 넘겨준 아이디 : " + user_id + "<br>");
			out.println("첫 번째 서블릿에서 넘겨준 비밀번호 : " + user_pw + "<br>");
			out.println("첫 번째 서블릿에서 넘겨준 주소 : " + user_address + "<br>");
			out.println("</body></html>");
		} else {
			out.println("로그인 하지 않았습니다.<br><br>");
			out.println("다시 로그인하세요!!<br>");
			out.println("<a href='/pro09/login2.html'>로그인 창으로 이동하기 </>");
		}
	}

}
{% endhighlight %}

- 실습5. 브라우저 실행

<br>
<br>

#### \<hidden> 태그와 GET 방식 연동의 단점

- 페이지가 많아지면 일일이 로그인 상태를 확인하기 위해 로그인 정보를 다른 웹 페이지로 전송해야 함

- ID와 비밀번호를 GET 방식으로 전송하므로 브라우저에 노출되어 보안상으로 좋지 않음

- 전송할 수 있는 데이터 용량에도 한계가 있음

- 웹 페이지 사이에 간단한 정보 정도를 공유할 때만 사용하는 것이 좋음

## 쿠키를 이용한 웹 페이지 연동 기능
