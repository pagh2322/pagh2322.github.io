---
title: GroupBox
description: Label요소를 그룹화하여 스타일링 해주는 View.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- GroupBox
---

# GroupBox
> A stylized view, with an optional label, that visually collects a logical grouping of content.

`GroupBox`는 `Label`요소를 그룹화하여 스타일링 해주는 View이다.

# 예제
제목을 나타내는 `label`와 내용을 나타내는`content` 2가지로 구성이 되어있다.

```swift
GroupBox(
    label: Label("swiftUI", systemImage: "heart.fill")
        .foregroundColor(.red)
) {
    Text("Welcome to swiftUI world")
}
```

# GroupBoxStyle
스타일을 적용시켜주는 프로토콜이다.

# 실제 앱 예시
아이폰 기본 전화 앱을 보면 나온다.

![](/images/swiftui/groupbox/phone.png)

# 참고 링크
[SwiftUI GroupBox, Menu](https://daesiker.tistory.com/57)

[SwiftUI Form / Group / GroupBox / Section](https://zeddios.tistory.com/1178)