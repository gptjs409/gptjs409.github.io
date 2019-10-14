---
layout: post
title:  "CentOS GUI로 설치하기(default)"
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
---

## 1. CentOS 7 ISO 파일 다운로드
CentOS 7버전 다운로드(현재 최신버전은 8)
  - CentOS 종류별 다운로드 [Link](https://wiki.centos.org/Download) → 7버전 선택
  ![image](/img/2019-10-14/CentOS-Install-GUI-001-downloads1.png)
  
  - CentOS 7 임의의 미러페이지 선택
  ![image](/img/2019-10-14/CentOS-Install-GUI-002-downloads2.png)
  
  - CentOS-7-x86_64-Minimal-1908.iso 다운로드
  ![image](/img/2019-10-14/CentOS-Install-GUI-003-downloads3.png)
  
  - Down받은 iso파일 경로 `D:\Program Files\Oracle\OS ISO`로 이동(하지 않아도 무관, 경로지정 편의상 진행)
  ![image](/img/2019-10-14/CentOS-Install-GUI-004-downloads4.png)
  
  > iso 파일이란?
  >
  > CD나 DVD의 이미지파일로 CD나 DVD의 내용 뿐만 아니라 구조까지 그대로 복사해놓은 파일이라 생각하면 됨
  > 즉, CentOS 7 설치 디스크(CD)를 다운받는 것과 같음
  
<br>
<br>

## 2. CentOS 7를 설치할 빈 서버 생성
  - 새로만들기 → 이름지정(CentOS7-GUI) → 종류/버전확인(Linux, Red Hat (64-bit)) → 다음
  ![image](/img/2019-10-14/CentOS-Install-GUI-005-Install1.png)
  
  - 메모리 크기 지정 4096MB (추천 1024MB, 디스크 용량 및 설치 파일 용량 고려하여 설정할 것)
  ![image](/img/2019-10-14/CentOS-Install-GUI-006-Install2.png)
  
  - 하드 디스크 : 지금 새 가상 하드디스크 만들기
  ![image](/img/2019-10-14/CentOS-Install-GUI-007-Install3.png)
  
  - 하드 디스크 파일 종류 : VDI(VirtualBox 디스크 이미지)
  ![image](/img/2019-10-14/CentOS-Install-GUI-008-Install4.png)
  
  - 물리적 하드 드라이브에 저장 : 동적 할당
  ![image](/img/2019-10-14/CentOS-Install-GUI-009-Install5.png)
  
  - 파일 위치 및 크기 : Default 경로, 8.00G
  ![image](/img/2019-10-14/CentOS-Install-GUI-010-Install6.png)
  
  - 빈 서버가 생성됨
  ![image](/img/2019-10-14/CentOS-Install-GUI-011-Install7.png)
  
<br>
<br>

## 3. CentOS 7 ISO를 빈 서버에 설정하기
  - 서버 선택 → 설정 → 저장소
  ![image](/img/2019-10-14/CentOS-Install-GUI-012-CInstall1.png)
  
  - 비어 있음(컨트롤러:IDE) 선택 → 속성의 CD모양 클릭 → 가상 광디스크 파일 선택
  ![image](/img/2019-10-14/CentOS-Install-GUI-013-CInstall2.png)
  
  - 가상 광 디스크 파일 선택 : 아까 저장했던 ISO 파일 선택 후 열기
  ![image](/img/2019-10-14/CentOS-Install-GUI-014-CInstall3.png)
  
  - ISO 파일 적용 확인 후 확인
  ![image](/img/2019-10-14/CentOS-Install-GUI-015-CInstall4.png)
  
  - 서버 정보에서 ISO 파일 적용되어있는지 확인
  ![image](/img/2019-10-14/CentOS-Install-GUI-016-CInstall5.png)

<br>
<br>

## 4. CentOS7 GUI 설치
  
  
