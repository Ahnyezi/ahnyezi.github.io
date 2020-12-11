---
title:  "[Java] 선택문과 제어문"
date: 2020-12-03
categories: ['Java']
tags: ['Java']
---

***Java언어에서 flow control을 담당하는 선택문과 제어문을 공부해보자!*** :raising_hand:<br>
<br>

:point_right: **과제 회고**<br>
자바의 기본적인 문법은 알고 있었지만, 속성식으로 빠르게 배웠기 때문에 깊이가 없다고 생각했다.<br>
그래서 **이번 과제의 목표**를 다음과 같이 잡았다.<br>

- **oracle documentation**과 예제보고 키워드 사용법 제대로 이해하기 
- 새롭게 **업데이트 된 부분 공부**하기

문서와 예제를 통해 공부를 하니까, 이미 알고있던 키워드도 더 잘 활용하는 방법을 배울 수 있었다. <br>
또 새롭게 알게 된 내용도 많았다.<br>
- `forEach` 메서드
- 새로운 `Switch`문 사용법
   - `->`와 `yield`문 사용

배운 내용을 잘 활용하면, 좀 더 가독성 있는 코드를 짤 수 있을 거 같다. <br>

<br>
<br>


### :pencil2: **목차**
 
1. [if](1-if--표현식이-참인지-판별)
2. [else](2-else)
3. [else if](3-else-if)
4. [switch](4-switch)
   - 4.1. Java SE 12 switch expressions (yield, ->)
5. [while](5-while)
6. [do while](6-do-while)
7. [for](7-for)
   - forEach(->)
8. [break](8-break)
9. [continue](9-continue) 

<br>
<br>


## :pushpin: 프로그램의 흐름을 제어하는 선택문과 제어문

**Java**언어에서 프로그램의 흐름을 제어하기 위해서, 다음과 같은 키워드를 사용할 수 있다.<br>

1. `조건 확인` : **if, else, switch**
2. `반복 사이클 생성` : **while, for**
3. `사이클 제어` : **break, continue**

각 **keyword**의 쓰임을 예제를 통해 살펴보자. <br>
<br>

### 1. if : 표현식이 참인지 판별

기본적인 형식은 다음과 같다.<br>

```java
if (표현식 expression){
	명령문 statement;
}
```

- `if`는 **표현식**이 참인지를 판별하는 키워드이다. 
- 만약 참이라면, **명령문**이 실행된다.
-  **명령문**이 여러줄 인 경우,  `block`을 사용하여 모든 **명령문**를 묶어준다.
<br>


:point_right: **예제1 : 생성한 난수가 양수인지 판별하기**<br>

```java
package javastudy;

import java.util.Random;

public class IFStatement {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		Random r = new Random();
		int num = r.nextInt();
		
		if (num > 0) {
			//statement가 다수인 경우 블록을 사용
			System.out.println("생성한 ");
			System.out.println("난수가 ");
			System.out.println("양수입니다.");
			
		}
		
		//statement가 하나인 경우
		if (num > 0) System.out.println("양수라구요.");  
	}

}
```
<br>



### 2. else

**else 키워드를 사용하여 간단한 분기문을 만들 수 있다.** <br>

```java
if (expression){
	statement;
} else {
	statement2;
}
```

- if 키워드의 **표현식**이 **거짓**인 경우, **else 키워드 뒤의 명령문이 자동으로 실행**된다.
<br>

:point_right: **예제2 : 생성한 난수가 양수인지 아닌지 판별하기**<br>


```java
package javastudy;

import java.util.Random;

public class Branch {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		Random r = new Random();
		int num = r.nextInt();
		
		if (num > 0) {
			
			System.out.println("양수입니다.");
			
		}
		else {
			
			System.out.println("양수가 아닙니다.");
			
		}

	}

}
```
<br>

### 3. else if

`else if`를 사용하면 좀 더 복잡한 분기문을 만들 수 있다.<br>

```java
if (expression){
	statement;
} else if (expression2){
	statement2;
} else if (expression3){
	statement3;
}	.
	.
	.
else {
	statement10;
}
```

- **else if** 키워드는 앞의 조건들이 모두 거짓일 경우에 또 다른 조건을 시험할 수 있는 키워드이다. 
- `if`나 `else`와 달리, 여러번 사용하여 분기를 만들 수 있다.  
<br>

:point_right: **예제3 :  생성한 난수가 양수인지 0인지 음수인지 판별하기**<br>

