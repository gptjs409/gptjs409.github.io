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
  


#### 

<br>
