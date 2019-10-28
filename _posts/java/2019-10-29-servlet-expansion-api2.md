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

- 

<br>
<br>

#### 두 서블릿 간 회원 정보 조회 바인딩 실습

<br>
<br>

## ServletContext와 ServletConfig 사용법

#### ServletContext 클래스

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
