---
layout: post
title:  "[Java] 재귀함수"
date:   2019-08-01 08:37:23
author: Choi HyeSun
categories: java
tags:
  - Java
  - 재귀함수
  - 팩토리얼
  - 자바팩토리얼
  - 자바재귀함수
---

## 재귀함수

- 함수 내에서 자기 자신(함수)를 특정 조건이 될 때까지 계속적으로 호출하여 풀어가는 방식

  - 특정 조건 (if 조건문)이 들어 있고, 끝나는 조건이 파라미터로 들어감

  - return에서 자기 자신을 호출하거나, return은 void로 두고 if문에서 자기 자신을 호출하거나

- Stack이라고 생각할 수 있음

  - 함수를 호출하면서 Stack에 차곡차곡 쌓이게 됨

- 장점 : 코드가 간결, 오류 수정이 용이

- 단점 : 코드를 사람이 이해하기 어려움, 기억 공간을 많이 차지함

<br>
<br>

## 재귀함수 적용 비교(팩토리얼)

재귀함수 미적용

{% highlight java %}
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
    public static void main(String[] args) {
        new Main().test();
    }
    
    public void test() {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        
        try {
            int a = Integer.parseInt(br.readLine());
            br.close();
            bw.write(factorial(a)+"");
            bw.flush();
            bw.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    public int factorial (int num) {
        int total = num;
        for (int i = 1; i < num ; i++) {
            total *= num-i;
        }
        if (total == 0) {
            total++;
        }
        return total;
    }
}
{% endhighlight %}

<br>

재귀함수 적용

{% highlight java %}
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

public class Main {
    public static void main(String[] args) {
        new Main().test();
    }
    
    public void test() {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
        
        try {
            int a = Integer.parseInt(br.readLine());
            br.close();
            bw.write(factorial(a)+"");
            bw.flush();
            bw.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    public int factorial (int num) {
        if (num > 1) { 
            return factorial(num-1) * num;
        } else if (num == 1) {
            return num;
        } else {
            return 1;
        }
    }
}
{% endhighlight %}

<br>
<br>

## 재귀함수의 이해

- 호출 흐름
![image](/img/2019-08-01/Recursive-001-flow1.png)

- stack에 넣는다고 한 것 같은 이유 (나중에 들어온게 먼저 나감)
![image](/img/2019-08-01/Recursive-002-queue1.png)
