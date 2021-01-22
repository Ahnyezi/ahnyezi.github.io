---
title:  "[Java] 자바의 멀티 스레드 프로그래밍"
date: 2021-01-22
categories: ['Java']
tags: ['Java']
---

**자바의 쓰레드와 멀티쓰레드 프로그래밍을 알아보자** :raising_hand:<br>
<br>

## 목차 
1. [스레드와 프로세스의 개념](#1-스레드와-프로세스의-개념)
2. [쓰레드 구현방법 - Thread 클래스와 Runnable 인터페이스](#2-쓰레드-구현방법---thread-클래스와-runnable-인터페이스)
3. [스레드의 종류](#3-스레드의-종류)
4. [멀티 스레드 프로그램의 장점](#4-멀티-스레드-프로그램의-장점)
5. [스레드의 우선순위](#5-스레드의-우선순위)
6. [스레드의 상태와 실행제어 메서드](#6-스레드의-상태와-실행제어-메서드)
7. [스레드의 동기화](#7-스레드의-동기화)

<br><br><br>

 
## 1. 스레드와 프로세스의 개념

**스레드를 공부하기 전에 몇 가지 기본지식을 알아보자.**<br>

- **프로세스** : 실행 중인 프로그램
   - 자원(메모리, CPU, 기타 컴퓨팅 디바이스)와 스레드로 구성
- **스레드** : 프로세스 내에서 실제 작업을 수행
   - 모든 프로세스는 최소한 하나의 쓰레드를 가지고 있다.
<br><br>

`프로세스 : 스레드 = 공장 : 일꾼`<br>

쉽게 비유하자면 프로세스는 공장, 스레드는 일꾼으로 비유할 수 있다.  2가지 작업을 수행하는 경우에 싱글 스레드 프로세스와 멀티 스레드 프로세스는 다음과 같다.
 
- **싱글 스레드 프로세스 = 자원 + 스레드**
   - 공장에 일꾼이 한 명인 것
- **멀티 스레드 프로세스 = 자원 + 스레드1 + 스레드2 + 스레드3........**
   - 공장에 일꾼이 여러 명인 것
   - 여러 작업을 나눠서 동시에 수행할 수 있음
   - 우리가 사용하는 대부분의 프로그램이 멀티 스레드로 작성되어 있음. 

일반적으로 여러 개의 작업을 수행하기 위해서 하나의 프로세스를 새로 생성하는 것보다 하나의 새로운 쓰레드를 생성하는 것이 더 적은 비용이 든다.<br>
 

> 현재 실행 중인 프로그램의 스레드 개수

대부분이 멀티 스레드 프로그램인 것을 알 수 있다.<br>
 
<img src="https://user-images.githubusercontent.com/62331803/105343494-0a393a00-5c25-11eb-988b-92e235960527.png" width="80%"><br>

<br><br>

**작업관리자로 확인한 것처럼 대부분의 프로그램이 멀티스레드로 작성되어 있다.**
**하지만 멀티스레드 프로그래밍이 장점만 있는 것은 아니다.**<br>

#### 장점
- 시스템 자원을 보다 효율적으로 사용할 수 있다.
- 사용자에 대한 응답성이 향상된다.
   - (예시) 채팅 프로그램 : 싱글 스레드 프로그램은 파일 전송 마칠 때까지 채팅 기능을 사용할 수 없지만, 멀티 스레드 프로그램에서는 가능하다.
- 작업이 분리되어 코드가 간결해진다.
    -  작업을 스레드 별로 수행하기 때문

#### 단점
- 시스템 자원을 여러 스레드가 공유해야 한다.
- 동기화(synchronization)에 주의해야 한다.
- 교착상태(dead-lock)가 발생하지 않도록 주의해야 한다.
   - 교착상태? 스레드A는 톱을 가졌으며, 망치가 필요하다. 스레드 B는 망치를 가졌으며 톱이 필요하다. 이 때 서로가 먼저 자원을 내놓게 기다리고 있는 상태를 교착상태라 말한다.
- 기아상태가 발생할 수 있다.
   - 기아(굶어죽는) 상태? 특정 스레드는 작업할 기회를 갖지 못하고 계속 자원을 기다리는 상태에 머무를 수 있다.
   - 따라서 각 쓰레드가 효율적으로 고르게 실행될 수 있게 해야한다.

**결론적으로 멀티 스레드 프로그래밍은 효율적이지만 고려해야할 사항이 많아서 프로그래밍이 어렵다.**<br>

<br><br>

## 2. 쓰레드 구현방법 - Thread 클래스와 Runnable 인터페이스

**쓰레드를 구현하는 방법은 2가지가 있다.**<br>
<br>

#### 방법1. Thread 클래스 상속
- Thread 클래스를 상속받은 하위클래스를 생성한다.
- run()을 오버라이딩 하여 작업할 내용을 작성한다.
```java
class MyThread extends Thread{
   public void run(){ // Thread클래스의 run() 오버라이딩
      // 작업 내용
   }
}
.
.
.
public static void main(String[] args){
   MyThread t1 = new MyThread(); // 쓰레드 생성
   t1.start(); // 쓰레드 실행
}
```
<br>

#### 방법2. Runnable 인터페이스 구현

- Runnable 인터페이스는 `public abstract void run();`만 가지고 있는 함수형 인터페이스이다. 
- 해당 인터페이스를 구현한 클래스를 선언하고 run()안에 스레드가 작업할 내용을 작성한다.
- Thread 객체는 run()이라는 메서드의 구현체(Runnable 인터페이스 구현체)를 외부에서 매개변수로 받아 사용한다.

```java
class MyThread2 implements Runnable{
   public void run(){ // Runnable 인터페이스의 추상메서드 run() 구현
      // 작업 내용
   }
}
.
.
.
public static void main(String[] args){
   Runnable r = new MyThread2();
   Thread t2 = new Thread(r); // Thread(Runnable r)
   // Thread t2 = new Thread(new MyThread2());
   t2.start();
}
```
<br>


2가지 방법 모두 스레드가 작업할 내용을 run() 메서드에 구현하는 것이다. <br>
하지만 단일상속만 가능한 자바의 특성상 1번 방법을 쓰면 다른 클래스를 상속받을 수 없게 된다. 따라서 후자가 더 유연하게 사용할 수 있는 방법이다. 


<br><br>

:point_right: **예제 :  0과 1을 각각 출력하는 작업스레드 생성 및 실행**<br>

```java
public class ThreadTest {
    public static void main(String args[]) {
        ThreadEx1 t1 = new ThreadEx1();

        Runnable r = new ThreadEx2();
        Thread t2 = new Thread(r); // 생성자: Thread(Runnable target)

        t1.start();
        t2.start();
    }
}

class ThreadEx1 extends Thread { // 1. Thread 클래스를 상속해서 쓰레드를 구현
    @Override
    public void run() { // 쓰레드가 수행할 작업을 작성
        for (int i = 0; i < 500; i++) {
            System.out.print(0);

//            System.out.println(this.getName()); // 조상인 Thread의 getName() 호출
        }
    }
}

class ThreadEx2 implements Runnable { // 2. Runnable 인터페이스를 구현해서 쓰레드를 구현
    @Override
    public void run() { // 쓰레드가 수행할 작업을 작성
        for (int i = 0; i < 500; i++) {
            System.out.print(1);

            // Thread.currentThread() : 현재 실행 중인 Thread를 반환
//            System.out.println(Thread.currentThread().getName());
        }
    }
}

```
```java
001111111111111111110011011000110000000
001111111111111111111111111111111111111
111111111111111111111111111111111111011
....
```

<br><br>

### (+) start()와 run() 더 알아보기

#### :question: 의문점1. 왜 start() 호출순서대로 실행되지 않는가?
```java
ThreadEx1 t1 = new ThreadEx1();
ThreadEx1 t2 = new ThreadEx1();

t1.start(); // t1을 첫번째로 실행
t2.start(); // t2를 두번째로 실행
```

:point_right: **start()와 동시에 해당 스레드가 실행되는 것이 아니다.**
- `start()`는 해당 스레드가 작업이 가능한 상태(RUNNABLE)이라는 것을 JVM에 알려주는 것일 뿐이며, 스레드의 실행은 OS의 스케줄러가 담당한다. 
- 따라서 어떤 스레드를 먼저 실행할지도 start()의 호출 순서가 아닌 OS의 스케줄러가 결정하는 것이다.

<br>

#### :question: 의문점2. 왜 run()을 작성했는데 start()를 호출하는가?

```java
class ThreadTest{
  public static void main(String[] args){
     MyThread1 t1 = new MyThread(); // 스레드 생성
     t1.start(); // 작업 스레드 RUNNABLE 로 전환
  }
}

class MyThread extends Thread{ // Thread 클래스 상속
   public void run(){ 
      //...
   }
}
```

**위의 코드에서 스레드의 작업과정은 다음과 같다.**<br>

<img src="https://user-images.githubusercontent.com/62331803/105485353-16d69480-5cf0-11eb-866f-ef7df4371198.png" width="85%"><br>

1. **메인 스레드**가 **th1**의 `start()` 호출
2. 새로운 호출스택을 생성
3. 새로운 호출스택에 `run()`을 push, **메인 스레드**의 호출스택에서 `start()`를 pop
4. **메인 스레드**는 메인의 호출스택, **th1**은 **th1**의 호출스택에서 작업을 수행
<br>

다음과 같은 과정을 거쳐 각각의 스레드가 서로 독립적인 작업을 수행할 수 있다.
만약 메인 스레드가 `start()`가 아닌 `run()`을 호출한다면, 메인 스레드의 호출스택에 `run()`이 push되어 **메인 스레드의 호출스택에서 작업하게 되는 것**이다. 
따라서 2개의 스레드가 병렬적으로 작업하게 하기 위해 반드시 `start()`를 호출해야 한다.


<br><br><br>

## 3. 스레드의 종류

- 스레드는 '사용자 스레드'와 '데몬 스레드' 두 종류가 있다.
   - 사용자 스레드는 일반적인 스레드이고, 데몬 스레드는 보조역할을 하는 스레드로서 사용자 스레드가 하는 역할을 보조해준다.
- 프로그램은 실행 중인 사용자 스레드가 하나도 없을 때 종료된다.
   - 데몬 스레드는 보조스레드이기 때문에 실행 중이더라도 프로그램이 종료된다.

### 3-1) 메인 스레드(main thread)

- JVM이 생성한 스레드로서, 메인 메서드의 코드를 수행하는 스레드이다.
- 메인 스레드도 사용자 스레드에 속한다.
<br>

### 3-2) 데몬 스레드(daemon thread)

- 사용자 스레드의 작업을 돕는 보조역할을 하는 스레드이다.
- 일반스레드가 모두 종료되면 자동적으로 종료된다.
- 가비지 컬렉터, 자동저장, 화면 자동갱신 등에 사용된다.
- 무한루프와 조건문을 이용해서 실행 후 대기하다가 특정조건이 만족되면 작업을 수행하고 다시 대기하도록 작성한다. 

- `boolean isDaemon()`  : 스레드가 데몬 스레드인지 확인한다. 
   - 데몬 스레드이면 true를 반환
- `void setDaemon(boolean on)`  : 스레드를 데몬 스레드로 또는 사용자 스레드로 변경. 
   - 매개변수 on을 true로 지정하면 데몬 스레드가 된다.
   - 반드시 `start()`를 호출하기 전에 실행되어야 한다. 그렇지 않으면 `IllegalThreadStateException`이 발생한다.
   - 이미 실행되면 데몬 스레드로 변경 불가


<br><br>


:point_right: **예시 : 자동저장기능을 수행하는 데몬스레드 생성하기**<br>

- 1초부터 10초까지 카운트를 센다.
- 5초가 넘어가면 자동저장기능을 시작하여, 3초마다 작업을 저장한다.
- 데몬 스레드는 무한루프지만, 메인 스레드가 종료됨과 동시에 종료된다.

```java
public class ThreadTest implements Runnable {
    static boolean autoSave = false;

   // 메인 스레드
    public static void main(String[] args) {
        Thread t = new Thread(new ThreadTest());
        t.setDaemon(true); // 이 부분이 없으면 종료되지 않는다.
                            // start 전에 세팅해야 한다.
        t.start();

        for (int i = 1; i <= 10; i++) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) { }
            System.out.println(i);

            if (i == 5) autoSave = true;
            // 5초 이후에는 자동저장 시작
        }

        System.out.println("프로그램을 종료합니다.");
    }

   // 데몬 스레드
    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(3 * 1000); // 3초마다
            } catch (InterruptedException e) { }
            // autoSave 값이 true이면 autoSave()를 호출한다.
            if (autoSave) autoSave();
        }
    }

    public void autoSave(){
        System.out.println("작업 파일이 자동저장 되었습니다.");
    }
}

```

```java
1
2
3
4
5
작업 파일이 자동저장 되었습니다.
6
7
8
작업 파일이 자동저장 되었습니다.
9
10
프로그램을 종료합니다.
```
<br><br>



## 4. 멀티 스레드 프로그램의 장점

멀티 스레드 프로그램은 다수의 작업을 여러개의 스레드가 나누어서 처리한다.
이 때 각각의 스레드가 수행되는 시간과 수행순서는 OS의 스케줄러가 결정한다.
만약 스레드A에서 스레드B로 수행권한이 넘어가게 되면, 실행할 작업에 대한 정보가 변경되어야 하고 정보를 변경하는데에는 추가적인 시간이 소요된다. 
즉, 하나의 스레드가 작업 전체를 완료하는 것보다 몇 개의 스레드로 나눠서 처리하는 것이 더 많은 시간이 소요된다는 것이다. <br>

**그렇다면 왜 멀티 스레드 프로그램을 이용하는 것일까?**<br>
<br>

### 4-1) 2가지 작업을 동시에 수행할 수 있다.

첫 번째 장점은 2가지 작업을 동시에 수행할 수 있다는 것이다.
카카오톡에서 친구와 채팅을 하면서 오늘 찍은 사진을 전송한다고 생각해보자.
카카오톡이 싱글 스레드 프로그램이라면 사진을 전송하는 시간동안 친구와 채팅할 수 없게 된다.
하지만 멀티 스레드 프로그램이라면 사진 전송 시간이 좀 더 걸리더라도 사진을 보내는 동안에도 친구와 채팅할 수 있다. 
<br>

### 4-2) I/O 블락킹이 있는 프로그램에서 효율적이다.

#### :question: I/O Blocking이란?  입출력시 스레드의 작업이 중단되는 것을 말한다. 

어떻게 효율적일 수 있는지 예시를 통해서 이해해보자.<br>
<br>

:point_right: **사용자에게 값을 입력받아 출력하고, 숫자를 카운트다운하는 2가지 기능을 수행하는 프로그램이 있다.**<br>

#### 먼저 싱글 스레드로 작성한 프로그램이다,

```java
import javax.swing.*;

public class ThreadTest {
    public static void main(String[] args) {
        // 1. 사용자로부터 입력받기
        String input = JOptionPane.showInputDialog("아무 값이나 입력하세요");
        // ----- 입력될 때까지 대기-----
        System.out.println("입력하신 값은 " + input + "입니다.");

        // 2. 카운트 다운
        for (int i = 10; i > 0; i--) {
            System.out.println(i);
            try {
                Thread.sleep(1000);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
   }
}
```

메인 스레드가 2가지 작업을 모두 수행하는 코드이다.
따라서 사용자부터 입력을 기다리는 동안 메인 스레드는 카운트다운을 시작할 수 없다. 
따라서 해당 시간동안 자원을 낭비하게 된다. 
<BR>

#### 이번엔 멀티 스레드로 프로그램을 작성해보자.

```java
import javax.swing.*;

public class ThreadTest {
    public static void main(String[] args) {
        ThreadC th1 = new ThreadC();
        th1.start();

        // 1. 사용자로부터 입력받기
        String input = JOptionPane.showInputDialog("아무 값이나 입력하세요");
        System.out.println("입력하신 값은 " + input + "입니다.");
    }
}

class ThreadC extends Thread{
    @Override
    public void run() { // 2. 카운트 다운
        for (int i = 10; i > 0; i--) {
            System.out.println(i);
            try {
                Thread.sleep(1000);
            } catch (Exception e) {
                e.printStackTrace();
            }

        }
    }
}
```

사용자에게 입력받는 작업은 메인스레드가 수행하고, 카운트다운 작업은 `th1` 스레드가 작업한다. 따라서 사용자에게 입력받는 시간을 기다릴 필요없이 카운트다운 작업을 수행할 수 있다.
<br>

**결론적으로 사용자로부터 I/O 블락킹이 발생하는 상황에서는 멀티 스레드 프로그램이 소요시간이 적다.  즉, I/O 때문에 지연되는 시간동안 다른 작업을 할 수 있기 때문에, 자원을 보다 효율적으로 사용할 수 있다.**<br>

<br><br>

## 5. 스레드의 우선순위

**프로그램 작성시 어떤 작업을 다른 작업보다 더 우선적으로 처리해야 할 경우, 우선순위를 줄 수 있다.**<br>

```java
public static final int MAX_PRIOIRITY = 10; // 최대우선순위
public static final int MIN_PRIOIRITY = 1; // 최소우선순위
public static final int NORM_PRIOIRITY = 5; // 보통우선순위

void setPriority(int newPriority) // 스레드의 우선순위를 지정한 값으로 변경

int getPriority() // 스레드의 우선순위를 반환
```

- 자바의 우선순위는 1부터~10까지 존재하며, 기본은 5이다.
- setPriority()를 이용해서 우선순위를 변경할 수 있다.
   - 스레드 실행 중에도 우선순위 바꿀 수 있음.
- getPriority()를 이용해서 우선순위를 확인할 수 있다.

<br><br>


**작업스레드 A와 B가 있다.  'A의 우선순위 >  B의 우선순위'인 경우를 생각해보자.**<br>

- **실제 작업 수행결과와 우선순위가 다를 수 있다.**
   - 이론적으로는 우선 순위가 높으면 더 많은 시간을 할당받기 때문에, 우선순위가 높은 A가 더 빨리 끝나게 된다.
   - 하지만, 우선순위는 JVM에서 정하는 것(희망사항)이고 실제 스레드를 시작하게 하는 것은 OS의 스케줄러이다. 
   - 스케줄러는 해당 OS에서 돌아가는 모든 프로세스와 스레드가 잘 수행되도록 하기 때문에 JVM의 희망사항은 참고만 한다.
   - 따라서 실제 작업 수행결과와 우선순위가 다를 수 있다.
- **우선 순위는 상대적이다.**
   - A, B스레드의 우선순위를 7,5로 설정한 것과, 3,1으로 설정한 것은 비슷한 효과를 낸다.
   - 만약 작업 스레드 자체의 우선순위를 다른 프로그램에 비해 낮추고 싶다면, 작업관리자에 들어가서 IDE 자체의 우선순위를 다르게 설정할 수 있다. 이 설정결과에 따라 OS가 처리하는 우선순위가 달라진다.
<br><br>

:point_right: **예제 : 스레드 우선순위 설정해보기**<br>

- `th1`의 우선순위는 기본인 5로 두고 `th2`의 우선순위를 9로 설정한다.
- 우선순위를 더 높게 설정한 `th2`의 작업이 일찍 끝날 확률이 높다. 
- 하지만 실제로는 OS의 스케줄러가 스레드 실행을 관리하기 때문에 반드시 우선순위대로 작업한다고 할 수 없다.

```java
package javabasic.week10;

public class ThreadTest {

    public static void main(String[] args) {
        ThreadEx1 th1 = new ThreadEx1();
        ThreadEx2 th2 = new ThreadEx2();

        th2.setPriority(9); // th2의 우선순위 9로 변경

        System.out.println("Prioirity of th1(-) : " + th1.getPriority());
        System.out.println("Prioirity of th2(|) : " + th2.getPriority());
        th1.start();
        th2.start();
    }
}

class ThreadEx1 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("-");
            for (int j = 0; j < 100_000_000; j++) ; // 시간 지연용 for문
        }
    }
}

class ThreadEx2 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("|");
            for (int j = 0; j < 100_000_000; j++) ; // 시간 지연용 for문
        }
    }
}

```
```java
Prioirity of th1(-) : 5
Prioirity of th2(|) : 9
||-||||||-|||||||||||||||||||||||||||||||||||||||
|||||||||||||||||||||||||||||||||||||||
|||||||||||||||||||||||||||||||||||||||
|||||||||||||||||||||||||||||||||||||||
|||||------------------......
```

<br><br><br>


##  6. 스레드의 상태와 실행제어 메서드

:point_right: **스레드가 가질 수 있는 상태는 다음과 같다.**<br>

|상태|설명|
|:---:|:---:|
|NEW|스레드가 생성되고 아직 start()가 호출되지 않은 상태|
|RUNNABLE|실행 중 또는 실행 가능한 상태|
|BLOCKED|동기화블럭에 의해서 일시정지된 상태(lock이 풀릴 때까지 기다리는 상태)|
|WAITING, TIMED_WAITING|스레드의 작업이 종료되지는 않았지만, 실행가능하지 않은(unrunnable) 일시정지 상태, TIMED_WAITING은 일시정지시간이 지정된 경우를 의미|
|TERMINATED|스레드의 작업이 종료된 상태|

<br>

**1. NEW :  스레드를 생성한 상태**<br>

- Thread 클래스 상속 클래스나 Runnable 인터페이스 구현 클래스로 작업 스레드를 만들면 **NEW**상태가 된다.

**2.  RUNNABLE**<br>

- 해당 작업 스레드에 대해서 start()를 호출하면 **RUNNABLE** 상태가 된다.
- 실행 중이거나 실행대기인 상태를 말한다.
- **RUNNABLE**상태가 된 스레드는 실행대기줄에서 자신의 차례를 기다리다가 자신의 차례가 되면 실행된다.
- 주어진 시간이 끝나면 다시 뒤로 가서 줄을 선다.

**3. TERMINATED**<br>

- 해야할 작업이 끝나거나 `stop()`이 호출되면 소멸(terminated)한다.

**4. WAITING**<br>

- `join()`, `sleep()`, `suspend()`에 의해 실행이 중지된 스레드가 대기하는 상태이다.

<br><br>


:point_right: **스레드의 실행제어 메서드는 다음과 같다.**<br>

작업 스레드의 상태를 적절히 변경하여 보다 효율적인 프로그램을 만들 수 있다.


- **Sleep** : 지정된 시간동안 스레드를 잠들게 함
   - `static void sleep(long mills)`
   - `static void sleep(long mills, int nanos)`
- **Join** : 다른 스레드를 기다림
   - `void join()`
   - `void join(long mills)`
   - `void join(long mills, int nanos)`

- **Interrupt** : 잠들어 있거나(`sleep()`) 기다리고 있는( `join()`) 스레드를 깨움
   - `void interrupt()`
- **stop** : 스레드를 즉시 종료시킨다.
   - `void stop()` 
- **suspend** : 작업 중인 스레드를 일시정지시킨다
   - `void suspend()`
- **resume** : 일시정지시킨 스레드를 재개한다.
   - `void resume()`
- **yield** : 자신에게 주어진 시간을 다른 스레드에게 양보한다.
   - `static void yield()`

**:exclamation:  static 메서드인 sleep()과 yield()는 자기자신 스레드에게만 적용할 수 있다.**<br>


<br><br><br>


## 7. 스레드의 동기화

**멀티 스레드 프로세스에서는 다른 스레드의 작업에 영향을 미칠 수 있다. 하나의 프로세스에 할당된 자원을 여러 스레드가 공유하기 때문이다. 따라서 진행중인 작업이 다른 스레드에게 간섭받지 않게 하려면 '동기화'가 필요하다.**<br>

:pencil2: **동기화? 한 스레드가 진행중인 작업을 다른 스레드가 간섭하지 못하게 막는 것**<br>

#### 동기화 방법
- 동기화를 위해서 **간섭받지 않아야 하는 문장들을 하나의 영역(임계 영역)으로 묶는다.** 
- 임계 영역 내의 문장들을 처음부터 끝까지 실행하기 전에는 다른 스레드가 해당 영역에 들어올 수 없다. 
- **임계영역은 락(lock)을 얻은 단 하나의 스레드만 출입가능**하며, 객체 1개에 락 1개가 존재한다. 

<br><br>

### 7-1) synchronized를 이용한 동기화

`synchronized`**로 임계영역(lock이 걸리는 영역)을 설정하는 방법은 2가지가 있다.**<br>
<br>

**1. 메서드 전체를 임계 영역으로 지정**<br>

```java
// 임계 영역(critical section)
public synchronized void calcSum(){
   
}
// 
```
예시
```java
    public synchronized void withdraw(int money) {
        if (balance >= money) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
            }

            balance -= money;
        }
    }
```
<br>

**2. 특정한 영역을 임계 영역으로 지정**<br>

한 스레드만 한 임계영역 내에서 작업 할 수 있기 때문에, 임계영역이 커지면 성능이 떨어질 것이다.
따라서 1번 방법을 사용하면 비효율적인 코드가 될 수 있다. 

```java
// 임계 영역(critical section)
synchronized (객체의 참조변수){
}
//
```
예시

```java
    public void withdraw(int money) {
        synchronized (this) {
            if (balance >= money) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                }

                balance -= money;
            }
        }// synchronized(this)
    }
```
<br><br>


:point_right: **동기화 예제 : 출금 기능 구현하기**<br>

- 출금 메서드가 동기화 되어 있지 않다면, 스레드1의 출금 작업과 스레드2의 출금작업이 섞이게 되서 balance가 음수가 될 수 있다. 
- 이를 예방하기 위해 출금 기능을 임계영역으로 설정해두고, 스레드1의 출금이 종료되기 전에는 스레드2가 출금 메서드에 접근할 수 없도록 한다.

```java
public class ThreadTest {
    public static void main(String[] args) {
        Runnable r = new RunnableEx1();
        new Thread(r).start(); 
        new Thread(r).start(); 
    }
}

class Account{
    private int balance = 1000; // private으로 해야 동기화가 의미있다.

    public synchronized int getBalance(){
        return balance;
    }

//    public void withdraw(int money){ // 동기화 안함. balance가 음수로 떨어질 수 있음
    public synchronized void withdraw(int money){ // 동기화 함.
        if (balance >= money){
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) { }
            balance -= money;
        }
    }// withdraw
}

class RunnableEx1 implements Runnable{
    Account acc = new Account();

    @Override
    public void run() {
        while(acc.getBalance() > 0){
            // 100,200,300 중 한 값으로 임의로 선택해서 출금(withdraw)
            int money = (int) (Math.random() * 3 + 1) * 100;
            acc.withdraw(money);
            System.out.println("balance:"+acc.getBalance());
        }
    }
}

```

**(동기화 전) 출금작업 도중 다른 스레드의 방해를 받기 때문에, 남은 잔액이 음수 나올 수 있음**
```java
balance:700
balance:400
balance:300
balance:100
balance:100
balance:100
balance:-100
```
**(동기화 후) A가 출금하는 동안 다른 스레드는 임계영역에 접근할 수 없기 때문에 남은 잔액이 음수가 나올 수 없음**
```java
balance:900
balance:600
balance:300
balance:100
balance:100
balance:0
balance:0
```



<br><br><br>

:orange_book: **References**<br>
- 자바의 정석
   - [쓰레드](https://www.youtube.com/watch?v=kNNHaAaFDs8&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=147)
   - [쓰레드의 구현과 실행](https://www.youtube.com/watch?v=P1zDndoy4pI&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=148)
   - [싱글 쓰레드와 멀티 쓰레드](https://www.youtube.com/watch?v=2eJc9ZdRe2c&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=149)
   - [쓰레드의 우선순위](https://www.youtube.com/watch?v=KWZjeW6zkyY&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=150)
   - [데몬쓰레드](https://www.youtube.com/watch?v=W0v3Gwx92hc&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=151)
   - [쓰레드의 동기화](https://www.youtube.com/watch?v=g4vP5wuAoPI&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=155)