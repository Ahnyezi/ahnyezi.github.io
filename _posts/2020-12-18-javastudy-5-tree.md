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


# 1. 트리 자료구조
## 1.1. 트리

**트리(Tree)란 계층적인 구조를 표현하기 위해 일상적으로 사용하는 구조이다.**<br>

- 회사의 조직도

- 컴퓨터 파일 시스템에서의 디렉토리와 서브디렉토리 구조

- 내 컴퓨터\C:\Program Files

<br>

### 트리의 용어

<img src="https://user-images.githubusercontent.com/62331803/102054701-1ca5a400-3e2d-11eb-9851-eba53a2cb006.png" width="80%">



#### :point_right: 노드의 명칭

- **루트(root) 노드** : 맨 위에 위치한 노드
   - `dog` 가 루트노드이다.
- **리프(leaf) 노드** : 자식이 없는 노드
   - `canine`, `fox`, `wolf`가 리프노드이다.
   - 리프노드가 아닌 노드를 **인터널(internal) 노드**라 부른다.

#### :point_right: 기타 용어

- **서브트리(sub-tree)**: 트리에서 어떤 한 노드와 그 노드의 자손들로 이루어진 트리를 부트리(sub-tree)라고 부른다.
   - `cat`과 `canine`으로 이루어진 트리가 서브트리이다.

- **간선/엣지/링크/브랜치**: 노드와 노드를 연결하는 선
- **레벨(level)**: 0이나 1부터 시작

- **높이(height)**: 트리에 존재하는 서로 다른 레벨의 총 개수
   - 레벨1,2,3이 존재하므로 높이는 3이다.

- **깊이(depth)**: 루트노트부터 현재노드까지 오는 데 거치는 간선의 개수
   - 현재 노드가 `cat`이라고 할 때 하나의 간선을 거치므로 깊이는 1이다.

#### :point_right: 노드간의 관계

- **부모-자식(parent-child) 관계** : 루트노드를 제외한 모든 노드는 하나의 부모를 갖는다. 링크로 이어진 2개의 노드 중, 위의 노드가 부모이고 아래 노드가 자식이 된다.
   - `cat`, `fox`, `wolf`는 `dog`의 자식 노드이며, `dog`가 부모 노드가 된다.
   - 루트 노드인 `dog`는 부모 노드를 가지지 않는다.

- **형제(sibling) 관계**: 부모 노드가 같은 노드들은 형제 관계를 가진다.
   -  `cat`, `fox`, `wolf`는 `dog`를 부모노드로 가지고 있으므로 서로 형제 관계이다.

- **조상-자손(ancestor-descendant) 관계**: 부모-자식 관계를 확장한 관계이다.
   - `canine`은 `cat`의 자손이자 `dog`의 자손 노드가 된다.

<br>
<br>

## 1.2. 이진트리(Binary Tree)

### 1.2.1. 이진트리의 개념

**각 노드가 최대 2명의 자식을 가지는 트리이다.** <br>

<img src="https://user-images.githubusercontent.com/62331803/102056890-6c399f00-3e30-11eb-90e7-6e7f2c9d3aac.png" width="70%"><br>
<br>

**자식노드는 항상 자신이 부모의 왼쪽자식인지 오른쪽자식인지를 부여받는다**. 

<img src="https://user-images.githubusercontent.com/62331803/102056897-6f348f80-3e30-11eb-9dfd-15a365427fc4.png" width="60%"><br>

- 자식이 한 명일 때도 동일하게 왼쪽/오른쪽에 대한 정보를 부여받는다. 
- 따라서 **두 개의 트리가 같은 key를 가지는 노드를 자식으로 가지고 있다고 해도, 왼쪽/오른쪽 위치가 다를 경우 다른 트리로 취급한다**.<br>
<br>
<br>


### 1.2.2.이진트리 구현

**이진트리는 연결리스트로 구현할 수 있다.**<br>

<img src="https://user-images.githubusercontent.com/62331803/102066200-e2440300-3e3c-11eb-985a-7a4309f34a27.png" width="80%"><br>


#### 특징
- **Linked Structure**를 사용
   - `full binary tree`나  `complete binary tree`처럼 특별한 경우가 아니라면, data를 저장하는 **배열방의 인덱스에 규칙성이 없기 때문에 일반 배열을 사용할 수 없다.**

