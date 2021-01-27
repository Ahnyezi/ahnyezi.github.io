---
title:  "[Java] 자바의 열거형(Enum)"
date: 2021-01-27
categories: ['Java']
tags: ['Java']
---

**자바의 열거형(enum)을 알아보자** :raising_hand:<br>
<br>

## 목차 
1. [열거형(Enum)의 기본](#0-열거형enum이란)
2. [열거형 정의와 사용법](#1-열거형-정의와-사용법)
3. [java.lang.Enum(열거형의 조상)](#2-javalangenum열거형의-조상)
4. [enum에 멤버 추가하기](#3-enum에-멤버-추가하기)
5. [EnumSet (열거셋)](#4-enumset-열거셋)

<br><br><br>


## 0. 열거형(Enum)이란?

**열거형은 관련된 상수들을 같이 묶어놓은 것을 말한다.**<BR>

예를 들면 카드게임 프로그램을 위한 상수들이 있다. 이렇게 일일히 상수를 정의해서 사용하면 카드의 종류가 많아질수록 상수의 양이 많아져서 프로그램을 관리하기가  힘들 것이다.<br>

```java
class Card{
   // 카드 무늬
   static final int CLOVER = 0;
   static final int HEART = 1;
   static final int DIAMOND = 2;
   static final int SPADE = 3;
   // 카드 숫자
   static final int TWO = 0;
   static final int THREE = 1;
   static final int FOUR = 2;

   final int kind;
   final int num;
}
```

따라서 다음과 같이 무늬는 무늬끼리, 숫자는 숫자끼리 간단하게 정의할 수 있게 도와주는 역할이 enum이다.
아래의 코드는 위의 코드와 동일한 기능을 하며 enum의 요소에는 자동으로 숫자가 부여된다(0부터 시작). 

```java
class Card{//      0,      1,      2,      3
   enum Kind = { CLOVER, HEART, DIAMOND, SPADE } // 열거형 Kind를 정의
}
   enum Value = { TWO, THREE, FOUR} // 열저형 Value를 정의
   
   final Kind kind; // 타입이 int가 아닌 Kind임에 유의
   final Value value; 
```

<br>

**다음의 수식을 보자.**<br>
`if (Card.CLOVER == Card.TWO)`<br>


`Card의 클로버 무늬`와 `Card의 Two 숫자`가 동일한지를 묻는 식이다. 무늬와 숫자이므로 의미상 다르며 따라서 false가 나와야 한다.

- **C언어의 경우에는,** 두 상수의 값을 비교한다. 따라서 두 값 모두 `0`이기 때문에 true가 나오게 된다.
- **Java의 경우에는**, 타입에 안전한 열거형을 제공하기 때문에(값과 타입 둘다 체크하기 때문에) false가 나오게 된다. 

> 자바는 타입에 안전한 열거형을 제공한다.

```java
if (Card.Kind.CLOVER == Card.Value.TWO){}
// 컴파일 에러 => 값은 같지만, 타입이 다르기 때문에 비교 불가하다
```

<BR><BR>


## 1. 열거형 정의와 사용법

**1) 열거형 정의하는 방법**<br>
`enum 열거형이름 {상수명1, 상수명2, ...}`

```java
//      이름      0      1     2      3
enum Direction {EAST, SOUTH, WEST, NORTH}
```

**2) 열거형 타입의 변수를 선언하고 사용하는 방법**<br>

```java
class Unit{
   int x, y; // 유닛의 위치
   Direction dir; // 열거형 인스턴스 변수를 선언
   
   void init(){
      dir = Direction.EAST; // 유닛의 방향을 EAST로 초기화
   }
}
```

**3) 열거형 상수의 비교에** `==`**와** `compareTo()` **사용가능**<br>

```java
if (dir == Direction.EAST){
   x++;
} else if (dir > Direction.WEST){ // 에러. 열거형 상수에 비교연산자 사용불가
   ...
} else if (dir.compareTo(Direction.WEST) > 0){ // compareTo() 사용가능

}
```

#### cf. a.compareTo(b) 메서드
- a,b 값이 같으면 0
- a가 크면 양수
- b가 크면 음수


<BR><BR>

## 2. java.lang.Enum(열거형의 조상)


**1. 모든 열거형은 Enum 클래스의 자손이며, 따라서 `Enum` 클래스의 멤버를 상속받는다.**<br>

- `String name()` : 열거형 상수의 이름을 문자열로 반환
- `int ordinal()` : 열거형 상수가 정의된 순서를 반환(0부터 시작)
- `T valueOf(Class<T> enumType, String name)` : 지정된 열거형에서 name과 일치하는 열거형 상수를 반환


**2.** `values()`, `valueOf()`**는 컴파일러가 자동으로 추가해준다.**<br>

- `static E[] values()` : 열거형 상수가 가진 모든 상수를 배열로 반환

```java
Direction[] dArr = Direction.values();

for(Direction d : dArr) // 각 열거형 상수의 이름과 순서 출력
   System.out.println("%s=%d%n", d.name(), d.ordinal());
```

- `static E valueOf(String name)` : 문자열로 열거형 상수의 값을 얻는다.

```java
Direction d = Direction.valueOf("WEST");
// Direction.WEST와 같다.
```

<BR><BR>

:point_right: **예제 : values(), valueOf() 사용하기** <br>

```java
//                0      1      2      3
enum Direction {EAST, SOUTH, WEST, NORTH};

public class EnumTest {

    public static void main(String[] args) {
        Direction d1 = Direction.EAST;
        Direction d2 = Direction.valueOf("WEST");
        Direction d3 = Enum.valueOf(Direction.class, "EAST");

        System.out.println("d1=" + d1);
        System.out.println("d2=" + d2);
        System.out.println("d3=" + d3);

        System.out.println("d1==d2 ? " + (d1 == d2));
        System.out.println("d1==d3 ? " + (d1 == d3));
        System.out.println("d1.equals(d3) ?" + d1.equals(d3)); // Direction의 각 값이 단순한 상수가 아니라 객체이기 때문에 equals로 비교 가능
//        System.out.println("d2 > d3 ?" + (d1 > d3)); // 에러
        System.out.println("d1.compareTo(d3) ? " + (d1.compareTo(d3)));
        System.out.println("d1.compareTo(d2) ? " + (d1.compareTo(d2)));

        switch (d1) {
            case EAST: // Direction.EAST 라고 쓸 수 없다.
                System.out.println("The direction is EAST.");
                break;
            case WEST:
                System.out.println("The direction is WEST.");
                break;
            case SOUTH:
                System.out.println("The direction is SOUTH.");
                break;
            case NORTH:
                System.out.println("The direction is NORTH.");
                break;
            default:
                System.out.println("Invalid Direction.");
        }

        Direction[] dArr = Direction.values(); // 열거형의 모든 상수를 배열로 반환

        for (Direction d : dArr) { // 상수의 이름, 상수의 순서(!= 값)
            // 부여한 상수와 순서값이 일치하지 않을 수 있다.
            System.out.printf("%s=%d%n", d.name(), d.ordinal());
        }
    }
}
```

```java
d1=EAST
d2=WEST
d3=EAST
d1==d2 ? false
d1==d3 ? true
d1.equals(d3) ?true
d1.compareTo(d3) ? 0
d1.compareTo(d2) ? -2
The direction is EAST.
EAST=0
SOUTH=1
WEST=2
NORTH=3
```


 <br> <br> <br>

## 3. enum에 멤버 추가하기

**1. 불연속적인 열거형 상수의 경우, 원하는 값을 괄호() 안에 적는다. (생성자를 호출해서 값을 할당하는 방법)**<br>
- `enum Direction{ EAST(1), SOUTH(5), WEST(-1), NORTH(10) }`


*여러 개를 할당하는 것도 가능하다 : `EAST(1, ">")`


**2. 괄호()를 사용하려면, 인스턴스 변수와 생성자를 새로 추가해줘야 한다.**<br>

```java
enum Direction{
   EAST(1), SOUTH(5), WEST(-1), NORTH(10); // 끝에 ';'를 추가해야 한다.

   private final int value; // 정수를 저장할 필드를 추가
   Direction(int value) { this.value = value; } // 생성자를 추가 

   public int getValue() { return value;}
}
```

**3. 열거형의 생성자는 묵시적으로 private이므로, 외부에서 객체생성 불가**<br>

```java
Direction d = new Direction(1); // 에러 => 열거형의 생성자는 외부에서 호출 불가
```

 <br> <br>


:point_right: **예제: 열거형에 멤버추가하고, 상수값 지정하기**<BR> 

```java
package javabasic.week11;

enum Direction2 {
    EAST(1, ">"), SOUTH(2, "V"), WEST(3, "<"), NORTH(4, "^");

    private static final Direction2[] DIR_ARR = Direction2.values();
    private final int value; // 상수1
    private final String symbol; // 상수2

    Direction2(int value, String symbol) { // 접근제어자 private 생략됨
        this.value = value;
        this.symbol = symbol;
    }

    public int getValue() {
        return value;
    }

    public String getSymbol() {
        return symbol;
    }

    public static Direction2 of(int dir) {
        if (dir < 1 || dir > 4)
            throw new IllegalArgumentException("Invalid value : " + dir);

        return DIR_ARR[dir - 1];
    }

    // 방향 회전시키는 메서드 : num의 값만큼 90도씩 시계방향으로 회전한다.
    public Direction2 rotate(int num) {
        num = num % 4;

        if (num < 0) num += 4; // num이 음수일 때는 시계반대 방향으로 회전

        return DIR_ARR[(value - 1 + num) % 4];
    }

    public String toString(){
        return name() + getSymbol();
    }
} // enum Direction2

public class EnumTest {

    public static void main(String[] args) {
        for (Direction2 d : Direction2.values())
            System.out.printf("%s=%d%n", d.name(), d.getValue()); // *d.ordinal(): 순서

        Direction2 d1 = Direction2.EAST;
        Direction2 d2 = Direction2.of(1); // 첫번째 열거형 상수 반환.

        System.out.printf("d1=%s, %d%n", d1.name(), d1.getValue());
        System.out.printf("d2=%s, %d%n", d2.name(), d2.getValue());
        System.out.println(Direction2.EAST.rotate(1));
        System.out.println(Direction2.EAST.rotate(2));
        System.out.println(Direction2.EAST.rotate(-1));
        System.out.println(Direction2.EAST.rotate(-2));
    }
}

```

<br><br>

## 4. EnumSet (열거셋)

`Java.util.EnumSet`**은 enum 유형에 사용하기 위한 특수 Set 구현체로, 자바 1.5부터 등장하였다. AbstractSet 클래스를 상속하고  Set 인터페이스를 구현한 형태이다.**<br>

**특징**<br>

- 열거형 타입으로 지정해놓은 요소들을 가장 쉽고 빠르게 다룰 수 있게 한다. 
- 요소의 개수가 2^6을 넘지 않을 경우 long 데이터형의 비트필드를 사용한다
- 겉으로는 Set을 기반으로 하지만 내부적으로 비트필드를 사용하기 때문에, 메모리 공간도 적게 차지하고 속도도 빠르다.
- enum과 static 메서드로 구성되어 있어 안정성을 추구하면서 편리한 사용이 가능하다.

:point_right: **예제: EnumSet 다루기**<BR> 

```java
import java.util.EnumSet;

public class EnumSetTest {

    enum Day{
        SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
    }

    public static void main(String[] args) {
        // 1. allOf() : 열거형 Day의 모든 요소를 반환
        EnumSet es = EnumSet.allOf(Day.class);
        EnumSet es2 = EnumSet.copyOf(es); // es를 복사
        System.out.println("EnumSet Day: "+ es2);

        // 2. noneOf() : 열거형 Day를 비워라
        es2 = EnumSet.noneOf(Day.class);
        System.out.println("Day를 비워라 => "+ es2);

        // 3. of() : 지정한 요소를 반환
        es = EnumSet.of(Day.FRIDAY, Day.WEDNESDAY);
        System.out.println("금요일과 수요일만 => "+ es);

        // 4. comeplementOf() : 지정된 요소를 제외한 나머지를 반환
        es2 = EnumSet.complementOf(es);
        System.out.println("금요일과 수요일 제외한 나머지 => "+ es2);

        // 5. range() : 지정된 구간 내의 요소만 반환
        es2 = EnumSet.range(Day.TUESDAY, Day.FRIDAY);
        System.out.println("화요일부터 금요일까지 => "+ es2);
    }
}
```
```java
EnumSet Day: [SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY]
Day를 비워라 => []
금요일과 수요일만 => [WEDNESDAY, FRIDAY]
금요일과 수요일 제외한 나머지 => [SUNDAY, MONDAY, TUESDAY, THURSDAY, SATURDAY]
화요일부터 금요일까지 => [TUESDAY, WEDNESDAY, THURSDAY, FRIDAY]
```

<br><br><br>

:orange_book: **References**<br>
- 자바의 정석
   - [열거형](https://www.youtube.com/watch?v=ODHC-n4mpMY&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=141)
   - [열거형에 멤버 추가하기](https://www.youtube.com/watch?v=ODHC-n4mpMY&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=141)
- [자바의 열거셋](http://alecture.blogspot.com/2012/11/enumset.html)
- [SO Documentation - EnumSet 클래스](https://sodocumentation.net/ko/java/topic/10159/enumset-%ED%81%B4%EB%9E%98%EC%8A%A4)