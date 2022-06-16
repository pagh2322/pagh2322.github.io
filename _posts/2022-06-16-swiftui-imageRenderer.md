---
title: ImageRenderer
description: SwiftUI의 뷰를 사진으로 저장.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- ImageRenderer
- Image
---

# ImageRenderer
이번 WWDC2022에서 나온 새로운 개념이다. SwiftUI로 구현한 View를 사진으로 저장할 때 사용이 되는 듯 하다.

# 코드
```swift
struct ContentView: View {
    var body: some View {
        VStack {
            saveView

            Button("클릭하여 위 뷰를 사진으로 저장") {
                guard let image = ImageRenderer(content: saveView).uiImage else {
                    return
                }

                UIImageWriteToSavedPhotosAlbum(image, nil, nil, nil)
            }
        }
    }

    var saveView: some View {
        VStack {
            Text("Text")
        }
        .padding()
        .background(Color.red)
    }
}
```

# 참고 링크
[SwiftUI ImageRenderer: Convert View to Image | iOS 16 Tutorial](https://www.youtube.com/watch?v=nQNnHOeGmU4)