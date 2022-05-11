---
title: A Swift Tour - Part 1
description: 애플 스위프트 공식 문서를 정리하였습니다.
categories:
- Swift
tags: 
- Swift
- Apple Tutorials
---

# Swift Tour
## 시작하기 전에
이 게시글은 애플 스위프트 공식 문서를 정리하였습니다. 본인이 이해가 안 가거나 기억하고 싶은 부분들을 위주로 정리하였기에, 자세한 내용을 알고 싶으시다면 아래 링크로 이동해주세요!

## 링크
[애플 스위스프 공식 문서](https://docs.swift.org/swift-book/GuidedTour/GuidedTour.html)


# Simple Values
## 상수와 변수의 개념
1. `var`키워드와 `let`키워드는 각각 변수와 상수를 뜻한다.
2. 이름 뒤에 `: 타입`을 추가하여 해당 변수 혹은 상수의 타입을 선언한다.

```swift
var x = 42
let pi: Double = 3.14
```

## 타입 변환
변수는 Swift는 C/C++와 다르게 **암묵적으로** 다른 타입으로 전환할 수 없다.
전환을 해야 한다면, **명시적으로** 전환할 타입의 인스터스를 생성해야한다.

```swift
let laber = "The width is "
let width = 94
let widthLabel = label + String(width)
```

## 문자열 보간법
문자열 안에 변수를 삽입할 경우, `\(변수)`를 문자열 안에 넣는 방식으로 구현한다.

```swift
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```
## 다중 문자열
여러 줄이 포함된 문자열을 생성할 경우, `"""`를 통해 감싸서 생성한다.

```swift
let quotation = """
I said "I have \(apples) apples."
And then I said "I have \(apples + oranges) pieces of fruit."
"""
```

## 배열 및 딕셔너리
둘 다 `[]`를 통해 생성 및 접근할 수 있으며, 마지막 원소 뒤에 `,`가 있어도 된다.

```swift
var shoppingList = ["catfish", "water", "tulips"]
shoppingList[1] = "bottle of water"

var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```

`[타입]`과 `[타입 : 타입]`형식으로 생성할 수 있다.
```swift
let emptyArray: [String] = []
let emptyArray: [String: Float] = [:]

emptyArray = []
emptyArray = [:]
```

# Control Flow
## if문
### if문을 통해 옵셔널 언래핑
`if let value = optionalValue`와 같이 해당 오셔널 변수를 언래핑하여, 만약 `nil`이 아닐 경우 헤당 조건문이 `true`값이 되어 `value`에 `optionalValue`가 할당되어 `if`문 안에 있는 코드를 실행한다. (`nil`일 경우, 해당 조건문은 `false`가 된다.)

```swift
var optionalName: String? = "John"
if let name = optionalName {
    print("Hello, \(name)")
}
```

혹은 옵셔널 변수 뒤에 `?? 기본값`방식을 통해 간단하게 언래핑을 할 수 있다.

```swift
let nickname: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "Hi \(nickname ?? fullName)"
```

## switch문
3번째 케이스는 나중에 다시 알아보겠다.

```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
// Prints "Is it a spicy red pepper?"
```


# Functions and Closures
## 함수
패러미터가 있는 함수를 생성한 뒤에 해당 함수를 호출할 때, 해당 패러미터의 이름을 적어줘야 한다. (이런 방식이 이후에 함수를 사용함에 있어서 패러미터의 이름을 적음으로써 더 쉽게 이해할 수 있다고 생각한다.)

```swift
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet(person: "Bob", day: "Tuesday")
```

혹시나 함수를 호출할때는 다른 이름으로 전달하고 싶으면, 해당 패러미터의 이름 앞에 원하는 이름을 적어주면 된다.

```swift
func greet(_ person: String, on day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet("John", on: "Wednesday")
```

## 튜플 반환
복잡한 값을 반환하고 싶으면 튜플로 반환하여, 해당 결과값은 이름 혹은 인덱스로 접근할 수 있다. (결과값을 statistics에 저장하여 원하는 원소를 접근하였다.)

```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0

    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }

    return (min, max, sum)
}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
print(statistics.sum)
// Prints "120"
print(statistics.2)
// Prints "120"
```

## 함수 중첩
함수 내에 함수를 중첩시켜 구현하는 방법이다. (이 방법을 실제로 개발하면서 쓰는지 궁금하다.)

```swift
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()
```

## 일급 객체
함수는 일급 객체이기에 **기존 변수들**처럼 사용할 수 있다. (즉, Swift는 함수형 프로그래밍 언어다.)
예를 들어, 어떤 함수는 다른 함수를 결과 값으로 반환할 수 있다. 이 경우, 반환값은 `(타입) -> 반환 타입`으로 표시할 수 있다.

```swift
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

또 다른 함수는 패러미터로 함수를 받을 수 있다. (C++에 있는 sort함수도 패러미터로 함수를 전달할 수 있는데, 이것 또한 같은 방식으로 볼 수 있을까?)

```swift
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```

## 클로저
함수는 실질적으로 클로저의 특별한 케이스로 볼 수 있다.

클로저는 이름이 없는 함수, 즉 익명함수로 그 자체가 변수로 볼 수 있다. (고차 함수와 같은 곳에서 자주 쓰는 듯 하다.) `in`키워드로 패러미터와 반환값의 타입을 본체와 분리시킨다.

```swift
numbers.map({ (number: Int) -> Int in
    let result = 3 * number
    return result
})
```

만약 클로저의 타입을 **이미 알고** 있으면, 패러미터의 타입, 반환값의 타입, 또는 둘 다 **생략**할 수 있다. **단일 문** 클로저는 암묵적으로 해당 문의 값을 반환시킨다

```swift
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
```

패러미터는 이름 대신 숫자로 접근할 수 있는데, 이 경우는 짧고 간단한 클로저에서 쓰인다. (안그러면 가독성 측면에서 안 좋을 듯 하다.)
또한, 어떤 함수의 **마지막 패러미터**로 클로저를 전달할 경우, 함수의 **괄호 뒤**에 따로 클로저를 적을 수 있다. 만약 함수가 오로지 **패러미터가 하나만 있고 클로저**로 전달받으면, **괄호 자체를 생략**하고 클로저를 함수 이름 뒤에 바로 적을 수 있다. (SwiftUI에서 View뒤에 바로 {}를 적는 것도 이 때문인가?)
```swift
let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbers)
// Prints "[20, 19, 12, 7]"
```