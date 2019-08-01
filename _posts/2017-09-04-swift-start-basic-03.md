---
layout: post
title: Swift3 기초문법 - 3
date: 2017. 09. 04
---


### # 조건문
조건문은 말 그대로 특정 조건이 참 또는 거짓에 따라 분기되는 것을 말한다.
그래서 조건문은 다른말로 분기문 이라고도 불린다.

조건문에는 **if, guard, switch** 세가지 종류가 있다.


- - -


### # if 구문
 > 조건문이 true 일 경우 코드 블록 내부 구문을 실행한다.

```swift
	/*
		if 조건문 {
			if 조건문이 참 일 경우 실행
		} else if 조건문 {
			else if 조건문이 참 일 경우 실행
		} else {
			조건문들이 거짓 일 경우 실행
		}
	*/
	var name: String = "JAEYOUNG"
	if name == "KWAG" {
		print("당신에 이름은 JAEYOUNG 이군요.")
	} else if name == "JAEYOUNG" {
		print("당신에 이름은 JAEYOUNG 이군요.")
	} else {
		print("당신 이름은 무엇 인가요??")
	}
 ```


- - -


### # guard 구문
 > if 구문과 비슷한 구문이며 else 구문을 필수로 정의해야 한다.
 > guard 구문은 조건 참일 때 실행되는 블록이 없는게 특징이다.
 > 특정 함수에서 값을 확인 후 처리해야 하는 경우 간단히 사용할 수 있는 구문이다.

```swift
	/*
	 * Guard 구문
	 */
	func showMove(age: Int){
		guard !(age < 19) else {
			print("19세 이상만 시청할 수 있습니다.")
			return
		}
		print("시청 가능 합니다.")
	}

	var myAge: Int = 18;
	showMove(age: myAge)
```
```swift
	/*
	 * Guard 구문를 if 구문으로 대체 할 경우
	 */
	func showMove(age: Int){
		if age < 19 {
			print("19세 이상만 시청할 수 있습니다.")
			return
		}
		
		print("시청 가능 합니다.")
	}

	var myAge: Int = 18;
	showMove(age: myAge)
 ```


- - -


### # switch 구문
 > if else 구문과 유사하게 처리 되지만, swift 구믄은 다양한 가능성이 있는 여러개의 조건 비교에 효율적이다.
 > swift 구문의 경우 default 구문은 필수로 작성해야 한다.

```swift

	var name: String = "jaeyoung"

	/*
		switch 비교대상 {
			case 비교값 1:
				비교대상과 비교값 1이 같을 경우 실행

			case 비교값 2:
				비교대상과 비교값 2이 같을 경우 실행

			default:
				비교대상과 같은 비교값이 없을 경우 실행
		}
	*/
	switch name {
		case "jaeyoung":
			print("이름은 소문자 \(name) 입니다.")

		case "JAEYOUNG":
			print("이름은 대문자 \(name) 입니다.")

		default:
			print("일치하는 이름이 없습니다.")
	}

 ```