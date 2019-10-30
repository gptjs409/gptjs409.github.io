---
layout: post
title:  "[Java] 서블릿 확장 API 사용하기-(2)"
date:   2019-10-29 13:17:13
author: Choi HyeSun
categories: java
tags:
  - Java
  - 서블릿
  - Servlet
  - 서블릿 확장 API
  - Servlet API
---

## 바인딩

- 포워딩시 GET 방식으로 데이터 전달하는 방법

  - 데이터 양이 적을 때는 이 방법이 편리
  
  - 대량의 상품 정보를 JSP로 전달할 때는 불편
  
  - 대량의 데이터를 공유하거나 전달할 때는 바인딩(binding) 기능 사용
  
- 바인딩(binding)

  - 사전적의미 : 두 개를 하나로 묶는다
  
  - 서블릿에서 다른 서블릿이나 JSP로 대량의 데이터를 공유/전달하고 싶을 때 사용
  
  - 웹 프로그램 실행 시 자원(데이터)을 서블릿 관련 객체에 저장하는 방법
  
  - 주로 HttpServletRequest, HttpSession, ServletContext 객체에서 사용
  
  - 저장된 자원(데이터)은 프로그램 실행 시 서블릿이나 JSP에서 공유하여 사용
  
  - 모델2, 스트럿츠, 스프링 프레임워크로 구현하는 웹 프로그래밍은 바인딩 기능을 이용하여 서블릿이나 JSP간 데이터를 전달하고 공유
  
- 바인딩 관련 기능을 제공하는 여러 가지 메서드

|**관련 메서드**|기능|
|---|---|
|**setAttribute(String name, Object obj)**|자원(데이터)을 각 객체에 바인딩|
|**getAttribute(String name)**|각 객체에 바인딩된 자원(데이터)을 name으로 가져옴|
|**removeAttribute(String name)**|각 객체에 바인딩된 자원(데이터)을 name으로 제거|

<br>

#### HttpServletRequest를 이용한 redirect/refresh/location 포워딩 시 바인딩

- 처음 웹 브라우저에서 서블릿으로 요청하면, 서블릿에서 웹 브라우저에 재 요청할 정보를 주고, 웹 브라우저에서 재 요청

- 즉 첫 번째 요청과 두 번째 요청이 각기 별개의 요청(다른 요청)

- 그러므로 서블릿에서 바인딩한 데이터를 다른 서블릿으로 전송할 수 없음

- GET 방식을 쓰면 되는게 아닌가?

  - 데이터가 보안과 상관 없으며, 양이 적다면 가능

  - 안되는 예) DB에서 조회된 수십 개의 회원정보나 상품정보 전달

<br>

#### HttpServletRequest를 이용한 dispatch 포워딩 시 바인딩

- 첫 번째 서블릿에서 두 번째 서블릿으로 전달되는 request가 브라우저를 거치지 않고 바로 전달

  - 첫 번째 서블릿의 request에 바인딩된 데이터가 그대로 전달
  
- 모델2, 스트럿츠,  스프링 프레임워크로 개발할 때 dispatch 방식으로 바인딩된 데이터를 서블릿이나 JSP로 전달

- 실습1. sec04.ex02 패키지 생성

- 실습2. FirstServlet 생성

{% highlight java %}
package sec04.ex02;

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
@WebServlet("/firstDispatcher2")
public class FirstServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset-utf-8");
		request.setAttribute("address", "서울시 성북구");	// 웹 브라우저의 최초 요청 reqeust에 바인딩
		RequestDispatcher dispatch = request.getRequestDispatcher("secondDispatcher2");
		dispatch.forward(request, response);
	}

}
{% endhighlight %}

- 실습3. SecondServlet 생성

{% highlight java %}
package sec04.ex02;

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
@WebServlet("/secondDispatcher2")
public class SecondServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		String address = (String) request.getAttribute("address");
		
		out.println("<html><body>");
		out.println("주소 : " + address);
		out.println("<br>");
		out.println("dispatch를 이용한 forword 실습");
		out.println("</body></html>");
	}

}
{% endhighlight %}

- 실습4. 브라우저 테스트

  - http://localhost:8090/pro08/firstDispatcher2
  
  ![image](/img/2019-10-29/servlet-expansion-api2-001-web1.png)

