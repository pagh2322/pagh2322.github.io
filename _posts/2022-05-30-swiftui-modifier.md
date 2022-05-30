---
title: Custom Modifier
description: SwiftUI에서 Modifier추가하기.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- Modifier
---

# ViewModifier
여러 View에서 공용으로 사용하는 modifier를 하나로 만들어 사용하고 싶을 경우가 있다.

커스텀하게 View를 만드는 것처럼, modifier 역시 커스텀하게 만들어 적용할 수 있다.

# 예제 코드
```swift
struct CustomModifiers: ViewModifier {
    var size: CGFloat

    init(size: CGFloat) {
        self.size = size
    }

    func body(content: Content) -> some View {
        content
            .font(Font.system(size: size).weight(.semibold))
            .foregroundColor(.blue)
    }
}

struct ContentView: View {
    var body: some View {
        Image(system: "star.fill")
            .modifier(CustomModifier(size: 50))
    }
}
```