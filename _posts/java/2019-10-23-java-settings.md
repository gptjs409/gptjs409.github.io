---
layout: post
title:  "[Java] 개발환경 설정"
date:   2019-10-23 18:54:42
author: Choi HyeSun
categories: java
tags:
  - Java
  - JDK 설치
  - 이클립스 설치
  - Oracle 설치
  - Tomcat 설치
  - exERD 설치
  - Tomcat 포트 
---

## JDK 설치

#### JDK 다운로드

- JDK 다운로드 사이트 [LINK](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

- Java SE 8u221 설치

  - 사이트 로그인해야 다운로드 받아짐
  
<br>

#### 환경변수 셋팅

- JAVA_HOME 등록

  - C:\\Program Files\\Java\\jdk1.8.0\_221
  
- PATH 추가
 
  - \%JAVA\_HOME\%\\bin
  
<br>

#### 버전 확인

{% highlight bash %}
C:\Users\User>java -version
java version "1.8.0_221"
Java(TM) SE Runtime Environment (build 1.8.0_221-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.221-b11, mixed mode)

C:\Users\User>javac -version
javac 1.8.0_221
{% endhighlight %}

<br>
<br>

## 이클립스 설치하기

- 사이트 [LINK](https://www.eclipse.org/downloads/)

- 다운로드 버전 : Eclipse IDE 2019‑09

- 다운로드 경로 : D:\\Program Files\\eclipse

- workspace 경로 : D:\\eclipse-workspace

<br>
<br>

## 톰캣(WAS) 

#### 설치하기
- 사이트 [LINK](https://tomcat.apache.org/download-90.cgi)

- 다운로드 버전 : Tomcat 9, 64-bit Windows zip

- 놓고 싶은 경로에 압축 풀기

  - D:\\ 아래 tomcat9으로 풀었음(D:\\tomcat9)
  
<br>

#### 포트가 겹치므로(오라클도 8080을 씀) 포트 바꿔주기

- D:\\tomcat9\\conf\\server.xml 파일 수정

- <Connector port="8080" → <Connector port="8090" 
 
<br>
<br>

## JAVA EE API 문서 즐겨찾기에 추가

- API 문서 [LINK](https://javaee.github.io/javaee-spec/javadocs/)

<br>
<br>

## Oracle DBMS 설치

#### Oracle 설치

- 18c 설치 사이트 [LINK](https://www.oracle.com/database/technologies/xe-downloads.html)

- 11g 설치 사이트 [LINK](https://www.oracle.com/database/technologies/xe-prior-releases.html)

- 교육 환경과 동일하게 11g로 설치

  - 설치시 Oracle 계정 필요
  
  - Oracle Database 11gR2 Express Edition for Windows x64
  <br>→ 설치 마법사
   
  - 경로 : ~~D:\\Program Files\\oraclexe 화이트스페이스 별로 안좋대서..~~ D:\\oraclexe
   
  - Specify Database Passwords → 그냥 잘 쓰는 비밀번호 넣어줬음
   
- Expression Edition : 무료

<br>

#### SQLPLUS로 사용할 일반 계정 생성하기

- 요즘은 SQL Developer 같은 윈도우 기반의 프로그램이 많지만, 예전에는 sqlplus같은 것으로 작업했었음

  - 그래서 한 번 해보기로

- sqlplus 실행

  - cmd창(설치 후 새로 열어줘야 정상 동작) → sqlplus (입력)

{% highlight bash %}
C:\Users\User>sqlplus

SQL*Plus: Release 11.2.0.2.0 Production on 수 10월 23 19:20:27 2019

Copyright (c) 1982, 2014, Oracle.  All rights reserved.

Enter user-name: system  # 기본 계정
Enter password:          # 설치시 셋팅한 비밀번호

Connected to:
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production

SQL>
{% endhighlight %}

- 생성할 계정 : sun / 비밀번호 : 1234

  - resource와 connect는 오라클에서 미리 사용자가 일반적인 작업을 할 수 있도록 권한을 묶어 만들어 놓은 권한 룰
  
{% highlight bash %}
SQL> create user sun identified by 1234;

User created.

SQL> grant resource, connect to sun;

Grant succeeded.
{% endhighlight %}

- system 계정 로그아웃(exit)

{% highlight bash %}
SQL> exit
Disconnected from Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production
{% endhighlight %}

- sun 계정 로그인

{% highlight bash %}
C:\Users\User>sqlplus

SQL*Plus: Release 11.2.0.2.0 Production on 수 10월 23 19:24:53 2019

Copyright (c) 1982, 2014, Oracle.  All rights reserved.

Enter user-name: sun
Enter password:

Connected to:
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production

SQL>
{% endhighlight %}

<br>

#### 재부팅시 자동 시작 제거

- 재부팅시 Oracle이 자동 시작되게 기본 셋팅

  - 컴퓨터 성능에 영향을 줄 수 있으므로 자동 시작 제거

- 시작 > 서비스 검색 > 서비스 앱

- OracleServiceXE / OracleXETNSListener

  - 속성 > 시작 유형 : 수동으로 변경
  
<br>

#### SQL Developer 설치

- 오라클과 연동하는 여러가지 도구 중 무료로 편리하게 사용할 수 있는 프로그램

- 다운로드 검색 [LINK](https://www.oracle.com/downloads/)

  - SQL Developer 다운로드 [LINK](https://www.oracle.com/tools/downloads/sqldev-v192-downloads.html)
  
- 버전 : Windows 64-bit with JDK 8 included	

  - 다운로드시 오라클 계정 필요 (오라클 프로그램들은 다 필요한 듯)
  
- 압축파일 받아서 압축 풀자마자 실행파일 나옴

- sqldeveloper.exe 실행

  - 이전 버전에서 설정을 임포트..(뭐시기)... → 이전에 안썼기 때문에 걍 N

- 연결하기

  - 좌측 상단 파일 아래 아래 접속 아래 '+' 선택
  
  - Name : JSP 실습 계정
  
  - 접속 이름 : sun
  
  - 비밀번호 : 1234
  
  - 호스트 이름 : localhost (기본값)
  
  - 포트 : 1521 (기본값)
  
  - SID : xe (오라클 실행 시 인스턴스 이름, 11g express는 xe로 자동설정)
  
  - 저장 후 접속
  
- 접속하면 SQL을 입력할 수 있는 

<br>
<br>

## exERD 이클립스에 설치

- exERD : 데이터베이스 모델링에 사용하는 도구

- 이클립스(상단) > Help > Install New Software...

  - Work with 우측 우측의 Add... 선택
  
  - Name : exERD
  <br>Location : http://exerd.com/update → Add
  
  - 프로그램 그리드에 eXERD 이름 앞 체크박스 체크 → Next
  
  - 라이선스 사용 동의 → Finish
  
  - (수 초 ~ 수십 초 후) Certificate → Accept Selected
  
  - Would you like to restart Eclipse IDE to apply the change? → Restart Now
  
- 이클립스(상단) > File > Now > others를 선택

  - 중간에 eXERD 폴더가 잘 있는지 Check

  - 리버스 엔지니어링 / 용어사전 / ERwin XML / eXERD File / X스크립트
