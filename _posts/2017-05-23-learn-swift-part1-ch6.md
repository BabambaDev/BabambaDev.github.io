---
layout: post
title:  "Part1 Ch6 흐름 제어"
date: 2017-05-23 10:00
categories: Swift
tags:  Swift3 
author: Babamba
---

* content
{:toc}

스위프트에서 제공하는 흐름제어에 대한 야곰님 책 요약입니다~!

---

## Ch6 흐름 제어


* 흐름제어 구문에서 소괄호 (**()**) 생략 가능. 하지만 중괄호 (**{}**) 생략 불가.

## 6.1 조건문

### 6.1.1 if구문

* 스위프트의 if구문은 조건의 값이 꼭 Bool 타입이어야 한다.

```swift

let first: Int = 5
let second: Int = 7
var biggerValue: Int = 0

if first > second { // 소괄호 생략 가능
    biggerValue = first
} else if (first == second) {
    biggerValue = second
} else if first <  second {
    biggerValue = second
} else if first == 5 {
    // 조건이 충족되더라고 이미 first == second라는 조건이 충족되어 위에서 실행되었기 때문에 실행 않됨.
    biggerValue = 100
}

/*
 마지막 else는 생략 가능
 */

print(biggerValue) // 7

```

### 6.1.2 switch구문

### 특징

||특징|
|:--|:--|
|소괄호(**()**) | 생략 가능 |
| break |선택 사항. case 내부 코드 모두 실행하면 break 없이도 switch 구문 종료됨.|
| fallthrough | case 연속 실행시 사용. |
|switch구문 조건| 다양한 값 가능. 단, case에 들어갈 비굑값은 입력값과 데이터 타입이 같아야 한다. |
|default|비교될 값이 명확히 한정적인 값이 아닐 때 꼭 작성|
|case|범위 연산자 사용가능. where절 사용 조건 확장 가능.|


```swift

let integerValue: Int = 10

switch integerValue {
case 0:
    print("Value == zero")
case 1...10: // 범위 연산자 사용가능
    print("Value == 1~10")
    fallthrough // switch문 탈출 않함. 아래 case로 넘어감.
case Int.min..<0, 101..<Int.max: //
    print("Value < 0 or Vlaue >100")
    break // 선택사항
default: // 한정된 범위가 명확하지 않다면 default 필수!
    print("10 < Value <= 100")

/*
Value == 1~10
Value < 0 or Vlaue >100
*/

```
### 범위 연산자는 부동 소수 타입에도 사용가능!

```swift

let doubleValue: Double = 3.0

switch doubleValue {
case 0:
    print("Value == zero")
//    fallthrough
case 1.5...10.5:
    print("1.5 <= Value <= 10.5")
//    fallthrough
default:
    print("Value == \(doubleValue)")
}

// 1.5 <= Value <= 10.5

```

### 입력값으로 다양한 타입 가능. (예: 문자, 문자열, 열거형, 튜플, 범위, 패턴이 적용된 타입 등)

* 문자열 사용
 * 여러 항목을 case로 지정 가능
 * case 다음에는 꼭 실행 가능 코드가 위치해야함.

```swift

let stringValue: String = "Joker"

switch stringValue {
case "babamba":
    print("He is babamba")
case "Jay":
    print("He is Jay")
case "Jenny", "Joker", "Nova":
    print("He or She is \(stringValue)")
default:
    print("\(stringValue) said 'I don't know who you are'")
}

// He or She is Joker

```

* 튜플 사용

```switch

typealias NameAge = (name: String, age: Int)

let tupleValue: NameAge = ("babamba", 99)

switch tupleValue {
case ("babamba", 99):
    print("correct")
default:
    print("incorrect")
}

// correct

```

* 와일드카드 식별자
 * 값을 무시하고 매치하고자 할 때 사용.
 
```swift

for _ in 1...3 {
    // Do something three times.
}

// _ : 와일드카드 식별자

```

```swift

switch tupleValue {
case ("babamba", 50):
    print("둘 다 맞았습니다.")
case ("babamba", _):
    print("이름만 맞았습니다. 나이는 \(tupleValue.age)입니다.")
case (_, 99):
    print("나이만 맞았습니다. 이름은 \(tupleValue.name)입니다.")
default:
    print("둘 다 틀렸습니다.")
}

//이름만 맞았습니다. 나이는 99입니다.

```
* 값 바인딩 사용
 * 와일드카드 식별자 사용시, 무시된 값을 직접 가져와야하는 불편함을 해결할 수 있음.

