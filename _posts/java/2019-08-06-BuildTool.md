---
layout: post
title:  "[Java] BuildTool - Ant(+Ivy) vs Maven vs Gradle"
date:   2019-08-06 18:52:21
author: Choi HyeSun
categories: java
tags:
  - Java
  - BuildTool
  - 자바빌드도구
  - 빌드도구
  - Ant
  - Ivy
  - Maven
  - Gradle
---

## Build

- 컴파일 + 테스팅 + 점검 + 배포

  - 컴파일 : 소스코드를 사용할 수 있는 실행 파일로 변환하는 것

- 소스의 컴파일을 포함해 Application을 사용할 수 있는 형태로 만들어주는 과정

- 소프트웨어가 하나의 단위로써 잘 동작하는지 확인하는 과정

<br>
<br>

## BuildTool(Build Automation)

- Build Tool : Build를 쉽게 하기 위한 Tool

- Build Automation : 소프트웨어 개발자가 일상 활동에서 수행하는 다양한 작업을 스크립팅하거나 자동화하는 작업이며 아래 일들이 포함되어 있음

  - 소스 코드를 바이너리 코드로 컴파일

  - 바이너리 코드의 패키징

  - 테스트 실행

  - 프로덕션 환경에 배포

  - 문서 및 릴리즈 노트 작성
  
- 초기에 Make가 있었음(아직도 있음)

  - Make는 유일한 빌드 자동화도구였고, C프로그램에 맞춰져있기 때문에 Java 생태계에 맞지 않았음

  - 그래서 Java Build Tool들이 출시되었음
  
<br>
<br>

## Java Build Tool이 없다면?

- IDE(이클립스/인텔리제이 등)에서 컴파일된 클래스 파일을 FTP를 이용하여 업로드해야 함

- 단점

  - FTP 접속, 파일 업로드 등 단순한 일련의 작업 반복으로 인해 리소스 낭비

  - 잘못된 경로로의 업로드 등 휴먼 에러 발생 가능
  
<br>
<br>

## Ant (2000 ~ ) && Ivy (2004 ~ )

