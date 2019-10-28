---
layout: post
title:  "[Java] 서블릿 비즈니스 로직 처리-(3)"
date:   2019-10-28 08:31:59
author: Choi HyeSun
categories: java
tags:
  - Java
  - 서블릿
  - Servlet
  - 서블릿 비즈니스 로직
  - Servlet Business Logic
  - DB연동
---

## DataSource를 이용해 회원 정보 등록하기

- sec02.ex02에 (sec02.ex01의) MemberDAO, MemberServlet, MemberVO 복사 후 Servlet 매핑룰 /member4로 변경

<br>

#### 회원 가입 창 작성

- WebContent에 memberForm.html이라는 회원 가입창 작성

- \<hidden> 태그를 이용해 회원 가입창에서 새 회원 등록 요청을 서블릿에 전달

{% highlight html %}
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>회원 가입창</title>
		<script type="text/javascript">
			function fn_sendMember()
			{
				var frmMember = document.frmMember; // 자바스크립트에서 <form> 태그의 name으로 접근해 입력한 값들을 얻어옴
				var id = frmMember.id.value;
				var pwd = frmMember.pwd.value;
				var name = frmMember.name.value;
				var email = frmMember.email.value;
				
				if (id.length == 0 || id =="" )
				{
					alert("ID는 필수입니다.");				
				}
				else if (pwd.length == 0 || pwd == "" ) 
				{
					alert("Password는 필수입니다.")
				}
				else if (name.length == 0 || email == "")
				{
					alert("E-Mail은 필수입니다.")	
				}
				else
				{
					frmMember.method = "post";		// 전송방식 Post
					frmMember.action = "member4";	// 서블릿 매핑명 member4
					frmMember.submit();				// 서블릿으로 전송
				}
			}
		</script>
	</head>
	<body>
		<form name = "frmMember">
			<table>
				<th>회원 가입창</th>
				<tr>
					<td>ID</td>
					<td><input type="text" name="id"></td>		<!-- 입력한 ID를 서블릿으로 전송 -->
				</tr>
				<tr>
					<td>Password</td>
					<td><input type="password" name="pwd"></td> <!-- 입력한 Password를 서블릿으로 전송 -->
				</tr>
				<tr>
					<td>이름</td>
					<td><input type="text" name="name"></td>    <!-- 입력한 이름을 서블릿으로 전송 -->
				</tr>
				<tr>
					<td>E-Mail</td>
					<td><input type="text" name="email"></td>	<!-- 입력한 email을 서블릿으로 전송 -->
				</tr>
			</table>
			<input type="button" value="가입하기" onclick="fn_sendMember()">
			<input type="reset" value="다시 입력">
			<input type="hidden" name="command" value="addMember" /> <!-- <hidden> 태그를 이용해 서블릿에게 회원 등록임을 알림 -->
		</form>
	</body>
</html>
{% endhighlight %}

<br>

#### MemberServlet 클래스 변경

- command 값을 받아와 addMember이면(/<hidden> 태그를 통해) 같이 전송된 회원 정보를 받아옴

- 회원 정보를 MemberVO 객체에 설정한 후 MemberDAO의 메서드로 전달해 SQL문을 이용해 테이블에 추가

{% highlight java %}
package sec02.ex02;

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
@WebServlet("/member4")
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
			//dao.delMember(id);
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
			out.print("<tr><td>" + id + "</td><td>" + pwd + "</td><td>" + name + "</td><td>" + email + "</td><td>" + joinDate + "</td><td>" + "<a href='/pro07/member4?command=delMember&id=" + id + "'> 삭제 </a></td><tr>" );
		}
		out.print("</table>");
		out.print("<a herf='/pro07/memberForm.html'>새 회원 가입하기</a></body></html>");
	}

}
{% endhighlight %}

<br>

#### PrepareStatement에서 insert문 사용하기

- PrepareStatement의 insert문은 회원 정보를 저장하기 위해 ?(물음표)를 사용

- ?는 id, pwd, name, age에 순서대로 대응

- 각 ?에 대응한느 값을 지정하기 위해 PrepareStatement의 setter를 이용

- setter() 메서드의 첫 번째 인자는 '?'의 순서 지정

- '?'는 1부터 시작함

- insert, delete, update문은 executeUpdate()문을 사용

<br>

#### MemberDAO 클래스 변경하기

