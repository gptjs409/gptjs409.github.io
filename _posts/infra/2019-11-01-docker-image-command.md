---
layout: post
title:  "[Docker] 도커 이미지 명령어"
date:   2019-11-01 21:51:56
author: Choi HyeSun
categories: infra
tags:
  - Docker
  - 도커
  - Docker image
  - 도커 이미지
  - image
  - 이미지
---

## [Docker Hub](https://hub.docker.com/)

- 공식 레파지토리 서비스

  - GitHub이나 Bigbucket과 같은 소스코드 관리 툴과 연계하여 코드를 빌드하는 기능을 갖춘 서비스
  
  - 실행 가능한 애플리케이션 이미지를 관리하는 기능을 갖춘 서비스

- 사용하여 물리 서버, 가상 머신, 클라우드 등 환경 상관없이 Docker 이미지를 배포할 수 있음

- 많은 이미지들이 공개되어 있음

  - 공식 Docker 이미지 외에도 사용자가 작성한 독자적인 Docker 이미지를 공개할 수 있음

  - 공식 Docker 이미지 : Official

- 메뉴

  - Repo Info : Docker 이미지 상세 정보 표시
  <br>레파지토리에서 공개되어 있는 버전 정보나 유의사항, 지원하는 Docker 버전 등
  
  - Tags : 태그 정보, lastest 태그 지정 가능 → 릴리즈마다 달라짐(레파지토리에 공개되어 있는 최신판 이미지)
  
  > Docker 이미지 지정
  >
  > 이미지명 [:태그명]
  >
  > 예) debian:7 → Debian의 버전 7을 베이스 이미지로 가지고 잇는 Docker 이미지

<br>
<br>

## 이미지 다운로드

- docker image pull \[옵션] 이미지명[:태그명]

- 태그명을 명시하면 해당 태그 이미지를 다운로드

  - 실습 : CentOS 버전 7(태그명 7)을 다운로드

{% highlight bash %}
$ sudo docker image pull centos:7
7: Pulling from library/centos
Digest: sha256:307835c385f656ec2e2fec602cf093224173c51119bbebd602c53c3653a3d6eb
Status: Downloaded newer image for centos:7
docker.io/library/centos:7

$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED                  SIZE
nginx               latest              540a289bab6c        Less than a second ago   126MB
centos              7                   67fa590cfc1c        2 months ago             202MB
{% endhighlight %}

- 태그명을 생략하면 최신판(latest)을 다운로드

  - 실습 : CentOS 최신판 다운로드
{% highlight bash %}
$ sudo docker image pull centos
Using default tag: latest
latest: Pulling from library/centos
729ec3a6ada3: Pull complete
Digest: sha256:f94c1d992c193b3dc09e297ffd54d8a4f1dc946c37cbeceb26d35ce1647f88d9
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest

$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED                  SIZE
nginx               latest              540a289bab6c        Less than a second ago   126MB
centos              latest              0f3e07c0138f        2 weeks ago              220MB
centos              7                   67fa590cfc1c        2 months ago             202MB
{% endhighlight %}

- `-a` 옵션 + 태그명 명시를 안하면 모든 태그 이미지 다운로드

  - 실습 : CentOS의 모든 이미지를 다운로드
  
