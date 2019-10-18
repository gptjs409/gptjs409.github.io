---
layout: post
title:  "[Java] 유용한 클래스"
date:   2019-09-11 23:15:53
author: Choi HyeSun
categories: java
tags:
  - Java
  - 유용한 클래스
  - useful class
  - BigDecimal
  - BigInteger
  - objects
  - random
  - regax
  - 정규식
  - Scanner
  - StringTokenizer
---

## java.util.Objects 클래스

- Object 클래스의 보조클래스, 모든 메서드가 static

- 객체의 비교나 널 체크(null check)에 유용

- import를 해주어도 Object와 메서드가 같으면 중복되어서 컴파일 에러가 나기 때문에, 클래스 이름을 붙여줘야 함

  - ex) Objects.toString();

- 메서드

|메서드|뜻|
|---|---|
|**static boolean isNull(Object obj)**|해당 객체가 널인지 확인해서 null이면 true, 아니면 false 반환|
|**static boolean nonNull(Object obj)**|해당 객체가 널인지 확인해서 null이면 false, 아니면 true 반환|
|**static \<T> T requireNonNull(T obj)<br>static \<T> T requireNonNull(T obj, String message)<br>static \<T> T requireNonNull(T obj, Supplier\<String> message Supplier)**|만일 객체가 널이면, NullPointerException 발생<br>두번째 매개변수는 예외 메시지 문자열이 됨|
|**static int compare(Object a, Object b, Comparator c)**|비교자 Comparator c로 비교했을 때,<br>두 비교대상이 같으면 0, 크면 양수, 작으면 음수 반환|
|**static boolean equals(Object a, Object b)**|Object 클래스에 정의되어있는 것 - Null 검사를 해야함<br>Objects 클래스에 정의되어있는 것 - Null 검사를 하지 않아도 됨<br>A와 B가 모두 Null인 경우 true|
|**static boolean deepEquals(Object a, Object b)**|객체를 재귀적으로 비교 - 다차원 배열 비교 가능(equals는 false)|
|**static String toString(Object o)<br>static String toString(Object o, String nullDefault)**|Object 클래스에 정의되어있는 것 - Null 검사를 해야함<br>Objects 클래스에 정의되어있는 것 - Null 검사를 하지 않아도 됨<br>만약 객체가 Null이라면 두 번째 매개변수가 있다면, 그것을 반환|
|**static int hashCode(Object o)**|Object 클래스에 정의되어있는 것 - Null 검사를 해야함<br>Objects 클래스에 정의되어있는 것 - Null 검사를 하지 않아도 됨<br>만약 객체가 Null이라면 0을 반환|
|**static int hash(Object... values)**|보통은 클래스에 선언된 인스턴스 변수들의 hashCode()를 조합하여 반환하도록 hashCode()를 오버라이딩, 대신 매개변수 타입이 가변인자인 해당 메서드를 사용하면 편리|

<br>
<br>

## java.util.Random 클래스

- Math.random()과의 차이점

  - Random은 종자값(seed)을 설정할 수 있음

  - 종자값이 같은 Random 인스턴스들은 항상 같은 난수를 같은 순서대로 반환

- 종자값

  - 난수를 만드는 공식에 사용되는 값

  - 같은 종자값을 넣으면 같은 결과를 얻음

- 메서드

  - 아래 외에도 스트림(Stream) 관련 메서드들이 있음(JDK 1.8~)

|메서드|뜻|
|---|---|
|**Random()**|System.현재시간을 종자값(seed)으로 이용하는 Random인스턴스를 생성|
|**Random(long seed)**|매개변수 seed를 종자값으로 하는 Random 인스턴스를 생성|
|**boolean nextBoolean()**|boolean타입의 난수 반환|
|**void nextByte(byte\[] byte)**|bytes 배열에 byte 타입의 난수를 채워서 반환|
|**double nextDouble()**|double타입의 난수를 반환 (0.0 \<= x \< 1.0)|
|**float nextFloat()**|float 타입의 난수를 반환 (0.0 \<= x \< 1.0)|
|**double nextGaussian()**|평균은 0.0, 표준 편차는 1.0인 가우시안(Gaussian)분포에 따른 double형의 난수를 반환|
|**int nextInt()**|int 범위 내의 int 타입의 난수를 반환|
|**int nextInt(int n)**|0 \<= x \< n 범위의 int 값을 반환|
|**long nextLong()**|long 범위 내의 long 타입의 난수를 반환|
|**void setSeed(long seed)**|종자값을 주어진 값(seed)로 변경|

