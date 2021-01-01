---
title:  "[Java] 자바의 패키지와 클래스패스"
date: 2021-01-01
categories: ['Java']
tags: ['Java']
---

**자바의 패키지와 클래스패스를 알아보자** :raising_hand:<br>
<br>

## 목차 
### 패키지
1. [패키지의 역할](#1-패키지의-역할)
2. [패키지 사용방법](#2-패키지-사용방법)
3. [패키지 내의 클래스에 접근하기](#3-패키지-내의-클래스에-접근하기)
4. [패키지의 종류](#4-패키지의-종류)
5. [static import 사용하기](#5-static-import-사용하기)
6. [클래스명 conflicts 처리하기](#5-static-import-사용하기)
7. [디렉토리 구조](#7-디렉토리-구조)
- 7.1. [CLASSPATH란?](#7-1-classpath란)
- 7.2. [CLASSPATH 세팅하기](#7-2-classpath-세팅하기)
- 7.3. [classpath 옵션](#7-3-javac-java-옵션--classpath)
8. [접근제어자](#8-접근제어자) 

<br><br><br>


# 자바의 패키지

**Package**란 다수의 클래스와 인터페이스를 캡슐화하기 위한 매커니즘이다. <br>

## 1. 패키지의 역할
패키지는 연관된 클래스를 담는 컨테이너 역할을 한다. 패키지 내에는 외부로부터 접근 가능한 클래스들과 특정 목적을 위해 접근이 제한된 클래스가 존재한다.<br>


### 1-1) 동일한 이름을 가진 클래스들을 구분할 수 있다.

- 예를 들어, `Test`라는 이름의 클래스가 2개의 패키지에 각각 존재할 수 있다.  
- `livestudy.week7.Test` , `livestudy.week6.Test`

### 1-2) class, interface, enumeration, annotation 등을 보다 쉽게 검색하고 사용할 수 있다.

### 1-3) 접근을 제어할 수 있다.
- 패키지 레벨에서 `protected` 와 `default` 접근제어를 사용하여 접근을 통제할 수 있다. 
- `protected` : 같은 패키지에 존재하는 클래스와 자식 클래스에서 접근 가능
- `default` : 같은 패키지에 존재하는 클래스에서 접근 가능

### 1-4) 데이터캡슐화(혹은 데이터 은닉)의 용도로 사용할 수 있다.

<br>
<br>

## 2. 패키지 사용방법

**패키지의 이름은 디렉토리 구조와 동일한 형태를 띄고 있다.**<br>

예를 들어 패키지 이름이 `korea.seoul.gangnam`이라면, `korea`, `seoul`, `gangnam` 3개의 디렉토리가 존재함을 의미한다. `gangnam`디렉토리는 `seoul`디렉토리 안에 존재하고, `seoul`디렉토리는 `korea`디렉토리 안에 존재한다. 이 때, `korea` 디렉토리는 **CLASSPATH**변수에 의해 접근 가능하다. `korea`패키지의 부모 디렉토리가 **CLASSPATH**에 존재하기 때문이다.<br>


### 2-1) 패키지 네이밍 컨벤션
- 기본적인 패키지의 이름은 도메인 네임을 거꾸로 뒤집은 형태이다. 
  -  ex. `com.ahnyezi`<br>

### 2-2) 패키지에 클래스 추가
- 파일 상단에 패키지이름을 추가하여, 해당 클래스를 패키지에 포함시킬 수 있다.
- ex. `package javabasic.week7;`<br>

### 2-3) 하위 패키지
- 패키지 내에 존재하는 또 다른 패키지를 하위 패키지라 일컫는다.
- 하위 패키지는 자동적으로 import 되지 않기 때문에, 명시적으로 `import` 키워드를 사용하여 가져와야 한다.
   - ex. `import java.util.*;`
   - `java` 패키지 내에 `util` 이라는 하위패키지가 존재
- 또한 하위패키지의 멤버들은 각각 다른 패키지처럼 여겨지기 때문에, 하나의 하위패키지를 import했다고 해서 다른 하위패키지들도 import된 것이 아니다.<br>
<br>
<br>

## 3. 패키지 내의 클래스에 접근하기

**다른 패키지의 클래스를 가져와서 사용하기 위해서는 다음과 같은 작업이 필요하다.**<br>

```java
// util 패키지의 vector 클래스를 가져오기
import java.util.vector;

// util 패키지의 모든 클래스 가져오기
import java.util.*;
```

- 첫번째 명령문은 `java`패키지에 포함되어 있는 `util`패키지의 `vector`클래스를 가져온다.
- 두번째 명령문은 `util`패키지의 모든 클래스를 가져온다.<br>
<br>

```java
// 해당 패키지의 모든 클래스와 인터페이스에 접근가능하다. (하위패키지는 제외)
import package.*;

// 언급된 클래스(classname)만 접근 가능하다.
import package.classname;

// 일반적으로 클래스명은 2개의 패키지에 동일한 이름의 클래스가 존재할 때 구별하기 위해 쓰인다.
import java.util.Date;
import my.package.Date;
```
<br>
<br>

## 4. 패키지의 종류

<img src="https://user-images.githubusercontent.com/62331803/103433164-afb54b00-4c2f-11eb-81cc-fdd442f53131.png" width="60%"><br>

### 4-1) Built-in 패키지
**Java API에 포함되는 다수의 클래스를 포함하는 패키지이다.**<BR>
:point_right: **예시**<br>

- `java.lang` : language support 클래스들을 포함하는 패키지
   - 프리미티브 타입이나 수학 연산이 정의되어 있는 클래스들
   - 자동적으로 import 됨
- `java.io` : 입출력 기능을 지원하는 클래스들을 포함하는 패키지
- `java.util` : 자료 구조 구현을 위한 유틸리티 클래스를 포함하는 패키지
   - Linked List, Dictionary...
   - Date, Time 기능 또한 지원
- `java.applet` : Applets을 생성하기 위한 클래스들을 포함하는 패키지
- `java.awt` : GUI 컴포넌트를 구현하기 위한 클래스들을 포함하는 패키지
- `java.net` : 네트워킹 기능을 지원하기 위한 클래스를 포함하는 패키지<BR>
<BR>

### 4-2) 사용자 정의 패키지
**사용자가 직접 정의한 패키지이다.** <BR>
먼저 패키지명과 동일한 디렉토리를 생성하고, 해당 패키지에 포함할 클래스를 만든 뒤, 맨 첫번째 명령문으로 패키지 이름을 넣는다.<BR>

```java
// 패키지 이름은 파일이 저장된 디렉토리의 이름과 반드시 같아야 함
package myPackage;

public class MyClass{
	public void getNames(String s){
		System.out.println(s);
	}
}
```

다른 패키지에서 import 해서 사용할 수 있다.

```java
// myPackage에 존재하는 `MyClass` 클래스를 import해서 사용할 수 있다.
import myPackage.MyClass;

public class PrintName{
	public static void main(String args[]){
		String name = "Pikachu";
		MyClass mc = new MyClass();
		mc.getNames(name);
	}
}
```
<BR>
<BR>

## 5. static import 사용하기

**Static import**는 자바 5버전 이상에서 소개된 기능이다.<br>

**임의의 패키지의 클래스에서 public static으로 정의된 멤버(필드나 메서드)를 사용할 때, 클래스 풀네임을 나열하지 않고도 사용할 수 있다.**<br> 

```java
// import 뒤에 static을 붙여 가져온다.
import static java.lang.System.*;

class StaticImportTest{
	public static void main(String args[]){
	
	// static import를 사용했기 때문에 'System.out'을 모두 나열할 필요 없다.
	out.println("Pocketmon");
	
	}
}
```
```java
Pocketmon
```

:pencil2: cf. [System 클래스](https://docs.oracle.com/javase/7/docs/api/java/lang/System.html)<br>

<BR>
<BR>

## 6. 클래스명 conflicts 처리하기

**서로 다른 패키지에 동일한 이름의 클래스가 존재할 경우 사용에 주의해야 한다.**<br>

```java
import java.util.*;
import java.sql.*;

// 2개의 패키지에 모두 Date라는 이름의 클래스가 존재
// 어떤 패키지에 존재하는 Date인지 명시해주지 않았기 때문에 컴파일 에러를 발생시킴.

Date today; // ERROR-- java.util.Date or java.sql.Date?
```
<BR>

다음과 같이 수정해서 사용할 수 있다.<BR>

```java
import java.util.Date;
import java.sql.*;
```
<BR>
 
 만약 2개의 패키지에 있는 Date를 다 사용하려할 경우, 클래스 풀네임을 사용해서 코드를 작성한다.<BR>

```java
java.util.Date deadLine = new java.util.Date();
java.sql.Date today = new java.sql.Date();
```
<br>
<br>

## 7. 디렉토리 구조

**패키지명**은 클래스들을 저장하기 위해 사용하는 **디렉토리의 구조와 연관되어 있다.**<br>

- **실제 디렉토리 구조는 패키지명과 동일하게(패키지\하위 패키지) 저장되어 있다.**
   - `com.zzz.project1.subproject2`패키지의 `MyClass 클래스`가 존재한다면, 실제 클래스 파일은 다음과 같이 저장되어 있다.
   - `$BASE_DIR/com/zzz/project1/subproject2/MyClass.class` 
   - `$BASE_DIR`은 패키지의 기본 디렉토리가 된다.
- **기본 디렉토리($BASE_DIR)은 파일 시스템의 어디에나 위치할 수 있다.** 
   - 따라서 자바 컴파일러와 JVM은 기본 디렉토리의 위치를 알고 있어야 한다.
   - 이를 위해서 필요한 환경 변수(environment variable)을 **CLASSPATH**라 부른다.
- **CLASSPATH**는 커맨드 쉘이 실행할 프로그램을 찾을 수 있게끔 PATH를 알려주는데 사용된다.<BR>
<BR>


### 7-1) CLASSPATH란?


import 키워드를 사용한 명령어가 있다.<br>

```java
import org.company.Menu;
```

해당 명령어의 의미는 org.company 패키지에 있는 Menu라는 클래스를 현재 클래스에서 사용할 수 있게 하는 것이다.<br>
<br>

JVM은 `Menu`클래스의 위치를 찾아서 해당 클래스의 인스턴스를 생성한다.<br>

```java
Menu menu = new Menu();
```
그럼 JVM은 어떻게 해당 클래스를 찾는 것일까? <BR>
만약 JVM이 Menu 클래스를 찾기 위해 존재하는 모든 클래스를 검사해야 한다면 매우 비효율적일 것이다. 그러므로 우리는 CLASSPATH 변수를 사용하여 해당 클래스가 위치한 곳을 JVM에게 알려준다. <BR>
<BR>

만약 Menu 클래스가 `dir`이라는 디렉토리에 존재한다면, Menu 클래스의 전체 경로는 `dir/org/company/Menu`가 된다.<br>

이 때 `dir`이라는 디렉토리를 **classpath 변수**로 등록해 놓고, 나머지 정보(org/company/Meny)는 `import` 명령어를 통해 제공해주므로서 외부 패키지의 클래스를 가져와 사용할 수 있게 된다.<br>

`jar`파일도 동일한 형태로 동작시킬 수 있다. `jar`파일의 path를 classpath로 등록하면 JVM이 해당 `jar` 파일 안에 있는 클래스들을 찾아서 사용할 수 있게 된다.<br>
<br>


### 7-2) CLASSPATH 세팅하기

CLASSPATH는 어플리케이션에서 필요한 파일의 위치를 자바 컴파일러와 JVM에 알려주는 역할을 한다고 했다.<BR>

만약 CLASSPATH가 세팅되어 있지 않다면 자바 컴파일러는 필요한 파일을 찾을 수 없고 컴파일 타임에 ERROR를 던지게 된다.<BR>

```java
Error: Could not find or load main class <class name> (e.g. GFG)
```
<br>

- CMD에서 JAVA CLASSPATH 환경변수 세팅(Windows)
```java
// set CLASSPATH=_path1_;_path2_
set CLASSPATH=.;C;/Program Files\Java\jdk-15.0.1\bin
```
세미콜론은 separator 역할을 담당하며, 닷(.)은 위 명령에서 CLASSPATH의 기본값이다.<BR>

- GUI로 JAVA CLASSPATH 환경변수 세팅(Windows)
[참고](https://www.geeksforgeeks.org/how-to-set-classpath-in-java/?ref=rp)<br>
<br><br>

### 7-3) javac java 옵션 -classpath

**CLASSPATH 환경변수를 등록하여 사용하는 대신, 커맨드라인 옵션 (javac와 java의** `-classpath` **혹은** `-cp`**)을 사용할 수 있다.**<br>
<br>

:point_right: **사용 방법**<br>

```java
-classpath(cp) path(파일 절대 경로)
```

**자바파일을 컴파일을 하기 위해서 필요한 클래스 파일을 찾기 위해, 컴파일 시 파일 경로를 지정해주는 옵션이다.** <br>
<br>

:point_right: **실습**<br>

> 'RequiredClass' 클래스와 해당 클래스를 사용하는 'ClasspathDemo' 클래스를 선언한다.

```java
package javabasic.week7;  
  
class RequiredClass{  
    public void print(String s){  
        System.out.println(s);  
  }  
}  
  
public class ClasspathDemo {  
    public static void main(String[] args) {  
        RequiredClass rc = new RequiredClass();  
  rc.print("classpath test");  
  }  
}
```

```java
C:\IdeaProjects\JavaStudy\src\main\java\javabasic\week7>javac ClasspathDemo.java

C:\IdeaProjects\JavaStudy\src\main\java\javabasic\week7>dir

2021-01-01  오후 03:54    <DIR>          .
2021-01-01  오후 03:54    <DIR>          ..
2021-01-01  오후 03:54               411 ClasspathDemo.class
2021-01-01  오후 03:53               300 ClasspathDemo.java
2021-01-01  오후 03:54               405 RequiredClass.class

```
<br>

> RequiredClass 클래스 파일을 lib 디렉토리로 이동시킨 후 실행

```java
java ClasspathDemo
```

**결과**<br>

```java
`Exception` `in` `thread` `"main"` `java.lang.NoClassDefFoundError: RequiredClass`
`at ClasspathDemo.main(ClasspathDemo.java:9)`
```
requiredClass.class가 현재 디렉터리에 존재하지 않기 때문<br>
<br>

**해결**<br>

```java
java -classpath ".;lib" ClasspathDemo
```

- `.`: 현재 디렉토리에서 클래스를 찾는다
- `;`: 경로와 경로를 구분해주는 구분자<br>

<br>
<br>

## 8. 접근제어자
[출처](https://yongku.tistory.com/entry/%EC%9E%90%EB%B0%94Java-%EC%A0%91%EA%B7%BC-%EC%A7%80%EC%A0%95%EC%9E%90-%EB%B0%8F-%ED%8C%A8%ED%82%A4%EC%A7%80)<BR>


|멤버에 접근하는 클래스|private|default|protected|public|
|:---:|:---:|:---:|:---:|:---:|
|같은 패키지의 클래스|X|O|O|O|
|다른 패키지의 클래스|X|X|X|O|
|접근 가능 영역|클래스 내|동일 패키지 내|동일 패키지와 자식 클래스|모든 클래스|

<BR>
<BR>

### +plus
1. 모든 클래스는 패키지에 속해있다.
2. 만약 파일 내에 패키지가 명시되어 있지 않다면, 해당 클래스 파일은 특별한 unnamed package로 이동한다.<br>
<br>
<br>
<br>


:orange_book: **References**<br>

- GeeksforGeeks
   - [Package in Java](https://www.geeksforgeeks.org/packages-in-java/)
   - [CLASSPATH in Java](https://www.geeksforgeeks.org/classpath-in-java/)
   - [How to Set Classpath in Java?](https://www.geeksforgeeks.org/how-to-set-classpath-in-java/?ref=rp)
- [생활코딩-클래스패스](https://opentutorials.org/course/1223/5527)