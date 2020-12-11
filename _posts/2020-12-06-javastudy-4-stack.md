---
title:  "[Java] Stack 구현하고 JUnit5로 테스트하기"
date: 2020-12-06
categories: ['Java']
tags: ['Java']
---

**Java로 Stack을 구현해보자!** :raising_hand:<br>
<br>

### :bulb:  Requirements
- `int 배열`을 사용해서 정수를 저장하는 Stack을 구현하세요.
   - `void push(int data)`를 구현하세요.
   - `int pop()`을 구현하세요.
- ListNode head를 가지고 있는 `ListNodeStack` 클래스를 구현하세요.
   - `void push(int data)`를 구현하세요.
   - `int pop()`을 구현하세요.
<br><br>



### 목차
1. **스택**

- 1.1. 스택의 개념
- 1.2. 스택의 기본연산
- 1.3. 스택의 활용 : 괄호검사문제
- 1.4. 자바의 스택

2. **코드**

- 2.1. **int 배열**을 사용한 스택
   - 2.1.1. 구현코드
   - 2.1.2. 테스트코드
- 2.2. **노드**를 사용한 스택
   - 2.2.1. 구현코드
   - 2.2.2. 테스트코드

<br><br>



## 1. 스택

### 1.1. 스택의 개념

<img src="https://user-images.githubusercontent.com/62331803/101846354-16e16180-3b94-11eb-8a39-8713bc72d8b9.png" width="50%"><br>

- **제약조건이 있는 리스트**<br>

리스트란 데이터가 순서가 있게 나열된 구조를 말한다. 배열이나 연결리스트 형태로 된 리스트의 경우, 리스트의 모든 요소에 대해 삽입/삭제가 가능했다. 이와 달리 **스택은 리스트의 한 쪽 끝에서만 삽입/삭제가 이루어지도록 만든 구조**이다.

-  **LIFO(Last-In, First-Out)**<br>

삽입/삭제가 `top`에서만 일어나기 때문에, 가장 **먼저 들어온 요소가 가장 나중에 나가고**, 가장 **나중에 들어온 요소가 가장 먼저 나간다**.

<br>

### 1.2. 스택의 기본연산

- `push()`: 스택에 새로운 원소를 삽입
- `pop()`: 스택의 top에 있는 원소를 스택에서 제거하고 반환
- `peek()`: 스택의 top의 원소를 제거하지 않고 반환
- `empty()`: 스택이 비었는지 검사

<br>

### 1.3. 스택의 활용 : 괄호 검사 문제

입력 수식의 괄호가 올바른지 검사한다.<br>
`[a+b*{c/(d-e)}]+(d/e)`<br>
여는 괄호와 닫는 괄호의 개수 비교만으로는 괄호의 순서를 확인할 수 없기 때문에 스택이 필요하다.<br>

- 스택을 활용한 방법
   - **여는 괄호**가 나오면 스택에 `push`
   - **닫는 괄호**가 나오면 `pop`을 실행한 결과가 **동일한 유형인지 검사**
   - 마지막에 **스택에 남는 괄호가 없는지 검사**


<img src="https://user-images.githubusercontent.com/62331803/101847809-66755c80-3b97-11eb-8c3e-b9f6f6d663d9.png" width="50%"><br>
<br>

### 1.4. 자바의 스택

오라클 공식 문서에 따르면 다음과 같다.<br>

<img src="https://user-images.githubusercontent.com/62331803/101850280-d4705280-3b9c-11eb-9120-cc0782bff043.png" width="50%"><br>

**java.util.Stack**은 Serializable, Cloneable, Iterable, Collection, List, RandomAccess 인터페이스를 구현한 구현체이며, Vector 클래스를 상속받고 있다.<br>
Stack 클래스는 LIFO의 스택 자료구조를 구현한 클래스이지만, 완전하고 일관된 형태의 LIFO 구조를 사용하기 위해서는 Deque 인터페이스를 구현하여 사용하는 것이 더 효과적이다.<br>

```java
      Deque<Integer> stack = new ArrayDeque<Integer>();
```

<br>

