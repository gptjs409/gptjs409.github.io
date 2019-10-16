---
layout: post
title:  "[Java] 클래스(Class)와 객체(Object)"
date:   2019-08-14 23:15:27
author: Choi HyeSun
categories: java
tags:
  - Java
  - Class
  - 클래스
  - Object
  - 객체
---

## 클래스와 객체

|클래스|객체|
|---|---|
|제품 설계도|제품|
|TV 설계도|TV|
|붕어빵 기계|붕어빵|

- 클래스

  - 정의 : 객체를 정의해놓은 것

  - 용도 : 객체를 생성하는데 사용

- 객체

  - 정의 : 실제로 존재하는 사물 또는 개념

  - 용도 : 가지고 있는 기능과 속성에 따라 다름

  - 유형의 객체 : 책상, 의자, 자동차, TV 등의 사물

  - 무형의 객체 : 수학공식, 프로그램 에러와 같은 논리나 개념

- JDK(Java Development Kit)에서는 프로그래밍을 위해 많은 수의 유용한 클래스(Java API)를 기본적으로 제공

  - 이 클래스들을 이용해서 원하는 기능의 프로그램을 보다 쉽게 작성할 수 있음

<br>
<br>

## 객체와 인스턴스

- 인스턴스화(instantitate) : 클래스로부터 객체를 만드는 과정

- 인스턴스(instance) : 어떤 클래스로부터 만들어진 객체를 그 클래스의 인스턴스라고 함

- 객체 : 모든 인스턴스를 대표하는 포괄적인 의미

  - 붕어빵은 객체다

- 인스턴스 : 어떤 클래스로부터 만들어진 것인지 강조하는 보다 구체적인 의미

  - 붕어빵은 붕어빵틀 클래스의 인스턴스다

<br>
<br>

## 객체의 속성과 기능

- 객체 : n개의 속성 + n개의 기능

- 속성(Property) : 멤버변수(member variable), 특성(attribute), 필드(field), 상태(state)

  - 객체지향에서는 변수로 표현

- 기능(Function) : 메서드(method), 함수(function), 행위(behavior)

  - 객체지향에서는 메서드로 표현
  
<br>
<br>

## 인스턴스 생성과 사용

- 클래스명 변수명;  (ex) Weather sunny;

  - Weather 클래스 타입의 참조변수 sunny를 선언

  - 메모리에 참조변수 sunny의 공간이 마련됨(주소값 저장될 공간)

- sunny = new Weather();

  - 연산자 new에 인해 Weather클래스의 인스턴스가 메모리에 빈 공간에(예.0x101) 생성

  - 멤버변수는 각 자료형에 해당하는 기본값으로 초기화됨

  - 대입연산자(=)를 통해 생성된 객체의 주소값(예.0x101)이 참조변수 sunny에 저장됨

  - 참조변수 sunny를 통해 인스턴스에 접근 및 조작이 가능함

- sunny.today = "Sunny";

  - 참조변수 sunny에 저장된 주소(예.0x101)에 있는 인스턴스의 멤버변수 today에 "Sunny"를 저장

  - 인스턴스의 멤버변수(속성) 사용시 '참조변수.멤버변수'

- sunny.todayCloudy();

  - 참조변수 sunny에 저장된 주소(예.0x101)에 있는 인스턴스의 todayCloudy메서드를 호출

  - todayCloudy메서드 → today 멤버변수를 "Cloudy"로 변환

- System.out.println(sunny.today);

  - 참조변수 sunny가 참조하고 있는 Weather인스턴스의 멤버변수 today에 저장된 값을 출력

  - Cloudy 출력
  
<br>
<br>

## 인스턴스 특징

- 인스턴스는 참조변수를 통해서만 다룰 수 있으며, 참조변수의 타입은 인스턴스의 타입과 일치해야 함

- 같은 클래스로부터 생성되었을지라도 각 인스턴스의 속성(멤버 변수)은 서로 다른 값을 유지할 수 있으며, 메서드의 내용은 모두 인스턴스에 대해 동일함

  - 인스턴스별로 각각 다른 메모리에 올라가기 때문

  - 메서드의 내용의 경우, 클래스로부터 만들어지기 때문(틀)

