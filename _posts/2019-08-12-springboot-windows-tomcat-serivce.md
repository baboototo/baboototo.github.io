---
layout: post
title: Windows Server 서비스에 Tomcat 등록 및 확인
date: 2019. 08. 12
---

Spring Boot / Gradle 기반 서비스를 외부 Tomcat 배포 및 서비스 등록까지 블로그 작성
 1. [[Spring Boot / Gradle 기반] Spring Boot 서비스 배포 War 만들기 (IntelliJ IDE 사용)](https://baboototo.tistory.com/29)
 2. [[Spring Boot / Gradle 기반] Windows Server Java 및 Tomcat 설치 및 설정](https://baboototo.tistory.com/30)
 3. [[Spring Boot / Gradle 기반] Windows Server 서비스에 Tomcat 등록 및 확인](https://baboototo.tistory.com/31)


- - -

#### #  Windows Service 설정 준비
 * Tomcat service.bat 설정
 * Tomcat 설정 옵션 exe 파일 만들기
 * 윈도우 서비스 Tomcat 등록
 * 윈도우 서비스 Tomcat 삭제
 * 윈도우 서비스 Tomcat 확인

- - -

#### # Tomcat service.bat 설정 [ tomcat\bin\service.bat ]
* Tomcat service.bat 에서 SERVICE_NOME / DISPLAYNAME 설정
 > 소스 편집기 또는 메모장을 열어서 라인 83 줄에 SERVICE_NOME / DISPLAYNAME 변경
 > SERVICE_NOME : 서비스 이름
 > DISPLAYNAME  : 표시 이름

 ```[xml]
	rem Set default Service name
	rem set SERVICE_NAME=Tomcat8
	rem set DISPLAYNAME=Apache Tomcat 8.5 %SERVICE_NAME%

	set SERVICE_NAME=test-api
	set DISPLAYNAME=Test Api Server
 ```

- - -

#### #Tomcat 설정 옵션 exe 파일 만들기 [ tomcat\bin\tomcat8w.exe ]
* Tomcat Properites 설정 화면 만든기
  > tomcat\bin\tomcat8w.exe 파일을 복사 붙여 넣기 한다.
  > tomcat8w.exe 파일명을 위에 설정한 SERVICE_NAME 동일하게 변경
  > test-api.exe 파일을 실행하면 Tomcat Properties 설정 화면 실행 됨

 ![](http://baboototo.github.io/images/blogs/springboot/springboot-windows-service-setting-01.png)
 ![](http://baboototo.github.io/images/blogs/springboot/springboot-windows-service-setting-02.png)

- - -

#### # 윈도우 서비스 Tomcat 등록 [ tomcat\bin\service.bat ]
* Tomcat 윈도우 서비스 등록 (cmd에서 실행)
 ```[xml]
	tomcat\bin\service.bat install
 ```

- - -

#### # 윈도우 서비스 Tomcat 삭제 [ tomcat\bin\service.bat ]
* Tomcat 윈도우 서비스 삭제 (cmd에서 실행)
 ```[xml]
	tomcat\bin\service.bat remove
 ```

- - -

#### # 윈도우 서비스 Tomcat 확인
* 윈도우 서비스 실행
 > 윈도우키 + R 누르고 services.msc 입력 후 엔터
 > 윈도우 서비스 창에서 등록된 서비스명 검색 후 속성 확인

- - -



