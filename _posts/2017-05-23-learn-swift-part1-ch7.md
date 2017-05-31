---
layout: post
title:  "Part1 Ch7 함수"
date: 2017-05-23 11:10
categories: Swift
tags:  Swift3(책 저자 yagom님) 
author: Babamba
---

* content
{:toc}

스위프트에서 제공하는 함수에 대한 야곰님 책 요약입니다~!

##Ch7 함수

##7.1 함수와 메서드

|메서드| 구조체, 클래스, 열거형 등 특정타입에 연관되는 함수|
|:--|:--|
|함수|모듈전체에서 전역적으로 사용할 수 있는 함수|

* 상황, 위치에 따라 다른 용어로 부르는것 뿐이다.

##7.2 함수의 정의와 호출

* 소괄호(**()**) 생략 불가
* 오버라이드(재정의), 중복정의(오버로드) 모두 지원

|   | 오버라이딩  | 오버로딩  |
|:-:|:-:|:-:|
| 메서드 명  | 같음  | 같음  |
| 파라미터  | 같음  | 개수가 다르거나\n 개수가 같을 시 자료형이 달라야함  |
| 리턴형  | 같음  | 관계없음  |
| 메서드정의  | 재정의O  | 재정의X  |

###7.2.1 기본적인 함수의 정의와 호출

```swift

func 함수이름 (매개변수...) - > 반환타입 {
	실행구문
	return 반환값
}

```

```swift

func helloWithName(name: String) -> String {
    return "Hello \(name)"
}

let helloBabamba: String = helloWithName(name: "babamba")
print(helloBabamba)
//Hello babamba

```

|매개변수|외부로부터 받아들이는 전달 값|
|:--|:--|
|전달인자|함수를 실제로 호출할 때 전달하는 값|

* 상기 코드에서 매개변수는 name, 전달인자는 "Jenny" 

###7.2.2 매개변수

###매개변수가 없는 함수와 매개변수가 여러 개인 함수

* 매개변수가 필요없다면 공란으로 함

```swift
func helloWorld() -> String {
    return "hello, world!"
}
```

* 매개변수가 여러개일때 : 쉼표(**,**)로 구분
* 매개변수 이름 : 호출 시 매개변수에 붙이는 이름 

```swift
func sayHello(myName: String, yourName: String) -> String {
    return "Hello \(yourName)! i'm \(myName)"
}

print(sayHello(myName: "babamba", yourName: "Jenny"))
```

###매개변수 이름과 전달인자 레이블

```swift
/*
  매개변수      : from, to
  전달인자 레이블 : myName, name
*/

func sayHello(from myName: String, to yourName: String) -> String {
    return "Hello \(yourName)! I'm \(myName)"
}

print(sayHello(from: "babamba", to: "Jhone"))

```
* 함수 내부에서는 전달인자 레이블을 이용하여 구현하며, 이후 함수를 사용할 때는 매개변수 이름을 사용하여 값을 넣는다.

###와일드 카드 식별자를 이용해 매개변수 이름이 없는 함수 구현

* 와일드 카드를 사용하여 매개변수 이름을 없에면, 함수 내부에서는 전달인자 레이블을 사용하여 구현하고, 이후 함수를 사용할 때는 매개변수 이름이나 전달인자 레이블을 사용하지 않고 바로 값을 넣는다. 

```swift
func sayHello(_ name: String, _ times: Int) -> String {
    var result: String = ""
    
    for _ in 0..<times {
        result += "Hello \(name)!" + " "
    }
    
    return result
}

print(sayHello("babamba", 3))

```

* 매개변수 이름을 변경하면 함수 이름이 변경된다.
* 매개변수 이름만 다르게 써준다면 오버로드가 된다.
* 전달인자 레이블은 함수 이름에 포함되지 않는다. 따라서 매개변수 이름과 타입이 같고 함수이름이 동일한 상태에서 전달인자만 바꾼다면 오버로드가 되지 않는다.

* 아래와 같이 정의를 하면 오버로드가 되지 않는다

```swift
func sayHello(_ name: String, _ time: Int) -> String {
    var result: String = ""
    
    for _ in 0..<time {
        result += "Hello \(name)!" + " "
    }
    
    return result
}
 => 오류 코드
```

###매개변수 기본값

* 매개변수마다 기본값을 지정가능 -> 매개변수가 전달 되지 않으면 기본값 사용
* 매개변수 기본값이 들어간다고 해서 오버로드가 되는 것 아님 -> 책에서는 다르게 나온다.

```swift
func sayHello(_ name: String, _ times: Int = 3) -> String {
    var result: String = ""
    
    for _ in 0..<times {
        result += "Hello \(name)!" + " "
    }
    
    return result
}
// => 오류 코드
```
* 기본값이 없는 매개변수, 중요한 매개변수를 앞쪽에 배치

```swift

/* print 함수 */
/* print 함수
public func print(_ items: Any..., separator: String = default, terminator: String = default)
 items 전달인자만 넘겨도 되는 이유가 매개변수 기본값이 정의 되어있기 때문
 separator는 여러 item이 들어올 경우 구분을 해주는 표식을 넣는 부분임. 기본값은 " "로 되어있다.
 terminator는 출력 마지막에 들어가는 부분. "\n"가 기본값이다.
 */

var name: String = "babamba"
var age: String = "99"

print(name, age, name, age, separator: "/", terminator: "")
print("***")
/* 기본값인 \n에서 ""로 바꾼결과  babamba/99/babamba/99*** 과 같은 형태로 출력된다.
 */

```

###가변 매개변수와 입출력 매개변수

