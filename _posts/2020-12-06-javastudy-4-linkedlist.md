---
title:  "[Java] LinkedList 구현하고 JUnit5로 테스트하기"
date: 2020-12-06
categories: ['Java']
tags: ['Java','LinkedList']
---

**Java로 LinkedList를 구현해보자!** :raising_hand:<br>

### :bulb:  Requirements
- LinkedList에 대해 공부하세요.
- 정수를 저장하는 `ListNode 클래스`를 구현하세요.
- `ListNode add(ListNode head, ListNode nodeToAdd, int position)`를 구현하세요.
- `ListNode remove(ListNode head, int positionToRemove)`를 구현하세요.
- `boolean contains(ListNode head, ListNode nodeTocheck)`를 구현하세요.
<br><br>

### 목차
**1. 리스트 자료구조**<br>
- 1.1. 배열로 만든 리스트
- 1.2. 연결리스트

**2. 연결리스트**<br>
- 2.1. 노드
- 2.2. 연결리스트의 종류
- 2.3. 자바의 연결리스트

**3. 코드**<br>
- 3.1. 연결리스트 구현코드
- 3.2. 테스트코드
<br><br>


## 1. 리스트 자료구조

**List**는 다수의 데이터를 저장하는 자료구조로, **집합과 달리** 순서가 있게 데이터가 나열되어 있는 형태이다.<br>

기본적으로 삽입(insert), 삭제(remove), 검색(search) 등의 연산이 있으며, 대표적으로 **배열**과 **연결리스트** 형태로 리스트를 구현한다.
<br>
<br>

> **배열**과 **연결리스트**로 만든 리스트 자료구조<br>

<img src="https://user-images.githubusercontent.com/62331803/101842000-776ba100-3b8a-11eb-9ad9-97469ad85887.png" width="50%">

<br>

### 1.1. 배열로 만든 리스트

<img src="https://user-images.githubusercontent.com/62331803/101842012-7a669180-3b8a-11eb-8adf-e5cd98c417ff.png" width="35%"><br>

리스트를 표현하는 가장 대표적인 방법이다. 데이터를 **연속된 공간에 저장**하므로서 데이터들의 **순서 관계를 유지**할 수 있다. <br>

   - `랜덤 엑세스 가능` :  내가 원하는 칸이 어디에 위치해 있던지, 간단한 연산을 통해 메모리 주소를 계산하여 접근할 수 있다.
      -  `내가 원하는 메모리 주소 == 배열의 시작주소 + (내가 찾고자하는 칸의 인덱스 * 1칸의 크기)`

   - `크기가 고정` : 기존의 배열칸 이상의 데이터를 저장할 경우 **rellocation**이 필요하다.
   - `삽입과 삭제연산 비용 큼`: 리스트 중간에 원소를 삽입하거나 삭제할 경우, 다수의 데이터를 옮겨야 하기 때문에 비용이 크다.


### 1.2. 연결리스트

<img src="https://user-images.githubusercontent.com/62331803/101842015-7d618200-3b8a-11eb-8ac7-5c394c5c6e06.png" width="35%"><br>

**노드**들이 **링크로 연결**된 형태의 자료구조이다.  **데이터 영역**에 데이터를 저장하고, **포인터(링크)영역**에 다음/이전 노드로 가는 주소를 저장하여 **순서**를 나타낸다. <br>
  
   - `삽입과 삭제연산 비용 적음`: 다른 데이터의 이동없이 리스트 중간에 삽입 삭제할 수 있다.
   - `길이제한 없음`: 노드를 연결한 형태이기 때문에 길이제한이 없다.
   - `메모리 효율 낮음` : 노드를 연결하기 위한 포인터 저장 영역이 필요하기 때문에, 배열에 비해 메모리 효율이 낮다.
   - `랜덤 엑세스 불가능` : 연속된 메모리 주소에 데이터가 저장된 것이 아니기 때문에, 배열처럼 랜덤 액세스가 불가능하다.

<br>

종합적으로 보면 **요소의 삽입/삭제가 빈번**한 경우 **연결리스트**를 사용하고, **그렇지 않은 경우**라면 **배열**을 사용하는 것이 효과적이다.<br>
<br>


## 2. 연결리스트


### 2.1. 노드

**노드**는 **데이터와 다음 데이터로의 주소가 하나로 묶여있는 단위**를 의미한다.<br>

![image](https://user-images.githubusercontent.com/62331803/101843660-0b8b3780-3b8e-11eb-845a-a8f7d32d02a1.png)
<br>

- 각 노드는 하나의 **데이터 필드**와 하나의 **링크(next) 필드**로 구성된다.
-  **첫 번째 노드**의 주소는 따로 저장(head)해야 한다.
-  **마지막 노드**의 next 필드에는 **NULL**을 저장하여 연결리스트의 끝임을 표시한다.
<br>
<br>

### 2.2. 연결리스트의 종류

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


### 2.3. 자바의 연결리스트

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
        return head; // 새로운 연결리스트의 head를 반환
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
        return head; // 새로운 연결리스트의 head를 반환
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
- [자료구조 제11-1강 연결리스트 - 개념과 기본 동작들](https://www.youtube.com/watch?v=eTwYE-ercNM&t=1400s)
- [위키백과 : 연결리스트](https://ko.wikipedia.org/wiki/%EC%97%B0%EA%B2%B0_%EB%A6%AC%EC%8A%A4%ED%8A%B8)
- [단순 연결 리스트(singly linked list) - 정리 및 연습문제](https://atoz-develop.tistory.com/entry/%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-%EB%8B%A8%EC%88%9C-%EC%97%B0%EA%B2%B0-%EB%A6%AC%EC%8A%A4%ED%8A%B8-%EC%A0%95%EB%A6%AC-%EB%B0%8F-%EC%97%B0%EC%8A%B5%EB%AC%B8%EC%A0%9C)
- [LinkedList(Java Platform SE 7) - Oracle Help Center](https://docs.oracle.com/javase/7/docs/api/java/util/LinkedList.html)
- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
- [단위 테스트 활용 방법: JUnit 참조 가이드](https://brunch.co.kr/@pubjinson/16)
