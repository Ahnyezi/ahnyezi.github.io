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
<br><br>



### 목차
1. **스택**<br>
- 1.1. 스택의 개념
- 1.2. 스택의 기본연산
- 1.3. 스택의 활용 : 괄호검사문제

2. **코드**<br>
- 2.1. int 배열을 사용한 스택
   - 2.1.1. 구현코드
   - 2.1.2. 테스트코드
- 2.2. ListNode를 사용한 스택
   - 2.2.1. 구현코드
   - 2.2.2. 테스트코드

<br><br>



## 1. 스택

### 1.1. 스택의 개념

<img src="https://user-images.githubusercontent.com/62331803/101846354-16e16180-3b94-11eb-8a39-8713bc72d8b9.png" width="50%"><br>

- **제약조건이 있는 리스트**
리스트란 데이터가 순서가 있게 나열된 구조를 말한다. 배열이나 연결리스트 형태로 된 리스트의 경우, 리스트의 모든 요소에 대해 삽입/삭제가 가능했다. 이와 달리 **스택은 리스트의 한 쪽 끝에서만 삽입/삭제가 이루어지도록 만든 구조**이다.

-  **LIFO(Last-In, First-Out)**
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
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class StackImplTest {
    private StackImpl stack;

    @BeforeEach
    public void init() {
        stack = new StackImpl();
    }

    @Test
    public void pushTest() {
        stack.push(1);
        stack.push(3);
        stack.push(5);
        stack.push(7);
        assertEquals("1,3,5,7", stack.toString());
    }

    @Test
    public void popTest() {
        pushTest();
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


<br><br>