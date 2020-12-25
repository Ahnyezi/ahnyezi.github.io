---
title:  "[Java] 자바의 다형성"
date: 2020-12-26
categories: ['Java']
tags: ['Java']
---

**자바의 다형성과 관련된 개념을 알아보자** :raising_hand:<br>
<br>

## 목차 
### 2. 다형성
- 2.1. [오버라이딩](#21-오버라이딩overriding)
- 2.2. [다이나믹 메서드 디스패치](#22-동적-메서드-디스패치dynamic-method-dispatch)
- 2.3. [더블 디스패치](#)

<br><br><br>



# 2. 다형성(Polymorphism)

동일한 부모로부터 태어난 자식이라도 외모, 성격, 취향 등이 서로 다른 것 처럼, 동일한 부모 클래스로부터 상속받은 하위 클래스들을 각기 다르게 수정하여 사용할 수 있다.<br>

**다형성**은 상속받은 기본 형질에 서로 다른 변화를 주어 다양한 형태를 표현하는 것이다. **자바의 다형성**은 `메서드 오버라이딩`, `업캐스팅`, `다운캐스팅`,  `추상 클래스`, `인터페이스` 등을 통해 구현할 수 있다.<br>
<br>


## 2.1. 오버라이딩(Overriding)

**객체지향언어에서의 오버라이딩**은 자식클래스가 부모클래스에서 이미 선언된 메서드를 구현해서 사용할 수 있게 하는 기능이다. 
- 오버라이딩이란 부모클래스의 메서드와 자식 클래스의 메서드가 `메서드 이름`, `메서드 파라미터`, `리턴타입`이 모두 같은 경우를 일컫는다.<br>
<br>

<img src="https://user-images.githubusercontent.com/62331803/103136284-6a11f300-4702-11eb-9fc4-a226bf9fd17f.png" width="60%"><br>

- 메서드 오버라이딩은 런타임시 다형성을 구현하는 방법 중 하나이다. 
- 어떤 클래스의 인스턴스가 attack() 메서드를 실행시키는지에 따라서 실행되는 메서드가 달라진다.
   - 부모클래스인 `Pocketmon`클래스의 인스턴스로 `attack()`을 호출한다면, `Pocketmon의 attack()`이 실행
   - 자식클래스인 `Pikachu`클래스의 인스턴스로 attack()을 호출한다면 `Pikachu의 attack()`이 실행 <br>
<br>

### 1) 메서드 오버라이딩의 규칙

**1-1) 접근제어자에 따른 오버라이딩 가능여부**<br>

|접근제어자|가능여부|
|:---:|:---:|
|public|O|
|protected|O|
|private|X|

- `private` 메서드는 오버라이딩할 수 없다.
- 따라서 부모클래스의 메서드와 동일한 `메서드명`, `파라미터`, `리턴타입`으로 자식클래스에 메서드를 생성할 경우, 해당 메서드는 자식클래스의 추가적인 메서드로서 작동한다. <br>

> private 메서드 오버라이딩 테스트

```java
public class Test {
    
    static class Parent {
        // private 메서드는 오버라이딩 될 수 없다. 
        private void m1(){
            System.out.println("From parent m1()");
        }
        protected void m2(){
            System.out.println("From parent m2()");
        }
    }

    static class Child extends Parent {
        // 새로운 m1() 메서드
        // Parent의 m1()이 오버라이딩 된 것이 아니라
        // Child 클래스만이 가진 메서드
        private void m1() {
            System.out.println("From child m1()");
        }

        // 오버라이딩 메서드
        @Override
        public void m2() {
            System.out.println("From child m2()");
        }
    }

    public static void main(String[] args) {
        Parent obj1 = new Parent();
        obj1.m2();
        Parent obj2 = new Child();
        obj2.m2();
    }
}
```
```java
From parent m2()
From child m2()
```
<br>

**1-2). final 메서드는 오버라이딩 될 수 없다.**<br>
- 메서드에 final키워드를 붙여 자식 클래스에게 오버라이든될 수 없게 설정할 수 있다.

```java
class Parent {
    // final 메서드는 오버라이든 불가
    final void display() {}
}

class Child extends Parent {
    // 컴파일 에러 발생
    void display() {}
}
```
```java
13: error: display() in Child cannot override display() in Parent
    void display() {  }
         ^
  overridden method is final
```
<br>

**1-3) static 메서드는 Method Overriding이 아니라 Method Hiding(은닉)을 수행한다.**<br>