- 만들어두면 도움이 될만한 메서드

{% highlight java %}
import java.util.*;

/* 배열 arr을 from과 to 범위의 값들로 채워서 반환 */
public static int[] fillRand(int[] arr, int from, int to) {
    for (int i = 0; i < arr.length, i++) {
        arr[i] = getRand(from, to);
    }    
    return arr;
}

/* 배열 arr을 배열 data에 있는 값들로 채워서 반환 */
public static int[] filRand(int[] arr, int[] data) {
    for(int i = 0; i < arr.length, i++) {
        arr[i] = data[getRand(0, datalength-1)];
    }
    return arr;
}

/* from과 to 범위의 정수(int)값을 반환, from <= int <= to */
public static int getRand(int from, int to) {
    return (int)(Math.random() + (Math.abs(to-from)+1)) + Math.min(from, to);
}
{% endhighlight %}

<br>
<br>

## 정규식(Regular Expression) - java.util.regex 패키지

- 정규식

  - 텍스트 데이터 중 원하는 조건(패턴, pattern)과 일치하는 문자열을 찾아내기 위해 사용하는 것

  - 미리 정의된 기호와 문자를 이용하여 작성한 문자열

  - 원래 Unix에서 사용되던 것, Perl의 강력한 기능이었음

  - Java 등을 비롯한 다양한 언어에서 지원 중

  - 많은 양의 텍스트 파일에 대하여 원하는 데이터를 손쉽게 추출가능

  - 많은 양의 텍스트 파일에 대하여 입력된 데이터가 형식에 맞는지 체크할 수 있음

  - Java API 문서에서 java.util.regex.Pattern을 찾아보면 정규식에 사용되는 기호와 작성방법이 나와있음

- java.util.regex.*; import 필요 (Pattern과 Matcher가 속해있음)

<br>
<br>

## 정규식(Regular Expression) - 정규식을 정의하고 데이터를 비교하는 과정

- 첫째. 정규식을 매개변수로 Parttern 클래스의 static 메서드인 Pattern compile(String regex)을 호출하여 Pattern 인스턴스 획득

  - Pattern p = Pattern.compile("a\[a-z]*");
 
- 둘째. 정규식으로 비교할 대상을 매개변수로 Pattern 클래스의 Matcher matcher(CharSequence input)를 호출해서 Matcher 인스턴스를 얻음
  
  - Matcher m = p.matcher(data\[i]);
  
- 셋째. Matcher 인스턴스에 boolean matches()를 호출해서 정규식에 부합하는지 확인
  
  - if(m.matches());
  
<br>
<br>

## 정규식(Regular Expression) - 그룹화(grouping)

- 정규식의 일부를 괄호로 나누어 묶은 것

- 그룹화된 부분은 하나의 단위로 묶이는 셈이 되어서, 한 번 또는 그 이상의 반복을 의미하는 '+'나 '\*'가 뒤에 오면 그룹화된 부분이 적용 대상이 됨

- 그룹화된 부분은 group(int i)를 통해 나누어 얻을 수 있음

  - group(0) or group() → 그룹으로 매칭된 문자열 전체를 나누어지지 않은 채로 반환

  - group(1), ... , group(int i) → i번째 그룹 호출

  - group(int i)의 i > 실제 그룹수, java.lanv.IndexOufOfBoundsException 발생
  
<br>
<br>

## 정규식(Regular Expression) - find()

- find()  
  
  - 주어진 소스 내에서 패턴과 일치하는 부분을 찾아내면 true, 찾지 못하면 false 반환

  - find()를 호출해서 패턴과 일치한 부분을 찾아낸 후, 다시 find()를 호출하면 이전에 발견한 패턴과 일치하는 부분의 다음부터 다시 패턴매칭 시작

  - while 반복문 사용할 수 있음

- start() end()

  - 위치 확인 가능

- appendReplacement(StringBuffer sb, String replacement)

  - 문자열로 치환 가능

{% highlight java %}
// Pattern과 Matcher가 속한 패키지에 Import
import java.util.regex.*;

