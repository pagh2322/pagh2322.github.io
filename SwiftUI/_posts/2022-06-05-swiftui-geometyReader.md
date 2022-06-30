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

`GeometryReader`는 **자식 뷰**에 부모 뷰와 기기에 대한 `크기` 및 `좌표계` 정보를 전달하는 기능을 수행하는 컨테이너 뷰이다.

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

1. size: 지오메트리 리더의 크기를 반환한다
2. safeAreaInsets: 지오메르티 리더가 사용된 환경에서의 safe area에 대한 크기를 반환
3. frame(in:): 특정 좌표계를 기준으로 한 프레임 정보를 제공
4. subscript(anchor:): 자식 뷰에서 anchorPreference 수식어를 이용해 제공한 좌표나 프레임을 지오메트리 리더의 좌표계를 기준으로 다시 반환하여 사용하는 첨자이다. 이때 매개 변수의 타입은 `CGRect` 혹은  `CGPoint`이다.

## CoordinateSpace
열거형 타입이며, 세 가지 값 중 하나를 지정하면 그 좌표 공간에 관한 정보를 반환한다.

```swift
enum CoordinateSpace {
    case global
    case local
    case named(AnyHashable)
}
```

1. global: 화면 전체 영역(윈도우 bounds)을 기준으로 한 좌표 정보
2. local: 지오메트리 리더 bounds를 기준으로 한 좌표 정보
3. named: 명시적으로 이름을 할당한 공간을 기준으로 한 좌표 정보

### 예제
```swift
var body: some View {
    HStack {
        Rectangle()
            .fill(.yellow)
            .frame(width: 30)

        VStack {
            Rectangle()
                .fill(.blue)
                .frame(height: 200)

            GeometryReader {
                self.contents(geometry: $0)
            }
            .background(.green)
            .border(.red, width: 4)
        }
        .coordinateSpace(name: "VStackCS")
    }
    .coordinateSpace(name: "HStackCS")
}

func contents(geometry g: GeometryProxy) -> some View {
    VStack {
        Text("Local")
            .bold()
        Text(stringFormat(for: g.frame(in: .local).origin))
            .padding(.bottom)

        Text("Global")
            .bold()
        Text(stringFormat(for: g.frame(in: .global).origin))
            .padding(.bottom)

        Text("Named VStackCS")
            .bold()
        Text(stringFormat(for: g.frame(in: .named("VStackCS")).origin))
            .padding(.bottom)

        Text("Named HStackCS")
            .bold()
        Text(stringFormate(for: g.frame(in: .named("HStackCS")).origin))
    }
}

func stringFormat(for point: CGPoing) -> String {
    String(format: "(x: %.f, y: %.f)", arguments: [point.x, point.y])
}
```