<br>
<br>

#### 두 서블릿 간 회원 정보 조회 바인딩 실습

- DB에서 조회된 회원 정보를 화면 기능을 담당하는 서블릿에 전달하여 웹 브라우저에 출력

- 실습1. sec04.ex03 생성, 7장의 MemberDAO와 MemberVO 클래스 복사

- 실습2. MemberServlet.java 생성

{% highlight java %}
package sec02.ex03;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Date;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class MemberServlet
 */
@WebServlet("/member5")
public class MemberServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		doHandle(request, response);
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		doHandle(request, response);		
	}
	
	/**
	 * @see HttpServlet#doHandle(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doHandle(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		MemberDAO dao = new MemberDAO(); // SQL문으로 조회할 MemberDAO 객체 생성
		PrintWriter out = response.getWriter();
		String command = request.getParameter("command"); // command 값(<hidden> 태그에서 넘겨준 값)을 받아옴
		
		if (command != null && command.equals("addMember")) // 회원 가입창에서 전송된 command가 addMember이면 전송된 값들을 받아옴
		{
			String _id = request.getParameter("id");		// 회원 가입창에서 전송된 값들을 얻어와 MemberVO 객체에 저장 후 SQL문을 이용해 전달
			String _pwd = request.getParameter("pwd");
			String _name = request.getParameter("name");
			String _email = request.getParameter("email");
			MemberVO vo = new MemberVO();
			vo.setId(_id);
			vo.setPwd(_pwd);
			vo.setName(_name);
			vo.setEmail(_email);
			dao.addMember(vo);
		} else if (command != null && command.contentEquals("delMember"))
		{
			String id = request.getParameter("id");
			dao.delMember(id); // 주석 해제
		}
						
		List<MemberVO> list = dao.listMembers();    // listMembers() 메서드로 회원 정보 조회
		out.print("<html><body>");
		out.print("<table border=1><tr align='center' bgcolor='lightgreen'>");
		out.print("<td>아이디</td><td>비밀번호</td><td>이름</td><td>이메일</td><td>가입일</td><td>삭제</td></tr>");
		
		for (int i = 0; i < list.size(); i++)
		{
			MemberVO memberVO = (MemberVO) list.get(i);   // 조회한 회원 정보를 for과 <tr> 태그를 이용해 리스트로 출력
			String id = memberVO.getId();
			String pwd = memberVO.getPwd();
			String name = memberVO.getName();
			String email = memberVO.getEmail();
			Date joinDate = memberVO.getJoinDate();
			out.print("<tr><td>" + id + "</td><td>"
			+ pwd + "</td><td>" + name + "</td><td>"
			+ email + "</td><td>" + joinDate + "</td><td>"
			+ "<a href='/pro07/member5?command=delMember&id=" + id + "'> 삭제 </a></td><tr>" );
		}
		out.print("</table>");
		out.print("<a href='/pro07/memberForm2.html'>새 회원 가입하기</a></body></html>");
	}

}
{% endhighlight %}

- 실습3. ViewServlet.java 생성

{% highlight java %}
package sec04.ex03;

import java.io.IOException;
import java.io.PrintWriter;
import java.sql.Date;
import java.util.List;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ViewServlet
 */
@WebServlet("/viewMembers")
public class ViewServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		List membersList = (List) request.getAttribute("membersList");
		out.print("<html><body>");
		out.print("<table border=1><tr align='center' bgcolor='lightgreen'>");
		out.print("<td>ID</td><td>Password</td><td>Name</td><td>E-Mail</td><td>가입일</td></tr>");
		for (int i = 0; i < membersList.size(); i++) {
			MemberVO memberVO = (MemberVO) membersList.get(i);
			String id = memberVO.getId();
			String pwd = memberVO.getPwd();
			String name = memberVO.getName();
			String email = memberVO.getEmail();
			Date joinDate = memberVO.getJoinDate();
			out.print("<tr><td>" + id + "</td><td>" + pwd + "</td><td>" + name + "</td><td>" + email + "</td><td>" + joinDate);
		}
		out.print("</table></body></html>");
	}

}
{% endhighlight %}

- 실습4. 브라우저 테스트

  - http://localhost:8090/pro08/member

  ![image](/img/2019-10-29/servlet-expansion-api2-002-web2.png)
  
