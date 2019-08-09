---
layout: post
title: Windows Server Java 및 Tomcat 설치 및 설정
date: 2019. 08. 09
---

Spring Boot / Gradle 기반 서비스를 외부 Tomcat 배포 및 서비스 등록까지 블로그 작성
 1. [Spring Boot / Gradle 기반] Spring Boot 서비스 배포 War 만들기 (IntelliJ IDE 사용)
 2. [Spring Boot / Gradle 기반] Windows Server Java 및 Tomcat 설치 및 설정
 3. [Spring Boot / Gradle 기반] 윈도우 서버에 서비스 등록 및 확인


- - -

#### #  Windows Server 설정 준비
 > Spring Boot 설정된 내부 톰캣이 아닌 외부 톰캣을 이용해 윈도우 서버에 배포
 > 외부 톰캣 설정 시 아래 3가지 설정값을 꼭 넣어서 설정해 줘야 다른 서비스와 충돌을 피할 수 있다.
 > JRE_HOME 따로 설정한 이유는 기본 설정된 Java 버전이 다를 경우를 위해 설정한 것이다.
 > * JRE_HOME
 > * CATALINA_HOME
 > * CATALINA_BASE


* Java / Tomcat 다운로드
* 서비스 디렉토리 구성
* Tomcat 설정
* Tomcat Start / Shutdown

- - -

#### # Java / Tomcat 다운로드
* [Java Download](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
 > Java 다운로드 및 설치 방법은 구글링을 통해 참고

* [Tomcat Download](https://tomcat.apache.org/download-80.cgi)

- - -

#### # 서비스 디렉토리 구성
* Java / Was / Source 디렉토리 구성
 1. java - Java 설치된 디렉토리
  > 신규로 개발된 서비스가 기존 동일한 자바 버전이 같은 경우 기존 자바 디렉토리를 설정
  > 신규로 개발된 자바 버전이 더 높을 경우 위와 동일하게 설정한다.

 2. was - Tomcat 설치된 디렉토리
  > 다른 서비스와 같은 톰캣으로 사용할 경우 배포 또는 장애시 모든 서비스가 Shutdown 되는 것을 막기 위해 별도로 사용

 3. source - 서비스 대상 디렉토리
  > 신규로 개발된 소스 디렉토리 (WAR 파일 또는 WAR 압축 해제 후 소스 적용)

 ![](http://baboototo.github.io/images/blogs/springboot/springboot-windows-tomcat-setting.png)

- - -

#### # Tomcat startup.bat 설정 [ tomcat\bin\startup.bat ]
 * Java / Catalina 경로 설정
  > 소스 편집기 또는 메모장을 열어서 setlocal 아래에 JRE_HOME/CATALINA_HOME/CATALINA_BASE 경로 정보 설정

 ```[xml]
	setlocal

	set "JRE_HOME=E:\Test\java\jdk1.8.0_171\jre"
	set "CATALINA_HOME=E:\Test\was\apache-tomcat-8.5.43"
	set "CATALINA_BASE=E:\Test\was\apache-tomcat-8.5.43"
 ```

- - -

#### # Tomcat shutdown.bat 설정 [ tomcat\bin\shutdown.bat ]
 * Java / Catalina 경로 설정
  > 소스 편집기 또는 메모장을 열어서 setlocal 아래에 JRE_HOME/CATALINA_HOME/CATALINA_BASE 경로 정보 설정

 ```[xml]
	setlocal

	set "JRE_HOME=E:\Test\java\jdk1.8.0_171\jre"
	set "CATALINA_HOME=E:\Test\was\apache-tomcat-8.5.43"
	set "CATALINA_BASE=E:\Test\was\apache-tomcat-8.5.43"
 ```

- - -

#### # Tomcat shutdown.bat 설정 [ tomcat\bin\shutdown.bat ]
 * Java / Catalina 경로 설정
  > 소스 편집기 또는 메모장을 열어서 setlocal 아래에 JRE_HOME/CATALINA_HOME/CATALINA_BASE 경로 정보 설정

 ```[xml]
	setlocal

	set "JRE_HOME=E:\Test\java\jdk1.8.0_171\jre"
	set "CATALINA_HOME=E:\Test\was\apache-tomcat-8.5.43"
	set "CATALINA_BASE=E:\Test\was\apache-tomcat-8.5.43"
 ```

- - -

#### # Tomcat catalina.bat 설정 [ tomcat\bin\catalina.bat ]
 * Catalina 옵션 설정
  > 소스 편집기 또는 메모장을 열어서 setlocal 아래에 JRE_HOME/CATALINA_HOME/CATALINA_BASE 경로 정보 설정
  >
  > - 옵션 참고
  > -Xms : 최소 힙 사이즈
  > -Xmx : 최대 힙 사이즈
  > -XX:NewSize : New Generation의 최소 사이즈
  > -XX:MaxNewSize : New Generation의 최대 사이즈
  > -XX:MaxPermSize : Permanent Generation의 최대 사이즈
  > -XX:SurvivorRatio : 영역비율(New Generation)

 ```[xml]
	setlocal

	set "CATALINA_OPTS=-Xms1024M -Xmx2048M -XX:PermSize=512M -XX:MaxPermSize=512M"
 ```

- - -

#### # Tomcat setenv.bat 설정 [  tomcat\bin\setenv.bat ]
 * Tomcat bin 디렉토리에 setenv.bat 파일 생성
  > setenv.bat 파일은 톰캣 구동시 자동으로 실행 된다.
  > setenv.bat 파일에는 커스텀 옵션을 지정할 수 있다.

 ```[xml]
	set JAVA_OPTS=%JAVA_OPTS% -Djava.awt.headless=true -Dfile.encoding=UTF-8 -server -Xms1024M -Xmx1024M -XX:NewSize=512m -XX:MaxNewSize=512m -XX:PermSize=512M -XX:MaxPermSize=512M -XX:+DisableExplicitGC

 ```

- - -

#### # Tomcat web.xml 설정 [  tomcat\conf\web.xml ]
 * [참고] Spring Boot application.yaml 또는 application.properties 설정 시 Active profiles을 설정 할 수 있다.
 ```[xml]
	<context-param>
		<param-name>spring.profiles.active</param-name>
		<param-value>live</param-value>
	</context-param>

 ```

- - -

#### # Tomcat server.xml 설정 [  tomcat\conf\server.xml ]
 * Tomcat Context 등록
 ```[xml]
	<Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
		 <!-- Tomcat 서비스 구동 Context 설정  -->
		<Context docBase="E:\Test\source\test-api" path="/" reloadable="true"/>
	</Host>

 ```

- - -

#### # Tomcat Start / Shutdown
 * Windows cmd Console 실행 후 아래 명령어로 실행 및 중지
  > E:\Test\was\apache-tomcat-8.5.43\bin> startup.bat
  > E:\Test\was\apache-tomcat-8.5.43\bin> shutdown.bat


- - -