class RegularSunny {
    public static void main(String[] args) {
        String source = "Today is Sunny! How weather? It's Sunny!";
        String pattern = "Sunny";
        
        StringBuffer sb = new StringBuffer();
        
        Pattern p = Pattern.compile(pattern);
        Matcher m = p.matcher(source);
        System.out.println("source:" + source);
        
        int i = 0;
        
        while(m.find()) {
            System.out.println(++i + "번째 매칭 : " + m.start() + "~" + m.end());
            // Sunny를 Cloudy로 치환하여 sb에 저장
            m.appendReplacement(sb, "Cloudy");
        }
        
        m.appendTail(sb);
        System.out.println("Replacement count : " + i);
        System.out.println("result : " + sb.toString());
    }
}

/* source:Today is Sunny! How weather? It's Sunny!
 * 1번째 매칭:9~14
 * 2번째 매칭:34~39
 * Replacement count : 2
 * result : Today is Cloudy! How weather? It's Cloudy!
*/

/* 1. 문자열 source에서 "Sunny"를 m.find()로 찾은 후
 *    처음으로 m.appendReplacement(sb, "Cloudy");가 호출되면
 *    source의 시작부터 "Sunny"를 찾은 위치까지의 내용에 "Cloudy"를 더해서 저장함
 *    - sb에 저장된 내용 : "Today is Cloudy"
 *
 * 2. m.find()는 첫 번째로 발견된 위치의 끝에서부터 재검색을 시작하여
 *    두 번째 "Sunny"를 찾게됨. 다시 m.appendReplacement(sb, "Cloudy");가 호출
 *    - sb에 저장된 내용 : "Today is Cloudy! How weather? It's Cloudy"
 *
 * 3. m.appendTail(sb);이 호출되면
 *    마지막으로 치환된 이후의 부분을 sb에 덧붙임
 *    - sb에 저장된 내용 : "Today is Cloudy! How weather? It's Cloudy!"
 */
{% endhighlight %}

<br>
<br>

## 정규식(Regular Expression) - 자주쓰는 패턴

- 큰 따옴표 " " 안에서 escape문자 '\\'를 표현하려면 escape 문자를 '\\\\'와 같이 두 번 사용할 것

- .이나 + 등 표현시 (패턴이 아닐경우) escape 문자를 붙여줄 것

|정규식|패턴	뜻|
|---|---|
|**.**|문자 한개|
|**.+**|문자 1개 이상|
|**.\***|문자 0개 이상|
|**\[a-z]**|영소문자 1개|
|**\[A-Z]**|영대문자 1개|
|**\[a-zA-Z]**|영대/소문자 1개|
|**\[0-9]<br>\\d**|숫자 1개|
|**\[a-zA-Z0-9]<br>\\w**|영대/소문자+숫자 1개|
|**.{n}\<br>ex) .{2}**|문자 n개<br>(ex) 문자 2개|
|**.{n,m}<br>ex) .{1, 2}**|문자 n개 ~ m개<br>(ex) 문자 1개 ~ 2개|
|**\[a\|b].\*<br>\[a-b].\*<br>\[ab].\***|a또는 b로 시작하는 문자열|
|**\[^a\|b].\*<br>\[^a-b].\*<br>\[^ab].\***|a또는 b로 시작하지 않는 문자열|

<br>
<br>

## java.util.Scanner 클래스

- Scanner는 화면, 파일, 문자열과 같은 입력소스로부터 문자데이터를 읽어오는데 도움을 줄 목적(JDK 1.5~)

- 정규식 표현(Regular expression)을 이용한 라인단위의 검색을 지원하고, 구분자(delimiter)에도 정규식 표현을 사용할 수 있어서 복잡한 형태의 구분자도 처리 가능

  - Scanner useDelimiter(Pattern pattern)

  - Scanner useDelimiter(String pattern)

- JDK 1.6 ~ 화면 입출력만 전문적으로 담당하는 java.io.Console 추가

  - 이클립스와 같은 IDE에서 잘 동작하지 않는 문제

  - Scanner와 사용법과 성능측면이 거의 같음(무엇을 써도 상관 없음)

{% highlight java %}
// JDK 1.5 이전
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
String input = br.readLine();

// JDK 1.5 ~ (java.io.Scanner)
Scanner s = new Scanner(System.in);
String input = s.nextLine();

// JDK 1.6 ~ (java.io.Console) - 이클립스에서 동작 안함
Console c = System.console();
String input = c.readLine();
{% endhighlight %}

<br>
<br>

## java.util.Scanner 클래스 - 생성자

- Scanner(String source)

- Scanner(File source)

- Scanner(InputStream Source)

