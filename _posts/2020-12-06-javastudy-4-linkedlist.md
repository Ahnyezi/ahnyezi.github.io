---
title:  "[Java] LinkedList 구현하고 JUnit5로 테스트하기"
date: 2020-12-06
categories: ['Java']
tags: ['Java','LinkedList']
---

**Java로 LinkedList를 구현해보자!** :raising_hand:<br>
<br><br>

### :bulb:  Requirements
- LinkedList에 대해 공부하세요.
- 정수를 저장하는 `ListNode 클래스`를 구현하세요.
- `ListNode add(ListNode head, ListNode nodeToAdd, int position)`를 구현하세요.
- `ListNode remove(ListNode head, int positionToRemove)`를 구현하세요.
- `boolean contains(ListNode head, ListNode nodeTocheck)`를 구현하세요.
<br><br>

## 1. 연결리스트 자료구조

![image](https://user-images.githubusercontent.com/62331803/101612807-687fd400-3a4e-11eb-9f3b-589ec5729f9a.png)
<br>


**연결리스트**는 **'노드'를 기반으로 데이터를 저장하는 선형 자료구조**이다. <br>

각 노드는 **데이터**와 **포인터**를 가지고 있으며, 포인터는 **다음이나 이전 노드와의 연결**을 담당한다. <br>
<br>

### 1.1 연결리스트의 특징

- 연결리스트는 순서를 가지고 있는 **선형 자료구조**이다
- 각 노드의 **포인터**로 **다른 노드와 연결**하여 **순서를 저장**한다.
- 포인터를 사용하기 때문에, **요소 삽입과 삭제 연산이 빠르다**는 장점이 있다.
- **하지만** 각 노드에 포인터를 저장할 공간이 필요하기 때문에 **메모리 공간 활용 효율이 낮다**.
<br>



### 1.2 연결리스트의 종류


**연결 리스트의 종류**로는 **단일 연결 리스트**와 **이중 연결 리스트** 등이 있다. 
<br>

> 단일연결리스트<br>

![image](https://user-images.githubusercontent.com/62331803/101612807-687fd400-3a4e-11eb-9f3b-589ec5729f9a.png)
<br>

**단일 연결 리스트**는 각 노드에 자료 공간과 **한 개의 포인터 공간**을 가진 형태를 말한다.  노드의 포인터는 **다음 노드**를 가리킨다.<br>


> 이중연결리스트<br>

![image](https://user-images.githubusercontent.com/62331803/101612819-6c135b00-3a4e-11eb-86bc-63376112d109.png)
<br>

**이중 연결 리스트**는 단일 연결 리스트와 비슷하지만, **포인터 공간이 2개**이고 각 포인터는 **앞의 노드**와 **뒤의 노드**를 가리킨다.<br>

 **이중연결리스트**는 앞 노드와 뒷 노드 모두 순회할 수 있기 때문에, **추가**와 **삭제** 연산에 있어서 더 효율적인 성능을 낼 수 있다. 
 하지만, 2개의 포인터를 사용하기 때문에 추가적인 공간이 필요하다.
<br><br>


## 2. 자바의 연결리스트

![image](https://user-images.githubusercontent.com/62331803/101623514-e7c7d480-3a5b-11eb-8fb1-0b97a15bdcb9.png)
<br>

**java.util.LinkedList**는 Serializable, Cloneable, Iterable, Collections, Deque, List, Queue와 같은 다양한 인터페이스를 구현하고 있으며, 특히 **List**와 **Deque** 인터페이스의 **이중연결리스트 구현체**이다. 

특징은 다음과 같다.<br>
- 모든 메서드는 **이중연결리스트**의 성능을 가지고 있다.
- 비동기적으로 구현되기 때문에, 여러 스레드가 동시에 LinkedList의 노드를 추가/삭제할 수 없다.
   - 즉, 외부적으로만 동기화 될 수 있다.
<br><br>




## 3. 코드

단일연결리스트로 구현하였다.<br>

> ListNode <br>

```java
package datastructure.linkedlist;

public class ListNode {
    private int data;
    public ListNode next;

    public ListNode(int data) {
        this.data = data;
        this.next = null;
    }

    public int getData() {
        return data;
    }
}

```
<br>


> 구현코드 <br>

```java
package datastructure.linkedlist;

import java.util.ArrayList;
import java.util.List;

public class LinkedListImpl implements LinkedList{

    public LinkedListImpl() {}

    public LinkedListImpl(int data) {
        add(null, new ListNode(data), 0);
    }

    public LinkedListImpl(ListNode nodeToAdd) {
        add(null, nodeToAdd, 0);
    }

    public LinkedListImpl(ArrayList<Integer> datas) {
        ListNode head = null;
        for (int i = 0; i < datas.size(); i++) {
            add(head, new ListNode(datas.get(i)), i);
        }
    }

    @Override
    public ListNode add(ListNode head, ListNode nodeToAdd, int position) {// position == index
        // position이 허용범위를 벗어난 경우
        if (!isAvailablePosition(head, position)) throw new IndexOutOfBoundsException();
        
        // 아직 연결된 노드가 없는 경우
        if (head == null) return nodeToAdd;

        // nodeToAdd를 head에 위치시키려는 경우
        if (position == 0) {
            nodeToAdd.next = head;
            return nodeToAdd;
        }

        ListNode prevNode = getNodeAtThePosition(head, position - 1);// 삽입하려는 위치의 바로 이전 노드(position - 1)를 가져온다.
        nodeToAdd.next = prevNode.next;                                     // 삽입하려는 노드(nodeToAdd)에 기존의 linkedlist에서 position에 위치했던 node를 연결시킨다.
        prevNode.next = nodeToAdd;                                          // 삽입하려는 위치의 바로 이전 노드에 삽입하려는 노드를 연결시킨다.
        return head;
    }

    @Override
    public ListNode remove(ListNode head, int positionToRemove) {// positionToRemove == index
        // positionToRemove가 가능범위를 벗어난 경우
        if (!isAvailablePosition(head, positionToRemove) ||
                positionToRemove == getSize(head)) throw new IndexOutOfBoundsException();
        
        // 연결리스트가 null인 경우
        if (head == null) return null;

        // head를 제거하는 경우
        if (positionToRemove == 0) return head.next;

        ListNode prevNode = getNodeAtThePosition(head, positionToRemove - 1);// 제거하려는 위치의 바로 이전 노드를 가져온다.
        prevNode.next = prevNode.next.next;                                         // prevNode에 해당 노드의 다다음 노드를 연결시킨다.
        return head;
    }

    @Override
    public boolean contains(ListNode head, ListNode nodeToCheck) {
        while (head != null) {
            if (head.getData() == nodeToCheck.getData()) {//순회하며 가져온 노드의 값과 nodeToCheck의 값이 일치하는지 확인한다.
                return true;
            }
            head = head.next;//연결리스트를 head부터 tail까지 순회
        }
        return false;
    }

    // position이 허용가능한 인덱스 값인지 확인
    public boolean isAvailablePosition(ListNode head, int position) {
        if (position < 0 || position > (getSize(head))) { // available value : 0 <= position <= linkedlist 크기
            return false;
        }
        return true;
    }

    // 연결리스트의 head를 기준으로 순회하여, position 위치에 있는 노드를 반환
    public ListNode getNodeAtThePosition(ListNode head, int position) {
        for (int i = 0; i < position; i++) {
            head = head.next;
        }
        return head;
    }

    // 현재 노드가 다음 노드와 연결되어 있는지 확인
    public boolean hasNext(ListNode head) {
        if (head.next == null) return false;
        return true;
    }

    // 연결리스트의 크기(노드 개수)를 반환
    public int getSize(ListNode head) {
        if (head == null) return 0;
        if (!hasNext(head)) return 1;
        ListNode node = head.next;
        int size = 2;
        while (hasNext(node)) {
            size++;
            node = node.next;
        }
        return size;
    }

    public String toString(ListNode head) {
        List<String> nodes = new ArrayList<>();
        while (head != null) {
            nodes.add(String.valueOf(head.getData()));
            head = head.next;
        }
        return String.join(",", nodes);
    }
}

```
<br>


> 테스트코드<br>

```java
package datastructure.linkedlist;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertAll;
import static org.junit.jupiter.api.Assertions.assertTrue;
import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class LinkedListImplTest {
    private LinkedListImpl list;
    private ListNode head;

    @BeforeEach
    public void init() {
        list = new LinkedListImpl();
        head = list.add(null, new ListNode(1), 0);
        head = list.add(head, new ListNode(3), 1);
        head = list.add(head, new ListNode(5), 2);
        head = list.add(head, new ListNode(7), 3);
    }

    @Test
    @DisplayName("addTest")
    public void addTest() {
        assertAll("add test",
                () -> {// init()에서 생성한 linkedlist를 확인
                    assertEquals("1,3,5,7", list.toString(head));
                },
                () -> {// 허용범위를 벗어난 position에 add하려는 경우
                    Exception exception = assertThrows(IndexOutOfBoundsException.class, () ->
                            list.add(head, new ListNode(9), list.getSize(head) + 1));
                }
        );
    }

    @Test
    @DisplayName("removeTest")
    public void removeTest() {
        assertAll("remove test",
                () -> {// 연결리스트(1,3,5,7)에서 위치가 3(idx 기준)인 node 삭제
                    assertEquals("1,3,5", list.toString(list.remove(head, 3)));
                },
                () -> {// 허용범위를 벗어난 position의 노드를 remove하려는 경우
                    Exception exception = assertThrows(IndexOutOfBoundsException.class, () ->
                            list.remove(head, list.getSize(head)));
                });
    }

    @Test
    @DisplayName("containsTest")
    public void containsTest() {
        assertAll("contains test",
                () -> {// 연결리스트(1,3,5,7)가 1을 포함하는지 확인
                    assertTrue(list.contains(head, new ListNode(1)));
                },
                () -> {// 연결리스트(1,3,5,7)가 11을 포함하지 않는지 확인
                    assertFalse(list.contains(head, new ListNode(11)));
                });
    }
}
```
<br>

<img src="https://user-images.githubusercontent.com/62331803/101296740-707e1f00-3868-11eb-9fb3-8372a0afe46b.png" width="80%">
<br>


<br><br>

:orange_book: *References*<br>
- [위키백과 : 연결리스트](https://ko.wikipedia.org/wiki/%EC%97%B0%EA%B2%B0_%EB%A6%AC%EC%8A%A4%ED%8A%B8)
- [단순 연결 리스트(singly linked list) - 정리 및 연습문제](https://atoz-develop.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EB%8B%A8%EC%88%9C-%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8-%EC%A0%95%EB%A6%AC-%EB%B0%8F-%EC%97%B0%EC%8A%B5%EB%AC%B8%EC%A0%9C)
- [LinkedList(Java Platform SE 7) - Oracle Help Center](https://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html)
- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
- [단위 테스트 활용 방법: JUnit 참조 가이드](https://brunch.co.kr/@pubjinson/16)
