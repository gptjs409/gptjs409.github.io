---
layout: post
title:  "[Java] BigDecimal"
date:   2019-08-02 08:25:52
author: Choi HyeSun
categories: java
tags:
  - Java
  - BigDecimal
  - setScale
  - compareTo
  - add
  - substract
  - multiply
  - ROUND_UP
  - ROUND_DOWN
  - ROUND_HALF_UP
  - ROUND_HALF_DOWN
---

## BigDecimal

- 정확한 숫자 계산을 위한 클래스

- java.math 패키지에 포함되어있음 (import 필요)

- 선언 방식

  - BigDecimal (이름) = new BigDecimal("double형식 String");

> 주의) 선언시 Double이 아니라 String을 넣어야 함

{% highlight java %}
// import 필요
import java.math.BigDecimal;

// 선언 및 초기화
BigDecimal db1 = new BigDecimal("1.0");
{% endhighlight %}

<br>
<br>

## BigDecimal 사칙연산(+-*/)
- BigDecimal Type끼리만 가능

{% highlight java %}
BigDecimal number1 = new BigDecimal("1.0");
BigDecimal number2 = new BigDecimal("2.0");
{% endhighlight %}

- + = add

{% highlight java %}
number1.add(number2);
{% endhighlight %}

- - = subtract

{% highlight java %}
number1.substract(number2);
{% endhighlight %}

- * = multiply

{% highlight java %}
number1.multiply(number2);
{% endhighlight %}

- / = divide

{% highlight java %}
/* BigDecimal.ROUND_UP : 올림
 * BigDecimal.ROUND_DOWN : 내림
 * BigDecimal.ROUND_HALF_UP : 반올림(5<=x<9 ↑)
 * BigDecimal.ROUND_HALF_DOWN : 반내림(5<x<9 ↑, 5가 포함되지 않음)
 */
/* 기준값.divide("나눌값", 올림내림표현) → num = 1과 같음
 * 기준값.divide("나눌값, num, 올림내림표현) → 소숫점 num째 자리까지 남겨둠(num+1째 자리에 적용)
 * 0일 경우 → 1의자리, 1이상일경우 → 지수부/가수부 표현(예 1E+1)
 */
number1.divide(number2, BigDecimal.ROUND_UP);
number1.divide(number2, 2, BigDecimal.ROUND_HALF_UP);
{% endhighlight %}


<br>
<br>

## 비교(VS)

- 비교 연산자로 비교가 불가능함(==, >=, >, <=, <, !=)

- compareTo 함수 이용

- BigDecimal Type끼리만 가능

- 응답값 (a.compareTo(b))
  
  - -1 : 첫 값이 둘째 값보다 작음 (a < b)
  
  - 0 : 첫 값이 둘째 값과 같음 (a = b)
  
  - 1 : 첫 값이 둘째 값보다 큼 (a > b)

{% highlight java %}
BigDecimal number1 = new BigDecimal("1.0");
BigDecimal number2 = new BigDecimal("2.0");

number1.compareTo(number2);
{% endhighlight %}

<br>
<br>

## 반올림/올림/내림/반내림

- 특정 자리수에서 반올림/올림/내림/반내림 작업

- 올림내림 표현
  
  - BigDecimal.ROUND_UP : 올림
  
  - BigDecimal.ROUND_DOWN : 내림
  
  - BigDecimal.ROUND_HALF_UP : 반올림(5<=x<9 ↑)
  
  - BigDecimal.ROUND_HALF_DOWN : 반내림(5<x<9 ↑, 5가 포함되지 않음)

- setScale 함수 이용

  - 기준값.setScale(올림내림표현) → num = 1과 같음
  
  - 기준값.setScale(num, 올림내림표현) → 소숫점 num째 자리까지 남겨둠(num+1째 자리에 적용)
  
  - 0일 경우 → 1의자리, 1이상일경우 → 지수부/가수부 표현(예 1E+1)
 
{% highlight java %}
BigDecimal number1 = new BigDecimal("1.0");

number1.setScale(BigDecimal.ROUND_UP);
number1.setScale(1, BigDecimal.ROUND_HALF_UP);
{% endhighlight %}
