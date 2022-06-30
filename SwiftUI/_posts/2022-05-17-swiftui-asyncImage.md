---
title: AsyncImage
description: 외부 url을 통해 이미지를 불러오자.
categories:
- SwiftUI
tags:
- Swift
- SwiftUI
- iOS
- Image
---

# AsyncImage
외부 url을 통해 이미지를 불러올 수 있는 View이다.

## placeholder
**placeholder 패러미터**로 이미지가 불러오기 전까지 보여질 이미지를 설정한다.

```swift
AsyncImage(url: URL(string: "https://hws.dev/img/bad.png")) { image in
    image
        .resizeable()
        .scaledToFit()
} placeholder: {
    Image(systemName: "photo.circle.fill")
        .resizeable()
        .scaledToFit()
        .frame(maxWidth: 120)
}
.padding(40)
```

## phase
각 단계에 맞춰 이미지를 보여줄 수 있다.

```swift
AsyncImage(url: URL(string: "https://hws.dev/img/bad.png")) { phase in
    if let image = phase.image {
        image
        .resizeable()
        .scaledToFit()
    } else if phase.error != nil {
        Image(systemName: "ant.circle.fill")
        .resizeable()
        .scaledToFit()
        .frame(maxWidth: 120)
    } else {
        Image(systemName: "photo.circle.fill")
        .resizeable()
        .scaledToFit()
        .frame(maxWidth: 120)
    }
}
.padding(40)
```

혹은 `switch 구문`으로 진행할 수 있다.

```swift
AsyncImage(url: URL(string: "https://hws.dev/img/bad.png")) { phase in
    switch phase {
    case .success(let image):
        image
        .resizeable()
        .scaledToFit()
        .transition(.scale)
    case .failure(_):
        Image(systemName: "ant.circle.fill")
        .resizeable()
        .scaledToFit()
        .frame(maxWidth: 120)
    case .empty:
        Image(systemName: "photo.circle.fill")
        .resizeable()
        .scaledToFit()
        .frame(maxWidth: 120)
    @unknown default:
        ProgressView()
    }
}
.padding(40)
```