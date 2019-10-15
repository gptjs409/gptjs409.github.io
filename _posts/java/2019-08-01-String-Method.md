---
layout: post
title:  "[Java] String 문자열 메소드"
date:   2019-08-01 08:43:27
author: Choi HyeSun
categories: java
tags:
  - Java
  - String
  - 문자열메소드
  - String메소드
  - indexOf
  - length
  - charAt
  - substring
  - toUpperCase
  - toLowerCase
  - trim
  - replace
  - replaceAll
  - replaceFirst
---

## 대상문자열.indexOf(문자(열), 찾기시작할index) 메소드 (↔ lastIndexOf)

- 대상문자열의 찾기시작할index부터 해당 문자(열)가 들어있는 시작점의 index를 알려줌

  - index는 0부터 시작, 만약 없다면 -1를 return
  
- 인수를 넣지 않을 경우,  찾기 시작할 index = 0

  - (문자) `"abcd".indexOf('a')` → 0 = `"abcd".indexOf('a', 0))`
  
  - (문자) `"abcd".indexOf('e')` → -1 = `"abcd".indexOf('e', 0))`
  
  - (문자열) `"abcd".indexOf("bc")` → 1 = `"abcd", indexOf("bc", 0))`
  
  - (문자열) `"abcd".indexOf("bd")` → -1 = `"abcd", indexOf("bd", 0))`
  
- 인수를 넣을 경우, 찾기 시작할 index부터 문자(열) 검색
 
  - index 3부터인 "dabcd"에서 검색 `"abcdabcd".indexOf('a', 3) → 4` 
  
  - index 5부터인 "bcd"에서 검색 `"abcdabcd".indexOf("ab", 5) → -1` 

- lastIndexOf는 indexOf의 반대로 검색 (뒤에서부터)

  - 여기서 뒤에서부터라는건 뒤에서부터 검색한다는 의미(인덱스 번호가 뒤에서부터 체킹된다는 것이 아님)
  
  - 인수를 넣을 경우, 처음부터 찾기 시작할 index까지 문자(열) 검색
  
  - `"abcdeabce".indexOf('ab')` → 5
  
  - `"abcdeabce".indexOf("ab", 4)` → 0 (index 4까지인 "abcde"에서 검색)
  
<br>
<br>

## 대상문자열.length() 메소드

- 대상문자열의 길이를 나타냄

- `"abcd".length()` → 4

<br>
<br>

## 대상문자열.charAt(인덱스번호) 메소드

- 대상문자열에서 인덱스번호의 단어를 char로 호출

  - 인덱스번호 : 0부터 시작 

- `"abcd".charAt(0)` → a

<br>
<br>

## 대상문자열.substring(인덱스번호a, 인덱스번호b) 메소드

- 대상문자열에서 인덱스번호a <=찾을문자열<인덱스번호b 에 해당하는 찾을문자열 찾기

  - 인덱스번호a는 찾을문자열에 속하고, b는 속하지 않음

- `"abcd".subString(1,3)` → "bc"

<br>
<br>

## 대상문자열.toUpperCase() 메소드

- 대상 문자열 모두 대문자로 변환

- `"hello java".toUpperCase()` → "HELLO JAVA"

- `"Hello java".toUpperCase()` → "HELLO JAVA"

<br>
<br>

## 대상문자열.toLowerCase() 메소드

- 대상 문자열 모두 소문자로 변환

- `"HELLO JAVA".toLowerCase()` → "hello java"

- `"Hello Java".toLowerCase()` → "hello java"

<br>
<br>

## 대상문자열.trim() 메소드

- 대상 문자열 앞/뒤의 공백문자를 모두 제거

- `" hello java   ".trim()` → "hello java"

<br>
<br>

## 대상문자열.replace("바꾸기전문자열", "바꾸고난문자열") 메소드

- 해당 문자열의 "바꾸기전문자열"을 모두 "바꾸고난문자열"로 찾아바꿔줌

- 특수문자 가능

<br>
<br>

## 대상문자열.replaceAll("바꾸기전문자열", "바꾸고난문자열") 메소드

- 해당 문자열의 "바꾸기전문자열"을 모두 "바꾸고난문자열"로 찾아바꿔줌

- 특수문자입력시 정규표현식으로 인식

- 특수문자를 입력하려면 [ ]를 붙여주면 됨

- 괄호를 입력하려면 \\\\를 붙여주면 됨 (예외 ^도 \\\\를 붙여줘야 함)

- 자바의 특수문자는 \\를 붙여주면 됨

<br>
<br>

## 대상문자열.replaceFirst("바꾸기전문자열", "바꾸고난문자열") 메소드

- 해당 문자열의 "바꾸기전문자열"을 "바꾸고난문자열"로 앞에서부터 한 번만 찾아바꿔줌
