---
title:  "[Java] 자바의 예외"
date: 2021-01-16
categories: ['Java']
tags: ['Java']
---

**자바의 예외와 그 처리방법을 알아보자** :raising_hand:<br>
<br>

## 목차 
### 예외처리
1. [프로그램 오류와 예외 클래스 계층 구조  ](#)
2. [예외처리하기, try-catch문의 흐름 ](#)
3. [printStackTrace()와 멀티 catch 블럭](#)
4. [메서드에 예외 선언하기, finally 블럭](#)
5. [사용자 정의 예외 만들기, 예외 되던지기](#)
6. [연결된 예외](#)
7. [추가할 내용 : try- with -resource](#)

<br><br><br>


## 1. 프로그램 오류와 예외 클래스 계층 구조

### 1-1) 프로그램 오류의 종류

**프로그램의 오류는 크게 3가지가 있다.**<BR>

- 컴파일 에러 : 컴파일할 때 발생하는 에러를 일컫는다.
- 런타임 에러 : 실행 중에 발생하는 에러를 일컫는다.
- 논리적 에러 : 의도와 다르게 동작하는 것을 일컫는다. 
<br>

**Java에서 정의한 런타임 에러(실행 중 발생하는 에러)**
- `에러(error)` : 프로그램 코드에 의해 수습될 수 없는 **심각한 오류**
   - OutOfMemoryException...
- `예외(exception)` : 프로그램 코드에 의해서 수습될 수 있는 **다소 미약한 오류**
<br><br>

### 1-2) 예외처리(exception handling)의 정의와 목적
- 정의 : 프로그램 실행 시 발생할 수 있는 예외의 발생에 대비한 코드를 작성하는 것
- 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것
<br><br>


### 1-3) 예외 클래스의 계층 구조

[source: soen.kr](http://www.soen.kr/lecture/library/java/5-1.htm)<br>

<img src="https://user-images.githubusercontent.com/62331803/104864493-7fcead00-597c-11eb-83c2-c0b3936c2dc4.png" width="80%"><br>


- `Object` : 최고조상
- `Throwable` : 모든 오류의 조상
- `Error` : 시스템 구조상의 문제로 발생하는 심각한 오류
- `Exception` : 프로그램의 알고리즘이나 실행 절차상의 문제로 발생하는 경미한 오류

<br><br>

### 1-4) Exception과 RuntimeException

Exception 클래스는 다시 `Exception의 자손`과 `RuntimeException의 자손`으로 구분지을 수 있다. (RuntimeException또한 Exception의 자손이다)

- `Exception 클래스와 그 자손` : 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외
   - I/OException(입출력예외), ClassNotFoundException(사용하려는 클래스가 존재하지 않음)...
- `RuntimeException 클래스와 그 자손` : 프로그래머의 실수로 발생하는 예외
   - ArithmeticException(잘못된 계산), ClassCastException(잘못된 형변환), NullPointerException(객체가 null), IndexOutOfBoundaryException(배열 범위 벗어남)...

<br><br><br>




## 2. 예외처리하기, try-catch문의 흐름

### 2-1) 예외처리(exception handling)의 정의와 목적
- 정의 : 프로그램 실행 시 발생할 수 있는 예외의 발생에 대비한 코드를 작성하는 것
- 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것

**예외가 발생할 가능성이 있는 문장을** `try` **블럭에 넣고, 예외가 발생했을 때 처리할 문장을**`catch`**문장에 넣는다. 괄호를 생략할 수 없음에 주의하자!**<br>

```java
try{
	// 예외가 발생할 가능성 있는 문장..
} catch (Exception1 e1){
	// Exception1이 발생할 경우, 이를 처리하기 위한 문장
} catch (Exception2 e2){
	// Exception2이 발생할 경우, 이를 처리하기 위한 문장
} catch (Exception3 e3){
	// Exception3이 발생할 경우, 이를 처리하기 위한 문장
}
```
<br><br>

### 2-2) try-catch 문에서의 흐름

**1. try블럭에서 예외가 발생한 경우**<br>

- 발생한 예외와 일치하는 catch 블럭이 있는지 확인
- 일치하는 catch 블럭을 찾게 되면, 그 catch 블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속 수행한다. 만일 일치하는 catch블럭을 찾지 못하면, 예외는 처리되지 못한다.
- try 블럭 내에서 예외가 발생하면, 예외발생문장 뒤의 문장들을 수행되지 않는다.
<br>

**2. try 블럭 내에서 예외가 발생하지 않은 경우**<br>

- catch 블럭을 거치지 않고 전체 try-catch문을 빠져나가서 수행을 계속한다.

> 예제 : ArithmeticException(0으로 나누는 예외) 발생시키고 처리하기<br>

```java
class ExceptionTest {
    public static void main(String[] args) {
        System.out.println(1);
        try {
            System.out.println(1 / 0); // 0으로 나누기 오류
            System.out.println(2);     // 예외발생한 다음문장은 실행되지 않는다.
        } catch (ArithmeticException ae) {
            System.out.println(3);      // 예외처리 문장
        } catch (Exception e) { // 모든 예외의 최고 조상
            System.out.println("Exception");
        }
        System.out.println(4);
    }
}
```
```java
1
3
4
```

<br><br><br>

## 3. printStackTrace()와 멀티 catch 블럭

### 3-1) printStackTrace()와 getMessage()

- `printStackTrace()` : 예외발생 당시의 호출스택(Call Stack)에 있던 메서드의 정보와 예외 메세지를 화면에 출력한다.
- `getMessage()` : 발생한 예외클래스의 인스턴스에 저장된 메세지를 얻는다.


> 예제 : ArithmeticException 처리
```java
public class ExceptionTest {
    public static void main(String[] args) {
        try {
            System.out.println(1 / 0); // 0으로 나누기 오류
        } catch (ArithmeticException ae) {
            ae.printStackTrace();
            System.out.println(ae.getMessage());
        }
    }
}
```
```java
java.lang.ArithmeticException: / by zero
	at javabasic.week9.ExceptionTest.main(ExceptionTest.java:6)
/ by zero
```

:point_right: **코드 실행 순서**<br>
**1. 0으로 나누는 예외 발생**<br>
**2. ArithmeticException 타입의 (예외)객체 생성**<br>

- 객체에는 발생한 **예외에 대한 정보**가 들어있으며, **printStackTrace(), getMessage() 등 정보를 가져올 수 있는 메서드**를 가지고 있다.

**3. 예외를 처리할 수 있는 catch 블럭이 있는지 확인한다.**<br>

- 발생한 예외객체 타입과 catch블럭의 참조변수 타입이 일치한다(ArithmeticException).

**4. 발생한 객체의 주소가 catch블럭의 참조변수 `ae`에 들어간다.**<br> 

- 즉 참조변수(ae)가 예외객체를 가리키게 된다.
- 이 때 참조변수(ae)를 사용할 수 있는 유효범위(scope)는 해당 catch블럭이 끝날 때까지이다.

**5. 참조변수를 통해 객체에 담긴 예외정보를 가져온다.**<br>

<br><br>

### 3-2) 멀티 catch 블럭

**멀티 catch 블럭이란, 코드 중복을 제거하기 위해서 내용이 같은 catch 블럭을 하나로 합친 것을 말한다. (JDK 1.7부터)**<br>

> 멀티 catch 사용 전의 try-catch 문<br>

예외 처리 문장이 같더라도 catch블록을 따로 작성해야 한다. 

```java
try {
	...
} catch(ExceptionA e){
	...
} catch(ExceptionB e2){
	...
}
```

> 멀티 catch 사용 후의 try-catch 문<br>

예외 처리 문장이 같다면 catch블록을 묶어서 사용한다.

```java
try{
	...
} catch (ExceptionA | ExceptionB e){
	...
}
```
<br>

:point_right: **멀티 catch문을 사용할 때 주의할 점**

**1. 부모-자식관계인 예외클래스를 사용**<br>

부모타입의 참조변수가 선언된 catch블록을 선언하면, 해당 부모를 상속받은 자손은 모두 해당 catch 블럭에서 예외처리 된다. 따라서 굳이 따로 자식타입의 catch 블록을 선언할 필요없다. 

> 오류 : 부모-자식 예외클래스를 함께 선언한 catch블록 사용 (에러 발생)

```java
try{
	...
} catch (ParentException | ChildException e){
   // 에러 발생
}
```

> 올바른 방법

```java
try{
	...
} catch (ParentException e){
   ...
}
```
<br>

**2. 예외클래스 A와 B의 공통멤버만 사용가능**<br>

두 개의 클래스에 모두 존재하는 멤버만 사용할 수 있다.

```java
try{
	...
} catch (ExceptionA | ExceptionB e){
//   e.methodA(); // error! : ExceptionA만 선언된 메서드 호출불가

   if (e instanceof ExceptionA){
      ExceptionA e1 = (ExceptionA) e;
      e1.methodA(); // 형변환 후에 사용 가능
   }
}
```

<br><br><br>

## 4. 예외 발생시키기

### 4-1) 예외 발생시키기

**1. 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든 다음**<br>

- `Exception e = new Exception("고의로 발생시킨 예외");`

**2. 키워드 throw를 이용해서 예외를 발생시킨다.**<br>

> 예제 : 예외 고의로 생성하고 처리하기

```java
public class ExceptionTest {

    public static void main(String[] args) {
        try {
            Exception e = new Exception("고의로 발생시킨 예외");
            throw e;
            // throw new Exception("고의로 발생시킨 예외");
        } catch (Exception e) {
            System.out.println("에러 메세지:" + e.getMessage());
            e.printStackTrace();
        }
        System.out.println("프로그램 정상 종료");
    }

}
```
```java
에러 메세지:고의로 발생시킨 예외
프로그램 정상 종료
java.lang.Exception: 고의로 발생시킨 예외
	at javabasic.week9.ExceptionTest.main(ExceptionTest.java:8)
```
<br><br>

### 4-2) checked 예외, unchecked 예외
- `checked 예외` : 컴파일러가 예외처리여부를 체크(예외 처리 필수)
   - Exception과 그 자손
- `unchecked 예외` : 컴파일러가 예외처리여부를 체크 안함(예외 처리 선택)
   - RuntimeException과 그 자손

> 예제 : checked 예외 발생시키기 <br>

예외처리 안하면 컴파일 에러를 발생시킨다.

```java
public class ExceptionTest {
    public static void main(String[] args) {
throw new RuntimeException("unchecked 예외");    }
}
```
```java
java: unreported exception 
java.lang.Exception; must be caught or declared to be thrown
```

> 예제 : unchecked 예외 발생시키기<br>

예외처리 안하면 런타임 에러를 발생시켜, 프로그램이 비정상적으로 종료된다.

```java
public class ExceptionTest {
    public static void main(String[] args) {
        throw new RuntimeException("unchecked 예외");
    }
}
```
```java
Exception in thread "main" java.lang.RuntimeException: unchecked 예외
	at javabasic.week9.ExceptionTest.main(ExceptionTest.java:7)
```


<br><br><br>



## 4. 메서드에 예외 선언하기, finally 블럭

### 4-1) 메서드에 예외 선언하기
- 예외 처리 방법 : try-catch문(직접처리), 예외선언하기(떠넘기기)
- 메서드가 호출되면 발생가능한 예외를 호출하는 쪽에 알리는 것
- (cf) 예외를 발생시키는 키워드 throw와 예외를 메서드에 선언할 때 쓰이는 throws 잘 구별하기
<br>

:point_right: **메서드의 예외 선언 방법** <br>

메서드를 호출해서 사용하는 이에게, 이러한 예외가 발생할 수 있음을 알려준다.<br>

-  여러 개의 예외를 선언할 수 있다.
-  메서드를 호출한 쪽에서는 해당 예외들에 대한 try-catch블럭을 사용하거나 또 다시 메서드에 예외를 선언하여 예외를 처리할 수 있다.
- (주의:exclamation:) unchecked/checked 예외 모두 선언가능하지만, 정석적으로는 checked예외(필수처리예외)만 선언한다.

```java
   // 메서드에 예외 선언
   void method() throws Exception1, Exception2, Exception3 ....{
      // 메서드 내용
}
```

<br>

:point_right: **Exception 예외 선언** <br>

- Exception은 모든 예외의 조상이다.
- 따라서 해당 클래스를 메서드에 선언하면 모든 종류의 예외가 발생가능하다.

```java
   // 모든 종류의 예외 발생 가능
   // method()에서 Exception과 그 자손 예외 발생 가능
   void method() throws Exception{
      // 메서드 내용
}
```
<br>

> 예제 : 프로그램 설치 시 예외처리 <br>

- 설치를 진행하는 도중, 공간이 부족해서 또는 메모리가 부족해서 정상작업이 불가능할 수 있다.
- 이 때 `SpaceException`, `MemoryException`을 발생시켜 자신을 호출한 쪽에 문제가 발생했음을 알린다.

```java
   static void startInstall() throws SpaceException, MemoryException{
        // 충분한 설치 공간이 없을 경우
        if (!enoughSpace())
            throw new SpaceException("설치할 공간이 부족합니다.");
        // 충분한 메모리가 없을 경우
        if (!enoughMemory())
            throw new MemoryException("메모리가 부족합니다");
    }
```

<br>

> 예제2 : 예외처리의 흐름

```java
 public static void main(String[] args) throws Exception{
        method1();
    }

    static void method1() throws Exception{
        method2();
    }

    static void method2() throws Exception{
        throw new Exception();
    }
```
#### 예외처리순서

1. main()이 method1()을 호출
2. method1()이 method2()를 호출
3. method2()에서 예외 발생
4. method2()는 예외 처리하지 않고 죽음
5. method2()를 호출한 method1()이 method2()의 예외를 받음
6. method1()은 예외 처리하지 않고 죽음
7. method1()을 호출한 main()이 method1()으로부터 예외를 받음
8. main()이 예외 처리하지 않고 죽음
9. main 메서드가 죽으면서 비정상 종료가 되고, 이 예외는 JVM에게 넘겨진다.
10. JVM이 받아서 마지막으로 예외를 처리함 <BR>

#### 콘솔 (JVM의 기본예외처리기가 printStackTrace()로 출력한 내용)

```java
Exception in thread "main" java.lang.Exception
	at javabasic.week9.ExceptionTest.method2(ExceptionTest.java:14)
	at javabasic.week9.ExceptionTest.method1(ExceptionTest.java:10)
	at javabasic.week9.ExceptionTest.main(ExceptionTest.java:6)
```
<br>

> 예제3 : 파일입출력

- 파일이름이 null값이면 Exception을 main으로 던져서 다시 입력받게끔

```java
class ExceptionTest {

    public static void main(String[] args) {
        try {
            File f = createFile("exception_test.txt");
            System.out.println(f.getName()+"파일이 성공적으로 생성되었습니다.");
        } catch(Exception e){
            System.out.println(e.getMessage()+" 다시 입력해주시기 바랍니다.");
        }
    }

    static File createFile(String fileName) throws Exception{
        if (fileName == null || fileName.equals(""))
            throw new Exception("파일 이름이 유효하지 않습니다.");
        File f = new File(fileName);    // file 클래스 객체 생성
        f.createNewFile();              // file 객체의 createNewFile 메서드로 파일 생성
        return f;                       // 생성된 객체 참조 반환
    }
}
```
```java
exception_test.txt파일이 성공적으로 생성되었습니다.
```
<br><br>


### 4-3) finally 블럭

- 예외 발생 여부와 관계없이 수행되어야 하는 코드를 넣는다.
- (cf) try 블럭 안에 return 문이 있어서 try 블럭을 벗어나는 경우에도, finally 블럭을 실행한 뒤에 return된다.

>  finally 블럭 사용 방법

```java
try{
   // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1){
   // 예외처리를 위한 문장을 적는다.
} finally{
   // 예외의 발생여부와 관계없이 항상 수행되어야 하는 문장들을 넣는다.
   // finally 블럭은 try-catch문의 맨 마지막에 위치해야 한다.
}
```
<br>

> 예시 : 설치 임시파일 삭제

:point_right: **finally 블럭 사용 전**<br>

```java   
try {
    startInstall();
    copyFiles();
    deleteTempFiles();
} catch(Exception e){
    e.printStackTrace();
    deleteTempFiles();
} 
```
:point_right: **finally 블럭 사용 후**<br>

- try블럭과 catch 블럭에서 모두 사용되던 코드중복을 제거한다.

```java
try {
    startInstall();
    copyFiles();
} catch(Exception e){
    e.printStackTrace();
} finally {
    deleteTempFiles();
}
```

<br><br><br>


## 5. 사용자 정의 예외 만들기, 예외 되던지기

### 5-1) 사용자 정의 예외 만들기

우리가 직접 예외 클래스를 만들어서 사용할 수 있다. Exception(실제 사용자가 발생시키는 예외) 클래스 혹은 RuntimeException(프로그래머의 실수로 발생시키는 예외) 클래스를 상속받아서 만들 수 있다.<br>

- Exception(필수처리), RuntimeException(선택처리) 이므로 가능한한 RuntimeException을 상속받아서 사용하는 것이 사용에 자유롭다.
- 예외 메세지를 받는 생성자를 만드는 것이 좋다. 블럭 내에서 조상생성자를 호출하도록 해야한다.
   - `MyException(String msg)`
   - `{	super(msg);} // 조상클래스의 생성자 Exception(String msg)호출`
<br>


>  예제 : 사용자 예외 MyException 만들기

```java
// Exception(필수처리 예외)를 상속받은 클래스
// try-catch 필요하다.
class MyException extends Exception{
    
    // 에러 코드값을 저장하기 위한 필드(필수는 아니지만 에러 코드 사용해서 쓸 수도 있다.)
    private final int ERR_CODE;
	
	// String 매개변수를 가진 생성자로 에러 메세지 세팅
    MyException(String msg){
        this(msg,100);// ERR_CODE를 100(기본값)으로 초기화
    }    

    MyException(String msg, int errCode){
        super(msg);
        ERR_CODE = errCode;
    }
        
    // 에러 코드를 얻을 수 있는 메서드
    // 주로 getMessage()와 함께 사용될 것임
    public int getErrCode() {
        return ERR_CODE;
    }
}
```
<br>


### 5-2) 예외 되던지기 (exception re-throwing)

- 예외를 처리한 후에 다시 예외 발생시킴
- 호출한 메서드와 호출된 메서드 양쪽에서 모두 예외처리
<br>

> 예제 : 예외 되던지기

```java
class ExceptionTest {

    public static void main(String[] args) {
        try {
            method1(); // 5) 예외 받음
        } catch (Exception e) { // 6) 받은 예외 처리
            System.out.println("main 메서드에서 예외가 처리되었습니다.");
        }
    }

    static void method1() throws Exception { // 4) 호출된 쪽으로 예외 던지기
        try {
            throw new Exception(); // 1)예외발생
        } catch (Exception e) { // 2) 예외 처리
            System.out.println("method1 메서드에서 예외가 처리되었습니다.");
            throw e;    // 3) 다시 예외를 발생시킴
        }
    }
}
```
```java
method1 메서드에서 예외가 처리되었습니다.
main 메서드에서 예외가 처리되었습니다.
```

<br><br><br>


## 6. 연결된 예외


한 예외가 다른 예외를 발생시킬 수 있다. 예외 A가 예외 B를 발생시키면, A는 B의 원인 예외(cause Exception)이다. 이렇게 두 가지 예외를 연결하는 것을 `연결된 예외`라 부른다.<br>

#### 예외 연결을 위한 메서드

 - `Throwable initCause(Throwable cause) `: 지정한 예외(`cause`)를 원인 예외로 등록
 - `Throwable getCause()` : 원인 예외를 반환
<br>


> 예제 : 하나의 예외 안에 또다른 예외를 포함시키기

- `Throwable` : `Exception`과 `Error`의 조상
- 여기서는 Exception으로 이해하면 된다.

```java
public class Throwable implements Serializeable{
    private Throwable cause = this; // 원인 예외 저장을 위한 변수
    ...
    
    // 원인 예외 초기화 메서드
    public synchronized Throwable initCause(Throwable cause){
        ...
        this.cause = cause; // cause를 원인 예외로 등록
        return this;
    }
}
```
<br>


> 예제2 : SpaceException을 InstallException과 연결해서 사용

- 실제 발생한 예외(SpaceException)를 새로운 예외(InstallException)에 포함시키고
- 새로운 예외(InstallException)를 호출한 곳으로 던진다.

```java
void install() throws InstallException {
    try {
        startInstall(); // spaceException(저장공간부족) 발생
        copyFiles();
    } catch (SpaceException e){
        InstallException ie = new InstallException("설치중 예외 발생"); // 새로운 예외 InstallException 생성
        ie.initCause(e); // InstallException의 원인 예외로 SpaceException을 지정
        throw ie;        // InstallException을 발생시킴.
    } 
}
```
<br>
<br>


:point_right: **연결된 예외 사용이유1** <br>

**: 여러 예외를 하나로 묶어서 다루기 위함**<br>

**1. 예외 연결 전**<br>

- 발생가능한 예외가 많다면 catch블럭 수가 무한정 늘어나야 함.

```java
try{
    install();
} catch(SpaceException e){
    e.printStackTrace();
} catch(MemoryException e){
    e.printStackTrace();
} catch(Exception e){
    e.printStackTrace();
}
```

**결과**<br>

```java
SpaceException: 설치할 공간이 부족합니다.
      at ExceptionTest.install(ExceptionTest.java:22)
      at ExceptionTest.main(ExceptionTest.java:4)
```
<br>



**2. 예외 연결 후**<br>

- 발생가능한 예외를 하나의 예외에 포함시켜서 하나의 catch블럭으로 예외를 처리할 수 있다.

```java
try{
    install();
} catch(InstallException e){
    e.printStackTrace();
} catch(Exception e){
    e.printStackTrace();
}
```

**결과**<br>

- 대략적인 정보와 세부정보를 나눠서 볼 수 있음

```java
InstallException: 설치중 예외발생
      at ExceptionTest.install(ExceptionTest.java:17)
      at ExceptionTest.main(ExceptionTest.java:4)
Caused by: SpaceException: 설치할 공간이 부족합니다.
      at ExceptionTest.startInstall(ExceptionTest.java:31)
      at ExceptionTest.install(ExceptionTest.java:14)
      ... 1 more
```
<br><br>



:point_right: **연결된 예외 사용이유2** <br>

**: checked 예외를 unchecked 예외로 변경하기 위함 (필수처리예외를 선택처리예외로 변경)**<br>


> 예제 : 선택처리예외로 변경하기


**변경 전**<BR>
- SpaceException과 MemoryException은 Exception 클래스의 자손으로 checked(예외 필수처리) 예외이다. 

```java
static void startInstall() throws SpaceException, MemoryException{// Exception의 자손만 선언
    if (!enoughSpace())
       throw new SpaceException("설치할 공간이 부족합니다."); // Checked 예외
    if (!enoughMemory())
       throw new MemoryException("메모리가 부족합니다.");	// Checked 예외
}
```

**변경 후**<BR>

- RuntimeException을 생성해서 해당 예외 안에 포함시킨다.
   - 1번째 경우처럼 initCause를 사용하지 않고, RuntimeException의 생성자에 원인 예외를 넣어준다.
- MemoryException은 선택처리 예외가 되었으므로, 메서드에 선언하지 않는다.

```java
static void startInstall() throws SpaceException{ // MemoryException은 선택처리예외로 변경
    if (!enoughSpace())
       throw new SpaceException("설치할 공간이 부족합니다.");
    if (!enoughMemory())
       // 원인 예외로 등록
       throw new RuntimeException(MemoryException("메모리가 부족합니다."));
}

```

<br><br><br>


:orange_book: **References**<br>

- 자바의 정석
   - [프로그램 오류, 예외 클래스의 계층 구조](https://youtu.be/fcRapZYB29c)
   - [예외 처리하기, try-catch문의 흐름](https://youtu.be/I4XrVgCzKM4)
   - [printStackTrace, 멀티 catch블럭](https://youtu.be/81_BL9qSa9w)
   - [예외 발생시키기, 체크드/언체크드 예외](https://youtu.be/Ak7Z4jhMKRg)
   - [메서드에 예외 선언하기, finally 블럭](https://youtu.be/Px3u24AvadM)
   - [연결된 예외](https://www.youtube.com/watch?v=XmufHjSDwjA&t=978s)