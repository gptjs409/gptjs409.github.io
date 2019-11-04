---
layout: post
title:  "[Docker] 도커 컨테이너 네트워크"
date:   2019-11-03 19:30:21
author: Choi HyeSun
categories: infra
tags:
  - Docker
  - 도커
  - Docker Container
  - 도커 컨테이너
  - Container
  - 컨테이너
  - Docker Container Network
  - 도커 컨테이너 네트워크
---

## 도커 컨테이너 네트워크

- Docker 컨테이너끼리 통신할 때는 Docker 네트워크를 통해 수행

- Docker는 기본 네트워크 값으로 bridge, hosts, none 세 개의 네트워크를 만듦

<br>
<br>

## Docker 네트워크 목록 표시

- Docker network ls \[옵션]

- 주요 옵션

  - \-f, --filter=\[] : 출력을 필터링
  
  - \--no-trunc : 상세 정보를 출력
  
  - \-q, --quiet : 네트워크 ID만 표시
  
<br>

#### 네트워크 목록 표시 (예제)

{% highlight bash %}
$ sudo docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
914f387eab6b        bridge              bridge              local
c8fa1e69139e        host                host                local
478c12382ce4        none                null                local
{% endhighlight %}

- 따로 추가한게 없으므로 기본 네트워크 세 개가 추가됨

- 표시할 네트워크의 상세 정보 확인 : --no-trunc 옵션

- 네트워크 ID만 확인할 경우 : -q 또는 --quiet 옵션

- 필터링을 하고 싶을 때 : -f 또는 --filter 옵션

   - 필터링시 key=value 형태로 지정

|필터링값|설명|
|---|---|
|driver|드라이버 지정|
|id|네트워크 ID|
|label|네트워크에 설정된 라벨(label=\<Key> 또는 \<Key>=\<value>로 지정
|name|네트워크명|
|scope|네트워크의 스코프(swarm/global/local|
|type|네트워크의 타입(사용자 정의 및 네트워크 custom/정의 완료 네트워크 builtin)


<br>

#### 네트워크 목록 표시의 필터링 (예제)

{% highlight bash %}

{% endhighlight %}

<br>

#### 컨테이너 시작 (예제)

{% highlight bash %}

{% endhighlight %}

<br>

#### 컨테이너 네트워크 확인 (예제)

{% highlight bash %}

{% endhighlight %}

<br>
<br>

## 네트워크 작성

<br>

#### 브리지 네트워크 작성 (예제)

{% highlight bash %}

{% endhighlight %}

<br>

#### 작성한 네트워크 확인 (예제)

{% highlight bash %}

{% endhighlight %}

<br>
<br>

## 네트워크 연결

<br>

#### 네트워크에 대한 연결 (예제)

{% highlight bash %}

{% endhighlight %}

<br>

#### 컨테이너 네트워크 확인 (예제)

{% highlight bash %}

{% endhighlight %}

<br>

#### 네트워크를 지정한 컨테이너 시작 (예제)

{% highlight bash %}

{% endhighlight %}

<br>

#### 네트워크에 대한 연결 해제 (예제)

{% highlight bash %}

{% endhighlight %}

<br>
<br>

## 네트워크 상세 정보 확인 (예제)

{% highlight bash %}

{% endhighlight %}

<br>

#### 네트워크 상세 정보 표시 (예제)

{% highlight bash %}

{% endhighlight %}

<br>
<br>

## 네트워크 삭제

<br>

#### 네트워크 삭제 (예제)

{% highlight bash %}

{% endhighlight %}
