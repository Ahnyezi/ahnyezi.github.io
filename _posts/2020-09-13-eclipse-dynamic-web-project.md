---
title:  " [Web] Dynamic Web Project 구조 파헤치기"
date: 2020-9-13
categories: ['Web']
tags: ['Web','Eclipse','Dynamic Web Project']
---

 [Eclipse] Dynamic Web Project 구조 파헤치기


#### 1. 새 프로젝트 생성
<img src="https://user-images.githubusercontent.com/62331803/93004233-1e3aa400-f580-11ea-8639-8d233302f1ab.png" width="60%"> <br>

- `Dynamic Web Project`: JSP와 같이 동적인 웹페이지를 가지는 웹 애플리케이션 개발 시에 사용하는 프로젝트
- `Static Web Project`: JSP와 같은 동적인 페이지가 없는 순수하게 웹 컨텐츠로만 구성되어 있는 웹 컨텐츠를 위한 프로젝트
 - `Web fragment Project` : 다른 웹 프로젝트에 하나의 라이브러리와 같은 형태로 재사용될 때 유용. 해당 프로젝트의 output은 jar파일로 생성되어 다르웹 프로젝트에 추가될 수 있음.
<br>

#### 2. 다이나믹 웹 프로젝트 
<img src="https://user-images.githubusercontent.com/62331803/93004257-58a44100-f580-11ea-8238-12c5e0eff233.png" width="60%"><br>

- `Target runtime`: 웹 어플리케이션 실행 서버. tomcat 관련 라이브러리 프로젝트에 자동 추가
- `Dynamic web model version`: 서블릿 버전. 이클립스가 해당 버전으로 코드문법을 검사.
<br>

#### 3. 웹 어플리케이션 배치 정보 설정
<img src="https://user-images.githubusercontent.com/62331803/93004407-a2d9f200-f581-11ea-9b14-381312b75657.png" width="60%"><br>
- Context root: 웹 애플리케이션 이름. 웹 브라우저 실행 요청할 때 여기 지정된 이름을 URL에서 사용
- Content directory: 웹 콘텐츠 파일을 저장할 작업 폴더 이름.
- Generate web.xml deployment descriptor: 웹 어플리케이션 배치 설명서 파일 자동 생성 옵션. 프로젝트의 WEB-INF 폴더에 web.xml파일 자동 생성

<br>

#### 4. 프로젝트 구조
<img src="https://user-images.githubusercontent.com/62331803/93004483-5f33b800-f582-11ea-987a-0064dc2e9fe9.png" width="50%"><br>
1. **Java Resources | 자바기반의 자원들**
 - `src`: Java 소스파일, 프로퍼티(.properties)파일 위치
- `libraries`: 실제 WebContent/WEB-INF/lib 폴더를 가상으로 보여줌
2. **build | 소스파일 컴파일 이후의 결과물들**
- `build`: 자바 클래스 파일(.class) 위치
3. **WebContent | HTML(.html), CSS(.css), JavaScript(.js), JSP, 이미지 파일 등의 웹 콘텐츠**
-  웹 어플리케이션을 서버에 배치할 때 이 폴더의 내용물이 그대로 복사
   - `WebContent/WEB-INF`: 웹 어플리케이션 설정 관련 파일들이 위치하는 디렉토리  
      - 이 폴더에 있는 파일은 클라이언트에서 요청할 수 없음
   - `WebContent/WEB-INF/web.xml`: 웹 어플리케이션 Deployment Descriptor(배치 설명서, DD파일)  
     - 서블릿, 필터, 리스너, 매개변수, Welcome Pages 등의 웹 어플리케이션 컴포넌트 배치 정보를 작성  
     - 서블릿 컨테이너는 클라이언트의 요청을 처리할 때 이 파일의 정보를 참고하여 서블릿 클래스를 찾거나 필터를 실행하는 등의 작업을 수행
   - `WebContent/WEB-INF/lib`: 자바 아카이브 파일(.jar)이 위치하는 디렉토리
      - jar(Java Archive)?
          - zip 파일 형태의 압축파일
          - 여러개의 자바 클래스 파일과 클래스들이 이용하는 관련 리소스(텍스트, 그림 등) 및 메타 데이터를 하나의 파일로 모아서 압축
         - 하나의 파일로서 자바 플랫폼에 응용 SW나 라이브러리를 배포하기 위한 파일 포맷
<br>

<details>
<summary>출처</summary>

- https://deeds-not-words.tistory.com/entry/%EC%9D%B4%ED%81%B4%EB%A6%BD%EC%8A%A4-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EC%A2%85%EB%A5%98%EC%99%80-%EC%B0%A8%EC%9D%B4<br>

- https://atoz-develop.tistory.com/entry/Eclipse-Dynamic-Web-Project-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EA%B5%AC%EC%A1%B0<br>

- http://blog.naver.com/PostView.nhn?blogId=da91love&logNo=221193103318<br>

- https://ko.wikipedia.org/wiki/JAR_(%ED%8C%8C%EC%9D%BC_%ED%8F%AC%EB%A7%B7)<br>


</details>