- 각 노드는 `왼쪽자식 노드주소`, `오른쪽자식 노드주소`, `데이터`, `부모노드주소`를 담는 필드를 가진다.
   - 부모노드의 주소는 반드시 필요한 경우가 아니면 보통 생략한다.
<br>
<br>

### 1.2.3.이진트리 순회(traversal)

연결리스트는 선형구조(데이터를 순차적으로 나열시킨 형태)이기 때문에, 순회하는 방법이 오직 한개 였다. **트리는 비선형구조이기 때문에 다양한 방법으로 노드를 방문할 수 있다.**<br>

#### 순회 방법

<img src="https://user-images.githubusercontent.com/62331803/102670518-ac06da80-41d2-11eb-8e64-500d9f4d0a8a.png" width="80%"><br>


- **중순위(inorder) ,선순위(preorder) , 후순위(postorder) 순회 (==dfs)**
- 이진트리를 루트노드 r, 왼쪽 부트리 Tl, 오른쪽 부트리 Tr로 나눠 차례로 순회한다.
    - inorder : 왼쪽 서브트리 - > 루트 -> 오른쪽 서브트리
    - preorder : 루트 -> 왼쪽 서브트리 -> 오른쪽 서브트리
    - postorder: 왼쪽 서브트리 -> 오른쪽 서브트리 -> 루트
- **레벨오더(level order) 순회 ( == bfs)**
   - 레벨 0부터 끝까지 차례대로 순회
<br>

#### 순회 예시

<img src="https://user-images.githubusercontent.com/62331803/102670552-add09e00-41d2-11eb-8650-1628a53299e6.png" width="100%"><br>
<br>
<br>

## 1.2. 검색트리(Search Tree)

일반적인 트리 자료구조는 사용하려는 데이터 자체가 계층적인 구조를 가지고 있다. 하지만 **검색트리**는 다르다.<br>

원래의 데이터가 트리 형태를 가지고 있는 것이 아니라,  `insert`, `search`, `delete` **등의 연산을 효율적으로 수행하기 위하여, 해당 데이터에 대해 tree 형태의 search structure를 구현한 형태이다.**<br>

즉, 여러 개의 데이터를 저장하고 있는 일종의 컨테이너로 insert, search, delete 연산을 지원하기 위한 목적의 트리형 자료구조를 **검색트리(Dictionary , Dynamic set, Search structure)라 부른다**. <br>

#### 검색트리 종류
- 이진검색트리(Binary Search Tree)
- 레드블랙트리(Red Black Tree)
- B트리(B Tree)
<br>

## 1.2.1. Binary Search Tree (이진검색트리)

<img src="https://user-images.githubusercontent.com/62331803/102674776-7b2aa380-41da-11eb-85cf-d1377620630d.png" width="50%"><br>

**검색트리의 가장 기본적인 형태이다.**<br>
- 특정한 조건을 가지는 이진트리이다.
- 임의의 노드 v에 대해, 해당 노드의  왼쪽 부트리에 있는 key들은 key[v]보다 작거나 같고, 오른쪽 부트리에 있는 key들은 key[v]와 같거나 크다.<br>
   - **즉, 현재 노드에서 내 왼쪽에 있는 노드의 값은 모두 나보다 작고, 내 오른쪽에 있는 노드의 값은 모두 나보다 크게 정렬된 형태이다**.
<br>
<br>

### 1.2.1.2. insert 연산

<img src="https://user-images.githubusercontent.com/62331803/102678650-280e1c00-41ed-11eb-850a-d3be9d4d00b2.png" width="60%"><br>

**트리에 노드를 추가하는 연산이다.**<br>
- 루트에서부터 key값을 비교하며 내려온다.
- 비어있는 자리를 확인하면, 해당 자리에 자기자신을 연결시킨다.


### 1.2.1.3. search 연산
<img src="https://user-images.githubusercontent.com/62331803/102674781-7cf46700-41da-11eb-8884-d951585ef73a.png" width="60%"><br>

