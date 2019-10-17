---
layout: post
title:  "[Git] 자주쓰는 명령어 정리"
date:   2019-09-07 21:14:53
author: Choi HyeSun
categories: git
tags:
  - Git
  - Git Command
  - 깃 명령어
  - 깃
  - commit
  - add
  - push
  - pull
---
## 기존 디렉토리를 Git 저장소로 만들기 (git 시작)

- git init

<br>
<br>

## 기존 git 저장소를 사용하기위해 복사해오기

- git clone [git_path]

<br>
<br>

## git 브랜치 관리

- git branch : 브랜치 목록 확인

  - git branch [브랜치명] : 브랜치명으로 브랜치 신규 생성

- git checkout [브랜치명] : 해당 브랜치로 이동

  - git checkout -b [브랜치명] : 브랜치가 없으면 브랜치를 생성한 후 이동
  
<br>
<br>

## 현재 파일들 상태 확인

- git status : 파일이 변경됨을 확인(어떤 파일이 staged 상태인지 아닌지)

- git diff : staged 상태가 아닌 파일이 어떤 내용으로 변경되었는지 확인

  - git diff --staged : staged 상태인 파일을 비교
  
<br>
<br>

## Stage(local) area에 git file 변경 저장하기

- git add : 변경된 파일을 반영

  - git add 후 파일 추가 수정시에도 git status확인 후 git add를 할 것

- git rm [파일명] : git add한 파일 제거 (rm 후 git add한 효과)

  - git rm -f [파일명] : 파일이 캐시되어있을 때 강제 제거할 수 있는 옵션

- git mv [원본 이름] [변경할 이름] : git으로 관리된 파일의 이름 변경(history 관리됨)

- Staged → Unstaged (add 취소)

  - git reset HEAD [File명] : add 취소

  - git checkout -- [File명] : 수정한 사항 처음으로 되돌리기

<br>
<br>

## git commit - git add에 추가한 file (staged file) commit하기

- git commit -m "commit message" : commit massage 명시하여 commit

- git commit -a : git add + git commit 효과 (git add 생략)

- git commit --ammend : 직전 커밋에 덮어쓰기(파일 누락, commit message 변경 등)
  
<br>
<br>

## git log - git commit 히스토리 조회

- git log -[number] : 최근 number개의 결과만 보여줌

- git log -p : 각 커밋의 diff 결과를 보여줌

- git log --word-diff : -p와 비슷하지만 줄 단위 대신 단어 단위로 보여줌
<br>-U1 옵션 : 단어비교일 때 -p와 같이 3줄을 보여주는 것이 아닌 현재 줄만 확인할 수 있도록

- git log --stat : 각 커밋의 통계 정보를 조회

- git log --shortstat : --stat 명령 결과 중 수정 파일, 추가/삭제한 줄만 보여줌

- git log name-only : 커밋 정보 중 수정된 파일의 목록만 보여줌

- git log name-status : 수정된 파일 목록 + 파일 추가/수정/삭제 구분

- git log --pretty=[option] : option별로 출력 형식 설정 가능
 
  - option 종류 : 아래에 별도로 작성

- git log --graph : 브랜치와 머디 히스토리를 보여주는 아스키 그래프 출력

- git log --since=[date] : 시간을 기준으로 이후 조회, 다양한 형식 지원 (=after)
  <br>예) 2018-01-15, 2.weeks, 2years 6day 1 minutes ago

- git log --util=[date] : 시간을 기준으로 이전 조회, 다양한 형식 지원(=before)

- git log --author : 지정한 저자의 커밋만 보기

- git log --commitor : 지정한 커미터의 커밋만 보기

<br>
<br>

## git log --pretty=[option] 종류

- option 종류

  - online : 각 커밋을 한 줄로 보여줌

  - short, full, fuller : 각 커밋 정보를 조금씩 가감샣서 보여줌

  - format:"[format들..]" : 나만의 포맷으로 포맷팅하여 보여줌
    <br>예) %h - %an; %ar : %s
  
