---
layout: post
title:  "[Java] 서블릿 기초"
date:   2019-10-23 22:10:14
author: Choi HyeSun
categories: java
tags:
  - Java
  - 서블릿
  - Servlet
  - 서블릿 기초
  - Servlet Basic
---

## 서블릿의 세 가지 기본 기능

- 톰캣과 같은 WAS(Web Application Server, 웹 애플리케이션 서버)가 처음 나왔을 때 웹 브라우저의 요청을 스레드 방식으로 처리하는 기술 : 서블릿

<br>

#### 서블릿 기본 기능 수행 과정

- 서블릿의 세 가지 주요 기능

  - 클라이언트로부터 요청을 받음

  - 데이터베이스 연동과 같은 비즈니스 로직을 처리

  - 처리된 결과를 클라이언트에 돌려줌

<br>

#### 서블릿 응답과 요청 수행 API 기능

- 요청이나 응답과 관련된 API는 모두 javax.servlet.http 패키지에 있음

  - 요청 관련 API : javax.servlet.http.HttpServletRequest 클래스
  
  - 응답 관련 API : javax.servlet.http.HttpSErvletResponse 클래스
  
- 서블릿의 doGet()이나 doPost()메서드의 매개변수로 사용되는 예

  - doGet(HttpServletRequest request, HttpServletResponse response)
  
  - doPost(HttpServletRequest request, HttpServletResponse response)
  
- 세 가지 주요 기능에는 이렇게 동작

  - 클라이언트로부터 **톰캣 컨테이너**가 요청을 받음
  
  - **사용자의 요청이나 응답에 대한 HttpServletRequest 객체와 HttpServletResponse 객체를 만듦**

  - **서블릿의 doGet()이나 doPost() 메서드를 호출하며 만든 객체들을 전달**

  - **서블릿은** 데이터베이스 연동과 같은 비즈니스 로직을 처리

  - 처리된 결과를 클라이언트에 돌려줌
  
<br>

#### HttpServletRequest의 여러 가지 주요 메서드

|**반환형**|**메서드명**|기능|
|:-:|:-:|---|
|**boolean**|**authenticate(<br>HttpServletResponse response)**|현재 요청한 사용자가 ServletContext 객체에 대한 인증을 하기 위한 컨테이너 로그인 메커니즘 사용|
|**String**|**changeSessionId()**|현재 요청과 연관된 현재 세션의 id를 변경하여 새 세션 id를 반환|
|**String**|**getContextPath()**|요청한 컨텍스트를 가리키는 URI를 반환|
|**Cookie[]**|**getCookies()**|클라이언트가 현재의 요청과 함께 보낸 쿠키 객체들에 대한 배열을 반환|
|**String**|**getHeader(String name)**|특정 요청에 대한 헤더 정보를 문자열로 반환|
|**Enumeration\<String>**|**getHeaderNames()**|현재의 요청에 포함된 헤더의 name 속성을 enumeration 으로 반환|
|**String**|**getMethod()**|현재 요청이 GET, POST 또는 PUT 방식 중 어떤 HTTP 요청인지를 반환|
|**String**|**getRequestURI()**|요청한 URL의 컨텍스트 이름과 파일 경로까지 반환|
|**String**|**getServletPath()**|요청한 URL에서 서블릿이나 JSP 이름을 반환|
|**HttpSession**|**getSession()**|현재의 요청과 연관된 세션을 반환, 세션이 없다면 새로 만들어서 반환|

<br>

#### HttpServletResponse의 여러 가지 주요 메서드

|**반환형**|**메서드명**|기능|
|:-:|:-:|---|
|**void**|**addCookie(Cookie cookie)**|응답에 쿠키를 추가|
|**void**|**addHeader(String name, String value)**|name과 value를 헤더에 추가|
|**String**|**encodeURL(String url)**|클라이언트가 쿠키를 지원하지 않을 때 세션 id를 포함한 특정 URL을 인코딩|
|**Collection\<String>**|**getHeaderNames()**|현재 응답이 헤더에 포함된 name을 얻어옴|
|**void**|**sendRedirect(String location)**|클라이언트에게 리다이렉트(redirect) 응답을 보낸 후 특정 URL로 다시 요청하게 함|

