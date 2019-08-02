---
layout: post
title: Windows10 Elasticsearch7 설치
date: 2019. 08. 02
---

#### # Windows10에 Elasticsearch7 설치
* Java8 이상 버전 설치 (추후 Java11 버전 설치 권장)
* Elasticsearch 다운로드
* Elasticsearch 설치
* Elasticsearch 최초 실행 및 서비스 등록
* Elasticsearch 실행 및 종료

- - -

#### # Elasticsearch 다운로드
![](http://baboototo.github.io/images/blogs/elasticsearch/elasticsearch-setting-01.png)

![](http://baboototo.github.io/images/blogs/elasticsearch/elasticsearch-setting-02.png)

- - -

#### # Elasticsearch 설치
 > 다운로드 받은 폴더에서 압축을 해제 한다.
 > 압축을 해제한 폴더를 원하는 특정 폴더로 이동
 > 설치 끝 ( 다운로드 해지 후 바로 실행이 가능함. )


- - -

#### # Elasticsearch 실행 및 서비스 등록
* Window cmd 실행

* **cd ..\elasticsearch-7.3.0\bin** 폴더로 이동

* **elasticsearch.bat** 입력 후 실행 아래 메세지 출력시 실행 완료

 ![](http://baboototo.github.io/images/blogs/elasticsearch/elasticsearch-setting-03.png)

* **curl localhost:9200** cmd 입력 후 실행

 ![](http://baboototo.github.io/images/blogs/elasticsearch/elasticsearch-setting-04.png)

* **elasticsearch-service.bat install** cmd 입력 후 실행 아래 메세지 출력시 서비스 등록 완료

 ![](http://baboototo.github.io/images/blogs/elasticsearch/elasticsearch-setting-05.png)

- - -

#### # Elasticsearch 실행 및 종료
* elasticsearch-service 명령어를 통해 시작/중지/삭제/관리등을 실행할 수 있음.
 ![](http://baboototo.github.io/images/blogs/elasticsearch/elasticsearch-setting-06.png)

- - -

#### # Elasticsearch elasticsearch.bat 실행 오류 시
> **elasticsearch-7.3.0\bin** 폴더로 이동
> **elasticsearch.bat** 파일 편집 실행(메모장, Atom, EditPlus 등..)
> **SET "JAVA_HOME=C:\Program Files\Java\jdk1.8.0_191"** 내용 입력 후 저장(자바경로 설정)
> **elasticsearch.bat** Window cmd 다시 실행


