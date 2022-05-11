---
title: A Swift Tour - Part 2
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

# Objects and Classes
## 생성자와 소멸자
모든 프로퍼티는 인스턴스 생성 시 **무조건** 값이 할당되어야 한다.
`self`키워드를 통해 클래스 패러미터와 초기화 함수의 패러미터를 구분한다.

```swift
class Shape {
    var edge: Int

    init(edge: Int) {
        self.edge = edge
    }
}
```

소멸자의 키워드는 `deinit`

## Override
오버라이드 된 메소드는 무조건 `override`키워드를 추가한다.(없으면 컴파일 에러가 난다.)

## 연산 프로퍼티
연산 프로퍼티는 값을 저장하는 공간 없이, 다른 저장 프로퍼티(일반적인 프로퍼티) 값을 받아와서 연산 후 반환하는 역할을 한다(getter와 setter)

**setter**에서 변수에 새로 대입할 값은 `newValue`라는 암묵적 이름이 있으며, 반약 이름을 수정하고 싶으면 `set`키워드 뒤에 괄호를 생성하여 안에 원하는 이름을 적는다.

```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }

    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }

    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter)
// Prints "9.3"
triangle.perimeter = 9.9
print(triangle.sideLength)
// Prints "3.3000000000000003"
```

## willSet와 didSet
연산 프로퍼티가 필요 없지만, 새로운 값을 할당하기 전 어떠한 코드가 실행되길 원한다면, `willSet` 혹은 `didSet`키워드를 사용한다.

```swift
class TriangleAndSquare {
    var triangle: EquilateralTriangle {
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
    var square: Square {
        willSet {
            triangle.sideLength = newValue.sideLength
        }
    }
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
}
var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
print(triangleAndSquare.square.sideLength)
// Prints "10.0"
print(triangleAndSquare.triangle.sideLength)
// Prints "10.0"
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
print(triangleAndSquare.triangle.sideLength)
// Prints "50.0"
```

## optional value
`옵셔널변수?.프로퍼티`방식을 통해 언래핑을 할 수 있다.

```swift
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength
```

# Enumerations and Structures
## 열거형
클래스와 같이 열거형 또한 메소드를 가질 수 있다.

```swift
enum Rank: Int {
    case ace = 1
    case two, three, four, five, six, seven, eight, nine, ten
    case jack, queen, king

    func simpleDescription() -> String {
        switch self {
        case .ace:
            return "ace"
        case .jack:
            return "jack"
        case .queen:
            return "queen"
        case .king:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
}
let ace = Rank.ace
let aceRawValue = ace.rawValue
```

## 원시값
기본적으로 Swift는 원시값을 0부터 시작해서 할당하며, 만약 아까와 같이 사용자가 임의로 1로 설정한다면, 다음 값들은 순차적으로 할당이 된다.

만약 원시값에 대해 큰 의미가 없다면, 굳이 사용자가 임의로 설정할 필요가 없다

```swift
if let convertedRank = Rank(rawValue: 3) {
    let threeDescription = convertedRank.simpleDescription()
}
```

만약 원시값에 대해 의미를 부여한다면, `case 값([타입])`형태로 한다.

```swift
enum ServerResponse {
    case result(String, String)
    case failure(String)
}

let success = ServerResponse.result("6:00 am", "8:09 pm")
let failure = ServerResponse.failure("Out of cheese.")

switch success {
case let .result(sunrise, sunset):
    print("Sunrise is at \(sunrise) and sunset is at \(sunset).")
case let .failure(message):
    print("Failure...  \(message)")
}
// Prints "Sunrise is at 6:00 am and sunset is at 8:09 pm."
```


## switch문에서
`switch`뒤에 오는 변수가 열거형일 경우, 자동으로 `case`에서는 무조건 해당 열거형에 대해 다루기때문에, `.값`형태로 한다.(코드의 길이가 확실히 줄어진다.)

```swift
enum Suit {
    case spades, hearts, diamonds, clubs

    func simpleDescription() -> String {
        switch self {
        case .spades:
            return "spades"
        case .hearts:
            return "hearts"
        case .diamonds:
            return "diamonds"
        case .clubs:
            return "clubs"
        }
    }
}
let hearts = Suit.hearts
let heartsDescription = hearts.simpleDescription()
```


# Protocols and Extensions
## 프로토콜
클래스는 프로토콜을 채택하여 구현한다.(Java의 인터페이스와 유사하다.)

`mutating`키워드를 통해 해당 메소드는 구조체를 수정할 수 있다고 명시한다.

