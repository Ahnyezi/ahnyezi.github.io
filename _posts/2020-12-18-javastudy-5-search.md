---
title:  "[Java] DFS와 BFS로 이진트리 탐색하기"
date: 2020-12-18
categories: ['Java']
tags: ['Java']
---

**DFS와 BFS로 이진트리 탐색하기** :raising_hand:<br>
<br>


:bulb: **requirements**<br>

-   int 값을 가지고 있는 이진 트리를 나타내는 Node 라는 클래스를 정의하세요.
-   int value, Node left, right를 가지고 있어야 합니다.
-   BinrayTree라는 클래스를 정의하고 주어진 노드를 기준으로 출력하는 bfs(Node node)와 dfs(Node node) 메소드를 구현하세요.
-   DFS는 왼쪽, 루트, 오른쪽 순으로 순회하세요.

<br><br>


# 1. 깊이우선탐색(DFS)

**재귀와 반복문을 사용해서 각각 구현해보았다.** <br>
<br>

## 1.1. 재귀를 사용한 DFS

**리프 노드를 만날 때까지 재귀적으로 함수를 호출한다.** <br>

```java
public void dfsRecursive(Node node){
        if (node == null) return;
        dfsRecursive(node.left);
        System.out.print(node.data+" ");
        dfsRecursive(node.right);
 }
```
<br>

## 1.2. 반복문을 사용한 DFS

**방문여부를 표시하는 방법과 표시하지 않는 방법으로 각각 구현해보았다.**<br> 

### 1.2.1. 방문여부 표시한 코드

```java
 public void dfsIterative1(Node node){
        Deque<Node> stack = new ArrayDeque<>();
        Map<Node,Integer> visited = new HashMap<>(); // 방문여부 체크를 위한 map

        Node current = node;
        stack.push(node);
        visited.put(node,1);

        while (! stack.isEmpty()){

            while(current.left != null && !visited.containsKey(current.left)){
                current = current.left;
                stack.push(current);
            }

            current = stack.pop();
            System.out.print(current.data+" ");
            visited.put(current,1);// 출력한 노드 방문체크

            if (current.right != null && !visited.containsKey(current.right)){
                current = current.right;
                stack.push(current);
            }
        }
    }
```
<br>

### 1.2.1. 방문여부 표시 없이 구현한 코드

```java
 public void dfsIterative2(Node node){
        Deque<Node> stack = new ArrayDeque<>();
        Node current = node;
        // 현재 노드에서 가장 좌측 하단의 노드로 이동한다
        while(!stack.isEmpty() || current != null){

            while(current != null){
                stack.push(current);
                current = current.left;
            }
            // 현재 시점에서 current는 항상 null이다
            current = stack.pop();
            System.out.print(current.data +" ");

            current = current.right;
        }
    }
```
<br>
<br>


# 2. 너비우선탐색

```java
public void bfs(Node node){
        Queue<Node> queue = new LinkedList<>();
        queue.add(node);

        while(!queue.isEmpty()){
            Node current = queue.poll();
            System.out.print(current.data + " ");
            if (current.left != null) queue.add(current.left);
            if (current.right != null) queue.add(current.right);
        }
    }
```
<br><br>

# 3. 테스트

```java
public static void main(String[] args) {
    /*
                (3)
              ↙     ↘
           (6)        (9)
         ↙          ↙     ↘
     (12)        (15)     (18)
        ↘
         (21)

    bfs          : 3 -> 6 -> 9 -> 12 -> 15 -> 18 -> 21
    dfs(inorder) : 12 -> 21 -> 6 -> 3 -> 15 -> 9 -> 18
    */

        BinaryTree bt = new BinaryTree();
        Node n7 = bt.makeNode(null, 21, null);
        Node n6 = bt.makeNode(null, 18, null);
        Node n5 = bt.makeNode(null, 15, null);
        Node n4 = bt.makeNode(null, 12, n7);
        Node n3 = bt.makeNode(n5, 9, n6);
        Node n2 = bt.makeNode(n4, 6, null);
        Node n1 = bt.makeNode(n2, 3, n3);

        //Elapsed time
        long a = System.nanoTime();
        bt.dfsRecursive(n1);
        System.out.println(System.nanoTime() - a);

        long b = System.nanoTime();
        bt.dfsIterative1(n1);
        System.out.println(System.nanoTime() - b);

        long c = System.nanoTime();
        bt.dfsIterative2(n1);
        System.out.println(System.nanoTime() - c);

        long d =  System.nanoTime();
        bt.bfs(n1);
        System.out.println(System.nanoTime() - d);

    }
```

```java
// dfs 재귀
12 21 6 3 15 9 18 
548800 

// dfs 반복1
12 21 6 3 15 9 18 
376100

// dfs 반복2
12 21 6 3 15 9 18 
200600 

// bfs 
3 6 9 12 15 18 21 
511100 
```

<br><br>


:orange_book: References<br>

- [Implementing a binary tree in Java](https://www.baeldung.com/java-binary-tree)
- [Depth First Search in Java](https://www.baeldung.com/java-depth-first-search)
- [Binary Search Tree - Java](https://yaboong.github.io/data-structures/2018/02/12/binary-search-tree/)
- [위키 : 트리 순회](https://ko.wikipedia.org/wiki/%ED%8A%B8%EB%A6%AC_%EC%88%9C%ED%9A%8C)
- [Inorder Tree Traversal without Recursion](https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion/)

<br><br>