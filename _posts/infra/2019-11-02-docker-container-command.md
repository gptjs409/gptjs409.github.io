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

  - \--attach, -a : 표준 입력(STDIN), 표준 출력(STDOUT), 표준 오류 출력(STDERR)에 attach

  - \--cidfile : 컨테이너 ID를 파일로 출력

  - \--detach, -d : 컨테이너를 생성하고 백그라운드에서 실행
  
  - \--interactive, -i : 컨테이너의 표준 입력을 엶
  
  - \--tty, -t : 단말기 디바이스를 사용

<br>

#### 달력을 출력하는 컨테이너(예제)

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

- docker container run -it --name "test1" centos /bin/cal

- `docker container run` : 컨테이너를 생성 및 실행

- `-it` : 콘솔에 결과를 출력하는 옵션

  - `-i` - 컨테이너의 표준 출력을 엶
  
  - `-t` - tty(단말 디바이스)를 확보

- `--name "test"` : 컨테이너명 지정

- `centos` : 이미지명 지정

- `/bin/cal` : 컨테이너에서 실행할 명령

<br>

#### 컨테이너 안에서 쉘(/bin/bash)을 실행(예제)
  
{% highlight bash %}
$ sudo docker container run -it --name "test2" centos /bin/bash
[root@883baf01dc40 /]#

// container 생략 가능
$ sudo docker run -it --name "test2" centos /bin/bash
[root@883baf01dc40 /]#
{% endhighlight %}

- exit로 쉘 종료

<br>
<br>

## 컨테이너 백그라운드 실행 (백그라운드에서 실행-detach mode)

- docker container run \[실행 옵션] 이미지명\[:태그명] \[인수]

- 지정할 수 있는 주요 옵션  

  - \--detach, -d : 백그라운드 실행
  
  - \--user, -u : 사용자명 지정

  - \--restart=\[no \| on-failure \| on-failure:횟수n \| always \| unless-stopped] 
  <br>: 명령의 실행 결과에 따라 재시작하는 옵션, --rm과 동시 사용 불가

  - \--rm : 명령 실행 후 컨테이너 자동 삭제, --restart와 동시 사용 불가

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

- docker container run \[네트워크 옵션] 이미지명\[:태그명] \[인수]

- 지정할 수 있는 주요 옵션

  - \--add-host=\[호스트명:IP주소] : 컨테이너의 /etc/hosts에 호스트명과 IP 주소를 정의
  
  - \--dns=\[IP주소] : 컨테이너용 DNS 서버의 IP 주소 지정
  
  - \--expose : 지정한 범위의 포트 번호를 할당
  
  - \--mac-address=\[MAC 주소] : 컨테이너의 MAC 주소를 지정
  
  - \--net=\[bridge \| none \| container:\<name \| id> \| host \| NETWORK] : 컨테이너의 네트워크 지정
  
  - \--hostname, -h : 컨테이너 자신의 호스트명을 지정
  
  - \--publish, -p\[호스트의 포트번호]:\[컨테이너의 포트 번호] : 호스트와 컨테이너의 포트 매핑
  
  - \--publish-all, -P : 호스트의 임의의 포트를 컨테이너에 할당
  
<br>
  
#### 컨테이너의 포트 매핑(예제)

{% highlight bash %}
$ sudo docker container run -d -p 8080:80 nginx
3bbdadd9611949c97b8965c3f7b115d34ad70a067161199b6112216e8e067c6d

// Container 생략 가능
$ sudo docker run -d -p 8080:80 nginx
3bbdadd9611949c97b8965c3f7b115d34ad70a067161199b6112216e8e067c6d
{% endhighlight %}

- sudo docker container run -d -p 8080:80 nginx

- nginx라는 이름의 이미지를 바탕으로 컨테이너를 생성

- 백그라운드에서 실행 (-d 옵션)

- 호스트OS 포트 8080과 컨테이너 포트 80을 매핑

- 포트 번호 할당시는 `--expose`사용, 호스트 머신의 임의의 포트 지정시에는 `-P` 옵션 사용

<br>

#### 컨테이너의 DNS 서버 지정(예제)

- docker container run -d --dns 192.168.1.1 nginx

- DNS서버는 IP로 지정
  
{% highlight bash %}
$ docker container run -d --dns 192.168.1.1 nginx
5b7f487230c6531a1a8adf525e861854c89619dfd0491246276d420886766934

