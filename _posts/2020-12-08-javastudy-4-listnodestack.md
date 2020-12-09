---
title:  "[Java] node 사용하여 Stack 구현하고 JUnit5로 테스트하기"
date: 2020-12-08
categories: ['Java']
tags: ['Java']
---

**node를 사용하여 Stack을 구현해보자!** :raising_hand:<br>
<br><br>

### :bulb:  Requirements
- ListNode head를 가지고 있는 `ListNodeStack` 클래스를 구현하세요.
- `void push(int data)`를 구현하세요.
- `int pop()`을 구현하세요.
<br><br>


## 스택

> 스택 구현코드<br>

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

> 테스트코드<br>

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