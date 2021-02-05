---
title:  "[Java] 자바의 어노테이션(Annotation)"
date: 2021-02-05
categories: ['Java']
tags: ['Java']
---

**자바의 어노테이션(Annotation)을 알아보자** :raising_hand:<br>
<br>

## 목차 
1. [어노테이션이란?](#1-어노테이션이란)
2. [표준 어노테이션](#2-표준-어노테이션)
- @Override
- @Deprecated
- @FunctionalInterface
- @SuppressWarning
3. [메타 어노테이션](#3-메타-어노테이션)
- @Target
- @Retention
- @Documented
- @Inherited
- @Repeatable
4. [어노테이션 타입 정의방법](#4-어노테이션-타입-정의방법)
5. [어노테이션 요소의 규칙](#5-어노테이션-요소의-규칙)

<br><br><br>


## 1. 어노테이션이란?

**Annotation**은 프로그램 내에서 **주석**과 유사하게, 프로그래밍 언어에는 영향을 미치지 않으면서 프로그램/프로그래머에게 유의미한 정보를 제공하는 역할을 한다.<br>

우선 주석의 등장배경을 살펴보자.<br>
- 주석이 등장하기 전에는 프로그램 소스코드와 프로그램 문서를 따로 작성했고, 프로그램이 변경될 때 해당 문서도 함께 변경해야 했다.
- 하지만 프로그래머들이 소스코드만 변경하고 문서는 변경하지 않는 경우가 많았고, 코드와 문서의 버전 불일치 때문에 코드를 관리하는 데에 어려움이 따랐다.
- 이러한 문제를 해결하기 위해 코드와 문서를 합쳐 하나로 만들 수 있는 방법을 만들었다. 
- 문서 내용을 코드와 함께 주석으로 달아놓고, javadoc.exe 프로그램이 소스파일 내의 주석을 추출해서 자동으로 문서로 만들어 주게끔 하였다.


어노테이션도 이와 마찬가지이다.<br>

- 어노테이션이 등장하기 전에는 프로그램 소스코드와 프로그램 설정파일(대부분 .xml 형태)을 따로 작성했기 때문에, 코드와 설정파일 분리에 따른 어려움이 있었다. 
   - 예를 들어, `@Test`는  => Junit이라는 특정 프로그램에게 설정정보를 제공하기 위한 것이다.
- 또한 프로젝트를 여러 사람이 함께 할 경우 모든 프로그래머가 설정정보를 공유해야 하는데, xml파일을 사용할 경우 많은 사람이 하나의 xml 파일을 두고 사용하기 때문에 사용/수정하기에 어려움이 있었다.
   - 어노테이션을 사용한다면 각자 자기가 맡은 부분에서 어노테이션을 사용하면 되므로 이와 같은 문제를 해결할 수 있다.

:point_right: 어노테이션의 사용 예<br>

```java
@Test // method()가 테스트 대상임을 JUnit 단위테스트 프로그램에게 알린다. 
public void method(){
}
```
`@Test` 어노테이션은 자체적인 프로그램에는 영향을 미치지 않으면서 `method()`가 테스트 대상임을 **JUnit 단위 테스트 프로그램**에 알린다.<br>
<br>

## 2. 표준 어노테이션

- 자바에서 제공하는 어노테이션은 다음과 같다.
   - `*`은 메타 어노테이션으로 어노테이션을 만드는데 사용한다.

|애노테이션|쓰임|
|:---:|:---:|
|@Override| 컴파일러에게 오버라이딩하는 메서드라는 것을 알린다.|
|@Deprecated|앞으로 사용하지 말 것을 권장하는 대상에 붙인다.|
|@SuppressWarnings|컴파일러의 특정 경고메세지가 나타나지 않게 해준다.|
|@SafeVarargs|지네릭스 타입의 가변인자에 사용한다.(JDK 1.7)|
|@FunctionalInterface|함수형 인터페이스라는 것을 알린다. (JDK 1.8)|
|@Native|native 메서드에서 참조되는 상수 앞에 붙인다. (JDK 1.8)|
|@Target*|애노테이션이 적용가능한 대상을 지정하는데 사용한다.|
|@Documented*|애노테이션 정보가 javadoc으로 작성된 문서에 포함되게 한다.|
|@Inherited*|애노테이션이 자속 클래스에 상속되도록 한다.|
|@Retention*|애노테이션이 유지되는 범위를 지정하는데 사용한다.|
|@Repeatable*|애노테이션을 반복해서 적용할 수 있게 한다. (JDK 1.8)|


<br><br>

## 2-1) @Override

**부모의 멤버를 오버라이딩할 때 오탈자를 입력하는 경우가 많다. 이러한 실수를 방지하기 위한 애노테이션이다.**<br> 

- 오버라이딩을 올바르게 했는지 컴파일러가 체크하게 한다.
	- 즉, 컴파일러(javac.exe)가 사용하는 애노테이션

:point_right: **parentMethod => parentmethod**<br> 

오탈자로 인해서 부모의 메서드를 상속받는 것이 아닌 새로운 메서드가 만들어지게 된다. <br>

```java
class Parent{
   void parentMethod(){}
}
class Child extends Parent{
   void parentmethod(){} // 오버라이딩하려 했으나 실수로 이름을 잘못적음
   // 새로운 메서드가 만들어진 것
}
```

**오버라이딩시 메서드 선언부 앞에 @Override를 붙여 검사하면 이러한 문제를 방지할 수 있다.**<br>

```java
class Child extends Parent{
   @Override
   void parentMethod(){ }
}
```
**오탈자가 있을 경우 컴파일 에러를 발생시킨다.**
```java
AnotationTest.java:6: error: method does not override or implement a method from a supertype
		@Override
		^
1 error
```

<br>

## 2-2) @Deprecated

**앞으로 사용하지 않을 것을 권장하는 필드나 메서드에 붙인다.**<br>

- 해당 메서드에 문제가 있거나 같은 기능의 더 좋은 메서드가 있거나

:point_right: **사용 예 - Date 클래스의 getDate()**<br>

```java
@Deprecated // 사용 권장 x
public int getDate(){
   return normalize().getDayOfMonth();
}
```

`@Deprecated`가 붙은 대상이 사용된 코드를 컴파일하면 나타나는 메세지<br>
- 에러는 아니지만 알림 메세지가 출력된다.

```java
Note: AnnotationTest.java uses or overrides a deprecated API.
Note: Recompile with -XLint:deprecation for details.
```
<br>

:question: **왜 메서드를 없애지 않고** `@Deprecation`**을 사용할까?**<br>

자바는 하위 호환성을 중요시 여긴다.
과거 getDate()를 사용해서 작성된 프로그램이 있다면, jdk 윗 버전에서는 해당 프로그램이 실행될 수 없을 것이다. 따라서 그러한 프로그램도 jdk 최신버전에서 돌아갈 수 있게끔 하되, @Deprecated 애노테이션을 붙여 사용하지 말 것을 권하는 것이다. 
<br>

:point_right: **예제 : Deprecated 된 메서드 사용하고 컴파일 결과 확인**<br>

```java
package javabasic.week12;  
  
class Parent{  
    void parentMethod(){}  
}  
  
class Child extends Parent{  
    @Override  
 @Deprecated  void parentMethod(){}  
}  
  
public class AnnotationTest {  
    public static void main(String[] args) {  
        Child c = new Child();  
  c.parentMethod(); // deprecated method  
  }  
}
```
 
 :point_right: **컴파일 결과**<br>

![image](https://user-images.githubusercontent.com/62331803/106703309-5c1e8e80-662d-11eb-99fa-5f32678ac84c.png)

컴파일시 경고가 출력된다. => `컴파일이 되었지만, deprecated API를 사용했다. 자세한 정보를 보려면 옵션을 주고 다시 컴파일해라.`
재컴파일 하면, warning에 대한 상세정보를 볼 수 있다.
<br>

## 2-3) @FunctionalInterface

**함수형 인터페이스의 제약사항이 지켜져 있는지 확인하는 어노테이션이다.**<br>

- 컴파일러(javac.exe)가 사용하는 어노테이션
- 함수형 인터페이스에 붙이면, 컴파일러가 올바르게 작성했는지 체크한다.
   - 함수형 인터페이스에는 **하나의 추상메서드만 있어야 한다**는 제약이 있다.

```java
@FunctionalInterface
public interface Runnable{
   public abstract void run(); // 추상메서드
}
```

:point_right: **함수형 인터페이스 제약에 맞지 않는 경우. 컴파일 에러를 발생시킨다.**<br>

![image](https://user-images.githubusercontent.com/62331803/106704366-5a55ca80-662f-11eb-8344-47b2d1aabcca.png)

<br>

## 2-4) @SuppressWarning

**컴파일러의 경고메세지가 나타나지 않게 억제하는 어노테이션이다.**<br>

- 컴파일러(javac.exe)가 사용하는 어노테이션
- 괄호()안에 억제하고자 하는 경고의 종류를 문자열로 지정
- 컴파일러의 경고메세지가 나타나지 않게 억제하여, 프로그래머가 이미 해당 경고를 확인했다는 것을 프로그램에 알린다.

:point_right: **사용 예시**<br>

```java
// unchecked 경고를 억제
@SuppressWarnings("unchecked") 
ArrayList list = new ArrayList(); // 지네릭 클래스는 원시타입 쓸 수 없지만, 지정해주지 않음
list.add(obj); // 여기서 경고 발생
```

- 둘 이상의 경고를 동시에 억제하려면 다음과 같이 한다.

```java
@SuppressWarnings(("deprecation", "unchecked","varargs", "rawtypes"))
```

- `-Xlint` 옵션으로 컴파일하면, 경고메세지 확인 가능

<br>

:point_right: **예제: rawtype 경고 억제하기**<br>

- 지네릭 클래스인 ArrayList에 rawtype사용

![image](https://user-images.githubusercontent.com/62331803/106705567-8bcf9580-6631-11eb-86bc-2b8eed807155.png)

- 컴파일시 unchecked warning을 생성한다.

![image](https://user-images.githubusercontent.com/62331803/106706021-537c8700-6632-11eb-90c3-f12b869d410f.png)

- SuppressWarnings 어노테이션을 붙여 unchecked 경고를 억제

![image](https://user-images.githubusercontent.com/62331803/106706098-79099080-6632-11eb-96c3-f16bdb5bc9f0.png)

- 억제하면 컴파일 시 경고를 발생시키지 않는다.

![image](https://user-images.githubusercontent.com/62331803/106706142-8fafe780-6632-11eb-9f86-a9b4403559ef.png)


<br>

## 3. 메타 어노테이션

- 메타 어노테이션은 어노테이션을 위한 어노테이션이다.
   - 어노테이션을 만들 때 사용하는 어노테이션 
- 메타 어노테이션은 java.lang.annotation 패키지에 포함되어 있다.

|애노테이션|쓰임|
|:---:|:---:|
|@Target*|애노테이션이 적용가능한 대상을 지정하는데 사용한다.|
|@Documented*|애노테이션 정보가 javadoc으로 작성된 문서에 포함되게 한다.|
|@Inherited*|애노테이션이 자속 클래스에 상속되도록 한다.|
|@Retention*|애노테이션이 유지되는 범위를 지정하는데 사용한다.|
|@Repeatable*|애노테이션을 반복해서 적용할 수 있게 한다. (JDK 1.8)|
<br>

## 3-1) @Target

- 어노테이션을 정의할 때, 적용대상 지정에 사용
  - 해당 어노테이션을 어디에 붙일 수 있는가를 지정한다.

**SuppressWarning의 소스코드를 살펴보자**<br>

```java
@Target( {TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE} )
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
   String[] value();
}
```
- 타입(클래스, 인터페이스, enum), 필드, 메섣, 생성자, 인스턴스 변수에 사용할 수 있는 어노테이션이다.
<br>


:point_right: **사용 대상**<br>

|대상 타입|설명
|:---:|:---:|
|ANNOTATION_TYPE|어노테이션|
|CONSTRUCTOR|생성자|
|FIELD|필드(멤버변수, enum상수)|
|LOCAL_VARIABLE|지역변수|
|METHOD|메서드|
|PACKAGE|패키지|
|PARAMETER|매개변수|
|TYPE|타입(클래스, 인터페이스,enum)|
|TYPE_PARAMETER|타입 매개변수(JDK 1.8)|
|TYPE_USE|타입이 사용되는 모든 곳(JDK 1.8)|


:point_right: `@Target` **사용 예시** <br>

```java
import static java.lang.annotation.ElementType.*;

@Target({ FIELD, TYPE, TYPE_USE })
public @interface MyAnnotation{ } // 적용대상이 FIELD(IV), TYPE(Class or Interface), TYPE_USE(RV)

@MyAnnotation // 적용대상이 TYPE인 경우
class MyClass{
   @MyAnnotation // 적용대상이 FIELD인 경우
   int i;

   @MyAnnotation // 적용대상이 TYPE_USE인 경우
   MyClass mc;
}
```

<br>

## 3-2) @Retention

- 어노테이션이 유지(retention)되는 기간을 지정하는데 사용한다. (유지 정책 결정)

|유지 정책|설명|
|:---:|:---:|
|SOURCE|소스 파일에만 존재. 클래스파일에는 존재하지 않음|
|CLASS|클래스 파일에 존재. 실행시에 사용불가. 기본값|
|RUNTIME|클래스 파일에 존재, 실행시에 사용가능|


:point_right: SOURCE 예시 <BR>

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override(){ }
```

- 컴파일러에 의해 사용되는 어노테이션의 유지 정책은 SOURCE이다. 
    - 오버라이딩이 제대로 되었는지 컴파일러가 확인하는 용도이기 때문에, 클래스 파일에 남길 필요 없이 컴파일시에만 확인하고 사라짐.


:point_right: RUNTIME 예시<BR>

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementaryType.TYPE)
public @interface FuntionalInterface{ }
```

- 실행시에  사용 가능한 어노테이션 정책은 RUNTIME이다.
   - 컴파일 타임을 포함해서 런타임까지 존재한다.

<br>

## 3-3) @Documented

- javadoc으로 작성한 문서에 포함시키려면 @Documented를 붙인다.
   - javadoc을 통해서 자동으로 html문서가 만들어진다. 이 문서에 포함시킬 내용

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Targer(ElementType.TYPE)
public @interface FunctionalInterface {}
```

<br>

## 3-4) @Inherited

- 어노테이션을 자손 클래스에 상속하고자 할 때, @Inherited를 붙인다.
 
```java
@Inherited //@SuperAnno가 자손까지 영향 미치게 함
@interface SuperAnno{}

@SuperAnno
class Parent{ }

class Child extends Parent{ } //Child에 애너테이션이 붙은 것으로 인식 
```

<br>

## 3-5) @Repeatable

- 반복해서 붙일 수 있는 어노테이션을 정의할 때 사용한다.

```java
@Repeatable(ToDos.class) // ToDo 애너테이션을 여러 번 반복해서 쓸 수 있게 한다.  
@interface ToDo{  
    String value();  
}
```

- @Repeatable이 붙은 어노테이션은 반복해서 붙일 수 있다.

```java
@ToDo("delete test codes.")  
@ToDo("override inherited methods")  
class Myclass{  
  
}
```

- @Repeatable인 @ToDo를 하나로 묶을 컨테이너 어노테이션도 정의해야 한다.

```java
@interface ToDos {  // 여러개의 ToDo애너테이션을 담을 컨테이너 애너테이션 ToDos
    ToDo[] value(); // ToDo애너테이션 배열 타입의 요소를 선언. 이름이 반드시 value여야 함  
}
```

<br>

## 4. 어노테이션 타입 정의방법

**어노테이션을 직접 만들어서 사용할 수 있다.  `@interface`를 사용해서 정의한다.**<br>

- 어노테이션의 메서드는 추상 메서드이며, 어노테이션을 적용할 때 지정한다. (순서 없음)

```java
@interface 어노테이션이름{
   타입 요소이름(); // 어노테이션의 요소를 선언한다.
}
```
<BR>

:point_right: **@TestInfo 어노테이션 정의해서 사용하기**<br>

```java
enum TestType { FIRST, FINAL }

// DateTime 어노테이션 정의
@interface DateTime{
   String yymmdd(); // 날짜 
   String hhmmss(); // 시간
}

// TestInfo 어노테이션 정의 : 다양한 타입을 요소로 가질 수 있다.(다른 어노테이션도 요소로 사용 가능)
@interface TestInfo{
   // 타입,  요소이름
   int count();
   String testedBy();
   String[] testTools();
   TestType testType(); // enum TestType { FIRST, FINAL }
   DateTime testDate(); // 자신이 아닌 다른 어노테이션(@DateTime)
}

// 어노테이션 사용
@TestInfo(count=3, testedBy="Kim", testTools={"JUnit","AutoTester"}, testType=TestType.FIRST, testDate=@DateTime(ymmdd="210204",hhmmss="112759"))
public class NewClass { ... }
```

<br>

## 4-2) 사용자 어노테이션의 특징

어노테이션의 요소를 정의하고 사용하는 방법을 알아보자.<BR>

**A. 디폴트 값을 지정할 수 있다.**<BR>
-  적용시 값을 지정하지 않으면, 사용될 수 있는 기본값을 지정한다.(null 제외)

```java
@interface TestInfo{
   int count() default 1; // 기본값을 1로 지정 
}

@TestInfo // TestInfo(count=1)과 동일
public class NewClass{ ... }
```
<br>

**B. 요소가 하나이고 이름이 value일 때는 요소의 이름을 생략할 수 있다.**<BR>

```java
@interface TestInfo{
   String value();
}

@TestInfo("passed") // @TestInfo(value="passed")와 동일
class NewClass { ... }
```
<br>

**C. 요소의 타입이 배열일 때,  요소가 여러개이면 괄호{}를 사용해야 한다.**<BR>

```java
@interface TestInfo{
   String[] testTools();
}

@TestInfo(testTools={"JUnit", "AutoTester"}) // 값이 여러개이면 괄호 필요
@TestInfo(testTools="JUnit") // 값이 하나면 괄호 불필요
@TestInfo(testTools={}) // 값이 없을 때는 괄호가 반드시 필요
```
<br>

**D. 모든 애너테이션의 조상은 Annotation이라는 인터페이스이다.**<br>

- 따라서 모든 애너테이션이 해당 인터페이스의 추상메서드를 상속받는다. 
- 이 때 해당 추상메서드들은 따로 구현하지 않아도 사용할 수 있다.  (컴파일러가 자동으로 추상메서드들을 구현해주기 때문)

```java
package java.lang.annotation;

public interface Annotation{ // Annotation자신은 인터페이스이다.
   boolean equals(Object obj);
   int hashCode();
   String toString();

   Class<? extends Annotation> annotationType(); // 애너테이션의 타입을 반환
}
```

하지만 명시적으로 상속은 불가능하다.

```java
@interface TestInfo extends Annotation{ // 에러. 허용되지 않는 표현
  int count();
  String testedBy();
  ... 
}
```
<br>

**E. 마커 어노테이션(Marker Annotation)은 요소가 하나도 정의되지 않은 애너테이션이다.**<br>

다음과 같이 사용할 수 있다.

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override{ } // 마커 애너테이션. 정의된 요소가 하나도 없다.

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Test{ } 

@Test // 정의된 요소가 없으므로 따로 넣어줄 값이 없다.
// 이 메서드가 테스트 대상임을 테스트 프로그램에 알리는 역할만 수행
public void method(){ ... }

@Deprecated
public int getDate(){ ... }
```

<br>

## 5. 어노테이션 요소의 규칙

**어노테이션의 요소를 선언할 때 아래의 규칙을 반드시 지켜야 한다.**<br>

1. `요소의 타입은 기본형, String, enum, 어노테이션, Class만 허용된다.`
2. `추상메서드의 괄호() 안에 매개변수를 선언할 수 없다.`
3. `예외를 선언할 수 없다.`
4. `요소를 타입 매개변수(ex. `<T>`)로 정의할 수 없다.`

- 잘못 사용한 예시
```java
@interface AnnoTest{
   int id = 100;
   String major(int i, int j); // 매개변수 사용 불가
   String minor() throws Exception; // 예외 선언 불가
   ArrayList<T> list(); // 타입 매개변수 정의 불가
}
```

<br>

:point_right: **예제: 어노테이션 값 가져오기** <br>

```java
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

enum TestType { 
    FIRST, SECOND;
}
@interface DateTime{
    String yymmdd();
    String hhmmss();
}

@Retention(RetentionPolicy.RUNTIME)
@interface TestInfo {
    int count() default 1;

    String testedBy();

    String[] testTools() default "JUnit"; // 배열 요소 (기본값 지정)

    TestType testType() default TestType.FIRST; // 상수 요소

    DateTime testDate(); // 어노테이션 요소
}

@SuppressWarnings("1111")  // 유효하지 않은 애너테이션은 무시된다.
@TestInfo(testedBy = "yeji", testDate = @DateTime(yymmdd = "20210202", hhmmss = "101055")) // 어노테이션 값 지정
public class PrintAnnotationTest {
    public static void main(String[] args) {
        Class<PrintAnnotationTest> cls = PrintAnnotationTest.class;

		// 어노테이션에 지정한 값들을 출력한다.
        TestInfo anno = cls.getAnnotation(TestInfo.class);
        System.out.println("anno.testedBy() : " + anno.testedBy());
        System.out.println("anno.testDate().yymmdd() : " + anno.testDate().yymmdd());
        System.out.println("anno.testDate().hhmmss() : " + anno.testDate().hhmmss());

        for(String str: anno.testTools()){
            System.out.println("testTools : "+str);
        }
        System.out.println();
    }
}
```

```java
anno.testedBy() : yeji
anno.testDate().yymmdd() : 20210202
anno.testDate().hhmmss() : 235959
testTools : JUnit
```

<br><br><br>

:orange_book: **References**<br>

- 자바의 정석
   - [애너테이션](https://www.youtube.com/watch?v=i4V8ZI9Undc&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=14)
   - [표준 애너테이션](https://www.youtube.com/watch?v=7eX1EB76Dio&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=144)
   - [메타 애너테이션](https://www.youtube.com/watch?v=p7KStWk8hWU&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=145)
   - [애너테이션 타입 정의하기, 애너테이션의 요소](https://www.youtube.com/watch?v=81U0MyuZQKo&list=PLW2UjW795-f6xWA2_MUhEVgPauhGl3xIp&index=146)

