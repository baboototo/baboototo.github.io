---
layout: post
title: Spring Boot Gradle Mybatis SQL Logback 설정
date: 2019. 08. 01
---

Spring Boot / Gradle 기반 Mybatis SQL Logback 설정 입니다.


#### # [참고] Logback 기본 설정
* [Spring Boot Gradle Logback 설정](https://baboototo.tistory.com/24)

- - -

#### # Logback 설정 디렉토리 및 파일 구성
```
	- src
		- main
			- resources
				- log
					- console.xml
					- file.xml
				- logback-spring.xml
				- log4jdbc.log4j2.properties
```

- - -

#### # Logback 로그 설정 파일 추가
* log4jdbc.log4j2.properties : SQL Log Delegator 설정
```
log4jdbc.spylogdelegator.name=net.sf.log4jdbc.log.slf4j.Slf4jSpyLogDelegator
log4jdbc.dump.sql.maxlinelength=0
```

#### # Loagback 설정 application.yaml Datasource 설정 변경
 * url: log4jdbc 변경
 * driver-class-name: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
```
	spring:
		profiles: local
		datasource:
			url: jdbc:log4jdbc:oracle:thin:@123.123.123:1521:ORCL
			driver-class-name: net.sf.log4jdbc.sql.jdbcapi.DriverSpy
			username: name
			password: password
```