- PrepareStatement의 ?를 이용하기

{% highlight java %}
package sec02.ex02;

import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.sql.DataSource;

public class MemberDAO 
{
	
	private Connection con;
	private PreparedStatement pstmt;
	private DataSource dataFactory; 						// import javax.sql.DataSource; 필요

	public MemberDAO() 
	{
		try
		{
			Context ctx = new InitialContext();				// import javax.naming.InitialContext; 필요, JDNI에 접근하기 위해 기본 경로(java:/comp/env) 지정
			Context envContext = (Context) ctx.lookup("java:/comp/env");
			dataFactory = (DataSource) envContext.lookup("jdbc/oracle"); // 톰캣 context.xml에 설정한 name값인 jdbc/oracle을 이용해 미리 연결한 DataSource 받아오기
		} catch (Exception e)
		{
			e.printStackTrace();
		}
	}
	
	public List<MemberVO> listMembers()
	{
		List<MemberVO> list = new ArrayList<MemberVO>();
		try
		{
			//connDB();		// 더이상 안 씀
			con = dataFactory.getConnection();				// DataSource를 이용해 DB 연동
			String query = "select * from t_member";
			System.out.println("preparedStatement: " + query);
			pstmt = con.prepareStatement(query); // prepareStatement() 메서드에 SQL문을 전달해 PrepareStatement 객체를 생성
			ResultSet rs = pstmt.executeQuery(); // 미리 설정한 SQL문을 executeQuery()로 실행
			while (rs.next())
			{
				String id = rs.getString("id");
				String pwd = rs.getString("pwd");
				String name = rs.getString("name");
				String email = rs.getString("email");
				Date joinDate = rs.getDate("joinDate");
				MemberVO vo = new MemberVO();
				vo.setId(id);
				vo.setPwd(pwd);
				vo.setName(name);
				vo.setEmail(email);
				vo.setJoinDate(joinDate);
				list.add(vo);
			}
			rs.close();
			pstmt.close();
			con.close();
		} catch (Exception e)
		{
			e.printStackTrace();
		}
		return list;
	}
	
	public void addMember(MemberVO memberVO)
	{
		try
		{
			con = dataFactory.getConnection();     // DataSource를 이용해 데이터베이스와 연결
			String id = memberVO.getId();					  // 테이블에 저장할 회원 번호를 받아옴
			String pwd = memberVO.getPwd();
			String name = memberVO.getName();
			String email = memberVO.getEmail();
			
			String query = "insert into t_member";			  // insert 문을 문자열로 생성
			query += " (id,pwd,name,email)";
			query += " values(?,?,?,?)";
			System.out.println("preparedStatement: " + query);
			pstmt = con.prepareStatement(query);
			pstmt.setString(1, id);							  // insert문의 각 ?에 순서대로 회원정보를 대입
			pstmt.setString(2, pwd);
			pstmt.setString(3, name);
			pstmt.setString(4, email);
			pstmt.executeUpdate();							  // 회원정보를 테이블에 추가
			pstmt.close();
		} catch (Exception e)
		{
			e.printStackTrace();
		}
	}
}
{% endhighlight %}

<br>

#### 회원 정보 등록해보기

- 웹 브라우저 : http://localhost:8090/pro07/memberForm.html

  ![image](/img/2019-10-28/servlet-business-logic3-001-web1.png)

- 입력 결과 - 정상 출력

  ![image](/img/2019-10-28/servlet-business-logic3-001-web1.png)
  
<br>
<br>

## DataSource를 이용해 회원 정보 삭제하기

#### HTML 파일 생성

- WebContent 하위의 memberForm.html 복사 후 memberForm2.html 생성

  - action(매핑룰)만 member5로 변환
  
<br>

#### DAO, VO, Servlet 생성

- sec02.ex03 패키지 생성

- sec02.ex02의 DAO, VO, Servlet 복사

- Servlet 매핑룰만 /member5로 변경

<br>

#### MemberServlet 클래스에 회원 정보 삭제 기능 추가

- 삭제 기능  및 결과 UI 변경

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
			+ "<a href='/pro  07/member5?command=delMember&id=" + id + "'> 삭제 </a></td><tr>" );
    }
		out.print("</table>");
		out.print("<a herf='/pro07/memberForm2.html'>새 회원 가입하기</a></body></html>");
	}

}
{% endhighlight %}

