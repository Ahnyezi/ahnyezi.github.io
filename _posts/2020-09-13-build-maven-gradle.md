---
title:  "[빌드 도구] Gradle vs Maven "
date: 2020-9-13
categories: ['others']
tags: ['build','gradle','maven']
---

 [빌드 도구] Gradle vs Maven 

```
1. 컴파일: 사용자가 작성한 코드를 컴퓨터가 이해할 수 있는 언어로 번역하는 일

2. 빌드: 컴파일된 코드를 실제 실행할 수 있는 상태로 만드는 일

3. 배포: 빌드가 완성된 실행 가능한 파일을 사용자가 접근할 수 있는 환경에 배치시키는 일

4. 혹은 컴파일을 포함해 war, jar 등의 실행 가능한 파일을 뽑아내기까지의 과정을 빌드한다고도 함.
```



### 1. 빌드 
- 소스코드 파일을 컴퓨터에서 실행할 수 있게 하는 과정 및 결과물
- 우리가 작성한 소스코드(java)와 프로젝트 내의 파일과 자원(.xml, jpg, .jar, .properties)을
-  WAS(Web Application Server)가 인식할 수 있는 구조로 패키징하는 과정 및 결과물
    - WAS  예시: JVM, 톰캣..
- 예시: 자바프로젝트
   - resource(소스코드.java)
   - 소스코드.java를 compile하여 소스코드.class로 변환
   - resource를 .class에서 참조할 수 있는 위치로 이동
   - MERA-INF와 MANIFEST.MF를 하나로 압축
<br>

### 2. 빌드 도구
- 프로젝트 생성, 테스트 빌드 , 배포 등의 작업을 위한 전용 프로그램
- 프로젝트 진행 중 라이브러리 추가나 라이브러리 버전 동기화의 어려움 해소를 위해 등장
- 초기 Java 빌드도구로 Ant를 많이 사용
   - 하지만, 스크립트 작성도 많고 라이브러리 의존관리 되지 않아 불편
-  최근 많은 빌드 도구가 생김
   - Maven, Gradle(현재 가장 많이 사용되는 추세) 

<br>

### 1. Maven
- 자바용 프로젝트 관리도구
- Apache Ant의 대안으로 만들어짐
- 필요한 라이브러리를 특정 문서(pom.xml)에 정의해 놓으면,  내가 사용할 라이브러리 뿐만 아니라, 해당 라이브러리가 작동하는데 필요한 다른 라이브러리까지 관리하여, 네트워크를 통해서 자동으로 다운로드 함

> pom.xml 예시

```xml
<?xml version="1.0" encoding="UTF-8"?>  <project  xmlns="http://maven.apache.org/POM/4.0.0"  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">  <modelVersion>4.0.0</modelVersion>  <!--POM model의 버전-->  <parent>  <!--프로젝트의 계층 정보-->  <groupId>org.springframework.boot</groupId>  <artifactId>spring-boot-starter-parent</artifactId>  <version>2.2.4.RELEASE</version>  <relativePath/>  <!-- lookup parent from repository -->  </parent>  <groupId>com.god</groupId>  <!--프로젝트를 생성하는 조직의 고유 아이디를 결정한다. 일반적으로 도메인 이름을 거꾸로 적는다.-->  <artifactId>bo</artifactId>  <!--프로젝트 빌드시 파일 대표이름 이다. groupId 내에서 유일해야 한다.Maven을 이용하여 빌드시 다음과 같은 규칙으로 파일이 생성 된다. artifactid-version.packaging. 위 예의 경우 빌드할 경우 bo-0.0.1-SNAPSHOT.war 파일이 생성된다.-->  <version>0.0.1-SNAPSHOT</version>  <!--프로젝트의 현재 버전, 프로젝트 개발 중일 때는 SNAPSHOT을 접미사로 사용-->  <packaging>war</packaging>  <!--패키징 유형(jar, war, ear 등)-->  <name>bo</name>  <!--프로젝트, 프로젝트 이름-->  <description>Demo project for Spring Boot</description>  <!--프로젝트에 대한 간략한 설명-->  <url>http://goddaehee.tistory.com</url>  <!--프로젝트에 대한 참고 Reference 사이트-->  <properties>  <!-- 버전관리시 용이 하다. ex) 하당 자바 버전을 선언 하고 dependencies에서 다음과 같이 활용 가능 하다. <version>${java.version}</version> -->  <java.version>1.8</java.version>  </properties>  <dependencies>  <!--dependencies태그 안에는 프로젝트와 의존 관계에 있는 라이브러리들을 관리 한다.-->  <dependency>  <groupId>org.springframework.boot</groupId>  <artifactId>spring-boot-starter-web</artifactId>  </dependency>  <dependency>  <groupId>org.springframework.boot</groupId>  <artifactId>spring-boot-starter-tomcat</artifactId>  <scope>provided</scope>  </dependency>  <dependency>  <groupId>org.springframework.boot</groupId>  <artifactId>spring-boot-starter-test</artifactId>  <scope>test</scope>  <exclusions>  <exclusion>  <groupId>org.junit.vintage</groupId>  <artifactId>junit-vintage-engine</artifactId>  </exclusion>  </exclusions>  </dependency>  </dependencies>  <build>  <!--빌드에 사용할 플러그인 목록-->  <plugins>  <plugin>  <groupId>org.springframework.boot</groupId>  <artifactId>spring-boot-maven-plugin</artifactId>  </plugin>  </plugins>  </build>  </project>  
  
출처: [https://goddaehee.tistory.com/199](https://goddaehee.tistory.com/199) [갓대희의 작은공간]
```

