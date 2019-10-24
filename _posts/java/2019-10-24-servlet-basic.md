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
|**boolean**|**authenticate(HttpServletResponse response)|현재 요청한 사용자가 ServletContext 객체에 대한 인증을 하기 위한 컨테이너 로그인 메커니즘 사용|
|**String**|**changeSessionId()**|현재 요청과 연관된 현재 세션의 id를 변경하여 새 세션 id를 반환|
|**String**|**getContextPath()**|요청한 컨텍스트를 가리키는 URI를 반환|
|**Cookie[]**|getCookies()**|클라이언트가 현재의 요청과 함께 보낸 쿠키 객체들에 대한 배열을 반환|
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
|**Collection\<String>**|**getHeaderNames()|현재 응답이 헤더에 포함된 name을 얻어옴|
|**void**|sendRedirect(String location)|클라이언트에게 리다이렉트(redirect) 응답을 보낸 후 특정 URL로 다시 요청하게 함|

<br>

## \<form> 태그 이용해 서블릿에 요청하기

<br>

## 서블릿에서 클라이언트의 요청을 얻는 방법

<br>

## 서블릿의 응답 처리 방법

<br>

## 웹 브라우저에서 서블릿으로 데이터 전송하기

<br>

## GET 방식과 POST 방식 요청 동시에 처리하기

<br>

## 자바 스크립트로 서블릿에 요청하기

<br>

## 서블릿을 이용한 여러 가지 실습 예제
