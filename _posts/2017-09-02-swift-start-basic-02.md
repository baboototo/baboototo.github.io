---
layout: post
title: Swift3 기초문법 - 2
date: 2017. 09. 02
---

### # While 반복문
주어진 조건에 의해 특정 코드 블록을 반복적으로 실행하는 구분


- - -


### # While 반복문 사용법

* 기본 While 반복문
 > 비교조건 결과값이 false 일 경우까지 반복

 ```swift
 	var index: Int = 0
	/*
	 * while 비교조건
	 */
	while index < 10 {
		index = index + 1
		print("지금 index 값은 \(index) 입니다")
	}
 ```

* repeat ~ while 반복문
 > 처음 한번은 실행 후, 비교조건 결과값 false 일 경우까지 반복
 > 예) repeat 실행 -> while 비교조건 확인 (true 경우) -> repeat 실행 ...
 > 예) repeat 실행 -> while 비교조건 확인 (fasle 경우) -> 종료 ...

 ```swift
	var index: Int = 0

	repeat{
		index = index + 10
		print("지금 index 값은 \(index) 입니다")
	}while index < 10
 ```

- - -
