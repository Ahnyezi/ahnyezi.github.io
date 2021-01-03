---
title:  "[Java] 기계어(바이너리코드)와 바이트코드"
date: 2020-11-10
categories: ['Java']
tags: ['Java']
---

**기계어(바이너리코드)와 바이트코드를 알아보자** :raising_hand:<br>
<br>


**컴퓨터(CPU)는 일종의 계산기이다.**
**0과 1로 이루어진 바이너리 코드(binary code)를 해석하여 프로그램을 실행한다.**<br>


> 사람이 이해할 수 있는 high-level language

```c
x = 10 + 2
y = x + 4
```

> 기계가 이해할 수 있는 low-level language (binary/machine code)

```c
001001 11101 11101 1111111111111000  
001000 00001 00000 0000000000001010  
001000 00001 00001 0000000000000010  
101011 11101 00001 0000000000000000  
001000 00010 00001 0000000000000100  
101011 11101 00010 0000000000000100  
001001 11101 11101 0000000000001000
```
<br>

컴퓨터에게 일을 시키기 위해서는, 기계가 이해할 수 있는 기계어로 명령을 내려야 한다.<br>
 하지만 위에서 본 것 처럼, 기계어는 실제 인간이 사용하는 언어와 거리가 멀기 때문에 기계어(저급언어)로 프로그래밍을 하는 것은 매우 비효율적이다.<br>

이러한 문제를 해결하기 위해 등장한 것이 **고급언어**이다. 컴퓨터가 이해하기 쉽도록 쓰여진 저급언어와 달리 고급언어는 사용자가 이해하기 쉽도록 쓰여져있다. 따라서 인간의 입장에서 볼 때 보다 가독성 높고 다루기 편하다는 장점이 있다. 

> 저급언어와 고급언어의 종류<br>

source : [www.tutorialandexample.com](https://www.tutorialandexample.com/low-level-language/)<br>

<img src="https://user-images.githubusercontent.com/62331803/103471903-ad86f400-4dc9-11eb-8a7c-9bd5ac9052c5.png" width="70%"><br>
<br>

고급언어로 작성된 프로그램은 기계어가 아니기 때문에 컴퓨터가 이해할 수 없다. 따라서 기계어로 번역하는 과정을 거쳐야하는데 이 과정을 **Compile**이라 한다. 

> 자바파일 컴파일 과정<br>

[source](https://images.app.goo.gl/yzmWUhHasy6CqHcD7)<br>

<img src="https://user-images.githubusercontent.com/62331803/103471940-503f7280-4dca-11eb-91e9-0d711e728903.png" width="80%"> <br>


**즉, 사용자가 작성한 고급언어가 컴파일러를 통해서 기계어로 번역되고, 번역된 코드를 컴퓨터가 이해하여 프로그램을 실행시키는 것이다.**<br>
<br><br>


:orange_book: **References**<br>

- https://www.geeksforgeeks.org/difference-between-byte-code-and-machine-code/
- https://namu.wiki/w/%EA%B8%B0%EA%B3%84%EC%96%B4
- https://m.blog.naver.com/PostView.nhn?blogId=ygszzang11&logNo=50106750281&proxyReferer=https:%2F%2Fwww.google.com%2F