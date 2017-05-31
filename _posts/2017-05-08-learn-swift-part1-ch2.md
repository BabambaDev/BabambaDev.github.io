---
layout: post
title:  "Part1 Ch2 스위프트 기초"
date: 2017-05-08 10:00
categories: Swift
tags:  Swift3
author: Babamba
---

* content
{:toc}

ch2 스위프트 처음 시작하기 부분을 요약 정리 하겠습니다~! 중간에 빠진 부분은 임의로 생략한 부분입니다~

---

## 2장

### 2.1 기본 명명 규칙

* 변수, 상수, 함수, 메서드, 타입 등의 이름은 유니코드에서 지원하는 어떤 문자(한글, 한자, 영문, 숫자, 이모티콘 등등)라도 사용가능. 다만, 다음과 같은 예외경우는 사용불가.
 * 스위프트의 예약어, 키워드
 * 해당 코드 범위내 사용되고 있는 기존 이름과 동일한 이름
 * 연산자(+,-,*,/)로 사용될 수 있는 기호
 * 숫자로 시작하는 이름
 * 공백이 포함된 이음

* 함수, 메서드, 인스턴스 이름은 소문자 카멜케이스
* 클래스, 구조체, 익스텐션, 프로토콜, 열거형 이름은 대문자 카멜케이스
* 대소문자 구별. 예 : Var과 var는 다르게 인식됨.
* 스위프트에서 세미콜론 : 명령구문 뒤 세미콜론(;)을 붙이는 것은 선택 사항이다.

### 2.2 콘솔로그

디버깅 중 디버깅 콘솔에 보여줄 로그. print()와 dump() 함수를 사용한다.

* 차이점

|print|dump|
|:--|:--|
|디버깅 콘솔에 간략한 정보를 출력해줌|print함수에 비해 좀더 자세한 정보 출력|
|출력하려는 인스턴스의 description 프로퍼티에 해당하는 내용을 출력|출력하려는 인스턴스의 자세한 내부 컨텐츠까지 출력|

```swift
class Person {
    var height: Float = 0.0
    var weight: Float = 0.0
}

let babamba: Person = Person()
babamba.height = 175.0
babamba.weight = 80.0

print(babamba)
//__lldb_expr_105.Person
print(babamba.height)
//175.0
print(babamba.weight)
//80.0

dump(babamba)
/*
▿ __lldb_expr_109.Person #0
  - height: 175.0
  - weight: 80.0
*/

let name: String = "babamba"
print(name)
//babamba
dump(name)
//- "babamba"

struct Point {
    let x: Int, y: Int
}


/******* extention 추가 안했을 경우 *******/
let p = Point(x: 10, y: 12)
print(p)
//Point(x: 10, y: 12)
dump(p)
/*
▿ __lldb_expr_113.Point
  - x: 10
  - y: 12
*/

/******* extention 추가시 *******/
let p = Point(x: 10, y: 12)
print(p)
//(10,  12)
dump(p)
/*
▿ (10,  12)
  - x: 10
  - y: 12
*/

extension Point: CustomStringConvertible {
    var description: String {
        return "(\(x),  \(y))"
    }
}

print(p)
//(10,  12)
dump(p)
/*
▿ (10,  12)
  - x: 10
  - y: 12
*/

/*
extention 추가 여부로 알아낸 사실!
extention 추가 됐을 떄의 예제를 보면,
순서상 상수 p가 먼저 불리고 이후에 extension 부분이 불려,

Point(x: 10, y: 12)
(10,  12)

이와 같은 결과가 나올 줄 알았지만,

extention 추가 됐을 떄의 예제처럼 나왔다.

print(), dump()함수가 불릴때 순서 대로 불리는것이 아니라, extention이 있을때 extention 부분을 거친후 콘솔로그에 표시된다.

즉, 'let p -> print(p) -> extention 부분 -> 콜솔로그 출력'의 순서가 된다.

프로젝트를 생성하여 아래와 같은 코드를 작성하여 브레이크 포인트를 찍어 확인하였다.

struct Point {
    let x: Int, y: Int
}

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        let p = Point(x: 10, y: 12)

        print(p)
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

}

extension Point: CustomStringConvertible {
    var description: String {
        return "(\(x), \(y))"
    }
}
*/

```

### 2.2.2 문자열 보간법

* 변수 또는 상수 등의 값을 문자열 내에 나타내고 싶을 때 사용.
* 보간법을 통해 문자열로 치환되려면 변수나 상수의 타입이 CustomStringConvertible 프로토콜을 준수해야 한다.

```swift

let name: String = "babamba"
print("My name is \(name)")

```

### CustomStringConvertible

* 사용자 정의 텍스트 표현이있는 유형.
* CustomStringConvertible 프로토콜을 준수하는 유형은 인스턴스를 문자열로 변환 할 때 사용할 자체 표현을 제공 할 수 있다.
* description 프로퍼티를 갖는다.
 * print()함수를 사용할 경우 이를 가져온다.

```swift

let age: Int = 34
print(age.description)
//34
print(age)
//34

```

### 2.3 주석


### 2.4 변수와 상수

* 변수 : 생성 후 데이터 값을 변경할 수 있다.
* 상수 : 한번 설정하면 추후 변경 불가.

### 2.4.1 변수

* var [변수명]: [데이터타입] = [값]

* 변수 생성시 데이터 타입을 생략하면 컴퍼일러가 타입을 추론하여 지정한다. (야곰님은 잘못된 타입 추론으로 인한 오류를 방지하기 위해 타입을 설정하기를 권장합니다.)

```swift

var name: String = "babamba"
var age: Int = 100

```

### 2.4.2 상수

* let [상수명]: [데이터 타입] = [값]
* 변수와 마찮가지로 타입을 생략할 수 있다.

```swift

let name: String = "babamba"
let age: Int = 100

```

* 변수를 사용하지 않고 상수를 사용하는 경우의 이유
 * 가독성(이후 코드에서 값의 변화가 없다는 사실을 주석이나,API문서 등을 살펴보지 않고 직관적으로 알 수 있음)