```java
package javastudy;

import java.util.Scanner;

public class MultipleBranches {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		System.out.println("숫자를 입력하세요:");

		Scanner sc = new Scanner(System.in);
		int num = sc.nextInt();

		if (num > 0) {
			
			System.out.println("양수입니다.");
			
		} else if (num == 0) {
			
			System.out.println("0 입니다.");
			
		} else {
			
			System.out.println("음수입니다.");
			
		}

	}

}
```

```java
숫자를 입력하세요:
-3
음수입니다.

숫자를 입력하세요:
1
양수입니다.

숫자를 입력하세요:
0
0 입니다.
```

<br>

### 4. switch

**switch는 선택을 제어하는 키워드이다.**<br>
 여러가지 분기를 만들 수 있다는 점에서 `if-else if`와 유사하지만, **보다 간단하고 가독성이 높은 형태로 구현할 수 있다.** <br>
<br>

**기본형태는 다음과 같다.** <br>
```java
switch(domain){
	case 1:
		break;
	case 2:
		break;
		.
		.
		.
	default:
		break;
}
```

- **switch**는 대괄호 안에 오는 `변수`나 `표현식`을 기준으로, 각 분기에 해당하는 값들을 비교한다.	 
- **case** 키워드 뒤에 각 분기에 해당하는 값을 제시하고, 만약 `변수` 혹은 `표현식`이 **해당 case**의 값과 일치할 경우 case 뒤의 명령문이 실행된다.  
- 일치 하는 값이 없을 경우에는 아무 명령문도 실행되지 않거나, **default** 옵션을 사용해서 일치하는 case가 없을 경우 실행할 명령문을 지정해 줄 수 있다. 
- 각각의 분기는 `break` 키워드로 종료한다.
   - `break`문이 없다면, 해당 **case**하단에 모든 case들이 실행된 후 종료된다.
- 여러가지 case를 그룹화하여 사용할 수  있다. (예제5)

<br>

:point_right: **예제4 :  국가 판별하기**<br>

```java
package javastudy;

import java.util.Scanner;

public class SwitchStatement {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		System.out.println("도메인을 입력하세요:");
		
		Scanner sc = new Scanner(System.in);
		String domain = sc.nextLine().toLowerCase();
		
		switch (domain) {
		
			case "kr":
				System.out.println("대한민국");
				break;
				
			case "us":
				System.out.println("미국");
				break;
			
			case "cn":
				System.out.println("중국");
				break;
				
			case "jp":
				System.out.println("일본");
				break;
				
			default:
				System.out.println("알 수 없는 나라");
				break;
				
		}
	}
}

```

```java
도메인을 입력하세요:
kr
대한민국

도메인을 입력하세요:
de
알 수 없는 나라
```
<br>

:point_right: **예제5 :  어느 구인지 판별하기**<br>

```java
package javabasic;  
  
import java.util.Scanner;  
  
public class SwitchDemo2 {  
    public static void main(String[] args){  
        Scanner sc = new Scanner(System.in);  
  System.out.print("동을 입력하세요>>");  
  String district = sc.nextLine();  
  
 switch(district.trim()){  
            case "방배동": case "양재동": case "서초동":  
            case "반포동": case "잠원동":  
                System.out.println(district + " in 서초구");  
 break; case "합정동": case "망원동": case "연남동":  
            case "아현동": case "서교동":  
                System.out.println(district + " in 마포구");  
 break; default:  
                System.out.println("unknown");  
  }  
    }  
}
```

```java
동을 입력하세요>>망원동
망원동 in 마포구

동을 입력하세요>>서교동
서교동 in 마포구

동을 입력하세요>>대학동
unknown
```
<br>

### 4.1. Java SE 12 switch expressions

