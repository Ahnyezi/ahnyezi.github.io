---
title:  "[CS] 빌드도구 Gradle vs Maven "
date: 2020-9-13
categories: ['CS']
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

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <!-- 필수사항 : 프로젝트 정보 기재 부분 -->
  <modelVersion>4.0.0</modelVersion>
  <groupId>ojava.blog</groupId>
  <artifactId>mavendemo</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  <name>mavendemo Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <!-- 선택사항 : 해당 프로젝트에 대한 설명 -->
  <description>demo project for blog</description>
  
  <!-- 선택사항 : pom.xml에서 공통적으로 사용할 버전 또는 설정값 정보 -->
  <properties>
  	<!-- 각 Dependency에 지정해줘도 되나 상단에 써두어서 알아보기 쉽게하려는 의도가 더 많음 -->
  
  	<!-- build properties -->
  	<jdk.source>1.11</jdk.source>
  	<jdk.target>1.11</jdk.target>
  	
  	<!-- JSTL / JSP -->
  	<jstl.version>1.2</jstl.version>
  	<jsp.version>2.3.0</jsp.version>
  	<servlet.version>3.0.1</servlet.version>
  	
  	<!-- 위 내용처럼 tag명을 원하는 대로 지정하고 값을 주는 방식으로 자유롭게 사용 가능 -->
  </properties>
  
  
  <!-- 필수내용 : 프로젝트에 import 되는 dependency 목록 -->
  <dependencies>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <!-- 상단에 선언했던 properties에 기재한 내용을 이렇게 사용함 -->
      <version>${servlet.version}</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>jstl</artifactId>
      <version>${jstl.version}</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>javax.servlet.jsp-api</artifactId>
      <version>${jsp.version}</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.el</groupId>
      <artifactId>javax.el-api</artifactId>
      <version>2.2.2</version>
      <scope>provided</scope>
    </dependency>
                  
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    
    <!-- 기타 등등... 각 프로젝트에 맞는 내용을 추가하면 된다. -->
  </dependencies>

  <!-- 필수내용 : 프로젝트 build 정보 -->
  <build>
    <!-- 선택사항 : resource 정보 기재 -->
    <resources>
    	<resource>
    		<directory>src/main/webapp</directory>
    	</resource>
    </resources>
    
    <!-- 선택사항 : maven 관련 plugin 정보 기재 -->
    <plugins>
    	<groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-source-plugin</artifactId>
        <version>2.2.1</version>
        <executions>
          <execution>
        	<id>attach-sources</id>
            <goals>
            	<goal>jar</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <includes>
            <include>${basedir}/src/main/webapp/**</include>
          </includes>
        </configuration>
      </plugin>
      
      <!-- 상단 내용 외 원하는 plugin 관련 내용을 기재하면 된다. -->
    </plugins>
  </build>
  
  <!-- 선택사항 : repository 별도 설정 -->
  <distributionManagement>
  	<repository>
  		<id>nexus</id>
        <name>nexus-release</name>
        <url>${nexus.uri}/content/repository/release</url>
  	</repository>
    
    <!-- 목적에 따라 release, snapshots, 3rd party 등 나눠서 등록할 수 있다 -->
  </distributionManagement>
  
  <!-- 선택사항 : 검증/운영 배포 정보를 구분할 수 있는 설정 profiles -->
  <profiles>
 	<profile>
 		<id>test</id>
 		<activation>
 			<activeByDefault>true</activeByDefault>
 		</activation>
 		<build>
 			<finalName>${project.artifactId}-${project.version}</finalName>
 			<directory>${project.basedir}/target</directory>
 			<resources>
 				<resource>
 					<directory>${project.basedir}/src/main/resources</directory>
 				</resource>
 				<resource>
 					<directory>${project.basedir}/src/main/setting/test</directory>
 				</resource>
 			</resources>
 		</build>
 	</profile>  
 	<profile>
 		<id>product</id>
 		<activation>
 			<activeByDefault>true</activeByDefault>
 		</activation>
 		<build>
 			<finalName>${project.artifactId}-${project.version}</finalName>
 			<directory>${project.basedir}/target</directory>
 			<resources>
 				<resource>
 					<directory>${project.basedir}/src/main/resources</directory>
 				</resource>
 				<resource>
 					<directory>${project.basedir}/src/main/setting/product</directory>
 				</resource>
 			</resources>
 		</build>
 	</profile>  
  </profiles>
</project>
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

- https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:dev3.6:dep:build_tool:gradle <br>
- https://goddaehee.tistory.com/199<br>
- https://galid1.tistory.com/644<br>
- https://itholic.github.io/qa-compile-build-deploy/<br>
- https://woowabros.github.io/tools/2019/04/30/gradle-kotlin-dsl.html<br>
- https://ojava.tistory.com/147<br>

</details>