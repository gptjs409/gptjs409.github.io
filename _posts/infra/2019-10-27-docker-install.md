---
layout: post
title:  "[Docker] 도커 설치/작동확인/Nginx 띄워보기"
date:   2019-10-27 20:14:43
author: Choi HyeSun
categories: infra
tags:
  - Docker
  - 도커
  - Nginx
  - 엔진엑스
  - 웹서버
  - WebServer
---

## Docker 설치

#### Docker의 클라이언트 툴

- Docker는 Linux 커널 기능을 사용하기 때문에 보통은 Linux 배포판 상에서 작동

  - 개발 환경에서 이용하기 위한 클라이언트 PC용 툴을 제공

- Docker for Mac

  - macOS용 'Docker for Mac'
  
  - macOS상에서 네이티브 애플리케이션으로 움직임
  
  - macOS 10.10 Yosemite에서 이용 가능하게 된 Hypervisor 프레임워크인 'xhyve'를 사용

- Docker for Windows

  - windows용 'Docker for Windows'
  
  - Windows 10 이후에 이용가능하게 됨
  
  - Microsoft가 제공하는 하이퍼바이저인 x64용 가상화 시스템 'Hyper-V'를 사용
  
  - Windows 10 Pro, Windows 10 Enterprise, Windows 10 Education에서 작동
  
  - OS에서 Hyper-V를 유효화하면 Oracle VirtualBox와 같은 다른 가상화 툴은 사용할 수 없음!

- Docker ToolBox

  - Oracle이 제공하는 가상화 툴
  
  - 위의 두 개를 모두 사용할 수 없는 오래된 머신을 사용할 때 사용
  
> Docker for Mac 17.12.0-ce Edge, Docker for Windows 18.02.0-ce Edge 이후 버전
>
> Docker 컨테이너의 오케스트레이션 툴인 "Kubernetes"가 포함

<br>

#### 다운로드

