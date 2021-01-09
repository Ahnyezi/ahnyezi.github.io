---
title:  "[Java] 자바의 인터페이스"
date: 2021-01-08
categories: ['Java']
tags: ['Java']
---

**자바의 인터페이스를 알아보자** :raising_hand:<br>
<br>

## 목차 
### 인터페이스
1. [인터페이스의 선언](#1-인터페이스의-선언)
2. [인터페이스의 상속](#2-인터페이스의-상속)
3. [인터페이스의 구현](#3-인터페이스의-구현)
4. [인터페이스를 활용한 다형성](#4-인터페이스를-활용한-다형성)
5. [인터페이스의 역할(의미)](#5-인터페이스의-역할의미)
6. [default 메서드 - 자바8](#6-인터페이스의-default-메서드---자바8)
7. [static 메서드 - 자바8](#7-인터페이스의-static-메서드---자바8)
8. [private 메서드 - 자바9](#8-인터페이스의-private-메서드---자바9) 

<br><br><br>


# 자바의 인터페이스

## 1. 인터페이스의 선언

자바 어플리케이션에서 사용되는 인터페이스는 **추상 메서드의 집합**이라고 설명할 수 있다. 사실상 추상메서드 이외에도 static 메서드, final이 붙은 상수, 자바 1.8부터 사용가능한 default 메서드를 포함할 수 있다. 하지만 이러한 것들은 인터페이스의 본질에서 벗어난 추가적인 기능이기 때문에, 인터페이스는 추상 메서드의 집합으로 이해해도 문제는 없다. 인터페이스의 본질과 이론적 의미에 대해서는 뒤에서 알아보겠다. <br>

- 구현된 것이 없는 설계도(껍데기)
- 모든 멤버가 public
- public, abstract, static, final 등의 키워드는 생략 가능. 언제나 그렇기 때문에.

> 인터페이스의 선언

```java
interface 인터페이스이름{
	(public) (static) (final) 타입 상수이름 = 값;	// 상수
	(public) (abstract) 메서드이름(매개변수목록);	// 추상메서드
}
```
<br>

:question: **인터페이스와 추상클래스의 차이**<br>
- **추상클래스 : 추상 메서드를 포함하는 일반 클래스**
   - 생성자, 인스턴스 변수 등을 멤버로 가질 수 있다.

```java
abstract class Player{
	boolean pause;	// 인스턴스 변수
	
	Player(){}		// 생성자

	abstract void stop(); // 추상메서드

	void play(){} 		  // 인스턴스 메서드
}
```

- **인터페이스 : 추상 메서드만으로 이루어진 집합**
   - 자바8부터 인터페이스도 일반 메서드를 가질 수 있도록 변경되었지만, 이것은 인터페이스의 본질과는 거리가 있음.

```java
interface Fightable{
	public static final int x = 10; // 상수	
	void move(int x, int y);		// 추상메서드
}
```
<br>
<br>



## 2. 인터페이스의 상속

자바는 클래스의 다중상속이 금지되어 있다. 부모들의 메서드가 자손에서 충돌나는 문제를 방지하기 위해서이다. 하지만 **인터페이스에 존재하는 추상메서드는 선언부(head)만 존재하기 때문에 충돌날 가능성이 없다. 따라서 다중상속(조상이 여러개인 형태)이 가능하다.**<br> 

- 인터페이스의 조상은 인터페이스만 가능(Object가 최고 조상 x)
<br>

> 추상메서드를 1개씩 가지고 있는 인터페이스 Movable과 Attackable

```java
interface Movable {
	// 지정된 위치(x,y)로 이동하는 메서드
	void move(int x, int y);
}

interface Attackable{
	// 지정된 대상(u)를 공격하는 메서드
	void attack(Unit u);
}
```

> Movable과 Attackable 인터페이스를 다중 상속 받은 Fightable 인터페이스

```java
interface Fightable extends Movable, Attackable{ }
// Fightable 인터페이스는 자동으로 멤버를 2개 가지게 됨 (move, attack)
```
<br>
<br>


## 3. 인터페이스의 구현

**인터페이스**는 추상클래스의 집합으로 **미완성된 설계도**라 생각하면 된다. <br>
이를 완성시키는 것이 `인터페이스의 구현(implements)`이다. 
<br>

> 인터페이스 구현 키워드

- 인터페이스를 구현하기 위해서는, 정의된 모든 추상메서드를 구현해야 한다. 
- 일부 추상메서드만 구현할 경우 해당 클래스는 abstract class가 된다. 

```java
class 클래스이름 implements 인터페이스이름{
	// 인터페이스에 정의된 추상메서드를 모두 구현해야 한다.
}
```
<br>

> Fightable 인터페이스 구현

```java
interface Fightable{
	// public abstract 생략된 형태
	void move(int x, int y);
	void attack(Unit u);
}

// 모두 구현
class Fighter implements Fightable{
	public void move(int x,int y){}
	public void attack(Unit u){}
}

// 일부만 구현(추상클래스)
abstract class Fighter implements Fightable{
	public void move(int x, int y){ /* 구현 내용*/}
	// public abstract void attack(Unit u); 가 생략된 형태
}
```
<br>

:bulb: **주의할 점**<br>

- 인터페이스의 모든 메서드는 `public` 접근제어를 가진다.
- 구현 클래스에서 메서드를 오버라이딩 할 때, 오버라이딩 메서드 접근 제어 범위가 조상보다 좁아서는 안된다. 

```java
interface Fightable { // 인터페이스의 모든 메서드는 public abstract이다.
	void move(int x, int y); // public abstract가 생략됨
}

class Fighter implements Fightable{
	// 오버라이딩 규칙: 조상(public)보다 접근제어자 범위가 좁으면 안된다.

	public void move(int x, int y){   // public 안쓰면 default가 되므로 컴파일 에러 발생
		System.out.println("이동");
	}
}
```
<br>
<br>



## 4. 인터페이스를 활용한 다형성

**다형성은 조상 타입의 참조변수가 자손타입의 인스턴스를 가리키는 형태를 말했다.**<br>
> 다형성의 예시

```java
// 부모타입(Tv)으로 자손타입(SmartTv) 인스턴스를 가리킴
Tv t = new SmartTv();
```
<br>

:point_right: **인터페이스도 다형성이 성립하기 때문에, 인터페이스 타입으로 자손 타입의 인스턴스를 가리킬 수 있다.**<br> 

```java
interface Fightable{
	void move(int x, int y);
	void attack(Fightable f);
}

class Fighter implements Fightable{
	//추상클래스를 구현한 메서드
	public void move(int x, int y){}
	public void attack(Fightable f){}

             // 자손 클래스에서 추가한 인스턴스 메서드
	public void skill(){} 
}
```

**(1)  인터페이스 타입으로 자손 타입의 인스턴스를 가리킬 수 있다.**<br> 

- 부모 타입(Fightable)의 참조변수를 사용하면, 자손 타입(Fighter)에 얼마나 많은 멤버가 있든 부모의 멤버만 사용 가능

```java
// 인터페이스 타입으로 구현클래스 인스턴스를 가리킴
Fightable f = new Fighter();

f.move(100,200);
f.attack(new Fighter());

// 자손클래스에만 존재하는 메서드 호출시 컴파일 에러
// f.skill() 
```
<br>

**(2) 매개변수 타입을 인터페이스로 둘 수도 있다.**<br>

 - 이럴 경우, 해당 인터페이스를 구현한 클래스 인스턴스만 들어올 수 있다.

```java
interface Fightable{
   // Fightable 인터페이스 구현클래스 인스턴스만 들어올 수 있음
   void attack(Fightable f);		
}
```
<br>

**(2) 인터페이스를 메서드 리턴타입으로 지정할 수 있다.**<br>

- 메서드의 반환타입이 인터페이스라면, 해당 인터페이스를 구현한 클래스 타입의 인스턴스를 반환한다.
- 코드의 유연성을 높이기 위해서 사용한다. 
   - `Fightable`을 구현한 다른 클래스가 있다면, new 뒤 부분만 수정해서 같은 코드를 사용할 수 있음.

```java
class Fighter implements Fightable {
    @Override
    public void attack(Fightable f) {}
}

class B {
    public Fightable testMethod() {
        Fighter f = new Fighter();  // 인터페이스 구현클래스의 인스턴스
        return f;            // return (Fightable) f; 자동으로 부모로 형변환(업캐스팅)
    }
}

class C {
    void Test() {
        B b = new B();
        Fightable f = b.testMethod(); // 인터페이스(부모) 타입으로 저장
    }
}
```
<br>
<br>


## 5. 인터페이스의 역할(의미)

**앞에서 인터페이스를 선언하고 구현하는 방법을 알아보았다. 그럼 인터페이스를 왜 사용하는 것일까?**<br>
**인터페이스가 하는 역할과 의미에 대해 알아보자.**<br> 
<br> 

**(1) 두 대상(객체)의 '중간 역할'**<br>
- 예시1 : 사용자 - 기계의 껍데기 - 자판기
- 예시2:  사용자 - GUI - 컴퓨터
   -  컴퓨터를 직접 다루기 힘들기 때문에 그래픽 화면을 통해서 좀 더 쉽게 연결, 대화, 소통을 돕는 중간역할. 
   - 컴퓨터 내부가 바뀌어도 껍데기가 안바뀌면 사용자는 영향을 받지 않음
   - 따라서 변경에 유리. 
   - 연결, 대화, 소통에 도움.
<br><br> 

**(2) 선언(설계)와 구현의 분리**
   - 껍데기와 알맹이가 붙어있던 형태를, 인터페이스를 사용하여 인터페이스(껍데기) 클래스(알맹이)로 분리
   - 전자는 유연하지 않고 변경에 불리. 
   - 후자는 껍데기와 알맹이가 분리되어 있기 때문에 알맹이(클래스)를 다른 것으로 바꾸기 쉬움 => 유연한 코드

> 선언과 구현 동시에

```java
class B {
    public void method() {
        System.out.println("methodInB");
    }
}
```
> 선언과 구현 분리

```java
// 선언(설계)
interface I {
    public void method();
}

// 구현
class B implements I {
    public void method() {
        System.out.println("methodInB");
    }
}
```


### :question: 강한 결합과 느슨한 결합

<img src="https://user-images.githubusercontent.com/62331803/104047821-9dd23a00-5225-11eb-9f89-d24354bcbd7a.png" width="70%"><br>

**왼쪽의 그림(강한 결합)부터 살펴보자.**<br>

- A는 B에 의존하고 있다. (A가 B를 사용)
- 이 때, A가 C를 사용하게 하려면?
- A는 B를 의존하고 있는 코드를 C를 의존하게끔 변경해야 한다. (강한 결합)

**이번엔 오른쪽 그림(느슨한 결합)을 살펴보자.**<br>
-  A는 I 인터페이스에 의존하고 있고, I 인터페이스를 구현한 B를 사용한다.
- 이 때, A가 C를 사용하게 하려면?
- A는 I에 의존하고 있기 때문에, I 인터페이스를 구현한 C를 사용한다면 따로 코드를 변경하지 않아도 된다. (느슨한 결합)
<br>

`강한 결합` : 빠르지만 변경에 불리<br>
`느슨한 결합` : 느리지만 유연하고 변경에 유리<br>

:point_right: **[강한결합] 직접적인 관계의 두 클래스 (A -> B)**<BR>

```java
class A {
    public void methodA(B b) { // B를 사용!!(따라서 B와 관계 있음)
        b.methodB();
    }
}

class B {
    public void methodB() {
        System.out.println("methodB()");
    }
}

class InterfaceTest {
    public static void main(String args[]) {
        A a = new A();
        a.methodA(new B());
    }
}
```
:point_right: **[느슨한결합] 간접적인 관계의 두 클래스 (A -> I -> B)**<BR>

- `methodB()`를 추상 메서드로 갖는 인터페이스 작성
- 해당 인터페이스를 구현한 클래스 생성
- 인터페이스 타입을 매개변수로 사용해서 다형성을 구현

```java
class A {
    public void methodA(I i) {// I를 사용! (따라서 A는 B클래스와 관계 없음.I 인터페이스랑만 관계 있음)
        i.methodB();
    }
}

// 껍데기
interface I {
    public abstract void methodB();
}

// 알맹이
class B implements I {
    public void methodB() {
        System.out.println("methodB()");
    }
}

// 나중에 B를 C로 변경하여도 C만 변경하면 됨. methodB를 호출하는 A를 변경할 필요 없음
class c implements I {
    public void methodB() {
        System.out.println("methodB() in C");
    }
}
```

즉 A가 B의 메서드를 호출하는 형태였다가 C의 메서드를 호출하게 바뀐다면, **강한결합 형태**는 **A가 B를 직접 의존하기 때문에, A의 내부를 변경**해줘야 한다. 하지만 인터페이스를 사용한 **느슨한 결합**은 **A가 I를 거쳐 B를 의존하기 때문에 A 내부를 변경해주지 않아도 된다**. <BR>
<BR>

**(3) 개발시간 단축**<br>

강한결합 형태는 A가 B를 직접 의존하기 때문에, B가 완성된 후에 A를 개발할 수 있다.
하지만 느슨한결합 형태는 B가 완성되지 않아도 껍데기인 I를 이용해서 A를 개발할 수 있다. 
A에서 I의 추상메서드를 호출할 수 있기 때문에, 메서드가 완성되었다고 가정하고 개발하는 것이다. 
- 인스턴스 변수는 메서드를 이용해서 접근. (캡슐화된 형태에서 setter와 getter를 이용해 데이터 전달)
<br><br> 

**(4) 표준화**<br>

표준화가 가능하다. 대표적인 예로 JDBC(Java Database Connectivity) API(Application Programming Interface)가 있다. <br>

<img src="https://user-images.githubusercontent.com/62331803/104070712-dc2e2000-524a-11eb-995f-ecc800d48438.png" width="70%"><br>

과거에 DB를 사용해서 자바 어플리케이션을 개발하면, 사용하는 DB에 따라서 코드가 달라졌다. 오라클 DB를 사용한다면 오라클에 맞는 자바 코드를 짜야했고, 다른 DB로 바꿀 경우 코드를 다 변경해야 하는 문제가 있었다. 즉, A가 B에 의존적일 때 B에서 C로 바뀌면 A도 많이 바꿧어야 하는 형태였다.<BR>

이러한 문제를 해결하기 위해 중간에 JDBC라는 인터페이스 집합(껍데기)을 두기로 했다. JDBC 인터페이스를 각 DB 회사들에게 제공하고, 회사들은 해당 인터페이스에 맞춰서 자사의 서비스를 개발하였다. 이렇게 된다면 자바 어플리케이션을 개발하는 회사는 해당 인터페이스에 맞게 개발하면되기 때문에, JDBC 인터페이스 자체가 변경되지 않는 이상 다양한 종류의 DB를 코드 수정없이 사용할 수 있게 되었다.<BR>
<BR><br> 

**(5) 관계 없는 클래스들 관계생성**<BR>

다음과 같은 상속관계를 가지는 클래스들이 있다. <br>

<img src="https://user-images.githubusercontent.com/62331803/104069873-f2d37780-5248-11eb-8a97-341d060eb27b.png" width="70%"><br>

`SCV`, `Tank`, `Dropshop` 클래스에 수리를 위한 메서드를 추가하려 한다. 

**1. 첫번째 방법 : 메서드 오버로딩(수리가 필요한 클래스를 매개변수로 설정)**<br>

```java
void repair(SCV s){}
void repair(Tank t){}
void repair(Dropship d){}
```
 - 비효율적이고 반복되는 코드 여럿 생김


**2. 두번째 방법 :  (다형성 이용) `SCV`, `Tank`의 부모인 `GroundUnit` 을 repair의 매개변수 타입으로 설정**<br>

```java
void repair(GroundUnit gu){}
```

- `Marine` 클래스는 불필요한 repair 메서드를 가지게 됨.


**3. 세번째 방법 : (인터페이스 이용) 빈 인터페이스를 작성하여, `SCV`, `Tank`, `Dropshop`가 해당 인터페이스를 implements하게 한다.**<br>

```java
interface Repairable{}

class SCV extends GroundUnit implements Repairable{}
class Tank extends GroundUnit implements Repairable{}
class Dropship extends AirUnit implements Repairable{}

...
void repair(Repairable r){} // 인터페이스를 매개변수로 하여, Repairable 구현클래스만 올 수 있게함
```

- repairable 인터페이스는 아무런 내용도 없지만 이를 구현한 클래스들에 공통점이 생김.
   - 즉, 서로 관계 없는 클래스들의 관계를 맺어줌.
- 인터페이스를 `repair` 메서드의 매개변수로 설정
   - 해당 인터페이스 구현 클래스만 `repair` 메서드를 사용할 수 있게함. 
<br>
<br><br> 

## 6. 인터페이스의 default 메서드 - 자바8

자바8 이전에 인터페이스가 가질 수 있는 메서드는 추상메서드뿐이었다. 따라서 해당 인터페이스를 구현한 클래스에서 메서드의 body를 완성해서 쓸 수 있었다. <br>

이 때 발생하는 문제점은 인터페이스의 구현 클래스들이 메서드 구현 코드를 작성할 때, 그 내용이 일치하더라도 클래스마다 body를 새롭게 적어줘야 한다는 것이었다. <br>

이 문제를 해결하기 위해, 자바8부터 **default 메서드**를 도입하게 되었다. default 메서드는 인터페이스 내부에 존재할 수 있는 구현메서드이다. 인터페이스를 implements 하면 메서드 구현없이 바로 사용할 수 있다. <br>

:point_right: **인터페이스에 디폴트 메서드 선언하고, 구현클래스에서 호출하기**

```java
interface TestInterface {
    // 이미 구현된 default 메서드
    default void show() {
        System.out.println("디폴트 메서드 실행");
    }
}

class TestClass implements TestInterface {
    public static void main(String args[]) {
        TestClass d = new TestClass();
        d.show(); // 디폴트 메서드 호출
    }
}
```
```java
디폴트 메서드 실행
```
<br><br> 


## 7. 인터페이스의 static 메서드 - 자바8

인터페이스의 static 메서드는 인터페이스 내에서 이미 body를 구현한 메서드이다(default 메서드와 동일). 하지만 구현 클래스에서 오버라이딩하여 사용할 수 없다. 

```java
interface NewInterface {  
    // static 메서드  
  static void hello() {  
        System.out.println("static 메서드 실행");  
  }  
  
    // 추상메서드  
  void overrideMethod(String str);  
}  
  
public class InterfaceDemo implements NewInterface {  
    // Implementing interface method  
  @Override  
  public void overrideMethod(String str) {  
        System.out.println("추상메서드 구현>>" + str);  
  }  
  
    public static void main(String[] args) {  
        InterfaceDemo id = new InterfaceDemo();  
  NewInterface.hello();  
  id.overrideMethod("구현 메서드 실행");  
  }  
}
```
```java
static 메서드 실행
추상메서드 구현>>구현 메서드 실행
```
<br><br> 

## 8. 인터페이스의 private 메서드 - 자바9

자바9부터 인터페이스 내에 private 메서드를 가질 수 있게 되었다. <br>


:point_right: **인터페이스에서 사용 가능한 멤버**<br>
1. 상수
2. abstract 메서드
3. default 메서드
4. static 메서드
5. private 메서드
6. private static 메서드
<br><br> 

private 메서드는 오직 해당 인터페이스 내에서만 접근 가능하며, 인터페이스를 상속받은 클래스나 서브 인터페이스에서는 접근할 수 없다. <br>

> 인터페이스 메서드 종류별 실행

```java

public interface TestInterface {
    void mul(int a, int b);

    default void add(int a, int b) {
        System.out.print("default 메서드 => ");
        System.out.println(a + b);
    }

    static void mod(int a, int b) {
        System.out.print("static 메서드 => ");
        System.out.println(a % b);
    }

    private void sub(int a, int b) {
        System.out.print("private 메서드 => ");
        System.out.println(a - b);
    }

    private static void div(int a, int b) {
        System.out.print("private static 메서드 => ");
        System.out.println(a / b);
    }
}

class Test implements TestInterface {
    @Override
    public void mul(int a, int b) {
        System.out.print("abstract 메서드 => ");
        System.out.println(a * b);
    }

    public static void main(String[] args) {
        Test t = new t();
        t.mul(2, 3);
        t.add(6, 2);
        Test.mod(5, 3);
    }
}
```
```java
abstract 메서드 => 5
default 메서드 => 8
static 메서드 => 2
```

<br>
<br>

:orange_book: **References**<br>

- [자바의 정석- 기초편](https://www.youtube.com/watch?v=vW1PylkVGuM)
- GeeksforGeeks
   - [Default Methods in Java 8](https://www.geeksforgeeks.org/default-methods-java/)
   - [Static method in Interface in Java](https://www.geeksforgeeks.org/static-method-in-interface-in-java/)
   - [Private Methods in Java 9 Interfaces](https://www.geeksforgeeks.org/private-methods-java-9-interfaces/)