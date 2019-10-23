---
layout: post
title:  "[Java] 웹 애플리케이션 이해하기"
date:   2019-10-23 19:54:42
author: Choi HyeSun
categories: java
tags:
  - Java
  - 웹 애플리케이션
  - 웹 애플리케이션 정의
  - 웹 애플리케이션 기본 구조
  - 톰캣
  - war파일
  - 배치
  - 배포
  - 톰캣연동
  - Tomcat
  - deploy
---

## 웹 애플리케이션

#### 웹 애플리케이션의 정의

- 정적인 웹 애플리케이션 + 동적인 서비스

- 웹 컨테이너에서 실행되는 JSP, 서블릿, 자바 클래스들을 사용해 정적 웹 프로그래밍 방식의 단점을 보완하여 서비스를 제공하는 서버 프로그램

  - 정적 웹 애플리케이션의 기능인 HTML(HTML5), 자바스크립트, CSS 등도 그대로 사용할 수 있음
  
<br>
<br>

## 웹 애플리케이션 기본 구조

#### 기본 구조

{% highlight bash %}
Web Application Name
        │
        └─────── WEB-INF
                   │
                   ├─────── classes
                   │
                   ├─────── lib 
                   │
                   └─────── web.xml
{% endhighlight %}

- 웹 컨테이너(톰캣 등)에서 실행하는 웹 애플리케이션의 기본 디렉터리 구조

  - 이런 구조를 갖추지 않고 컨테이너에서 웹 어플리케이션 실행시 오류 발생
  
  - 기본 구조 이외의 다른 기능이 추가되면 디렉터리를 추가해서 사용
  
<br>
<br>

#### 파일 탐색기로 웹 애플리케이션 기본 구조를 만들어보자 - 예제 (webshop)
> 예제 전에 일단 Git Repo 받아두려 함! 새로 공부용으로 하나 만들었음
>
> /D/git
> 
> $ git clone https://github.com/gptjs409/sunny.git

- D:\git\sunny에 webShop 폴더 생성

- webShop 폴더에 WEB-INF 폴더 생성

- WEB-INF 폴더에 classess, lib 폴더 생성

- WEB-INF 폴더에 텍스트 파일 생성 (마우스 우클릭 → 새로 만들기 → 텍스트 문서)

- 텍스트 파일 이름을 web.xml로 변경

- 내용을 입력 (톰캣 8까지는 자동 생성, 톰캣 9부터는 직접 추가해야 함)
  
