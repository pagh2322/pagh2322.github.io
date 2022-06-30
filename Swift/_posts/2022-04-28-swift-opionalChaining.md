---
title: Optional Chaining
description: nil일 수도 있는 프로퍼티나, 메소드 그리고 서브스크립트에 질의를 하는 과정.
categories:
- Swift
tags:
- Swift
- Optional
---

# 강제 언래핑
느낌표를 이용해 강제 언래핑을 할 경우, 언래핑할 값이 없게 되면 런타임 에러가 발생한다.

```swift
class Residence {
    var numberOfRooms = 1
}
class Person {
    var residence: Residence?
}
let john = Person()
let roomCount = john.residence!.numberOfRooms // 런타임 에러 발생
```

물음표를 통해 옵셔널 체이닝을 사용하면 이러한 상황을 방지할 수 있다. 만약 언래핑할 값이 없으면 `nil`를 건네준다.

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("not nil")
} else {
    print("nil")
}
// Print "nil"
```

# 옵셔널 체이닝을 통한 프로퍼티의 접근
아래 코드는 첫 예제 코드를 기준으로, Address()인스턴스를 생성해 john.residence의 address로 할당하는 코드이다.

```swift
class Residence {
    var address: Address?
}
let someAddress = Address()
someAddress.buildingNumber = "29"
someAddress.street = "Acacia Road"
john.residence?.address = someAddress
```

여기서 john.residence?가 `nil`이기 때문에 address의 할당이 실패한다. 즉, **할당받는 왼쪽 항**이 `nil`이면 오른쪽 항이 실행되지 않는다.

# 옵셔널 체이닝을 통한 메소드 호출
함수나 메소드는 리턴 값이 없는 경우 암시적으로 Void라는 값을 갖는다. 그래서 아래 메소드가 옵셔널 체이닝에서 호출되면 반환 값은 Void?가 반환된다.

```swift
class Residence {
    func printNumberOfRooms() {
        print("test")
    }
}

if john.residence?.printNumberOfRooms() != nil {
    print("not nil")
} else {
    print("nil")
}
```

printNumberOfRooms()엔 직접적인 반환 값이 명시되어 있지 않지만, 암시적으로 Void를 반환하고 해당 메소드 호출이 옵셔널 체이닝에서 이루어 지기 때문에 Void?가 반환되서 nil비교를 할 수 있다.