- ViewServlet 클래스는 웹 브라우저에서 화면 기능을 담당

  - 이런 기능을 하는 서블릿이 분화하여 발전한 것이 JSP
  
<br>
<br>

## ServletContext와 ServletConfig 사용법

- 서블릿과 더불어 웹 프로그래밍 개발 시 유용한 기능을 제공하는 클래스들이 있음

<br>

#### ServletContext 클래스

- ServletContext 클래스

  - 톰캣 컨테이너 실행시 각 컨텍스트(웹 애플리케이션)마다 한 개의 ServletContext 객체를 생성
  
  - 톰캣 컨테이너가 종료시 ServletContext 객체 소멸
  
  - 웹 어플리케이션이 실행되면서 애플리케이션 전체의 공통 자원이나 정보를 미리 바인딩해서 서블릿들이 공유하여 사용
  
- ServletContext 클래스 특징

  - javax.servlet.ServletContext로 정의
  
  - 서블릿과 컨테이너 간의 연동을 위해 사용
  
  - 컨텍스트(웹 어플리케이션)마다 하나의 ServletContext가 생성
  
  - 서블릿끼리 자원(데이터)을 공유하는데 사용
  
  - 컨테이너 실행시 생성되고 컨테이너 종료시 소멸
  
- ServletContext에서 제공하는 기능

  - 서블릿에서 파일 접근 기능
  
  - 자원 바인딩 기능
  
  - 로그 파일 기능
  
  - 컨텍스트에서 제공하는 설정 정보 제공 기능
  
- ServletContext vs ServletConfig

  - ServletContext는 컨텍스트 당 생성
  
  - ServletConfig는 각 서블릿에 생성

- ServletContext에서 제공하는 여러가지 메서드와 기능

|**메서드**|기능|
|---|---|
|**getAttribute(String name)**|- 주어진 name을 이용해 바인딩된 value를 가져옴<br>-name이 존재하지 않으면 null을 반환|
|**getAttributeNames**|- 바인딩된 속성들의 name을 반환|
|**getContext(String uripath)**|- 지정된 uripath에 해당되는 객체를 반환|
|**getInitParameter(String name)**|- name에 해당하는 매개변수의 초기화 값을 반환<br>- name에 해당하는 매개변수가 존재하지 않으면 null을 반환|
|**getInitParameterName()**|- 컨텍스트의 초기화 관련 매개변수들의 이름을 String 객체가 저장된 Enumeration 타입으로 반환<br>- 매개변수가 존재하지 않으면 null을 반환|
|**getMajorVersion()**|- 서블릿 컨테이너가 지원하는 주요 서블릿 API 버전을 반환|
|**getRealPath(String path)**|- 지정한 path에 해당하는 실제 경로를 반환|
|**getResource(String path)**|- 지정한 path에 해당하는 Resource를 반환|
|**getServerInfo()**|- 현재 서블릿이 실행되고 있는 서블릿 컨테이너의 이름과 버전을 반환|
|**getServletContextName()**|- 해당 애플리케이션의 배치 관리자가 지정한 ServletContext에 대한 해당 웹 어플리케이션의 이름을 반환|
|**log(String msg)**|- 로그 파일에 로그 기록|
|**removeAttribute(String name)**|- 해당 name으로 ServletContext에 바인딩된 객체를 제거|
|**setAttribute(String name, Object object)**|- 해당 name으로 객체를 ServletContext에 바인딩|
|**setInitParameter(String name, String value)**|- 주어진 name으로 value를 컨텍스트 초기화 매개변수로 설정|

<br>

#### ServletContext 바인딩 기능

- 특징
 
  - ServletContext에 바인딩된 데이터는 모든 서블릿(사용자)들이 접근할 수 있음

  - 웹 애플리케이션에서 모든 사용자가 공통으로 사용하는 데이터는 ServletContext에 바인딩해놓고 사용하면 편리

- 실습 1. sec05.ex01 패키지 생성

  - GetServletContext, SetServletContext 서블릿 생성

- 실습 2. SetServletContext.java 수정

{% highlight java %}
package sec05.ex01;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class SetServletContext
 */
