---
layout: post
title: Spring Boot Gradle Logback 설정
date: 2019. 08. 01
---

Spring Boot / Gradle 기반에서 Logback 설정 입니다.
콘솔 로그와 파일 로그 기본 설정 방법으로 작성되었습니다.


#### # Gradle 설정
* Logback dependencies 추가
```
	dependencies {
		implementation 'org.bgee.log4jdbc-log4j2:log4jdbc-log4j2-jdbc4:1.16'
	}
```

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
```

- - -

#### # Logback 로그 설정 파일 추가
* console.xml : 콘솔 로그 설정
```
	<?xml version="1.0" encoding="UTF-8"?>
	<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
		<encoder>
			<charset>UTF-8</charset>
			<pattern>%d{yyyyMMdd HH:mm:ss.SSS} [%thread] %-3level %logger{5} - %msg %n</pattern>
		</encoder>
	</appender>
```

* 파일 로그 설정 XML 파일 추가
```
	<?xml version="1.0" encoding="UTF-8"?>
	<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
		<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
			<fileNamePattern>${LOG_FILE}/stc_api_server_log.%d{yyyy-MM-dd}-%i.log</fileNamePattern>
			<maxHistory>30</maxHistory>
			<timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
				<maxFileSize>100MB</maxFileSize>
			</timeBasedFileNamingAndTriggeringPolicy>
		</rollingPolicy>
		<encoder>
			<charset>UTF-8</charset>
			<pattern>%d{yyyy:MM:dd HH:mm:ss.SSS} %-5level --- [%thread] %logger{35} : %msg %n</pattern>
		</encoder>
	</appender>
```

* Spring Boot Logback 설정 XML 추가
```
<?xml version="1.0" encoding="UTF-8"?>
	<configuration>

		<logger name="jdbc.sqlonly" level="off" />
		<logger name="jdbc.sqltiming" level="info" />
		<logger name="jdbc.audit" level="off" />
		<logger name="jdbc.resultset" level="off" />
		<logger name="jdbc.resultsettable" level="off" />
		<logger name="jdbc.connection" level="off" />

		<springProfile name="local">
			<include resource="log/console.xml" />
			<root level="info">
				<appender-ref ref="CONSOLE"/>
			</root>
		</springProfile>

		<springProfile name="local-live">
			<include resource="log/console.xml" />
			<root level="info">
				<appender-ref ref="CONSOLE"/>
			</root>
		</springProfile>

		<springProfile name="real">
			<include resource="log/file.xml" />
			<root level="info">
				<appender-ref ref="FILE"/>
			</root>
		</springProfile>

	</configuration>
```




#### # Loagback 설정 application.yaml 추가
 * com.project.package : 프로젝트 기본 패키지로 설정
```
	logging:
		file: /usr/local/logs
		level:
			com.project.package: info
```
