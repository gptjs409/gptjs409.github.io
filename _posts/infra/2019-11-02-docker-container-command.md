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
$ sudo docker container run -d --dns 192.168.1.1 nginx
5b7f487230c6531a1a8adf525e861854c89619dfd0491246276d420886766934

// container 생략 가능
$ sudo docker container run -d --dns 192.168.1.1 nginx
5b7f487230c6531a1a8adf525e861854c89619dfd0491246276d420886766934
{% endhighlight %}

<br>

#### MAC 주소 지정 (예제)

{% highlight bash %}
$ sudo docker container run -d --mac-address="92:d0:c6:0a:21:32" centos
93e4fd73bf5572d599b4b88a2dcaa7712c8ed552cd46eb0820dbc9360aa3a566
$ sudo docker container inspect --format="{{ .Config.MacAddress }}" 93e4
92:d0:c6:0a:21:32

// container 생략 가능
$ sudo docker run -d --mac-address="92:d0:c6:0a:21:32" centos
93e4fd73bf5572d599b4b88a2dcaa7712c8ed552cd46eb0820dbc9360aa3a566
$ sudo docker inspect --format="{{ .Config.MacAddress }}" 93e4
92:d0:c6:0a:21:32
{% endhighlight %}

- docker container run -d --mac-address="92:d0:c6:0a:21:32" centos
  
<br>

#### 호스트명과 IP 주소 정의 (예제)

{% highlight bash %}
$ sudo docker container run -it --add-host test.com:192.168.1.1 centos

// container 생략 가능
$ sudo docker container run -it --add-host test.com:192.168.1.1 centos

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
$ sudo docker container run -it --hostname www.test.com --add-host node1.test.com:192.168.1.1 centos

// container 생략 가능
$ sudo docker run -it --hostname www.test.com --add-host node1.test.com:192.168.1.1 centos

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
$ sudo docker network create -d bridge webap-net
47fc2eb62179ae4591db1a5672b2f775ecc6b20d43f01a04c82ec1ba9085d65c
$ sudo docker run --net=webap-net -it centos
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

  - \--cpu-shares, -c : CPU의 사용 배분(비율), 기본 1024
  
  - \--memory, -m : 사용할 메모리를 제한하여 실행(단위는 b, k, m, g)
  
  - \--volume=\[호스트의 디렉토리]:\[컨테이너의 디렉토리], -v : 호스트와 컨테이너의 디렉터리를 공유

<br>

#### CPU 시간의 상대 비율과 메모리 사용량 지정 (예제)

{% highlight bash %}
$ sudo docker container run --cpu-shares=512 --memory=1g centos

// container 생략 가능
$ sudo docker run --cpu-shares=512 --memory=1g centos
{% endhighlight %}

- docker container run \--cpu-shares=512 --memory=1g centos

- CPU 시간을 기본의 반정도로 설정

<br>

#### 디렉토리 공유 (예제)

{% highlight bash %}
$ sudo docker container run -v /root/sun/web:/usr/share/nginx/html nginx

// container 생략 가능
$ sudo docker run -v /root/sun/web:/usr/share/nginx/html nginx
{% endhighlight %}

- 호스트 OS와 컨테이너 안의 디렉토리를 공유 : -v(--volume)옵션 사용

- 호스트 OS의 /root/sun/web 디렉터리와 컨테이너의 /usr/share/nginx/html 디렉터리를 공유

<br>
<br>

## 컨테이너 생성 및 시작하는 환경 지정

- docker container run \[환경설정 옵션] 이미지명\[:태그명] \[인수]

- 컨테이너의 환경변수나 컨테이너 안의 작업 디렉터리를 지정하여 컨테이너를 생성/실행