- 부모클래스에서 정의된 static 메서드를 자식클래스에서 동일하게 static으로 재정의하는 것을 `Method hiding`이라 일컫는다.
- 메서드 은닉이 일어나면 자식 클래스 인스턴스가 메서드를 호출할 경우 부모 클래스의 메서드가 호출된다. 
- 런타임시에 인스턴스가 실제로 참조하고 있는 클래스를 찾아가는 것이 아니라 컴파일 시에 결정된 클래스를 찾아가기 때문에 이와 같은 상황이 발생한다.

```java
public class MethodHidingTest {
    static class Parent {
        static void m1() {
            System.out.println("From parent static m1()");
        }
    }

    static class Child extends Parent {
        // Child의 m1()이 Parent의 m1()을 hide한다
        static void m1() {
            System.out.println("From child static m1()");
        }
    }

    public static void main(String[] args) {
        Parent c = new Child(); // 자식 클래스의 인스턴스 생성
        c.m1(); // 부모 클래스의 static 메서드가 실행됨
    }
```
```java
From parent static m1()
```
<br>


**1-4) 오버라이딩시 예외 처리 규칙**<br>

> Exception의 종류<br>

source : [자바 예외 구분](https://madplay.github.io/post/java-checked-unchecked-exceptions)<br>


|구분|Checked Exception| Unchecked Exception|
|:---:|:---:|:---:|
확인 시점 | 컴파일 시 | 런타임 시|
처리 여부 | 반드시 예외처리 | 명시적으로 하지 않아도 됨|
종류 | IOException, ClassNotFoundException 등 | NullPointerException, ClassCastException 등|

<br>

**4-1) 부모 클래스가 예외를 throw 하지 않는 경우, 자식 클래스는 unchecked exception(런타임 예외)만 throw 가능**<br>

**4-1) 부모 클래스가 예외를 throw 하는 경우, 자식 클래스는 동일한 예외만 throw하거나 예외를 throw하지 않을 수 있음**<br>
<br>
<br>

## 2.2. 동적 메서드 디스패치(Dynamic Method Dispatch)

**위에서 살펴본 메서드 오버라이딩은 런타임 다형성을 구현하는 방법 중 하나였다. 이번에 살펴볼 다이나믹 메서드 디스패치는 호출할 메서드가 런타임시에 결정되도록 하는 매커니즘이다.** <br>
<BR>

#### 1) 동적 메서드 디스패치의 개념

- **업캐스팅** 속성에 의해 부모클래스의 참조변수가 자식클래스의 인스턴스를 참조할 수 있다.
- 즉, **부모클래스 참조변수가 다양한 클래스 타입(자식 클래스일 경우)의 인스턴스를 참조할 수 있는 것**이다.

- 부모클래스에서 자식클래스에 메서드가 오버라이든 된다고 할 때, 다양한 클래스 타입 인스턴스로 동일한 오버라이딩 메서드를 호출할 수 있다.

- 이 때 호출되는 메서드의 버전은 **런타임시 생성된 인스턴스의 타입에 의해 결정**되고, 이것을 **동적 메서드 디스패치**라 일컫는다. <br><br>


#### 2) 동적 메서드 디스패치 예시

- 부모 클래스 A와 자식클래스 B,C를 선언한다.
- A 클래스 타입의 참조변수 `ref`를 선언한다.
- 참조변수 `ref`가 참조하는 인스턴스를 변경하면서, 각 시행에서 호출되는 `m1()`의 결과를 확인한다.<br>

> A,B,C 클래스의 메서드 디스패치 테스트

