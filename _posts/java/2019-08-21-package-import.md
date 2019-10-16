---
layout: post
title:  "[Java] 패키지(package)와 임포트(import)"
date:   2019-08-21 20:09:59
author: Choi HyeSun
categories: java
tags:
  - Java
  - 패키지
  - package
  - 임포트
  - import
---

## 패키지(package) - 정의

- 하나의 소스파일에는 첫 번째 문장으로 단 한 번의 패키지 선언만을 허용

- 물리적으로 클래스파일(.class)을 포함하는 하나의 디렉토리

- 모든 클래스는 반드시 하나의 패키지에 속해야 함

- 점'.'을 구분자로 계층구조로 구성할 수 있음

  - 예) java.lang 패키지
  <br>lang 패키지 : java 패키지의 하위 패키지
  
- 클래스 또는 인터페이스를 포함할 수 있으며, 관련된 클래스끼리 그룹 단위로 묶어놓음

  - 클래스를 효율적으로 관리할 수 있음

  - 서로 다른 패키지에 같은 이름의 클래스가 존재할 수 있음
  <br>: 자신만의 패키지 체계를 유지하면서 다른 개발자가 개발한 클래스 라이브러리의 클래스와 이름이 충돌하는 것을 피할 수 있음

- 클래스의 실제 이름(full name)

  - 패키지를 포함한 것

  - 예) String 클래스의 실제 이름 : java.lang.String
  <br>java.lang 패키지에 속한 String 클래스
  <br>rt.jar 패키지에 압축되어 있으며 java의 서브디렉토리인 lang에 속한 String.class파일

<br>
<br>

## 패키지 - 선언

- package 패키지명;

- 주석과 공백을 제외한 첫 번째 문장

- 한 소스 파일에 단 한 번만 선언할 수 있음

- 대소문자를 모두 허용 → 클래스명과 구분하기 위하여 소문자로 하는 것을 원칙

- unnamed package(이름 없는 패키지)

  - package를 선언하지 않으면 자동으로 이름없는 패키지에 속함

  - 패키지를 지정하지 않는 모든 클래스는 같은 패키지에 속함

- 컴파일시 '-d' 옵션
<br>javac -d (해당 패키지의 루트 디렉토리 경로) (소스파일).java

  - 예1) javac -d . Sunny.java (이곳이 root 디렉토리)

  - 예2) javac -d c:\sunny\ Sunny.java (c:\sunny가 root 디렉토리)

  - 소스파일에 지정된 경로를 통해 패키지의 위치를 찾아서 클래스파일을 생성
  <br>해당 디렉토리가 없다면, 디렉토리를 만들어줌

- 컴파일시 '-cp' 옵션

  - 클래스패스(classpath)
  <br>: 컴파일러(javac.exe)나 JVM이 클래스의 위치를 찾는데 사용되는 경로

  - 일시적으로 클래스 패스 지정
  
<br>
<br>

## 임포트(import) - 정의

- 사용하고자 하는 클래스의 패키지를 미리 명시해주면, 소스코드에 사용되는 클래스이름에서 패키지명을 생략할 수 있음

  - 원래는 다른 패키지의 클래스를 사용하려면 패키지명이 포함된 클래스 이름(클래스의 실제 이름)을 사용해야함 > 이게 불편하기 때문에 사용

- 컴파일러에게 소스파일에 사용될 클래스의 패키지에 대한 정보를 제공

  - 컴파일시에 컴파일러가 import문을 통해 소스파일에 사용된 클래스들의 패키지를 알아내고, 모든 클래스이름 앞에 패키지명을 붙여줌

  - 즉, 실행시간에 영향을 미치진 않고, 컴파일 시간에 영향을 조금 미침

- 이클립스

  - Ctrl + Shift + o 를 누르면 자동으로 import를 추가해주는 편리한 기능

<br>
<br>

## 임포트(import) - 선언

- 일반적인 소스 파일 구성

  - package문
  <br>import문
  <br>클래스 선언

- import 패키지명.클래스명; vs import 패키지명.*;

  - 컴파일 시간의 차이지 실행시간의 차이는 전혀 없음
  
  - \*의 경우 하위 패키지는 포함하지 않고 현재의 패키지만 전체 확인
  
<br>
<br>

## 정적 임포트(static import) - 정의

- import와 차이점 

  - import : 클래스 패키지명을 생략

  - static import : static 멤버를 호출할 때 클래스 이름 생략

- 특정 클래스의 static 멤버를 자주 사용할 때 편리

<br>
<br>

## 정적 임포트(static import) - 선언

- 패키지명.클래스명.정적메서드명
<br>해당 클래스의 해당 static 메서드
<br>패키지명.클래스명.*
<br>해당 클래스의 모든 static 메서드

- 예시) Math 클래스 => static 멤버들
<br>import static java.lang.math.* // math 클래스의 모든 static 메서드
<br>import static java.lang.math.random // math 클래스의 random static 메서드
<br>=> Math.random() → random() 사용
