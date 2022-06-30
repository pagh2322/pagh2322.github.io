---
title: SwiftUI에서 뷰의 크기를 계산
description: SwiftUI에서 뷰를 계산하여 가져오기.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- GeometryReader
---

# 뷰의 크기를 가져온다
SwiftUI에서 뷰의 크기를 **동적**으로 계산하여 가져와야 하는 경우가 있다. 

검색한 결과 `UIKit`에선 관련 프로퍼티가 존재하여 쉽게 알 수 있지만, SwiftUI에선 아직 존재하기 않기에 당시 진행함에 있어서 약간의 어려움이 있었다.

# 어떻게
결론적으로 GeometryReader를 해당 뷰에 `background`수식어에 집어넣어 계산하는 방법이 있다.

# 기초 설명
## GeometryReader
SwiftUI에서 공간적인 정보를 가져오려면 `GeometryReader`를 사용할 수 밖에 없다. 

GeometryReader는 가능한 공간만큼 최대한 감싸고 `GeometryProxy`인스턴스를 반환하는 뷰이다. 프록시 인스턴스를 통해 우리는 해당 뷰 혹은 컨테이너의 크기와 좌표 공간에 대한 정보를 알 수 있다.

## overlay와 background
SwiftUI에서 overlay와 background는 **뷰의 앞 또는 뒤**에 새로운 뷰를 배치를 시킨다고 볼 수 있다. 이 때, 새로운 뷰의 크기는 **해당 뷰의 크기**만큼 설정이 된다.

## PreferenceKey
SwiftUI에서 자식 뷰가 부모 뷰에게 정보를 전달할 때 사용한다. `PreferenceKey`은 프로토콜이며 하나의 **타입 변수**(defaultValue)와 하나의 **타입 메소드**(reduce)를 가지고 있다.

### reduce
SizePreferenceKey를 사용하는 자식 뷰들을 순회하면서 부모 뷰가 **접근할 수 있는 값**을 만들기 위해, SizePreferenceKey를 사용하는 자식 뷰의 값들을 취합하는 역할을 한다.

```swift
struct SizePreferenceKey: PreferenceKey {
    static var defaultValue: CGSize = .zero
    static func reduce(value: inout CGSize, nextValue: () -> CGSize) {}
}
```

# onPreferenceChange(_:perform:)
관찰할 `PreferenceKey`를 전달하고, 클로저를 전달하여 해당 키 값이 변경되면 실행이 된다.

# 코드
```swift
var body: some View {
    Text("Test")
    .background(
        GeometryReader { proxy in
            Color.clear
            .preference(key: SizePreferenceKey.self, value: proxy.size) // Text의 크기가 트리의 계층 구조안에 들어간다
        }
    )
    .onPreferenceChange(SizePreferenceKey.self) { size in
        print("new size is \(size)")
    }
}
```

## extension
```swift
extension View {
    func readSize(onChange: @escaping (CGSize) -> Void) -> some View {
        background(
            GeometryReader { proxy in
                Color.clear
                .preference(key: SizePreferenceKey.self, value: proxy.size)
            }
        )
        .onPreferenceChange(SizePreferenceKey.self, perform: onChange)
    }
}

// ...
var body: some View {
    Text("Test")
    .readSize { size in
        print("new size is \(size)")
    }
}
```


# 참고 링크
[How to read a view size in SwiftUI](https://www.fivestars.blog/articles/swiftui-share-layout-information/)

[SwiftUI under the hood](https://protocorn93.github.io/tags/PreferenceKey/)