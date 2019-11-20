---
layout: post
title:  "[자바기본스터디] 2019-11 자바기본스터디 2주차"
date:   2019-11-13 20:04:51
author: Choi HyeSun
categories: study
tags:
  - 자바스터디
---

### 스터디 2주차

#### Chapter 9 java.lang패키지 : 스터디 2주차

|중분류|소분류|
|---|---|
|1. Object클래스|1.1 equals메서드<br>1.2 hashCode메서드<br>1.3 toString메서드<br>1.4 clone메서드|
|2. String클래스|2.1 String클래스의 특징<br>2.2 빈 문자열(empty string)<br>2.3 String클래스의 생성자와 메서드|
|3. StringBuffer클래스|3.1 StringBuffer클래스의 특징<br>3.2 StringBuffer클래스의 생성자<br>3.3 StringBuffer인스턴스의 비교<br>3.4 StringBuffer클래스의 메서드|
|4. Math클래스|4.1 Math클래스<br>4.2 Math클래스의 메서드|
|5. wrapper클래스|5.1 wrapper클래스<br>5.2 Number클래스|

<br>
<br>

#### 참여인원

4명

<br>
<br>

#### 스터디 진행 방법

중분류 1 / 2 / 3 / 4 + 5 나눠서 각자 담당하여 강의식으로 설명 (with 화이트보드)

<br>
<br>

#### 스터디 중 궁금했던 부분

##### String Class

  - Java API의 speecial class
  
  - 많은 프로그래머에게 no obvious(분명하지 않은), special(특별한) behaviours(행위)들이 있음
  
  - Java를 마스터하기 위한 첫 번째 단계가 바로 String Class를 마스터하는 것
  
  - 따라서 면접때 String 관련 질문을 할 수도 있음!

<br>

##### JAVA에서 String literal과 New String Object의 차이점

{% highlight java %}
String strObject = new String("Java");
{% endhighlight %}

VS

{% highlight java %}
String strLiteral = "Java";
{% endhighlight %}

- 가장 자주 묻는 질문 중 하나
  
- 두 표현식 모두 String 객체를 제공

- new () 연산자 사용시 항상 heap memory에 새 객체를 생성함

- 문자열 리터럴 구문 (예 : "Java") 사용시, 문자열 풀에서 기본 오브젝트를 리턴할 수 있음

  - String pool(문자열 풀) : Perm Gen Space에 Spring object를 캐시했었으나, 최근 버전의 Java에서는(JDK 1.7 ~) Heap Space로 옮겨짐
  
  - 이미 존재하는 경우 : 문자열 풀에서 리턴
  
  - 존재하지 않는 경우 : 새 문자열 객체 생성 후 문자열 풀에 배치
  
  > PermGen? Permanent Generation
  >
  > 클래스의 정의들과 관련된 메타데이터를 위해 사용되는 메모리 공간
  >
  > Heap 메모리로 종종 착각되지만 Heap의 일부가 아님
  > 
  > JVM은 기본적으로 64MB의 메모리 공간(PermGen)을 할당 - JDK1.5기준 (JVM 종류/버전마다 다름)
  
<br>

##### String literal과 String pool

- String은 모든 응용 프로그램에서 가장 많이 사용되는 유형


  

<br>

##### String literal VS String object

- 둘 다 String 객체

- new ()로 생성되는 String object는 new () 연산자가 항상 새로운 String 객체를 생성

- 리터럴을 사용하여 문자열을 만들면 interned 됨 (아래서 설명)

- 예제1 : String literal VS String literal

{% highlight java %}
String a = "Java";
String b = "Java";
System.out.println(a == b);  // True
{% endhighlight %}

- 예제 2 : String object VS String object

{% highlight java %}
String c = new String("Java");
String d = new String("Java");
System.out.println(c == d);  // False
{% endhighlight %}

- 예제 3 : String literal VS String object

{% highlight java %}
String e = "JDK";
String f =  new String("JDK");
System.out.println(e == f);  // False
{% endhighlight %}

- 가능하면 문자열 리터럴 표기법을 사용해야 함

  - 읽기 쉽고 컴파일러가 코드를 최적화할 수 있는 기회를 제공

<br>

##### inter() method를 이용한 String interning

- Java는 기본적으로 모든 String 객체를 String Pool에 임의의 객체를 명시적으로 저장할 수 있는 유연성(flexibility)을 제공

  - 무조건 String Pool에 넣는 것이 아님!
  
- java.lang.String 클래스의 intern() 메소드를 이용하여 String pool에 Object를 넣을 수 있음

  - **Java의 String literal 방법을 이용한 String 생성시 자동으로 intern() 메서드를 호출하여 해당 객체를 String 풀에 넣음**


> String interning
>
> Computer science에서 string interning은 각각의 고유한 문자열 값의 사본 하나만 저장하는 방법으로, 변경이 불가능함(immutable)
>
>> **즉 String interning시,**
>> 
>> 해당 값이 있다면 String Pool에서 반환
>>
>> 해당 값이 없다면 Heap 메모리에 한 번 저장!! 후 그 값을 String Pool에 복제!

- Java에서 intern() 메소드 사용시

  - intern() 메소드가 실행되면 String이 String Pool의 String Object와 같은지 여부를 확인
  
  - 있다면 String Pool의 문자열 리턴
  
  - 없다면 String 객체가 풀에 추가되고 String 객체에 대한 참조가 반환
  
  - 두 문자열 s와 t에 대해 s.equals(t)가 true인 경우만 s.intern() == t.intern()가 true

<br>

##### 즉!

String literal 

- 자동으로 intern() 메서드를 호출하여 String pool에 Object를 넣음

- String pool에 해당 내역이 있는지 확인 후 있으면 해당 내역을 리턴

- String pool에 해당 내역이 없다면, heap memory에 생성 후 String pool에 복제

- ==시 동일 결과

String Object (new () 이용)

- intern() 메서드를 호출하지 않음, 수동으로 호출은 가능

- String pool을 확인하지 않음, heap memory에 생성

- ==는 결과가 다르고, .equals()시 동일 결과

종합

- String은 .equals()로 비교할 것!

  - String pool에서 객체가 오는지 새로 생성되는건지 알 수 없기 때문
