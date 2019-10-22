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

- 예시

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

- 예시

{% highlight bash %}
$ which find # O
/usr/bin/find

$ which abc # X
/usr/bin/which: no abc in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
{% endhighlight %}

<br>

#### 명령어 + 옵션

- 명령어는 주로 하나 이상의 옵션들과 함께 사용

- 옵션 : 보다 구체적으로 실행할 수 있게 함

- 단축옵션 : 명령어를 입력하고 `-`와 함께 옵션 명시

  - 여러 옵션을 한 명령어에 연이어 사용 가능

  - 예시
  
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

  - 예시
  
{% highlight bash %}
$ ls -all
합계 4
drwxr-xr-x.  3 root root   17 10월 15 10:00 .
dr-xr-xr-x. 17 root root  224 10월 15 10:00 ..
drwx------.  2 sun  sun  4096 10월 15 13:29 sun
{% endhighlight %}

<br>

#### 명령어 도움말 보기 1 - help $쉘빌트인

- 쉘 빌트인 도움말 보기

  - bash의 쉘 빌트인마다 내장된 도움말이 있는데, 이것을 확인하기 위한 명령어가 `help`
  
- 도움말시 기호의 의미

  - `[]` : 명령어 문장에서 나타날 때, `[]`안에 있으면 옵션이라는 의미
  
  - `|` : 둘 중 하나의 항목만을 사용한다는 뜻
  
- 예시 : 쉘빌트인 `cd`의 도움말 확인

{% highlight bash %}
$ help cd
cd: cd [-L|[-P [-e]]] [dir]
    Change the shell working directory.

    Change the current directory to DIR.  The default DIR is the value of the
    HOME shell variable.

    The variable CDPATH defines the search path for the directory containing
    DIR.  Alternative directory names in CDPATH are separated by a colon (:).
    A null directory name is the same as the current directory.  If DIR begins
    with a slash (/), then CDPATH is not used.

    If the directory is not found, and the shell option `cdable_vars' is set,
    the word is assumed to be  a variable name.  If that variable has a value,
    its value is used for DIR.

    Options:
        -L      force symbolic links to be followed
        -P      use the physical directory structure without following symbolic
        links
        -e      if the -P option is supplied, and the current working directory
        cannot be determined successfully, exit with a non-zero status

    The default is to follow symbolic links, as if `-L' were specified.

    Exit Status:
    Returns 0 if the directory is changed, and if $PWD is set successfully when
    -P is used; non-zero otherwise.
{% endhighlight %}

<br>

#### 명령어 도움말 보기 2 - man \[$sectionNumber] $커맨드라인용실행프로그램

- man : 특별한 페이징 프로그램

  - 커맨드라인용 실행 프로그램들의 대부분은 man 페이지(혹은 매뉴얼)라고 불리는 공식적인 프로그램 설명서를 제공
  
  - 매뉴얼 페이지를 볼 때 사용하는 명령어
  
- 보기 형식이 다소 다양한 편

  - 일반적으로 제목, 명령어 문법 개요, 명령어 사용 목적, 명령어 옵션에 대한 설명
  
  - 명령어 사용 예제는 없음(지침서가 아니기 때문)
  
- 대부분의 리눅스 시스템에서는, man 명령어가 메뉴얼 페이지를 표시하기 위해 less 명령어를 사용

  - man 페이지를 표시하는 동안 less 명령어가 사용 가능하다는 의미
  
- 예시(섹션 미지정)
{% highlight bash %}
$ man ls
LS(1)                                       User Commands                                       LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information about the FILEs (the current directory by default).  Sort entries alphabeti‐
       cally if none of -cftuvSUX nor --sort is specified.

       Mandatory arguments to long options are mandatory for short options too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..

 Manual page ls(1) line 1 (press h for help or q to quit)
{% endhighlight %}

- man 명령어(매뉴얼)가 표시하는 각각의 섹션

  - LS(**1**)                                       User Commands                                       LS(**1**)
  <br>Manual page ls(**1**) line 1 (press h for help or q to quit)

  - `1` : 사용자 명령어
  
  - `2` : 커널시스템 콜 API
  
  - `3` : C 라이브러리 API
  
  - `4` : 장치 노드, 드라이버 등 특수파일
  
  - `5` : format(형식)
  
  - `6` : 스크린세이버와 같은 게임 or 미디어파일
  
  - `7` : 기타
  
  - `8` : 시스템 관리용 명령어
  
  - 옵션으로 SectionNumber를 주면, 해당 섹션을 바로 띄워줌
  