- Scanner(Readable source)

- Scanner(ReadableByteChannel source)

- Scanner(Path source) // JDK 1.7 ~

<br>
<br>

## java.util.Scanner 클래스 - 메서드

- 입력받을 값의 타입 따라 메서드를 제공

|기본형|메서드|
|---|---|
|**boolean**|nextBoolean()|
|**byte**|nextByte()|
|**short**|nextShort()|
|**int**|nextInt()|
|**long**|nextLong()|
|**double**|nextDouble()|
|**float**|nextFloat()|
|**String**|nextLine()|

<br>
<br>

## java.util.Scanner 클래스 - split(String s)

- 화면으로부터 라인단위로 입력받아서 입력받은 내용을 구분자로 나누는 용도

- 정규식 지원

- 예시

  - 하나 이상의 공백(정규식사용)
  
  - arrArr = input.split(" +"); // 입력받은 내용을 공백(하나 이상)을 구분자로 자르기
  
<br>
<br>

## java.util.StringTokenizer 클래스

- 긴 문자열을 지정된 구분자(delimiter)를 기준으로 토큰(token)이라는 여러 개의 문자열로 잘라내는 데 사용됨

  - 단, 구분자로 단 1개의 문자만 받을 수 있음

  - 문자열로 여러 문자를 받는다면 (예) "+-\*/", 1개 문자마다 다 구분자임. "+", "-", "/", "\*"
  
- 예시

  - "100,200,300,400"이라는 문자열을 ',' 구분자로 자르면 "100", "200", "300", "400"이라는 4개의 문자열(토큰)을 얻을 수 있음
  
- 생성자 & 메서드

  |생성자 / 메서드|설명|
  |---|---|
  |**StringTokenizer(String str, String delim)**|문자열(str)을 지정된 구분자(delim)로 나누는 StringTokenizer를 생성.<br>(구분자는 토큰으로 간주되지 않음)|
  |**StringTokenizer(String str, String delim, boolean returnDelims)**|문자열(str)을 지정된 구분자(delim)로 나누는 StringTokenizer를 생성.<br>returnDelims의 값을 true로 하면 구분자도 토큰으로 간주|
  |**int countTokens()**|전체 토큰의 수를 반환|
  |**boolean hasMoreTokens()**|토큰이 남아있는지 확인|
  |**String nextToken()**|다음 토큰을 반환|

- StringTokenizer를 이용하는 방법 이외에도, String의 split(String regex)나 Scanner의 useDelimiter(String pattern)을 사용할 수 있지만 두 가지의 경우 정규식 표현(Regular expression)을 사용 
  
  - StringTokenizer → 정규식사용X

  - 정규식 사용이 익숙하지 않으면 StringTokenizer가 더 간단하고 명확한 결과
  <br>대신, 구분자로 단 1개의 문자밖에 사용하지 못하므로 보다 복잡한 구분자가 필요한 경우엔 다른 방식 이용
  
<br>
<br>

## java.util.StringTokenizer 클래스 - StringTokenizer vs String.split()

- split() : 빈 문자열도 토큰으로 인식

  - 데이터를 토큰으로 잘라낸 결과를 배열에 담아서 반환 → 성능이 조금 느림

- StringTokenizer : 빈 문자열은 토큰으로 인식하지 않음 

  - 데이터를 토큰으로 바로바로 잘라서 반환 → 성능이 조금 빠름

- 데이터 양이 많지 않으면 별 문제가 되지 않을 정도의 성능차이

{% highlight java %}
// String split(String regex) 이용 - 정규식
String[] result = "100,200,300,400".split(",");

// Scanner useDelimiter(String pattern) 이용 - 정규식
Scanner sc = new Scanner("100,200,300,400").useDelimiter(",");

// StringTokenizer 이용 - 정규식X
StringTokenizer st = new StringTokenizer("100,200,300,400", ",");
{% endhighlight %}

<br>
<br>

## java.math.BigInteger 클래스

- 내부적으로 int 배열을 사용해서 값을 다룸

  - final int signum; // 부호. 1(양수), 0, -1(음수) 셋 중 하나

  - final int\[] mag; // 값(magnitude)

- long 타입보다 훨씬 큰 값을 다룰 수 있음

  - 대신 성능은 long보다 떨어짐

  - 최대값 (2진수) ±2^Integer.MAX_VALUE = (10진수) 10의 60억제곱

