---
title: GeometryReader
description: 크기 및 좌표계 정보를 전달하는 기능을 수행하는 컨테이너.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- GeometryReader
---

# GeometryReader
> A container view that defines its content as a function of its own size and coordinate space.

지오메트리 리더는 **자식 뷰**에 부모 뷰와 기기에 대한 `크기` 및 `좌표계` 정보를 전달하는 기능을 수행하는 컨테이너 뷰이다.

## 생성자
`GeometryProxy` 타입의 정보를 받아 콘텐츠를 정의하는 함수를 전달 받는다.

```swift
init(@ViewBuilder content: @escaping (GeometryProxy) -> Content)
```

### 특성
`content` 매개변수는 **뷰 빌더** 속성이 선언되어 있어, 뷰를 나열하는 것만으로도 사용할 수 있다.

이때 뷰는 ZStack과 같이 겹겹이 쌓이는 계층 구조를 가진다. 지오메트리 리더에 자식 뷰가 **하나만** 있을 때는 `중앙`에 정렬되지만, **두 개 이상**이라면 `좌상단`을 기준으로 배치가 된다.

또한 크기를 지정하지 않으면, 주어진 공간 내에서 최대 크기를 가지게 된다.

# GeometryProxy
> A proxy for access to the size and coordinate space (for anchor resolution) of the container view.

지오메트리 프록시는 **두 개의 프로퍼티**와 **하나의 메소드**, **하나의 첨자**를 제공하여 지로메트리 리더의 레이아웃 정보를 자식 뷰에 제공할 수 있다.

```swift
struct GeometryProxy {
    var size: CGSize { get }
    var safeAreaInsets: EdgeInsets { get }
    func frame(in coordinateSpace: CoordinateSpace) -> CGRect
    subscript<T>(anchor: Anchor<T>) -> T { get }
}
```

