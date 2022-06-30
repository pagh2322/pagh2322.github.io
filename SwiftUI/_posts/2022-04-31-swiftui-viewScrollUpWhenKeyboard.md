---
title: 키보드 띄울 시 다른 View가 안 따라오게 하기
description: TextField와 같은 곳을 터치하여 키보드가 띄어져도 다른 View들은 원래 위치에 있게 하기.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- Keyboard
---

# 문제점
해당 View는 `NavigationView`로 감싸진 `VStack`안에 `ScrollView`와 **"모두 삭제"**라고 적혀진 `Button`으로 구성되어 있다.

`searchable`를 통해 검색 기능을 구현하려고 하는데 SearchBar를 터치하면 아래 Button이 같이 따라 올라온다.

![](/images/swiftui/viewScrollUpWhenKeyboard/keyboardProblem.gif)

# 해결 방법
제일 외부에 있는 `NavigationView`에다가 `.ignoresSafeArea`를 추가해준다.

```swift
NavigationView {
    VStack {
        // ...
    }
}
.ignoresSafeArea(.keyboard, edges: .bottom)
```

![](/images/swiftui/viewScrollUpWhenKeyboard/keyboardSolve.gif)

# 참고 링크
[iOS 14 SwiftUI Keyboard lifts view automatically](https://stackoverflow.com/questions/63958912/ios-14-swiftui-keyboard-lifts-view-automatically)