---
layout: post
title:  "[Java] DecimalFormat"
date:   2019-08-02 18:05:24
author: Choi HyeSun
categories: java
tags:
  - Java
  - DecimalFormat
  - Decimal
  - Format
---

## DecimalFormat

- java.text에 import 필요

- 소수점 자리수를 표시하기 위해 사용

- String 타입으로 반환해줌

<br>
<br>

## Format 종류
|Format|의미|
|---|---|
|0|10진수 빈자리는 0으로 채우기|
|#|10진수 빈자리는 채우지 않음|
|.|소수점|
|,|단위 구분|
|-|음수 기호|
|;|양수 음수 구분하여 출력|
|%|* 100을 한 후 %를 붙여 출력|
|\\u00A4|\\기호|
|E|지수 문자|

<br>
<br>

## 예제
{% highlight java %}
import java.text.DecimalFormat;

public class Sunny {
    public static void main(String[] args) {
        DecimalFormat df = new DecimalFormat("#.###");
        String a = df.format(12.2345);
        
        System.out.println(a);
    }
}
// 출력
// #.### = 12.235
// #.## = 12.23
// #.# = 12.2
{% endhighlight %}