<br>
<br>

## 사전 공부1. HTML의 \<form>태그

{% highlight html %}
<form action="http://validator.w3.org/check" method="get">
  <label for="uri">URL 주소 입력</label>
  <input id="uri" type="text" name="uri" />
  <input type="submit" value="전송" />
</form>
{% endhighlight %}

#### \<form>

- action : 이 폼을 전송할 URL 지정

- method : 폼에 전송할 방식을 지정

  - GET : URL로 값을 받는 방식(쿼리스트링)
  
  - POST : 서버에 값을 URL이 아닌 body에 숨겨서 보내는 방식
  
- autocomplete : 폼 내부 요소의 자동 완성 기능을 사용할지 안할지 결정
 
- accept-charset : 폼 전송시 인코딩 방식을 명시함(utf-8, euc-kr 등)

- enctype : 인코딩 방법 지정, 어떤 문자를 인코딩할 지 등을 결정

- 서버에 데이터를 보내는 양식이면 꼭 form 요소를 이용하는 것이 좋음

  - 자바스크립트를 이용해도 되지만 접근성을 떨어뜨림
  
  - 서버에 데이터를 보내지 않는다면 굳이 form안에 사용하지 않아도 됨

#### \<input>

- 스스로 닫는 태그, 마지막을 /로 닫아줘야 함

- type : 인풋의 형태 타입 결정

  - 속성에 따라 텍스트 입력 뿐만 아니라 전송 버튼, 라디오 버튼, 체크 박스 등 여러가지로 표현 될 수 있음
  
  - text : 텍스트

  - submit : 해당 폼 안에 있는 값들이 해당 서버로 전송 됨
  
- value : 기본 요소의 할당 값

  - 텍스트 - 기본으로 적힌 텍스트 값
  
  - 버튼 - 버튼에 표시되는 테스트

  - submit - value가 없으면 해당 브라우저의 기본 텍스트로 보여짐 (크롬>제출, 파이어폭스>질의보내기, IE>쿼리전송)
  
- name : 데이터가 서버로 전송될 때 할당되는 변수의 이름

  - 해당 변수로 value 값이 전송(name="send" value="전송" → "send=전송")
  
#### \<label>

- 해당 폼 요소에 어떤 값을 넣어야 하는지 라벨을 붙여주는 요소

- 웹 접근성 기준 상, 폼 입력 요소가 있다면 이에 해당하는 label 요소를 반드시 가지고 있어야 함

- for 속성에 해당하는 폼 요소의 id 입력을 연결해서 연결

  - \<label for="id:>아이디 입력</label>
  
  - 잘 매칭이 된다면 마우스로 label 클릭시 폼 입력 요소로 자동 포커싱
  
- id값이 없다면 폼 요소를 label 안에 넣어서 표현할 수 있음

  - \<label>아이디 입력<input type="text" /></label>

  - 국내 스크린 리더 프로그램에서 지원하지 않을 수도 있음
  
  - id와 for 속성을 매칭시켜주는 것이 좋음
  
<br>
<br>

## \<form> 태그 이용해 서블릿에 요청하기

#### \<form> 태그로 서블릿에 요청하는 법

- JSP, ASP, PHP가 나오기 전에는 HTML, CSS, 자바스크립트를 이용해 웹 프로그램을 만들었음

- 서블릿과 JSP는 이러한 HTML, CSS, 자바스크립트 같은 기존의 것을 버리는 것이 아님

  - 기존의 것에 자신의 기능을 추가하여, 즉 서로 연동하여 동작
  
  - 사용자의 요청은 HTML의 \<form> 태그나 자바스크립트로부터 전송받아서 처리

- 여러가지 폼 태그 요소들을 이용하여 입력 서식에 입력 후 전송하면 사용자가 입력한 데이터가 서블릿으로 전송

  - 서블릿은 여러 가지 메서드를 이용해서 전송된 데이터를 받아옴

<br>

#### \<form> 태그의 여러가지 속성

![image](/img/2019-10-24/servlet-basic-001-web1.png)