@WebServlet("/cset")
public class SetServletContext extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		ServletContext context = getServletContext(); // ServletContext 객체를 가져옴
		List<Object> member = new ArrayList<Object>();
		member.add("세종대왕");
		member.add(40);
		context.setAttribute("member", member);	  // ServletContext 객체에 데이터를 바인딩
		out.print("<html><body>");
		out.print("세종대왕과 40 설정");
		out.print("</body></html>");
	}
}
{% endhighlight %}

- 실습 3. GetServletContext.java 수정

{% highlight java %}
package sec05.ex01;

import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class GetServletContext
 */
@WebServlet("/cget")
public class GetServletContext extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		ServletContext context = getServletContext();	 // ServletContext 객체를 가져옴
		List<Object> member = (ArrayList<Object>) context.getAttribute("member");	// member로 이전에 바인딩한 회원 정보를 가져옴
		String name = (String) member.get(0);
		int age = (Integer) member.get(1);
		out.print("<html><body>");
		out.print(name + "<br>");
		out.print(age + "<br>");
		out.print("</body></html>");
		
	}

}
{% endhighlight %}

- 실습 4. 웹브라우저로 http://localhost:8090/pro08/cset 호출

  ![image](/img/2019-10-29/servlet-expansion-api2-003-web3.png)


- 실습 5. 웹브라우저로 http://localhost:8090/pro08/cget 호출

  ![image](/img/2019-10-29/servlet-expansion-api2-004-web4.png)

<br>

#### ServletContext의 매개변수 설정 기능

- 메뉴

  - 대부분의 웹 애플리케이션에서 공통으로 사용하는 기능
  
  - web.xml에 설정해놓고 프로그램 시작 시 초기화할 때 가져와서 사용하면 편리
  
  - 새로운 메뉴 항목이 생성되거나 기본 메뉴 항목을 추가, 삭제할 때도 쉽게 수정할 수 있음
  
  - ContextServlet 객체를 통해 접근하면 모든 웹 브라우저에서 공유하면서 접근할 
  
- 실습 1. web.xml 파일

{% highlight html %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
	<context-param> <!-- <context-param>태그 안에 다시 <param-name>과 <param-value>로 초기 값 설정 -->
		<param-name>menu_member</param-name>
		<param-value>회원등록  회원조회 회원수정</param-value>
	</context-param>
	<context-param>
		<param-name>menu_order</param-name>
		<param-value>주문조회  주문등록 주문수정 주문취소</param-value>
	</context-param>
	<context-param>
		<param-name>menu_goods</param-name>
		<param-value>상품조회  상품등록 상품수정 상품삭제</param-value>
	</context-param>
</web-app>
{% endhighlight %}

- 실습 2. sec05.ex02.ContextParamServlet.java 작성

{% highlight java %}
package sec05.ex02;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ContextParamServlet
 */
@WebServlet("/initMenu")
public class ContextParamServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		ServletContext context = getServletContext();					// ServletContext 객체를 가져옴
		String menu_member = context.getInitParameter("menu_member");	// web.xml의 <param-name> 태그의 이름으로 <param-value> 태그의 값인 메뉴 이름들을 받아옴
		String menu_order = context.getInitParameter("menu_order");
		String menu_goods = context.getInitParameter("menu_goods");
		
		out.print("<html><body>");
		out.print("<table border=1 cellspacing=0><tr>메뉴 이름</tr>");
		out.print("<tr><td>" + menu_member + "</td></tr>");
		out.print("<tr><td>" + menu_order + "</td></tr>");
		out.print("<tr><td>" + menu_goods + "</td></tr>");
		out.print("</tr></table></body></html>");
	}

}
{% endhighlight %}

- 실습 3. 크롬 브라우저 테스트 http://localhost:8090/pro08/initMenu
 
  ![image](/img/2019-10-29/servlet-expansion-api2-005-web5.png)
 
- 실습 4. 인터넷 브라우저 테스트 http://localhost:8090/pro08/initMenu

  ![image](/img/2019-10-29/servlet-expansion-api2-006-web6.png)

<br>

#### ServletContext의 파일 입출력 기능

- 실습 1. WebContent/WEB-INF에 bin 레파지토리 생성

- 실습 2. bin 레파지토리 하위에 init.txt 파일 생성