**특정 값의 위치를 찾는 연산이다.**<br> 
- **13**을 찾는다고 가정해보자. 
   - 루트노드 15보다 작다(왼쪽에 있을 것) => 왼쪽 부트리로 이동
   - 그 다음 노드인 6보다 크다(오른쪽에 있을 것) => 오른쪽 부트리로 이동
   - 그 다음 노드인 7보다 크다(오른쪽에 있을 것) => 오른쪽 부트리로 이동
   - 발견
- **시간 복잡도는 트리의 높이인 O(h)이다.**
   - 어떤 경우에도 트리의 높이보다 더 아래로 내려갈 수는 없다.
   - 하지만 Skewed Tree의 형태처럼 최악의 경우 O(N)이 될 수 있다. (높이자체가 N이 되기 때문)
   - 평균적으로는 O(log N)
<br>
<br>

### 1.2.1.4. delete 연산

**자식노드의 개수에 따라 3가지 케이스로 나뉜다.**<br>
<br>

#### 1.2.1.4.1. 자식노드가 없는 경우

<img src="https://user-images.githubusercontent.com/62331803/102678073-50941700-41e9-11eb-9444-0d07d4ef0347.png" width="60%"><br>

**4를 가진 노드를 delete해보자.**<br>

- 그냥 삭제
<br>
<br>

#### 1.2.1.4.2. 자식노드가 1개인 경우

<img src="https://user-images.githubusercontent.com/62331803/102678071-4e31bd00-41e9-11eb-8bb4-d8119ad276a8.png" width="60%"><br>

**7을 가진 노드를 delete해보자.**<br>

- 자신의 원래 위치에 자식노드를 연결
<br>
<br>

#### 1.2.1.4.3. 자식노드가 2개인 경우

<img src="https://user-images.githubusercontent.com/62331803/102678222-7837af00-41ea-11eb-8835-60721b6ada2a.png" width="60%"><br>

**루트노드를 delete해보자.**<br>
- 앞의 경우들과 달리 자식 노드가 2개인 노드를 삭제할 경우에는 변화가 크다. 
- **BST의 정렬 순서를 유지하면서 delete 하기 위해서는, 해당 노드의** `predecessor(왼쪽 부트리 중 가장 큰 노드)` **혹은** `successor(오른쪽 부트리의 가장 작은 노드)`**가 삭제할 노드위치에 와야 한다.**
- `successor`**를 사용하여 DELETE하는 방식**을 살펴보자.
   - **삭제할 노드의 data를 successor의 data로 초기화한다.**
   - **successor의 기존 위치에 오른쪽 자식이 존재했다면, successor의 기존 부모 노드에 해당 자식노드를 연결한다.**
      - successor는 오른쪽 부트리의 최소값을 가지므로, 왼쪽 자식이 있을 수 없다.<br>
<br>
<br>

# 2. 이진트리 탐색코드
## 2.1. 깊이우선탐색(DFS)

**재귀와 반복문을 사용해서 각각 구현해보았다.** <br>
<br>

### 2.1.1. Recursive한 DFS

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

### 2.1.2. Iterative한 DFS

**방문여부를 표시하는 방법과 표시하지 않는 방법으로 각각 구현해보았다.**<br> 

#### 2.1.2.1. 방문여부 표시한 코드

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

#### 2.1.2.1. 방문여부 표시 없이 구현한 코드

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


## 2.2. 너비우선탐색(BFS)

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

## 2.3. 테스트

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
<br>

#### 순회 순서와 실행시간

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


# 3. BST(이진검색트리) 코드





:orange_book: References<br>

- [Implementing a binary tree in Java](https://www.baeldung.com/java-binary-tree)
- [Depth First Search in Java](https://www.baeldung.com/java-depth-first-search)
- [Binary Search Tree - Java](https://yaboong.github.io/data-structures/2018/02/12/binary-search-tree/)
- [위키 : 트리 순회](https://ko.wikipedia.org/wiki/%ED%8A%B8%EB%A6%AC_%EC%88%9C%ED%9A%8C)
- [Inorder Tree Traversal without Recursion](https://www.geeksforgeeks.org/inorder-tree-traversal-without-recursion/)
- https://shoark7.github.io/insight/rationality/relationship-between-graph-and-tree
- https://namu.wiki/w/%ED%8A%B8%EB%A6%AC(%EA%B7%B8%EB%9E%98%ED%94%84)
- https://ko.gadget-info.com/difference-between-tree

<br><br>