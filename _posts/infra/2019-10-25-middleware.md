---
layout: post
title:  "[Server] 미들웨어, 인프라구성관리 툴"
date:   2019-10-25 19:52:11
author: Choi HyeSun
categories: infra
tags:
  - 미들웨어
  - Middleware
  - 인프라구성관리툴
  - Kickstart
  - Vagrant
---

## 미들웨어(Middleware)

- OS와 업무 처리를 수행하는 애플리케이션 사이에 들어가는 소프트웨어

- 목적과 용도에 따라 폭넓은 종류가 있음

  - OS가 갖고 있는 기능을 확장한 것
  
  - 애플리케이션에서 상요하는 공통 기능을 제공하는 것
  
  - 각종 서버 기능을 제공하는 것 등

  - 오픈소스인 것
  
  - 상용으로 제공하는 것
  
<br>

#### 미들웨어1 - 웹 서버

- 클라이언트의 브라우저가 보내온 HTTP 요청을 받아, 웹 콘텐츠(HTML, CSS 등)를 응답으로 반환하거나 다른 서버 사이드 프로그램을 호출하는 기능

- 종류 : Apache HTTP Server, Internet Information Services, Nginx 등

<br>

#### 미들웨어2 - DB서버

- 시스템이 생성하는 다양한 데이터를 관리

- DBMS라 부르기도 함

- 종류 : MySQL, PostgreSQL, OracleDataBase 등

- NoSQL

  - RDBMS와는 다른 새로운 방식을 통틀어 일컫는 말
  
  - 벙렬분산처리나 유연한 스키마 설정 등이 특징
  
  - 주요방식 : KVS(Key-Value Store), 도큐먼트 지향 데이터베이스(Document DB), XML DB 등
  
  - 병렬처리에 뛰어나기 떄문에 다수의 사용자 엑세스를 처리할 필요가 있는 온라인 시스템에 널리 이용
  
  - 종류 : Redis, MongoDB, Apache Cassandra 등

<br>

#### 미들웨어3 - 시스템 모니터링 서버

- 시스템을 안정적으로 운영하기 위해 시스템이 어떤 상태로 가동되고 있는지를 감시하기 위함

- 종류 : Zabbix, Datadog, Mackerel 등

<br>

## 인프라 구성관리 툴

- 인프라 구성 관리를 자동으로 관리하는 툴

<br>

#### OS의 시작을 자동화하는 툴

- 서버 OS를 설치하거나 가상화 툴을 설치 및 설정하는 작업을 자동화

- 예

  - KickStart : RedHat 계열 Linux 배포판에서 이용할 수 있음
  
  - Vagrant : PC에 가상 환경을 만들 수 있음
  
<br>

#### OS나 미들웨어의 설정을 자동화하는 툴

- DB 서버, 웹 서버, 감시 Agent 등과 같은 미들웨어의 설치/버전관리, Unix 계열 OS의 /etc 아래의 OS/미들웨어설정파일/방화벽기능설정 등 보안관련 설정을 자동화하기 위한 툴

- 주요 툴

  - [Chef](https://www.chef.io/products/chef-infra/) : Chef사가 제공하는 오픈소스 Ruby에 의한 인프라 구성 관리 틀
  
  - [Ansible](https://www.ansible.com/) : Python으로 구축
  
  - [Puppet](https://puppet.com/) : 2005년에 릴리즈된 오픈소스 구성관리 툴
  
  - [Itamae](http://itamae.kitchen/) : Ruby DSL로 기술할 수 있는 심플한 구성 관리 툴
  
<br>

#### 여러 서버의 관리를 자동화하는 툴

- 분산 환경의 서버들을 리하기 위한 툴

- 컨테이너 오케스트레이션의 사실상 표준 [Kubernetes](https://kubernetes.io/)

  - Docker의 여러 