- 불변(immutable)

  - String과 같이 불변임

- 모든 정수형이 그렇듯 값을 '2의 보수' 형태로 표현

  - 부호를 따로 저장하고 배열에는 값 자체 저장

  - signum 값이 -1(음수)인 경우, 2의 보수법에 맞게 mag의 값을 변환해서 처리

  - 부호만 다른 두 값의 mag는 같고 signum은 다름
  
<br>
<br>

## java.math.BigInteger 클래스 - 생성

- 문자열로 숫자를 표현하는 것이 일반적

- 정수형 리터럴로는 표현할 수 있는 값의 한계가 있기 때문

{% highlight java %}
BigInteger val;

// 문자열
val = new BigInteger("12345678901234567890");

// n진수(radix)의 문자열로 생성
val = new BigInteger("FFFF", 16);

// 숫자로 생성(정수리터럴)
val = BigInteger.valueOf(1234567890L);
{% endhighlight %}

<br>
<br>

## java.math.BigInteger 클래스 - 다른 타입으로의 변환

- 문자열 또는 byte 배열로 변환하는 메서드

|메서드|의미|
|---|---|
|**String toString()**|문자열로 변환|
|**String toString(int radix)**|지정된 진법(radix)의 문자열로 변환|
|**byte\[] toByteArray()**|byte배열로 변환|

- Number부터 상속받은 기본형으로 변환하는 메서드 1

|기본형|메서드|
|---|---|
|**int**|intValue()|
|**long**|longValue()|
|**float**|floatValue()|
|**double**|doubleValue()|

- Number부터 상속받은 기본형으로 변환하는 메서드 2
<br>: Exact가 붙은 메소드는 변환한 결과가 변환한 타입의 범위에 속하지 않을 경우 ArthmeticException 발생

|기본형|메서드|
|---|---|
|**byte**|byteValueExact()|
|**short**|shortValueExact()|
|**int**|intValueExact()|
|**long**|longValueExact()|

<br>
<br>

## java.math.BigInteger 클래스 - 사칙 연산과 나머지

- 정수형에 사용할 수 있는 모든 연산자와 수학적인 계산을 쉽게 해주는 메서드들이 정의되어 있음

- BigInteger는 불변이므로, 반환 타입이 BigInteger란 얘기는 새로운 인스턴스가 반환된다는 뜻

- 기본적인 연산을 수행하는 메서드 몇 개 (예시)

  - Java API를 보면, 메서드마다 연산기호가 적혀있기 때문에 각 메서드가 어떤 연산자를 구현한 것인지 쉽게 알 수 있음

|메서드|의미|
|---|---|
|**BigInteger add(BigInteger val)**|덧셈, this + val|
|**BigInteger subtract(BigInteger val)**|뺼셈, this - val|
|**BigInteger multiply(BigInteger val)**|곱셈, this * val|
|**BigInteger divide(BigInteger val)**|나눗셈, this / val|
|**BigInteger remainder(BigInteger val)**|나머지, this % val|
|**BigInteger mod(BigInteger val)**|나머지, this % val, 나누는 값이 음수일 경우 ArithmeticException 발생|

<br>
<br>

## java.math.BigInteger 클래스 - 비트연산 메서드

- 워낙 큰 숫자를 다루기 때문에, 성능을 향상시키기 위해 비트단위로 연산을 수행하는 메서드들을 많이 가지고 있음

- 가능한 산술연산 대신 비트연산으로 처리하도록 노력

- and, or, not과 같이 비트 연산자를 구현한 메서드들을 제공

- 다음과 같은 메서드들도 제공

|메서드|의미|
|---|---|
|**int bitCount()**|2진수로 표현했을 때, 1의 개수(음수는 0의 개수)를 반환|
|**int bitLength()**|2진수로 표현했을 때, 값을 표현하는데 필요한 bit수|
|**boolean testBit(int n)**|우측에서 n+1번째 비트가 1이면 true, 0이면 false 반환|
|**BigInteger setBit(int n)**|우측에서 n+1번째 비트를 1로 변경|
|**BigInteger clearBit(int n)**|우측에서 n+1번째 비트를 0으로 변경|
|**BigInteger flipBit(int n)**|우측에서 b+1번째 비트를 전환(1→0, 0→1)|

<br>
<br>

## java.math.BigDecimal 클래스

