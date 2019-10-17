---
layout: post
title:  "[Java] 다형성(Polymorphism)"
date:   2019-08-23 23:19:25
author: Choi HyeSun
categories: java
tags:
  - Java
  - 다형성
  - polymorphism
  - vector
  - 벡터
  - instanceof
---

## 다형성(Polymorphism) - 정의

- 객체지향개념에서의 다형성

  - 여러가지 형태를 가질 수 있는 능력

- 자바에서의 다형성
  
  - 한 타입의 참조변수로 여러 타입의 객체를 참조할 수 있음

  - 부모클래스 타입의 참조변수로 자식클래스의 인스턴스를 참조할 수 있음
  
<br>
<br>

## 다형성 - 특징

- 부모클래스 타입의 참조변수에서 자식클래스의 인스턴스를 참조할 수 있음

  - 부모클래스명 참조변수 = new 자식클래스(매개변수);

- 단, 참조변수의 타입에 따라 사용할 수 있는 멤버의 개수가 달라짐

  - 자식클래스 타입의 참조변수 = 전체 접근

  - 부모클래스 타입의 참조변수 = 부모클래스에 정의된 멤버만 접근

- 반대로는 불가능함

  - 컴파일 에러

  - 참조변수가 사용할 수 있는 멤버의 개수는 인스턴스의 멤버 개수보다 같거나 적어야함
  <br>자식클래스가 부모클래스의 사용가능한 멤버 개수가 더 많기 때문에
  
<br>
<br>

## 참조변수의 형변환

- 기본형 변수처럼 참조형 변수도 형변환이 가능함

  - 단, 서로 상속관계에 있는 클래스 사이에서만 가능(조상의 조상도 가능)

- 참조변수의 타입을 변경하는 것 != 실제 인스턴스의 타입을 변환하는 것

- 인스턴스에는 아무 영향을 미치지 않음

- 자식타입 ↔ 부모타입

  - 자식타입 → 부모타입(Up-casting) : 생략 가능

  - 자식타입 ← 부모타입(Down-casting) : 생략불가
  <br>다룰수 있는 참조변수의 멤버 > 인스턴스의 멤버 이므로, 문제가 발생할 가능성이 있음
  <br>instanceof 연산자를 사용하여 확인 필요

- 주의)

  - 참조변수가 가리키는 인스턴스의 자식타입으로는 형변환 비허용

  - 참조변수가 가리키는 인스턴스의 타입이 무엇인지 확인할 것 → instanceof
  
<br>
<br>

## instanceof 연산자

- 참조변수가 참조하고 있는 인스턴스의 실제 타입을 알아보기 위해 사용

- 사용방법

  - 참조변수 instanceof 클래스명(타입)

- 반환값(boolean)

  - true : 참조변수가 검사한 타입으로 형변환 가능

  - false : 불가, 값이 null인 참조변수에 대해 수행시도 false

- 참조변수.getClass().getName() → 참조변수가 가리키고 있는 인스턴스의 클래스 이름을 문자열로 변환

<br>
<br>

## 참조변수와 인스턴스의 연결

- static

  - 변수, 메서드 모두 참조변수의 타입에 영향을 받음

  - 부모클래스타입 참조변수 → 부모클래스의 static 변수/메서드

  - 자식클래스타입 참조변수 → 자식클래스의 static 변수/메서드

  - 혼란의 소지가 있으니, 반드시 참조변수가 아닌 '클래스이름.메서드()'로 호출할 것

- 중복정의되지 않은 멤버변수/메서드

  - 참조변수의 타입에 영향을 받지 않고 실제 인스턴스의 변수/메서드 호출

- 중복정의된(overriding) 메서드

  - 참조변수의 타입에 영향을 받지 않고 실제 인스턴스의 메서드(오버라이딩된 메서드) 호출

- 중복정의된(overriding) 멤버변수

  - 참조변수의 타입에 영향을 받음

  - 부모클래스타입 참조변수 → 조상클래스에 선언된 멤버변수

  - 자식클래스타입 참조변수 → 자식클래스에 선언된 멤버변수

  - 그래서 멤버변수들을 주로 private로 접근 제어하고, 메소드로 값에 접근할 수 있도록 !

<br>
<br>

## 매개변수의 다형성

- 매개변수도 다형적인 특징이 적용됨

<br>
<br>

## 배열로 여러 종류의 객체 다루기

- 조상타입의 참조변수 배열을 사용하면, 공통의 조상을 가진 서로 다른 종류의 객체를 배열로 묶어서 다룰 수 있음

  - 묶어서 다루고싶은 객체들의 상속관계를 따져서 가장 가까운 공통조상 클래스 타입의 참조변수 배열을 생성해서 객체들을 저장
  
- Vector 클래스

  - 내부적으로 Object 타입의 배열을 가지고 있음 > 해당 배열에 객체를 추가/제거
  
  - 배열의 크기를 알아서 관리

  - 주요 메서드
  
|메서드와 생성자|설명|
|---|---|
|Vector()|10개의 객체를 저장할 수 있는 Verctor 인스턴스를 생성<br>10개 이상의 인스턴스가 저장되면 크기가 자동 조절|
|boolean add(Object o)|Vector에 객체 추가<br>추가에 성공하면 결과값 true, 실패하면 false 반환|
|boolean remove(Object o)|Vector에 저장되어 있는 객체 제거<br>제거에 성공하면 결과값 true, 실패하면 false 반환|
|boolean isEmpty()|Vector가 비어있는지 확인<br>비어있다면 결과값 true, 비어있지 않다면 false 반환|
|Object get(int index)|지정된 위치(index)의 객체를 반환<br>반환타입 Object<br>적절한 타입으로 형변환 필요|
|int size()|Vector에 저장된 객체의 개수 반환|