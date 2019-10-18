---
layout: post
title:  "[Web Server] Apache VS Nginx"
date:   2019-08-04 19:42:17
author: Choi HyeSun
categories: infra
tags:
  - webserver
  - web
  - server
  - nginx
  - apache
  - nginx vs apache
  - 엔진엑스
  - 아파치
---

## 출처

kinsta_blog [LINK](https://kinsta.com/blog/nginx-vs-apache/)

netcraft_news [LINK](https://news.netcraft.com/archives/category/web-server-survey/)

<br>
<br>

## Web Server

- 웹 페이지를 제공하기 위한 서버 프로그램

  - 웹 페이지 : 본질적으로는 HTML 문서

- Apache나 Nginx 같은 소프트웨어는 요청을 처리하고 분석한 후 Client의 브라우저에서 볼 수 있도록 해당 문서(웹 페이지)를 반환

<br>
<br>

## Apache vs Nginx

||Apache|Nginx|
|---|---|---|
|시작|1995년|2004년|
|성능||경우에 따라 Apache보다 경쟁력이 있음|
|점유율|많음|많음, (2019.04 아파치 추월)|
|특징|동적 모듈 시스템<br>구성 시스템 사용|모듈 시스템<br>구성 시스템이 없으므로 빠름|
|계획||2019.05 ~ Quic 및 Http/3 지원 관련 개발 시작 발표|
|캐싱|Varnish 캐싱|FastCGI 캐싱<br>- 최근 일부 테스트에서 Varnish 캐싱보다 명확|
|요청 처리|mpm 이벤트<br>- 아파치의 많은 성능 문제를 완화|작업자 프로세스 각 작업자가 수십만개의 네트워크 연결을 처리<br>(각 연결에 대해 새 스레드 또는 프로세스 작성 불필요)<br>- 일부 테스트에서 이벤트 mpm이 최적화 측면에서는 더 나아갔지만 성능을 넘어가지 못함<br>- 정적 파일에 대해서는 Nginx는 Apache의 요청의 2배를 제공|

- 2019년 4월 Nginx가 Apache 사용량을 추월

![image](/img/2019-08-04/web-server-001-graph1.png)

![image](/img/2019-08-04/web-server-002-graph2.png)

<br>
<br>

## 결론

- 요청량이 많지 않다면 apache나 nginx나 편한 것을 사용

- 요청량이 많다면 nginx가 더 성능이 좋은 듯

- 점유율(현재) : nginx > apache