:orange_book: **References**<br>
- [https://docs.oracle.com/en/java/javase/13/language/switch-expressions.html](https://docs.oracle.com/en/java/javase/13/language/switch-expressions.html)
- [https://dev-kani.tistory.com/21](https://dev-kani.tistory.com/21)
- [https://stackoverflow.com/questions/58049131/what-does-the-new-keyword-yield-mean-in-java-13](https://stackoverflow.com/questions/58049131/what-does-the-new-keyword-yield-mean-in-java-13)

자바 12에 break대신 `yield문`과 `arrow`를 사용한 switch문이 소개되었다.<br>
- `yield` : case의 결과로 반환하고 싶은 값이 있을 경우. yield를 사용한다.
- `->`:  `:`대신 사용할 수 있다.


:point_right: **예제6 :  arrow 사용전후 코드 비교**<br>

```java
	public enum Day { SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY; }  
  
    // 기존의 switch문  
	  int numLetters = 0;  
	  Day day = Day.WEDNESDAY;  
	  switch (day) {  
	        case MONDAY:  
	        case FRIDAY:  
	        case SUNDAY:  
	            numLetters = 6;  
				break; 
			case TUESDAY:  
	            numLetters = 7;  
				break; 
			case THURSDAY:  
	        case SATURDAY:  
	            numLetters = 8;  
				break; 
			case WEDNESDAY:  
	            numLetters = 9;  
				break; 
			default:  
	            throw new IllegalStateException("Invalid day: " + day);  
	  }  
	   System.out.println(numLetters);  
    
  // arrow 사용한 switch문  
	  Day day = Day.WEDNESDAY;  
	  System.out.println(  
	            switch (day) {  
			        case MONDAY, FRIDAY, SUNDAY -> 6;  
					case TUESDAY                -> 7;  
					case THURSDAY, SATURDAY     -> 8;  
					case WEDNESDAY              -> 9;  
					default -> throw new IllegalStateException("Invalid day: " + day);  
			  }  
);

```
<br>

:point_right: **예제7 : yield문을 사용하여 정수값을 반환**<br>
```java
    Day day = Day.WEDNESDAY;
    // numLetters case문 실행 결과값 numLetter에 삽입
    int numLetters = switch (day) {
        case MONDAY:
        case FRIDAY:
        case SUNDAY:
            System.out.println(6);
            yield 6; // `월, 금, 일`인 경우 6을 yield하여 numLetter변수 초기화
        case TUESDAY:
            System.out.println(7);
            yield 7;// `화`인 경우 7을 yield하여 numLetter변수 초기화
        case THURSDAY:
        case SATURDAY:
            System.out.println(8);
            yield 8; // `목,토`인 경우 8을 yield하여 초기화
        case WEDNESDAY:
            System.out.println(9);
            yield 9;
        default:
            throw new IllegalStateException("Invalid day: " + day);
    };
    System.out.println(numLetters);
```
<br>




### 5. while

**while은 대괄호 안에 주어진 boolean값의 상태에 따라, 반복을 제어할 수 있는 키워드이다.**<br>
<br>

**기본형태는 다음과 같다.**<br>

```java
while (표현식 expression){
	명령문 statement;
}
```
- **표현식**이 참인 경우 명령문을 계속해서 수행한다.
<br>

:point_right: **예제8 :  더하기 연산 반복하기**<br>

```java
package javastudy.fourth;

public class WhileStatement {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		int i = 0;
		int sum = 0;

		while (i < 10) {

			i++;
			sum += i;
			System.out.println("i : " + i);
			System.out.println("sum : " + sum + "\n");

		}
	}
}

```

```java
i : 1
sum : 1

i : 2
sum : 3
...
i : 9
sum : 45

i : 10
sum : 55
```

<br>

### 6. do while

**do while**은 `while`문의 방식을 살짝 수정해서 사용할 수 있는 키워드이다.<br>
- 명령문이 최소 1번 실행된 후에, while문 뒤에 있는 표현식을 확인하여 반복여부를 결정한다.
<br>

:point_right: **예제9 :  횟수 카운팅하기**<br>  


```java
package javastudy.fourth;

public class DoWhile {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		int count = 0;
		
		do {
			
			System.out.println("횟수 : " + count);
			
		} while (count != 0);//expression이 false이므로 break

	}

}
```

```java
횟수 : 0
```

<br>

### 7. for

**for은 반복할 횟수가 주어져 있을 경우 반복을 제어할 수 있는 키워드이다.**<br>
<br>

**기본형태는 다음과 같다.**<br>
```java
for (카운팅 변수 초기화; 표현식 ; 명령문) {
    statement;
}
//예시
for (int i = 0; i < 10 ; i++) {
    System.out.println(i);
}
```

**for**는 기본적으로 4단계의 과정을 거친다. <br>

`변수 초기화` -> `표현식 참/거짓 확인` -> `statement 실행` -> `괄호 안의 statement 실행`

1.  `int i = 0` : 카운팅 변수 i를 0으로 초기화
   - 이 단계는 오직 한번만 시행된다. 
2.  `i < 10` : 해당 조건이 참인지를 판별한다. 
   - 참일 경우 다음 단계로 넘어가고, 
   - 거짓일 경우 반복문에서 나간다.
3. `System.out.println(i)` : for가 감싸고 있는 **명령문**을 실행한다.
4.   `i++` : 괄호 안의 변수 i를 1 증가시키는 명령을 시행한다.

`2`단계의 조건이 거짓이 될 때까지 시행을 반복한다. <br>
<br>

:point_right: **예제 10 :  배열 원소 출력하기**<br> 


```java
package javastudy.fourth;

public class ForStatement {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		String[] ch = { "내", " ", "이", "름", "은", " ", "이", "효", "리", "\n" };

		for (int i = 0; i < ch.length; i++) {

			System.out.print(ch[i]);

		}

		System.out.print("거꾸로 해도 ");

		for (int i = ch.length - 1; i >= 0; i--) {

			System.out.print(ch[i]);

		}

	}

}

```

```java
내 이름은 이효리
거꾸로 해도 
리효이 은름이 내
```

<br>

**괄호 안에 2개 이상의 명령문을 사용할 수도 있다**<br>

:point_right: **예제 11 :  난수의 합 구하기**<br>  

- for문의 각 시행마다 `i++`과  `sum += num` 연산을 함께한다.
- `block`안에 **sum**을 구하는 명령문을 따로 넣을 필요 없이, 괄호 안에서 연산할 수 있다.


```java
package javastudy.fourth;

import java.util.Arrays;
import java.util.Random;

public class ForStatement2 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		Random r = new Random();

		int[] values = new int[10];
		int num;
		int sum = 0;

		for (int i = 0; i < 10; i++, sum += num) {// i++, sum += num

			num = r.nextInt(10);// 10 아래의 난수를 생성
			values[i] = num;// 배열의 각 방에 초기화

		}

		System.out.println(Arrays.toString(values));
		System.out.println("합은 " + sum + "입니다");

	}

}

```
```java
[6, 8, 5, 7, 2, 8, 2, 2, 0, 5]
합은 45입니다
```

<br>

**배열의 원소를 하나씩 빼오는 형태로 사용할 수도 있다.**<br>

:point_right: **예제 12 :  배열 원소 출력하기**<br>  

```java
package javastudy.fourth;

public class ForStatement3 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		String[] str = { "내", " ", "이", "름", "은", " ", "이", "효", "리", "\n" };

		for (String s : str) {

			System.out.print(s);

		}

	}

}
```

<br>

**(추가1) 무한루프**<br>
```java
// infinite loop
for ( ; ; ) {
    
    // your code goes here
}
```
<br>

**(추가2) forEach 메서드**<br>
```java
// 기존의 forEach문
for (String name : names) {
	System.out.println(name);
}
// arrow를 사용하는 새로운 forEach 함수
names.forEach( name -> { // name 배열을 순회하며 꺼내온 값을 `name` 변수에 삽입하고, 매 시행마다 블럭을 실행
	System.out.println(name);
});
```
<br>



### 8. break

**break는 반복을 종료할 수 있는 키워드이다.**<br>

- `while`, `for`, `switch`와 함께 쓰인다.
<br>

:point_right: **예제13 :  특정숫자일 경우 반복 종료하기**<br>  


```java
package javastudy.fourth;

import java.util.Random;

public class BreakStatement {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
        Random random = new Random();

        while (true) {

            int num = random.nextInt(50);
            System.out.print(num + " ");

            if (num == 22) {

                break;//while문을 빠져나간다
            }
        }

        System.out.print('\n');

	}

}

```

```java
20 19 46 49 16 49 18 6 47 31 38 38 31 24 36 33 43 37 9 22
```

<br>

### 9. continue

**continue는 반복 중 특정 조건하에서 명령문 실행을 skip할 수 있는 키워드 이다.**<br>

- `for`, `while`과 함께 쓰인다.
<br>

:point_right: **예제14 : 홀수만 출력하기**<br>


```java
package javastudy.fourth;

public class ContinueStatement {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		int num = 0;

		while (num < 30) {
			
			num++;
			
			if ((num % 2) == 0) {//짝수일 경우 뒤의 명령은 시행하지 않는다
				
				continue;
				
			}

			System.out.println(num + " ");
		}

		System.out.println('\n');

	}

}
```  

```java
1 
3 
5 
...
25 
27 
29 
```

<br>
<br>

:orange_book: **References**<br>
- [http://zetcode.com/lang/java/flow/](http://zetcode.com/lang/java/flow/) 

<br> <br> 