{% highlight html %}
<form name="/frmLogin" method="get" action="login" encType-"UTF-8">
  아이디 :<input type="text" name="user_id"><br>
  비밀번호 :<input type="password" name="user_pw"><br>
  <input type="submit" value="로그인"> <input type="reset" value="다시입력">
</form>
{% endhighlight %}

- 프로세스

  - 사용자가 자신의 ID와 비밀번호를 입력한 후 로그인을 클릭
  
  - \<form> 태그의 action 속성은 데이터를 전송할 서블릿이나 JSP의 이름을 지정

  - 지정된 이름이 login인 서블릿으로 ID와 비밀번호가 전송

- 로그인을 클릭했을 때 실제로 데이터가 전송되는 과정

  - 실제 데이터는 각 \<input> 태그의 name 속성 값과 쌍으로 전송
  <br>user_id=(값), user_pw=(값)
  
- \<form> 태그와 관련된 여러가지 속성

|**속성**|기능|
|:-:|---|
|**name**|- \<form> 태그의 이름을 지정<br>- 여러 개의 form이 존재하는 경우 구분하는 역할<br>- 자바 스크립트에서 \<form> 태그에 접근할 때 자주 사용|
|**method**|- \<form> 태그 안에서 데이터를 전송할 때 전송 방법을 지정<br>- GET 또는 POST로 지정(미지정시 GET)|
|**action**|- \<form> 태그에서 데이터를 전송할 서블릿이나 JSP를 지정<br>- 서블릿으로 전송할 때는 매핑 이름을 사용|
|**encType**|- \<form> 태그에서 전송할 데이터의 encoding 타입을 지정<br>- 파일을 업로드할 때는 multipart/form-data로 지정|

<br>

## 서블릿에서 클라이언트의 요청을 얻는 방법

- HttpServletRequest 클래스에서 \<form> 태그로 전송된 데이터를 받아오는 데 사용하는 메서드로는 아래 표 같은 것들이 있음

- 가장 많이 사용되는 것 : getParameter() 메서드

  - 여러 개의 값은 배열로 값을 반환하는 : getParametervalue() 메서드

|**메서드**|기능|
|---|---|
|**String getParameter(String name)**|name의 값을 알고 있을 때 그리고 name에 대한 전송된 값을 받아오는데 사용|
|**String[] getParameterValues(String name)**|같은 name에 대해 여러 개의 값을 얻을 때 사용|
|**Enumeration getParameterNames()**|name 값을 모를 때 사용|

<br>

#### HttpServletRequest로 요청 처리 실습 - 한 개의 값을 전송

- 로그인창에서 ID와 비밀번호를 입력 받아 HttpServletRequest로 처리하는 간단한 프로그램

  - 실제 이클립스에서 \<form> 태그로 전송된 정보를 서블릿에서 받아와서 출력하는 과정
  
- 실습1. login.html 만들기

  - pro06이라는 새 프로젝트를 생성 후 servlet-api.jar를 클래스 패스에 지정

  - WebContent 폴더 하위에 사용자 정보를 입력 받을 login.html 생성 후 다음과 같이 작성
  
{% highlight html %}
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>로그인 창</title>
</head>
<body>
  <form name="frmLogin" method="get" action="login" encType="UTF-8)"> <!-- action="login" → 입력된 데이터를 서블릿 매핑명 login인 서블릿으로 전송 -->
	아이디 : <input type="text" name="user_id"><br>                     <!-- textbox에 입력된 id를 user_id로 전송 -->
 	비밀번호 : <input type="password" name="user_pw"><br>               <!-- textbox에 입력된 비밀번호를 user_pw로 전송 -->
 	<input type="submit" value="로그인"> <input type="reset" value="다시입력"> 
  </form>
</body>
</html>
{% endhighlight %}

- 실습2. LoginServlet 클래스 만들기

  - sec01.ex01 패키지 생성
  
  - LoginServlet 클래스 생성, 단 애너테이션 서블릿 매핑 사용, 매핑명 : /login, 메서드 : init/doGet/destroy
  
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
 * Servlet implementation class LoginServlet
 */
