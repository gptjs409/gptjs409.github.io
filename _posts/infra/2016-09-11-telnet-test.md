---
layout: post
title:  "[Telnet] telnet 명령어로 html 파일 만들어서 실행해보기"
date:   2016-09-11 22:24:31
author: Choi HyeSun
categories: infra
tags:
  - Telnet
  - Telnet test
  - 텔넷
---

## Telnet 명령어로 Html 파일 만들고 실행해보기

#### 사전작업

- telnet은 root 로그인이 불가하므로 centos에 imsi 계정 생성해둠

- CentOS6에 httpd(아파치 서버) 설치해둠

#### CentOS6에서

- yum으로 telnet, telnet-server, xinetd 설치

{% highlight bash %}
$ yum install -y telnet
$ yum install -y telnet-server
$ yum install -y xinetd
{% endhighlight %}

- vi /etc/xinetd.d/telnet 후 disable = yes를 no로 수정

- xinetd 프로세스 start

{% highlight bash %}
$ service xinetd start
{% endhighlight %}

- netstat –pant로 23번이 열렸는지 확인한다.

{% highlight bash %}
$ netstat –pant
{% endhighlight %}

![image](/img/2016-09-11/telnet-test-001-test1.png)

<br>

#### Windows에서

- telnet 명령어 활성

  - 제어판\프로그램\프로그램 및 기능 > windows 기능 사용/사용 안함
  
  - 텔넷 서버 / 텔넷 클라이언트 체크 후 확인(1분정도 소요)
  
- cmd창에서 telnet 입력 후 접속

![image](/img/2016-09-11/telnet-test-002-test2.png)

![image](/img/2016-09-11/telnet-test-003-test3.png)

- imsi 계정으로 로그인

![image](/img/2016-09-11/telnet-test-004-test4.png)

- 아파치 서버 경로 확인

{% highlight bash %}
$ cat /etc/httpd/conf/httpd.conf | grep DocumentRoot
DocumentRoot "/var/www/html"
{% endhighlight %}

- 아파치 서버 경로(/var/www/html)에 html 파일 생성

  - html file test _ imsi라는 내용의 imsi.html 파일을 생성
  
  ![image](/img/2016-09-11/telnet-test-005-test5.png)
  
  ![image](/img/2016-09-11/telnet-test-006-test6.png)
  
  ![image](/img/2016-09-11/telnet-test-007-test7.png)
  
<br>

#### Web페이지에서

- 호출 확인

![image](/img/2016-09-11/telnet-test-008-test8.png)