- 예시(섹션지정)

{% highlight bash %}
$ man 8 ping
PING(8)                            System Manager's Manual: iputils                           PING(8)

NAME
       ping - send ICMP ECHO_REQUEST to network hosts

SYNOPSIS
       ping  [-aAbBdDfhLnOqrRUvV46]  [-c  count]  [-F  flowlabel]  [-i  interval]  [-I interface] [-l
       preload] [-m mark] [-M pmtudisc_option] [-N nodeinfo_option] [-w deadline]  [-W  timeout]  [-p
       pattern]  [-Q tos] [-s packetsize] [-S sndbuf] [-t ttl] [-T timestamp option] [hop ...] desti‐
       nation

DESCRIPTION
       ping uses the ICMP protocol's mandatory ECHO_REQUEST datagram to elicit an ICMP  ECHO_RESPONSE
       from  a  host or gateway.  ECHO_REQUEST datagrams (``pings'') have an IP and ICMP header, fol‐
       lowed by a struct timeval and then an arbitrary number of ``pad'' bytes used to fill  out  the
       packet.

       ping works with both IPv4 and IPv6. Using only one of them explicitly can be enforced by spec‐
       ifying -4 or -6.

 Manual page ping(8) line 1 (press h for help or q to quit)

$ man 7 ls # 없는 메뉴얼
No manual entry for ls in section 7
{% endhighlight %}

- 옵션 k

  - 검색어에 따라 일치하는 명령어의 man 페이지를 검색(명령어 + 간략한 정보 한 줄)
  
  - 사용법 : man -k $검색할단어 (= apropos $검색할단어)
  
  - 대략적인 정보
  
  - 첫 번째 필드 : man 페이지의 이름, 두 번째 필드 : 섹션 번호
  
- 예시(옵션k)

{% highlight bash %}
$ man -k ping
arping (8)           - send ARP REQUEST to a neighbour host
ausyscall (8)        - a program that allows mapping syscall names and numbers
getkeycodes (8)      - print kernel scancode-to-keycode mapping table
loadunimap (8)       - load the kernel unicode-to-font mapping table
mapscrn (8)          - load screen output mapping table
newgidmap (1)        - set the gid mapping of a user namespace
newuidmap (1)        - set the uid mapping of a user namespace
ping (8)             - send ICMP ECHO_REQUEST to network hosts
ping6 (8)            - send ICMP ECHO_REQUEST to network hosts
projid (5)           - the project name mapping file
service_seusers (5)  - The SELinux GNU/Linux user and service to SELinux user mapping configuration f...
setkeycodes (8)      - load kernel scancode-to-keycode mapping table entries
seusers (5)          - The SELinux GNU/Linux user to SELinux user mapping configuration file
swapoff (8)          - enable/disable devices and files for paging and swapping
swapon (8)           - enable/disable devices and files for paging and swapping
thin_delta (8)       - Print the differences in the mappings between two thin devices.
ts (1ssl)            - Time Stamping Authority tool (client/server)
xfs_bmap (8)         - print block mapping for an XFS file

### 완전 동일
$ apropos ping
arping (8)           - send ARP REQUEST to a neighbour host
ausyscall (8)        - a program that allows mapping syscall names and numbers
getkeycodes (8)      - print kernel scancode-to-keycode mapping table
loadunimap (8)       - load the kernel unicode-to-font mapping table
mapscrn (8)          - load screen output mapping table
newgidmap (1)        - set the gid mapping of a user namespace
newuidmap (1)        - set the uid mapping of a user namespace
ping (8)             - send ICMP ECHO_REQUEST to network hosts
ping6 (8)            - send ICMP ECHO_REQUEST to network hosts
projid (5)           - the project name mapping file
service_seusers (5)  - The SELinux GNU/Linux user and service to SELinux user mapping configuration f...
setkeycodes (8)      - load kernel scancode-to-keycode mapping table entries
seusers (5)          - The SELinux GNU/Linux user to SELinux user mapping configuration file
swapoff (8)          - enable/disable devices and files for paging and swapping
swapon (8)           - enable/disable devices and files for paging and swapping
thin_delta (8)       - Print the differences in the mappings between two thin devices.
ts (1ssl)            - Time Stamping Authority tool (client/server)
xfs_bmap (8)         - print block mapping for an XFS file
{% endhighlight %}

