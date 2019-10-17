---
layout: post
title:  "[Java] 예외처리(Exception Handling)"
date:   2019-09-06 22:10:40
author: Choi HyeSun
categories: java
tags:
  - Java
  - 예외
  - 예외처리
  - Exception
  - Exception Handling
---

## 프로그램 오류

- 오류(또는 에러)

  - 프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우

- 발생 시점에 따른 분류

  - 컴파일 에러 : 컴파일시 발생

  - 런타임 에러 : 실행시 발생

  - 논리적 에러 : 실행은 되지만, 의도와 다르게 동작

- 런타임 에러 구분

  - 에러(error) : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
  <br>예) 메모리 부족(OutOfMemoryError), 스택오버플로우(StackOverflowError)

  - 예외(exception) : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

<br>
<br>

## 예외 클래스의 계층 구조

- 자바에서는 실행 시 발생할 수 있는 오류(Exception과 Error)를 클래스로 정의

- Exception클래스와 Error클래스도 맨 위 부모(시조)클래스는 Object

- 모든 예외의 최고 조상은 Exception 클래스

<br>
<br>

## 예외를 두 분류로 나누면

![image](/img/2019-09-06/exception-handling-001-exception1.png)

- Exception 클래스들 : Exception 클래스와 그 자식 클래스들(하늘색 부분)

  - 사용자의 실수와 같은 외적인 요인에 의해 발생

  - 프로그램 사용자들의 동작에 의해서 발생

  - 예시
  <br>- FileNotFoundException : 존재하지 않은 파일의 이름을 작성
  <br>- ClassNotFoundException : 클래스명을 잘못적음
  <br>- DataFormatException : 데이터 형식이 잘못됨

- RuntimeException 클래스들 : RuntimeException 클래스와 그 자식 클래스들(주황색 부분)

  - 프로그래머의 실수에 의해서 발생될 수 있는 예외

  - 자바의 프로그래밍 요소와 관계가 깊음

  - 예시
  <br>- IndexOutOfBoundsException : 배열의 범위를 벗어남
  <br>- NullPointerException : 값이 null인 참조변수의 멤버를 호출하려 함
  <br>- ClassCastException : 클래스간의 형변환을 잘못함
  <br>- ArithmeticException : 정수를 0으로 나누려 함

<br>
<br>

## 예외 처리(Exception Handling)하기 : try - catch문

- 정의 : 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것

- 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행 상태를 유지하는 것

- 발생한 예외를 처리하지 못한다면

  - 프로그램 비정상 종료

  - 처리하지 못한 예외(uncaught exception)는 JVM의 예외처리기(UncaughtExceptionHandler)가 받아서 예외의 원인을 화면으로 출력

- 예외 처리 방법

  - try catch문 사용

  - 중첩 try catch도 사용 가능 (try블록, catch 블록)

- (주의) try 블럭과 catch 블럭은 { } 괄호를 생략할 수 없음

{% highlight java %}
/* try catch문 */
try {
    // 예외가 발생할 가능성이 있는 문장
} catch (Exception1 e1) {
    // Exception1 발생시, 이를 처리하기 위한 문장
} catch (Exception2 e2) {
    // Exception2 발생시, 이를 처리하기 위한 문장
} catch (ExceptionN eN) {
    // ExceptionN 발생시, 이를 처리하기 위한 문장
}

/* 중첩 try catch문 */
try {
    // 예외가 발생할 가능성이 있는 문장
    try {
        // 예외가 발생할 가능성이 있는 문장
    } catch (Exception e) {
        // Exception 발생시, 이를 처리하기 위한 문장
    }
} catch (Exception e) {
    // Exception 발생시, 이를 처리하기 위한 문장
    try {
        // 예외가 발생할 가능성이 있는 문장
    } catch (Exception e1) { // 주의 위에 파라미터를 참조변수 e로 받으므로, 참조변수가 겹치지 않게끔
        // Exception 발생시, 이를 처리하기 위한 문장
    }
}
{% endhighlight %}

- 흐름1. try 블럭에서 예외가 발생한 경우

  - 발생한 예외와 일치하는 catch블럭이 있는지 확인
  
  - 일치하는 catch블럭을 찾게되면, 그 catch블럭 내의 문장들을 수행 → 전체 try-catch문을 빠져나가서 다음 문장 수행

  - 일치하는 catch블럭을 찾지 못하면, 예외는 처리되지 못함(프로그램 중단)

