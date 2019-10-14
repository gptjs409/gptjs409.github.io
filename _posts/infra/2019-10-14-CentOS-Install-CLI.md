---
layout: post
title:  "CentOS 텍스트모드(CLI)로 설치하기"
date:   2019-10-14 13:21:33
author: Choi HyeSun
categories: Infra
tags:
  - CentOS
  - CentOS설치
  - VirtualBox에CentOS설치
  - VirtualBox
  - CentOS7
  - CentOS7설치
  - CentOS ISO
  - CentOS 텍스트모드 설치
  - CentOS CLI 설치
---

## 1. CentOS 7 ISO 파일 다운로드
CentOS 7버전 다운로드(현재 최신버전은 8)
  - CentOS 종류별 다운로드 [Link](https://wiki.centos.org/Download) → 7버전 선택
  ![image](/img/2019-10-14/CentOS-Install-CLI-001-downloads1.png)
  
  - CentOS 7 임의의 미러페이지 선택
  ![image](/img/2019-10-14/CentOS-Install-CLI-002-downloads2.png)
  
  - CentOS-7-x86_64-Minimal-1908.iso 다운로드
  ![image](/img/2019-10-14/CentOS-Install-CLI-003-downloads3.png)
  
  - Down받은 iso파일 경로 `D:\Program Files\Oracle\OS ISO`로 이동(하지 않아도 무관, 경로지정 편의상 진행)
  ![image](/img/2019-10-14/CentOS-Install-CLI-004-downloads4.png)
  
  > iso 파일이란?
  >
  > CD나 DVD의 이미지파일로 CD나 DVD의 내용 뿐만 아니라 구조까지 그대로 복사해놓은 파일이라 생각하면 됨
  > 즉, CentOS 7 설치 디스크(CD)를 다운받는 것과 같음
  
<br>
<br>

## 2. CentOS 7를 설치할 빈 서버 생성
  - 새로만들기 → 이름지정(CentOS7-GUI) → 종류/버전확인(Linux, Red Hat (64-bit)) → 다음
  ![image](/img/2019-10-14/CentOS-Install-CLI-005-Install1.png)
  
  - 메모리 크기 지정 4096MB (추천 1024MB, 디스크 용량 및 설치 파일 용량 고려하여 설정할 것)
  ![image](/img/2019-10-14/CentOS-Install-CLI-006-Install2.png)
  
  - 하드 디스크 : 지금 새 가상 하드디스크 만들기
  ![image](/img/2019-10-14/CentOS-Install-CLI-007-Install3.png)
  
  - 하드 디스크 파일 종류 : VDI(VirtualBox 디스크 이미지)
  ![image](/img/2019-10-14/CentOS-Install-CLI-008-Install4.png)
  
  - 물리적 하드 드라이브에 저장 : 동적 할당
  ![image](/img/2019-10-14/CentOS-Install-CLI-009-Install5.png)
  
  - 파일 위치 및 크기 : Default 경로, 8.00G
  ![image](/img/2019-10-14/CentOS-Install-CLI-010-Install6.png)
  
  - 빈 서버가 생성됨
  ![image](/img/2019-10-14/CentOS-Install-CLI-011-Install7.png)
  
<br>
<br>

## 3. CentOS 7 ISO를 빈 서버에 설정하기
  - 서버 선택 → 설정 → 저장소
  ![image](/img/2019-10-14/CentOS-Install-CLI-012-CInstall1.png)
  
  - 비어 있음(컨트롤러:IDE) 선택 → 속성의 CD모양 클릭 → 가상 광디스크 파일 선택
  ![image](/img/2019-10-14/CentOS-Install-CLI-013-CInstall2.png)
  
  - 가상 광 디스크 파일 선택 : 아까 저장했던 ISO 파일 선택 후 열기
  ![image](/img/2019-10-14/CentOS-Install-CLI-014-CInstall3.png)
  
  - ISO 파일 적용 확인 후 확인
  ![image](/img/2019-10-14/CentOS-Install-CLI-015-CInstall4.png)
  
  - 서버 정보에서 ISO 파일 적용되어있는지 확인
  ![image](/img/2019-10-14/CentOS-Install-CLI-016-CInstall5.png)

<br>
<br>

## 4. CentOS7 CLI 설치
  - 서버 시작 (1) 서버 선택 후 상단 시작 (2) 서버 우클릭 후 시작 → 일반시작
  ![image](/img/2019-10-14/CentOS-Install-CLI-017-CentOS1.png)
  ![image](/img/2019-10-14/CentOS-Install-CLI-018-CentOS2.png)
  
  - 서버 모드 변경(너무 조그맣기 때문) → HOSTKEY(Ctrl + Alt) + C (창모드:크기조정X,메뉴O ↔ 스케일모드:크기조정O,메뉴X)
  ![image](/img/2019-10-14/CentOS-Install-CLI-019-CentOS3.png)
  ![image](/img/2019-10-14/CentOS-Install-CLI-020-CentOS4.png)
  
  - 서버 클릭하여 Test this media & install CentOS 7에 흰 불이 들어와있으면 엔터, 방향키로 선택가능
  ![image](/img/2019-10-14/CentOS-Install-CLI-021-CentOS5.png)
  
  - 마구마구 뭔가 확인되는중...
  ![image](/img/2019-10-14/CentOS-Install-CLI-022-CentOS6.png)
  
  - 언어 선택(한국어) 마우스 가능한데, 오류인지 설치시 창모드에서만 정상적으로 마우스가 인식됨(불편)
  ![image](/img/2019-10-14/CentOS-Install-CLI-023-CentOS7.png)

  - 설치 대상 선택(필수, 진행하지 않으면 다음으로 안넘어감) → 별도로 설정할 것은 없고, 바로 완료
  ![image](/img/2019-10-14/CentOS-Install-CLI-024-CentOS8.png)
  ![image](/img/2019-10-14/CentOS-Install-CLI-025-CentOS9.png)
  
  - 설치시작 활성화, 설치시작 클릭
  ![image](/img/2019-10-14/CentOS-Install-CLI-026-CentOS10.png)
  
  - 뭔가 아래설치되면서, ROOT 암호와 사용자 생성을 하라고 나와있음
  ![image](/img/2019-10-14/CentOS-Install-CLI-027-CentOS11.png)
  
  - ROOT 암호 설정, 너무 짧거나 하면 완료 못하게 함. 적당히 길고 특수문자 넣어서
  ![image](/img/2019-10-14/CentOS-Install-CLI-028-CentOS12.png)
  ![image](/img/2019-10-14/CentOS-Install-CLI-029-CentOS13.png)
  
  - 사용자 생성, 꼭 안해도 되는 것 같지만 'sun'이라는 사용자를 추가해주었음
  ![image](/img/2019-10-14/CentOS-Install-CLI-030-CentOS14.png)
  ![image](/img/2019-10-14/CentOS-Install-CLI-031-CentOS15.png)

  - 설정 확인하고 설정완료
  ![image](/img/2019-10-14/CentOS-Install-CLI-032-CentOS16.png)
  
  - 막 또 뭔가 설치 / 생성됨
  ![image](/img/2019-10-14/CentOS-Install-CLI-033-CentOS17.png)
  
  - 완료 → 재부팅
  ![image](/img/2019-10-14/CentOS-Install-CLI-034-CentOS18.png)
  
  - 처음에 뭐가 뜨면 엔터
  ![image](/img/2019-10-14/CentOS-Install-CLI-035-CentOS19.png)
  
  - 완료 : 계정과 비밀번호 입력해주면 됨 (root or sun)
  ![image](/img/2019-10-14/CentOS-Install-CLI-036-CentOS20.png)
  
<br>
<br>

## 5. Putty로 접근할 수 있게 하기
Host 네트워크 추가하기
  - 파일 → 호스트 네트워크 관리자
  ![image](/img/2019-10-14/CentOS-Install-CLI-037-putty1.png)
  
  - 만들기
  ![image](/img/2019-10-14/CentOS-Install-CLI-038-putty2.png)
  
  - 생성됨
  ![image](/img/2019-10-14/CentOS-Install-CLI-039-putty3.png)
  
  - DHCP 서버 사용함 → IP 대역 기억하기(192.168.134.1/24) → 닫기
  ![image](/img/2019-10-14/CentOS-Install-CLI-040-putty4.png)
  
<br>

서버 네트워크 설정하기



<br>

서버에서 네트워크 설정
  
  
<br>
<br>

## 결과
아니 GUI를 설치해보고... CLI를 설치해서 비교하려했는데

CLI가 설치되었음(?)

<br>

오랫만에 했더니 기억이 가물가물...

6버전일 땐 text라고 모드 입력해야 되었었던 것 같은데...

미네랄 ISO때문인가...(!!) 아항 :)

<br>

GUI는 PASS.... :P
  
