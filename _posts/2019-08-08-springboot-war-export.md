---
layout: post
title: Spring Boot / Gradle 기반 서비스 배포 War 만들기 (IntelliJ IDE 사용)
date: 2019. 08. 08
---

Spring Boot / Gradle 기반 서비스를 외부 Tomcat 배포 및 서비스 등록까지 블로그 작성
 1. [Spring Boot / Gradle 기반] 서비스 배포 War 만들기 (IntelliJ IDE 사용)
 2. [Spring Boot / Gradle 기반] 서비스 윈도우 서버에 Java 및 Tomcat 설치 및 설정
 3. [Spring Boot / Gradle 기반] 윈도우 서버에 서비스 등록 및 확인


- - -

#### # Spring Boot War 배포 준비
* Gradle build.gradle 설정
* Spring Boot Application 설정
* Gradle Build war 파일 생성
* Gradle Build war 파일 확인


- - -

#### # Gradle build.gradle 설정
* 플러그인 war 설정 되어 있는 지 확인 (없을 경우 플러그인 추가)
```[java]
	apply plugin: 'war'
```

* war 배포 파일명 설정
```[java]
	bootWar {
		archiveBaseName = "test-api"
		archiveFileName = "test-api.war"
		archiveVersion = "0.0.0"
	}
```

- - -

#### # Spring Boot Application 설정
* Spring Boot Application Class 설정 확인
 1. SpringBootServletInitializer extends 여부 확인
 2. configure 메소드 ServletInitializer.class 설정 확인
 3. main 메소드 ServletInitializer.class 설정 확인

 ```[java]
	@SpringBootApplication
	public class TestApiApplication extends SpringBootServletInitializer {
		@Override
		protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
			return application.sources(ServletInitializer.class);
		}

		public static void main(String[] args) {
			SpringApplication.run(ServletInitializer.class, args);
		}
	}
 ```

- - -

#### # Gradle Build war 파일 생성
 * Gradle > Tasks > build > bootWar 실행
![](http://baboototo.github.io/images/blogs/springboot/springboot-war-export.png)

- - -

#### # Gradle Build war 파일 확인
 * ${project directory}/build/libs/test-api.war

