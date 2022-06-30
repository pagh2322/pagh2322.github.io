---
title: ButtonStyle 만들기
description: SwiftUI에서 ButtonStyle을 만들어 보자.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- ButtonStyle
- Button
---

# 앞서서
하나의 앱에서 사용되는 버튼의 스타일들은 일정한 모양을 가지고 있다.

만약 ButtonStyle을 만들어 사용한다면 좀 더 통일화된 UI와 함께 작업 효율에도 긍정적인 영향을 끼칠 것이다.

# 제공되는 스타일들
iOS에서는 3가지의 버튼 스타일이 제공된다.

## DefaultButtonStyle
기본적으로 적용되는 값이다.

## BorderlessButtonStyle
테두리를 그리지 않는다. iOS에서 제공하는 스타일 중에 테두리는 그리는 버튼은 없다.

하지만 macOS에선 BorderedButtonStyle이 존재한다.

## PlainButtonStyle
유휴 상태(IDLE)에서는 버튼의 콘텐츠에 어떠한 시작적 요소도 적용하지 않는다.

# ButtonStyle 만들기 
```swift
struct CustomButtonStyle: ButtonStyle {
    func makeBody(configuration: Self.Configuration) -> somw View {
        configuration.label
        .frame(width: 230, height: 45)
        .foregroundColor(.black)
        .background(.white)
        .cornerRadius(22.0)
    }
}
```

# 참고 링크
[SwiftUI : Button / onTapGesture](https://seons-dev.tistory.com/entry/Button버튼)

[[SwiftUI] 버튼 스타일 (ButtonStyle) 만들어 쉽게 재사용하기](https://onelife2live.tistory.com/43)