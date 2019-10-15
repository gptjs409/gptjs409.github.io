---
layout: post
title:  "Shell Script 기본"
date:   2019-10-15 12:43:27
author: Choi HyeSun
categories: Infra
tags:
  - Shell Script
  - ShellScript
  - Script
  - Shell
  - bash
---

## Bash Shell
CentOS의 기본 쉘은 Bash(Bourne Again SHell : "배쉬 쉘")

<br>

Bash Shell의 특징

  - Alias 기능(명령어 단축 기능)

  - History 기능(↑/↓)

  - 연산 기능

  - Job Control 기능
  
  - 자동 이름 완성 기능(Tab키)
  
  - 프롬프트 제어 기능
  
  - 명령 편집 기능
  
<br>

Shell의 명령문 처리 방법

- (프롬프트) 명령어 [옵션...] [인자...]

- 예) # rm -rf /sun

<br>
<br>

## 환경 변수
`echo $환경변수이름`으로 확인 가능

{% highlight bash %}
[sun@localhost ~]$ echo $HOME
/home/sun
{% endhighlight %}

<br>

`export 환경변수=값`으로 환경 변수의 값 변경 가능 - 단, 로그인 세션에서만 유지

{% highlight bash %}
[sun@localhost ~]$ echo $WOW
[sun@localhost ~]$ export WOW=1234
[sun@localhost ~]$ echo $WOW
1234
{% endhighlight %}

<br>

주요 환경 변수

|환경 변수|설명|예|
|---|---|---|
|HOME|현재 사용자의 홈 디렉토리|`/home/sun`|
|LANG|현재 지원되는 언어|`ko_KR.UTF-8`|
|TERM|로그인 터미널 타입|`xterm`|
|USER|현재 사용자 이름|`sun`|
|COLUMNS|현재 터미널의 컬럼 수|`80`|
|PS1|1차 명령 프롬프트 변수|`[\u@\h \W]\$`|
|BASH|bash 셀의 경로|`/bin/bash`|
|HISTFILE|히스토리 파일 경로|`/home/sun/.bash_history`|
|HOSTNAME|호스트의 이름|`localhost.localdomain`|
|LOGNAME|로그인 이름|`sun`|
|MAIL|메일 보관 경로|`/var/spool/mail/sun`|
|PATH|실행 파일을 찾는 디렉터리 경로|`/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/sun/.local/bin:/home/sun/bin
`|
|PWD|사용자의 현재 작업 디렉터리|`/home/sun`|
|SHELL|로그인해서 사용중인 쉘|`/bin/bash`|
|LINES|현재 터미널 라인 수|`24`|
|PS2|2차 명령 프롬프트(대개 `>`)|`>`|
|BASH_VERSION|bash 버전|`4.2.46(2)-release`|
|HISTSIZE|히스토리 파일에 저장되는 개수|`1000`|
|OSTYPE|운영체제 타입|`linux-gnu`|

<br>
<br>

## 쉘 스크립트 프로그래밍 특징

- C언어와 유사하게 프로그래밍 가능

- 변수, 반복문, 제어문 등의 사용이 가능

- 별도로 컴파일하지 않고 텍스트 파일 형태로 바로 실행

- vi 등으로 작성이 가능

- 리눅스의 많은 부분이 쉘 스크립트로 작성되어 있음

<br>
<br>

## 쉘 스크립트의 작성과 실행

작성시

- vi로 작성

- 쉘 스크립트 파일의 확장명은 되도록 `*.sh`로

<br> 

작성 내용

{% highlight bash %}
[sun@localhost ~]$ vi name.sh
#!/bin/sh
echo "호스트명": $HOSTNAME
exit 0
{% endhighlight %}

- 첫줄 #!/bin/sh → 사용할 언어 선택(sh 쉘)

<br>

실행 방법 1. `sh <스크립트파일>`

{% highlight bash %}
[sun@localhost ~]$ ll
합계 4
-rw-rw-r--. 1 sun sun 48 10월 15 11:33 name.sh
[sun@localhost ~]$ sh name.sh
호스트명: localhost.localdomain
{% endhighlight %}

<br>

실행 방법 2. 실행 가능 속성 부여 `chmod +x <스크립트파일>` 후 `./<스크립트파일>`로 실행

{% highlight bash %}
[sun@localhost ~]$ chmod +x name.sh
[sun@localhost ~]$ ll
합계 4
-rwxrwxr-x. 1 sun sun 48 10월 15 11:33 name.sh
[sun@localhost ~]$ ./name.sh
호스트명: localhost.localdomain
{% endhighlight %}

<br>

> 스크립트 파일의 속성을 `755`로 변경해주면 모든 사용자가 스크립트를 사용할 권한이 생김
>
> 스크립트 파일을 /user/local/bin 디렉토리에 복사해두면 모든 사용자가 접근할 수 있음
>
> 보안상 root 권한으로 수행

<br>
<br>

## 변수의 기본
