---
title: UIViewRepresentable
description: SwiftUI에서 UIKit을 사용.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- UIKit
---

# UIViewRepresentable
> A wrapper for a UIKit view that you use to integrate that view into your SwiftUI view hierarchy.

아직까지 **SwiftUI**로 모든 기능을 구현하기엔 어려움이 있다. 저번에 했던 `Alert`창에 `TextField`를 넣는 것만 해도 위에서 말한 **UIViewRepresentable**를 통해 만들 수 밖에 없다.

**UIViewRepresentable**를 참조하게 되면 2개의 함수를 추가해야 한다.

## makeUIView(context:) -> some UIView
UIView를 생성하는 메소드. SwiftUI의 View의 라이프 사이클 중 **한 번** 호출된다. View의 프로퍼티들을 지정할 수 있다.

래핑하려는 UIView를 여기서 생성하여 리턴하면, UIViewRepresentable로 래핑되어 SwiftUI의 View가 된다.

## updateUIView(_:context:)
View의 정보를 업데이트하는 메소드. SwiftUI의 View의 state가 변경이 되면 호출된다.

`Binding`을 통해 SwiftUI에서 변경된 상태를 가져와 변화를 반영할 수 있다.

# 사용법 (UIActivityIndicator)
```swift
struct ActivityIndicator: UIViewRepresentable {
    @Binding var isAnimating: Bool

    func makeUIView(context: Context) -> UIActivityIndicatorView {
        let view = UIActivityIndicatorView()
        view.style = .large

        return view
    }

    func updateUIView(_ uiView: UIViewType, context: Context) {
        if self.isAnimating {
            uiView.startAnimating()
        } else {
            uiView.stopAnimating()
        }
    }
}

struct ContentView: View {
    @State var isAnimating = false

    var body: some View {
        VStack {
            ActivityIndicator(isAnimating: self.$isAnimating)
        }
    }
}
```

# 참고 링크
[SwiftUI에서 Swift UIKit 사용하는것에 대해 - UIRepresentable](https://ally10.tistory.com/43)