> 클래스는 필요없는 이유: 클래스에 있는 메소드는 언제나 해당 클래스를 수정할 수 있다. 여기서 수정의 기준이 뭔지 나중에 알아보는걸로. 혹시 복사와 참조의 차이와 연관이 있는 건가?

```swift
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}

class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += "  Now 100% adjusted."
    }
}
var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStructure: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}
var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription
```

## 익스텐션
익스텐션을 통해 기존에 존재한 타입에 기능을 추가할 수 있다.(예를 들어, 새로운 메소드나 연산 프로퍼티 등..)

```swift
extension Int: ExampleProtocol {
    var simpleDescription: String {
        return "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
}
print(7.simpleDescription)
// Prints "The number 7"
```

아래처럼, 프로토콜 자체를 타입으로 사용할 수 있는데, 이 경우 해당 프로토콜에 선언된 프로퍼터 및 메소드만 접근이 가능하다. 예를 들어, a는 ExampleProtocol를 채택한 클래스이지만, protocolValue는 ExampleProtocol이므로 anotherProperty와 같은 클래스의 프로퍼티를 접근할 수 없다.

```swift
let protocolValue: ExampleProtocol = a
print(protocolValue.simpleDescription)
// Prints "A very simple class.  Now 100% adjusted."
// print(protocolValue.anotherProperty)  // Uncomment to see the error
```


# Error Handling
## throw키워드
`throw`키워드를 통해 에러를 던질 수 있고, `throws`키워드를 통해 해당 함수가 에러를 던질 수 있다 명시할 수 있다.

```swift
func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never Has Toner" {
        throw PrinterError.noToner
    }
    return "Job sent"
}
```

## 에러 처리
### do-catch문
`do`블록 안에서, 에러를 던질 수 있는 코드의 앞에 `try`키워드를 적어주고,
`catch`블록 안에서, 자동으로 `error`라고 해당 에러의 이름이 설정된다.

```swift
do {
    let printerResponse = try send(job: 1040, toPrinter: "Bi Sheng")
    print(printerResponse)
} catch {
    print(error)
}
// Prints "Job sent"
```

여러 `catch`블록을 통해 에러들을 세분화하여 처리할 수 있다.

```swift
do {
    let printerResponse = try send(job: 1440, toPrinter: "Gutenberg")
    print(printerResponse)
} catch PrinterError.onFire {
    print("I'll just put this over here, with the rest of the fire.")
} catch let printerError as PrinterError {
    print("Printer error: \(printerError).")
} catch {
    print(error)
}
// Prints "Job sent"
```

### try?
`try?`키워드로 에러를 던질 수 있는 함수의 결과 값을 옵셔널로 받을 수 있다.
만약 에러를 던지게 된다면 `nil`을 반환한다.(이 방식이 제일 깔끔해서 마음에 든다.)

```swift
var fridgeIsOpen = false
let fridgeContent = ["milk", "eggs", "leftovers"]

func fridgeContains(_ food: String) -> Bool {
    fridgeIsOpen = true
    defer {
        fridgeIsOpen = false
    }

    let result = fridgeContent.contains(food)
    return result
}
fridgeContains("banana")
print(fridgeIsOpen)
// Prints "false"
```


# Generics
다른 언어와 유사하다. `<이름>`형태로 사용한다.

```swift
func makeArray<Item>(repeating item: Item, numberOfTimes: Int) -> [Item] {
    var result: [Item] = []
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    return result
}
makeArray(repeating: "knock", numberOfTimes: 4)
```

당연히 함수와 메소드, 클래스, 열거형 그리고 구조체에서도 사용할 수 있다.

```swift
enum OptionalValue<Wrapped> {
    case none
    case some(Wrapped)
}
var possibleInteger: OptionalValue<Int> = .none
possibleInteger = .some(100)
```

## where
`where`키워드를 함수의 본체 앞(혹은 반환타입 뒤)에 적어 요구사항들을 적는다.

### 요구사항
예를 들어, 해당 타입이 프로토콜을 채택하게 요구하거나, 두개의 타입이 같게 요구하거나, 어떤 클래스가 특정 부모 클래스를
상속받게 요구할 수 있다.

```swift
func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool
    where T.Element: Equatable, T.Element == U.Element
{
    for lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                return true
            }
        }
    }
    return false
}
anyCommonElements([1, 2, 3], [3])
```

`<T: Equatable>`과 `<T> ... where T: Equatable`은 같은 기능을 수행한다.