---
title:  "[CS] 프레임워크 VS 라이브러리 개념 및 종류"
date: 2020-9-13
categories: ['CS']
tags: ['framework','library']
---

프레임워크 VS 라이브러리 개념 및 종류

## 프레임워크

### 1. 개념
**FrameWork == Frame(틀, 규칙) + Work(일, 소프트웨어의 목적)**
-  정해진 규칙 안에서 프로그램을 짜는 것.
-  소프트웨어의 목적(Work)마다 사용해야 할 규칙(Frame)이 달라질 수 있다.
-  프레임 워크(주도권O) 사용자(주도권X) 
   -  반드시 해당 Framework 내에서 정해진 규칙을 지켜야 함
- 보다 효율적인 Frame을 찾아서 SW개발에 적용
-  작업 속도를 높이고 단순화 가능

### 2. 종류
1. 스트럿츠 프레임워크(STRUTS Framework)
  - JAVA 기반의 JSP만을 위한 프레임워크
  - MVC 모델을 기반으로 웹 애플리케이션을 구축
  - 웹 앱 초기에 많이 사용됨
2. 스프링 프레임워크(Spring Framework)
 - 엔터프라이즈급(기업을 대상으로 하는) 웹 앱 개발에 사용되는 경량형 프레임워크
- 프로젝트 규모가 클 수록 스트럿츠보다 스프링을 활용하는 추세
- J2EE(Java 2 Enterprise Edition)에서 제공하는 대부분의 기능을 지원 
   - J2EE: 분산 객체, 효율적 자원관리, 컴포넌트 기반 개발 등을 자바 환경에서 사용할 수 있게 하는 표준 규약
- 다양한 DB처리 라이브러리와 연동 
  - JDBC, iBatis, 하이버네이트, JPA
-  DI(Dependency Injection, 의존성 주입)를 지원
    - 스프링 container가 대신 객체를 생성해주고, 알아서 객체를 주입 
-  Ioc(Inversion Of Control, 제어 역전의 원칙)를 지원
   - container에 의해 생성된 객체는 자신이 어디에 쓰이는지 알지 못함


3. 앵귤러 JS(AngularJS)
- 자바스크립트 기반의 프레임워크
- 자바스크립트나 제이쿼리로 만든 코드 길이 단순화, 직관성 증대

4. 장고 프레임워크(Django Framework)
- 파이썬 기반의 오픈 소스 웹앱 프레임워크
- 파이썬 기반의 강력한 라이브러리 그대로 사용 가능
- MVC 패턴 기반의 MTV
- ORM(Object-Relational-Mapping)기능 지원

<br>


## 라이브러리

### 1. 개념
- 프로그래밍 할 때 활용가능한 도구의 집합
-  반복적인 코드 작성을 없애기 위해,  class나 function으로 정의하여 필요할 때 호출해서 사용
- 라이브러리(주도권 X) 사용자(주도권 O)
    - 라이브러리는 코드의 전체적 흐름에 영향을 주지 못함

### 2. 종류
1. 자바 클래스 라이브러리
-  java.lang 패키지
   -  Object 클래스
   -  String 클래스 ... 
-  java.util 패키지
   -  Date, Calendar 클래스 
2. 파이썬
-  Datetime
-  Pandas 

<br>

<details>
<summary>출처</summary>

- https://www.castingn.com/sourcing/kkultip_detail/110<br>
- https://webclub.tistory.com/458<br>
- https://engkimbs.tistory.com/673<br>
- https://www.linux.co.kr/home2/board/subbs/board.php?bo_table=lecture&wr_id=600<br>
- https://sehun-kim.github.io/sehun/springbean-lifecycle/<br>

</details>