{% highlight html %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/CMLSchema-instance" xmlns="http://java.sun.comm/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
</web-app>
{% endhighlight %}

<br>
<br>

#### 구조의 이해

- 만든 구성요소의 의미

|**구성 요소**|기능|
|:-:|---|
|**webShop**|웹 애플리케이션의 루트 디렉터리<br>다른 웹 애플리케이션 이름과 중복을 허용하지 않음, JSP HTML 파일 저장|
|**webShop/WEB-INF**|웹 애플리케이션에 관한 정보가 저장<br>외부에서 접근할 수 없는 디렉터리|
|**webShop/WEB-INF/classes**|웹 애플리케이션이 수행하는 서블릿과 다른 일반 클래스들이 위치|
|**webShop/WEB-INF/lib**|웹 애플리케이션에서 사용되는 여러가지 라이브러리 압축 파일(jar 파일)이 저장<br>DB 연동 드라이버나 프레임워크 기능 관련 jar 파일이 여기에 저장됨<br>lib 디렉터리의 jar 파일은 클래스패스가 자동 설정됨|
|**webShop/WEB-INF/web.xml**|배치 지시자(deployment descripotr)로서 일종의 환경 설정 파일<br>웹 애플리케이션에 대한 여러가지 설정을 할 때 사용|

- 웹 애플리케이션에 추가될(수 있는) 구성 요소의 기능

|**추가 구성 요소**|기능|
|:-:|---|
|**webshop/jsp**|JSP 파일 저장|
|**webshop/html**|HTML 파일 저장|
|**webShop/css**|스타일시트 파일 저장|
|**webShop/image**|웹 애플리케이션에서 사용되는 이미지가 저장|
|**webShop/js**|자바 스크립트 파일 저장
|**webShop/WEB-INF/bin**|애플리케이션에서 사용되는 각종 실행 파일 저장|
|**webShop/WEB-INF/conf**|프레임워크에서 사용되는 각종 설정 파일 저장|
|**webShop/WEB-INF/src**|자바 소스 파일 저장|

<br>
<br>

## 컨테이너에서 웹 애플리케이션 실행하기 - 이클립스 없이 해보기

#### 컨테이너에 웹 어플리케이션 등록

- 웹 애플리케이션을 톰캣 컨테이너에 등록하는 방법

  - 방법 1. \%CATALINA_HOME\%webApp 디렉터리에 애플리케이션 저장
  
  - 방법 2. server.xml에 직접 웹 애플리케이션을 등록
  
- \%CATALINA_HOME\% : 톰캣의 루트 디렉터리(D:\\tomcat9)

<br>

#### 실행 전. HTML 코드를 만들어주기

- webShop 프로젝트에 main.html 파일 만들어주기

{% highlight html %}
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8" />
  <title>Hello JSP!</title>
</head>
<body>
  Hello JSP!!<br>
  Sun입니다!
</body>
</html>
{% endhighlight %}

<br>

#### (참고) 윈도우 포트 확인 및 프로세스 죽이기

- 포트 확인 : netstat

  - \-a : 모든 포트 확인
  
  - \-n : ip주소:port 형테로 보여줌
  
  - \-o : 프로세스 ID (PID) 표시
  
{% highlight bash %}
C:\Users\User>netstat -ano

활성 연결

  프로토콜  로컬 주소              외부 주소              상태            PID
  TCP    0.0.0.0:8009           0.0.0.0:0              LISTENING       17556
{% endhighlight %}

- 확인한 PID를 작업관리자 > 세부정보 > PID에서 조회해서 해당 프로세스 죽이기

<br>

#### 방법 1. \%CATALINA_HOME\%webApp 디렉터리에 애플리케이션 저장

- webShop 폴더 전체를 복사한 후 톰캣 루트 디렉터리의 하위에 있는 webapps 폴더에 붙여넣기
  
  - D:\\git\\sunny\\webShop →(복붙)→ D:\\tomcat9\\webapps\\webShop
  
- 톰캣 실행 (또는 재시작)
  
  - D:\\tomcat9\\bin\\tomcat9.exe 더블클릭
  
- 브라우저에서 웹 어플리케이션 요청

  - http:\//IP주소:포트번호/컨텍스트이름/요청파일이름
  
  - http:\//127.0.0.1:8090/webShop/main.html
  
- 결과

![image](/img/2019-10-23/web-application-001-test1.png)

<br>

#### 사전 학습. 컨텍스트

- server.xml에 등록하는 웹 애플리케이션을 컨텍스트(Context)라고 함

- 톰캣 입장에서 인식하는 한 개의 웹 어플리케이션

- 컨테이너 실행 시 웹 애플리케이션당 하나의 컨텍스트가 생성

- 컨텍스트명은 웹 애플리케이션명과 동일하게 만드는 것이 일반적이나, 보안상 또는 이름이 긴 경우 다르게 만들 수도 있음

- 컨텍스트의 주요 특징

  - 웹 애플리케이션당 하나의 컨텍스트 등록
  
  - 웹 애플리케이션 이름과 같을 수도 있고 다를 수도 있음
  
  - 이름 중복 불허
  
  - 웹 애플리케이션의 의미를 가장 잘 나타낼 수 있는 명사로 지정
  
  - 대/소문자 구분
  
  - server.xml에 등록
  
- 등록은 server.xml에 함 (\<Context> 태그를 이용해야 함)

  - \<Context path="//컨텍스트명" docBase="실제 웹 애플리케이션의 WEB-INF 디렉터리 위치" reloadable="true 또는 false" />
  
- \<Context> 태그

  - 톰캣은 모든 설정 정보를 XML로 저장한 후 실행시 정보를 읽어와 설정대로 실행
  
  - 만든 웹 어플리케이션도 미리 \<Context> 태그를 이용해서 server.xml에 등록해두어야 톰캣이 설정한 대로 웹 어플리케이션을 실행
  
  - 위치는 \<Host> 태그 속(기본파일일 경우 149행 쯤 있음) `<HOST 샬라샬라> 요 사이 </HOST>`
  
  - \<Context 태그 구성 요소의 기능>
  
|**구성요소**|기능|
|:-:|---|
|**path**|웹 애플리케이션의 컨텍스트 이름<br>웹 어플리케이션 이름과 다를 수 있음<br>웹 브라우저에서 실제 웹 애플리케이션을 요청하는 이름|
|**docBase**|컨텍스트에 대한 실제 웹 애플리케이션이 위치한 경로<br>WEB-INF 상위 폴더까지의 경로|
|**reloadable**|실행 중 소스 코드가 수정될 경우 바로 갱신할지를 설정<br>false로 설정하면 톰캣을 다시 실행해야 추가한 소스 코드의 기능이 반영|

<br>

#### 방법 2. 임의의 위치에 작성한 웹 애플리케이션을 server.xml에 직접 등록

- 임의의 폴더에 만든 webShop 웹 애플리케이션을 server.xml에 컨텍스트로 등록해서 실행
  
- server.xml 찾기

  - server.xml는 톰캣 설치 루트 디렉터리 아래, conf 디렉터리 안에 있음
  
  - D:\\tomcat9\\conf\\server.xml
  
- server.xml에 컨텍스트 등록
  
  - \<Context path="//webMal" docBase="D:\\git\\sunny\\webShop" reloadable="true" />

{% highlight html %}
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
			
        <Context path="/webMal" docBase="D:\git\sunny\webShop" reloadable="true" />
{% endhighlight %}

- 톰캣 재실행

  - D:\\tomcat9\\bin\\shutdown.sh
  
  - D:\\tomcat9\\bin\\startup.sh
  
- 브라우저에서 웹 어플리케이션 요청

  - http:\//IP주소:포트번호/컨텍스트이름/요청파일이름
  
  - http:\//127.0.0.1:8090/webMal/main.html
  
- 결과

![image](/img/2019-10-23/web-application-002-test2.png)

<br>
<br>

## 톰캣 컨테이너에서 웹 애플리케이션 동작 과정

- 웹 브라우저에서 컨텍스트명(webMal)으로 요청

  - http:\//localhost:8090/webMal/main.html
  
- 요청을 받은 톰켓 컨테이너는 요청한 컨텍스트명이 server.xml에 있는지 확인

  - 없으면) 404 오류 발생