{% highlight html %}
회원등록  회원조회 회원수정, 주문조회  주문수정 주문취소,  상품조회  상품등록 상품수정 상품삭제
{% endhighlight %}

- 실습 3. sec05.ex03.ContextFileServlet 클래스

  - init.txt에서 데이터를 읽어와 출력하는 기능을 구현

{% highlight java %}
package sec05.ex03;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.util.StringTokenizer;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class ContextFileServlet
 */
@WebServlet("/cfile")
public class ContextFileServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=euc-kr");
		PrintWriter out = response.getWriter();
		ServletContext context = getServletContext();
		InputStream is = context.getResourceAsStream("/WEB-INF/bin/init.txt");
		BufferedReader buffer = new BufferedReader(new InputStreamReader(is));
		
		String menu = null;
		String menu_member = null;
		String menu_order = null;
		String menu_goods = null;
		while ((menu = buffer.readLine()) != null) {
			StringTokenizer tokens = new StringTokenizer(menu, ",");
			menu_member = tokens.nextToken();
			menu_order = tokens.nextToken();
			menu_goods = tokens.nextToken();
		}
		out.print("<html><body>");
		out.print(menu_member + "<br>");
		out.print(menu_order + "<br>");
		out.print(menu_goods + "<br>");
		out.print("</body></html>");
		out.close();
	}

}
{% endhighlight %}

- 실습 4. 브라우저에서 http://localhost:8090/pro08/cfile 호출

  - 파일의 메뉴 항목을 읽어와 브라우저로 출력함

  ![image](/img/2019-10-29/servlet-expansion-api2-007-web7.png)

<br>

#### ServletConfig

- ServletConfig

  - 각 Servlet 객체에 대해 생성
  
  - ServletConfig 인터페이스를 GenericServlet 클래스가 실제로 구현

  - javax.servlet 패키지에 인터페이스로 선언
  
  - 서블릿에 대한 여러가지 기능을 제공
  
  - 각 서블릿에서만 접근할 수 있으며, 공유는 불가능
  
  - 서블릿과 동일하게 생성되고, 서블릿이 소멸되면 같이 소멸

- 제공하는 기능

  - ServletContext 객체를 얻는 기능(앞에서 사용해봄)
  
  - 서블릿에 대한 초기화 작업 기능

<br>

#### 서블릿에 대한 초기화 작업 기능

- 서블릿에서 사용할 설정 정보를 읽어 들여와 초기화하는 기능

  - @WebServlet 애너테이션 이용하는 방법
  
  - web.xml에 설정하는 방법

<br>

#### 서블릿에 대한 초기화 작업 기능(1) - @WebServlet 애너테이션

- @WebServlet의 중요한 구성 요소들

|**요소**|설명|
|---|---|
|**urlPatterns**|웹 브라우저에서 서블릿 요청시 사용하는 매핑 이름|
|**name**|서블릿 이름|
|**loadOnStartup**|컨테이너 실행 시 서블릿이 로드되는 순서 지정|
|**initParams**|@WebinitParam 애너테이션을 이용해 매개변수를 추가하는 기능|
|**description**|서블릿에 대한 설명|

<br>

#### 서블릿에 대한 초기화 작업 기능(1) - 편리하게 이클립스에서 서블릿을 생성할 때 @WebServlet의 값들을 설정

- 실습1. sec06.ex01 패키지 생성 > 마우스 우클릭 > New > Servlet

- 실습2. 클래스명 InitParamServlet > Next

- 실습3. Initialization parameters:의 Add... > 값 입력 > OK > 추가된 그리드 확인

  - Name : email, Value : admin@web.com
  
  - Name : tel, Value : 010-1111-2222
  
- 실습4. URL mappings:의 Remove > 기본값 제거

- 실습5. URL mappings:의 Add.. > 값 입력 > OK > 그리드 확인 > Next

  - Pattern : sInit
  
  - Pattern : sInit2
  
- 실습 6. Inherited abstract methods + doGet 체크 > Finish

{% highlight java %}
package sec06.ex01;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class InitParamServlet
 */
@WebServlet(
		urlPatterns = { 
				"/sInit", 
				"/sInit2"
		}, 
		initParams = { 
				@WebInitParam(name = "email", value = "admin@web.com"), 
				@WebInitParam(name = "tel", value = "010-1111-2222")
		})