- 실수형과 달리 정수를 이용하여 실수를 표현 

  - double타입은 표현할 수 있는 값은 상당히 범위가 넓지만, 정밀도가 최대 13자리 & 실수형 특성상 오차를 피할 수 없음

  - 실수의 오차 : 10진 실수를 2진 실수로 정확히 변환할 수 없는 경우가 있기 때문에 발생

  - 오차가 없는 2진 정수로 변환하여 다루기

  - 실수를 정수와 10의 제곱의 곱으로 표현

- 정수 * 10^(-scale)

  - 0 \<= scale \<= Integer.MAX_VALUE

  - 정수를 저장하는데 BigInteger를 사용

- 불변(immutable)

  - private final BigInteger intVal; // 정수(unscaled value)

  - private final int sacle; // 지수(scale)

  - private transient int precision; // 정밀도(precision) - 정수의 자리수

- 예제) 543.21

  - 543.21 = 54321 \* 10^-2

  - BigDecimal val = new BigDecimal("543.21"); // 54321*10^-2

  - System.out.println(val.unscaledValue()); // 54321

  - System.out.println(val.scale()); // 2

  - System.out.println(val.precision()); // 5 - 정수의 전체 자리수
  
<br>
<br>

## java.math.BigDecimal 클래스 - 생성

{% highlight java %}
BigDecimal val;

// 문자열로 생성
val = new BigDecimal("987.654321");

// double 타입의 리터럴로 생성
val = new BigDecimal(987.654);

// int, long 타입의 리터럴로 생성 가능
val = new BigDecimal(987654);

// 생성자 대신 valueOf(double) 사용
val = BigDecimal.valueOf(123.456);

// 생성자 대신 valueOf(int) 사용
val = BigDecimal.valueOf(123456);

/* 주의
 * double 타입의 값을 매개변수로 갖는 생성자를 사용하면
 * 오차가 발생할 수 있음
 */
// 0.10000000000000000555111...
System.out.println(new BigDecimal(0.1);
// 0.1 출력
System.out.println(new BigDecimal("0.1");
{% endhighlight %}

<br>
<br>

## java.math.BigDecimal 클래스 - 다른 타입으로 변환

- 문자열로 변환하는 메서드

|메서드|의미|
|---|---|
|**String toPlainString()**<br>ex) 0.000000000000000000000010..........|어떤 경우에도 다른 기호 없이 숫자로만 표현|
|**String toString()**<br>ex) 1.000000000000000048...5E-22|필요하면 지수형태로 표현할 수도 있음|

- Number부터 상속받은 기본형으로 변환하는 메서드 1

|기본형|메서드|
|---|---|
|**int**|intValue()|
|**long**|longValue()|
|**float**|floatValue()|
|**double**|doubleValue()|

- Number부터 상속받은 기본형으로 변환하는 메서드 2
<br>: Exact가 붙은 메소드는 변환한 결과가 변환한 타입의 범위에 속하지 않을 경우 ArthmeticException 발생

|기본형|메서드|
|---|---|
|**byte**|byteValueExact()|
|**short**|shortValueExact()|
|**int**|intValueExact()|
|**long**|longValueExact()|
|**BigInteger**|toBigIntegerExact()|

<br>
<br>

## java.math.BigDecimal 클래스 - 연산

- BigDecimal에는 실수형에 사용할 수 있는 모든 연산자와 수학적인 계산을 쉽게 해주는 메서드들이 정의되어 있음

- BigDecimal는 불변이므로, 반환 타입이 BigDecimal이란 얘기는 새로운 인스턴스가 반환된다는 뜻

- 기본적인 연산을 수행하는 메서드 몇 개 (예시)

  - Java API를 보면, 메서드마다 연산기호가 적혀있기 때문에 각 메서드가 어떤 연산자를 구현한 것인지 쉽게 알 수 있음

  - 연산시 연산 결과의 정수, 지수, 정밀도가 달라짐

|메서드|의미|
|**BigDecimal add(BigDecimal val)**|덧셈, this + val|
|**BigDecimal subtract(BigDecimal val)**|뺼셈, this - val|
|**BigDecimal multiply(BigDecimal val)**|곱셈, this * val|
|**BigDecimal divide(BigDecimal val)**|나눗셈, this / val|
|**BigDecimal remainder(BigDecimal val)**|나머지, this % val|

- 연산 결과의 정밀도 차이(예시)

  - 곱셈 = 두 피연산자의 scale을 더함

  - 나눗셈 = 두 피연산자의 scale을 뺌

  - 덧셈 뺄셈 = 둘 중 자리수가 높은 쪽으로 맞춤(큰 scale이 결과)