<br>

### 2. Gradle
- 유연함과 성능에 초점을 둔 오픈소스 빌드도구
- Groovy를 기반으로 한 관리도구
    -  Groovy 언어 : java기반 동적 객체지향 프로그래밍 언어
- Ant 처럼 유연한 범용 빌드 도구이자
- Maven과 같은 구조화 된 build 프레임워크
- Build Script를 xml이 아닌 Groovy기반의 DSL(Domain Specific Language)을 사용
- Gradle 설치 없이 Gradle Wrapper를 이용해 빌드 지원
- Maven과 달리, 하위 프로젝트에 대한 정의를 최상위 프로젝트에서 관리 가능
   - allprojects, subprojects 속성을 통해 각 프로젝트의 공통 속성과 작동 정의 

> build.gradle.kts 예시

```gradle
import org.springframework.boot.gradle.tasks.bundling.BootJar

plugins {
    id("org.jetbrains.kotlin.jvm") version "1.3.21"
    id("org.jetbrains.kotlin.kapt") version "1.3.21"
    id("org.springframework.boot") version "2.1.4.RELEASE" apply false
    id("org.jetbrains.kotlin.plugin.spring") version "1.3.21" apply false
    id("com.gorylenko.gradle-git-properties") version "1.5.1" apply false
}

allprojects {
    repositories {
        jcenter()
    }
}


subprojects {
    apply(plugin = "kotlin")
    apply(plugin = "kotlin-kapt")
    apply(plugin = "org.springframework.boot")
    apply(plugin = "io.spring.dependency-management")
    apply(plugin = "org.jetbrains.kotlin.plugin.spring")
    apply(plugin = "com.gorylenko.gradle-git-properties")

    group = "io.honeymon.boot"
    version = "1.0.0"

    dependencies {
        compile("com.fasterxml.jackson.module:jackson-module-kotlin")
        compile("org.jetbrains.kotlin:kotlin-reflect")
        compile("org.jetbrains.kotlin:kotlin-stdlib-jdk8")
        compile("org.springframework.boot:spring-boot-starter-logging")

        /**
         * @see <a href="https://kotlinlang.org/docs/reference/kapt.html">Annotation Processing with Kotlin</a>
         */
        kapt("org.springframework.boot:spring-boot-configuration-processor")
        compileOnly("org.springframework.boot:spring-boot-configuration-processor")

        testCompile("org.springframework.boot:spring-boot-starter-test")
    }

    tasks {
        compileKotlin {
            kotlinOptions {
                freeCompilerArgs = listOf("-Xjsr305=strict")
                jvmTarget = "1.8"
            }
            dependsOn(processResources) // kotlin 에서 ConfigurationProperties
        }


        compileTestKotlin {
            kotlinOptions {
                freeCompilerArgs = listOf("-Xjsr305=strict")
                jvmTarget = "1.8"
            }
        }
    }
}

project("bootiful-core") {
    dependencies {
        compile("org.springframework.boot:spring-boot-starter-data-jpa")

        runtimeOnly("com.h2database:h2")
    }

    val jar: Jar by tasks
    val bootJar: BootJar by tasks

    bootJar.enabled = false
    jar.enabled = true
}

project(":bootiful-sbadmin") {
    dependencies {
        compile(project(":bootiful-core"))

        compile("de.codecentric:spring-boot-admin-starter-server:2.1.4")
        compile("org.springframework.boot:spring-boot-starter-web")
    }
}

project("bootiful-api") {
    dependencies {
        compile(project(":bootiful-core"))

        compile("de.codecentric:spring-boot-admin-starter-client:2.1.4")
        compile("org.springframework.boot:spring-boot-starter-web")
        compile("org.springframework.boot:spring-boot-starter-security")
        compile("org.springframework.boot:spring-boot-starter-actuator")

        runtime("org.springframework.boot:spring-boot-devtools")
    }
}
```

<br>

<details>
<summary>출처</summary>

- https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:dev3.6:dep:build_tool:gradle
- https://goddaehee.tistory.com/199
- https://galid1.tistory.com/644
- https://itholic.github.io/qa-compile-build-deploy/
- https://woowabros.github.io/tools/2019/04/30/gradle-kotlin-dsl.html

</details>