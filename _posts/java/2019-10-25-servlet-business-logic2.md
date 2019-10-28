---
layout: post
title:  "[Java] 서블릿 비즈니스 로직 처리-(2)"
date:   2019-10-26 20:25:34
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

## DataSource를 이용해 데이터베이스 연동하기

- DB에 연결하여 작업하는 방식

  - \[Java] 서블릿 비즈니스 로직 처리-(1)에서 헀음
  
  - 문제점 : DB 연결에 시간이 많이 걸림
  
  - 문제점을 극복하기 위해 커넥션풀이 등장함
  
- 커넥션 풀(Connection Pool)

  - 웹 애플리케이션이 실행됨과 동시에 연동할 데이터베이스와의 연결을 미리 설정
  
  - 필요할 때마다 미리 연결해 놓은 상태를 이용해 빠르게 데이터베이스와 연동하여 작업
  
  - 미리 데이터베이스와 연결시킨 상태를 유지하는 기술을 커넥션 풀이라 함

<br>

#### 커넥션풀 동작 과정

- 동작 과정

  - 톰캣 컨테이너를 실행한 후 응용 프로그램을 실행

  - 톰캣 컨테이너 실행시 ConenctionPool 객체 생성

  - 생성된 ConnectionPool 객체는 DBMS와 연결

  - DB와의 연동 작업이 필요할 경우 응용 프로그램은 ConnectionPool에서 제공하는 메서드를 호출하여 연동

- 톰캣 컨테이너는 자체적으로 ConnectionPool 기능을 제공

  - 실행시 톰캣은 설정 파일에 설정된 DB 정보를 이용해 미리 DB와 연결하여 ConnectionPool 객체를 생성
  
  - 애플리케이션이 DB와 연동할 일이 생기면 ConnectionPool 객체의 메서드를 호출해 빠르게 연동하며 작업

<br>

#### JNDI

- 웹 애플리케이션에서 Connection Pool 객체를 구현할 때

  - Java SE에서 제공하는 javax.sql.DataSource 클래스 이용
  
- 웹 애플리케이션 실행시 톰캣이 만들어놓은 ConnectionPool 객체 접근시

  - JNDI 이용
 
- JNDI(Java Naming and Directory Interface)란?

  - 필요한 자원을 키/값(key/value) 쌍으로 저장한 후 필요할 때 키를 이용해 값을 얻는 방법
  
  - 접근할 자원에 미리 키를 지정한 후, 애플리케이션이 실행중일 때 이 키를 이용해 자원에 접근하여 작업
  
- JNDI 사용 예

  - 웹 브라우저에서 name/value 쌍으로 전송한 후 서블릿에서 getParameter(name)으로 값을 가져올 때
  
  - 해시맵(HashMap)이나 해시테이블(HashTable)에 키/값으로 저장한 후 키를 이용해 값을 가져올 때
  
  - 웹 브라우저에서 도메인 네임으로 DNS 서버에 요청할 경우 도메인 네임에 대한 IP 주소를 가져올 때
  
- 톰캣 컨테이너가 ConnectionPool 객체를 생성하면?

  - 이 객체에 대한 JNDI 이름(key)을 미리 설정
  
  - 웹 애플리케이션에서 DB 연동 작업을 할 때 설정해놓은 JNDI 이름(key)으로 접근하여 작업

<br>

#### 톰캣의 DataSource 설정 및 사용 방법

- 톰캣 ConnectionPool 설정 과정

  - JDBC 드라이버를 /WEB-INF/lib 폴더에 설치
  
  - ConnectionPool 기능 관련 jar 파일을 /WEB-INF/lib 폴더에 설치
  
  - CATALINA_HOME/content.xml에 Connection 객체 생성시 연결할 데이터베이스 정보를 JNDI로 설정
  
  - DAO 클래스에서 데이터베이스와 연동시 미리 설정한 JNDI라는 이름으로 데이터베이스와 연결해서 작업
  
- 톰캣에서 ConnectionPool 기능을 사용하려면?

  - 이 기능을 제공하는 DBCP 라이브러리를 다운받아야 함(jar 파일로 제공됨)
  
  - 톰캣 9에도 DHCP 라이브러리가 있음(tomcat-dbcp.jar)

<br>

#### 이클립스에서 톰캣 DataSource 설정

- JDBC 드라이버(ojdbc6.jar)

  - pro07\\WebContent\\WEB-INF\\lib\\에 위치

- ConnectionPool 관련 jar파일(tomcat-dbcp.jar) 

  - pro07\\WebContent\\WEB-INF\\lib\\에 ojdbc6.jar와 같이 위치
  
