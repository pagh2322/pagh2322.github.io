---
title: System Color
description: iOS에서 사용하는 System Color들을 사용한다.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- Color
- UIColor
---

# 문제점
SwiftUI에선 `Color`를 통해 `View`의 배경색을 유여하게 바꿀 수 있다.

어떤 화면에서 `List`를 사용하면 바깥 배경색이 푸른빛을 나타내며, 해당 RGB의 값은 242, 242, 247이다. 본인은 현재 해당 색상을 사용하고 싶고 직접 색상을 지정해서 구현해도 되지만, 해당 색상은 분명히 iOS에서 시스템상으로 사용하고 있으며, 다크 모드에서도 자연스럽게 바뀌는 기능을 구현하기가 꺼려져 `.primary`혹은 `.secondary`와 같은 시스템에서 지원하는 값이 있지 않을 까 생각하였다.

# 색상 이름
![](/images/swiftui/systemColor/sibWU.jpg)

# 코드 구현
`Color(UIColor.이름)`을 통해 iOS에서 지원하는 여러 시스템 색상을 이용할 수 있다.

```swift
View {
    /* 구현 */ 
}
.background(Color(UIColor.label))
```

# 참고 링크
[What are the .primary and .secondary colors in SwiftUI?](https://stackoverflow.com/questions/56466128/what-are-the-primary-and-secondary-colors-in-swiftui)