```swift

switch tupleValue {
case ("babamba", 99):
    print("정확히 맞췄음. correct")
case ("babamba", var age):
    print("이름만 맞았습니다. 나이는 \(age)입니다.")
case (var name, 99):
    print("나이만 맞았습니다. 이름은 \(name)입니다.")
default:
    print("incorrect")
}

//정확히 맞췄음. correct

```

* where 사용
 * case 조건 확장 

```swift

let 직급: String = "사원"
let 연차: Int = 1
let 인턴인가: Bool = false

switch 직급 {
case "사원" where 인턴인가 == true:
    print("인턴입니다.")
case "사원" where 인턴인가 == false && 연차 < 2:
    print("신입사원입니다.")
case "사원" where 연차 > 5:
    print("연식이 좀 된 사원입니다.")
case "사원입니다.":
    print("사원입니다.")
case "대리":
    print("대리입니다.")
default:
    print("사장입니까?")
}
//신입사원입니다.


/* 구조체 안의 값만 사용 */
struct PositionYears {
    let position: String, years: Int
}

let babambaPostion = PositionYears(position: "사원", years: 30)

switch babambaPostion.position {
case "사원" where babambaPostion.years < 3:
    print("3년 미만 사원입니다.")
case "사원" where babambaPostion.years >= 3 && babambaPostion.years < 30:
    print("3년 이상 30년 미만 사원입니다.")
case "사원" where babambaPostion.years >= 30:
    print("30년 이상 근속자 입니다.")
default:
    print("unknown")
}
// 30년 이상 근속자 입니다.

```

* 열거형 사용

```swift

enum School {
    case primary, elementary, middle, high, college, university, graduate
}

let finalEducationLevel: School = .primary

switch finalEducationLevel {
case .primary:
    print("유치원")
case .elementary:
    print("초졸")
case .middle:
    print("중졸")
case .high:
    print("고졸")
case .college:
    print("전문대졸")
case .university:
    print("4년제 졸")
default:
    print("학력없음")
}

//유치원

```

## 6.2 반복문

* C스타일 for구문 삭제
* do-while문 -> repeat-while문으로 대체

### 6.2.1 for-in 구문

```swift

for i in 1...2 {
    print(i)
}


for i in 0...5 {
    if i % 2 == 0 {
        print(i)
    }
}


let helloSwift: String = "Hello Swift!"

for char in helloSwift.characters {
    print(char)
}


/* 시퀀스에 해당하는 값이 필요없고 반복만 필요할 때 */
var resultNum: Int = 1

for _ in 1...3 {
    resultNum *= 10
    print(resultNum)
}

```

### 기본 콜렉션 타입을 이용하여 사용가능
* 딕셔너리는 넘겨 받는 값의 타입이 튜플로 지정되어 넘어옴

```swift
/* for-in 기본 콜렉션 타입 */
let friends: [String: Int] = ["Jay":35, "Joe":29, "Jenny":31]

for tuple in friends {
    print("딕셔너리는 튜플형태! \(tuple)")
}

let addressDic: [String: String] = ["도":"충청북도","시군구":"청주시","동읍면":"율량동"]

for (key, value) in addressDic {
    print(key, value)
}

let regionNum: Set<String> = ["02","031","033","041","042","043"];

for number in regionNum {
    print(number)
}

```

### 6.2.2 while 구문

* 특정조건이 성립하는 한 블록 내부 코드를 반복실행
* 조건은 Bool 타입
* continue, break 사용가능

```swift

var i: [Int] = [1,2,3,4]

while i.isEmpty == false {
    print("\(i.removeFirst())")
    // removeFirst()는 요소를 삭제함과 동시에 삭제된 요소 반환
}

```

### 6.2.3 repeat-while문

* do-while문과 비슷
* repeat 블록의 코드를 1회 실행 후, while다음 조건 성립시 블록 내부코드 반복

```swift
var names: [String] = ["John","Jenny","Koe","Joe","Cal"]

repeat {
    print(names.removeFirst())
} while names.isEmpty == false


```

## 6.3 구문 이름표

* 반복문 앞에 이름과 함께 콜론을 붙여 구문 이름을 지정

```swift

var numbers: [Int] = [3, 2342, 6, 3252]

numbersLoop: for num in numbers {
    if num > 5 || num < 1 {
        continue numbersLoop
    }

    var count: Int = 0
    
    printLoop: while true {
        print(num)
        count += 1
        
        if count ==  num {
            break printLoop
        }
    }
    
    removeLoop: while true {
        if numbers.first != num {
            break removeLoop
        }
        numbers.removeFirst()
    }

}

```