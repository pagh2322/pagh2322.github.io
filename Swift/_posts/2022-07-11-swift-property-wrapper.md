---
title: Property Wrapper
description: Property Wrapper에 대해 알아보자.
categories:
- Swift
tags:
- Swift
- Property Wrapper
---

# Property Wrapper
공부를 계속 하면서 들었던 단어이다. 하지만 매번 들을 때마다 그냥 넘어가다 오늘 관련 강의 영상을 보게 되어 간단히 정리를 해보았다.

프로퍼티 래퍼는 나도 모르게 계속 사용했던 개념이었다. SwiftUI에서 **@State**, **@Binding**와 같은 것들이 프로퍼티 래퍼이며, 개발 상황에 맞게 적용을 시키면 더욱 효율적인 코드를 작성할 수 있다.

# 왜 필요할까
Zedd님의 블로그에서 코드를 가지고 왔다.

```swift
struct Address {
    private var _town: String = ""

    var town: String {
        get { self._town.uppercased() }
        set { self._town = newValue }
    }

    init(town: String) {
        self.town = town
    }
}

struct Person {
    private var _name: String = ""

    var name: String {
        get { self._name.uppercased() }
        set { self._name = newValue }
    }

    init(name: String) {
        self.name = name
    }
}
```

위 예제 코드를 보면 `town`과 `name`이라는 프로퍼티의 `get`, `set`로직이 **중복**된다.

이렇게 중복된 코드를 사용하지 않고, 하나의 프로퍼터 래퍼를 연결시켜 주면 중복된 코드를 줄일 수 있다.

# 사용법


```swift
@propertyWrapper
struct UserDefaultsHelper<T> {
    let key: String
    let defaultValue: T

    var wrappedValue: T {
        get {
            UserDefaults.standard.object(forKey: key) as? T ?? self.defaultValue
        }
        set {
            UserDefaults.standard.setValue(newValue, forKey: key)
        }
    }
}

struct PlayerSetting {
    @UserDefaultsHelper(key: "name", defaultValue: "홍길동") var name: String

    @UserDefaultsHelper(key: "age", defaultValue: 20) var age: Int
}
```

# 다른 예제(WWDC 19)
아래 역시 get과 set로직이 중복이 된다.

```swift
class UserManager {
    static var usesTouchID: Bool {
        get { return UserDefaults.standard.bool(forKey: "usesTouchID") }
        set { UserDefaults.standard.set(newValue, forKey: "usesTouchID") }
    }
    
    static var myEmail: String? {
        get { return UserDefaults.standard.string(forKey: "myEmail") }
        set { UserDefaults.standard.set(newValue, forKey: "myEmail") }
   }
   
    static var isLoggedIn: Bool {
        get { return UserDefaults.standard.bool(forKey: "isLoggedIn") }
        set { UserDefaults.standard.set(newValue, forKey: "isLoggedIn") }      
    }          
}
```

개선한 코드는 아래와 같다.

```swift
@propertyWrapper
struct UserDefaults<T> {
    let key: String
    let defaultValue: T
    let storage: UserDefaults

    var wrappedValue: T {
        get {
            self.storage.object(forKey: key) as? T ?? self.defaultValue
        }
        set {
            self.storage.set(newValue, forKey: key)
        }
    }

    init(key: String, defaultValue: T, storage: UserDefaults = .standard) {
        self.key = key
        self.defaultValue = defaultValue
        self.storage = storage
    }
}

class UserManager {
    @UserDefault(key: "usesTouchID", defaultValue: false)
    static var usesTouchID: Bool
    
    @UserDefault(key: "myEmail", defaultValue: nil)
    static var myEmail: String?
   
    @UserDefault(key: "isLoggedIn", defaultValue: false)
    static var isLoggedIn: Bool  
}
```

# 참고 링크
[[Mastering Swift] Property Wrapper #1 (Xcode 12, Swift 5.3)](https://www.youtube.com/watch?v=cRWojZb_36I&list=PLziSvys01OeklWvgtiF9tZVPA-awfOzUq&index=1)
[Zedd0202 - Property Wrapper](https://zeddios.tistory.com/1221)