---
title:  "[Java] 자바파일 컴파일(javac)과 실행(java)"
date: 2020-11-10
categories: ['Java']
tags: ['Java']
---

**IDE를 사용하지 않고 자바파일(.java)을 컴파일하고 실행해보자.** :raising_hand:<br><br>

자바파일을 실행하기 위해서는 컴파일과 실행과정을 거쳐야 한다. 이를 위해 자바 컴파일러와 자바 응용 프로그램 로더가 필요하다.<BR>

- `javac.exe` : JDK에 포함. 자바 컴파일러
- `java.exe` : JRE에 포함. 자바 응용 프로그램 로더. javac 컴파일러가 만든 클래스 파일을 해석 및 실행
<br>

:pencil2: **cf. 자바 11부터 JDK에 JRE를 포함한 형태로 배포된다.**<br> 

<br>

### 1. 컴파일

```java
package javabasic.week1;

public class CompileDemo {  
  
    static class SubClass{  
        void print(){  
            System.out.println("from SubClass.");  
  }  
    }  
  
    public static void main(String[] args) {  
        new SubClass().print();  
  }  
}
```

**컴파일**<br>
```java
javac CompileDemo
```


**컴파일 결과**<br>
```java
2021-01-03  오후 08:40    <DIR>          .
2021-01-03  오후 08:40    <DIR>          ..
2021-01-03  오후 08:40               525 CompileDemo$SubClass.class
2021-01-03  오후 08:40               428 CompileDemo.class
2021-01-03  오후 08:40               269 CompileDemo.java
```
<br>

### 2. 실행

**소스코드가 속한 패키지의 parent 위치에서 실행해야 한다.**<br>

- CompileDemo는 `javabasic`의 하위패키지 `week1`에 속한 클래스이다.
- 따라서 `javabasic`의 parent인 `java`에서 실행한다.
- FQCN(`패키지.하위패키지.클래스명`)으로 명령어를 작성한다. 

<img src="https://user-images.githubusercontent.com/62331803/103477684-84378980-4e04-11eb-89ec-403ebfc46b68.png" width="70%"><br>

**실행**<br>

```java 
java javabasic.week1.CompileDemo
```


**실행성공**<br>

```java
from SubClass.
```


<br><br>

:orange_book: **References**<br>
- https://sowhat4.tistory.com/61
- http://sjava.net/2008/02/javac-%EB%AA%85%EB%A0%B9%EC%96%B4%EC%9D%98-%EC%98%B5%EC%85%98-%EC%A0%95%EB%A6%AC/