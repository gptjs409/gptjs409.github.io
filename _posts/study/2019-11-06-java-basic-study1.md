---
layout: post
title:  "[자바기본스터디] 2019-11 자바기본스터디 1주차"
date:   2019-11-06 20:08:23
author: Choi HyeSun
categories: study
tags:
  - 자바스터디
---


## 차례

#### Chapter 7 객체지향 프로그래밍 II : 스터디 1주차

|중분류|소분류|
|---|---|
|3. package와 import|3.1 패키지(package)<br>3.2 패키지의 선언<br>3.3 import문<br>3.4 import문의 선언|
|4. 제어자(modifier)|4.1 제어자란?<br>4.2 static - 클래스의, 공통적인<br>4.3 final - 마지막의, 변경될 수 없는<br>4.4 생성자를 이용한 final 멤버변수 초기화<br>4.5 abstract - 추상의, 미완성의<br>4.6 접근 제어자(access modifier)<br>4.7 접근 제어자를 이용한 캡슐화<br>4.8 생성자의 접근 제어자<br>4.9 제어자(modifier)의 조합|
|5. 다형성(polymorphism)|5.1 다형성이란?<br>5.2 참조변수의 형변환<br>5.3 instanceof연산자<br>5.4 참조변수와 인스턴스의 연결<br>5.5 매개변수의 다형성<br>5.6 여러 종류의 객체를 하나의 배열로 다루기|
|6. 추상클래스(abstract class)|6.1 추상클래스란?<br>6.2 추상메서드(abstract method)<br>6.3 추상클래스 작성|
|7. 인터페이스(interface)|7.1 인터페이스란?<br>7.2 인터페이스의 작성<br>7.3 인터페이스의 상속<br>7.4 인터페이스의 구현<br>7.5 인터페이스를 이용한 다중상속<br>7.6 인터페이스를 이용한 다형성<br>7.7 인터페이스의 장점<br>7.8 인터페이스의 이해|

<br>

#### Chapter 10 내부 클래스 : 스터디 1주차(챕터 순서 유의!!)

|중분류|소분류|
|---|---|
|1. 내부 클래스(inner class)란?||
|2. 내부 클래스의 종류와 특징||
|3. 내부 클래스의 선언||
|4. 내부 클래스의 제어자와 접근성||
|5. 익명 클래스(anonymous class)||

<br>

#### Chapter 8 예외처리(Exception Handling) : 스터디 1주차(챕터 순서 유의!!)

|중분류|소분류|
|---|---|
|1. 예외처리(exception handling)|1.1 프로그램 오류<br>1.2 예외처리의 정의와 목적<br>1.3 예외처리구문 - try-catch<br>1.4 try-catch문에서의 흐름<br>1.5 예외 발생시키기<br>.6 예외 클래스의 계층구조<br>1.7 예외의 발생과 catch블럭<br>1.8 finally블럭<br>1.9 메서드에 예외 선언하기<br>1.10 예외 되던지기(exception re-throwing)<br>1.11 사용자정의 예외 만들기|

<br>
<br>

## 참여인원

2명

<br>
<br>

## 스터디 준비

![image](/img/2019-11-06/java-basic-study-001-room1.jpg)

![image](/img/2019-11-06/java-basic-study-002-room2.jpg)

<br>
<br>

## 스터디 진행 방법

키워드를 화이트보드에 적은 후, 해당 키워드의 의미 또는 연관된 부분을 한 번씩 번갈아가며 마인드맵 형식으로 그려가며 설명

<br>
<br>

## 스터디 중 궁금했던 부분 정리

#### abstract class vs interface

- abstract class

  - 미완성 클래스
  
  - 단일 상속 가능

  - 목적 : 상속받는 객체(클래스 / Object)에서 기능을 확장시키는 것 (즉, 클래스의 내용과 관련이 있음)

- interface

  - 클래스는 아님
  
  - 다중 구현 가능
  
  - 목적 : 구현하는 객체(클래스 / Object)의 같은 행위(동작)을 보장하기 위함 (즉, 클래스의 내용과는 관련이 없음)

<br>

#### Java 8에서 추가된 interface 기능

- Java 7까지의 Inteface의 메서드는 추상 메서드만 가능했었음(실행블록 없이)

- Java 8에서 다음 기능들이 추가됨

  - 디폴트 메서드 / 정적 메서드 → 추상메서드와 경계가 모호해짐
  
  - 인터페이스의 구현체가 존재하지 않더라도 익명 구현 객체 사용 가능(람다식 이용)

<br>

#### private class에는 접근이 불가능할까 import가 불가능할까

- 바로 아래 참고, private class는 없음

<br>

#### 제어자를 사용할 수 있는 부분

- 클래스

  - public, (default), final, abstract

- 메서드

  - 모든 접근제어자, final, static, abstract

- 필드

  - 모든 접근제어자, final, static

- 지역변수

  - final

- 초기화블럭

  - static

<br>

#### 제어자의 조합에서 가능한 부분

- 클래스에서 abstract과 final는 같이 사용할 수 없음(의미 충돌)

  - abstract 클래스 : 상속해서 사용해야 함
  
  - final 클래스 : 상속받을 수 없음
  
- 메서드에서 abstract과 static는 같이 사용할 수 없음(의미충돌)

  - abstract 메서드 : 선언부만 있고 구현부가 없음

  - static 메서드 : 인스턴스 생성 없이 바로 사용할 수 있어야 함  
  
- 메서드에서 abstract와 private는 같이 사용할 수 없음(의미충돌)

  - abstract 메서드 : 다른 클래스에서 상속해서 오버라이딩해서 사용해야 함
  
  - private 메서드 : 자식 클래스에서 접근할 수 없음
  
- 메서드에서 final과 private는 중복해서 쓰지 않아도 됨(의미 동일)

  - final 클래스 : 수정 불가 / 오버라이딩 불가
  
  - private 클래스 : 접근 불가 / 오버라이딩 불가

<br>

#### 접근제어자의 접근 차이

- public

  - 아래 영영 외의 

  - 자식 클래스의 멤버

  - 같은 패키지의 멤버
  
  - 같은 클래스의 멤버

- protected

  - 자식 클래스의 멤버

  - 같은 패키지의 멤버
  
  - 같은 클래스의 멤버

- (default)

  - 같은 패키지의 멤버

  - 같은 클래스의 멤버

- private

  - 같은 클래스의 멤버
  
<br>

#### Interface의 멤버의 접근제어자

- Interface 멤버의 접근제어자는 무조건 public

- 생략해도 (default)가 아닌 public!
