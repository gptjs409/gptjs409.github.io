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
[root@localhost ~]# echo $HOME
/root
{% endhighlight %}