{% highlight bash %}
$ sudo docker image pull -a centos
5.11: Pulling from library/centos
2068b24f564b: Pull complete
Digest: sha256:c40041f5894293d0df8f5c6c2049b92a82c53f1718ecdd73cbf3c1826a08ba4a
5: Pulling from library/centos
38892065247a: Pull complete
Digest: sha256:70fffd687ff9545662c30f9043108489c698662861cd5f76070f7e2cd350564f
6.10: Pulling from library/centos
06a11a3d840d: Pull complete
Digest: sha256:a0d7c1950ac8ada32d3ae7ff0c9c2c9cc061c605aeeec6b27fded5b58be16fab
6.6: Pulling from library/centos
5dd797628260: Pull complete
Digest: sha256:32b80b90ba17ed16e9fa3430a49f53ff6de0d4c76ad8631717a1373d5921fa26
6.7: Pulling from library/centos
cbddbc0189a0: Pull complete
Digest: sha256:4c952fc7d30ed134109c769387313ab864711d1bd8b4660017f9d27243622df1
6.8: Pulling from library/centos
7ce0cebb9dca: Pull complete
Digest: sha256:39abd0c8e375de6fb7334d42ec2a46643f34cbc1bbaf37e2b484065f05eaa7a2
6.9: Pulling from library/centos
831490506c47: Pull complete
Digest: sha256:6fff0a9edc920968351eb357c5b84016000fec6956e6d745f695e5a34f18ecd2
6: Pulling from library/centos
ff50d722b382: Pull complete
Digest: sha256:dec8f471302de43f4cfcf82f56d99a5227b5ea1aa6d02fa56344986e1f4610e7
7.0.1406: Pulling from library/centos
8cbae05c1fef: Pull complete
Digest: sha256:182394d34a0d73e987ffacf6d0e0caa3a0fe7bc1738798872b41e6245e0ee99d
7.1.1503: Pulling from library/centos
c6fb3d2c9cb8: Pull complete
Digest: sha256:713cbede4acabfe33b7901645d254dd0109cc7045dd00a6527e5391fbf72857a
7.2.1511: Pulling from library/centos
a8c7037c15e9: Pull complete
Digest: sha256:50cca1e74da4b6a4eb4ade029c8fdd4ee8564776801914d9bd89df8c6344add0
7.3.1611: Pulling from library/centos
9a598663ae73: Pull complete
Digest: sha256:8cc318f64d59d20035d7979824ced1c2aa01c8b2b5ab60b82044a43989262cfe
7.4.1708: Pulling from library/centos
Digest: sha256:8906d699cbd9406b07a105bedebc14a5945c200971b0a3a067aa245badc545b2

$ sudo docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED                  SIZE
nginx               latest              540a289bab6c        Less than a second ago   126MB
centos              latest              0f3e07c0138f        2 weeks ago              220MB
centos              7                   67fa590cfc1c        2 months ago             202MB
centos              6.6                 368c96d786ae        7 months ago             203MB
centos              6.7                 9f1de3c6ad53        7 months ago             191MB
centos              6.8                 82f3b5f3c58f        7 months ago             195MB
centos              6.9                 2199b8eb8390        7 months ago             195MB
centos              6.10                48650444e419        7 months ago             194MB
centos              7.0.1406            cc2cf48cc784        7 months ago             210MB
centos              7.1.1503            e1430271e2f9        7 months ago             212MB
centos              7.2.1511            9aec5c5fe4ba        7 months ago             195MB
centos              7.3.1611            c5d48e81b986        7 months ago             192MB
centos              7.4.1708            9f266d35e02c        7 months ago             197MB
centos              6                   d0957ffdf8a2        7 months ago             194MB
centos              5.11                b424fba01172        3 years ago              284MB
centos              5                   1ae98b2c895d        3 years ago              285MB
{% endhighlight %}

