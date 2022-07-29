---
title:  "[Java] 자바의 상속"
date: 2020-12-26
categories: ['Java']
tags: ['Java']
---

**자바의 상속과 관련된 개념을 알아보자** :raising_hand:<br>
<br>

## 목차 
### 상속
- 1.1. [상속의 기본](#11-상속의-기본)
- 1.2. [상속의 특징](#12-상속의-특징)
- 1.3. [Super 키워드](#13-super-키워드)
- 1.4. [Object 클래스](#14-object-클래스)
- 1.5. [final 키워드](#15-final-키워드)

<br><br><br>


# 1. 상속(Inheritance)

**상속은 객체지향 프로그래밍(Object Oriented Programming)에서 매우 중요한 개념이다.상속을 통해서 하나의 클래스가 가진 특징(필드와 메서드)을 다른 클래스들에게 물려줄 수 있다.** <br>
<br>

:point_right: **주요 용어**<br>
- **Super Class - 부모 클래스**
   - 다른 클래스들에게 상속을 하는 주체가 되는 클래스이다. 
   - 상위 클래스, super class, base class 라고도 불린다.

- **Sub Class - 자식 클래스**
   - 다른 클래스의 특징을 상속받는 클래스이다. 
   - 서브 클래스, derived class, extended class라고도 불린다. 
   - 자식 클래스는 부모 클래스로부터 상속받은 필드와 메서드뿐 아니라, 자기 자신의 필드와 메서드를 추가하여 사용할 수 있다.
- **Reusability - 재사용성**
   - 상속은 코드의 재사용성을 지원하는 기능이다. 
   - 새로운 클래스를 만드려고 할 때, 기존의 클래스를 상속받아 해당 클래스에 존재하는 기능을 그대로 사용할 수 있다.
   - 즉, 필드와 메서드를 반복할 필요가 없어지며 코드의 재사용성을 높이는 것이다.<br>
<br>
<br>


## 1.1. 상속의 기본

`extend` 키워드를 사용해 상속을 구현할 수 있다. <br>

> 문법

```java
class 자식클래스 extends 부모클래스{
	// 필드와 메서드
}
```
<br>

>  Pocketmon 클래스 상속받는 피카츄 클래스 생성 테스트

- **부모 클래스 Pocketmon 클래스**<br>
```java
 // 부모 클래스
    static class Pocketmon{

        // 3개의 필드
        public String name;
        public int hp;
        public int exp;


        // 1개의 생성자
        public Pocketmon(String name, int hp) {
            this.name = name;
            this.hp = hp;
            exp = 0;
        }

        // 3개의 메서드
        public void eat() {
            hp += 5;
        }
        public void exercise() {
            hp -= 10;
            exp += 5;
        }
        public String toString() {
            return name +" >> hp:"+hp+"     exp:"	+exp;
        }        
    }
```
<br>

-  **Pocketmon을 상속받은 Pikachu 클래스**<br>
```java
	static class Pikachu extends Pocketmon{
        public String owner;

        public Pikachu(String name, int hp, String owner) {
            // 부모 클래스의 생성자를 호출
            super(name, hp);
            this.owner = owner;
        }

        // 자식 클래스에서 추가한 메서드
        public void setOwner(String owner){
            this.owner = owner;
        }

		// 부모 클래스의 toString() 메서드 오버라이딩
		// 자식 클래스의 필드를 추가
        @Override
        public String toString() {
            return super.toString() + "     owner:"+owner;
        }
    }
```
<br>

- **자식 클래스의 인스턴스 생성과 결과 확인**<br>
```java
public static void main(String[] args) {
    Pikachu p = new Pikachu("피카츄",100,"지우");
    System.out.println(p.toString());
}
```
```java
피카츄 >> hp:100     exp:0     owner:지우
```
<br>

:point_right:  여기서 주목할 점은 **자식 클래스의 인스턴스가 생성될 때,  부모 클래스의 인스턴스가 생성되는 것이 아니라는 것**이다.<br>

source : [GeeksforGeeks - Java Object Creation of Inherited Class](https://www.geeksforgeeks.org/gfact-52-java-object-creation-of-inherited-classes/)<br>

> 피카츄 클래스 인스턴스 메모리 구조

<img src="https://user-images.githubusercontent.com/62331803/103097171-c402b200-4649-11eb-9e84-403712e8103f.png" width="70%"><br>

**자식 클래스의 인스턴스가 생성되면, 해당 인스턴스 내부에 부모 클래스 멤버를 위한 메모리 공간이 포함된다.**<br>
- 즉, 자식 클래스(Pikachu 클래스)의 인스턴스는 내부에 부모 클래스(Pocketmon 클래스)의 멤버에 대한 복사본을 가지게 된다. 
- 이 과정을 통해 자식 클래스의 인스턴스가 부모 클래스 멤버에 접근 가능한 것이다<br>
 <br><br>

:point_right:  **생성된 인스턴스의 해시코드와 등록된 클래스네임을 이용해서 이를 증명할 수 있다.**<br> 

> `this`와 `super` 키워드를 통해 해시코드 값과 등록된 인스턴스 네임을 비교

```java
public class PocketmonTest2 {

    static class Pocketmon{
        public Pocketmon(){
            System.out.println("부모 클래스의 생성자");
            System.out.println("부모 클래스 인스턴스의 해쉬코드 : "+this.hashCode());
            System.out.println(this.getClass().getName());
        }
    }
    static class Yadoran extends Pocketmon{
        public Yadoran(){
            System.out.println("자식 클래스의 생성자");
            System.out.println("자식 클래스 인스턴스의 해쉬코드 : "+this.hashCode());
            System.out.println(this.hashCode() + "     " + super.hashCode());
            System.out.println(this.getClass().getName() + "     " + super.getClass().getName());
        }
    }

    public static void main(String[] args) {
        Yadoran y = new Yadoran();
    }
}
```

<img src="https://user-images.githubusercontent.com/62331803/103098178-28734080-464d-11eb-9b24-8354396a7e21.png" width="100%"><br>


- `부모 클래스의 Object hashcode`와 `자식 클래스의 Object hashcode`가 일치한다. 
   - 즉, 하나의 객체만이 생성되었음을 알 수 있다.
- `getClass().getName()`을 통해서 생성된 객체가 자식 클래스인 `Yadoran`의 인스턴스임을 알 수 있다.<br>
<br><br>

**cf. 부모클래스 멤버의 접근제어별 접근범위**<br>

<img src="https://user-images.githubusercontent.com/62331803/102985808-1f9e4400-4553-11eb-9d33-bf99037a0dcf.png" width="60%"><br>

- `public`: 모든 클래스에서 접근 가능
- `protected`: 해당 클래스가 있는 패키지에서는 public과 동일한 역할을 하며, 외부 패키지에서는 상속받은 클래스만 접근 가능
- `default`: 해당 클래스가 있는 패키지 내에서만 접근 가능
- `private`: 해당 클래스 내에서만 접근 가능
<br>

> 클래스 종류별 멤버 접근 범위

| 클래스 종류 | public 멤버 | protected 멤버 | default 멤버 | private 멤버 |
|---|:---:|:---:|:---:|:---:|
| 내부패키지 상속관계 | O | O | O | X |
| 내부패키지 포함관계 | O | O | O | X |
| 외부패키지 상속관계 | O | O | X | X |
| 외부패키지 포함관계 | O | X | X | X |


<br><br><br>


## 1.2. 상속의 특징

- **모든 클래스의 부모 클래스인 Object class**
   - `Object` 클래스를 제외한 모든 클래스는 하나의 부모 클래스를 가진다. 
   - `extends` 키워드를 통해 명시적으로 선언되어있지 않더라도, 모든 클래스는 암묵적으로 `Object`클래스를 상속받고 있다.

- **오직 하나의 부모클래스를 갖는다.**

   - 부모 클래스는 여러개의 자식 클래스를 가질 수 있지만, 자식 클래스는 오직 하나의 부모 클래스를 갖는다.
   - 자바가 클래스의 다중상속을 지원하지 않기 때문
   - 하지만 Interface를 사용하여 다중상속을 구현할 수 있다. 

- **생성자는 상속되지 않는다.**

   - 자식 클래스는 부모 클래스의 모든 멤버를 상속받는다.
      - `클래스의 멤버 : 필드, 메서드, 중첩 클래스(Nested class)`
   - 생성자는 멤버에 속하지 않기 때문에 자식클래스로 상속되지 않는다.
   - 하지만, `super()`를 이용하여 자식 클래스에서 부모 클래스의 생성자를 호출할 수 있다.

- **private 멤버는 상속되지 않는다.**

   - private 멤버는 자식 클래스에 상속되지 않는다.
   - 하지만, `public`, `protected` 메서드를 이용하여 private 필드에 접근할 수 있다.
      - setter, getter<br>
<br>

## 1.3. Super 키워드

**super 키워드는 부모 클래스(super class) 객체를 참조할 수 있는 참조변수이다.**<br>
super 키워드가 사용되는 상황은 다음과 같다.<br><br>

**1) 부모 클래스 필드에 접근할 때**<br>

부모 클래스와 자식 클래스가 동일한 필드를 가지는 경우, JVM이 이를 해석하는데 모호함이 생길 수 있다. 이 때, 부모 클래스의 필드에 `super`를 붙여 구분지을 수 있다.<br>

> 부모와 자식 클래스 모두 `defaultHp` 필드를 가지고 있는 경우

```java
public class SuperTest {

    // 부모 클래스
    static class Pocketmon{
        int defaultHp = 100;
    }

    // Pocketmon을 상속받은 자식 클래스
    static class LongStone extends Pocketmon{
        int defaultHp = 150;

        // 부모클래스의 defaultHp 출력
        void display(){
            System.out.println("Default Hp : "+ super.defaultHp);
        }
    }

    public static void main(String[] args) {
        LongStone l = new LongStone();
        l.display();
    }
}
```
```java
Default Hp : 100
```
<br>

**2) 부모클래스의 메서드에 접근할 때**<br>

자식 클래스에서 부모 클래스의 메서드를 호출할 때 모호함이 생길 수 있다. `super` 키워드를 이용하면 이런 모호함을 제거할 수 있다.<br>

> 부모와 자식 클래스 모두 `message()`메서드를 가지고 있는 경우

```java
public class SuperTest {

    // 부모 클래스
    static class Pocketmon{
        void message(){
            System.out.println("This message is from Pocketmon class");
        }
    }

    // Pocketmon을 상속받은 자식 클래스
    static class LongStone extends Pocketmon{

        void message(){
            System.out.println("This message is from LongStone class");
        }

        void display(){
            // 현재 클래스의 message()를 호출
            message();
            // 부모 클래스의 message()를 호출
            super.message();
        }
    }

    public static void main(String[] args) {
        LongStone l = new LongStone();// 자식 클래스 인스턴스 생성
        l.display();				  // 자식 인스턴스 메서드 호출
    }
}
```
```java
This message is from LongStone class
This message is from Pocketmon class
```

- 자식 클래스에서 `message()`를 호출하면, 현재 클래스의 메서드가 실행된다. 
- `super` 키워드와 함께 `message()`를 호출할 경우 부모 클래스의 메서드가 실행된다.<br>
<br>


**3) 부모 클래스의 생성자에 접근할 때**<br>

디폴트 생성자와 파라미터가 존재하는 생성자 모두에 접근할 수 있다.<br>


```java
public class SuperTest {

    // 부모 클래스
    static class Pocketmon{
        Pocketmon(){
            System.out.println("Pocketmon class 생성자");
        }
    }

    // Pocketmon을 상속받은 자식 클래스
    static class LongStone extends Pocketmon{
        LongStone(){
            super();
            System.out.println("LongStone 클래스 생성자");
        }
    }

    public static void main(String[] args) {
        LongStone l = new LongStone();
    }
}

```
```java
Pocketmon class 생성자
LongStone 클래스 생성자
```

`super()`**로 부모 클래스의 생성자를 호출할 때 주의할 점**<br>

1. `super()`는 항상 자식 클래스 생성자의 맨 첫 줄에 와야한다.
2. 만약 자식 클래스의 생성자에 부모클래스의 생성자 호출코드가 없다면, 자바 컴파일러가 자동적으로 디폴트 super()를 삽입해 준다. 이 때 부모 클래스에 디폴트 생성자가 없다면 컴파일 에러가 난다.
3. 자식 클래스는 명시적으로든 암묵적으로든 자신의 부모 클래스 생성자를 호출하게 된다. 이러한 호출은 모든 클래스의 조상인 `Object`클래스의 생성자까지 이어지게 되고, 이를 `constructor chaining`이라 부른다.
<br><br><br>

## 1.4. Object 클래스

[Object 클래스](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/Object.html)는 [java.lang](https://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/lang/package-summary.html) 패키지에 존재하는 클래스로서, 모든 클래스가 직간접적으로 상속받고 있는 클래스이다. <br>
- 명시적으로 상속받은 부모 클래스가 없을 경우 ⇒   Object 클래스를 직접 상속 받은 자식 클래스
- 명시적으로 상속받은 부모 클래스가 있는 경우 ⇒ 간접적으로 상속된 자식 클래스
- 즉, 모든 자바 클래스에서 Object 클래스의 메서드를 사용할 수 있다. Object 클래스는 모든 자바 프로그램의 뿌리역할을 한다.<br>
<br>

:point_right: **Object 클래스의 메서드**<br>

<img src="https://user-images.githubusercontent.com/62331803/103102403-20bc9780-465f-11eb-8d9f-8a5d55c6c1be.png" width="200%"><br>

**1. toString()**<br>

- `toString() 메서드`는 인스턴스에 대한 정보를 String 형태로 제공해주는 메서드이다.
- 디폴트 형태는 `생성된 인스턴스의 클래스명+ @ + 생성된 인스턴스의 해시코드에 대한 16진수표현`을 String 형태로 반환한다.<br>

```java
// 디폴트 toString() 
// class name + @ + unsigned hexadecimal representation of the hash code of the object
public String toString()
{
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```
<br>

**2. hashCode()**<br>

- JVM은 생성된 각각의 인스턴스에 대하여 고유한 번호인 해시코드를 부여한다. 
- 해시코드는 인스턴스로 가는 직접적인 주소가 아니라, 인스턴스의 주소를 내부적인 알고리즘을 통해 정수형태로 바꾼 형태이다. 
- 자바는 C/C++과 다르게 인스턴스의 reference를 사용하지 않는 언어이다. 따라서 hashcode()는 [native](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EB%84%A4%EC%9D%B4%ED%8B%B0%EB%B8%8C_%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4)로 구현되어 있다. <br>
<br>

**3. equals(Object obj)**<br>

- 파라미터로 주어진 객체와 현재 객체를 비교한다. 
- `equals()`를 오버라이딩할 경우 `hashcode()`가 함께 오버라이딩되어 객체의 해쉬코드값을 비교한다. 
- 비교하는 두 객체가 같다면 해쉬코드값이 동일해야 한다.<br>
<br>

**4. getClass()**<br>

- `getClass()`는 실제 런타임에 생성된 클래스 인스턴스 반환한다.
- 클래스의 메타 데이터를 가져오는 데 사용되기도 한다.
- 반환된 클래스 인스턴스는 `static synchronized method(정적 동기화 메서드)`에 의해 lock된 상태이다.
- `final` 메서드이기 때문에 오버라이딩이 불가능하다.

```java
public class Test {
   
    public static void main(String[] args) {
        Object obj = new String("Merry Christmas!!");
        Class c = obj.getClass();
        System.out.println("Class of Object obj is :"+ c.getName());
    }
}
```
```java
Class of Object obj is : java.lang.String
```
위의 코드는 JVM은 컴파일된 `.class`파일을 로딩한 후, Heap영역에  `java.lang.Class`타입의 인스턴스를 생성한다. 우리는 이 `class 클래스 인스턴스`를 사용하여 클래스 레벨의 정보를 가져올 수 있다. 이같은 코드는 주로 [Reflection API](https://www.oracle.com/technical-resources/articles/java/javareflection.html)을 사용할 때 이용된다. <br>
<br>


**5. finalize()**<br>

- `finalize()`는 인스턴스의 마지막 순간. 즉, [가비지 콜렉팅](https://youtu.be/vZRmCbl871I)되기 직전에 호출된다.
- 자바의 GC작업은 `JVM의 Garbage Collector`에 의해 자동수행된다. 
- `Garbage Collector`가 해당 인스턴스를 참조한 변수가 있는지 탐색하고, 없다면 `finalize()`를 호출해 heap에서 인스턴스를 제거한다.
- 프로그래머가 `finalize()`를 직접 호출하여 인스턴스 제거를 명령할 수도 있다.
- `finalize()`를 오버라이딩하면 시스템 자원을 확보하고, 메모리 누수를 최소화 할 수 있게 된다.
- 예를 들면 웹 컨테이너의 서블릿 객체를 삭제하기 전에, `finalize()`를 호출하여 세션을 초기화한다.

```java
public class Test {

    public static void main(String[] args) {
        Test t = new Test();
        System.out.println(t.hashCode());
        t = null;

        // 가비지 콜렉터 호출
        System.gc();
        System.out.println("end");
    }

    @Override
    protected void finalize() throws Throwable {
        System.out.println("finalize 메소드 호출");
    }
}
```
```java
1922154895
end
finalize 메소드 호출
```
<br>

**6. clone()**<br>

- `clone()`은 인스턴스에 대한 클론(복사본)을 반환한다. <br>
<br>

**7. wait(), notify(), notifyAll()**<br>

- 다음의 메서드들은 멀티 스레딩 환경에서의 Polling 문제(특정 조건이 참인지를 지속적으로 테스팅하는 과정)를 방지하기 위해 사용한다. 
- final 메서드이다.
- `synchronized` 블럭 내부에서만 사용할 수 있다.

1. `wait()` : 작업을 진행하던 스레드가 `lock` 상태를 포기하고, 다른 스레드가 `notify()`를 호출할 때까지 `sleep`상태에 빠진다.
2. `notify()` : 동일한 인스턴스 내에서 `wait()`로 인해 `sleep` 상태에 빠졌던 하나의 스레드를 깨운다.
3. `notifyAll()` : `wait()`로 인해 `sleep` 상태에 빠졌던 모든 스레드를 깨운다<br>
<br><br><br>


## 1.5. final 키워드

**final 키워드**는 `변수`, `메서드`, `클래스`에 적용될 수 있으며, 적용되는 대상에 따라 다른 역할을 수행한다.<br>

- `Final Variable` ⇒ 변수의 재할당을 막기 위함
- `Final Methods` ==> 메서드 오버라이딩을 막기 위함
- `Final Classes` ⇒ 클래스 상속을 막기 위함
<br><br>

#### 1) Final 변수

- final 키워드가 변수의 앞에 붙으면, 해당 변수는 수정될 수 없다. 
- 따라서, 반드시 선언시에 초기화 값을 넣어주어야 한다.
- 하지만 final 변수가 참조변수일 경우에는 해당 참조값이 가리키는 인스턴스가 달라질 수 있다.
   - final array나 final collection은 요소 추가/제거가 가능<br>
<br>

:point_right: **1-1) value를 담는 final 변수**<br>

한 번 값이 할당되면 재할당이 불가능하기 때문에, 프로그램이 실행되는 동안 지속되어야하는 값에 final을 붙인다. <br>

- **final 변수 초기화 방법**<br>
   - final 변수를 초기화하지 않으면 컴파일 에러를 발생시킨다.
   - final 변수 오직 한 번만 초기화 될 수 있으며, 초기화 블럭과 할당연산자를 통해 초기화가 가능하다.

   - **1. 선언과 동시에 초기화** : 가장 보편적인 초기화 방법
   - **2. 초기화 블록에서 초기화**
   - **3. 생성자에서 초기화**
<br>

> final 변수 초기화 예시

```java
public class FinalTest {
    // 1. 선언과 동시에 초기화
    final int THRESHOLD = 5;
    static final double PI = 3.141592653589793;

    // 2. 초기화 블럭으로 초기화
    final int CAPACITY;
    {
        CAPACITY = 25;
    }
    static final double EULERCONSTANT;
    static {
        EULERCONSTANT = 2.3;
    }

    // 3. 생성자를 사용한 초기화
    // 다수의 생성자가 존재한다면, 모든 생성자에 MINIMUM 초기화 코드를 삽입해야 한다.
    final int MINIMUM;
    public FinalTest(){
        MINIMUM = -1;
    }
}
```
<br>
<br>

:point_right: **1-2) 참조값을 가지는 final 변수**<br>

final 변수가 인스턴스로의 참조값을 가진다면, 이 변수를 `reference final variable(참조형 파이널 변수)`라 부른다.<br>

- final 변수는 다른 값으로 재할당 될 수 없다.
- 하지만 final 변수가 가지고 있는 참조의 대상자체가 변할 수 있다. 
- 이것은 `re-assigning(재할당)`의 개념이 아니라, `non-transitivity(비이동성)`의 개념이다. <br>

> reference final variable 예시<br>

```java
public class FinalTest {
    public static void main(String[] args) {

        // final reference 변수
        final StringBuilder sb = new StringBuilder("Merry");
        System.out.println(sb);

        // 인스턴스의 내부 상태 변화
        sb.append(" Christmas");
        System.out.println(sb);
    }
}
```
```java
Merry
Merry Christmas
```
<br><br>

#### 2) Final 클래스

`final 클래스`란 다른 클래스로 상속될 수 없는 클래스를 말한다. 크게 2가지 종류가 있다.<br>

:point_right: **2-1) 상속을 방지하기 위함**<br>

- 대표적인 예로 `Integer`, `Float`와 같은 **Wrapper** 클래스가 있다.


```java
final class A{
     // methods and fields
}

// final 클래스인 A를 상속받으려 한다면, 컴파일 에러가 발생한다.
class B extends A { }
```
<br>

:point_right: **2-2) 불변 클래스를 만들기 위함**<br>

- String 클래스와 같은 불변 클래스를 만드는 용도로 사용된다. 
- final없이는 그러한 클래스를 생성할 수 없다.<br>
<br>

#### 3) Final 메서드

- `final`키워드가 붙은 메서드이며,  자식 클래스에서 오버라이딩 할 수 없다. 
- 대표적으로 Object 클래스의 `getClass()`, `wait()`, `notify()`등이 있다.

```java
class A {
    final void display() {
        System.out.println("This is a final method.");
    }
}

class B extends A {
    void display(){
        // 오버라이딩 할 수 없기 때문에 컴파일 에러가 발생한다. 
        System.out.println("Illegal!");
    }
}
```
<br><br>

:orange_book: References
- geeksforgeeks
   - [Inheritance in Java](https://www.geeksforgeeks.org/inheritance-in-java/)
   - [super](https://www.geeksforgeeks.org/super-keyword/)
   - [object class](https://www.geeksforgeeks.org/object-class-in-java/)
   - [Inter thread Communication in Java](https://www.geeksforgeeks.org/inter-thread-communication-java/)
   - [final keyword in Java](https://www.geeksforgeeks.org/final-keyword-java/?ref=rp)
   - [abstract classes in Java](https://www.geeksforgeeks.org/abstract-classes-in-java/)
- [TCP School - 상속](http://www.tcpschool.com/java/java_inheritance_concept)
- [자바의 정석 - 상속](https://youtu.be/Pgutf0G3nE4)
- [Scaler Topics - Inheritance in Java](https://www.scaler.com/topics/java/inheritance-in-java/)
<br>
<br>
