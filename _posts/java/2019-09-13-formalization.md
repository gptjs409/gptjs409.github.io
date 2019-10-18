---
layout: post
title:  "[Java] 형식화 클래스"
date:   2019-09-13 22:54:36
author: Choi HyeSun
categories: java
tags:
  - Java
  - 형식화
  - 패턴
  - DecimalFormat
---

## 형식화 클래스

- java.text패키지에 포함되어 있음

- 숫자, 날짜, 텍스트 데이터를 일정한 형식에 맞게 표현할 수 있는 방법을 객체지향적으로 설계하여 표준화

- 형식화에 사용될 패턴을 정의

  - 데이터를 정의된 패턴에 맞춰 형식화할 수 있을 뿐만 아니라 역으로 형식화된 데이터에서 원래의 데이터를 얻어낼 수도 있음

  - 패턴을 정의하는 것이 전부라고 해도 과언이 아님
  
<br>
<br>

## DecimalFormat

- 형식화 클래스 중, 숫자를 형식화하는데 사용되는 것

- 숫자 데이터를 정수, 부동소수점, 금액 등의 다양한 형식으로 표현할 수 있고 일정한 형식의 텍스트 데이터를 숫자로 쉽게 변환하는 것도 가능함

- 사용하는 방법

  - 원하는 출력형식의 패턴을 작성하여 DecimalFormat인스턴스를 생성

  - 출력하고자하는 문자열로 format메서드를 호출

  - 원하는 패턴에 맞게 변환된 문자열을 얻게 됨
  
{% highlight java %}
double number = 1234567.89;
DecimalFormat df = new DecimalFormat('#.#E0');
String result = df.format(number);
{% endhighlight %}

- 기호와 문자가 포함된 문자열을 숫자로 쉽게 변환하는 법

  - 사용시 : Integer.parseInt 메서드 등은 콤마(,)등이 포함된 문자열을 숫자로 변환하지 못함 
  
  - 사용법 : public Number parse(String source) throws ParseException;
  <br>Number 클래스 = Integer, Double과 같은 숫자를 저장하는 래퍼 클래스의 조상
  <br>doubleValue(), intValue(), floatValue() 등의 메서드가 정의되어 있음
  <br>→ Number에 저장된 값을 해당 형식으로 변환하여 반환
  
|기호|의미|패턴|결과(1234567.89)|
|---|---|---|---|
|**0**|**10진수(값이 없을 때는 0)**|0<br>0.0<br>0000000000.0000|12345678<br>1234567.9<br>0001234567.8900|
|**\#**|**10진수**|\#<br>\#.\#<br>\#\#\#\#\#\#\#\#\#.\#\#\#\#|12345678<br>1234567.9<br>1234567.89|
|**.**|**소수점**|\#.\#|1234567.9|
|**-**|**음수부호**|\#.\#\-<br>-\#.\#|1234567.9-<br>-1234567.9|
|**,**|**단위 구분자**|\#,\#\#\#.\#\#<br>\#,\#\#\#\#.\#\#|1,234,567.89<br>123,4567.89|
|**E**|**지수기호**|\#E0<br>0E0<br>\#\#E0<br>00E0<br>\#\#\#\#E0<br>0000E0<br>\#.\#E0<br>0.0E0<br>0.000000000E0<br>00.00000000E0<br>000.0000000E0<br>\#.\#\#\#\#\#\#\#\#E0<br>\#\#.\#\#\#\#\#\#\#E0<br>\#\#\#.\#\#\#\#\#\#E0|.1E7<br>1E6<br>1.2E6<br>12E5<br>123.5E4<br>1235E3<br>1.2E6<br>1.2E6<br>1.234567890E6<br>12.34567890E5<br>123.4567890E4<br>1.23456789E6<br>1.23456789E6<br>1.23456789E6|
|**;**|**패턴구분자**|\#,\#\#\#,\#\#+;\#,\#\#\#,\#\#-|1,234,567.89+(양수일 때)<br>1,234,567.89-(음수일 때)|
|**\%**|**퍼센트**|\#.\#\%|123456789\%|
|**\\u2030**|**퍼밀(퍼센트 * 10)**|\#.\#\\u2030|1234567890‰|
|**\\u00A4**|**통화**|\\u00A4 \#,\#\#\#|\\ 1,234,568|
|**'**|**escape 문자**|'\#'\#,\#\#\#<br>''\#,\#\#\#|\#1,234,568<br>'1,234,568|

