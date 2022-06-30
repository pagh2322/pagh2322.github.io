---
title: SwiftUI Tutorials - Building Lists and Navigation
description: 애플 SwiftUI 공식 튜토리얼를 정리하였습니다.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- Apple Tutorials
---

# SwiftUI Tutorials
## 시작하기 전에
이 게시글은 애플 SwiftUI 공식 튜토리얼를 정리하였습니다. 본인이 이해가 안 가거나 기억하고 싶은 부분들을 위주로 정리하였기에, 자세한 내용을 알고 싶으시다면 아래 링크로 이동해주세요!

### 링크
[애플 SwiftUI 공식 튜토리얼](https://developer.apple.com/tutorials/swiftui)

### 다른 방법
Xcode를 키신 후, `command` + `shift` + `0`을 누르시면 애플 공식 도움창을 통해 보실 수 있다.

# Building Lists and Navigation
중요한 파트라고 생각한다. 앱에서 화면 전환은 거의 필수이기 때문.

## Create a Landmark Model
### Codable
튜토리얼에선 아래와 같이 적혀있다.

> Adding Codable conformance makes it easier to move data between the structure and a data file. You’ll rely on the Decodable component of the Codable protocol later in this section to read data from file.

공식 문서에선 아래와 같이 말한다.

> A type that can convert itself into and out of an external representation.

추측하기론, 다른 파일(json)와 구조체로 전환하는 과정에서, `Codable`프로토콜을 채택해야 가능하다고 생각된다.

### 연산 프로퍼티
[참고하면 좋은 링크](https://zeddios.tistory.com/245)

### fetch JSON data

```swift
func load<T: Decodable>(_ filename: String) -> T {
    let data: Data

    guard let file = Bundle.main.url(forResource: filename, withExtension: nil)
    else {
        fatalError("Couldn't find \(filename) in main bundle.")
    }

    do {
        data = try Data(contentsOf: file)
    } catch {
        fatalError("Couldn't load \(filename) from main bundle:\n\(error)")
    }

    do {
        let decoder = JSONDecoder()
        return try decoder.decode(T.self, from: data)
    } catch {
        fatalError("Couldn't parse \(filename) as \(T.self):\n\(error)")
    }
}
```