```java
public class DispatchTest {
	// 부모클래스 A
    static class A{
        void m1(){
            System.out.println("A 클래스의 m1 메서드");
        }
    }

	// A의 자식클래스 B
    static class B extends A{
        @Override
        void m1() {
            System.out.println("B 클래스의 m1 메서드");
        }
    }
    // A의 자식클래스 C
    static class C extends A{
        @Override
        void m1() {
            System.out.println("C 클래스의 m1 메서드");
        }
    }

    public static void main(String[] args) {
        A a = new A();
        B b = new B();
        C c = new C();

        A ref;   // A 클래스타입(B,C의 부모)의 변수 선언

        ref= a; // A 타입 인스턴스
        ref.m1();

        ref= b; // B 타입 인스턴스
        ref.m1();

        ref= c; // C 타입 인스턴스
        ref.m1();
    }
}
```
```java
A 클래스의 m1 메서드
B 클래스의 m1 메서드
C 클래스의 m1 메서드
```
<br>

:point_right: **위 코드의 동작과정을 자세히 살펴보자**<br>


**1. A 클래스와 이를 상속받은 B,C 클래스의 인스턴스를 생성한다.**<br>

```java
A a = new A(); // A 클래스의 인스턴스
B b = new B(); // A의 자식인 B클래스의 인스턴스
C c = new C(); // A의 자식인 C클래스의 인스턴스
```

<img src="https://user-images.githubusercontent.com/62331803/103139262-c1718c80-471d-11eb-843a-9b42329e3ada.png" width="80%"><br>

<br>


**2. A 클래스 타입의 참조변수를 선언한다.**<br>

- 변수가 선언 직후에는 null값을 가리킨다.

```java
A ref;	// A 클래스 타입의 참조변수 ref
```

<img src="https://user-images.githubusercontent.com/62331803/103139289-ebc34a00-471d-11eb-8145-724a04b111e2.png" width="40%"><br>



<br>

**3. A,B,C 클래스 타입의 인스턴스에 대한 참조를 순차적으로 변수 ref에 삽입하고, 해당 참조를 이용하여 오버라이딩 메서드 m1()을 호출한다.**<br>

```java
ref= a; // A 타입 인스턴스
ref.m1();
```
<img src="https://user-images.githubusercontent.com/62331803/103139290-ee25a400-471d-11eb-8c01-d52aa57d0bf0.png" width="30%"><br>

```java
ref= b; // B 타입 인스턴스
ref.m1();
```
<img src="https://user-images.githubusercontent.com/62331803/103139291-f087fe00-471d-11eb-94ff-7bcd6c070eca.png" width="40%"><br>

```java
ref= c; // C 타입 인스턴스
ref.m1();
```
<img src="https://user-images.githubusercontent.com/62331803/103139292-f382ee80-471d-11eb-9135-e135629da4bf.png" width="40%"><br>
<br>

**즉, `ref`가 **참조하는 인스턴스의 타입**을 기준으로 `m1()`을 호출한다.**<br>
- 생성된 인스턴스 타입이 A일 경우 A버전 m1 호출
- 생성된 인스턴스 타입이 B일 경우 B버전 m1 
- 생성된 인스턴스 타입이 C일 경우 C버전 m1

<br>


:question: **다이나믹 '필드' 디스패치?**<br>

- 재정의 할 수 있는 대상은 `메서드`뿐이며,  `필드`에 대해서 동적 디스패치를 수행할 수 없다.

> 필드 디스패치 테스트

```java
public class DispatchTest {

    static class A {
        int x = 10;
    }

    static class B extends A {
        int x = 20;
    }

    public static void main(String[] args) {
        A a = new B(); // B 클래스 타입의 인스턴스 생성

        // A 클래스의 멤버로 접근한다.
        System.out.println(a.x);
    }
}
```
```java
10
```
<br>

#### 3) 다이나믹 메서드 디스패치의 장점
1. 런타임 다형성 구현의 핵심인 `메서드 오버라이딩`을 지원한다.
2. 부모 클래스는 자식 클래스에 공통되는 메서드를 상속시킬 수 있으며, 자식 클래스들은 자신들의 상황에 맞게 메서드를 재정의하여 사용할 수 있다.<br>
<br>
<br>



## 2.3. 더블 디스패치(Doubld Dispatch)





















<br><br>

:orange_book: **References**<br>

- geeksforgeeks
   - [overriding](https://www.geeksforgeeks.org/overriding-in-java/)
   - [dynamic method dispatch](https://www.geeksforgeeks.org/dynamic-method-dispatch-runtime-polymorphism-java/)