- context.xml

  - 이클립스에서 생성한 톰캣 서버의 설정 파일
  
  - Servers\\Tomcat v9.0 Server at localhost-config\\에 위치
  
- context.xml 파일 설정하기전 ConnectionPool로 연결할 DB 속성 알아보기

  - 다른 속성은 고정적으로 사용하며, 프로그래머는 주로 drvierClassName, user, password, url만 변경하여 설정

|**속성**|설명|
|---|---|
|**name**|DataSource에 대한 JNDI 이름|
|**auth**|인증 주체|
|**driverClassName**|연결할 DB 종류에 따른 드라이브 클래스명|
|**factory**|연결할 DB 종류에 따른 ConntectionPool 생성 클래스명|
|**maxActive**|동시에 최대로 DB에 연결할 수 있는 Connection 수|
|**maxIdle**|동시에 idle 상태로 대기할 수 있는 최대 수|
|**maxWait**|새로운 연결이 생길 때 까지 기다릴 수 있는 최대 시간|
|**user**|DB 접속 ID|
|**password**|DB접속 Password|
|**type**|DB 종류별 DataSource|
|**url**|접속할 DB 주소와 포트번호 및 SID|

<br>

- context.xml에 설정하는 예
{% highlight html %}
<Resource
    name="jdbc/oracle"
    auth="Container"
    type="javax.sql.DataSource"
    driverClassName="oracle.jdbc.OracleDriver"
    url="jdbc:oracle:thin:@localhost:1521:XE"
    username="sun"
    password="1234"
    maxActive="50"
    maxWait="-1" />
{% endhighlight %}

- context.xml에 설정하기

{% highlight html %}
<?xml version="1.0" encoding="UTF-8"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one or more
  contributor license agreements.  See the NOTICE file distributed with
  this work for additional information regarding copyright ownership.
  The ASF licenses this file to You under the Apache License, Version 2.0
  (the "License"); you may not use this file except in compliance with
  the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
--><!-- The contents of this file will be loaded for each web application --><Context>

    <!-- Default set of monitored resources. If one of these changes, the    -->
    <!-- web application will be reloaded.                                   -->
    <WatchedResource>WEB-INF/web.xml</WatchedResource>
    <WatchedResource>WEB-INF/tomcat-web.xml</WatchedResource>
    <WatchedResource>${catalina.base}/conf/web.xml</WatchedResource>

    <!-- Uncomment this to disable session persistence across Tomcat restarts -->
    <!--
    <Manager pathname="" />
    -->
    <Resource
    name="jdbc/oracle"
    auth="Container"
    type="javax.sql.DataSource"
    driverClassName="oracle.jdbc.OracleDriver"
    url="jdbc:oracle:thin:@localhost:1521:XE"
    username="sun"
    password="1234"
    maxActive="50"
    maxWait="-1" />
</Context>
{% endhighlight %}

<br>

#### 톰캣의 DataSource로 연동해 회원 조회 

- sec02.ex01 패키지 생성 후 MemberDAO, MemberServlet, MemberVO 복사+붙여넣기

- MemberServlet 매핑명을 /member3으로 변경

- MemberDAO 클래스 수정

  - DataSource를 이용해 DB랑 연결하는 MemberDAO 클래스 수정
  
  - 앞에서 사용한 connDB() 메서드는 주석처리
  
  - 생성자에서 톰캣 실행 시 톰캣에서 미리 생성한 DataSource를 name값인 jdbc/oracle을 이용해 미리 받아오기
  
  - 서블릿에서 listMembers() 메서드를  호출하면, getConnection() 메서드를 호출하여 DataSource에 접근 후 DB와의 연동 작업 수행

{% highlight java %}
package sec02.ex01;

import java.sql.Connection;
import java.sql.Date;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;
import java.util.List;

import javax.naming.Context;
import javax.naming.InitialContext;
import javax.sql.DataSource;

public class MemberDAO 
{
	
	//private static final String driver = "oracle.jdbc.driver.OracleDriver";       // 더이상 안 씀
	//private static final String url = "jdbc:oracle:thin:@localhost:1521:XE";		// 더이상 안 씀
	//private static final String user = "sun";										// 더이상 안 씀
	//private static final String pwd = "1234";										// 더이상 안 씀
	
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
	
	/* 안쓰므로 주석처리
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
	*/
}
{% endhighlight %}

- 호출 테스트

  - 결과는 같지만 Connection 풀을 이용함
  
  - http://localhost:8090/pro07/member3.html
  
  ![image](/img/2019-10-25/servlet-business-logic2-001-web1.png)
