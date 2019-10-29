---
layout: post
title:  "[Java] 서블릿 확장 API 사용하기-(2)"
date:   2019-10-28 13:27:91
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
  
  ![image](/img/2019-10-28/servlet-expansion-api2-001-web1.png)

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

  ![image](/img/2019-10-28/servlet-expansion-api2-002-web2.png)
  
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
|**getAttributeNames**|- 

<br>

#### ServletContext 바인딩 기능

<br>

#### ServletContext의 매개변수 설정 기능

<br>

#### ServletContext의 파일 입출력 기능

<br>

#### ServletConfig

<br>

#### 애너테이션을 이용한 서블릿 설정

<br>
<br>

## load-on-startup 기능 사용하기

<br>

#### 애너테이션을 이용하는 방법

<br>

#### web.xml에 설정하는 방법
