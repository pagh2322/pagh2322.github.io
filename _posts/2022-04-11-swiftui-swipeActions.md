---
title: .swipeActions
description: List에 있는 각 행이 스와이프할 수 있게 해주는 modifier.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- modifier
---

# swipeAction
iOS15에서 새로 생긴 이 modifier는 `List`에 있는 각 행이 스와이프를 함으로써 삭제와 같은 행동을 할 수 있게 되었다.

# 예제
```swift
List {
    Text("Pepperoni pizza")
        .swipeActions {
            Button("Order") {
                print("Awesome!")
            }
            .tint(.green)
        }

    Text("Pepperoni with pineapple")
        .swipeActions {
            Button("Burn") {
                print("Right on!")
            }
            .tint(.red)
        }
}
```

![](/images/swiftui/swipeAction/swiftui-swipeActions1.gif)

해당 버튼들의 색상은 `.tint`를 통해 변경할 수 있다.

# allowsFullSwipe
기본적으로 반대방향으로 끝까지 스와이프를 해도 작동이 된다. 만약 이 기능을 원하지 않는다면, `swipeActions`에 인자값 `allowsFullSwipe`의 값을 `false`로 할당한다.

```swift
.swipeActions(allowsFullSwipe: false) {
    // Button 혹은 Label 등...
}
```

# edge
만약 버튼들의 위치를 바꾸고 싶으면, `edge`의 값을 변경해준다.

```swift
ForEach(0..<5) {
    Text("\($0)")
    .swipeActions(edge: .leading) {
        Button {
            // do
        } label: {
            Label("plus", systemImage: "plus.circle")
        }
        .tint(.indigo)
    }
    .swipeActions(edge: .trailing) {
        Button {
            // do
        } label: {
            Label("minus", systemImage: "minus.circle")
        }
    }
}
```

SwiftUI에선 `swipeActions`을 사용 시, `Label`의 아이콘만 보여주지만, `VoiceOve`라는 기능을 통해 텍스트도 읽어준다.

# 참고 링크
[How to add custom swipe action buttons to a List row](https://www.hackingwithswift.com/quick-start/swiftui/how-to-add-custom-swipe-action-buttons-to-a-list-row)