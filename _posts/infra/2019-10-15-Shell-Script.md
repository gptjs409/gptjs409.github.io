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
|PATH|실행 파일을 찾는 디렉터리 경로|`/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/sun/.local/bin:/home/sun/bin`|
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

- 사용 전 미리 선언하지 않음. 처음 값이 할당되면서 자동으로 변수가 생성

- 모든 변수는 문자열(String)로 취급

- 대소문자를 구분

- 변수 대입시 `=` 좌우에 공백이 없어야 함

- 값에 ` `(띄어쓰기)가 있다면, `"` / `'` 등으로 묶어줘야 함

- `$` 문자가 들어간 글자를 사용하려면 `' '`로 묶어주거나 앞에 `\`를 붙이면 됨

{% highlight bash %}
[sun@localhost ~]$ vi var1.sh
#!/bin/sh
myvar="Hello Sun"
echo $myvar
echo "$myvar"
echo '$myvar'
echo \$myvar
echo 값 입력 :
read myvar
echo '$myvar' = myvar
exit 0
[sun@localhost ~]$ sh var1.sh
Hello Sun
Hello Sun
$myvar
$myvar
값 입력 :
아무 값을 입력해보고 있음
$myvar = 아무 값을 입력해보고 있음
{% endhighlight %}

<br>
<br>

## 숫자 계산

- 변수에 대입된 값은 모두 문자열로 취급

- 변수에 들어있는 값을 숫자로 해서 `+`, `-`, `*`, `/` 등의 연산을 하려면 `` `expr (연산식)` ``을 사용

- 수식에 괄호`()`나 곱하기`*`의 경우 그 앞에 꼭 역슬래쉬`\`를 붙임

{% highlight bash %}
[sun@localhost ~]$ vi num1.sh
#!/bin/sh
num1=100
num2=$num1+200
echo $num2
num3=`expr $num1 + 200`
echo $num3
num4=`expr \( $num1 + 200 \) / 10 \* 2`
echo $num4
exit 0
[sun@localhost ~]$ sh num1.sh
100+200
300
60
{% endhighlight %}

<br>
<br>

## 파라미터(Parameter) 변수

- 파라미터 변수는 $0, $1, $2...의 형태

- 전체 파라미터는 $*

{% highlight bash %}
[sun@localhost ~]$ vi para1.sh
#!/bin/sh
echo "실행파일 이름은 <$0>"
echo "첫 번째 파라미터 <$1>"
echo "두 번째 파라미터 <$2>"
echo "세 번째 파라미터 <$3>"
echo "전체 파라미터 <$*>"
exit 0
[sun@localhost ~]$ sh para1.sh 값1 값2 값3
실행파일 이름은 <para1.sh>
첫 번째 파라미터 <값1>
두 번째 파라미터 <값2>
세 번째 파라미터 <값3>
전체 파라미터 <값1 값2 값3>
{% endhighlight %}

<br>

예

|파라미터변수|$0|$1|$2|$3|
|---|---|---|---|---|
|명령어1|yum|-y|install|gftp|
|명령어2|para1.sh|값1|값2|값3|

<br>
<br>

## 조건문 - 기본 if문

<br>
<br>

## 조건문 - if~else문

<br>
<br>

## 조건문 - if~elif문

<br>
<br>

## 조건문에 들어가는 비교 연산자

<br>
<br>

## 파일과 관련된 조건

<br>
<br>

## 조건문 - case~esac문(1)

<br>
<br>

## 조건문 - case~esac문(2)

<br>
<br>

## AND / OR 관계 연산자

<br>
<br>

## 반복문 - for문(1)

<br>
<br>

## 반복문 - for문(2)

<br>
<br>

## 반복문 - while문(1)

<br>
<br>

## 반복문 - while문(2)

<br>
<br>

## 반복문 - while문(3)

<br>
<br>

## 반복문 - until문

<br>
<br>

## break, continue, exit, return 문

<br>
<br>

## 사용자 정의 함수

<br>
<br>

## 함수의 파라미터 사용

<br>
<br>

## eval

<br>
<br>

## export

<br>
<br>

## printf

<br>
<br>

## set과 $(명령어)

<br>
<br>

## shift(1)

<br>
<br>

## shift(2)

<br>
<br>

## 끝
