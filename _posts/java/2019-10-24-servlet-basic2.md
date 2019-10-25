---
layout: post
title:  "[Java] 서블릿 기초-(2)"
date:   2019-10-24 22:14:14
author: Choi HyeSun
categories: java
tags:
  - Java
  - 서블릿
  - Servlet
  - 서블릿 기초
  - Servlet Basic
---

## 웹 브라우저에서 서블릿으로 데이터 전송하기

#### GET/POST 전송 방식

- GET 방식

  - 서블릿에 데이터를 전송할 때 데이터가 URL 뒤에 name=value 형태로 전송
  
  - 여러 개의 데이터를 전송할 때 '&'로 구분해서 전송
  
  - 보안이 취약
  
  - 전송 최대 255자
  
  - 기본 전송방식, 사용이 쉬움
  
  - 웹 브라우저에서 직접 입력해서 전송 가능
  
  - 서블릿의 doGet()으로 처리
  
- POST 방식

  - 서블릿에 데이터를 전송할 때 TCP/IP 프로토콜 데이터의 HEAD 영역에 숨겨진 채 전송

  - 보안에 유리
  
  - 전송 데이터 용량 무제한
  
  - 전송시 서블릿에서 또 다시 가져오는 작업을 해야하므로 GET방식보다 처리속도가 느림
  
  - 서블릿의 doPost()로 처리
  
<br>

#### GET 방식으로 서블릿에 요청

- HTML의 \<form> 태그의 method 속성이 get
  
  - 서블릿에 GET 방식으로 데이터를 전송하겠다는 의미
  
  - `<form name="frmLogin" method="get" action="login" encType="UTF-8)">`

  
- doGet() 메서드

  - GET 방식으로 적용된 데이터를 처리
  
  - `protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {}

- 브라우저 URL 뒤에 'name=value'쌍

  - 어떤 데이터를 전송하는지 다 노출되므로 보안상으로 좋지 않음

  - `http://localhost:8090/pro06/calc?won=1&operator=dollar&command=calculate`

<br>

#### POST 방식으로 서블릿에 요청

- 실습1. - HTML / Servlet 생성

  - WebContent > login3.html 생성 (데이터는 login.html과 동일, action을 login3으로, method는 post)

  - sec03.01 패키지 생성 > LoginServlet3 클래스 생성, mapping-/login3, init/doPost/destroy

{% highlight java %}
package sec03.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoginServlet2
 */
