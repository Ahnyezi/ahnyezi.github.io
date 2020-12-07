---
title:  "[Java] Java로 LinkedList 구현하고 JUnit5로 테스트하기"
date: 2020-12-06
categories: ['Java']
tags: ['Java','LinkedList']
---

Java로 LinkedList를 구현해보자! :raising_hand:<br>
<br><br>

### :bulb:  Requirements
- LinkedList에 대해 공부하세요.
- 정수를 저장하는 `ListNode 클래스`를 구현하세요.
- `ListNode add(ListNode head, ListNode nodeToAdd, int position)`를 구현하세요.
- `ListNode remove(ListNode head, int positionToRemove)`를 구현하세요.
- `boolean contains(ListNode head, ListNode nodeTocheck)`를 구현하세요.
<br><br>


## 연결리스트

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

> LinkedListImpl <br>

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


> LinkedListImplTest<br>

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
<br>

<img src="https://user-images.githubusercontent.com/62331803/101296740-707e1f00-3868-11eb-9fb3-8372a0afe46b.png" width="80%">
<br>


<br><br>

:orange_book: *References*<br>
- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
- [단위 테스트 활용 방법: JUnit 참조 가이드](https://brunch.co.kr/@pubjinson/16)