- 해당 컨텍스트명이 있다면 컨텍스트명에 대한 실제 웹 애플리케이션이 있는 경로(D:\\git\\sunny\\webShop)로 가서 요청한 main.html을 클라이언트 웹 브라우저로 전송

- 클라이언트는 전송된 main.html을 브라우저에 나타냄

![image](/img/2019-10-23/web-application-002-test2.png)

- (오타나서) 등록되어 있지 않은 컨텍스트명으로 요청 - 404 오류가 발생

![image](/img/2019-10-23/web-application-003-test3.png)
  
<br>
<br>

## 이클립스에서 웹 애플리케이션 실습하기

#### 이클립스에서 웹 프로젝트 생성

- 이클립스에서는 한 개의 프로젝트 - 한 개의 웹 애플리케이션

  - 프로젝트명 = 웹 애플리케이션명
  
- 프로젝트 생성하기

  - 이클립스 > 상단 File > New > Dynamic Web Project 선택

  - 프로젝트명 : webShop2

  - 깃에 소스 업데이트를 위해 경로는 D:\\git\\sunny\\webShop2 로 설정
  
  - Next > Next > \[체크] Generate web.xml deployment descriptor > Finish (체크하면 web.xml을 자동 생성해줌)
  
  - Project Explorer에 webShop 프로젝트가 생성된 것을 확인

<br>
  
#### 이클립스에서 HTML 파일 생성

- 프로젝트 하위 메뉴에서 WebContent 선택 > 마우스 우클릭 > New > HTML File

- 파일명 : main.html > Finish

- 간단하게 내용 작성 후 \<Ctrl> + \<S>로 저장

{% highlight html %}
<!DOCTYPE html>
<html>
<head>
<meta charset="EUC-KR">
<title>Hello JSP</title>
</head>
<body>
HELLO JSP<br>
이번엔 2버전이지요~!
</body>
</html>
{% endhighlight %}

<br>

#### 이클립스와 톰캣 연동

- 이클립스는 통합 개발 환경 도구이므로 설정만 해주면 자동으로 톰캣에 프로젝트를 등록시켜줌

- 설정 방법

  - 이클립스 하단의 Servers 탭 선택 > 마우스 우클릭 > New > Server
  
  - 서버 설정창의 Apache 항목에서 Tomcat v9.0을 선택 > Next
  
  - 톰켓 설치 디렉터리를 설정 (D:\tomcat9) > Finish
  
  - 이클립스 하단의 Servers 탭에 Tomcat 서버가 등록된 것을 확인할 수 있음
  
  - 이클립스 좌측 프로젝트 탐색기에서도 Servers 항목이 등록됨
  
- 프로젝트 탐색기의 Servers

  - 톰캣과 관련된 xml 파일들이 나타남
  
  - 이클립스에서 톰캣 컨테이너 설정 때 톰캣 루트 디렉터리에 있던 여러 xml 설정 파일이 Project Explorer 상단에 자동으로 복사된 것
  
  - 편집을 여기서 하면 되서 편함
  
<br>

#### 이클립스와 연동한 톰캣에 프로젝트 등록

- 이클립스는 server.xml에 직접 등록하지 않고, UI로 조금 더 쉽게 등록 가능

