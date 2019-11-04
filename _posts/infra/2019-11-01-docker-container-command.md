---
layout: post
title:  "[Docker] 도커 컨테이너 명령어"
date:   2019-11-02 21:51:56
author: Choi HyeSun
categories: infra
tags:
  - Docker
  - 도커
  - Docker Container
  - 도커 컨테이너
  - Container
  - 컨테이너
---

## Docker 컨테이너의 라이프 사이클

#### 컨테이너를 조작하기 위한 기본 명령

- 컨테이너 생성

  - docker container create
  
  - 이미지로부터 컨테이너를 생성
  
  - 이미지 : Docker에서 서버 기능을 작동시키기 위해 필요한 디렉터리 및 파일, Linux의 작동에 필요한 /etc나 /bin 등과 같은 디렉터리나 파일

  - 명령 실행시 이미지에 포함될 Linux의 디렉터리와 파일들의 스냅샷을 취함
  <br>스냅샷 : 스토리지 안에 존재하는 파일과 디렉터리를 특정 타이밍에서 추출
  
  - 컨테이너를 작성하기만 할 뿐, 컨테이너를 시작하지는 않음. 즉 컨테이너를 시작할 준비로 만듦

- 컨테이너 생성 및 시작

  - docker conatiner run
  
  - 이미지로부터 컨테이너를 생성한 후, 컨테이너 상에서 임의의 프로세스를 시작
  
  - 백그라운드에서 데몬 형태로 띄우거나 강제종료할 수도 있고, 포트 번호와 같은 네트워크 설정도 가능

- 컨테이너 시작

  - docker container start
  
  - 정지중인 컨테이너를 시작할 때 사용
  
  - 컨테이너에 할당된 컨테이너 식별자를 지정하여 컨테이너를 시작

- 컨테이너 정지

  - docker container stop
  
  - 실행 중인 컨테이너를 정지할 때 사용
  
  - 컨테이너에 할당된 컨테이너 식별자를 지정하여 컨테이너를 정지
  
  - 컨테이너 삭제 전에는 컨테이너 정지를 해야 함
  
- 칸테이너 재시작

  - docker container restart
  
  - 실행 중인 컨테이너를 재시작할 때 사용

- 컨테이너 삭제

  - docker container rm
  
  - 정지된 컨테이너 프로세스를 삭제할 때 사용
  
- 컨테이너 상태 확인

  - docker container ps
  
- 일시정지

  - docker container pause

<br>
<br>

## 컨테이너 생성 및 시작 (대화식-attach mode)

- docker container run \[옵션] 이미지명\[:태그명]\[인수]

  - container를 생략해도 됨

- 지정할 수 있는 주요 옵션

|옵션|설명|
|---|---|
|--attach, -a|표준 입력(STDIN), 표준 출력(STDOUT), 표준 오류 출력(STDERR)에 attach|
|--cidfile|컨테이너 ID를 파일로 출력|
|--detach, -d|컨테이너를 생성하고 백그라운드에서 실행|
|--interactive, -i|컨테이너의 표준 입력을 엶|
|--tty, -t|단말기 디바이스를 사용|

<br>

- 예제) 달력을 출력하는 컨테이너

  - docker container run -it --name "test1" centos /bin/cal
  
  - `docker container run` : 컨테이너를 생성 및 실행
  
  - `-it` : 콘솔에 결과를 출력하는 옵션
  <br>`-i` - 컨테이너의 표준 출력을 엶, `-t` - tty(단말 디바이스)를 확보
  
  - `--name "test"` : 컨테이너명 지정
  
  - `centos` : 이미지명 지정
  
  - `/bin/cal` : 컨테이너에서 실행할 명령

{% highlight bash %}
$ sudo docker container run -it --name "test1" centos /bin/cal
Unable to find image 'centos:latest' locally
latest: Pulling from library/centos
729ec3a6ada3: Pull complete
Digest: sha256:f94c1d992c193b3dc09e297ffd54d8a4f1dc946c37cbeceb26d35ce1647f88d9
Status: Downloaded newer image for centos:latest
    October 2019
Su Mo Tu We Th Fr Sa
       1  2  3  4  5
 6  7  8  9 10 11 12
13 14 15 16 17 18 19
20 21 22 23 24 25 26
27 28 29 30 31
{% endhighlight %}


- 예제) 컨테이너 안에서 쉘(/bin/bash)을 실행

  - exit로 쉘 종료
  
{% highlight bash %}
$ sudo docker container run -it --name "test2" centos /bin/bash
[root@883baf01dc40 /]#
{% endhighlight %}

<br>
<br>

## 컨테이너 백그라운드 실행 (백그라운드에서 실행-detach mode)

- docker container run \[실행 옵션] 이미지명\[:태그명] [인수]

- 지정할 수 있는 주요 옵션

  - \--restart=\[no | on-failure | on-failure:횟수n | always | unless-stopped]

|옵션|설명|
|---|---|
|--detach, -d|백그라운드 실행|
|--user, -u|사용자명 지정|
|--restart=(이하생략)|명령의 실행 결과에 따라 재시작하는 옵션<br>--rm과 동시 사용 불가|
|--rm|명령 실행 후 컨테이너 자동 삭제<br>--restart와 동시 사용 불가|

<br>

- restart 옵션

  - no : 재시작하지 않음
  
  - om-failure : 종료 스테이터스가 0이 아닐 때 재시작
  
  - on-failure:횟수n : 종료 스테이터스가 0이 아닐 때 n번 재시작
  
  - always : 항상 재시작
  
  - unless-stopped : 최근 컨테이너가 정지 상태가 아니라면 항상 재시작
  


<br>
<br>

## 컨테이너 네트워크 설정

<br>
<br>

## 자원을 지정하여 컨테이너 생성 및 실행

<br>
<br>

## 컨테이너 생성 및 시작하는 환경 지정

<br>
<br>

## 가동중인 컨테이너 목록 표시

<br>
<br>

## 컨테이너 가동 확인

<br>
<br>

## 컨테이너 시작

<br>
<br>

## 컨테이너 정지

<br>
<br>

## 컨테이너 재시작

<br>
<br>

## 컨테이너 삭제

<br>
<br>

## 컨테이너 중단/재개
