---
layout: post
title:  "[Java] 형식화된 출력 - printf() / 화면에서 입력받기 - Scanner"
date:   2019-08-08 21:15:09
author: Choi HyeSun
categories: java
tags:
  - Java
  - printf
  - println
  - scanner
  - 출력
  - 형식화된출력
---

## System.out.println(값);

- 사용이 편리하지만, 변수의 값을 그대로 출력

- 값을 변환하지 않고는 다른 형식으로 출력할 수 없음

<br>
<br>

## System.out.printf(값(, 지시자));

- 지시자(specifier)를 통해 변수의 값을 여러 가지 형식으로 변환하여 출력하는 기능을 갖고 있음

- 지시자 : 값을 어떻게 출력할 것인지 지정해주는 역할

- printf()는 출력 후 줄바꿈을 하지 않음
  
  - 줄바꿈이 필요할 경우 개행문자 '\n' 또는 '%n'을 넣어주기

- 출력하는 값 개수 = 지시자 개수

{% highlight java %}
int age = 14;
System.out.printf("age:%d", age);
// → System.out.printf("age:%d", 14);
// → System.out.printf("age:14");
age:14 출력

int age = 14, year = 2017;
System.out.printf("age:%d year:%d", age, year);
// → System.out.printf("age:%d year:%d", 14, 2017);
// → System.out.printf("age:14, year:2017);
age:14 year:2017 출력
{% endhighlight %}

- 자주 사용되는 지시자

|지시자|설명|예|
|---|---|---|
|%b|boolean 형식으로 출력||
|%d|예) %d	decimal(10진) 형식으로 출력|printf("[%d]", 10);<br>→ [10]|
|    %nd<br>    예) %5d|공간 5칸 확보 후 뒤에서부터 출력<br>남는 공간 공란|printf("[%5d]", 10);<br>→ [   10]|
|    %-nd<br>    예) %-5n|공간 5칸 확보 후 앞에서부터 출력<br>남는 공간 공란|printf("[%-5d]", 10);<br>→ [10   ]|
|    %0nd<br>    예) %05d|공간 5칸 확보 후 뒤에서부터 출력<br>남는 공간 0으로 채우기|printf("[%05d]", 10);<br>→ [00010]|
|%o|octal(8진) 형식으로 출력||
|    %#o|접두사 0이 붙음||
|%x, %X|hexa-decimal(16진) 형식으로 출력||
|    %#x|접두사 0x가 붙음||
|    %#X|접두사 0X가 붙음||
|%f|floating-point(부동 소수점) 형식으로 출력||
|    %전체n자리.소수점아래n자리f<br>    예) %14.10f|전체 자리 중 소수점 아래 자리<br>- 소수점도 자릿수 1개로 포함<br>- 정수자리는 빈자리를 공백으로 채움<br>- 소수자리는 빈자리를 0으로 채움<br>예) 전체 자리 14자리 중 소수점 아래 10자리|printf("[%14.10f]", 1.23456789);<br>→ [  1.2345678900]|
|%e, %E|exponent(지수) 형식으로 출력||
|%c|character(문자)로 출력||
|%s|string(문자열)로 출력||
|    %ns|최소 n글자 출력공간 확보 후 뒤에서부터 출력<br>남은 공간 공란||
|    %-ns|최소 n글자 출력공간 확보 후 앞에서부터 출력<br>남는 공간 공란||
|    %.ns|왼쪽에서 n글자만 출력||

<br>
<br>

## Scanner

- 자바에서 화면으로부터 입력받는 방법 중 한 가지

  - 최신 방법은 JDK 1.6에서 추가된 Console 클래스를 이용하는 것
  <br>IDE에서 잘 동작하지 않을 수 있음
  
- 사용하려면 아래의 한 문장을 추가

{% highlight java %}
import java.util.*; // Scanner 클래스를 사용하기 위하여 추가
{% endhighlight %}

- Scanner 객체 생성

{% highlight java %}
Scanner scanner = new Scanner(System.in); // Scanner 객체 생성
{% endhighlight %}

- nexLine 메서드

  - 입력대기 상태에 있다가 입력을 마치고 '엔터키(Enter)'를 누르면 입력한 내용이 문자열로 반환

{% highlight java %}
String input = scanner.nextLine(); // 입력받을 내용을 input에 저장
int num = Integer.parseInt(input); // 입력받은 내용을 int 타입의 값으로 변환
{% endhighlight %}

- nextInt / nextFloat 메서드

  - 변환 없이 숫자로 바로 입력받을 수 있음

  - 화면에서 연속적으로 값을 입력받기 때문에 사용이 더 까다로울 수 있음
  
{% highlight java %}
int num = scanner.nextInt();
int num2 = scanner.nextFloat();
{% endhighlight %}
