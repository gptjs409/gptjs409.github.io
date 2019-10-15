---
layout: post
title:  "[Java] BigDecimal"
date:   2019-08-05 19:19:19
author: Choi HyeSun
categories: java
tags:
  - Java
  - BigInteger
  - long
---

## java long의 범위

- -9223372036854775808 ~ 9223372036854775807

<br>
<br>

## java long보다 큰 수는 어떻게 해야하는가?

- java의 BigInteger 사용

<br>
<br>

## BigInteger란?

- java.math에 import하여야 사용이 가능

- Immutable arbitary-precision integers. = 무한대의 불변한 정수 (String도 Immutable함)

  - (주의) Immutable의 경우, 객체 연산시 새로운 객체가 생김

- 무한대의 숫자 표시 가능

<br>
<br>

## 선언하기

- 선언시 new BigInteger(String);

{% highlight java %}
import java.math.BigIntger;

// 생성1. String
BigInteger bi1 = new BigInteger("1"); // 1 생성

// 생성2. 클래스 변수 3가지(ZERO, ONE, TEN)
BigInteger bi2 = new BigInteger.ZERO; // 0
BigInteger bi3 = new BigInteger.ONE; // 1
BigInteger bi4 = new BigInteger.TEN; // 10

// 그외는 API 문서 확인
{% endhighlight %}

<br>
<br>

## 연산하기

- BigInteger끼리의 사칙연산
{% highlight java %}
BigInteger sunnybi;

BigInteger bi1 = new BigInteger.ZERO;
BigInteger bi2 = new BigInteger("0");

sunnybi = bi1.add(bi2); // 더하기
sunnybi = bi1.subtract(bi2); // 빼기
sunnybi = bi1.multiply(bi2); // 곱하기
sunnybi = bi1.divide(bi2); // 나누기
{% endhighlight %}

- int(변수)와 사칙 연산
{% highlight java %}
BigInteger sunnybi;

BigInteger bi1 = new BigInteger.ZERO;
int sunnyi = 2;

sunnybi = bi.add(BigInteger.valueOf(sunnyi)); // 더하기
sunnybi = bi.add(BigInteger.subtract(sunnyi)); // 빼기
sunnybi = bi.add(BigInteger.multiply(sunnyi)); // 곱하기
sunnybi = bi.add(BigInteger.divide(sunnyi)); // 나누기
{% endhighlight %}

> (주의) BigInteger연산은 항상 BigInteger 타입으로 반환됨
>
> int와 연산시, int를 BigInteger로 타입 변경 후 연산
