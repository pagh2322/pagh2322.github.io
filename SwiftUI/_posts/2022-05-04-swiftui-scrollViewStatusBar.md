---
title: ScrollView에서 스크롤 할때 상태바와 화면 하단 부분을 침범하는 현상 해결
description: NavigationView안에 ScrollView를 스크롤을 하면 상태바까지 올라가거나 아래 하단 부분을 침범한다.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- Error
- ScrollView
- padding
---

# 문제점
아래와 같은 코드로 빌드를 하게 되면, 스크롤을 할때 상태바까지 올라가거나 화면 하단 부분을 침범한다.

```swift
struct ContentView: View {
    var body: some View {
        NavigationView {
            ScrollView(.vertical, showsIndicators: false) {
                ForEach(0..<100) {
                    Text("\($0) : 블로그에 올릴 예제 코드입니다. 가나다라마바사")
                        .padding(.bottom, 10)
                }
            }
            .navigationBarHidden(true)
        }
    }
}
```

![](/images/swiftui/scrollViewStatusBar/scrollViewStatus.gif)

# 해결 방법
`ScrollView`에 `.padding(.vertical, 1)`를 해줌으로써 안전 영역에 침법하지 않게 된다. 여기서 padding의 값은 0보다 크기만 하면 된다.

```swift
ScrollView {
    // ...
}
.padding(.vertical, 1)
```

![](/images/swiftui/scrollViewStatusBar/scrollViewStatusSolve.gif)

# 참고 링크
[iOS SwiftUI: ScrollView ignore top safe area](https://stackoverflow.com/questions/59137040/ios-swiftui-scrollview-ignore-top-safe-area)