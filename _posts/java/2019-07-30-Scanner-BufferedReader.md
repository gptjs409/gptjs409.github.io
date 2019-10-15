---
layout: post
title:  "[Java] Scanner vs BufferedReader"
date:   2019-07-30 12:43:27
author: Choi HyeSun
categories: java
tags:
  - Scanner
  - BufferedReader
  - Java
---

출처 [LINK](https://www.geeksforgeeks.org/difference-between-scanner-and-bufferreader-class-in-java/)

## java.util.Scanner

- 기본형과 문자열(String)을 분석할 수 있는 단순한 텍스트 스캐너 클래스

- 다른 타입들(기본형인지 문자형인지)을 읽기 위해서 정규표현식을 내부적으로 사용함

- 다음 메소드 제공 : nextLine(), nextInt(), nextFloat(), nextByte(), nextShort(), nextDouble(), nextLong(), next()

- nextLine() 메소드를 nextLine()을 제외한 nextXXX() 메소드 후 호출시 Scanner 클래스에서 nextLine()이 콘솔에서 값을 읽지 않고 커서가 콘솔에 들어가지 않게 됨

  - nextXXX()와 nextLine() 사이에 nextLine() 메서드를 한 번 더 호출하면 nextLine()이 줄 바꿈 문자를 사용하기 때문에 문제가 발생하지 않음
  
  - nextXXX() 메소드는 줄 바꿈 문자를 무시하고 nextLine()은 첫 번째 줄 바꿈 문자까지 읽음

- 사용법

{% highlight java %}
// import는 여기에다가
import java.util.Scanner;

public class Sunny {
    public static void main(String[] args) {
        // Scanner Object sc 생성
        Scanner sc = new Scanner(System.in);
        // Scanner sc의 nextInt() 사용
        int a = sc.nextInt();
        // nextXXX() 뒤의 nextLine()사용을 위해 넣어줌
        sc.nextLine();
        String b = sc.nextLine();
        System.out.println("a = " + a + ", b = " + b); 
    }
}
{% endhighlight %}


## java.io.BufferedReader

