---
layout: post
title:  "[CentOS] Command(명령어) 개념"
date:   2019-09-24 22:21:43
author: Choi HyeSun
categories: infra
tags:
  - CentOS
  - CentOS7
  - 명령어
  - Command
---

## 쉘

#### 쉘이란?

- 키보드로 입력한 명령어를 운영체제에 전달하여 이 명령어를 실행하게 하는 프로그램

- 대부분의 리눅스 배포판은 bash라고 하는 GNU 프로젝트의 쉘 프로그램을 제공

  - bash : Bourne Again Shell; 스티븐 본(Steve Bourne)이 개발한 최초 유닉스 쉘 프로그램인 sh의 확장판
  
<br>

#### 쉘 프롬프트

- 쉘이 입력 가능한 상태일때만 나타남

- `[sun@localhost ~]$`

  - \[ + username@machinename + 현재 작업 디렉토리 + ] + `$ or #`
  
  - `#` : 슈퍼 유저(root)권한, `$` : 일반 사용자
  
- 명령어 히스토리

  - `↑` `↓` 방향키로 : 최근 입력한 명령어를 프롬프트에서 다시 볼 수 있음

  - 대부분의 리눅스 배포판들은 기본적으로 가장 최근 500개의 명령어를 기억할 수 있음
  
- 커서 이동

  - `←`와 `→`로 이동
  
- 세션 종료(터미널 세션을 종료)하는 두 가지 방법

  - 직접 터미널창을 닫기
  
  - exit 명령어 입력하기

{% highlight bash %}
[sun@localhost ~]$ exit

앞으로는 특이사항 없을 경우 ↓ 처럼 표기
$ exit
{% endhighlight %}

<br>

## 명령어

#### 명령어란?

- 리눅스 명령어는 네 가지로 나뉨

- 첫 번째. 실행 프로그램

  - C나 C++ 언어 등으로 컴파일된 바이러니 형식의 프로그램 또는 Shell, Perl, Python 등의 스크립트 언어로 만든 프로그램
  
  - `/usr/bin`의 파일들이 대표적인 예

- 두 번째. 쉘에 내장되어 있는 명령어(쉘-빌트인; Shell builtin)

  - bash는 다수의 명령어를 내부적으로 지원함

  - `cd`가 대표적인 예

- 세 번째. 쉘 함수

  - 시스템 환경에 포함된 쉘 스크립트의 미니 버전

- 네 번째. 별칭

  - 다른 명령어로부터 새롭게 정의한 자기만의 명령어

<br>

#### type $명령어 - 명령어가 어떤 종류인지 확인하기

- 쉘에 내장된 형식으로 명령어를 입력하면 쉘이 실행하게 될 명령어가 어떤 타입인지 보여줌

{% highlight bash %}
$ type cp
cp is /usr/bin/cp (1)

$ type type
type is a shell builtin # (2)

$ type if
if is a shell keyword (3)

$ type ls
ls is aliased to `ls --color=auto' (4)
{% endhighlight %}

<br>

#### which $실행프로그램 - 실행 프로그램의 위치 확인

- 실행프로그램을 대상으로 위치를 파악

{% highlight bash %}
$ which find # O
/usr/bin/find

$ which abc # X
/usr/bin/which: no abc in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
{% endhighlight %}

#### 명령어 + 옵션

- 명령어는 주로 하나 이상의 옵션들과 함께 사용

- 옵션 : 보다 구체적으로 실행할 수 있게 함

- 단축옵션 : 명령어를 입력하고 `-`와 함께 옵션 명시

  - 여러 옵션을 한 명령어에 연이어 사용 가능

{% highlight bash %}
$ ls
sun

$ ls -l
합계 4
drwx------. 2 sun sun 4096 10월 15 13:29 sun

$ ls -a
.  ..  sun

$ ls -a -l
합계 4
drwxr-xr-x.  3 root root   17 10월 15 10:00 .
dr-xr-xr-x. 17 root root  224 10월 15 10:00 ..
drwx------.  2 sun  sun  4096 10월 15 13:29 sun

$ ls -al
합계 4
drwxr-xr-x.  3 root root   17 10월 15 10:00 .
dr-xr-xr-x. 17 root root  224 10월 15 10:00 ..
drwx------.  2 sun  sun  4096 10월 15 13:29 sun
{% endhighlight %}

- Long옵션 : `--`와 함께 옵션을 명시(연이어서 사용 불가)

{% highlight bash %}
$ ls -all
합계 4
drwxr-xr-x.  3 root root   17 10월 15 10:00 .
dr-xr-xr-x. 17 root root  224 10월 15 10:00 ..
drwx------.  2 sun  sun  4096 10월 15 13:29 sun
{% endhighlight %}

<br>

#### 명령어 도움말 보기 1 - help $쉘빌트인

((다음에 추가로 더 옮기기--시간부족))