- 흐름2. try 블럭에서 예외가 발생하지 않은 경우
  
  - catch 블럭을 거치지 않고 전체 try-catch문을 빠져나가서 다음 문장 수행
  
- catch 블럭

  - 괄호 ( ) : 처리하고자 하는 예외와 같은 타입의 참조변수 하나만 선언해야 함
  
  - 블럭 { } : 괄호에 정의된 예외가 발생했을 때 처리할 부분
  
- 멀티 catch블럭(JDK 1.7 ~)

  - 여러 catch 블럭을 '\|' 기호를 이용해서, 하나의 catch 블럭으로 합치는 것

  - catch블럭에서 사용되는 '\|'는 연산자가 아닌 그냥 기호로 인식
  <br>에) catch (ExceptionA \| ExceptionB e) { }
  
  - 중복된 코드를 줄일 수 있음
  
  - '\|' 기호를 통해 연결할 수 있는 예외 클래스의 개수에는 제한이 없음
  
  - 연결된 예외 클래스가 부모&자식 클래스의 관계이면 컴파일 에러 발생
  <br>→ 부모 클래스만 써주는 것과 같기 때문 (불필요한 코드 제거)

  - 제약조건
  <br>
  <br>멀티 catch 블럭으로 여러 예외를 한 번에 처리할 경우, 참조변수(e)의 경우 예외 클래스들의 공통 분모인 부모클래스에 선언된 멤버만 사용 가능
  <br> - 어떤 예외가 발생한 것인지 알 수 없기 때문
  <br>
  <br>멀티 catch블럭에 선언된 참조변수 e는 상수이므로 값을 변경할 수 없음
  <br> - 여러 catch블럭이 하나의 참조변수를 공유하기 때문
  
<br>
<br>

## 예외 발생

- 예외 발생

  - 예외가 발생하면, 발생한 예외에 해당하는 클래스의 인스턴스가 생성

  - 예외가 발생한 문장이 try 블럭에 있다면, 이 예외를 처리할 수 있는 catch 블럭이 있는지 찾음

  - 첫 번째 catch 블럭에서부터 차례로 내려가면서 catch 블럭의 괄호 ( ) 내에 선언된 참조변수의 종류와 생성된 예외 클래스의 인스턴스에 instanceof 연산자를 이용하여 검사
  <br>→ true를 찾을 때 까지
  <br>
  <br>찾으면 : 해당 catch 블럭의 문장을 모두 수행한 후 try-catch 탈출
  <br>못찾으면 : 예외처리되지 않음
  
  - 모든 예외 클래스는 Exception 클래스의 자식클래스이므로, catch 블럭 괄호( )에 Exception 클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 해당 catch 블럭에서 처리됨

- 예외의 발생 원인 찾기

  - printStackTrace() : 예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력

  - getMessage() : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있음

- 예제

{% highlight java %}
// 호출시 참조변수.printStackTrace(), 참조변수.getMessage()
class Sunny {
	public static void main(String[] args) {
        System.out.println(A);
        try {
            System.out.println(B);
            System.out.println(0/0);   // 예외 발생 → to catch
            System.out.println(C);     // 실행되지 않음
        } catch (ArithmeticException ae) {
            ae.printStackTrace();
            System.out.println(ae.getMessage());
        }
        System.out.println(D);
    }
}

/* A
 * B
 * java.lang.ArithmeticException : by zero
 *       at ExceptionSunny.main(ExceptionSunny.java:7)  // printStackTrace();
 * / by zero   // getMessage();
 * D
 */
{% endhighlight %}

<br>
<br>

## 예외 발생시키기

- 키워드 throw를 이용하여 고의로 예외를 발생시킬 수 있음

  - Exception인스턴스를 생성할 때, 생성자에 String을 넣어주면, Exception 인스턴스의 메시지로 저장됨
  <br>→ getMessage()를 통해 얻을 수 있음
  
-  방법

  - 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체 생성
  <br>Exception e = new Exception("고의 예외");
  
  - 키워드 throw를 이용해서 예외를 발생
  <br>throw e;
  
  - 두 개를 한 줄로 쓸 수도 있음
  <br>throw new Exception("고의 예외");


{% highlight java %}

{% endhighlight %}