@WebServlet("/login3")
public class LoginServlet3 extends HttpServlet 
{
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException 
	{
		System.out.println("init 메서드 호출. pro06.sec03.ex01.LoginServlet3.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() 
	{
		System.out.println("init 메서드 호출. pro06.sec03.ex01.LoginServlet3.java");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
	{// Post 방식으로 전송된 데이터 처리
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8"); // 응답할 데이터 종류 설정
		PrintWriter out = response.getWriter();	            // HttpServletResponse 객체의 getWriter()를 이용해 출력 스트림 PrintWriter 객체를 받아옴
		String id = request.getParameter("user_id");
		String pw = request.getParameter("user_pw");
	}

}
{% endhighlight %}

- 실습2. 실행 (톰캣 재시작)

  - 브라우저에서 http://localhost:8090/pro06/login3.html 호출 후 ID/PW 입력
  <br>![image](/img/2019-10-24/servlet-basic-015-web12.png)
  
  - POST는 GET과 달리 URL에 아무것도 남지 않음을 확인할 수 있음
  <br>![image](/img/2019-10-24/servlet-basic-016-web13.png)

- POST는 TCP/IP헤더에 값이 숨겨진 채 전송되므로 URL 뒤에 아무것도 표시되지 않음

- 서블릿에서는 웹 브라우저에서 전송되는 방식에 따라 doGet()이나 doPost() 메서드로 대응해서 처리해야 함

  - 만약 전송방식과 다른 메서드 사용시, 브라우저에서 오류가 발생
  <br>HTTP Status 405 - Method Not Allowed
  
<br>

#### 알아둘 점

- GET/POST 방식으로 전송된 데이터는 반드시 HttpServlet에서 오버라이딩된 doGet()이나 doPost() 메서드를 사용해야 함

- 두 메서드가 서블릿에 존재하지 않거나, 사용자가 임의로 만든 메서드를 사용하면 오류가 발생

<br>
<br>

## GET 방식과 POST 방식 요청 동시에 처리하기

#### doHandle()을 호출해서 모든 기능을 구현하는 예

- GET+POST 방식일 때, 두 기능을 각각 구분해서 구현하면 불편함

- 전송된 방식으로 doGet()이나 doPost()메서드로 처리 후 doHandle()을 호출하여 모든 기능을 구현하는 예제 실습

- 실습1. HTML, Servlet 생성

  - login4.html 생성, (copy login.html, action > login4) - Get방식임
  
  - sec03.ex02 패키지 생성 > LoginServlet4 서블릿 - /login4, init/doGet/doPost/destroy 
  
{% highlight java %}
package sec03.ex02;

import java.io.IOException;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoginServlet4
 */
@WebServlet("/login4")
public class LoginServlet4 extends HttpServlet 
{
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException 
	{
		System.out.println("init 메서드 호출. pro06.sec03.ex02.LoginServlet4.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() 
	{
		System.out.println("init 메서드 호출. pro06.sec03.ex02.LoginServlet4.java");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		System.out.println("doGet 메서드 호출. pro06.sec03.ex02.LoginServlet4.java");
		doHandle(request, response);
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		System.out.println("doPost 메서드 호출. pro06.sec03.ex02.LoginServlet4.java");
		doHandle(request, response);
	}

	private void doHandle(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		request.setCharacterEncoding("utf-8");
		String user_id = request.getParameter("user_id");
		System.out.println("doHandle 메서드 호출. pro06.sec03.ex02.LoginServlet4.java");
		String user_pw = request.getParameter("user_pw");
		System.out.println("아이디 : " + user_id);
		System.out.println("비밀번호 : " + user_pw);
	}
}
{% endhighlight %}

- 실습 2. doGet 테스트 

  - 톰캣 재실행
  
  - 요청 : http://localhost:8090/pro06/login4.html
  
  - 요청 후 URL : http://localhost:8090/pro06/login4?user_id=sun&user_pw=sunny
  
  - 콘솔 : 

{% highlight java %}
init 메서드 호출. pro06.sec03.ex02.LoginServlet4.java
doGet 메서드 호출. pro06.sec03.ex02.LoginServlet4.java
doHandle 메서드 호출. pro06.sec03.ex02.LoginServlet4.java
아이디 : sun
비밀번호 : sunny
{% endhighlight %}

- 실습 3. doPost 테스트

  - login4.html method를 post로 변경
  
  - 요청 : http://localhost:8090/pro06/login4.html
  
  - 요청 후 URL : http://localhost:8090/pro06/login4
  
  - 콘솔 : (초기화는 아까 되었기 때문에 X)

{% highlight java %}
doPost 메서드 호출. pro06.sec03.ex02.LoginServlet4.java
doHandle 메서드 호출. pro06.sec03.ex02.LoginServlet4.java
아이디 : sun
비밀번호 : sunny
{% endhighlight %}

<br>
<br>

## 자바 스크립트로 서블릿에 요청하기

- 자바스크립트로 서블릿에 요청하기

  - \<form> : 바로 서블릿으로 전달
  
  - 자바스크립트 : 유효성 검사 등을 할 수 있음
  
- 실습1. login.html을 추가

  - 자바 스크립트 구현
  
{% highlight html %}
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<script type="text/javascript">
		function fn_validate() {
			var frmLogin = document.frmLogin;		// <form>태그의 name 속성으로 <form>태그 객체를 받아옴
			var user_id = frmLogin.user_id.value;	// <form>태그의 <input>태그의 name 속성으로 입력한 ID와 비밀번호를 받아옴
			var user_pw = frmLogin.user_pw.value;
			
			if ((user_id.length == 0 || user_id =="") || (user_pw.langth == 0 || user_pw == ""))  
			{
				alert("아이디와 비밀번호는 필수입니다.");
			}	else 
			{
				frmLogin.method = "post";			// <form> 태그의 전송 방식(method)를 post로 지정(덮어쓰기)
				frmLogin.action = "login5";			// <form> 태그의 action을 서블릿 매핑명인 login5로 지정(덮어쓰기)
				frmLogin.submit();					// 자바스크립트에서 서블릿으로 전송
			}
			
		}
	</script>
	<title>로그인창</title>
</head>
<body>
	<form name="frmLogin" method="get" action="login" encType="UTF-8"> <!-- 여기의 method와 action은 자바스크립트에서 덮어 씌울 것 입니다. -->
		아이디 :<input type="text" name="user_id"><br>
		비밀번호 :<input type="password" name="user_pw"><br>
		<input type="button" onClick="fnvalidate()" value="로그인">
		<input type="reset" value="다시입력">
		<input type="hidden" name="user_address" values="서울시 성북구" /> <!-- <hidden>태그를 이용해 화면에는 보이지 않게 숨은 값을 서블릿으로 전송 -->
	</form>
</body>
</html>
{% endhighlight %}
  
- 실습2 : sec03.ex03 패키지에 LoginServlet5 클래스(서블릿) 생성

  - 매핑URL : /login5, init/getPost/destroy
  
{% highlight java %}
package sec03.ex03;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoginServlet5
 */
@WebServlet("/login5")
public class LoginServlet5 extends HttpServlet 
{
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException 
	{
		System.out.println("init 메서드 호출. pro06.sec03.ex03.LoginServlet5.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() 
	{
		System.out.println("distroy 메서드 호출. pro06.sec03.ex03.LoginServlet5.java");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		String id = request.getParameter("user_id");
		String pw = request.getParameter("user_pw");
		String address = request.getParameter("user_address");
		System.out.println("아이디 : " + id);
		System.out.println("비밀번호 : " + pw);
		
		String data = "<html>";
		data += "<body>";
		data += "아이디 : " + id;
		data += "<br>";
		data += "비밀번호 :" + pw;
		data += "<br>";
		data += "주소 :" + address;
		data += "</body>";
		data += "</html>";
		out.print(data);
	}

}
{% endhighlight %}

- 실습 3. 실행

  - http://localhost:8090/pro06/login5.html 브라우저로 호출
  
  - ID/PW 둘 중 하나라도 안넣으면 아이디와 비밀번호는 필수입니다. 알림
  
  - 다 넣으면 웹 브라우저에서 ID/PW 출력, 주소는 Hidden 기본값으로 셋팅되있던게 출력됨

<br>
<br>

## 서블릿을 이용한 여러 가지 실습 예제

- 실습 전, WebContent 위치에 실습용 HTML파일을 따로 저장하는 test01 폴더를 만듦

<br>

#### 실습 예제 1. 서블릿에 로그인 요청시 유효성 검사

- 문제) ID를 정상적으로 입력했을 때는 로그인 메시지를 표시하고, ID를 입력하지 않았을 때는 다시 로그인하라는 메시지를 표시하도록 작성하시오

- 실습1. Html 작성

  - test01 폴더(미리 생성해뒀던)에 loginTest.html을 만듦
  
{% highlight html %}
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>로그인 창</title>
	</head>
<body>      <!-- action에 서블릿을 지정할 때, test01에 위치한 HTML에서 요청하므로 프로젝트 이름을 붙여줘야 함(절대경로) -->
	<form name="frmLogin" method="post" action="/pro06/loginTest" encType="UTF-8">
		아이디 :<input type="text" name="user_id"><br>        <!-- 텍스트 박스에 입력한 ID를 name 속성인 user_id로 전송 -->
		비밀번호:<input type="password" name="user_pw"><br>   <!-- 텍스트 박스에 입력한 비밀번호를 name 속성인 user_pw로 전송 -->
		<input type="submit" value="로그인"> <input type="reset" value="다시 입력">
	</form>
</body>
</html>
{% endhighlight %}

- 실습2. Servlet 작성

  - sec04.ex01.LoginTest, /loginTest, init/destroy/doPost
  
{% highlight java %}
package sec04.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoginTest
 */
@WebServlet("/loginTest")
public class LoginTest extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init 메서드 호출. pro06.sec04.ex01.LoginTest.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() {
		System.out.println("destroy 메서드 호출. pro06.sec04.ex01.LoginTest.java");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		String id = request.getParameter("user_id");
		String pw = request.getParameter("user_pw");
		
		System.out.println("아이디 : " + id);
		System.out.println("비밀번호 : " + pw);
		
		if(id != null && (id.length() != 0))
		{
			out.print("<html>");
			out.print("<body>");
			out.print(id + "님!! 로그인하셨습니다.");
			out.print("</body>");
			out.print("</html>");
		}
		else 
		{
			out.print("<html>");
			out.print("<body>");
			out.print("아이디를 입력하세요.");
			out.print("<br>");
			out.print("<a href='http://localhost:8090/pro06/test01/loginTest.html'>로그인 창으로 이동</a>");
			out.print("</html>");
			out.print("</body>");
		}
	}

}
{% endhighlight %}

- 실습3. 실행

  - http://localhost:8090/pro06/test01/loginTest.html 브라우저에서 실행(폴더명까지 꼭!)
  
  - ID가 없을 시 : 아이디를 입력하세요. (버튼) 로그인 창으로 이동
  
  - ID가 있을 시 : ID님 로그인하셨습니다!!
  
  - 둘 다, 콘솔에 ID/PW 전달됨

<br>

#### 실습 예제 2. 서블릿으로 로그인 요청시 관리자 화면 나타내기

- 문제 : 실습 예제 1을 이용해 로그인시 admin으로 로그인하면 회원 관리와 회원 삭제 기능을 보여주도록 작성하시오

- 실습 1. HTML 작성

  - loginTest.html을 카피하여 loginTest2로 생성 → action은 /pro06/loginTest2를 보도록
  
- 실습 2. Servlet 작성

  - LoginServlet2.java를 작성

{% highlight java %}
package sec04.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoginTest
 */
@WebServlet("/loginTest2")
public class LoginTest2 extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException {
		System.out.println("init 메서드 호출. pro06.sec04.ex01.LoginTest2.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() {
		System.out.println("destroy 메서드 호출. pro06.sec04.ex01.LoginTest2.java");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		String id = request.getParameter("user_id");
		String pw = request.getParameter("user_pw");
		
		System.out.println("아이디 : " + id);
		System.out.println("비밀번호 : " + pw);
		
		if(id != null && (id.length() != 0))
		{
			if(id.equals("admin"))
			{
				out.print("<html>");
				out.print("<body>");
				out.print("<font size='12'>관리자로 로그인 하셨습니다!!</font>");
				out.print("<br>");
				out.print("<input type=button value='회원정보 수정하기' />");
				out.print("<input type=button value='회원정보 삭제하기' />");
				out.print("</body>");
				out.print("</html>");
			} else
			{
				out.print("<html>");
				out.print("<body>");
				out.print(id + "님!! 로그인하셨습니다.");
				out.print("</body>");
				out.print("</html>");
			}

		}
		else 
		{
			out.print("<html>");
			out.print("<body>");
			out.print("아이디를 입력하세요.");
			out.print("<br>");
			out.print("<a href='http://localhost:8090/pro06/test01/loginTest2.html'>로그인 창으로 이동</a>");
			out.print("</html>");
			out.print("</body>");
		}
	}

}
{% endhighlight %}

- 실습3. 실행하기

  - 아이디를 안넣었을 때와, 일반 아이디로 로그인했을 때는 위와 동일
  
  - admin으로 로그인시, 관리자로 로그인 하셨습니다!!가 뜨면서 아래 기능 버튼 2개 위치

<br>

#### 실습 예제 3. 서블릿으로 요청시 구구단 출력

- 문제 : 구구단 단수를 입력받아 단수를 출력하시오

- 실습 1. HTML 생성

  - 구구단의 단수를 입력받는 gugu.html을 생성
  
  - 단수를 입력받아 guguTest1 Servlet으로 전송

{% highlight html %}
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>단수 입력창</title>
	</head>
	<body>
		<h1>출력할 구구단의 수를 지정해주소서</h1>
		<form method="get" action="/pro06/guguTest">
			출력할 구구단 :<input type=text name="dan" /> <br>
			<input type="submit" value="구구단 출력">
		</form>
	</body>
</html>
{% endhighlight %}

- 실습 2. Servlet 생성

  - sec04.ex01.GuguTest.java
  
  - \<table>의 태그와 \<tr> 태그와 자바의 for문을 이용해 구구단을 연속해서 행으로 출력

{% highlight java %}
package sec04.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class GuguTest
 */
@WebServlet("/guguTest")
public class GuguTest extends HttpServlet 
{
	private static final long serialVersionUID = 1L;
       

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException 
	{
		System.out.println("init 메서드 호출. pro06.sec04.ex01.GuguTest.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() 
	{
		System.out.println("destroy 메서드 호출. pro06.sec04.ex01.GuguTest.java");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		int dan = Integer.parseInt(request.getParameter("dan"));
		out.print("<table border=1 with=800 align=center>");
		out.print("<tr align=center bgcolor='#FFFF66'>");
		out.print("<td colspan=2>" + dan + "단 출력</td>");
		out.print("</tr>");
		for (int i = 1; i < 10; i++)
		{
			out.print("<tr align=center>");
			out.print("<td width=400>");
			out.print(dan + " * " + i);
			out.print("</td>");
			out.print("<td width=400>");
			out.print(i * dan);
			out.print("</td>");
			out.print("</tr>");
		}
		out.print("</table>");
	}

}
{% endhighlight %}

- 실습 3. 실행하기

  - http://localhost:8090/pro06/test01/gugu.html
  
  ![image](/img/2019-10-24/servlet-basic-017-web14.png)
  
  ![image](/img/2019-10-24/servlet-basic-018-web15.png)

<br>

#### 실습 예제 4. 서블릿의 응답 기능을 이용해 구구단 테이블의 행 배경색 교대로 바꿔보기

> TIP1. HTML 소스 보기
>
>> 브라우저 우클릭 후 페이지 소스 보기
>
> TIP2. HTML 컬러 차트
>
>> [LINK](https://html-color-codes.info/)
>
> TIP3. 이클립스에서 계층형 패키지 구조 보기
>
>> 이클립스 > Project Explorer > ▼ > Package > Hierarchical
>>
>> 반대로는 Flat

- 실습 1. HTML 생성

  - gugu.html을 복사하여 gugu2.html을 생성 (action > /pro06/guguTest2)
  
- 실습 2. Servlet 생성

  - GuguTest2 클래스를 생성하고 코드를 지정
  
{% highlight java %}
package sec04.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class GuguTest
 */
@WebServlet("/guguTest2")
public class GuguTest2 extends HttpServlet 
{
	private static final long serialVersionUID = 1L;
       

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException 
	{
		System.out.println("init 메서드 호출. pro06.sec04.ex01.GuguTest.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() 
	{
		System.out.println("destroy 메서드 호출. pro06.sec04.ex01.GuguTest.java");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		int dan = Integer.parseInt(request.getParameter("dan"));
		out.print("<table border=1 with=800 align=center>");
		out.print("<tr align=center bgcolor='#FFFF66'>");
		out.print("<td colspan=2>" + dan + "단 출력</td>");
		out.print("</tr>");
		for (int i = 1; i < 10; i++)
		{
			if (i % 2 == 0)
			{
				out.print("<tr align=center bgcolor='#ACFA58'>");
			} else
			{
				out.print("<tr align=center bgcolor='#81BEF7'>");
			}
			
			out.print("<td width=400>");
			out.print(dan + " * " + i);
			out.print("</td>");
			out.print("<td width=400>");
			out.print(i * dan);
			out.print("</td>");
			out.print("</tr>");
		}
		out.print("</table>");
	}

}
{% endhighlight %}

- 실습 3. 실행

  - http://localhost:8090/pro06/test01/gugu2.html

  ![image](/img/2019-10-24/servlet-basic-019-web16.png)

<br>
<br>

#### 실습 예제 4. 서블릿의 응답 기능을 이용해 라디오박스와 체크박스를 구현하기

- 실습 1. HTML 생성

  - gugu2를 복사한 후, action만 /pro06/guguTest3으로 변경

- 실습 2. Servlet 생성

  - sec04.ex01.GuguTest3.java
  
{% highlight java %}
package sec04.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class GuguTest
 */
@WebServlet("/guguTest3")
public class GuguTest3 extends HttpServlet 
{
	private static final long serialVersionUID = 1L;
       

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException 
	{
		System.out.println("init 메서드 호출. pro06.sec04.ex01.GuguTest.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() 
	{
		System.out.println("destroy 메서드 호출. pro06.sec04.ex01.GuguTest.java");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		int dan = Integer.parseInt(request.getParameter("dan"));
		out.print("<table border=1 with=1600 align=center>");
		out.print("<tr align=center bgcolor='#FFFF66'>");
		out.print("<td colspan=4>" + dan + "단 출력</td>");
		out.print("</tr>");
		for (int i = 1; i < 10; i++)
		{
			if (i % 2 == 0)
			{
				out.print("<tr align=center bgcolor='#ACFA58'>");
			} else
			{
				out.print("<tr align=center bgcolor='#81BEF7'>");
			}
			out.print("<td width=200>");
			out.print("<input type='radio' />" + i);
			out.print("</td>");
			out.print("<td width=200>");
			out.print("<input type='checkbox' />" + i);
			out.print("</td>");
			out.print("<td width=400>");
			out.print(dan + " * " + i);
			out.print("</td>");
			out.print("<td width=400>");
			out.print(i * dan);
			out.print("</td>");
			out.print("</tr>");
		}
		out.print("</table>");
	}

}
{% endhighlight %}

- 실습 3. 실행

  - http://localhost:8090/pro06/test01/gugu3.html
  
  ![image](/img/2019-10-24/servlet-basic-020-web17.png)

<br>
<br>

## 핵심

- 서블릿은 결국 웹 애플리케이션의 화면을 구현하는 것

- 지금은 이렇게 사용하지 않음!