- Docker for Mac

  - 다운로드 [LINK](https://hub.docker.com/editions/community/docker-ce-desktop-mac)

  - 공식문서 [LINK](https://docs.docker.com/docker-for-mac/)

- Docker for Windows

  - 다운로드 [LINK](https://hub.docker.com/editions/community/docker-ce-desktop-windows)

  - 공식문서 [LINK](https://docs.docker.com/docker-for-windows/)
  
- Docker ToolBox

  - 다운로드 [LINK](https://github.com/docker/toolbox/releases)

  - 공식문서 [LINK](https://docs.docker.com/toolbox/overview/)

- Kitematic

  - Docker ToolBox처럼 제공되는 레거시 솔루션

  - 다운로드 [LINK](https://github.com/docker/kitematic)
  
  - 공식문서 [LINK](https://docs.docker.com/kitematic/userguide/)

<br>

#### CentOS에서 Docker 다운로드

- 공식문서 [LINK](https://docs.docker.com/install/linux/docker-ce/centos/)

- 기존 버전 삭제

{% highlight bash %}
$ sudo yum remove -y docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
{% endhighlight %}

- 설치1. 필요한 유틸리티 선 설치

{% highlight bash %}
$ sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
{% endhighlight %}

- 설치 2. stable 레포 추가

{% highlight bash %}
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
{% endhighlight %}

- 설치 3. 최신버전 설치

{% highlight bash %}
$ sudo yum install -y docker-ce docker-ce-cli containerd.io
{% endhighlight %}

- 실행

{% highlight bash %}
sudo systemctl start docker
{% endhighlight %}

- HelloWorld 테스트 실행

{% highlight bash %}
sudo docker run hello-world
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:c3b4ada4687bbaa170745b3e4dd8ac3f194ca95b2d0518b417fb47e5879d9b5f
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
{% endhighlight %}

<br>
<br>

## Docker 작동 확인

#### Hello world 문자 띄워보기

- docker container run : Docker 컨테이너를 작성 및 실행

  - docker container run \<Docker 이미지명> \<실행할 명령>
  
  - docker container run : 컨테이너를 작성 및 싫애
  
  - \<Docker 이미지명> : 바탕이 되는 Docker 이미지
  
  - \<실행할 명령> : 컨테이너 안에서 실행할 명령
  
- docker container run ubuntu:latest /bin/echo 'Hello world'

{% highlight bash %}
// 이미지 없을 때
$ sudo docker container run ubuntu:latest /bin/echo 'Hello world'
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
7ddbc47eeb70: Pull complete
c1bbdc448b72: Pull complete
8c3b70e39044: Pull complete
45d437916d57: Pull complete
Digest: sha256:6e9f67fa63b0323e9a1e587fd71c561ba48a034504fb804fd26fd8800039835d
Status: Downloaded newer image for ubuntu:latest
Hello world

// 이미지 있을 때
$ sudo docker container run ubuntu:latest /bin/echo 'Hello world'
Hello world
{% endhighlight %}

- 실행된 순서

  - Docker 컨테이너의 바탕이 되는 이미지를 확인, 없다면 Docker 레파지토리에서 Docker 이미지를 다운로드
  
  - 다운로드 완료되거나, 있다면 컨테이너가 시작됨
  
  - Linux의 echo 명령이 실행

- 로컬 캐시

  - 로컬 환경에 다운로드 된 Docker 이미지
  
<br>

#### Docker 버전 확인

- docker version 명령을 통해 설치한 Docker 버전 확인

{% highlight bash %}
sudo docker version
Client: Docker Engine - Community
 Version:           19.03.4
 API version:       1.40
 Go version:        go1.12.10
 Git commit:        9013bf583a
 Built:             Fri Oct 18 15:52:22 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.4
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.10
  Git commit:       9013bf583a
  Built:            Fri Oct 18 15:50:54 2019
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.10
  GitCommit:        b34a5c8af56e510852c35414db4c1f4fa6172339
 runc:
  Version:          1.0.0-rc8+dev
  GitCommit:        3e425f80a8c931f88e6d94a8c831b9d5aa481657
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
{% endhighlight %}

<br>

#### Docker 실행 환경 확인

- docker system info 명령을 통해 Docker 실행 환경의 상세 설정을 표시

{% highlight bash %}
$ sudo docker system info
Client:
 Debug Mode: false

Server:
 Containers: 6                       // 컨테이너 수
  Running: 0
  Paused: 0
  Stopped: 6
 Images: 145
 Server Version: 19.03.4             // Docker 버전
 Storage Driver: overlay2            // Storage Driver 종류
  Backing Filesystem: xfs
  Supports d_type: false
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: b34a5c8af56e510852c35414db4c1f4fa6172339
 runc version: 3e425f80a8c931f88e6d94a8c831b9d5aa481657
 init version: fec3683
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 3.10.0-693.2.2.el7.x86_64
 Operating System: CentOS Linux 7 (Core)
 OSType: linux                  // OS 종류
 Architecture: x86_64           // 아키텍쳐
 CPUs: 2
 Total Memory: 3.693GiB
 Name: dev-chs001-ncl
 ID: RABD:SMHH:GDIZ:MU5X:OMOZ:3PSA:BQHY:GUY2:2QQQ:U2UN:A5AI:72IM
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
{% endhighlight %}

<br>

#### Docker 디스크 이용 상황 확인

- docker system df 명령을 통해 Docker가 사용하고 있는 디스크의 이용 상황을 표시

{% highlight bash %}
$ sudo docker system df
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              7                   5                   3.458GB             1.909GB (55%)
Containers          6                   0                   53.87MB             53.87MB (100%)
Local Volumes       0                   0                   0B                  0B
Build Cache         0                   0                   0B                  0B
{% endhighlight %}

- 상세내용 확인시 -v 옵션

<br>
<br>

## Nginx 띄워보기

#### Nginx Docker 이미지 다운로드

- [공식 리포지토리의 Nginx 공식 이미지](https://hub.docker.com/_/nginx)

- 이미지 다운로드

{% highlight bash %}
$ sudo docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
8d691f585fa8: Pull complete
5b07f4e08ad0: Pull complete
abc291867bca: Pull complete
Digest: sha256:922c815aa4df050d4df476e92daed4231f466acc8ee90e0e774951b0fd7195a4
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
{% endhighlight %}

- 이미지 확인

{% highlight bash %}
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED                  SIZE
nginx               latest              540a289bab6c        Less than a second ago   126MB

// 동일
$ sudo docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED                  SIZE
nginx               latest              540a289bab6c        Less than a second ago   126MB
{% endhighlight %}

<br>

#### Nginx 작동

- 이미지 nginx를 사용하여 webserver라는 이름의 컨테이너 기동

  - 포트 허용 > 호스트OS 8080포트와 컨테이너 80포트 연결
  
  - 정상완료되면 컨테이너 ID가 나타남

{% highlight bash %}
$ docker container run --name webserver -d -p 8080:80 nginx
68bb8c1317fa62cd0f631b3f429ea439b8805789f7e529f236d87e784fdbc280

// container 제외해도 동일
$ docker run --name webserver -d -p 8080:80 nginx
{% endhighlight %}

<br>

#### Nginx 작동 확인

- 호스트OS IP 주소의 8080포트로 웹 브라우저에서 요청 → nginx 초기화면 나타남

![image](/img/2019-10-27/docker-install-001-web1.png)

- 컨테이너 확인

{% highlight bash %}
$ sudo docker container ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
68bb8c1317fa        nginx               "nginx -g 'daemon of…"   4 minutes ago       Up 4 minutes        0.0.0.0:8080->80/tcp   webserver

// container 제외해도 동일
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
68bb8c1317fa        nginx               "nginx -g 'daemon of…"   4 minutes ago       Up 4 minutes        0.0.0.0:8080->80/tcp   webserver
{% endhighlight %}

- 컨테이너 상세 내용 확인

{% highlight bash %}
// 모두 동일
$ sudo docker container stats webserver
$ sudo docker stats webserve
$ sudo docker container stats 68bb8c1317fa
$ sudo docker stats 68bb8c1317fa
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT    MEM %               NET I/O             BLOCK I/O           PIDS
68bb8c1317fa        webserver           0.00%               1.41MiB / 3.701GiB   0.04%               2.33kB / 2.26kB     0B / 0B             2
{% endhighlight %}

<br>

#### Nginx 기동 및 정지

- Docker 컨테이너 정지

{% highlight bash %}
$ docker stop webserver
webserver

// ID를 써도 됨
$ docker stop 68bb8c1317fa
68bb8c1317fa
{% endhighlight %}

- 정지된 컨테이너는 ps -a로 조회해야 함

{% highlight bash %}
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

$ sudo docker ps -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
68bb8c1317fa        nginx               "nginx -g 'daemon of…"   9 minutes ago       Exited (0) 45 seconds ago                       webserver
{% endhighlight %}

- Docker 컨테이너 기동
{% highlight bash %}
$ docker start webserver
webserver

// ID를 써도 됨
$ docker start 68bb8c1317fa
68bb8c1317fa
{% endhighlight %}
