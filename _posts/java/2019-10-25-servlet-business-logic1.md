---
layout: post
title:  "[Java] 서블릿 비즈니스 로직 처리-(1)"
date:   2019-10-25 20:25:34
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

## 서블릿의 비즈니스 로직 처리 방법

- 서블릿 비즈니스 처리 작업 ★ 서블릿의 핵심

  - 서블릿이 클라이언트로부터 요청을 받으면 그 요청에 대해 작업을 수행하는 것을 의미
  
- 웹 프로그램에서 대부분의 비즈니스 처리 작업 
 
  - 대부분 데이터베이스 연동 관련 작업
  
  - 그 외 다른 서버와 연동해서 데이터를 얻는 작업도 수행
  
- 서블릿의 비즈니스 작업 예

  - 웹 사이트 회원 등록 요청 처리
  
  - 웹 사이트 로그인 요청 처리
  
  - 쇼핑몰 상품 주문 처리
  
- 서블릿의 비즈니스 처리 과정

  - 클라이언트로부터 요청
  
  - 데이터베이스 연동과 같은 비즈니스 로직 처리
  
  - 처리 결과를 클라이언트에게 리턴

<br>
<br>

## 서블릿에 데이터베이스 연동하기

- 서블릿은 SQL문을 사용해 데이터베이스에 접근하여 작업

  - 이 과정에서 DAO와 VO 클래스가 사용됨

> 데이터베이스의 SQL문을 어느정도 알아놔야 진행 가능
>
> DAO나 VO 클래스도 알아두기

<br>

#### 서블릿으로 회원 정보 테이블의 회원 정보 조회

- 조회 단계

  - 웹 브라우저가 서블릿에게 회원 정보 요청

  - MemberServlet은 요청을 받은 후 memberDAO 객체를 생성(create)하여 listMembers() 메서드를 호출

  - listMembers()에서 다시 connDB() 메서드를 호출하여 데이터베이스와 연결 후 SQL문을 실행해 회원 정보를 조회

  - 조회된 회원 정보를 MemberVO 속성에 설정 후 다시 ArrayList에 저장

  - ArrayList를 다시 메서드를 호출한 MemberServlet으로 반환한 후 ArrayList의 MemberVO를 차례데로 가져와 회원 정보를 HTML 태그의 문자열로 만듦

  - 만들어진 HTML 태그를 웹 브라우저로 전송해서 회원 정보를 출력

- 회원 정보를 저장하는 t_member 테이블의 구성
  
|NO|속성명|컬럼명|자료형|크기|유일키여부|NULL여부|키|기본값|
|---|---|---|---|---|---|---|---|---|
|1|ID|id|varchar2|10|Y|N|기본키||
|2|비밀번호|pwd|varchar2|10||N|||
|3|이름|name|varchar2|50|||||
|4|이메일|email|varchar2|50||N|||
|5|가입일자|joinDate|date|||N|||

- DB 생성하기

  - SQL Developer 실행 (회원테이블과 정보를 입력하려고) 및 접속
  
  - 다음 내역 ▶
  
{% highlight sql %}
--회원 테이블 생성
create table t_member(
  id varchar2(10) primary key,
  pwd varchar2(10),
  name varchar2(50),
  email varchar2(50),
  joinDate date default sysdate --명시적으로 추가하지 않을 경우 현재 시각 입력
);


-- 회원 정보 추가
insert into t_member
values ('hong', '1212', '홍길동', 'hong@gmail.com', sysdate);

insert into t_member
values ('lee', '1212', '이순신', 'lee@test.com', sysdate);

insert into t_member
values ('kim', '1212', '김유신', 'kim@jweb.com', sysdate);

commit; -- SQL Developer에서 테이블에 회원정보를 추가 후 반드시 커밋(commit)을 해줘야 영구적 반영이 됨

select * from t_member;
{% endhighlight %}

<br>

