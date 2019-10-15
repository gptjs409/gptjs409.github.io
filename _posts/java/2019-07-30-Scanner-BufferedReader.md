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

<br>
<br>

## java.io.BufferedReader

- 문자 입력 스트림에서 텍스트를 읽는 클래스

- 문자의 순서(시퀀스)를 효율적으로 읽기 위하여 문자를 버퍼링


<br>

설명

- Buffer
  
  - 데이터를 한 곳에서 다른 곳으로 전송하기 전 일시적으로 그 데이터를 보관하는 임시 메모리 영역
  
  - 입출력 속도 향상을 위해 버퍼 사용(속도 차이가 날 때, 묶어서 보내는 것이 더 효율적)
  
- Stream

  - 데이터의 흐름
  
- BufferedReader : 문자 버퍼 입력, 라인(줄) 해석

  - InputStreamReader : 바이트 스트림을 문자 스트림으로 변환
  
- BufferedWriter : 문자 스트림에 버퍼 출력, 줄바꿈 사용

  - OutputStreamWriter : 문자 스트림을 바이트 스트림으로 변환
  
- BufferedWriter.flush : 버퍼 비우기

<br>

사용방법

{% highlight java %}
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.BufferedWriter;
import java.io.OutputStreamWriter;

public class Sunny {
    public static void main(String[] args) {
        // BufferedReader Object br 생성, System에서 입력한 값을 받음
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        // BufferedWriter Object bw 생성, System에 보낼 값을 받음
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        
        try {
            // 시스템에서 입력받은 값(br)을 bw로 보내시오
            bw.write(br.readLine());
            // br 닫기
            br.close();
            // bw에 쌓인 것 내보내기
            bw.flush();
            // bw 닫기
            bw.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
{% endhighlight %}

<br>
<br>

## 차이점

- BufferedReader는 동기식

  - BufferedReader는 Scanner보다 훨씬 큰 버퍼 메모리를 가지고 있음
  
- 스캐너 : 1KB char 버퍼, BuffredReader : 8KB byte 버퍼

  - Scanner는 입력 데이터를 파싱, BufferedReader는 문자 시퀀스를 읽음

- BuffreredReader는 스캐너에 비해 조금 빠름