- 등록 방법

  - 이클립스 우측 하단의 Servers 탭 아래에 등록된 톰캣 서버 Tomcat v9.0 Server at localhost [Stopped] 선택 > 마우스 우클릭 > Add and Remove
  
  - 추가할 프로젝트(여기서는 webShop2) 선택 > Add(톰캣에 등록) > Finish(등록 완료-적용)
  
  - 톰캣 서버 아래에 webShop2가 등록된 것을 확인할 수 있음
  
- 등록 확인

  - 좌측 Project Explorer > Servers > server.xml 더블 클릭하여 하단의 Source 탭을 클릭
  
  - 스크롤을 내려 \<Context> 태그 부분을 보면 프로젝트에 대한 컨텍스트가 자동으로 추가된 것을 확인할 수 있음
  
  - 이클립스는 자동 추가시 프로젝트명과 동일하게 컨텍스트 명을 설정해줌 (필요하면 바꾸고 저장)
  
<br>

#### 톰캣을 실행하여 웹 브라우저로 요청하기

- 시작 전 기존 Tomcat 서비스 종료하기

  - D:\\tomcat9\\bin\\shutdown.sh

- Servers 탭 오른쪽에 있는 녹색 실행 버튼을 클릭해 서버 실행

- Console 탭에 로그가 출력되면서 톰캣이 실행됨

- 동일하게 브라우저에서 요청 확인

  - http:\//localhost:8090/webShop2/main.html
  
![image](/img/2019-10-23/web-application-004-test4.png)

<br>
<br>

## 웹 애플리케이션 서비스하기

#### 배치란?

- 배치란?

  - 이클립스에서 개발할 경우 개발자 입장에서는 자신이 만든 기능이 정상적으로 실행되는지 확인하기 위해 빈번하게 톰캣을 재시동

  - 위와 같은 개발 과정을 거쳐 애플리케이션이 완성되면 실제 사용자에게 서비스를 해야 함

  - 실 서비스시 이클립스에 등록된 톰캣에서 실행하는 것이 아닌, 유닉스나 리눅스 서버에 설치된 톰캣에서 실행해야 함

  - 위와 같이 하려면 이클립스에서 개발한 웹 애플리케이션 예제 소스 전체를 실제로 서비스하는 톰캣으로 이동해서 실행해야 함

  - 이 과정을 배치(deploy; 혹은 배포)한다라고 함
  
- 즉, 웹 애플리케이션을 실제로 서비스한다는 의미

<br>

#### 톰캣에 배치하기

- 톰캣은 webapps 폴더에 war 압축파일을 넣고 실행하면, 스스로 압축을 해제하면서 자동으로 등록하여 웹 애플리케이션을 실행시킴

  - 즉, 이클립스에서 소스파일을 war 압축파일 형식으로 저장하여, 그것을 톰캣의 webapps 폴더에 넣고 재시작!
  
- war 파일 추출 방법

  - 이클립스 상단 메뉴의 File > Export...
  
  - Web 항목의 WAR file 선택 > Next > 
  
  - Web Project 선택 : 웹 애플리케이션(프로젝트) WebShop2
  
  - Destination 선택 : 저장할 경로(이번은 톰캣의 webapps으로 바로 저장), D:\\tomcat9\\webapps\\webShop2.war
    <br>다른 곳에 저장 후 옮겨도 무관함
  
  - Finish

- war 파일 확인

  - D:\\tomcat9\\webapps에 webShop2.war가 잘 생겼는지 확인
  
- tomcat 재시작

  - 이클립스에 있는 것 중지 버튼 □ 으로 중지
  
  - D:\\tomcat9\\bin\\startup.sh
  
- 테스트

  - D:\\tomcat9\\webapps에 webShop2 프로젝트 소스 파일이 압축해제되어 있는 것을 확인할 수 있음
  
  - http:\//localhost:8090/webShop2/main.html 정상 호출이 되어, 압축해제한 후 자동 등록을 해줬음을 확인할 수 있음

![image](/img/2019-10-23/web-application-005-test5.png)

<br>
<br>

## 이클립스에서 HTML 파일과 JSP 파일의 한글 인코딩을 UTF-8로 설정하기

- 이클립스에서 HTML이나 JSP 파일을 생성하면 한글 인코딩이 EUC-KR로 설정됨, 이를 UTF-8로 자동 설정하게 끔 셋팅

- 설정 방법

  - 이클립스 상단 메뉴의 Windows > Preference(속성창)
  
  - 속성창 왼쪽 메뉴에 있는 Web > HTML Files 선택
  
  - Encoding을 Korean, EUC-KR에서 ISO 10646/Unicode(UTF-8)로 변경
  
  - Web > JSP Files도 동일하게 설정
  
  - Apply and Close
  
- 설정 전 후 비교

  - 설정 전 HTML 파일 생성시 : \<meta charset="EUC-KR">
  
  - 설정 후 HTML 팡리 생성시 : \<meta charset>="UTF-8">