- 주요 옵션

  - \--env=\[환경변수], -e : 환경변수를 설정
  
  - \--env-file=\[파일명] : 환경변수를 파일로부터 설정
  
  - \--read-only=\[true \| false] : 컨테이너의 파일 시스템을 읽기 전용으로 만듦
  
  - \--workdir=\[패스], -w : 컨테이너의 작업 디렉터리를 지정
  
  - \-u, --user=\[사용자명\ : 사용자명 또는 UID 지정
  
<br>

#### 환경변수 설정 (예제)

{% highlight bash %}
$ sudo docker container run -it -e foo=bar centos /bin/bash

// container 생략 가능
$ sudo docker run -it -e foo=bar centos /bin/bash

[root@6486caeec0a7 /]$ set | grep foo
foo=bar
{% endhighlight %}

- \-e 옵션을 사용해 환경변수 등록 가능

<br>

#### 환경변수의 일괄 설정 (예제)

{% highlight bash %}
$ echo foo=bar >> env_list
$ echo bar=foo >> env_list
$ cat env_list
foo=bar
bar=foo

$ sudo docker container run -it --env-file=env_list centos /bin/bash

// container 생략 가능
$ sudo docker run -it --env-file=env_list centos /bin/bash

[root@f39730d32d78 /]$ set | grep foo
bar=foo
foo=bar
{% endhighlight %}

- 환경변수를 정의한 파일로부터 일괄적으로도 환경변수 등록 가능

<br>

#### 작업 디렉터리 설정 (예제)

{% highlight bash %}
$ sudo docker container run -it -w=/sun centos /bin/bash

// container 생략 가능
$ sudo docker run -it -w=/sun centos /bin/bash

[root@ad4e56e15490 sun]$ pwd
/sun
{% endhighlight %}

- 작업 디렉터리 설정 가능

<br>
<br>

## 가동중인 컨테이너 목록 표시

- docker container ls \[옵션]

- docker 상에서 작동하는 컨테이너의 가동 상태를 확인 → 가동중인 컨테이너 상태가 표시

- 주요 옵션

  - \--all, -a : 실행 중/정지 중인 것도 포함하여 모든 컨테이너 표시
  
  - \--filter, -f : 표시할 컨테이너의 필터링
  
  - \--format : 표시 포맷을 지정
  
  - \--last, -n : 마지막으로 실행된 n건의 컨테이너만 표시
  
  - \--latest, -l : 마지막으로 실행된 컨테이너만 표시
  
  - \--no-trunc : 정보를 생략하지 않고 표시
  
  - \--quiet, -q : 컨테이너 ID만 표시
  
  - \--size, -s : 파일 크기 표시
  
<br>

#### 컨테이너 목록 표시 (예제)

{% highlight bash %}
$ sudo docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
883baf01dc40        centos              "/bin/bash"         7 hours ago         Up 6 seconds                            test2

// 아래와 같은 명령어임
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
883baf01dc40        centos              "/bin/bash"         7 hours ago         Up 1 second                             test2
{% endhighlight %}

- docker container ls

  - docker ps와 같은 명령
  
- 명령 결과

  - CONTAINER ID : 컨테이너 ID
  
  - IMAGE : 컨테이너 바탕 이미지
  
  - COMMAND : 컨테이너 안에서 실행되고 있는 명령
  
  - CREATED : 컨테이너 작성 후 경과 시간
  
  - STATUS : 컨테이너 상태 (restarting \| running \| paused \| exited)
  
  - PORTS : 할당된 포트
  
  - NAMES : 컨테이너 이름

<br>

#### 컨테이너 목록의 필터링 (예제)

{% highlight bash %}
$ sudo docker container ls -a -f name=test1
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                   PORTS               NAMES
cdc978e92871        centos              "/bin/cal"          7 hours ago         Exited (0) 7 hours ago                       test1
$ sudo docker container ls -a -f exited=0
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                         PORTS               NAMES
6486caeec0a7        centos              "/bin/bash"              32 minutes ago      Exited (0) 31 minutes ago                          vibrant_kowalevski
c84f8b6f627c        nginx               "nginx -g 'daemon of…"   40 minutes ago      Exited (0) 40 minutes ago                          gallant_ptolemy
c7b1179a2e1d        nginx               "nginx -g 'daemon of…"   42 minutes ago      Exited (0) 40 minutes ago                          kind_hugle
2223286ed06d        centos              "/bin/bash"              About an hour ago   Exited (0) About an hour ago                       elated_aryabhata
641a54b142fe        centos              "/bin/bash"              About an hour ago   Exited (0) About an hour ago                       gracious_lewin
60011eddb4d3        centos              "/bin/bash"              About an hour ago   Exited (0) About an hour ago                       trusting_bhaskara
2b45b2f7112c        centos              "/bin/bash"              About an hour ago   Exited (0) 12 minutes ago                          hardcore_rubin
494489da99ce        centos              "/bin/bash"              About an hour ago   Exited (0) About an hour ago                       gifted_keldysh
42d91f1b800c        centos              "/bin/bash"              About an hour ago   Exited (0) About an hour ago                       reverent_cori
93e4fd73bf55        centos              "/bin/bash"              3 hours ago         Exited (0) 3 hours ago                             elastic_aryabhata
5b7f487230c6        nginx               "nginx -g 'daemon of…"   3 hours ago         Exited (0) 3 hours ago                             dreamy_golick
3bbdadd96119        nginx               "nginx -g 'daemon of…"   3 hours ago         Exited (0) 3 hours ago                             gallant_hawking
cdc978e92871        centos              "/bin/cal"               7 hours ago         Exited (0) 7 hours ago                             test1
68bb8c1317fa        nginx               "nginx -g 'daemon of…"   10 hours ago        Exited (0) 10 hours ago                            webserver
{% endhighlight %}

- \-a : 정지 중인 컨테이너까지 모두 표시

- \-f : 컨테이너 필터링시 사용

  - key=value 형식으로 필터링 지정

<br>

#### 출력 형식의 지정

|플레이스 홀더|설명|
|.ID|컨테이너 ID|
|.Image|이미지 ID|
|.Command|실행 명령|
|.CreatedAt|컨테이너 작성 시간|
|.RunningFor|컨테이너 가동 시간|
|.Ports|공개 포트|
|.Status|컨테이너 상태|
|.Size|컨테이너 디스크 크기|
|.Names|컨테이너명|
|.Mounts|볼륨 마운트|
|.Networks|네트워크명|

<br>

#### 컨테이너 목록의 출력 형식 지정 (예제)

{% highlight bash %}
$ sudo docker container ls -a --format "{{.Names}}: {{.Status}}"
recursing_jones: Exited (127) 30 minutes ago
fervent_benz: Exited (130) 34 minutes ago
vibrant_kowalevski: Exited (0) 36 minutes ago
gallant_ptolemy: Exited (0) 45 minutes ago
kind_hugle: Exited (0) 46 minutes ago
elated_aryabhata: Exited (0) About an hour ago
gracious_lewin: Exited (0) About an hour ago
sad_buck: Created
trusting_bhaskara: Exited (0) About an hour ago
infallible_hertz: Created
hardcore_rubin: Exited (0) 17 minutes ago
gifted_keldysh: Exited (0) About an hour ago
reverent_cori: Exited (0) About an hour ago
great_bose: Exited (130) About an hour ago
confident_kilby: Exited (130) 3 hours ago
modest_allen: Exited (130) 3 hours ago
elastic_aryabhata: Exited (0) 3 hours ago
dreamy_golick: Exited (0) 3 hours ago
gallant_hawking: Exited (0) 3 hours ago
test2: Up 17 minutes
test1: Exited (0) 8 hours ago
webserver: Exited (0) 10 hours ago
{% endhighlight %}

<br>

#### 컨테이너 목록을 표 형식으로 출력 (예제)

{% highlight bash %}
$ sudo docker container ls -a --format "{{.Names}}\t{{.Status}}\t {{.Mounts}}"
recursing_jones Exited (127) 37 minutes ago
fervent_benz    Exited (130) 41 minutes ago
vibrant_kowalevski      Exited (0) 43 minutes ago
gallant_ptolemy Exited (0) 52 minutes ago        /root/sun/web
kind_hugle      Exited (0) 52 minutes ago        /root/sun/web
elated_aryabhata        Exited (0) About an hour ago
gracious_lewin  Exited (0) About an hour ago
sad_buck        Created
trusting_bhaskara       Exited (0) About an hour ago
infallible_hertz        Created
hardcore_rubin  Exited (0) 24 minutes ago
gifted_keldysh  Exited (0) About an hour ago
reverent_cori   Exited (0) About an hour ago
great_bose      Exited (130) About an hour ago
confident_kilby Exited (130) 3 hours ago
modest_allen    Exited (130) 3 hours ago
elastic_aryabhata       Exited (0) 3 hours ago
dreamy_golick   Exited (0) 3 hours ago
gallant_hawking Exited (0) 3 hours ago
test2   Up 24 minutes
test1   Exited (0) 8 hours ago
webserver       Exited (0) 10 hours ago
{% endhighlight %}

<br>
<br>

## 컨테이너 가동 확인

- docker container stats \[컨테이너 식별자]

- 컨테이너 가동 상태를 확인, 컨테이너 가동 상태가 목록으로 표시 됨

<br>

#### 컨테이너 가동 확인 (예제)

{% highlight bash %}
$ sudo docker container stats test2
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT   MEM %               NET I/O             BLOCK I/O           PIDS
883baf01dc40        test2               0.00%               528KiB / 3.701GiB   0.01%               656B / 0B           0B / 0B             1
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT   MEM %               NET I/O             BLOCK I/O           PIDS
883baf01dc40        test2               0.00%               528KiB / 3.701GiB   0.01%               656B / 0B           0B / 0B             1
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT   MEM %               NET I/O             BLOCK I/O           PIDS
883baf01dc40        test2               0.00%               528KiB / 3.701GiB   0.01%               656B / 0B           0B / 0B             1
{% endhighlight %}

- test2라는 이름의 컨테이너 가동 상황 확인

  - 확인 종료시 `Ctrl` + `c`로 명령 종료

- 명령 결과

|항목|설명|
|---|---|
|CONTAINER ID|컨테이너 식별자|
|NAME|컨테이너명|
|CPU %|CPU 사용률|
|MEM USAGE/LIMIT|메모리 사용량/컨테이너에서 사용할 수 있는 메모리 제한|
|MEM %|메모리 사용률|
|NET I/O|네트워크 I/O|
|BLOCK I/O|블록 I/O|
|PIDS|PID(Windows 컨테이너 제외)|

<br>

#### 프로세스 확인 (예제)

{% highlight bash %}
$ sudo docker container top test2
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                20093               20078               0                   04:01               pts/0               00:00:00            /bin/bash
{% endhighlight %}

- 컨테이너에서 실행 중인 프로세스 확인

<br>
<br>

## 컨테이너 시작

- docker container start \[옵션] \<컨테이너 식별자> \[컨테이너 식별자]

- 주요 옵션

  - \--attach, -a : 표준 출력, 표준 오류 출력을 엶
  
  - \--interactive, -i : 컨테이너의 표준 입력을 엶

<br>

#### Docker 컨테이너 시작 (예제)

{% highlight bash %}
$ sudo docker start a
Error response from daemon: Multiple IDs found with provided prefix: a
Error: failed to start containers: a
$ sudo docker start ad
ad
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
ad4e56e15490        centos              "/bin/bash"         About an hour ago   Up 2 seconds                            recursing_jones
{% endhighlight %}

- container ID가 ad4e56e15490인 컨테이너를 시작하려면 위와 같이 하면 됨

- 컨테이너식별자

  - ID 시작부터 n자리까지만 적으면 되는데, 다른 컨테이너와 겹치지 않고 해당 컨테이너만 지칭할 수 있으면 몇자리던 상관없음
  
  - 아니면 컨테이너명(recursing_jones - 생략불가)

<br>
<br>

## 컨테이너 정지

- docker container stop \[옵션] \<컨테이너 식별자> \[컨테이너 식별자]

- 주요 옵션

  - \--time, -t : 컨테이너의 정지 시간을 지정(기본값 : 10초)

<br>

#### 컨테이너 정지 (예제)

{% highlight bash %}
$ sudo docker container stop -t 2 ad
ad
{% endhighlight %}

- 강제적으로 정지할 때는 docker container kill을 사용할 것(사용 방법 동일)

<br>
<br>

## 컨테이너 재시작

- docker container restart \[옵션] \<컨테이너 식별자> \[컨테이너 식별자]

- 주요 옵션

  - \--time, -t : 컨테이너의 재시작 시간을 지정(기본값 : 10초)

- 컨테이너 안에서 실행하는 명령의 종료 스테이터스에 따라 컨테이너를 자동으로 재시작하고 싶은 경우 docker container run에서 '--restart'옵션 사용

<br>

#### 컨테이너 재시작 (예제)

{% highlight bash %}
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
ad4e56e15490        centos              "/bin/bash"         About an hour ago   Up 17 seconds                           recursing_jones
$ sudo docker restart -t 2 ad
ad
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
ad4e56e15490        centos              "/bin/bash"         About an hour ago   Up 1 second                             recursing_jones
{% endhighlight %}

- 2초 후 재시작

<br>
<br>

## 컨테이너 삭제

- docker container rm \[옵션] \<컨테이너 식별자> \[컨테이너 식별자]

- 정지하고 있는 컨테이너 삭제

- 주요 옵션

  - \--force, -f : 실행 중인 컨테이너를 강제로 삭제
  
  - \--volumes, -v : 할당한 볼륨 삭제
  
<br>

#### 컨테이너 삭제 (예제)

- 사용중이라면 다음과 같이 나타나고, `-f` 옵션을 통해 강제로 삭제할 수 있음

{% highlight bash %}
// 대상 : ad4e56e15490
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
ad4e56e15490        centos              "/bin/bash"         About an hour ago   Up 6 minutes                            recursing_jones
$ sudo sudo docker container rm ad4
Error response from daemon: You cannot remove a running container ad4e56e15490095153ba5b6b1e2847edbf06caab60b640c9c6f7c8e25e4983ef. Stop the container before attempting removal or force remove
$ sudo docker container rm -f ad4
ad4
{% endhighlight %}

- 정지중이라면 다음과 같이 삭제됨

{% highlight bash %}
$ sudo docker rm 22232
22232
{% endhighlight %}

- 정지 중인 모든 컨테이너 삭제 

{% highlight bash %}
// container 생략 불가
$ sudo docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Deleted Containers:
f39730d32d78a4dbf2da73f74ecb8be1ce8966260e357316e63c47f7e60b0eb8
6486caeec0a73ad33003ffc04fb7dcf1f2fd96f3dfbc1ad8573fa13faed8f537
c84f8b6f627c6eb794df0b10106ab77e6dc5a7e7c7a4f9cdc7a098b0563fa414
c7b1179a2e1d2feeb2c38ccf60565ff9252995df3e26acc1e56dc82165d5fec9
641a54b142fee2f742cb8d5094290c203b6bae1bdb84ead6fa0e2f812783f32f
f3d0bf98c5675b73f78dd8fafa6fa3a721ef74777a70a2906386d05d4b954154
60011eddb4d3696538f29654de3b77b93e122c9e4eb4deb5a2b319e65ebf0ed2
a76b19584d43787404f6ca82d76753f1519c62319239251782d7e5bd6046dd12
2b45b2f7112c74a83ef1d71eac287d7587471dff663f0d59b503133d96512c97
494489da99ce2138ba5925a1d6543c89cd6dbec57ca496162b29ef4f40ee9c7d
42d91f1b800c51d152add0551926b097ae650af2250f0aeba5c9a331ad2c7571
fb4dbc0b9ced8e307e3d05968efbf918ee9539baeff3b0ad9e80bb128854f83b
6da42c4274a87397e42b4771b7e8dcc449571953f3a57b6caa7713938c87a5e0
564dee65a1f2d9b9c2556757976abfb38a54d8566890c2781b5b7afab9677882
93e4fd73bf5572d599b4b88a2dcaa7712c8ed552cd46eb0820dbc9360aa3a566
5b7f487230c6531a1a8adf525e861854c89619dfd0491246276d420886766934
3bbdadd9611949c97b8965c3f7b115d34ad70a067161199b6112216e8e067c6d
cdc978e92871e08f6cd60f55d5e669a4c8e0fd0265c0ba00b0b773a471d846f7
68bb8c1317fa62cd0f631b3f429ea439b8805789f7e529f236d87e784fdbc280
{% endhighlight %}

<br>
<br>

## 컨테이너 중단/재개

- docker container pause \<컨테이너 식별자>

- 작동 중인 프로세스를 모두 중단

<br>

#### 컨테이너 중단 (예제)

{% highlight bash %}
$ sudo docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
883baf01dc40        centos              "/bin/bash"         8 hours ago         Up About an hour                        test2
$ sudo docker container pause test2
test2
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
883baf01dc40        centos              "/bin/bash"         8 hours ago         Up About an hour (Paused)                       test2
{% endhighlight %}

- pause로 일시 중단

- status 확인시 (Paused)로 표시

<br>

#### 컨테이너 재개 (예제)

{% highlight bash %}
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
883baf01dc40        centos              "/bin/bash"         8 hours ago         Up About an hour (Paused)                       test2
$ sudo docker container unpause test2
test2
$ sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
883baf01dc40        centos              "/bin/bash"         8 hours ago         Up About an hour                        test2
{% endhighlight %}

- unpause로 컨테이너 재개
