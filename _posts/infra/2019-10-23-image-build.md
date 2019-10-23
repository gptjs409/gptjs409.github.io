---
layout: post
title:  "[Docker] Dockerfile로 이미지 빌드하고 컨테이너 띄워보기"
date:   2019-10-23 08:43:23
author: Choi HyeSun
categories: infra
tags:
  - Docker
  - Dockerfile
  - 도커파일
  - 도커파일 작성
---

## Dockerfile이란?

- Docker 이미지 설정 파일

- Docker에서 Dockerfile이라는 설정 파일을 읽게끔 설정이 되어있음

<br>
<br>

## Dockerfile 작성

- 해당 포스트에 따로 포스팅 [LINK](https://gptjs409.github.io/infra/2019/10/22/dockerfile.html)

<br>
<br>

## Dockerfile 생성하기(root로 진행하였음)

- 예제 Dockerfile 만들기

{% highlight bash %}
$ vi Dockerfile
FROM centos:latest
MAINTAINER HyeSun Choi <gptjs409@gmail.com>

RUN echo "Dockerfile"

VOLUME ["/data", "/root/centos/logs", "/var/log/nginx"]
{% endhighlight %}

<br>
<br>

## Dockerfile 이미지 Build하기

- Dockerfile을 기반으로 이미지를 생성하는 작업

- 명령어

  - docker build <옵션> <Dockerfile 경로>
  
- 옵션 --tag $태그명

  - 태그를 지정할 수 있음
  
  - 미지정시 latest로 자동 지정

{% highlight bash %}
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
$ docker build --tag dockerfile:0.1 .
Sending build context to Docker daemon 11.26 kB
Step 1/3 : FROM centos:latest
Trying to pull repository docker.io/library/centos ...


latest: Pulling from docker.io/library/centos
729ec3a6ada3: Pull complete
Digest: sha256:f94c1d992c193b3dc09e297ffd54d8a4f1dc946c37cbeceb26d35ce1647f88d9
Status: Downloaded newer image for docker.io/centos:latest
 ---> 0f3e07c0138f
Step 2/3 : MAINTAINER HyeSun Choi <gptjs409@gmail.com>
 ---> Running in b130b3b25a7d
 ---> e84c4c379b04
Removing intermediate container b130b3b25a7d
Step 3/3 : RUN echo "Dockerfile"
 ---> Running in 75c63b42acbb

Dockerfile  ## echoi
 ---> b4b737ac1ec0
Removing intermediate container 75c63b42acbb
Successfully built b4b737ac1ec0
{% endhighlight %}

<br>
<br>

## Build한 이미지 확인해보기

- 확인해보면 생성하려는 이미지와 베이스이미지가 모두 이미지 생성되어있음을 확인할 수 있음

  - 생성하려는 이미지 - dockerfile:0.1
  
  - Base 이미지 - centos:latest

{% highlight bash %}
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
dockerfile          0.1                 b4b737ac1ec0        13 seconds ago      220 MB
docker.io/centos    latest              0f3e07c0138f        2 weeks ago         220 MB
{% endhighlight %}

<br>
<br>

## Build한 이미지 컨테이너로 띄우기 위해 실행하기

- docker run $이미지명:$태그
  <br>docker run --name dockerC -d -p 8080:8080 -v /root/centos/data:/data dockerfile:0.1

- 옵션

  - `--name dockerC` - 컨테이너명 : nginxC

  - `-d` - 백그라운드 실행
  
  - `-p 8080:8080` - 포트포워딩 : 로컬포트(8080):컨테이너포트(8080)
  
  - `-v /root/centos/data:/data` - 볼륨설정 : 로컬 /root/centos/data와 컨테이너 /data를 연결, 파일을 보낼 수 있도록

{% highlight bash %}
$ docker run --name dockerC -d -p 8080:8080 -v /root/centos/data:/data dockerfile:0.1
25b6ab0e1a8f6c41fc3b09521d50ff02cea95df55a56eb1be9464f395c58a173
{% endhighlight %}

<br>
<br>

## 컨테이너 확인하기

- 조회 안 됨

{% highlight bash %}
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
{% endhighlight %}

- 정지된 것으로 확인

{% highlight bash %}
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS                          PORTS               NAMES
25b6ab0e1a8f        dockerfile:0.1      "/bin/bash"         About a minute ago   Exited (0) About a minute ago                       dockerC
{% endhighlight %}

- 왜일까..

<br>

#### 왜 Docker 컨테이너가 죽는지 확인해보자

- 포그라운드로 실행하면 메시지가 보이니까 포그라운드로 dockerC2로 생성해보자

  - 별 로그 없이 또 정지되어있음
  
{% highlight bash %}
$ docker run --name dockerC2 -p 8080:8080 -v /root/centos/data:/data dockerfile:0.1
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
0afffa5ce7db        dockerfile:0.1      "/bin/bash"         28 seconds ago      Exited (0) 27 seconds ago                       dockerC2
25b6ab0e1a8f        dockerfile:0.1      "/bin/bash"         3 minutes ago       Exited (0) 3 minutes ago                        dockerC
{% endhighlight %}

- Exited (0) 요게 왜 나오는 걸까

  - 찾아본 결과, docker는 완전한 서버를 만드는 느낌이 아니라, 그 안에 있는걸 실행하도록 해주는 느낌
  
  - 그 말인 즉슨.. 데몬이 돌고 있거나 쨋든 뭔가 물려있어야 한다는 의미인듯

- while로 물려보자 - dockerC3

  - docker 컨테이너는 계속 떠있게 됨

{% highlight bash %}
$ docker run --name dockerC -d -p 8080:8080 -v /root/centos/data:/data dockerfile:0.1 /bin/bash -c "while true; do echo "wow"; sleep 1000; done"
aadc93f1a079fe64ebf630022b5a9a289952d4b337bd966f87c777879d1ea23f
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
aadc93f1a079        dockerfile:0.1      "/bin/bash -c 'whi..."   30 seconds ago      Up 28 seconds       0.0.0.0:8080->8080/tcp   dockerC3
$ docker attach dockerC3
오 못나온다...:0
이유 : wow라는 echo가 계속 찍히는 상황이므로 쉘 스크립트를 쓸 수 없음
{% endhighlight %}

<br>
<br>

## docker 컨테이너 한번에 지우기

- docker 컨테이너 지우는 명령인 docker rm은, 여러 인자를 받음

- docker ps -a -q : 도커의 모든 컨테이너 ID 확인

{% highlight bash %}
$docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                            PORTS               NAMES
6ea0d0497a3f        dockerfile:0.1      "/bin/bash -c 'whi..."   2 minutes ago       Exited (137) About a minute ago                       dockerC12
96cbb03741ce        dockerfile:0.1      "/bin/bash -c 'whi..."   2 minutes ago       Exited (0) 2 minutes ago                              dockerC11
064a474ba340        dockerfile:0.1      "/bin/bash -c 'whi..."   5 minutes ago       Exited (137) 5 minutes ago                            dockerC10
ffdd66e8fc8e        dockerfile:0.1      "/bin/bash -c 'whi..."   6 minutes ago       Exited (137) 6 minutes ago                            dockerC9
23a838cb0f54        dockerfile:0.1      "/bin/bash -c 'whi..."   6 minutes ago       Exited (0) 6 minutes ago                              dockerC8
cb590266ed50        dockerfile:0.1      "/bin/bash -c 'whi..."   8 minutes ago       Exited (0) 8 minutes ago                              dockerC7
2b8020b7e411        dockerfile:0.1      "/bin/bash -c 'whi..."   8 minutes ago       Exited (1) 8 minutes ago                              dockerC6
42ae3a6bfa21        dockerfile:0.1      "/bin/bash -c 'whi..."   9 minutes ago       Exited (1) 9 minutes ago                              dockerC5
befb9bf22767        dockerfile:0.1      "/bin/bash -c 'whi..."   13 minutes ago      Exited (137) 9 minutes ago                            dockerC4
aadc93f1a079        dockerfile:0.1      "/bin/bash -c 'whi..."   16 minutes ago      Exited (137) 14 minutes ago                           dockerC3
0afffa5ce7db        dockerfile:0.1      "/bin/bash"              22 minutes ago      Exited (0) 22 minutes ago                             dockerC2
25b6ab0e1a8f        dockerfile:0.1      "/bin/bash"              24 minutes ago      Exited (0) 24 minutes ago                             dockerC

$ docker rm $(docker ps -a -q)

### 깔끔
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
{% endhighlight %}

<br>
<br>

## docker 이미지 한번에 지우기

- docker 컨테이너와 비슷

- docker 이미지 지우는 명령인 docker rmi은, 여러 인자를 받음

- docker images -q : 이미지 ID만 받아오기

{% highlight bash %}
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
dockerfile          0.1                 b4b737ac1ec0        39 minutes ago      220 MB
docker.io/centos    latest              0f3e07c0138f        2 weeks ago         220 MB

$ docker rmi $(docker images -q)
Untagged: dockerfile:0.1
Deleted: sha256:b4b737ac1ec0497e6c71b4f0dc2ca72edaf61a24e678bb2780d95ac141f289c1
Deleted: sha256:976c26f7b905719a19720db9c4db1c4d356c40b8ed5b1f817183c955684aa8db
Deleted: sha256:e84c4c379b0476022c4a915f1c711a65b6e001e4bd99c3cf7caf0d54f4f771e4
Untagged: docker.io/centos:latest
Untagged: docker.io/centos@sha256:f94c1d992c193b3dc09e297ffd54d8a4f1dc946c37cbeceb26d35ce1647f88d9
Deleted: sha256:0f3e07c0138fbe05abcb7a9cc7d63d9bd4c980c3f61fea5efa32e7c4217ef4da
Deleted: sha256:9e607bb861a7d58bece26dd2c02874beedd6a097c1b6eca5255d5eb0d2236983

### 깔끔
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
{% endhighlight %}
<br>
<br>

## 알게 된 점

- 도커는 아예 서버환경을 구축해놓고 사용자를 대기하는 것은 아니고, 무언가 프로세스가 돌아갈 때 떠있고 안 돌아가면 정지됨
  
  - 백그라운드로 `&` 띄워놓으면 그냥 정지됨(포그라운드 프로세스로 돌아가야 하는걸까?)

- 일반적으로 web server, was server, ... etc를 띄워놓으면 상관 없을 듯

- 일반 옵션에서는 에코로 띄워놓으면 접속해도 쉘을 못쓰니까... 에코를 띄우는건 소용이 없음
