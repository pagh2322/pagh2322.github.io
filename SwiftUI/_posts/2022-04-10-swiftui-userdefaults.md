---
title: UserDefaults
description: 사용자 설정을 저장 할 수 있는 UserDefaults.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- UserDefaults
---

# UserDefaults
> An interface to the user’s defaults database, where you store key-value pairs persistently across launches of your app.

안드로이드의 `SharedPreferences`와 비슷하며, 해당 키에 본인이 원하는 값을 저장 및 불러오기를 할 수 있다.

# 사용하기
## 값 불러오기
```swift
UserDefaults.standard.integer(forKey: "id") // Int 불러오기
UserDefaults.standard.array(forKey: "recentSearch") as? [String] // [String] 불러오기
```
## 값 저장하기

```swift
var id = 20
UserDefaults.standard.set(id, forKey: "id") // Int 저장
var recentSearch = ["ONE", "TWO"]
UserDefaults.standard.set(recentSearch, forKey: "recentSearch") // [String] 저장
```

## 구조체 배열 저장 및 불러오기

```swift
struct Person: Codable {
    let name: String
    let age: String
}

let key = "person"

func save(_ person: [Person]) {
    let data = person.map { try? JSONEncoder().encode($0) }
    UserDefaults.standard.set(data, forKey: key)
}

func load() -> [Person] {
    guard let encodedData = UserDefaults.standard.array(forKey: key) as? [Person] else {
        return []
    }

    return encodedData.map { try! JSONDecoder().decode(Person.self, from: $0) }
}
```

# 참고 링크
[SwiftUI: UserDefaults 정보 수집 및 저장](https://seons-dev.tistory.com/31)

[SwiftUI: Storing an Array in UserDefaults](https://stackoverflow.com/questions/64295407/swiftui-storing-an-array-in-userdefaults)

[Github - save array of struct to UserDefaults](https://gist.github.com/enomoto/629a85bd4e82902057c0b614602a71b3)