`Stack`클래스가 있는데 굳이  `Deque`를 구현해서 사용하는 것에 대해 이해가 되지 않아서 조사해봤다.<br>

[출처:Why should I use Deque over Stack?](https://stackoverflow.com/questions/12524826/why-should-i-use-deque-over-stack)

**1. 객체지향관점에서의 문제 Object oriented design** <br>

인터페이스가 다중상속이 가능한 것과 달리, 클래스는 하나의 클래스로만 extends 될 수 있다. **Deque 인터페이스**를 사용하면 구체적인 Stack 클래스와 그 조상에 대한 종속성이 제거되어 **더 유연하게 사용할 수 있다**. 즉, 다른 클래스를 extends하거나 Deque의 다른 구현체(LinkedList, ArrayDeque)등으로 교체하는 것이 자유롭다.<br>

**2. 일관성 문제 Inconsistency** <br>

Stack클래스는 **Vector 클래스**를 상속받은 형태이다. 따라서 랜덤액세스가 가능한데 스택의 원칙인 **LIFO에 불필요한 작업을 수행**할 수 있게 된다. Deque를 사용하는 경우 랜덤액세스가 불가능하다. <br>

**3. 성능 문제 Performance**<br>

Stack클래스가 상속받는 Vector 클래스는 ArrayList의 **thread-safe**(멀티 스레드가 접근하는 것을 허용) 버전이다. 이런 식으로 **동기화**를 할 경우, **어플리케이션에 성능 저하**를 일으킬 가능성이 있다. 또한 굳이 필요하지 않은 기능 사용하는 다른 클래스를 확장할 경우(2와 마찬가지로) 사용하는 객체가 커져서 **추가 메모리와 성능 오버헤드**를 발생시킬 수 있다. <br>

<br>

## 2. 코드
### 2.1. int 배열을 사용한 스택

> int 배열 스택 구현코드<br>


 ```java

package datastructure.stack;

public class StackImpl implements Stack {
    private int arr[];

    @Override
    public void push(int data) {
        // data가 arr의 첫번째 원소가 되는 경우
        if (arr == null) {
            arr = new int[]{data};
            return;
        }
        
        int size = arr.length;          // 기존 arr 크기 가져오기
        int tmp[] = arr.clone();        // 깊은 복사
        arr = new int[size + 1];        // arr 크기 증가
        for (int i = 0; i < size; i++) {// 새로운 arr에 기존 값 삽입
            arr[i] = tmp[i];
        }
        arr[size] = data;// 마지막 방에 data 삽입
    }

    @Override
    public int pop() {
        // pop할 원소가 없는 경우
        if (arr == null) throw new IndexOutOfBoundsException();

        // arr 원소가 1개인 경우
        if (arr.length == 1) {
            int result = arr[0];
            arr = null;
            return result;
        }

        int size = arr.length;              // 기존 arr 크기 가져오기
        int tmp[] = arr.clone();            // 깊은 복사
        arr = new int[size - 1];            // arr 크기 감소
        for (int i = 0; i < size - 1; i++) {// 새로운 arr에 기존 값 삽입
            arr[i] = tmp[i];
        }
        return tmp[size - 1];
    }

    public String toString() {
        if (arr == null) return "";
        String result = "";
        for (int i = 0; i < arr.length; i++) {
            result += String.valueOf(arr[i]) + ',';
        }
        return result.substring(0, result.length() - 1);
    }
}
 
```


<br>

> int배열 스택 테스트코드<br>


```java
package datastructure.stack;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class StackImplTest {
    private StackImpl stack;

    @BeforeEach
    public void init() {
        stack = new StackImpl();
        stack.push(1);
        stack.push(3);
        stack.push(5);
        stack.push(7);
    }

    @Test
    @DisplayName("push test")
    public void pushTest() {
        assertEquals("1,3,5,7", stack.toString());
    }

    @Test
    @DisplayName("pop test")
    public void popTest() {
        assertAll("pop test",
                () -> {//스택(1,3,5,7)에서 2번 pop한 후의 배열 확인
                    stack.pop();
                    stack.pop();
                    assertEquals("1,3", stack.toString());
                },
                () -> {//스택(1,3)에서 2번 pop한 후의 배열 확인
                    stack.pop();
                    stack.pop();
                    assertEquals("", stack.toString());
                },
                () -> {//스택(null)에서 pop을 시도하면 예외 처리
                    Exception exception = assertThrows(IndexOutOfBoundsException.class, () ->
                            stack.pop());
                });
    }
}

```
<br>


<img src="https://user-images.githubusercontent.com/62331803/101296999-efc02280-3869-11eb-973c-9355fae6357c.png" width="60%">
<br>
<br>

### 2.1. ListNode를 사용한 스택

> 노드를 이용한 스택 구현코드<br>


```java
package datastructure.stack;

import datastructure.linkedlist.ListNode;

public class ListNodeStack implements Stack {
    private ListNode head;
    private int stackSize = 0; // 노드 개수

    @Override
    public void push(int data) {
        stackSize += 1;
        if (head == null) {                                             // 스택이 비어있는 경우
            head = new ListNode(data);
            return;
        }
                                                                        // 비어있지 않은 경우
        getNodeAtThePosition(stackSize - 1).next = new ListNode(data);  // 노드 생성하여 기존의 마지막 노드와 연결
    }

    @Override
    public int pop() {
        int result;
        if (head == null) {throw new IndexOutOfBoundsException();}      // 스택이 비어있는 경우
        if (stackSize == 1) {                                           // 스택의 노드가 하나인 경우
            result = head.getData();
            head = null;
            stackSize = 0;
            return result;
        }
                                                                        // 일반적인 경우
        ListNode prevNode = getNodeAtThePosition(stackSize - 1);        // 끝에서 2번째 노드를 가져온다
        result = prevNode.next.getData();                               // 마지막 노드의 data를 result에 저장
        prevNode.next = null;                                           // 끝에서 2번째 노드의 link를 null로 초기화
        stackSize -= 1;                                                 // 사이즈 하나 감소
        return result;
    }

    // position 위치의 노드를 반환
    public ListNode getNodeAtThePosition(int position) {// position: 1 ~ stackSize
        if (position < 1 || position > stackSize) { throw new IndexOutOfBoundsException(); } // 인덱스가 범위를 벗어난 경우
        if (position == 1) { return head; }                                                 // 첫번째 노드를 요구하는 경우

        ListNode node = head;
        for (int i = 1; i < position; i++) {                                                // 일반적인 경우
            node = node.next;
        }
        return node;
    }


    public String toString() {
        if (head == null) return "";

        String result = String.valueOf(head.getData()) + ',';
        while (head.next != null) {
            head = head.next;
            result += String.valueOf(head.getData()) + ',';
        }
        return result.substring(0, result.length() - 1);
    }
}


```
<br>

> 노드를 이용한 스택 테스트코드<br>


```java
package datastructure.stack;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class ListNodeStackTest {
    private ListNodeStack stack;

    @BeforeEach
    public void init() {
        stack = new ListNodeStack();
        stack.push(1);
        stack.push(3);
        stack.push(5);
    }

    @Test
    @DisplayName("push test")
    public void pushTest() {
        assertEquals("1,3,5", stack.toString());
    }


    @Test
    @DisplayName("pop test")
    public void popTest() {
        assertAll("pop test",
                ()->{//스택(1,3,5)에서 2번 pop 한 결과
                    stack.pop();
                    stack.pop();
                    assertEquals("1",stack.toString());
                },
                ()->{//스택(1)에서 1번 pop 한 결과 null
                    stack.pop();
                    assertEquals("",stack.toString());
                },
                ()->{//빈 스택에서 pop 시도할 경우 예외처리
                    Exception exception = assertThrows(IndexOutOfBoundsException.class,()->
                            stack.pop());
                });
    }
}

```
<br>


<img src="https://user-images.githubusercontent.com/62331803/101426745-e5248c80-3940-11eb-92eb-fddb9a6c5641.png" width="60%">
<br>
<br>

:orange_book: References<br>
- [스택의 개념과 구현](https://www.youtube.com/watch?v=MaLRjxoaowA&list=PL52K_8WQO5oXIATx2vcTvqwxXxoGxxsIz&index=32)



<br><br>