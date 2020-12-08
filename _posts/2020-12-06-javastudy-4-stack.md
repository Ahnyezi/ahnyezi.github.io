---
title:  "[Java] int 배열로 Stack 구현하고 JUnit5로 테스트하기"
date: 2020-12-06
categories: ['Java']
tags: ['Java']
---

*** int 배열로 Stack을 구현해보자!*** :raising_hand:<br>
<br><br>

### :bulb:  Requirements
- `int 배열`을 사용해서 정수를 저장하는 Stack을 구현하세요.
- `void push(int data)`를 구현하세요.
- `int pop()`을 구현하세요.
<br><br>


## 스택

> StackImpl

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

> StackImplTest<br>

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

<br><br>