- URL로 이미지 취득할 수 있음

  - 이미지명에 이미지를 취득할 URL을 지정할 수도 있음
  
  - URL은 프로토콜(http://, https://) 제외하고 지정

<br>
<br>

## 이미지 목록

- docker image ls \[옵션] \[레파지토리명]

  - 또는 docker images \[옵션] \[레파지토리명]

- 지정할 수 있는 주요 옵션

|옵션|설명|
|---|---|
|-all, -a|모든 이미지 표시(중간 이미지도 모두 표시)|
|--digests|다이제스트를 표시할지 말지|
|--no-trunc|결과 모두 표시|
|--quiet, -q|ID만 표시|

<br>

{% highlight bash %}
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED                  SIZE
nginx               latest              540a289bab6c        Less than a second ago   126MB

$ sudo docker images -a
REPOSITORY          TAG                 IMAGE ID            CREATED                  SIZE
nginx               latest              540a289bab6c        Less than a second ago   126MB

$ sudo docker images --digests
REPOSITORY          TAG                 DIGEST                                                                    IMAGE ID            CREATED                  SIZE
nginx               latest              sha256:922c815aa4df050d4df476e92daed4231f466acc8ee90e0e774951b0fd7195a4   540a289bab6c        Less than a second ago   126MB

$ sudo docker images --no-trunc
REPOSITORY          TAG                 IMAGE ID                                                                  CREATED                  SIZE
nginx               latest              sha256:540a289bab6cb1bf880086a9b803cf0c4cefe38cbb5cdefa199b69614525199f   Less than a second ago   126MB

$ sudo docker images --quiet
540a289bab6c

$ sudo docker images -q
540a289bab6c
{% endhighlight %}

- Docker 이미지ID

  - 이미지를 작성하면 고유한 이미지 ID(Image ID)가 부여됨
  
  - 이미지 ID는 랜덤한 문자열
  
- 항목 설명

|항목|설명|
|---|---|
|REPOSITORY|이미지명|
|TAG|이미지 태그명|
|IMAGE ID|이미지 ID|
|CREATED|작성일|
|SIZE|이미지 크기|
|DIGEST|레지스트리에 업로드한 이미지를 고유하게 식별하기 위한 다이제스트 확인|

<br>
<br>

## DCT(Docker Content Trust) 기능

- 공식문서 [LINK](https://docs.docker.com/v17.09/engine/security/trust/content_trust/)

- Docker는 인프라 구성이 포함되기 때문에 악의를 가진 제3자가 위장/변조를 하지 못하도록 이미지를 보호해야 함

  - 이 때 사용하는 기능, 이미지의 정당성을 확인할 수 있음
  
- 서명

  - 이미지 작성자가 Docker 레지스트리에 이미지를 업로드(docker image push)하기 전 로컬 환경에서 이미지 작성자의 비밀키를 이용해 이미지에 서명
  
  - 이 때 이용하는 비밀키를 Offline Key라고 함 → 보안상 매우 중요함! 엄중한 관리 필요

- 검증

  - 서명이 된 이미지를 다운로드(docker image pull)할 때 이미지 작성자의 공개키를 사용하여 이미지 진본여부 검증
  
  - 변조된 경우에는 그 이미지를 무효로 만듦
  
  - 이 때 이용하는 공개키를 Tagging Key라고 함
  
- DTC 기능을 사용하려면, 다음과 같은 설정이 필요

  - 아래 기능을 유효화해놓으면 docker image pull 명령시 이미지 검증이 일어남
  
{% highlight bash %}
export DOCKER_CONTENT_TRUST=1
{% endhighlight %}

- DTC 기능을 사용하여 이미지 다운로드

  - Tagging 부분 확인 가능(이미지 검증 부분)
  
  - 서명이 되어있지 않은 이미지 사용시 오류 발생

{% highlight bash %}
$ sudo docker image pull ubuntu:latest
Pull (1 of 1): ubuntu:latest@sha256:6e9f67fa63b0323e9a1e587fd71c561ba48a034504fb804fd26fd8800039835d
sha256:6e9f67fa63b0323e9a1e587fd71c561ba48a034504fb804fd26fd8800039835d: Pulling from library/ubuntu
7ddbc47eeb70: Pull complete
c1bbdc448b72: Pull complete
8c3b70e39044: Pull complete
45d437916d57: Pull complete
Digest: sha256:6e9f67fa63b0323e9a1e587fd71c561ba48a034504fb804fd26fd8800039835d
Status: Downloaded newer image for ubuntu@sha256:6e9f67fa63b0323e9a1e587fd71c561ba48a034504fb804fd26fd8800039835d
Tagging ubuntu@sha256:6e9f67fa63b0323e9a1e587fd71c561ba48a034504fb804fd26fd8800039835d as ubuntu:latest
docker.io/library/ubuntu:latest
{% endhighlight %}

- DTC 기능 사용 해지

{% highlight bash %}
export DOCKER_CONTENT_TRUST=1
{% endhighlight %}

<br>
<br>

## 이미지 상세 정보 확인

- docker image inspect 명령 사용

  - 단, 현재 보유중인 이미지여야 함

  - 예) centos:7의 이미지 상세 정보 확인

{% highlight bash %}
// 없는 경우
$ sudo docker image inspect centos:7
[]
Error: No such image: centos:7

// 다운로드
$ docker image pull centos:7
7: Pulling from library/centos
Digest: sha256:307835c385f656ec2e2fec602cf093224173c51119bbebd602c53c3653a3d6eb
Status: Downloaded newer image for centos:7
docker.io/library/centos:7

// 있는 경우
$ sudo docker image inspect centos:7
[
    {
        "Id": "sha256:67fa590cfc1c207c30b837528373f819f6262c884b7e69118d060a0c04d70ab8",
        "RepoTags": [
            "centos:7",
            "centos:7"
        ],
        "RepoDigests": [
            "centos@sha256:307835c385f656ec2e2fec602cf093224173c51119bbebd602c53c3653a3d6eb",
            "centos@sha256:307835c385f656ec2e2fec602cf093224173c51119bbebd602c53c3653a3d6eb"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2019-08-20T20:21:01.219060918Z",
        "Container": "c77d57543414f78d333710b96b816033f91a246d606e4dd644e75051719a88c3",
        "ContainerConfig": {
            "Hostname": "c77d57543414",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"/bin/bash\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:4c66d610f9092e18227ae1d0de68350d3da2875452762261ccf1c552462dd90d",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20190801",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "DockerVersion": "18.06.1-ce",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:4c66d610f9092e18227ae1d0de68350d3da2875452762261ccf1c552462dd90d",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20190801",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 201878234,
        "VirtualSize": 201878234,
        "GraphDriver": {
            "Data": {
                "MergedDir": "/var/lib/docker/overlay2/25c2b015d290da33f4c53e6236ff02547394204f69d7a3094593888628fb5ffd/merged",
                "UpperDir": "/var/lib/docker/overlay2/25c2b015d290da33f4c53e6236ff02547394204f69d7a3094593888628fb5ffd/diff",
                "WorkDir": "/var/lib/docker/overlay2/25c2b015d290da33f4c53e6236ff02547394204f69d7a3094593888628fb5ffd/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:877b494a9f30e74e61b441ed84bb74b14e66fb9cc321d83f3a8a19c60d078654"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
{% endhighlight %}

- 명령 실행시 이미지의 상세 정보 표시

  - 주요 정보 : 이미지ID, 작성일, Docker 버전, CPU 아키텍쳐

- 결과는 JSON 형식 표시

  - JSON : JavaScript Object Notation, 텍스트 기반 데이터 포맷
  
- 부분만 확인하기

  - \--format 옵션 이용하여 JSON 형식 데이터의 계층 구조를 지정
  
  - 예1) OS의 값 → 루트 아래(이름 없음)에 있는 "OS" 안에 설정
  
  - 예2) ContainerConfig의 Image값 
  
{% highlight bash %}
// 예1
$ sudo docker image inspect --format="{{.Os}}" centos:7
linux

// 예2
$ sudo docker image inspect --format="{{.ContainerConfig.Image}}" centos:7
sha256:4c66d610f9092e18227ae1d0de68350d3da2875452762261ccf1c552462dd90d
{% endhighlight %}

<br>
<br>

## 이미지 태그설정

- 태그 : 이미지에 표식이 됨

  - 식별하기 쉬운 버전명을 붙이는 것이 일반적
  
- DockerHub에 작성한 이미지를 등록하려면 다음과 같은 규칙으로 이미지에 사용자명을 붙여야 함(붙이는 이유 : 고유성을 위해)

  - \<Docker Hub 사용자명>/이미지명:\[태그명]
  
  - 사용자명이 없는 이미지 → 공식 이미지

- 예) nginx라는 이미지의 이름에 사용자명이 gptjs409이고, 컨테이너명이 webserver이며, 태그에 버전 정보가 1.0인 태그를 붙일 때

  - 보면 TAG는 다르지만, IMAGE ID는 같음 (nginx, gptjs409/webserver)
  
  - 즉, 이미지에 별명을 새롭게 붙인 것, 이미지를 복사하거나 이름을 바꾼 것이 아님!

{% highlight bash %}
$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED                  SIZE
nginx               latest              540a289bab6c        Less than a second ago   126MB
$ sudo docker image tag nginx gptjs409/webserver:1.0
$  sudo docker images
REPOSITORY           TAG                 IMAGE ID            CREATED                  SIZE
gptjs409/webserver   1.0                 540a289bab6c        Less than a second ago   126MB
nginx                latest              540a289bab6c        Less than a second ago   126MB
{% endhighlight %}

<br>
<br>

## 이미지 검색

- Docker Hub에 공개되어있는 이미지를 검색

- docker search \[옵션] \<검색 키워드>

  - 예) nginx 검색

{% highlight bash %}
$ sudo docker search nginx
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
nginx                             Official build of Nginx.                        12133               [OK]
jwilder/nginx-proxy               Automated Nginx reverse proxy for docker con…   1680                                    [OK]
richarvey/nginx-php-fpm           Container running Nginx + PHP-FPM capable of…   744                                     [OK]
linuxserver/nginx                 An Nginx container, brought to you by LinuxS…   80
bitnami/nginx                     Bitnami nginx Docker Image                      72                                      [OK]
tiangolo/nginx-rtmp               Docker image with Nginx using the nginx-rtmp…   58                                      [OK]
nginxdemos/hello                  NGINX webserver that serves a simple page co…   31                                      [OK]
jlesage/nginx-proxy-manager       Docker container for Nginx Proxy Manager        27                                      [OK]
jc21/nginx-proxy-manager          Docker container for managing Nginx proxy ho…   26
nginx/nginx-ingress               NGINX Ingress Controller for Kubernetes         22
privatebin/nginx-fpm-alpine       PrivateBin running on an Nginx, php-fpm & Al…   18                                      [OK]
schmunk42/nginx-redirect          A very simple container to redirect HTTP tra…   17                                      [OK]
blacklabelops/nginx               Dockerized Nginx Reverse Proxy Server.          12                                      [OK]
centos/nginx-18-centos7           Platform for running nginx 1.8 or building n…   12
centos/nginx-112-centos7          Platform for running nginx 1.12 or building …   10
nginxinc/nginx-unprivileged       Unprivileged NGINX Dockerfiles                  9
nginx/nginx-prometheus-exporter   NGINX Prometheus Exporter                       7
1science/nginx                    Nginx Docker images that include Consul Temp…   5                                       [OK]
sophos/nginx-vts-exporter         Simple server that scrapes Nginx vts stats a…   5                                       [OK]
mailu/nginx                       Mailu nginx frontend                            4                                       [OK]
pebbletech/nginx-proxy            nginx-proxy sets up a container running ngin…   2                                       [OK]
travix/nginx                      NGinx reverse proxy                             2                                       [OK]
ansibleplaybookbundle/nginx-apb   An APB to deploy NGINX                          1                                       [OK]
wodby/nginx                       Generic nginx                                   0                                       [OK]
centos/nginx-110-centos7          Platform for running nginx 1.10 or building …   0
{% endhighlight %}

- 실행 결과

|항목|설명|
|---|---|
|NAME|이미지명|
|DESCRIPTION|이미지 설명|
|STARS|즐겨찾기 수|
|OFFICIAL|공식 이미지일 경우 \[OK]|
|AUTOMATED|Dockerfile 기반으로 자동생성 되었을 경우 \[OK]|

<br>

- 지정할 수 있는 주요 옵션

|옵션|설명|
|---|---|
|--no-trunc|결과를 모두 표시|
|--limit|n건의 검색 결과를 표시|
|--filter=starts=n|즐겨찾기의 수가 n이상인 것만 표시|

<br>

- Docker Hub에 공개되어있는 이미지가 모두 안전한 것은 아님

  - 안전을 위해 되도록 공식이미지거나 Dockerfile이 제대로 공개되어 있는 것을 선택하여 확인할 것

<br>
<br>

## 이미지 삭제

- 작성한 이미지를 삭제

- docker image rm \[옵션] 이미지명...

  - 또는 docker rmi \[옵션] 이미지명...

  - 옵션이 없을 경우, 중간 이미지도 함께 삭제됨

- 이미지명 : REPOSITORY 또는 IMAGE ID를 지정

  - 여러 이미지 제거의 경우, 이미지명을 스페이스로 구분하여 지정

  - IMAGE ID는 고유하게 지정만 할 수 있으면 되므로, 모든 자리를 지정하지 않고 처음 3자리 정도만 지정해도 됨

- 지정할 수 있는 주요 옵션

|옵션|설명|
|---|---|
|--force, -f|강제 삭제|
|--no-prune|중간 이미지 삭제하지 않음|

<br>

{% highlight bash %}
$ sudo docker images
REPOSITORY           TAG                 IMAGE ID            CREATED                  SIZE
gptjs409/webserver   1.0                 540a289bab6c        Less than a second ago   126MB
nginx                latest              540a289bab6c        Less than a second ago   126MB
centos               latest              0f3e07c0138f        2 weeks ago              220MB
centos               7                   67fa590cfc1c        2 months ago             202MB
$ sudo docker rmi centos  # 태그가 별도로 없으면 latest만 제거
Untagged: centos:latest
Untagged: centos@sha256:f94c1d992c193b3dc09e297ffd54d8a4f1dc946c37cbeceb26d35ce1647f88d9
Deleted: sha256:0f3e07c0138fbe05abcb7a9cc7d63d9bd4c980c3f61fea5efa32e7c4217ef4da
Deleted: sha256:9e607bb861a7d58bece26dd2c02874beedd6a097c1b6eca5255d5eb0d2236983
$ sudo docker images
REPOSITORY           TAG                 IMAGE ID            CREATED                  SIZE
gptjs409/webserver   1.0                 540a289bab6c        Less than a second ago   126MB
nginx                latest              540a289bab6c        Less than a second ago   126MB
centos               7                   67fa590cfc1c        2 months ago             202MB
$ sudo docker rmi centos:7
Untagged: centos:7
$ sudo docker images
REPOSITORY           TAG                 IMAGE ID            CREATED                  SIZE
gptjs409/webserver   1.0                 540a289bab6c        Less than a second ago   126MB
nginx                latest              540a289bab6c        Less than a second ago   126MB
{% endhighlight %}

<br>

#### 사용하지 않는 Docker 이미지 삭제

- 사용하지 않는 Docker 이미지는 디스크 용량만 쓸데없이 차지하기 때문에 정기적으로 삭제 권장

- docker image prune \[옵션]

- 주요 옵션

|옵션|설명|
|---|---|
|--all, -a|사용하지 않은 이미지를 모두 삭제|
|--force, -f|이미지를 강제로 삭제|

<br>
<br>

## DockerHub에 로그인

- 선작업 : DockerHub 가입 [LINK](https://hub.docker.com/)

- Docker 레파지토리에 업로드를 하려면 docker login 명령을 사용하여 로그인

  - docker login \[옵션] \[서버]
  
- 주요 옵션

|옵션|설명|
|---|---|
|--password, -p|비밀번호|
|--username, -u|사용자명|

- 옵션을 지정하지 않으면 사용자명과 비밀번호를 물어봄 → DockerHub에 등록한 계정을 지정

- 로그인 성공 : Login Successed

- 서버명을 지정하지 않을 경우 DockerHub에 액세스됨

  - 다른 환경에 Docker Repository가 있는 경우에만 서버명 지정
  
{% highlight bash %}
$  docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: gptjs409
Password:
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
{% endhighlight %}

<br>
<br>

## 이미지 업로드

- DockerHub에 이미지 업로드하려면 docker image push 명령 사용

  - docker image push 이미지명\[:태그명]
  
- 업로드할 이미지는 다음과 같은 형식으로 지정

  - \<Docker Hub 사용자명>/이미지명:\[태그명]
  
- 업로드 전 DockerHub에 계정 생성 후 docker login으로 로그인해 둘 것

- 예) gptjs409라는 계정으로 webserver 이미지명의 태그가 1.0인 이미지 업로드

{% highlight bash %}
$ sudo docker images
REPOSITORY           TAG                 IMAGE ID            CREATED                  SIZE
nginx                latest              540a289bab6c        Less than a second ago   126MB
gptjs409/webserver   1.0                 540a289bab6c        Less than a second ago   126MB

$ sudo docker push gptjs409/webserver:1.0
The push refers to repository [docker.io/gptjs409/webserver]
a89b8f05da3a: Mounted from library/nginx
6eaad811af02: Mounted from library/nginx
b67d19e65ef6: Mounted from library/nginx
1.0: digest: sha256:f56b43e9913cef097f246d65119df4eda1d61670f7f2ab720831a01f66f6ff9c size: 948
{% endhighlight %}
  
- DockerHub에서 업로드 확인 가능(docker search로도 가능)

  ![image](/img/2019-11-01/docker-image-command-001-web1.png)

<br>
<br>

## DockerHub에서 로그아웃

- Docker Hub에서 로그아웃

- docker logout \[서버명]

  - 서버명 미지정시 Docker Hub에 액세스
  
  - 다른 환경에 Docker 레파지토리가 있는 경우는 서버명을 지정
  
{% highlight bash %}
$ docker logout
Removing login credentials for https://index.docker.io/v1/
{% endhighlight %}
