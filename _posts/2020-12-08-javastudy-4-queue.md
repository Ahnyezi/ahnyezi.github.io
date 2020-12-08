---
title:  "[Java] int 배열 Queue 구현하고 JUnit5로 테스트하기"
date: 2020-12-08
categories: ['Java']
tags: ['Java']
---

**int 배열을 사용하여 Queue를 구현해보자!** :raising_hand:<br>
<br><br>

### :bulb:  Requirements
- `int 배열`을 사용해서 정수를 저장하는 Queue를 구현하세요.
- `void enqueue(int data)`를 구현하세요.
- `int dequeue()`을 구현하세요.
<br><br>


## 큐

> 큐 구현 코드 <br>

```java
package datastructure.queue;

public class QueueImpl implements Queue {
    private int[] queue;

    @Override
    public void enqueue(int data) {
        if (queue == null) {
            queue = new int[]{data};
            return;
        }

        int size = queue.length;
        int[] tmp = queue.clone();
        queue = new int[size + 1];
        for (int i = 0; i < size; i++) {
            queue[i] = tmp[i];
        }
        queue[size] = data;
    }

    @Override
    public int dequeue() {
        if (queue == null) {throw new IndexOutOfBoundsException();}
        if (queue.length == 1) {
            int result = queue[0];
            queue = null;
            return result;
        }

        int size = queue.length;
        int[] tmp = queue.clone();
        queue = new int[size - 1];
        for (int i = 1; i < size; i++) {
            queue[i - 1] = tmp[i];
        }
        return tmp[0];
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

<br><br>