- format 종류 중 유용한 것들

  - %H : Commit hash

  - %h : Abbreviated commit hash

  - %T : Tree hash

  - %t : Abbreviated tree hash

  - %P : Parent hash

  - %p : Abbreviate parent hash

  - %an : Author name

  - %ae : Author e-mail

  - %ad : Author date (format respects the --date= option)

  - %ar : Autor date, relative

  - %cn : Committer name

  - %ce : Committer email

  - %cd : Committer date (format respects the --date= option)

  - %cr : Committer date, relative

  - %s : Subect
  
<br>
<br>

## git으로 관리하지 않을 파일 설정

- gitignore 파일 생성 후 무시할 파일 패턴 적기

- 빈 줄, #로 시작하는 주석줄 무시

- 표준 Glob 패턴 사용

  - * : 문자 0개 ~ 그 이상

  - [a-z] [0-9] : [ ] 사이에 있는 문자 중 하나를 의미

  - ? : 문자 1개

- 디렉토리는 /를 끝에 사용하는 것으로 표현

- !로 시작하는 패턴의 파일은 무시하지 않음

<br>
<br>

## git remote 저장소

- git remote : remote 저장소 확인

  - git remote -v : remote 저장소 단축이름 + URL로 확인

  - git remote show [remote-name(단축이름)] : remote 저장소 구체적인 정보 확인

  - git remote rename [단축이름변경전] [단축이름변경후] : remote 저장소 이름 변경

  - git remote rm : remote 저장소 제거

- git remote add [단축이름] [URL] : remote 저장소 추가, 단축이름으로 관리할 수 있음

- git fetch [remote-name(단축이름)] : remote 저장소의 데이터를 모두 로컬로 가져옴

  - 단 머지하지 않음, 수동 머지(git merge)

- git pull : remote 저장소의 데이터를 모두 로컬로 가져오고, 로컬 브랜치와 머지

- git push [remote-name(단축이름)] [브랜치명] : remote 저장소에 commit 내용 반영

<br>
<br>

## git tag

- git tag : tag 조회

  - git tag -l '검색패턴' : 검색패턴으로 조회 (예 1.2*)

- Annotated 태그 : 태그를 만든 사람 이름/이메일, 태그 만든 날짜, 태그 메시지 저장, 서명가능

  - git show : commit / tag 확인

  - git tag -a [태그명] -m '[태그메시지]' : 태그명으로 태그 생성
  <br>뒤에 커밋 체크섬(전체 혹은 앞의 유니크한 일부분) 명시하면 기존 커밋에 태그 가능

  - git tag -s [태그명] -m '[태그메시지]' : GPG 개인키로 서명 태그 생성

  - git tag -v [태그명] : GPG 사용하여 서명 검증, 서명자의 GPG 공개키 필요

- Lightweight 태그 : 특정 커밋에 대한 포인터(단순 태그)

  - git tag [태그명]

- git push [remote명] [태그명] : remote server에 태그 push (git push만 하면 태그 push X)

<br>
<br>

## Git commit, push 취소(롤백)

- 필요할 때 보려고 모아놓은 것이라 위랑 중복되는 것도 있음

- add 취소(Staged → Unstaged)

  - git reset HEAD [File명] : add 취소

  - git checkout -- [File명] : 수정한 사항 처음으로 되돌리기

- commit 취소

  - git commit --amend : 가장 최신 commit에 내용 추가

  - git reset --[Option] HEAD^ : Coomit 취소

  > Option
  >
  > soft : HEAD가 가리키는 브랜치를 옮긴다.
  >
  > mixed : default, Staged된 파일을 HEAD가 가리키는 상태로 만든다.
  >
  > hard : 현재 작업하는 디렉토리도 HEAD가 가리키는 상태로 만든다. (강력/위험)

  > HEAD : 현재 브랜치를 가리키는 포인터
  >
  > ^ : 바로 전 커밋 , ^^ : 바로 전전, ^^^ : 바로 전전전
  >
  > ~: 마지막 1개, ~2 : 마지막 2개, ~3 :마지막 3개

- push 취소

  - git reset 후 재 push : 기존 커밋으로 다시 push 해버리기
  <br>git push origin --force : git push 할 수 없다고 하면 강제로 밀어넣기

  - git revert {Commit ID} : commit은 유지하면서 내용을 rollback
  <br>Commit이 유지되기때문에 Remote Server와 충돌이 적게 나는 장점
