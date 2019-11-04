---
layout: post
title:  "[Docker] 도커 컨테이너 네트워크"
date:   2019-11-03 19:30:21
author: Choi HyeSun
categories: infra
tags:
  - Docker
  - 도커
  - 도커파일 만들기
---

## 명령

- FROM - **이미지**

  - 베이스 이미지 지정
  
  - FROM \[이미지명] → 베이스 이미지의 최신버전(latest)이 적용
  
  - FROM \[이미지명]:\[태그명]
  
  - FROM \[이미지명]:@\[다이제스트(DockerHub 업로드식별자)]

- MAINTAINER

  - 구(1.13 이하) 버전의 도커에서 작성자를 기술
  
  - 현재 사용하지 않고, LABEL로 기술할 것
  
  - LABEL maintainer "Sun\<gptjs409@gmail.com>"

- LABEL - **이미지**

  - 이미지에 버전 정보, 작성자 정보, 코멘트 등과 같은 부분 라벨링
  
  - LABEL \<key>=\<value>

- ONBUILD - **이미지**

  - 설정된 이미지를 다른 Dockerfile에서 베이스 이미지로 설정하고 빌드했을 때 실행시키는 명령
  
  - ONBUILD \[명령]
  
- ARG - **이미지**

  - Dockerfile 내 변수
  
  - 변수에 값에 따라 생성되는 이미지의 내용을 바꿀 수 있음 (이미지 빌드시 --build-arg 이름=값 으로 값 변경 가능)
  
  - ARG \<이름>\[=기본값]
  
- ENV - **이미지 + 컨테이너**

  - 환경 변수
  
  - ENV \[key] \[value] → 1번에 1개
  
  - ENV \[key]=\[value] → 1번에 여러개 가능(\\ 이용)

- RUN - **이미지**

  - 이미지 생성시 명령 실행
  
  - RUN \[명령]
  
  - 한 줄에 쓸 수 있을 경우 나누지 않고 한 줄에 쓰는 것이 좋음

- CMD - **컨테이너**

  - 컨테이너 실행시 명령 실행(데몬 실행)
  
  - CMD \[명령]
  
- ENTRYPOINT - **컨테이너**

  - 컨테이너 실행 명령 실행(데몬 실행)
  
  - CMD와 비슷하지만, CMD는 인수 덮어쓰기가 가능하고 ENTRYPOINT는 불가능 함 (그래서 변경이 필요하면 CMD와 결합해서 씀)
  
  - ENTRYPOINT \[명령]

- SHELL - **이미지 + 컨테이너**

  - RUN/CMD/ENTRYPOINT 명령시 기본 쉘 설정 (default - LINUX "/bin/sh" "-c")
  
  - SHELL \["쉘 경로", "파라미터"]

- WORKDIR - **이미지 + 컨테이너**

  - 작업 디렉터리(RUN/CMD/ENTRYPOINT/COPY/ADD 명령)
  
  - 여러 번 수정 가능하고, 절대경로/상대경로 모두 가능

- USER - **이미지 + 컨테이너**

  - 사용자(RUN/CMD/ENTRYPOINT)
  
  - USER \[사용자명 \| UID]

- EXPOSE - **이미지**

  - 공개 포트 지정
  
  - Docker에게 실행중인 컨테이너가 listen하고 있는 네트워크를 알려줌
  
  - run -p 옵션시 어떤 포트를 호스트에게 보여줄지 정의
  
  - EXPOSE \<포트번호>


- ADD

  - 파일이나 디렉토리 추가
  
  - ADD \<호스트파일경로> \<Docker이미지파일경로>
  
  - ADD \["\<호스트파일경로> \<Docker이미지파일경로>"]
  
  - 경로에는 와일드카드와 Go언어의 filepath.Match룰과 일치하는 패턴 사용 가능
  
  - 경로는 절대경로 or WORKDIR 기반 상대경로
  
  - 원격URL로부터 다운로드 받는다면 퍼미션은 600
  <br>단, 인증을 지원하지 않음, 인증이 필요할 경우 wget이나 curl을 RUN으로 사용할 것
  
  - 호스트 파일이 tar 아카이브거나 압축일 때는, 디렉토리로 압축 해제
  <br>단, 원격 URL로부터 다운로드한 리소스는 제외

- COPY

  - 파일 복사

  - ADD와 비슷하지만(명령 구문도 동일), 호스트상의 파일을 이미지 안으로 '복사'하는 처리만 함
  
  - 단순히 이미지 안에 파일 배치하고 싶을 때 사용
  
  - COPY \<호스트파일경로> \<Docker이미지파일경로>
  
  - COPY \["\<호스트파일경로> \<Docker이미지파일경로>"]

- VOLUME

  - 이미지에 볼륨을 할당

  - 영구데이터를 저장하기 위해서는 컨테이너 내부가 아닌 외부에 저장하는 것이 맞음

  - VOLUME \["마운트포인트"] → JSON 배열
  
  - VOLUME 마운트포인트 → 문자열

- HEALTHCHECK

  - 컨테이너 내 프로세스가 정상적으로 작동하고 있는 지를 체크
  
  - HEALTHCHECK \[옵션] CMD 명령
  
  - \--inteval=n : 헬스 체크 간격(기본 30s)
  
  - \--timeout=n : 헬스 체크 타임아웃(기본 30s)
  
  - \--retries=N : 타임아웃 횟수(기본 3)

- STOPSIGNALL

  - 시스템 종료 콜 시그널 설정
  
  - STOPSIGNAL \[시그널번호 \| 시그널명]