public class InitParamServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		response.getWriter().append("Served at: ").append(request.getContextPath());
	}

}
{% endhighlight %}

- 실습7. InitparamServlet 클래스 작성

  - getInitParameter() 메서드에 애너에티션으로 매개변수를 설정할 때 지정한 email과 name을 인자로 전달하여 각 값을 가져옴
  
{% highlight java %}
package sec06.ex01;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class InitParamServlet
 */
@WebServlet(
		urlPatterns = { 					// urlPatterns를 이용해 매핑명을 여러 개 설정할 수 있음
				"/sInit", 		
				"/sInit2"
		}, 
		initParams = { 
				@WebInitParam(name = "email", value = "admin@web.com"), 	// @WebInitParam을 이용해 여러 개의 매개변수를 설정할 수 있음
				@WebInitParam(name = "tel", value = "010-1111-2222")
		})
public class InitParamServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		String email = getInitParameter("email");	// 설정한 매개변수의 name으로 값을 가져옴
		String tel = getInitParameter("tel");
		out.print("<html><body>");
		out.print("<table><tr>");
		out.print("<td>email: </td><td>" + email + "</td></tr>");
		out.print("<tr><td>휴대전화: </td><td>" + tel + "</td>");
		out.print("</tr></table></body></html>");
	}

}
{% endhighlight %}

- 실습8. 브라우저 호출

  - http://localhost:8090/pro08/sInit
  
  - http://localhost:8090/pro08/sInit2
  
  ![image](/img/2019-10-29/servlet-expansion-api2-008-web8.png)

  ![image](/img/2019-10-29/servlet-expansion-api2-009-web9.png)

<br>

#### 서블릿에 대한 초기화 작업 기능(2) - web.xml에 설정

- 지금은 잘 이용하지 않음