- 자신을 참조하고 있는 참조변수가 하나도 없는 인스턴스는 더이상 사용되어질 수 없으므로 '가비지 컬렉터(Garbage Collector)'에 의해서 자동적으로 메모리에서 제거됨

- 참조변수 여럿이 하나의 인스턴스를 가리키는(참조하는)것은 가능함

  - 주의) 그럴 경우 값이 바뀌면 같은 주소를 바라보기 때문에, 다른 참조하는 변수를 사용시에도 값이 변경된 것으로 나옴

- 예시(다른 주소를 바꿀 경우)

{% highlight java %}
Weather sunny = new Weather(); 
// Weather에서 인스턴스를 생성하여(이하 인스턴스1) sunny가 참조함
Weather cloudy = new Weather();
// Weather에서 인스턴스를 생성하여(이하 인스턴스2) cloudy가 참조함 
sunny.today = 1;
// sunny를 통해 인스턴스1의 today 멤버변수를 1로 초기화
System.out.println(cloudy.today);
// cloudy를 통해 인스턴스2의 today 멤버변수에 접근하여 값을 가져옴 : 초기값인 0이 출력됨
{% endhighlight %}

- 예시(같은 주소를 바꿀 경우)

{% highlight java %}
Weather sunny = new Weather(); 
// Weather에서 인스턴스를 생성하여(이하 인스턴스1) sunny가 참조함
Weather cloudy = new Weather();
// Weather에서 인스턴스를 생성하여(이하 인스턴스2) cloudy가 참조함 
cloudy = sunny;
// cloudy가 참조하는 주소값이 인스턴스2에서 인스턴스1로 변경됨
// 인스턴스2는 garbage collector 제거될 예정
sunny.today = 1;
// sunny를 통해 인스턴스1의 today 멤버변수를 1로 초기화
System.out.println(cloudy.today);
// cloudy를 통해 인스턴스1의 today 멤버변수에 접근하여 값을 가져옴 : 1이 출력됨
{% endhighlight %}

<br>
<br>

## 객체 배열

- 참조변수배열임

  - 객체배열 → 객체배열값(객체주소참조값) → 객체값
  
  - 예) String 배열
  
- 선언 방법

{% highlight java %}
// 선언
Weather weather = new Weather[2];

// 초기화
weather[0] = new Weather();
weather[1] = new Weather();

// 선언 및 초기화 - 나열
Weather weather = { new Weather(), new Weather(), new Weather() };

// 선언 및 초기화 - 반복문
Weather weather = new Weather[200];

for(int i = 0; i < 200; i++) {
    weather[i] = new Weather();
}
{% endhighlight %}

- 하나의 배열로 여러 종류의 객체를 다루려면

  - 다형성을 공부하고 나면 할 수 있음

<br>
<br>

## 프로그램 관점에서의 클래스

- 데이터와 함수의 결합

  - 변수 : 하나의 데이터를 저장할 수 있는 공간

  - 배열 : 같은 종류의 여러 데이터를 하나의 집합으로 저장할 수 있는 공간

  - 구조체 : 서로 관련된 여러 데이터를 종류에 상관없이 하나의 집합으로 저장할 수 있는 공간

  - 클래스 : 데이터와 함수의 결합(구조체 + 함수)
  
- 사용자 정의 타입(user-defined type)

  - 프로그래밍언어에서 제공하는 자료형(primitive type)외에 프로그래머가 서로 관련된 변수를 묶어서 하나의 타입으로 새로 추가하는 것(user-defined type)

  - 자바와 같은 객체지향언어에서는 클래스가 곧 사용자정의 타입
  
  - 다른 프로그래밍 언어에서도 사용자정의 타입을 정의할 수 있는 방법을 제공
  
  - 객체지향언어에서는 입력값에 대한 추가적인 조건을 반영할 수 있음
  
|비객체지향 코드|객체지향 코드|
|int   a1, a2, a3;<br>float b1, b2, b3;<br>char c1, c2, c3;|Abc a1 = new Abc();<br>Abc b1 = new Abc();<br>Abc c1 = new Abc();|
|int[] a    = new int[3];<br>float[] b = new float[3];<br>char[] c = new char[3];|Abc[ ] abc = new Abc[3];<br>abc[0] = new Abc();<br>abc[1] = new Abc();|<br>abc[2] = new Abc();|
