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



## DataSource를 이용해 회원 정보 등록하기

<br>

## DataSource를 이용해 회원 정보 삭제하기