<br>
<br>

## SimpleDateFormat

- 형식화 클래스 중, 날짜를 형식화하는데 사용되는 것

  - Date와 Calendar를 이용해 날짜를 계산한다면, 이 클래스는 출력하는 방법

- Date와 Calender만으로는 날짜 데이터를 원하는 형태로 다양하게 출력하는 것이 불가능함

- SimpleDateFormat 인스턴스

  - DateFormat : 추상클래스이자 SimpleDateFormat 클래스의 부모클래스, 추상클래스이므로 인스턴스 생성 불가
  <br>인스턴스 생성시 getInstance()와 같은 static 메서드를 이용해야 함

  - getInstance()로 반환하는 것 → DateFormat을 상속받아 완벽히 구현한 SimpleDateFormat 인스턴스
  
- 사용 방법

  - 원하는 출력형식의 패턴을 작성하여 SimpleDateFormat 인스턴스를 생성

  - 출력하고자 하는 Date 인스턴스를 가지고 fommat(Date d)를 호출하면 지정한 출력 형식에 맞게 변환된 문자열을 얻게 됨

  - Date인스턴스만 format메서드에 사용될 수 있기 때문에 Calendar 인스턴스를 Date 인스턴스로 변환해야 함
  
{% highlight java %}
/* 오늘 날짜를 yyyy-MM-dd형태로 변환하여 반환하기
 */
Date today = new Date();
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");

String result = df.format(today);

/* Calender를 Date로 변환하기 
 */
Calendar calSunny = Calendar.getInstance();
calSunny.set(2019, 7, 13); // 2019년 8월 13일 Setting - Month는 0~11범위이므로!

Date daySunny = calSunny.getTime(); // Calendar를 Date로 변환

/* Date 인스턴스 → Calender 인스턴스 : Calender.setTime()
 */
{% endhighlight %}

- SimpleDateFormat.parse(String source)

  - 문자열 source를 날짜Date 인스턴스로 변환

  - 부모클래스인 DateFormat에 정의

  - 지정된 형식과 입력된 형식이 다를 경우 예외 발생 → 적절한 예외처리 필요 (parseException)

  - 사용법
  <br>DateFormat df = new SimpleDateFormat("yyyy년 MM월 dd일");
  <br>Date d = df.parse("2019년 8월 13일");

  - 사용 기호
  
|기호|의미|예시|
|---|---|---|
|G|연대(BC, AD)|AD|
|y|연도|2019|
|M|월(1~12 또는 1월~12월)|8 또는 8월, OCT|
|w|년의 몇 번째 주(1~53)|12|
|W|월의 몇 번째 주(1~5)|3|
|D|년의 몇 번째 일(1~366)|123|
|d|월의 몇 번째 일(1~31)|13|
|F|월의 몇 번째 요일(1~5)|2 (→ 이번달의 N번째 화요일)|
|E|요일|월|
|a|오전/오후(AM, PM)|AM|
|H|시간(0~23)|20|
|k|시간(1~24)|13|
|K|시간(0~11)|10|
|h|시간(1~12)|11|
|m|분(0~59)|50|
|s|초(0~59)|55|
|S|1/1000초 (0~999)|231|
|z|Time zone(General time zone)|GMT+9:00|
|Z|Time zone(RFC 822 time zone)|+0900|
|'|escape문자(특문표현)<br>'(따옴표)는 escape 문자이기 때문에, 사용하려면 escape 문자를 붙여서 ''|없음|

<br>
<br>

## ChoiceFormat

{% highlight java %}

{% endhighlight %}

{% highlight java %}

{% endhighlight %}