* 매개변수로 몇 개의 값이 들어올지 모를때, 가변 매개변수를 사용
* 0개 이상의 값을 받아올 수 있다.

```swift

func sayHelloToFriends(me: String, friends names: String...) -> String {
    var result: String = ""
    
    for friendName in names {
        result += "Hello \(friendName)!" + " "
    }
    result += "I'm \(me)"
    
    return result
}

print(sayHelloToFriends(me: "babamba", friends: "Jhone", "Jane", "Hong"))
//Hello Jhone! Hello Jane! Hello Hong! I'm babamba

print(sayHelloToFriends(me: "babamba"))
//I'm babamba

```

* 값이 아닌 참조로서 전달하려할 때 입출력 매개변수 사용
* 함수형 패러다임에서는 지양 -> 함수 외부 값에 어떤 영향을 줄지 모르기 때문에

전달순서

1. 함수를 호출할 때, 전달인자의 값을 복사
2. 해당 전달인자의 값을 변경하면 1에서 복사된 것을 함수 내부에서 변경
3. 함수를 반환하는 시점에 2에서 변경된 원래의 매개변수에 할당


-> 연산 프로퍼티 또는 감시자를 갖는 프로퍼티가 입출력 매개변수로 전달 -> 함수호출시점에 프로퍼티 접근자 호출 -> 함수 반환시점에 프로퍼티 설정자 호출

* 연산프로퍼티 : 실제 값을 저장하지 않고, 특정 상태에 따른 값을 연산하기 위한 프로퍼티.
 * 사용이유 
  * 하나의 프로퍼티에 접근자, 설정자 모두 모여있고, 해당 프로퍼티가 어떤 역할을 하는지 명확하게 표현가능

* 프로퍼티감시자 : 사용하면 프로퍼티 값의 변경에 따라 적절한 액션을 취할 수 있다.
 * 프로퍼티 값이 새로 할당될 때마다 호출, 변경되는 값이 현재 값과 같더라도 호출
 * 프로퍼티 재정의를 통해 상속된 저장 연산 프로퍼티에 적용가능
 * 상속 되지 않은 연산 프로퍼티에는 사용 불가


* 참조 표현 : inout 매개변수 전달될 변수 또는 상수 앞에 앰퍼샌드(&) 붙여 참조 표현

```swift
var number: [Int] = [1,2,3]

func nonReferenceParameter(_ arr: [Int]) {
    var copiedArr: [Int] = arr
    copiedArr[1] = 1
}

func referenceParameter(_ arr: inout [Int]) {
    arr[1] = 1
}

nonReferenceParameter(number)
print(number)
dump(number[1])

referenceParameter(&number)
print(number)
dump(number[1])

class Person {
    var height: Float = 0.0
    var weight: Float = 0.0
}

var babamba: Person = Person()

func reference(_ person: inout Person) {
    person.height = 130
    print(babamba.height)
    person = Person()
}

reference(&babamba)
babamba.height

```

###7.2.3 반환타입

생략

###7.2.4 데이터 타입으로서의 함수

* 함수는 일급 객체 이므로 데이터 타입으로 사용될 수 있다.
* 함수는 매개변수 타입과 반환타입으로 사용될 수 있다.

```swift
typealias CalculateTwoInt =  (Int, Int) -> Int

func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}

func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}

var mathFunction: CalculateTwoInt = addTwoInts

print(mathFunction(2,5))

mathFunction = multiplyTwoInts

print(mathFunction(2,5))

func printMathResult(_ mathFuntions: CalculateTwoInt, _ a: Int, _ b: Int) {
    return print("\(mathFuntions(a, b))")
}

printMathResult(addTwoInts, 3, 5)

func chooseFunction(_ toAdd: Bool) -> CalculateTwoInt {
    return toAdd ? addTwoInts(_:_:) : multiplyTwoInts(_:_:)
}

printMathResult(chooseFunction(false), 3, 5)


```

* 즉, 함수를 전달인자로 받을 수도 있고, 반환 값으로 돌려줄 수 있다.


##7.3 중첩함수

* 함수 안에 함수를 넣을 수 있다는 의미
* 특별한 위치에 속해 있지 않는 한 모두 전역함수

```swift
typealias MoveFunc = (Int) -> Int

func goRight (_ currentPosition: Int) -> Int {
    return currentPosition + 1
}

func goLeft (_ currentPosition: Int) -> Int {
    return currentPosition - 1
}

func functionForMove (_ shouldLeftMove: Bool) -> MoveFunc {
    
    return shouldLeftMove ? goLeft : goRight
    
}

var position: Int = 3

let moveToZero: MoveFunc = functionForMove(position > 0)

while position != 0 {
    print("\(position)")
    position = moveToZero(position)
}

/* 중첩함수 */
func nestedFunctionForMove (_ shouldLeftMove: Bool) -> MoveFunc {
    
    func goRight (_ currentPosition: Int) -> Int {
        return currentPosition + 1
    }
    
    func goLeft (_ currentPosition: Int) -> Int {
        return currentPosition - 1
    }
    
    return shouldLeftMove ? goLeft(_:) : goRight(_:)
}

var positionForNested: Int = -4

let moveToZeroUseNestedFunc: MoveFunc = nestedFunctionForMove(positionForNested > 0)

while positionForNested != 0 {
    print("\(positionForNested)")
    positionForNested = moveToZeroUseNestedFunc(positionForNested)
}

```

##7.4 종료되지 않은 함수

* 끝나지 않는 함수 -> 비반환 함수(메서드)
* 정상적으로 끝날 수 없는 함수



ㄸ