- Apache Ant("Another Neat Tool: 또 다른 능숙한 툴)

- 특징

  - Java Application의 빌드 프로세스를 자동화하는데 사용되는 Java 라이브러리

  - Java Application이 아니여도 ant를 사용할 수 있음

  - 처음에는 Tomat 코드베이스의 일부였지만 2000년에 독립 프로젝트로 출시됨

  - make와 유사하며, 매우 간단해서 누구나 특별한 전제조건 없이 사용할 수 있음

  - 빌드파일은 XML로 작성되고, 이것을 build.xml이라고 칭함

  - 빌드 프로세스의 다른 단계를 targets라고 칭함
  
- 예제

  - HellowWorld 메인 클래스를 사용하는 간단한 Java project를 위한 build.xml 파일

  - 아래 build.xml 파일은 4가지 targets를 정의 : clean, compile, jar, run
  
{% highlight html %}
<project>
    <target name="clean">
        <delete dir="classes" />
    </target>
 
    <target name="compile" depends="clean">
        <mkdir dir="classes" />
        <javac srcdir="src" destdir="classes" />
    </target>
 
    <target name="jar" depends="compile">
        <mkdir dir="jar" />
        <jar destfile="jar/HelloWorld.jar" basedir="classes">
            <manifest>
                <attribute name="Main-Class"
                  value="antExample.HelloWorld" />
            </manifest>
        </jar>
    </target>
 
    <target name="run" depends="jar">
        <java jar="jar/HelloWorld.jar" fork="true" />
    </target>
</project>

<!-- 실행하기 (예)-->
ant clean
ant compile
ant jar
ant run
{% endhighlight %}

- 장점

  - 유연성 : 어떤 코딩 규칙이나 프로젝트 구조도 강요하지 않음<br>개발자들이 스스로 모든 명령을 작성하도록 요구한다는 것을 의미
  
  - Ivy (Ant 하위) 프로젝트를 통한 종속성 관리
  
- 단점

  - (장점 유연성에서 이어온) 때때로 유지보수하기 어려운 거대한 XML 빌드파일

  - 규칙이 없기때문에, 다른 사람이 작성한 Ant 빌드 파일을 이해하는데 시간이 더 소요됨
  
  - 종속성 관리를 지원하지 않았었음, 추후 Ivy 프로젝트 개발 (종속성 관리 - Ant의 하위 프로젝트)<br>Ivy 프로젝트 개발 전 Maven이 출시됨(종속성 관리를 위하여)
  
<br>
<br>

## Maven (2002 ~ )

- 특징

  - 주로 Java Application에서 사용되는 종속성 관리 및 빌드 자동화 도구

  - ant처럼 XML 파일을 계속 사용하지만 훨씬 관리가 용이함

  - 규칙을 사용하고 미리 정의된 명령(목표)을 제공함

  - 빌드와 종속성 관리 지침이 포함된 메이븐 구성 파일의 경우 pom.xml이라는 규칙에 의해 결정

  - 모든 작업이 플러그인에 의해 수행되므로 플러그인 실행 프레임워크로 간주될 수 있음

- 예제

  - HelloWorld 기본 클래스가 포함된 동일한 간단한 Java 프로젝트에 대한 pom.xml 파일

{% highlight html %}
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
      http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>baeldung</groupId>
    <artifactId>mavenExample</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <description>Maven example</description>
 
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>

<!-- 프로젝트 규조 표준화, Maven 규칙 준수-->
+---src
|   +---main
|   |   +---java
|   |   |   \---com
|   |   |       \---baeldung
|   |   |           \---maven
|   |   |                   HelloWorld.java
|   |   |                   
|   |   \---resources
|   \---test
|       +---java
|       \---resources

<!-- 실행하기 (예)-->
mvn compile
{% endhighlight %}

- 장점

  - 빌드가 수행하는 작업에 집중할 수 있으며 이를 수행할 프레임워크를 제공할 수 있음

  - 의존성 관리를 기본적으로 지원함
  
  - 빌드 프로세스에서 각 단계를 Ant와 달리 수동으로 정의할 필요가 없음 - Maven의 내장 명령 호출 가능<br>(예) compile, test, package, install, desploy 등
  
  - 사용 가능한 광범위한 플러그인을 지원하며, 각 플러그인을 추가로 구성할 수 있음
  
  - 표준화되어 있지만 Maven 구성 파일은 여전히 크고 번거로움
  
- 단점

  - 엄격한 프로젝트 구조를 규정 - 유연성이 Ant에 비해 떨어짐

  - 사용자 정의 빌드 스크립트 작성은 Ant에 비해 훨씬 더 어려움

<br>
<br>

## Gradle (2012 ~ )

- 특징

  - Ant 및 Maven의 개념을 기반으로 구축된 종속성 관리 및 빌드 자동화 도구

  - Ant 또는 Maven과 달리 XML 파일을 사용하지 않음

  - 특정 도메인에 맞는 언어를 사용하여 특정 도메인의 문제를 해결<br>Groovy를 기반으로 하는 DSL(Domain-Specific Language)을 사용

  - 언어가 특정 도메인 문제를 해결하도록 설계되었기 때문에 구성 파일의 크기가 작아짐<br>구성파일 : build.gradle

  - 의도적으로 기능을 거의 제공하지 않고 플러그인으로 모든 유용한 기능을 추가
  
  - Ant의 targets, Maven의 phases처럼 gradle의 빌드단계는 tasks
  
- 예제

{% highlight html %}
apply plugin: 'java'
 
repositories {
    mavenCentral()
}
 
jar {
    baseName = 'gradleExample'
    version = '0.0.1-SNAPSHOT'
}
 
dependencies {
    compile 'junit:junit:4.12'
}

// 다음을 실행하여 컴파일
gradle classess
{% endhighlight %}

- 장점

  - 구성 파일의 크기가 작아짐

  - 플러그인 사용 가능

  - 유연함
  
<br>
<br>

## 그 외 Build Tool도 있음

<br>
<br>

## 출처

[baeldung](https://www.baeldung.com/ant-maven-gradle)
