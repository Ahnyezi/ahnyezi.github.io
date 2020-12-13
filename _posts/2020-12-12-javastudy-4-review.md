---
title:  "[whiteship 라이브스터디] 4주차 회고"
date: 2020-12-12
categories: ['Java']
tags: ['Java']
---

[whiteship 라이브스터디] 4주차
<br>

# 라이브 피드백 내용

## 1. 대시보드

- 선장님 코드<br>

[https://gist.github.com/whiteship/5f0d9f800c0cfb7867c59cfc4fd6d5a7](https://gist.github.com/whiteship/5f0d9f800c0cfb7867c59cfc4fd6d5a7)

- 대시보드코드 잘 짜신 분!<br>

[https://sowjd.github.io/docs/java/study-halle/week4](https://sowjd.github.io/docs/java/study-halle/week4)

<br><br>


## 2. JUnit5 사용법

### 2.1. 테스트 메서드에 public 접근제어자 쓸 필요 없다.

### 2.2.  `@displayname`은 테스트 출력과 메서드 이름을 다르게 해줄 때 사용하는 어노테이션이다.

 - 이모지나 한글을 사용해서 가독성 높게 한다. <br>

### 2.3. Stateful 한 테스트케이스 만들기

(참고: 전현우님 과제) [https://www.notion.so/4-c95ef49a7923425d84f403b5ba583bf4](https://www.notion.so/4-c95ef49a7923425d84f403b5ba583bf4) <br>

 JUnit은 기본적으로  인스턴스 상태 변화로 인해 예기치 않은 부작용이 발생하지 않도록, 테스트 메서드를 개별적으로 실행한다. <br>
   - `@TestInstance(Lifecycle.PER_METHOD)`가 원칙<br>

> 예제 : 테스트 메서드마다 인스턴스가 새로 생성되기 때문에, 출력값은 1,2아닌 1,1이 된다. 

```java
package junit;  
  
import org.junit.jupiter.api.Test;  
  
// @TestInstance(TestInstance.Lifecycle.PER_METHOD)가 default로 설정되어 있다.
class TestInstanceLifecycleTestTest {  
    int number = 0;  
  
  @Test  
  void addTest(){  
        System.out.println(++number);  
  }  
  
    @Test  
  void addTest2(){  
        System.out.println(++number);  
  }  
}
```

```java
1
1
```
<br>

하지만, Test Instance Lifecycle 옵션을 변경하여, 인스턴스를 클래스내부의 메서드가 공유하도록 할 수 있다. <br>

> 예제 : TestInstance가 클래스 단위로 생성되도록 변경하기.

```java
package junit;  
  
import org.junit.jupiter.api.Test;  
import org.junit.jupiter.api.TestInstance;  
  
@TestInstance(TestInstance.Lifecycle.PER_CLASS)  
class TestInstanceLifecycleTestTest {  
    int number = 0;  
  
  @Test  
  void addTest(){  
        System.out.println(++number);  
  }  
  
    @Test  
  void addTest2(){  
        System.out.println(++number);  
  }  
  
}
```

```java
1
2
```
<br>

**2.4. @Order 테스트 메서드 실행순서를 결정하는 어노테이션**<BR>

테스트 메서드의 실행 순서는 JUnit의 내부 알고리즘에 따라 결정된다.<br>
이 때 @Order를 사용하여 메서드 실행 순서를 지정해줄 수 있다. <br>
- 테스트 클래스에 `@TestMethodOrder(MethodOrderer.OrderAnnotation.class)` 어노테이션 추가
- 각 테스트 메서드에 `@Order()`로 순서 지정
<br>

> 예제 : 노드 이용한 스택 테스트

```java
package datastructure.stack;

import org.junit.jupiter.api.*;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertThrows;

@TestInstance(TestInstance.Lifecycle.PER_CLASS)
@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
public class ListNodeStackTest {
    private ListNodeStack stack;

    @BeforeAll
    public void init() {
        stack = new ListNodeStack();
        stack.push(1);
        stack.push(3);
        stack.push(5);
    }

    @Test
    @DisplayName("요소 추가 테스트")
    @Order(1)
    public void pushTest() {
        assertEquals("1,3,5", stack.toString());
    }


    @Test
    @DisplayName("요소 제거 테스트")
    @Order(2)
    public void popTest() {
        assertAll("요소 제거 오류",
                ()->{//스택(1,3,5)에서 2번 pop 한 결과
                    stack.pop();
                    stack.pop();
                    assertEquals("1",stack.toString());
                }
                ,
                ()->{//스택(1)에서 1번 pop 한 결과 null
                    stack.pop();
                    assertEquals("",stack.toString());
                },
                ()->{//빈 스택에서 pop 시도할 경우 예외처리
                    Exception exception = assertThrows(IndexOutOfBoundsException.class,()->
                            stack.pop());
                }
                );
    }
}

```


<br>


## 3. 코드리뷰

### 3.1. 잘 읽히는 코드
내가 짠 코드를 나중에 다시 이해할 수 있는 것은 기본이고, 내 코드를 읽는 다른 사람도 내 코드를 술술 읽을 수 있게 해라.<br>

- 특정 기능을 메서드로 빼기
- 메서드와 변수이름을 어떤 역할인지 알 수 있게 하기
- Operation의 일관성(consistency) 지키기
   - 예시 : Queue의 offer/add, poll/remove, peek/element => null or 예외
    - 일관성없는 코드 노노: 널이면 다 널하고, 예외면 다 예외로
<br>

### 3.2. 엣지케이스처리/시간복잡도
[참고: 최지우님 과제](https://www.notion.so/4-c45c85fea0254d5d8fbc911d7f66c046)

<br>
<br>

## 4. 내 과제 리뷰
- 테스트 코드에 public 쓸 필요 없음
- 클래스 명 dao-service 그렇게 하지 말래... (긁적)
- init() 그렇게 쓸 필요없다. 
   - 메소드마다 인스턴스 새로 생성되기 때문
<br>




<br><br>

## 회고

**진심으로 너무 x13837262 좋았다...** <br>
**스터디 듣고 반해서, 나의 선장님으로 모시기로 마음 먹었다. 따라 가겠습니다.** <br> 
<br>


**1. 제대로 된 공부 방향성을 제시**<br>

 나처럼 전공으로도 부전공으로도 심지어 대외활동으로도 코딩을 접해보지 않은 코린이들이 어떻게 공부를 해야 하는지에 대한 방향을 잡을 수 있다.<br>

 코딩 공부를 처음 시작했을 때, 내부동작원리는 전혀 모르는 상태로 문법만 주구장창 외웠다.  그러니까 정말 남는 게 없는 것 같았다... <br>
 
그것과 다르게 선장님이 내 주시는 스터디 과제는 **근본적인 동작 원리**를 다루는 부분이 많다. 자바 언어를 사용하는 데 있어 JVM 역할이 뭔지, 자바 코드는 어떻게 실행되는 것인지 등 근본적인 내용부터 다룬다는 점이 너무 좋았다.<br>
<br>

**2. 다양한 실력의 사람들과 함께 공부**<br>

제일 좋았던 점은 이거다.<br>

비전공자인 내가 공부하면서 가장 아쉬웠던 것은, 비슷한 분야를 공부하는 지인이 없다는 것이었다. 그래서 내가 제대로 공부하고 있는지, 내가 공부한 내용이 맞는 건지 알 수 있는 방법이 한정적이었다.<br>

그런 면에서 이 라이브 스터디는 많은 분들과 함께 공부할 수 있다는 장점이 있다. 심지어 이미 현업에 계신 분들도 계시기 때문에,  **같은 주제에 대해서 더 깊은 내용을 알 수 있다**는 것이 매우 뜻깊었다! <br>
<br>

**3. 선장님의 피드백**<br>

선장님께서 200명 가까이 되는 사람들의 과제를 하나하나 확인하시고, 하나씩 짚으면서 피드백을 해주시는 것을 보고 정말 감동받았다...ㅠㅠ<br>

이 라이브 스터디가 아니면 어떻게 마이크로 소프트 시니어 엔지니어의 피드백을 받을 수 있을까... <br>

굉장한 위치에서도 솔직하고 겸손하신 모습이 너무 멋졌고, 개발공부에 대한 피드백 뿐 아니라 과거 경험을 통해 여러가지 조언을 해주시는 것도 많은 자극이 되었다. <br>

이런 모습을 보면서 나중에 내가 가정을 꾸리고, 현직 개발자가 되어서 선장님처럼 할 수 있을까..? 라는 생각이 들었다. 저런 선한 영향력을 끼칠 수 있는 개발자가 되고 싶다. <br>
<br><br>

:point_right: **능동적인 공부하기**<br>
- 공부하고 싶은 부분 깊이 파기
- 발표
- 열심히 해야즤
<br><br>






