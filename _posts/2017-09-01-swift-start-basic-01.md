---
layout: post
title: Swift3 기초문법 - 1
date: 2017. 09. 01
---

### # For 반복문
주어진 조건에 의해 특정 코드 블록을 반복적으로 실행하는 구분

- - -

### # For 반복문 사용법

* 기본 for 증가 반복문
 > index 값이 0부터 9까지 1씩 증가 하는 기본 반복문

 ```swift
	/*
	 * for 초기값 설정; 비교조건; 증가값
	 */
	for var index = 0; index < 10; index++ {
		print("idx 값이 \(index) 이네요.")
	}
 ```

* 기본 for 감소 반복문
 > index 값이 10부터 까지 1씩 감소 하는 기본 반복문

 ```swift
	/*
	 * for 초기값 설정; 비교조건; 감소값
	 */
	for var index = 10; index > 2; index-- {
		print("index 값이 \(index) 이네요. ")
	}
 ```

* for ~ in
 > 배열 또는 딕셔너리에 들어 있는 데이터 개수 만큼 반복 처리하는 반복문

 ```swift
	var title: Array<String> = ["Egg", "Apple", "Dog"];
	/*
	 * for 초기값 설정; 비교조건; 감소값
	 */
	for var name in title  {
		print("이것의 이름은 \(name) 이네요. ")
	}
 ```

- - -

### # 부록
Swift3 에서는 기본 For 문을 사용할 수 없게 됐다. ㅡ.ㅡ;;;
기본 For 문법이 아닌.. for ~ in 문법으로 코딩해야 한다.
코드를 줄이기 위해 사용하지 못하게 하지 않았을까?? 라는 생각이...