- pro07 프로젝트 생성

  - 오라클 DB와 연동하는데 필요한 드라이버인 ojdbc6.jar를 프로젝트의 /WebContent/WEB-INF/lib 폴더에 복붙 (다운 [LINK](https://www.oracle.com/database/technologies/jdbcdriver-ucp-downloads.html))
  
  > ojdbc6.jar → JDK6, 7, 8과 함께 사용
  >
  > 찾기 쉽게 일단 /tomcat9/lib에 같이 넣어둠 ㅎㅎ
  
  - sec01.ex01 패키지 생성 (회원 조회와 관련된 자바 클래스 파일인 MemberDAO, MemberServlet, MemberVO 클래스가 들어갈 것)

- MemberServlet 클래스 생성

  - 브라우저의 요청을 받음
  
{% highlight java %}
package sec01.ex01;

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
@WebServlet("/member")
public class MemberServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException 
	{
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		MemberDAO dao = new MemberDAO(); // SQL문으로 조회할 MemberDAO 객체 생성
		List<MemberVO> list = dao.listMembers();    // listMembers() 메서드로 회원 정보 조회
		
		out.print("<html><body>");
		out.print("<table border=1><tr align='center' bgcolor='lightgreen'>");
		out.print("<td>아이디</td><td>비밀번호</td><td>이름</td><td>이메일</td><td>가입일</td></tr>");
		
		for (int i = 0; i < list.size(); i++)
		{
			MemberVO memberVO = (MemberVO) list.get(i);   // 조회한 회원 정보를 for과 <tr> 태그를 이용해 리스트로 출력
			String id = memberVO.getId();
			String pwd = memberVO.getPwd();
			String name = memberVO.getName();
			String email = memberVO.getEmail();
			Date joinDate = memberVO.getJoinDate();
			out.print("<tr><td>" + id + "</td><td>" + pwd + "</td><td>" + name + "</td><td>" + email + "</td><td>" + joinDate + "</td><tr>");
		}
		out.print("</table></body></html>");
	}

}
{% endhighlight %}

- MemberDAO 클래스 생성

  - 회원 정보 조회 SQL문을 실행하여 조회한 레코드의 컬럼값을 다시 MemberVO 객체의 속성에 설정한 후 ArrayList에 저장하고 호출한 곳으로 반환
  
{% highlight java %}
package sec01.ex01;

import java.sql.Connection;
import java.sql.Date;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.List;

public class MemberDAO {
	private static final String driver = "oracle.jdbc.driver.OracleDriver";
	private static final String url = "jdbc:oracle:thin:@localhost:1521:XE";
	private static final String user = "sun";
	private static final String pwd = "1234";
	private Connection con;
	private Statement stmt;
	
	
	public List<MemberVO> listMembers()
	{
		List<MemberVO> list = new ArrayList<MemberVO>();
		try
		{
			connDB();
			String query = "select * from t_member";
			System.out.println(query);
			ResultSet rs = stmt.executeQuery(query);
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
			stmt.close();
			con.close();
		} catch (Exception e)
		{
			e.printStackTrace();
		}
		return list;
	}
	
	
	private void connDB()
	{
		try
		{
			Class.forName(driver);
			System.out.println("Oracle 드라이버 로딩 성공");
			con = DriverManager.getConnection(url, user, pwd);
			System.out.println("Connection 생성 성공");
			stmt = con.createStatement();
			System.out.println("Statement 생성 성공");
		}	catch (Exception e)
		{
			e.printStackTrace();
		}
	}
	
}

{% endhighlight %}

- MemberVO 클래스 생성

  - 값을 전달하는 데 사용하는 VO(Value Object) 클래스

> TIP1. Getter Setter 만들어주는 거
>
>> 우클릭 → Source → Generate Getters and Setters
>
> TIP2. 생성자 만들어주기
>
>> 우클릭 → Source → Generate Constructor using fields
  
{% highlight java %}
package sec01.ex01;

import java.sql.Date;

public class MemberVO 

{
	private String id;
	private String pwd;
	private String name;
	private String email;
	private Date joinDate;
	
	public MemberVO() {
		System.out.println("MemberVO 생성자 호출");		
	}

	public String getId() {
		return id;
	}

	public void setId(String id) {
		this.id = id;
	}

	public String getPwd() {
		return pwd;
	}

	public void setPwd(String pwd) {
		this.pwd = pwd;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public Date getJoinDate() {
		return joinDate;
	}

	public void setJoinDate(Date joinDate) {
		this.joinDate = joinDate;
	}
}
{% endhighlight %}

- 실행

![image](/img/2019-10-25/servlet-business-logic1-001-web1.png)

<br>

#### PrepareStatement를 이용한 회원 정보 실습

- Statement 문의 단점

  - 연동할 때마다 DBMS에서 다시 SQL문을 컴파일해야 하므로 속도가 느림
    
- PrepareStatement 인터페이스

  - SQL문을 미리 컴파일해서 재사용
  
  - Statement 인터페이스보다 훨씬 빠르게 DB작업 수행 가능
  
  - 데이터베이스와 연동할 때 / 빠른 반복 처리가 필요할 때 사용
  
- PrepareStatement 인터페이스 특징

  - Statement 인터페이스를 상속 → 메서드를 그대로 사용
  
  - Statement : DBMS에 전달하는 SQL문 → 단순한 문자열 → DBMS가 스스로 이해할 수 있도록 컴파일 후 실행
  <br>PrepareStatement : 컴파일된 SQL문을 DBMS에 전달 → 성능 향상
  
  - 실행하려는 SQL문에 "?"를 넣을 수 있음
  <br>"?"값만 바꾸어 손쉽게 설정할 수 있어 Statement문보다 SQL문 작성하기가 더 간단함
  
- 실습

  - 기존 작성 3개 복붙 (to sec01.ex02)
  
  - Servlet 매핑을 /member2로 변경
  
  - MemberDAO 클래스를 PrepareStatement를 이용하여 연동하도록 작성

{% highlight java %}
package sec01.ex02;

import java.sql.Connection;
import java.sql.Date;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

public class MemberDAO {
	private static final String driver = "oracle.jdbc.driver.OracleDriver";
	private static final String url = "jdbc:oracle:thin:@localhost:1521:XE";
	private static final String user = "sun";
	private static final String pwd = "1234";
	private Connection con;
	private PreparedStatement pstmt;
	
	
	public List<MemberVO> listMembers()
	{
		List<MemberVO> list = new ArrayList<MemberVO>();
		try
		{
			connDB();
			String query = "select * from t_member";
			System.out.println("prepareStatement: " + query);
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
	
	
	private void connDB()
	{
		try
		{
			Class.forName(driver);
			System.out.println("Oracle 드라이버 로딩 성공");
			con = DriverManager.getConnection(url, user, pwd);
			System.out.println("Connection 생성 성공");
		}	catch (Exception e)
		{
			e.printStackTrace();
		}
	}
	
}
{% endhighlight %}

![image](/img/2019-10-25/servlet-business-logic1-002-web2.png)

- 차이

  - 눈으로 보면 Statement를 사용했을 때와 PrepareStatement 사용 결과가 같음
  
  - DB와 연동할 경우 수행 속도가 좀 더 


<br>