- whatis $찾아볼명령어 - man page 검색
  
  - 검색어에 따라 일치하는 명령어의 man 페이지를 검색(명령어)

  - 특정 키워드에 부합하는 man 페이지에 대하여 그 이름과 한 줄의 간략한 정보를 보여줌
  
- 예시

{% highlight bash %}
$ whatis ping
ping (8)             - send ICMP ECHO_REQUEST to network hosts
{% endhighlight %}

<br>

#### 명령어 도움말 보기 3 - info $GNU프로그램명

- GNU 프로그램은 info 페이지라는 man 페이지의 대안을 제공

- info 페이지

  - info라는 읽기 프로그램으로 볼 수 있음

  - 웹 페이지처럼 하이퍼링크 되어있음
  
- info 파일

  - 개별 노드마다 각각 개별 주제를 포함한 트리 구조
  
  - 노드 간에 이동할 수 있는 하이퍼링크 제공
  
  - 하이퍼링크는 * 기호를 시작
  
  - 하이퍼링크에 커서를 두고 `Enter`를 누르면 바로 활성화

- info 명령어

  - `?` : 명령어 도움말 보기
  
  - `Page Up` `Backspace` : 이전 페이지
  
  - `Page Down` `Spacebar` : 다음 페이지
  
  - `n` : 다음 - 다음 노드 보기
  
  - `p` : 이전 - 이전 노드 보기
  
  - `u` : 위로 - 현재 표시된 노드의 상위 노드(주로 메뉴) 보기
  
  - `Enter` : 현재 커서 위치에 있는 하이퍼링크로 이동
  
  - `q` : 종료
  
- 예시

{% highlight bash %}
$ info coreutils
File: coreutils.info,  Node: Top,  Next: Introduction,  Up: (dir)

GNU Coreutils
*************

This manual documents version 8.22 of the GNU core utilities, including
the standard programs for text and file manipulation.

   Copyright (C) 1994-2013 Free Software Foundation, Inc.

     Permission is granted to copy, distribute and/or modify this
     document under the terms of the GNU Free Documentation License,
     Version 1.3 or any later version published by the Free Software
     Foundation; with no Invariant Sections, with no Front-Cover Texts,
     and with no Back-Cover Texts.  A copy of the license is included in
     the section entitled "GNU Free Documentation License".

* Menu:

--zz-Info: (coreutils.info.gz)Top, 335 lines --Top------------------------------------------------------
No `Prev' or `Up' for this node within this document.
{% endhighlight %}

<br>

#### 명령어 도움말 보기 4 - ReadMe와 프로그램 문서파일 확인하기

- 사용자 컴퓨터에 설치된 대부분의 소프트웨어 패키지는 문서 파일을 가지고 있음

  - `/usr/share/doc` 디렉토리 내에 존재
  
- 대부분 일반적인 텍스트 파일 형식

  - `less`로 볼 수 있음
  
- HTML 포맷 문서

  - 웹 브라우저를 통해서도 볼 수 있음
  
- gz 확장자(gzip 압축 프로그램으로 압축된 파일)

  - `zless`로 볼 수 있음(gzip으로 압축된 텍스트파일의 내용 표시)
  
<br>

#### 별칭으로 명령어 만들기

- `;`

  - 각 명령어를 구분하여 한 줄에 하나 이상의 명령어를 입력할 수 있음
  
- `type $명령어`로 alias로 생성할 명령어의 이름이 혹시 사용중인지 체크하기

  - 중복되어도 상관 없지만, 중복되지 않도록 사용하기
  
- `alias name='string'`

  - 별칭 만들기
  
  - 정의되고 나면 쉘 어디서든 명령어로써 사용할 수 있음
  
  - 커맨드라인에서 생성하면 쉘 세션이 종료되면 사라짐
  
- `alias`

  - 사용자 환경에 정의된 모든 별칭 확인

- 예시

{% highlight bash %}
$ alias
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'

$ hi
bash: hi: command not found

$ alias hi='echo hi'

$ alias
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias hi='echo hi'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias which='alias | /usr/bin/which --tty-only --read-alias --show-dot --show-tilde'

$ hi
hi
{% endhighlight %}