|예시|value|scale|precision|
|---|---|---|---|
|**BigDecimal bd1 = new BigDecimal("123.456");**|123456|3|6|
|**BigDecimal bd2 = new BigDecimal("1.0");**|10|1|2|
|**BigDecimal bd3 = bd1.multiply(bd2);**|1234560|4|7|

<br>
<br>

## java.math.BigDecimal 클래스 - 반올림모드 : devide()와 setScale()

- 다른 연산과 달리 나눗셈을 처리하기 위한 메서드가 다양한 버전이 존재

- 나눗셈의 결과를 어떻게 반올림(rounding Mode)할 것인가, 몇 번째 자리(scale)에서 반올림할 것인지를 지정할 수 있음

- BigDecimal은 오차 없이 실수를 저장하지만, 나눗셈에서 발생하는 오차는 어쩔 수 없음

- 나눗셈의 다양한 버전

  - BigDecimal divide(BigDecimal divisor)

  - BigDecimal divide(BigDecimal divisor, int roundingMode)

  - BigDecimal divide(BigDecimal divisor, RoundingMode roundingMode)

  - BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)

  - BigDecimal divide(BigDecimal divisor, int scale, RoundingMode roundingMode)

  - BigDecimal divide(BigDecimal divisor, MathContext mc)
  
- roundingMode : 반올림 처리방법

  - BigDecimal에 정의된 ROUND_로 시작하는 상수 중 하나를 선택하여 사용
  
- RoundingMode

  - BigDecimal에 정의된 ROUND_로 시작하는 상수들을 열거형으로 정의한 것

  - 가능한 열거형 RoundingMode를 사용할 것

  - 사용 예) RoundingMode.CEILING
  
|상수|설명|
|---|---|
|CEILING|올림|
|FLOOR|내림(CEILING과 반대)|
|UP|양수일 때는 올림, 음수일 때는 내림|
|DOWN|양수일 떄는 내림, 음수일 때는 올림(UP과 반대)|
|HALF_UP|반올림(5이상 올림, 5미만 버림)|
|HALF_EVEN|반올림(반올림 자리의 값이 짝수면 HALF_DOWN, 홀수면 HALF_UP)|
|HALF_DOWN|반올림(6이상 올림, 6미만 버림)|
|UNNECESSARY|나눗셈의 결과가 딱 떨어지는 수가 아니면, ArithmeticException 발생|

> (주의) 1.0/3.0처럼 divide()로 나눗셈한 결과가 무한소수인 경우,
>
> 반올림 모드를 지정하지 않을 경우 ArithmeticException 발생

<br>
<br>

## java.math.BigDecimal 클래스 - java.math.MathContext

- 반올림 모드와 정밀도(precision)을 하나로 묶어놓은 것

- 별다른 것은 없음

- (주의) devide()에서는 scale이 소수점 이하의 자리수를 의미, MathContext에서는 precision이 정수와 소수점 이하를 모두 포함한 자리수를 의미

{% highlight java %}
BigDecimal bd1 = new BigDecimal("123.456");
BigDecimal bd2 = new BigDecimal("1.0");

// divide(), 123.46
System.out.println(bd1.divide(bd2, 2, HALF_UP));

// divide(), MathContext 사용, 1.2E+2
// new MathContext는 precision을 가지고 반올림, scale 2니까 나눗셈 결과가 소수점 두자리까지 출력
// 그래서 precision은 12000이 아닌 12가 되고, 거기에 scale 반영이 되므로 1.2E+2
SYstem.out.println(bd1.divide(bd2, new MathContext(2, HALF_UP)));
{% endhighlight %}

<br>
<br>

## java.math.BigDecimal 클래스 - scale의 변경

- BigDecimal을 10으로 곱하거나 나누는 대신, scale 값을 변경하면 같은 결과를 얻을 수 있음

- scale을 변경하려면 setScale() 이용

  - BigDecimal setScale(int newScale)

  - BigDecimal setScale(int newScale, int roundingMode)
  
  - BigDecimal setScale(int newScale, RoundingMode roundingMode)

- setScale()로 scale 값을 줄이는 것은 10의 n제곱으로 나누는 것과 같으므로 divide()를 호출할 때 처럼 오차 발생 가능 → 반올림 모드를 지정해줘야 함