{% highlight html %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app>
    <servlet>
        <servlet-name>sinit</servlet-name>
	<servlet-class>sec06.ex01.initParamServlet</servlet-class>
    	    <init-param>       <!-- <init-param> 태그 안에 매개변수를 설정 -->
	        <param-name>email</param-name>
		<param-value>admin@jweb.com<</param-value>
	    </init-param>
	    <init-param>
	        <param-name>tel</param-name>
		<param-value>010-1111-2222</param-value>
	    </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>sinit</servlet-name>
        <url-pattern>/first</url-pattern>
    </servlet-mapping>
</web-app>
{% endhighlight %}

<br>
<br>

## load-on-startup 기능 사용하기

- 서블릿

  - 브라우저에서 최초 요청시 init() 메서드를 실행한 후 메모리에 로드되어 기능을 수행
  
  - 최초 요청에 대해서는 실행시간이 길어질 수 밖에 없음
  
  - 이런 단점을 보완하기 위해 이용하는 기능이 load-on-startup
  
- load-on-startup의 특징

  - 톰캣 컨테이너가 실행되면서 미리 서블릿을 실행
  
  - 지정한 숫자가 0보다 크면 톰캣 컨테이너가 실행되면서 서블릿이 초기화
  
  - 지정한 숫자는 우선순위를 의미하며 작은 숫자부터 먼저 초기화됨
  
- load-on-startup을 구현하는 방법 두 가지

  - 애너테이션을 이용
  
  - web.xml에 설정

<br>

#### 애너테이션을 이용하는 방법

- 특징

  - 우선순위는 양의 정수로 지정하며 숫자가 작으면 우선순위가 높으므로 먼저 실행
  
  - 톰캣 실행시 init() 메서드를 호출하면 getInitParameter() 메서드를 이용해 web.xml의 메뉴 정보를 읽은 후 다시 ServletContext 객체에 setAttribute() 메서드로 바인딩
  
  - 브라우저에서 요청하면 web.xml이 아니라 ServletContext 객체에서 메뉴 항목을 가져온 후 출력 에서 ㅇ릭어 들여와 출력하는 것보다 빨리 

- 실습 1. sec06.ex02 패키지 생성 > 마우스 우클릭 > New > Servlet

- 실습 2. 클래스명 : LoadAppConfig > Next > 

- 실습 3. Name과 URL mapping을 loadConfig로 변경 > Next

  - Name을 바꾸면 URL mapping도 동일하게 바뀜
  
- 실습 4. Inherited abstract methods, init, doGet 체크 > Finish

{% highlight java %}
package sec06.ex02;

import java.io.IOException;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoadAppConfig
 */
@WebServlet(name = "loadConfig", urlPatterns = { "/loadConfig" })
public class LoadAppConfig extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException {
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

- 실습 5. LoadAppConfig 클래스 작성

  - 애너테이션으로 설정한 매개변수에 loadOnStartup 속성을 추가한 후 우선순위를 1로 설정
  
{% highlight java %}
package sec06.ex02;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletConfig;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class LoadAppConfig
 */
@WebServlet(name = "loadConfig", urlPatterns = { "/loadConfig" }, loadOnStartup=1)	// loadOnStartup 속성을 추가하고, 우선순위를 1로 설정
public class LoadAppConfig extends HttpServlet {
	private static final long serialVersionUID = 1L;
	private ServletContext context;		// 변수 context를 멤버 변수로 선언

	/**
	 * @see Servlet#init(ServletConfig)
	 */
	public void init(ServletConfig config) throws ServletException {
		System.out.println("pro08.sec06.ex02.LoadAppConfig의 init 메서드 호출");
		context = config.getServletContext();	// init() 메서드에서 ServletContext 객체를 받음
		String menu_member = context.getInitParameter("menu_member");
		String menu_order = context.getInitParameter("menu_order");
		String menu_goods = context.getInitParameter("menu_goods");
		context.setAttribute("menu_member", menu_member);
		context.setAttribute("menu_order", menu_order);
		context.setAttribute("menu_goods", menu_goods);
		
	}

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		// ServletContext context = getServletContext();   doGet() 메서드 호출 시 ServletContext 객체를 얻는 부분 주석처리
		String menu_member = (String) context.getAttribute("menu_member");	// 브라우저에 요청 시 ServletContext 객체의 바인딩된 메뉴 항목을 가져옴
		String menu_order = (String) context.getAttribute("menu_order");
		String menu_goods = (String) context.getAttribute("menu_goods");
		out.print("<html><body>");
		out.print("<table border=1 cellspacing=0><tr>메뉴이름</tr>");
		out.print("<tr><td>" + menu_member + "</td></tr>");
		out.print("<tr><td>" + menu_order + "</td></tr>");
		out.print("<tr><td>" + menu_goods + "</td></tr>");
		out.print("</tr></table></body></html>");
	}

}
{% endhighlight %}
  
- 실습 6. tomcat 재기동

  - 서블릿의 init() 메서드를 호출하므로 로그에 메시지가 출력됨
  
  ![image](/img/2019-10-29/servlet-expansion-api2-010-web10.png)
  
- 실습 7. http://localhost:8090/pro08/loadConfig로 브라우저 호출

  - 기다리지 않고 바로 공통 메뉴를 

  ![image](/img/2019-10-29/servlet-expansion-api2-011-web11.png)

<br>

#### web.xml에 설정하는 방법

- 애너테이션 이용 전엔 web.xml에 설정해서 사용했었었음

- 실행 결과는 애너테이션과 동일

- web.xml

  - \<servlet-name> 태그의 값은 반드시 서블릿을 생성할 때 name으로 지정한 값으로 설정해야 함
  
  - \<load-on-startup>은 우선순위를 설정
  
{% highlight html %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
<servlet>
    <servlet-name>loadConfig</servlet-name>                  <!-- 애너테이션으로 서블릿 생성 시 name 속성 값으로 설정 -->
    <servlet-class>sec06.ex02.LoadAppConfig</servlet-class>  <!-- 패키지명까지 포함한 서블릿 클래스 이름 설정 -->
    <load-on-startup>1</load-on-startup>                     <!-- 우선순위 설정 -->
</servlet>
<context-param>
    <param-name>menu_member</param-name>
    <param-value>회원등록  회원조회 회원수정</param-value>
</context-param>
<context-param>
    <param-name>menu_order</param-name>
    <param-value>주문조회  주문등록 주문수정 주문취소</param-value>
</context-param>
<context-param>
    <param-name>menu_goods</param-name>
    <param-value>상품조회  상품등록 상품수정 상품삭제</param-value>
</context-param>
</web-app>
{% endhighlight %}
