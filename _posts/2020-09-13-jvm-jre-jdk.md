---
title:  "[Java] JVM, JRE, JDK"
date: 2020-9-13
categories: ['Web']
tags: ['Java','JVM','JRE','JDK']
---

[Java]  JVM, JRE, JDK

### JDK, JRE, JVM
<img src="https://user-images.githubusercontent.com/62331803/93018740-d877ed00-f60c-11ea-99b1-d577ccd53995.png" width="70%">
- JDK(개발도구): JRE + Development kit<br>
- JRE(실행환경): JVM + Library <br>
즉, 자바 사용을 위해 JDK 필수
<br>

### JVM | 자바 가상머신
- 다른 프로그램을 실행시키는 것이 목적인 프로그램
-  기본 기능
    -  WORA(Write Once Run Anywhere)
        - 자바 프로그램이 어느 기기,  또는 어느 운영체제 상에서도 실행될 수 있게 한다 (플랫폼 독립적)
     - 프로그램 메모리 관리와 최적화
		  -  1995년 자바 공개 전, 모든 컴퓨터 프로그램은 특성 운영체제에 맞게 작성되었으며, 
		  - 프로그램 메모리는 소프트웨어 개발자가 관리했음

[참고] http://www.itworld.co.kr/news/110837

<br>

### JRE | 자바 실행환경
**Java Runtime Environment**
- JVM이 자바 프로그램을 동작시킬 때 필요한 라이브러리 파일과 기타 파일을 포함
- 즉, JVM의 실행환경을 구현하는 역할
- 자바 프로그램을 **실행**시키기 위해서, JRE 반드시 필요
- 자바 프로그래밍 도구(JDK)는 포함되어 있지 않음.

<br>

### JDK  | 자바 개발도구
**Java Development Kit**
- 자바로 프로그래밍을 하기 위한 도구
- 개발에 필요한 도구(자바컴파일러javac, java 등)를 포함
- JDK를 설치하면 JRE도 같이 설치됨
- 즉, JDK = JRE + @

<br>

<details>
<summary>출처</summary>

- https://devpouch.tistory.com/9 <br>
- https://wikidocs.net/257<br>
- https://medium.com/webeveloper/jvm-java-virtual-machine-architecture-94b914e93d86 <br>
- http://www.itworld.co.kr/news/110837<br>

</details>
