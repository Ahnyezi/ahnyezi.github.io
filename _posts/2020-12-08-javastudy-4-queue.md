---
title:  "[Java] Queue 구현하고 JUnit5로 테스트하기"
date: 2020-12-08
categories: ['Java']
tags: ['Java']
---

**java로 Queue를 구현해보자!** :raising_hand:<br>
<br><br>

### :bulb:  Requirements
- `int 배열`을 사용해서 정수를 저장하는 Queue를 구현하세요.
- ListNode head를 가지고 있는 `ListNodeQueue` 클래스를 구현하세요.
- `void enqueue(int data)`를 구현하세요.
- `int dequeue()`을 구현하세요.
<br><br>


## 1. int 배열을 사용한 Queue

> 구현 코드 <br>

```java
package datastructure.queue;

public class QueueImpl implements Queue {
    private int[] queue;

    @Override
    public void enqueue(int data) {
        if (queue == null) {                    // 빈 큐일 경우
            queue = new int[]{data};
            return;
        }
                                                // 일반적인 경우
        int size = queue.length;
        int[] tmp = queue.clone();              // 기존의 배열을 깊은 복사
        queue = new int[size + 1];              // 사이즈 1칸 증가시킨 배열로 초기화
        for (int i = 0; i < size; i++) {
            queue[i] = tmp[i];                  // 기존 값으로 초기화
        }
        queue[size] = data;                     // 마지막 방 새로운 값으로 초기화
    }

    @Override
    public int dequeue() {
        if (queue == null) {throw new IndexOutOfBoundsException();}    // 빈 큐일 경우
        if (queue.length == 1) {                                       // 값이 하나만 존재할 경우
            int result = queue[0];
            queue = null;
            return result;
        }
                                                                        // 일반적인 경우
        int size = queue.length;
        int[] tmp = queue.clone();                                      // 기존의 배열을 깊은 복사
        queue = new int[size - 1];                                      // 사이즈 1칸 감소시킨 배열로 초기화
        for (int i = 1; i < size; i++) {
            queue[i - 1] = tmp[i];                                      // 기존 값으로 초기화
        }
        return tmp[0];                                                  // 기존 배열의 첫번째 방 반환
    }

    public String toString() {
        if (queue == null) {return "";}

        String result = "";
        for (int i = 0; i < queue.length; i++) {
            result += String.valueOf(queue[i]) + ',';
        }
        return result.substring(0, result.length() - 1);
    }
}

```

> 테스트 코드<br>

```java
package datastructure.queue;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class QueueImplTest {
    private QueueImpl queue;

    @BeforeEach
    public void init() {
        queue = new QueueImpl();
        queue.enqueue(1);
        queue.enqueue(3);
        queue.enqueue(5);
    }

    @Test
    @DisplayName("enqueue test")
    public void enqueueTest() {
        assertEquals("1,3,5", queue.toString());
    }

    @Test
    @DisplayName("dequeue test")
    public void dequeueTest() {
        assertAll("dequeue test",
                () -> {//큐(1,3,5)에서 1번 pop 한 결과
                    queue.dequeue();
                    assertEquals("3,5", queue.toString());
                },
                () -> {//큐(3,5)에서 1번 pop 한 결과
                    queue.dequeue();
                    assertEquals("5", queue.toString());
                },
                () -> {//큐(5)에서 1번 pop 한 결과
                    queue.dequeue();
                    assertEquals("", queue.toString());
                },
                () -> {//빈 큐에서 pop 시도할 경우
                    Exception exception = assertThrows(IndexOutOfBoundsException.class, () ->
                            queue.dequeue());
                });
    }
}


```
<br>


<img src="https://user-images.githubusercontent.com/62331803/101426674-c45c3700-3940-11eb-9c09-e6b8467cc71a.png" width="60%">
<br>
<br>

## 2. ListNode를 사용한 queue

> 구현코드 <br>

```java
package datastructure.queue;

import datastructure.linkedlist.ListNode;

public class LinkedNodeQueue implements Queue {
    private ListNode head;
    private int queueSize = 0; // 노드 개수

    @Override
    public void enqueue(int data) {
        queueSize += 1;
        if (head == null) {                                         // 빈 큐일 경우
            head = new ListNode(data);
            return;
        }

        getNodeAtThePosition(queueSize-1).next = new ListNode(data); // 마지막 방에 생성한 노드 연결
    }

    @Override
    public int dequeue() {
        int result;
        if (head == null){throw new IndexOutOfBoundsException();}    // 빈 큐일 경우
        if (queueSize == 1){                                        // 1개의 노드만 존재할 경우
            result = head.getData();
            head = null;
            queueSize = 0;
            return result;
        }
                                                                    // 일반적인 경우
        result = getNodeAtThePosition(1).getData();                 // 1번째 노드 result에 저장
        head = getNodeAtThePosition(2);                             // 2번째 노드를 head로 지정
        queueSize -= 1;
        return result;
    }

    // position 위치의 노드 반환
    public ListNode getNodeAtThePosition(int position) {// position : 1 ~ queueSize
        if (position < 1 || position > queueSize) {                 // position이 인덱스 범위를 벗어난 경우
            throw new IndexOutOfBoundsException();
        }
        if (position == 1) {                                        // 1번째 노드를 요구하는 경우
            return head;
        }
                                                                    // 일반적인 경우
        ListNode node = head;
        for (int i = 1; i < position; i++) {                        // position 위치까지 노드 순환시키기
            node = node.next;
        }
        return node;
    }

    public String toString(){
        if (head == null) return "";
        String result = "";
        for (int i=1;i<=queueSize;i++){
            result += String.valueOf(getNodeAtThePosition(i).getData())+',';
        }
        return result.substring(0,result.length()-1);
    }

}

```

> 테스트코드<br>

```java
package datastructure.queue;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class LinkedNodeQueueTest {
    private LinkedNodeQueue queue;

    @BeforeEach
    public void init() {
        queue = new LinkedNodeQueue();
        queue.enqueue(1);
        queue.enqueue(3);
        queue.enqueue(5);
    }

    @Test
    @DisplayName("enqueue test")
    public void enqueueTest() {
        assertEquals("1,3,5", queue.toString());
    }

    @Test
    @DisplayName("dequeue test")
    public void dequeueTest() {
        assertAll("dequeue test",
                () -> {//큐(1,3,5)에서 2번 pop 한 결과
                    queue.dequeue();
                    queue.dequeue();
                    assertEquals("5", queue.toString());
                },
                () -> {//큐(5)에서 1번 pop 한 결과
                    queue.dequeue();
                    assertEquals("", queue.toString());
                },
                () -> {//빈 큐에서 pop 시도할 경우
                    Exception exception = assertThrows(IndexOutOfBoundsException.class, () ->
                            queue.dequeue());
                }
        );

    }

}



```
<br>

<img src="https://user-images.githubusercontent.com/62331803/101426674-c45c3700-3940-11eb-9c09-e6b8467cc71a.png" width="60%">
<br>

<br><br>