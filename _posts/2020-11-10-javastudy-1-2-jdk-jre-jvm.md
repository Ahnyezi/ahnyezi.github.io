---
title:  "[Java] JDK(개발도구), JRE(실행환경), JVM(가상머신)"
date: 2020-11-10
categories: ['Java']
tags: ['Java']
---

**자바언어의 특징인 WORA와 자바로 작성된 프로그램을 뒷바침하는 도구들(JDK,JRE,JVM)을 살펴보자.** :raising_hand:<br>
<br>


### 1. 자바의 WORA(Write Once Run Anywhere)

C언어의 특징을 먼저 살펴보자.
C언어로 작성된 프로그램은 플랫폼에 종속적이다. <br>

예를 들어, `Intel CPU + Windows OS` 환경에서 작성한 C 프로그램은 해당 환경에서만 실행이 가능하다. 즉, `Intel CPU + MAC OS` 또는 `AMD CPU + Windows OS`등의 환경에서는 실행되지 않는다는 것이다.<BR>
<br>

**C로 작성된 프로그램이 호환성이 없는 이유는 다음과 같다.**<BR>

- **CPU(Central Processing Unit, 중앙처리장치)마다 기계어가 다름**
   - 기계어는 Java나 Python처럼 통일된 문법을 가진 언어가 아니다. 
   - CPU 제조사마다 자사의 CPU가 이해할 수 있는 기계어의 규칙이 다르다.
   - 즉 다양한 CPU환경에서 프로그램 A를 실행하기 위해서, 각 CPU 제조사의 기계어로 번역 가능한 A를 개발해야 한다. 
- **OS마다 API가 다름**
- **OS마다 실행파일 형식이 다름**<br>
<br>


**카카오톡**으로 예를 들어보자.
유저가 생각하는 카카오톡은 1개이다. 하지만 다양한 CPU와 OS가 있고, 각 환경에서 실행 가능한 카카오톡 프로그램이 필요하다. 실행환경이 총 10개라면, 10개의 서로 다른 카카오톡 프로그램을 개발해야 하는 것이다. <BR>
<BR>

이와 같이 프로그램이 플랫폼에 종속적이라는 문제를 해결하며 등장한 언어가 **자바**이다.
자바는 `JVM`와 `Byte code`를 이용하여, 자바로 작성한 프로그램이 어떤 하드웨어나 운영체제에서도 동작할 수 있게 하였다(플랫폼 독립적). 이러한 자바언어의 특징을 WORA(Write Once Run Anywhere, 한번 작성하면 어디서든 동작한다)라 일컫는다. 

> 자바의 WORA

<img src="https://user-images.githubusercontent.com/62331803/103472352-ed50da00-4dcf-11eb-9887-888ff630d697.png" width="60%"><br>

- java로 쓰여진 소스코드(`.java`)를 작성했다고 가정해보자. 
- Java 컴파일러인 `javac.exe`가 해당 소스코드를 Byte Code(`.class`)로 컴파일한다. 
- 이 때 컴파일된 Byte Code(`.class`)는 CPU가 이해할 수 있는 기계어가 아니라, 가상머신이 이해할 수 있는 형태로 번역된 것이다. 
- Byte Code(`.class`)는 JVM(자바가상머신)에 의해서 해석되고 실행된다. 
- 이 때 JVM이 실행되는 플랫폼마다 다른 형태로 존재하며, 따라서 실행될 환경에 맞게 Byte Code(`.class`)를 인터프리팅하여 실행시킨다. <br>
<br>

### 2. JDK, JRE, JVM

**JDK, JRE, JVM은 다음과 같은 관계를 가지고 있다.**<br>
<br>

> JDK ⊃ JRE ⊃ JVM <br>

source : [catch-me-java.tistory.com](https://catch-me-java.tistory.com/11?category=438116)<br>

<img src="https://user-images.githubusercontent.com/62331803/103472667-42421f80-4dd3-11eb-8dbb-5f4ea412d06f.png" width="80%"><br>
<br>


**2-1) JDK(Java Development Kit)** : 컴파일러, 역 어셈블러, 디버거, 의존관계 분석 등 개발에 필요한 도구를 제공한다. 즉, 소스코드를 JVM이 읽을 수 있는 형태로 변경해준다. 
   - `javac.exe` : 자바 컴파일러
   - `javap.exe` : 디스어셈블 도구
   - `jar.exe` 서로 관련있는 클래스 라이브러 리들과 리소스를 하나의 JAR 파일로 묶는 도구
    - `jdb.exe` : 자바 디버깅 도구


**2-2) JRE(Java Runtime Environment)** : JVM이 원활하게 작동할 수 있도록 환경을 맞춰주는 역할을 한다. 클래스로더와 클래스 라이브러리를 통해 작성된 코드를 라이브러리와 결합한 후 JVM에 넘긴다. 개발과 관련된 도구는 포함하지 않는다.
   - `java.exe` : 자바 응용 프로그램 로더. javac 컴파일러가 만든 클래스 파일을 해석 및 실행
   - `Class Loader` 

**2-3) JVM(Java Virtual Machine)** : 플랫폼에 독립적으로 실행될 수 있는 추상층을 제공한다. 즉, 바이트코드를 OS에 맞추어 변경해주는데 이 때 사용하는 것이 인터프리터와 JIT이다.
   - `Interpreter` : 바이트코드를 읽고 해석
   - `JIT(Just In Time) Compiler` : 런타임에 기계어로 번역해주는 컴파일러. 동적 번역(Dynamic translation)이라고도 불리는 기법. 프로그램 실행 속도 향상을 위해 개발<br>
<br>



> 요약<br>

|JDK|JRE|JVM|
|:---:|:---:|:---:|
|개발에 필요한 툴|자바 클래스와 라이브러리, 필수 컴포넌트 제공|바이트코드 실행, 실행에 필요한 환경 제공|
|플랫폼 독립적|플랫폼 독립적|플랫폼 종속적|

<br><br>

:orange_book: **References**<br>

- https://zitto15.tistory.com/40
- http://www.tcpschool.com/java/java_intro_basic
- https://sowhat4.tistory.com/61
- https://m.blog.naver.com/duqrlwjddns1/221770110714
- https://catch-me-java.tistory.com/11?category=438116
