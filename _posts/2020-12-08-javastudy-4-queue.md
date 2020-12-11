---
title:  "[Java] Queue 구현하고 JUnit5로 테스트하기"
date: 2020-12-08
categories: ['Java']
tags: ['Java']
---

**java로 Queue를 구현해보자!** :raising_hand:<br>
<br>

### :bulb:  Requirements
- `int 배열`을 사용해서 정수를 저장하는 Queue를 구현하세요.
- ListNode head를 가지고 있는 `ListNodeQueue` 클래스를 구현하세요.
- `void enqueue(int data)`를 구현하세요.
- `int dequeue()`을 구현하세요.
<br><br>



### 목차
**1. 큐**<br>

- 1.1. 큐의 개념
- 1.2. 큐의 기본연산
- 1.3. 큐의 활용
- 1.4. 자바의 큐 인터페이스

**2. 코드**<br>

- 2.1.  `int 배열`을 사용한 큐
   - 2.1.1. 구현코드
   - 2.1.2. 테스트코드
- 2.2. **노드**를 사용한 큐
   - 2.2.1. 구현코드
   - 2.2.2. 테스트코드


<br><br>


## 1. 큐

### 1.1. 큐의 개념

<img src="https://user-images.githubusercontent.com/62331803/101854382-c7576180-3ba4-11eb-83bf-16ce8d25a59f.png" width="50%"><br>


- **제약조건이 있는 리스트**<br>

스택과 마찬가지로 제약조건이 있는 리스트이다. 데이터 삽입은 한쪽 끝에서, 삭제는 반대쪽 끝에서만 일어나도록 만든 구조이다. <br>
삽입이 일어나는 쪽을 `rear`, 삭제가 일어나는 쪽을 `front`라 부른다.<br>

-  **FIFO(First-In, First-Out)**<br>

**Queue**라는 용어의 의미는 대기행렬이다. 말 그대로 **가장 먼저온 요소가 가장 먼저 나가고**, **가장 나중에 온 요소가 가장 나중에 나간다**. <br>

큐는 스택보다 더 흔하게 볼 수 있는 형태의 구조다. **여러 개가 하나의 공유자원을 줄 서서 기다리는 경우 대부분 큐를 사용**한다.<br>
<br>

### 1.2. 큐의 기본연산
- `insert/enqueue/offer/push`: 큐의 rear에 새로운 원소를 삽입하는 연산
- `remove/dequeue/poll/pop`: 큐의 front에  있는 원소를 큐로부터 삭제하고 반환하는 연산
- `peek/element/front`: 큐의 front에 있는 원소를 제거하지 않고 반환하는 연산
- `is_empty`: 큐가 비었는지 검사
<br>

### 1.3. 큐의 활용

**1. CPU 스케쥴링**<br>
multitasking(동시에 여러 개의 프로그램이 실행되는) 환경에서 프로세스들은 큐에서 CPU가 할당되기를 기다린다. <br>

**2. 데이터 버퍼**<br>
네트워크를 통해 전송되는 패킷(packet)들은 도착한 순서대로 버퍼에 저장되어 처리되기를 기다린다. <br>

**3. 자원을 공유하는 대부분의 경우**<br>
여러 장의 문서를 프린트 하는 경우 등..<br>
<br>


### 1.4. 자바의 큐 인터페이스

오라클 공식 문서에 따르면 다음과 같다.<br>

<img src="https://user-images.githubusercontent.com/62331803/101855172-56b14480-3ba6-11eb-84d7-deb05ebc7b6e.png" width="50%"><br>

**java.util.Queue**는 인터페이스로서 존재하며, 해당 인터페이스를 구현한 `PriorityQueue` ,`LinkedTransferQueue`, `PriorityBlockingQueue`, `SynchronousQueue`가 존재한다.<br>

일반적으로는 FIFO 방식으로 요소를 정렬하지만 반드시 그래야하는 것은 아니며, 해당 인터페이스를 구현한 우선순위큐(PriorityBlockingQueue)는 LIFO구조를 가진다. <br>
<br>
<br>


### 2.1. int 배열을 사용한 Queue

> int배열 큐 구현코드 <br>

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

> int 배열 큐 테스트코드<br>

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

### 2.2. ListNode를 사용한 큐

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
<br>

:orange_book: References<br>
- [큐의 개념과 구현](https://www.youtube.com/watch?v=_aU2e70gYlo&list=PL52K_8WQO5oXIATx2vcTvqwxXxoGxxsIz&index=40)
- [java.util.Queue](https://docs.oracle.com/javase/7/docs/api/java/util/Queue.html)


<br><br>