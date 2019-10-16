---
layout: post
title:  "[Java] Scanner vs BufferedReader"
date:   2019-08-16 19:11:05
author: Choi HyeSun
categories: java
tags:
  - Java
  - Overloading
  - 오버로딩
  - varargs
  - 가변인자
---

## 오버로딩(Overloading) 정의

- 한 클래스 내에 같은 이름의 메서드를 여러 개 정의하는 것

- 매개변수의 개수 또는 타입이 다르면, 같은 이름으로 여러 개 정의 가능

- 반환 타입은 관계 없음

- 대표적인 예) println 메서드(10개의 overloading)

<br>
<br>

## 오버로딩의 장점

- 사용자가 같은 동작에 대하여 각각의 메서드 명을 알 필요가 없음

- 메서드의 이름 절약 가능

- 이름 짓는데 고민 절약

- 사용되었어야 할 메서드 이름을 다른 곳에 사용 가능

<br>
<br>

## 가변인자(varargs; variable arguments)

- 메서드의 매개변수 개수를 동적으로 지정할 수 있음

- 타입... 변수명과 같은 형식으로 선언

- 매개변수 중 가장 마지막에 선언해야함 (가변인자인지 아닌지 구분할 수 없기 떄문)

  - 예 - 안 됨) void sunny(double... sunnyDouble, int sunnyInt) { } // X

  - 예 - 됨)     void sunny(int sunnyInt, double... sunnyDouble) { } // O

- 가변인자는 인자가 0개 ~ n개 + 배열까지 가능

  - 가변인자는 내부적으로 배열을 이용
  
- 가변인자가 선언된 메서드를 호출할 때마다 배열이 새로 생성되므로 꼭 필요할 때만 사용할 것

- 사용 예시

  - 메소드 선언) String weather(String... str) { ... } // 가변인자

  - 됨) weather(); // 인자 없음. 단, 매개변수 타입이 배열일 경우엔 불가능함

  - 됨) weather("sunny"); // 인자 1개

  - 됨) weather("sunny", "cloudy"); // 인자 2개

  - 됨) weather(new String[]{"A", "B"}); // 배열도 가능<br>단, weather({"A", "B"});는 허용하지 않음 (주소 참조로 되는데, 올린 곳이 없으니까)

<br>
<br>

## 가변인자와 배열의 차이점

- 매개변수 타입이 배열인 경우에는 반드시 인자를 지정해줘야 함(생략 불가)

- null이나 길이가 0인 배열을 인자로 꼭 지정해줘야 함

<br>
<br>

## 가변인자 사용시 유의할 점

- 가변인자를 선언한 메서드를 오버로딩했을 때 구분하기 어려우므로 가변인자가 사용되었을 때는 가능한 오버로딩하지 않는 것이 좋음

  - 예 - 기준)  void sunny(String sunny, String... args);

  - 예 - 안 됨) void sunny(String... args); // 겹침

  - 예 - 됨)     void sunny(Int sunny, String... args); // 안겹침

{% highlight java %}

{% endhighlight %}

![image](/img/2019-10-14/CentOS-Install-CLI-040-putty4.png)
