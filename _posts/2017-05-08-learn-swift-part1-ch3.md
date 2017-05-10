---
layout: post
title:  "Part1 Ch3 데이터 타입 기본"
categories: Swift
tags:  Swift3(author yagom) 
author: Babamba
---

* content
{:toc}

스위프트에서 제공하는 기본 데이터 타입에 대한 야곰님 책 요약입니다~!

##Ch3 데이터 타입 기본


* '구조체'를 타입의 기반으로 삼아 스위프트의 기능(익스텐션, 제네릭 등)을 두루 사용해 구현 됨.
* 대문자 카멜케이스를 사용한다.


##3.1 Int와 UInt

* 정수 타입

|Int|UInt|
|:--|:--|
|+, - 정수| 0을 포함한 양의 정수|

* 스위프트에서 데이터타입은 굉장히 엄격하다. 따라서 ㅏ같은 정수라도 Int와 UInt를 완전히 다른 타입으로 인식한다.

* 진수에 따른 정수 표현법
 * 10진수 : 평소 사용하는 것과 같은 숫자 사용
 * 2진수  : 접두어 0b를 사용
 * 8진수  : 접두어 0o를 사용
 * 16진수 : 접두어 0x를 사용

##3.2 Bool 
* 불리언 타입.
* true 또는 false만 값으로 가진다.

##3.3 Float과 Double

* 부동소수 타입
* 부동소수점을 사용하는 실수형

| Double | Float |
|:--|:--|
|64비트| 32비트 |
|최소 15자리의 십진수 표현 가능| 6자리의 숫자까지만 가능 |

##3.4 Character

* 단 하나의 문자를 의미

##3.5 String

* 문자열

```swift
/* 유니코드 스칼라 값 사용시 */
let unicodeScalarValue: String = "\u{2665}"
print(unicodeScalarValue)
//♥


var introduce: String = String()
/*
String()을 통해 초기화 문법을 사용하여 String 인스턴스를 초기화 한다.
만일,
var introduce: String = ""
이러한 경우는 빈 문자열 리터럴을 변수에 할당하는 것이다.
*/

/* isEmpty 프로퍼티를 통해 비어있는지 확인 */
if introduce.isEmpty {
    print("empty")
} else {
    print("Not empty")
}
//empty

/* nil인지 확인 */
if introduce == nil {
    print("introduce == nil")
} else {
    print("introduce != nil")
}
//introduce != nil

/* ""와 같은 값인지 확인 */
if introduce == "" {
    print("true")
} else {
    print("false ")
}
//true

/* 문자의 수를 세는 것 */
print( introduce.characters.count)
//0

/* append 이용하여 문자열 이어붙이기 */
introduce.append("제 이름은")
print(introduce)

/* 연산자를 이용하여 이어붙이기 */
introduce = introduce + " " + "babamba" + "입니다"
print(introduce)


let stringName: String = "name"

/* 두 문자열간 비교 */
if (introduce == stringName) {
    print("equal")
} else {
    print("Not equal")
}
//Not equal

/* 메서드를 통한 접두어 확인 */
if stringName.hasPrefix("na") {
    print("equal")
} else {
    print("Not equal")
}
//equal

/* 메서드를 통한 접미어 확인 */
if stringName.hasSuffix("na") {
    print("equal")
} else {
    print("Not equal")
}
//Not equal

/* 메서드를 통한 접미어 확인 */
if stringName.hasSuffix("me") {
    print("equal")
} else {
    print("Not equal")
}
//equal

/* 메서드를 통한 접두어 확인 */
if stringName.hasPrefix("nam") {
    print("equal")
} else {
    print("Not equal")
}
//equal

/* 메서드를 통한 접두어 확인 */
if stringName.hasPrefix("n") {
    print("equal")
} else {
    print("Not equal")
}
//equal

```

##3.5.1 특수문자

| 특수문자 | 설명 |
|:--|:--|
|\n|줄바꿈 문자|
|\\\|문자열 내 백슬래시 표현 시|
|\\"| 문자열 내 큰따옴표 표현 시|
|\t|탭 문자, 키보드 탭키 눌렀을 때와 같은 효과|
|\O|문자열이 끝났음을 알리는 null문자|

##3.6 Any, AnyObject와 nil

* Any : 스위프트의 모든 데이터 타입을 사용하여 할당 할 수 있다는 의미.
* AnyObject : Any보다 한정된 의미. 클래스의 인스턴스만 할당 가능.
 * Any, AnyObject의 사용은 줄이는 것이 좋다. 값을 사용하려면 매번 타입 확인 및 변환을 거쳐야 함은 물론, 예상치 못한 오류와 위험이 증가됨.
* nil : 특정 타입이 아닌 '없음'을 나타내는 키워드. 변수 또는상수에 값이 들어있지 않고 비어있음을 나타낸다. 