@WebServlet("/login")
public class LoginServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init() throws ServletException {
		System.out.println("init 메서드 호출. pro06.sec01.ex01.LoginServlet.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() {
		System.out.println("distroy 메서드 호출. pro06.sec01.ex01.LoginServlet.java");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	// 웹 브라우저에서 전송한 정보를 톰캣 컨테이너가 HttpServletRequest 객체를 생성 후 doGet()으로 넘겨줌
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8"); // 전송될 데이터 UTF-8로 인코딩
		String user_id = request.getParameter("user_id"); // getParameter()를 이용해 <input>태그의 name 속성 값으로 전송된 value를 받아옴
		String user_pw = request.getParameter("user_pw"); // "
    System.out.println("아이디: " + user_id);
		System.out.println("비밀번호: " + user_pw);
	}

}
{% endhighlight %}

- 실습3. 실행하기

  - pro06 프로젝트를 톰캣에 등록하여 실행

  - 웹 브라우저에서 http://localhost:8090/pro06/login.html 호출
  <br>![image](/img/2019-10-24/servlet-basic-002-web2.png)
  
  - ID와 비밀번호 입력한 후 로그인
  <br>![image](/img/2019-10-24/servlet-basic-003-web3.png)
  
  - 콘솔창 확인
  <br>![image](/img/2019-10-24/servlet-basic-004-console1.png)

  - 서블릿이 처리한 후 응답 기능은 아직 미구현 → 웹 브라우저는 출력 없음
  
  - 로그인시 id와 pw가 querystring으로 붙음
  <br>http://localhost:8090/pro06/login?user_id=sun&user_pw=sunny
  
<br>

#### HttpServletRequest로 요청 처리 실습 - 여러 개의 값을 전송

- 위의 예제를 조금 더 발전

  - 하나의 name으로 여러 값을 서블릿으로 요청하는 방법
  
  - 예) 로그인 후 수강과목을 입력하되 한 번에 여러 과목을 입력해서 등록 → 서블릿에서는 각 과목에 대해 여러 개의 값을 처리
  
- 실습1. WebContent 아래에 input.html 생성

{% highlight html %}
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>여러 가지 input 타입 표시 창</title>
</head>
<body>
  <form name="frmInput" method="get" action="input">
  	아이디 :<input type="text" name="user_id"><br>
  	비밀번호:<input type="password" name="user_pw"><br>
  	<input type="checkbox" name="subject" value="java" checked>자바
  	<input type="checkbox" name="subject" value="C언어" checked>C언어
  	<input type="checkbox" name="subject" value="파이썬" checked>파이썬
  	<input type="checkbox" name="subject" value="Go" checked>Go
  	<br><br>
  	<input type="submit" value="전송"> <input type="reset" value="초기화">
  </form>
</body>
</html>
{% endhighlight %}

- 실습2. sec01.ex01.InputServlet 클래스 생성 - servlet mapping /input, init/doGet/destroy

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
 * Servlet implementation class InputServlet
 */