// container 생략 가능
$ docker container run -d --dns 192.168.1.1 nginx
5b7f487230c6531a1a8adf525e861854c89619dfd0491246276d420886766934
{% endhighlight %}

<br>

#### MAC 주소 지정 (예제)

{% highlight bash %}
$ docker container run -d --mac-address="92:d0:c6:0a:21:32" centos
93e4fd73bf5572d599b4b88a2dcaa7712c8ed552cd46eb0820dbc9360aa3a566
$ docker container inspect --format="{{ .Config.MacAddress }}" 93e4
92:d0:c6:0a:21:32

// container 생략 가능
$ docker run -d --mac-address="92:d0:c6:0a:21:32" centos
93e4fd73bf5572d599b4b88a2dcaa7712c8ed552cd46eb0820dbc9360aa3a566
$ docker inspect --format="{{ .Config.MacAddress }}" 93e4
92:d0:c6:0a:21:32
{% endhighlight %}

- docker container run -d --mac-address="92:d0:c6:0a:21:32" centos
  
<br>

#### 호스트명과 IP 주소 정의 (예제)

{% highlight bash %}
$ docker container run -it --add-host test.com:192.168.1.1 centos

// container 생략 가능
$ docker container run -it --add-host test.com:192.168.1.1 centos

[root@564dee65a1f2 /]$ cat /etc/hosts
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
192.168.1.1     test.com
172.17.0.2      564dee65a1f2
{% endhighlight %}

- docker container run -it --add-host test.com:192.168.1.1 centos

<br>

#### 호스트명 설정 (예제)

{% highlight bash %}
$ docker container run -it --hostname www.test.com --add-host node1.test.com:192.168.1.1 centos

// container 생략 가능
$ docker run -it --hostname www.test.com --add-host node1.test.com:192.168.1.1 centos

[root@www /]$ cat /etc/hosts
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
192.168.1.1     node1.test.com
172.17.0.2      www.test.com www
{% endhighlight %}

- docker container run -it --hostname www.test.com --add-host node1.test.com:192.168.1.1 centos

#### 네트워크 설정

- \--net 옵션

  - 기본적으로 호스트OS와 브리지 연결을 함

  - `--net`옵션 사용시 네트워크 설정이 가능함
  
|설정값|설명|
|---|---|
|bridge|브리지 연결(기본값)을 사용|
|none|네트워크에 연결하지 않음|
|container:\[name \| id]|다른 컨테이너의 네트워크를 사용|
|host|컨테이너가 호스트 OS의 네트워크를 사용|
|NETWORK|사용자 정의 네트워크 사용|

<br>

#### 사용자 지정 네트워크 작성 (예제)
  
{% highlight bash %}
$ docker network create -d bridge webap-net
47fc2eb62179ae4591db1a5672b2f775ecc6b20d43f01a04c82ec1ba9085d65c
$ docker run --net=webap-net -it centos
[root@fb4dbc0b9ced /]#
{% endhighlight %}

- docker network create -d bridge webap-net

- docker network create : 사용자 정의 네트워크 작성

- Docker 네트워크 드라이버 또는 외부 네트워크 드라이버 플러그인을 사용해야 함

- 같은 네트워크에 여러 컨테이너가 연결할 수 있음

  - 사용자 정의 네트워크에 연결하면 컨테이너는 컨테이너명 또는 IP 주소로 서로 통신 가능

- 오버레이 네트워크나 커스텀 플러그인 사용시 멀티호스트에 대한 연결 가능

  - 컨테이너가 동일한 멀티호스트 네트워크에 연결되어 있으면 이 네트워크를 통한 통신 가능)


<br>
<br>

## 자원을 지정하여 컨테이너 생성 및 실행

- docker container run \[자원 옵션] 이미지명\[:태그명] \[인수]

- 지정할 수 있는 주요 옵션

  - \--cpu-shares, -c : CPU의 사용 배분(비율)
  
  - \--memory, -m : 사용할 메모리를 제한하여 실행(단위는 b, k, m, g)
  
  - \--volume=\[호스트의 디렉토리]:\[컨테이너의 디렉토리], -v : 호스트와 컨테이너의 디렉터리를 공유
  
- CPU 시간의 상대 비율과 메모리 사용량 지정

- 

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
