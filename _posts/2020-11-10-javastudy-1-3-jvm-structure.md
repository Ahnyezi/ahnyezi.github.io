---
title:  "[Java] JVM의 구조"
date: 2020-11-10
categories: ['Java']
tags: ['Java']
---


**JVM의 구조를 살펴보자.** :raising_hand:<br><br>


JVM은 크게 `Class Loader SubSystem(클래스로더 서브시스템)`, `Runtime Data Areas`, `Execution Engine(실행엔진)`으로 구성되어 있다. 

> JVM 아키텍처

source : [dzone.com](https://dzone.com/articles/jvm-architecture-explained)

<img src="https://user-images.githubusercontent.com/62331803/103474419-07e27d80-4de7-11eb-823b-2296465812cb.png" width="80%"><br>
<br>

## 1. 클래스로더 서브 시스템 

**프로그램을 실행하는 데 필요한 클래스를 읽어오는 역할을 한다.**<br> 

<img src="https://user-images.githubusercontent.com/62331803/102473149-78289980-409a-11eb-9493-691391ef4a1c.png" width="70%"><br>

:point_right: **클래스 로딩 절차는 다음과 같이 요약할 수 있다**
1. **메서드 호출 문장을 만난다.**
2. **해당 메서드를 가진 클래스 바이트 코드가 로딩 전이다.**
3. `클래스 로더`가 `jre 라이브러리 디렉토리`**에서 해당 클래스의 유무를 조사한다.**
4. **발견하지 못했다면,** `jdk 확장 디렉토리`**에서 해당 클래스의 유무를 조사한다.**
5. **발견하지 못했다면,** `classpath 환경변수에 지정된 디렉토리`(직접 선언한 클래스)**에서 해당 클래스의 유무를 조사한다.**
6. **클래스를 발견한다.**
7. **해당 클래스 파일이 올바른 바이트 코드인지 검증한다.** 
8. **올바른 바이트 코드라면 method 영역으로 파일을 로딩한다.**
9. **클래스 변수를 만들라는 명령어가 있으면 method영역에 해당 변수를 준비한다.**
10. **클래스 블록이 있으면 순서대로 해당 블록을 실행한다.** 
11. **프로그램이 종료될 때까지 현재 상태를 유지한다.**
<br>
<br>


**그럼 각 과정에 대해 하나씩 살펴보자.**<br>
로딩 과정은 크게 `로딩`-> `연결`-> `초기화`의 3단계로 나눠진다. <br>
<br>

## 1-1) 로딩(Loading)

<img src="https://user-images.githubusercontent.com/62331803/102475811-b1aed400-409d-11eb-9832-a7931076ffd5.png" width="30%"><br>


**로딩과정**은 `클래스 로더`에 의해 이루어진다. <br>
`클래스 로더`는 **Bootstrap, Extension, Application**으로 총 3가지가 있다. 각 클래스 로더는 **상속관계**를 가지고 있으며, 부모가 수행하지 않은 로딩작업을 자식이 수행하는 방식으로 **위임(delegation)하며 작업을 진행**한다.

- **Bootstrap Class Loader : 기본 자바 API 라이브러리 로드 담당**
   - jre의 lib폴더에 있는 `rt.jar 파일`을 뒤져 라이브러리를 로드
   - 최우선적으로 로드됨

- **Extension Class Loader : 확장 코어 클래스 파일 담당**
   - `jdk 확장 디렉토리(JAVA_HOME\lib\ext 디렉토리 혹은 java.ext.dirs)`에서 로드됨


- **Application Class Loader : 시스템 클래스 로드 담당**
   - 사용자가 직접 정의한 클래스 파일을 로드
   - Classpath 환경변수에 있는 클래스 파일이나 `-cp` 명령어 옵션이 있는 파일들을 로드
<br>
 <br>

## 1-2) 연결(Linking)

<img src="https://user-images.githubusercontent.com/62331803/102475828-b7a4b500-409d-11eb-97e2-804c313fdb93.png" width="30%"><br>

**클래스 파일의 바이트 코드를 검증하고, 정적변수의 메모리를 할당하는 과정이다.**<br>

- **Verify : 바이트 코드 검증**
   - 생성된 자바 바이트코드가 적절한지를 검증
   - 검증 실패할 경우 오류를 내보냄
- **Prepare : 준비** 
   - 모든 정적변수의 메모리를 할당
   - default 값으로 할당하며 아직 명시된 값으로 초기화되어있진 않음
<br>
<br>

## 1-3) 초기화(Initializing)

<img src="https://user-images.githubusercontent.com/62331803/102475852-be332c80-409d-11eb-9363-f2b11bf1f157.png" width="30%"><br>

**클래스 로딩의 마지막 단계이다.** <br>

- **Initialization : 초기화**
   - 모든 정적 변수가 자바 코드에 명시된 값으로 초기화
   - 정적블록 실행  
<br><br><br>


## 2. 실행 데이터 영역(Runtime Data Area) 

 클래스 로드 과정에서 읽힌 클래스 수준의 데이터뿐만 아니라 **프로그램에서 사용되는 모든 데이터들이 저장되는 영역**이다.  **크게 5가지**로 나뉘며, **스레드간 공유하는 영역**과 **스레드별로 개별 할당되는 영역**이 존재한다. <br>

> Runtime Data Areas <br>

<img src="https://user-images.githubusercontent.com/62331803/102477137-4ebe3c80-409f-11eb-9223-dae51302bf28.png" width="70%"><br>


- **모든 스레드가 공유하는 영역**
   - Method area
   - Heap area
- **스레드마다 개별 할당되는 영역**
   - Stack area
   - PC Registers
   - Native Method Stack
<br>
  <br>

### 2-1) 공유영역 - 메서드 영역(method area)

**프로그램에서 사용되는 클래스 수준의 정보가 저장되는 영역이다.**<br>

- 런타임 상수풀, 클래스 메서드, 클래스 필드 등의 데이터를 저장
- JVM당 하나밖에 없으며, 모든 스레드가 공유한다. 
<br><br>

### 2-2) 공유영역 - 힙 영역(heap area)

**프로그램에서 사용되는 모든 인스턴스 변수가 저장되는 영역이다.** <br>
- `new` 키워드를 사용하여 인스턴스가 생성되면, 해당 인스턴스의 정보를 힙 영역에 저장한다.
- 저장한 인스턴스에 대한 참조값을 stack에 반환한다.
   - 만약 stack 값이 null이라면, 참조할 수 있는 값이 없으므로 `nullPointerException`을 발생시킨다. 
- JVM당 하나밖에 없는 공유자원이기 때문에, 멀티 스레드환경에서 안전하지 않다. 
- 힙 메모리 회수 권한은 가비지 컬렉터(Garbage Collector)만 있다.
<br><br>


### 2-3) 개별영역 - 스택 영역(stack area/JVM Stack)

source : [jaxenter.com](https://jaxenter.com/stackoverflowerror-causes-152027.html)<br>

<img src="https://user-images.githubusercontent.com/62331803/102499977-2a725800-40bf-11eb-93f8-5e56c5f5e201.png" width="80%"><br>


**프로그램에서 메서드가 호출될 때, 메서드의 스택 프레임이 저장되는 영역이다.**<BR>

- 스레드마다 JVM 스택이 생성된다.
- 메서드가 호출되면 메서드 호출과 관계되는 지역변수와 매개변수 정보(스택 프레임)를 스택영역에 쌓는다.
- 메서드 호출이 정상 종료되거나 예외가 던져지면 프레임은 스택에서 빠지면서 소멸된다. 
   - 스택 프레임의 개수가 스레드가 허락된 스택 사이즈를 넘기게 되면,  `java.lang.StackOverFlowError`를 발생시킨다.

- **스택 프레임**은 3부분으로 나누어 진다.
   - **Local Variable Array(LVA)**
      - 말 그대로 변수를 담을 수 있는 배열이다. 
      - 메서드의 모든 파라미터와 지역변수를 담고 있다.
   - **Operand Stack(OS)**
      - 중간 연산의 결과가 저장되는 공간이다.
      - push와 pop과정을 거쳐서 필요한 작업을 수행한다.
   - **Frame Data(FD)**
      - 특정 메서드와 관련된 모든 참조와 리턴 값 등이 저장되는 공간이다.

<br>


### 2-4) 개별영역 - PC 레지스터(Program Counter registers)

- 각 스레드마다 존재한다.
- 스레드가 시작할 때 생성되고, 종료되면 해제된다.
- 스레드가 실행중인 명령어 주소를 담고있다.
<br><br>

### 2-5) 개별영역 - 네이티브 메서드 스택(native method stack)

- bytecode가 아닌 binarycode를 실행하는 영역이다.
- 자바 이외의 언어(C, C++, 어셈블리 등)로 작성된 코드를 실행할 때 할당되는 영역이다.
- I/O 작업을 위한 C 라이브러리 함수 등..
<br><br><br>


## 3. 실행 엔진(Execution Engine) 

**실행 엔진**은 **(1)클래스 로딩 과정**을 통해 **(2)런타임 데이터 영역**에 배치된 바이트 코드를 명령어 단위로 읽어서 실행하는 역할을 한다.<br>

바이트 코드를 실행하기 위해서는 기계어로 번역해주는 과정을 거쳐야 하는데, 이 때 **인터프리터**와 **JIT 컴파일러**를 사용한다.<br>

### 3-1)  자바의 단점 : 느린 실행속도?

**자바는 컴파일 방식과 인터프리터 방식을 혼합시킨 하이브리드 방식의 언어다.**<br>

JVM이 이해할 수 있는 바이트코드로 컴파일한 뒤에 인터프리터에 의해 실시간으로 번역되어 실행된다. <br>

> 컴파일 방식과 인터프리팅 방식의 차이<br>

||컴파일 방식|인터프리팅 방식|
|:---:|:---:|:---:|
|특징|컴파일시 소스코드 전체를 기계어로 변환|런타임시 소스코드를 한 줄씩 기계어로 변환|
|장점|실행속도가 빠르다|런타임에 실시간 디버깅 및 코드 수정이 가능하다|
|단점|OS 및 빌드 환경에 종속적이다|실행속도가 느리다|

<br><br>

:porint_right: **하이브리드 방식을 채택함으로서 WORA가 가능해졌지만, 소스코드를 바이트코드로 변환한 후 또 다시 Interpreting하는 과정을 거치는 만큼 프로그램 성능(시간)에 영향을 미친다.**<br>

**java 실행단계**<br>
- Source code ---(Compiler)---> Byte code ---(Interpreter) ---> Binary code

**Python 실행단계**<br>
- Source code --- (Interpreter) ---> Binary code
<br><br><br>


**자바는** `JIT Compiler`**를 통해 이러한 단점을 극복한다.** <br>

source : [aboullaite.me](https://aboullaite.me/understanding-jit-compiler-just-in-time-compiler/)<br>

<img src="https://user-images.githubusercontent.com/62331803/103475541-42511800-4df1-11eb-950c-3498b37545a4.png" width="60%"><br>

**JIT(Just-In-Time) 컴파일러**는 반복되어 사용되는 코드나 기계어로 변환 시 많은 리소스가 필요한 부분을 코드가 실행되는 과정(Just-In-Time)에 실시간으로 변환하여(기계어로) 캐싱한다. <br>
<br>

> 처음 바이트 코드를 읽을 때 <br>

source : [catch-me-java.tistory.com](https://catch-me-java.tistory.com/11)<br>

<img src="https://user-images.githubusercontent.com/62331803/103476370-4d5b7680-4df8-11eb-8ff7-93ff7f908896.png" width="70%"><br>

- 특정 method가 호출되면 JIT 컴파일러가 해당 코드를 `native machine code(기계어)`로 컴파일한다.
- 한 번 읽어서 기계어로 번역한 코드는 캐싱해둔다. 
<br>

> 추후 동일한 바이트코드를 읽을 때<br>

source : [catch-me-java.tistory.com](https://catch-me-java.tistory.com/11)<br>

<img src="https://user-images.githubusercontent.com/62331803/103476391-6fed8f80-4df8-11eb-9746-bb3fdcd1638a.png" width="70%"><br>

- 추후에 동일한 method(이미 캐싱 해놓은)가 호출될 경우, 인터프릿하지 않고 저장소로 부터 해당 정보를 가져온다.

<br><br>



:orange_book: **References**<br>
- https://catch-me-java.tistory.com/12
- https://hongsii.github.io/2018/12/20/jvm-memory-structure/
- https://sowhat4.tistory.com/61