@WebServlet("/input")
public class InputServlet extends HttpServlet
{
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException
	{
		System.out.println("init 메서드 호출. pro06.sec01.ex01.InputServlet.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy()
	{
		System.out.println("distroy 메서드 호출. pro06.sec01.ex01.InputServlet.java");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
	{
		request.setCharacterEncoding("utf-8");
		String user_id = request.getParameter("user_id");
		String user_pw = request.getParameter("user_pw");
		System.out.println("아이디 : " + user_id);
		System.out.println("비밀번호 : " + user_pw);
		String[] subject = request.getParameterValues("subject");
		for (String str : subject)
		{
			System.out.println("선택한 과목 :" + str);
		}
	}

}
{% endhighlight %}

- 실습3. 실행하기(톰캣 재시작)

  - 웹 브라우저에서 http://localhost:8090/pro06/input.html 호출
  <br>![image](/img/2019-10-24/servlet-basic-005-web4.png)
  
  - ID와 비밀번호 입력한 후 로그인
  <br>![image](/img/2019-10-24/servlet-basic-006-web5.png)
  
  - 콘솔창 확인 (선택한 과목이 모두 나오는지 확인)
  <br>![image](/img/2019-10-24/servlet-basic-007-console2.png)

  - 서블릿이 처리한 후 응답 기능은 아직 미구현 → 웹 브라우저는 출력 없음
  
  - 로그인시 id와 pw가 querystring으로 붙음
  <br>http://localhost:8090/pro06/input?user_id=sun&user_pw=&subject=java&subject=%ED%8C%8C%EC%9D%B4%EC%8D%AC

<br>

#### HttpServletRequest로 요청 처리 실습 - getParameterNames() 메서드를 이용

- 전송된 데이터가 많아 일일이 name의 값을 기억하기 힘들 때

  - getParameterNames() 메서드 사용
  
  - 일일히 getParameter() 메서드를 이용하는 것보다 훨씬 편하고, getParameter()는 매개변수를 모두 알아야 함
  
- 실습1. WebContent 아래에 input2.html 생성

  - input1과 내용 거의 동일하고, action만 input2로 바꿔줌
  
{% highlight html %}
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>여러 가지 input 타입 표시 창</title>
</head>
<body>
  <form name="frmInput" method="get" action="input2">
  	아이디 :<input type="text" name="user_id"><br>
  	비밀번호:<input type="password" name="user_pw"><br>
  	<input type="checkbox" name="subject" value="java" checked>자바
  	<input type="checkbox" name="subject" value="C언어" checked>C언어
  	<input type="checkbox" name="subject" value="파이썬" checked>파이썬
  	<input type="checkbox" name="subject" value="Go" checked>Go
  	<br><br>
  	<input type="submit" value="전송"> <input type="reset" value="초기화">
  </form>
</body>
</html>
{% endhighlight %}

- 실습2. sec01.ex01.InputServlet2 클래스 생성 - servlet mapping /input2, init/doGet/destroy

{% highlight java %}
package sec01.ex01;

import java.io.IOException;
import java.util.Enumeration;

import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class InputServlet2
 */
@WebServlet("/input2")
public class InputServlet2 extends HttpServlet 
{
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException 
	{
		System.out.println("init 메서드 호출. pro06.sec01.ex01.InputServlet2.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() 
	{
		System.out.println("distroy 메서드 호출. pro06.sec01.ex01.InputServlet2.java");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		Enumeration enu = request.getParameterNames();  // 저장되어 온 name 속성들만 Enumeration 타입으로 받아옴
		while (enu.hasMoreElements())
		{
			String name = (String) enu.nextElement();
			String[] values = request.getParameterValues(name);
			for (String value : values)
			{
				System.out.println("name = " + name + ", value = " + value);
			}
		}
	
	}

}
{% endhighlight %}

- 실습3. 실행하기(톰캣 재시작)

  - 웹 브라우저에서 http://localhost:8090/pro06/input2.html 호출
  <br>![image](/img/2019-10-24/servlet-basic-008-web6.png)
  
  - ID와 비밀번호 입력한 후 로그인
  <br>![image](/img/2019-10-24/servlet-basic-009-web7.png)
  
  - 콘솔창 확인 (선택한 과목이 모두 나오는지 확인)
  <br>![image](/img/2019-10-24/servlet-basic-010-console3.png)

  - 서블릿이 처리한 후 응답 기능은 아직 미구현 → 웹 브라우저는 출력 없음
  
  - 로그인시 id와 pw가 querystring으로 붙음
  <br>http://localhost:8090/pro06/input?user_id=sun&user_pw=&subject=java&subject=%ED%8C%8C%EC%9D%B4%EC%8D%AC

> Enumeration : 객체들을 한 순간에 하나씩 처리할 수 있는 메서드를 제공하는 클래스
>
>> nextElement() → 다음 요소 반환
>>
>> hasMoreElements() → 요소가 있으면 true, 없으면 false

<br>
<br>

## 서블릿의 응답 처리 방법

#### 서블릿에서 응답을 처리하는 방법

- doGet()이나 doPost() 메서드 안에서 처리

- javax.servlet.http.HttpServletResponse 객체 이용

  - doGet()이나 doPost()의 두 번째 매개변수

- setContentType()을 이용해 클라이언트에게 전송할 데이터 종류(MIME-TYPE) 지정

- 클라이언트(웹브라우저)와 서블릿의 통신은 자바 I/O의 스트림 이용

<br>

#### MIME-TYPE

- Client ↔ Servlet 네트워크를 통해 서로 데이터를 주고 받음

  - 자바 I/O 스트림 클래스의 입출력 기능을 이용하면 쉽게 웹 어플리케이션의 네트워크 기능을 구현할 수 있음
  
- Servlet → Client 때는 어떤 종류의 데이터를 전송하는지 웹 브라우저에게 알려줘야 함

  - 웹 브라우저가 전송 받을 데이터의 종류를 미리 알고 있으면 더 빠르게 처리할 수 있음
  
  - 톰캣 컨테이너에서 미리 설정해놓은 데이터 종류들을 MIME-TYPE(마임 타입)이라 함
  
  > Multipurpose Internet Mail Extensions; 다목적 인터넷 메일 확장(전자 우편의 데이터 형식을 정의한 표준 포맷)
  
- 자바(서블릿)에서 자바 I/O의 스트림 클래스를 이용하여 웹 브라우저로 데이터를 전송할 때는 MIME-TYPE을 설정해서 전송할 데이터의 종류를 지정

- MIME_TYPE으로 지정하는 예

  - HTML 전송 : text/html
  
  - 일반 텍스트 전송 : text/plain
  
  - XML 데이터 전송 : application/xml
  
- 웹 브라우저는 기본적으로 HTML만 인식

  - 서블릿에서는 대부분 text/html을 사용
  
- 그 외 톰캣 컨테이너는 자주 사용하는 데이터 종류를 MIME-TYPE으로 지정

  - 종류를 지정 후 사용하면 됨
  
  - 새로운 종류의 데이터를 지정하고 싶으면 CATALINA_HOME\\conf\\web.xml에 추가
  
<br>

#### HttpServletResponse를 이용한 서비스 응답 실습

- 서블릿이 응답하는 예제

  - 사용자가 입력한 ID와 비밀번호를 전송하면 서블릿이 다시 브라우저에게 응답하는 예제
  
- 서블릿이 클라이언트(웹 브라우저)에 응답하는 과정

  - setContentType()을 이용해 MIME-TYPE을 지정
  
  - 데이터를 출력할 printWriter 객체를 생성
  
  - PrintWrite의 print()나 println()을 이용해 데이터를 출력
  
- 실습1. html과 servlet 만들기

  - WebContent에 login2.html 생성 (login.xml 복사 후 action만 login2로 변경

  - sec02.ex01 패키지에 Login2 클래스 추가, mapping url - /login2, init/doGet/destroy
  <br>전달받은 ID와 비밀번호를 HTML 태그로 만든 후 다시 브라우저로 응답
  
{% highlight java %}
package sec02.ex01;

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
@WebServlet("/login2")
public class LoginServlet2 extends HttpServlet 
{
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException 
	{
		System.out.println("init 메서드 호출. pro06.sec02.ex01.LoginServlet2.java");
	}

	/**
	 * @see Servlet#destroy()
	 */
	public void destroy() 
	{
		System.out.println("init 메서드 호출. pro06.sec02.ex01.LoginServlet2.java");
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
	{
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8"); // 응답할 데이터 종류 설정
		PrintWriter out = response.getWriter();	            // HttpServletResponse 객체의 getWriter()를 이용해 출력 스트림 PrintWriter 객체를 받아옴
		String id = request.getParameter("user_id");
		String pw = request.getParameter("user_pw");
		
		String data = "<html>";			// 브라우저 출력 데이터를 문자열로 연결해서 HTML 태그 만듦
			data += "<body>";
			data += "아이디 : " + id;
			data += "<br>";
			data += "패스워드 : " + pw;
			data += "</body>";
			data += "</html>";
		out.print(data);				// PrintWriter의 print()를 이용해 HTML 태그 문자열을 웹 브라우저로 출력
	}

}
{% endhighlight %}

- 실습2. 실행하기(톰캣 재시작)

  - 웹 브라우저에서 http://localhost:8090/pro06/login2.html 호출 후 id와 pw 입력
  <br>![image](/img/2019-10-24/servlet-basic-011-web8.png)
  
  - ID와 비밀번호 입력한 후 로그인
  <br>![image](/img/2019-10-24/servlet-basic-012-web9.png)
  
<br>

#### 서블릿을 이용한 환율 계산기 예제 실습

- 서블릿을 이용하여 환율 계산기 구현하기

  - 원화를 입력받아 외화로 변환해주는 프로그램
  
- 실습1. sec02.ex01.CalcServlet 생성 - mapping-url : /calc, doGet

  - 최초 매핑명(/calc)으로 요청시 cammnd 값이 null이므로 환율 계산기가 나오도록
  
  - 계산기에서 값을 입력해 요청할 경우 command 값이 calculate이므로 전달된 원화를 이용해 외화로 변환하여 결과 출력

{% highlight java %}
package sec02.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class CalcServlet
 */
@WebServlet("/calc")
public class CalcServlet extends HttpServlet 
{
	private static float USD_RATE = 1124.70F;
	private static float JPY_RATE = 10.113F;
	private static float CNY_RATE = 163.30F;
	private static float GBP_RATE = 1444.35F;
	private static float EUR_RATE = 1295.97F;
	
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html; charset=utf-8");
		PrintWriter pw = response.getWriter();
		String command = request.getParameter("command");    // 수행할 요청을 받아옴
		String won = request.getParameter("won"); 		    // 변환할 원화를 받아옴
		String operator = request.getParameter("operator"); // 변환할 외화 종류를 받아옴
		
		if (command != null && command.equals("calculate"))
		{ // 최초 요청시 command가 null이면 계산기 화면 출력, command 값이 calculate면 계산 결과를 출력
			String result = calculate(Float.parseFloat(won), operator);
			pw.print("<html><font size=10>변환 결과</font><br>");
			pw.print("<font size=10>" + result + "</font><br>");
			pw.print("<a href='/pro06/calc'>환율계산기</a>");
			return;
		}
		
		pw.print("<html><title>환율계산기</title>");
		pw.print("<font size=5>환율계산기</font><br>");
		pw.print("<form name='frmCalc' method='get' action='/pro06/calc' />"); // 환율 정보 입력 후 다시 서블릿 calc로 재요청
		pw.print("원화: <input type='text' name='won' size=10 />"); 
		pw.print("<select name='operator'>"); // 셀렉트 박스에서 선택된 값을 name으로 전달
		pw.print("<option value='dollar'>달러</option>");
		pw.print("<option value='en'>엔화</option>");
		pw.print("<option value='wian'>위안</option>");
		pw.print("<option value='pound'>파운드</option>");
		pw.print("<option value='euro'>유로</option>");
		pw.print("</select>");
		pw.print("<input type='hidden' name='command' value='calculate' />"); // <hidden> 태그를 이용해 계산기에서 서블릿으로 수행할 요청 전달
		pw.print("<input type='submit' value='변환' />");
		pw.print("</form>");
		pw.print("</html>");
		pw.close();
	}

	private static String calculate(float won, String operator)
	{ // 원화를 외화로 환산
		String result = null;
		if (operator.equals("dollar")) 
		{
			result = String.format("%.6f",  won / USD_RATE);
		} else if (operator.equals("en"))
		{
			result = String.format("%.6f",  won / JPY_RATE);
		} else if (operator.equals("wian")) 
		{
			result = String.format("%.6f",  won / CNY_RATE);
		} else if (operator.equals("pound")) 
		{
			result = String.format("%.6f",  won / GBP_RATE);
		} else if (operator.equals("euro"))
		{
			result = String.format("%.6f",  won / EUR_RATE);
		}
		return result;
	}
}
{% endhighlight %}

- 실습2. 실행하기(톰캣 재시작)

  - http://localhost:8090/pro06/calc로 요청 → 원화에 값 입력 → 변환 → 결과확인
  
  ![image](/img/2019-10-24/servlet-basic-013-web10.png)

  ![image](/img/2019-10-24/servlet-basic-014-web11.png)

<br>
<br>

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

- 실습 2. 테스트 

  - doGet 테스트 : 

<br>

## 자바 스크립트로 서블릿에 요청하기

<br>

## 서블릿을 이용한 여러 가지 실습 예제