<br>

#### MemberDAO 클래스 회원 정보 삭제 기능 추가

- delMember 기능 추가

{% highlight java %}
package sec02.ex03;

import java.sql.Connection;
import java.sql.Date;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.sql.DataSource;

public class MemberDAO 
{
	
	private Connection con;
	private PreparedStatement pstmt;
	private DataSource dataFactory; 						// import javax.sql.DataSource; 필요

	public MemberDAO() 
	{
		try
		{
			Context ctx = new InitialContext();				// import javax.naming.InitialContext; 필요, JDNI에 접근하기 위해 기본 경로(java:/comp/env) 지정
			Context envContext = (Context) ctx.lookup("java:/comp/env");
			dataFactory = (DataSource) envContext.lookup("jdbc/oracle"); // 톰캣 context.xml에 설정한 name값인 jdbc/oracle을 이용해 미리 연결한 DataSource 받아오기
		} catch (Exception e)
		{
			e.printStackTrace();
		}
	}
	
	public List<MemberVO> listMembers()
	{
		List<MemberVO> list = new ArrayList<MemberVO>();
		try
		{
			//connDB();		// 더이상 안 씀
			con = dataFactory.getConnection();				// DataSource를 이용해 DB 연동
			String query = "select * from t_member";
			System.out.println("preparedStatement: " + query);
			pstmt = con.prepareStatement(query); // prepareStatement() 메서드에 SQL문을 전달해 PrepareStatement 객체를 생성
			ResultSet rs = pstmt.executeQuery(); // 미리 설정한 SQL문을 executeQuery()로 실행
			while (rs.next())
			{
				String id = rs.getString("id");
				String pwd = rs.getString("pwd");
				String name = rs.getString("name");
				String email = rs.getString("email");
				Date joinDate = rs.getDate("joinDate");
				MemberVO vo = new MemberVO();
				vo.setId(id);
				vo.setPwd(pwd);
				vo.setName(name);
				vo.setEmail(email);
				vo.setJoinDate(joinDate);
				list.add(vo);
			}
			rs.close();
			pstmt.close();
			con.close();
		} catch (Exception e)
		{
			e.printStackTrace();
		}
		return list;
	}
	
	public void addMember(MemberVO memberVO)
	{
		try
		{
			con = dataFactory.getConnection();     // DataSource를 이용해 데이터베이스와 연결
			String id = memberVO.getId();					  // 테이블에 저장할 회원 번호를 받아옴
			String pwd = memberVO.getPwd();
			String name = memberVO.getName();
			String email = memberVO.getEmail();
			
			String query = "insert into t_member";			  // insert 문을 문자열로 생성
			query += " (id,pwd,name,email)";
			query += " values(?,?,?,?)";
			System.out.println("preparedStatement: " + query);
			pstmt = con.prepareStatement(query);
			pstmt.setString(1, id);							  // insert문의 각 ?에 순서대로 회원정보를 대입
			pstmt.setString(2, pwd);
			pstmt.setString(3, name);
			pstmt.setString(4, email);
			pstmt.executeUpdate();							  // 회원정보를 테이블에 추가
			pstmt.close();
		} catch (Exception e)
		{
			e.printStackTrace();
		}
	}
	
	public void delMember(String id) 
	{
		try
		{
			con = dataFactory.getConnection();
			String query = "delete from t_member" + " where id=?";    // delete 문을 문자열로 생성
			System.out.println("preparedStatement: " + query);
			pstmt = con.prepareStatement(query);
			pstmt.setString(1,  id);								  // 매개변수가 id 달랑 하나니까 바로 사용이 됨!
			pstmt.executeUpdate();
			pstmt.close();
		} catch (Exception e)
		{
			e.printStackTrace();
		}
	}
}
{% endhighlight %}

<br>

#### 실행1. 회원가입창에서 abc 계정 가입

- http://localhost:8090/pro07/memberForm2.html

![image](/img/2019-10-28/servlet-business-logic3-003-web3.png)

- 결과

![image](/img/2019-10-28/servlet-business-logic3-004-web4.png)

<br>

#### 실행2. 가입완료 페이지에서 abc 계정 삭제 클릭

- 결과창에서 삭제 클릭 - 삭제됨

![image](/img/2019-10-28/servlet-business-logic3-005-web5.png)
