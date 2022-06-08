---
title: Gesture
description: SwiftUI에서 제스처를 알아보자.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- Gesture
- Callback
---

# Gesture
SwiftUI에서는 5가지의 기본 제스처 유형을 제공한다.

1. Tap
2. Long Press
3. Drag
4. Magnification
5. Rotaiton

## Tap
정해진 횟수만큼 탭 했을 때 지정한 동작을 수행하는 제스처이다. UIKit의 `UITapGestureRecognizer`와는 다르게 **손가락 몇 개로 터치**했는지에 대한 조건은 설정이 불가능하다.

기본값으로는 탭의 횟수는 한 번이다.

```swift
Circle()
    .onTapGesture {
        print("tapped")
    }
```

만약 여러 횟수의 탭이 되었을때는 아래와 같이 하면된다.

```swift
Circle()
    .onTapGesture(count: 2) {
        print("tapped twice")
    }
```

### TapGesture
```swift
var tap: some Gesture {
    TapGesture(count: 2)
        .onEnded { // 제스터를 인식했을 때 수행할 액션
            print("tapped twice")
        }
}

return Circle().gesture(tap)
```

## Long Press
기본값으로 0.5초 이상 화면을 누르고 있을 때 지정한 동작을 수행하는 제스처이다.

```swift
Circle()
    .onLongPressGesture {
        print("long press")
    }

Circle()
    .onLongPressGesture(
        minimumDuration: 0.5, // 인식에 필요한 시간 지정
        maximumDistance: 10,  // 처음 누른 위치에서 지정한 거리 이상 떨어지면 인식 실패
        perform: { // 인식이 성공하면 실행할 동작
            print("long press")
        },
        onPressingChanged: { value in // 현재 상태가 눌린 것인지에 대한 값이 변화하면서 실행할 동작
            print(value)
        }
    )
```

### LongPressGesture
```swift
var longPress: some Gesture {
    LongPressGesture()
        .onChanged({ value in // 눌렀을 때 호출
            print(value)
        })
        .onEnded({ _ in // 길게 누른 것으로 인식됐을 때 호출
            print("recognized")
        })
}

return Circle().gesture(longPress)
```

## Drag
화면 터치 이후 손을 뗄 때가지 그 움직임에 따라 인식된 정보를 전달하는 제스처이다.

### DragGesture
`DragGesture`는 생성자를 통해 **minimumDistance**와 **coordinateSpace**를 지정하여 일정 거리 이상을 드래그해야 인식하도록 하거나 특정 좌표계를 기준으로 설정할 수 있다.

예시로 드래그한 위치를 따라 뷰가 함께 움직이고, 드래그가 끝나면 원위치로 돌아가는 기능을 구현한 코드이다.

```swift
@GestureState private var translation: CGSize = .zero

var drag: some Gesture {
    DragGesture()
        .updating($translation) { (value, state, _) in
            state = value.translation
        }
}

return Circle().offset(translation).gesture(drag)
```

## Magnification
두 손가락을 터치해 오므리거나 벌리는 정도에 따라 그 변화된 값을 반환하는 제스처이다. 이 값은 `CGFloat`값을 전달하므로 이것을 그대로 **scaleEffect** 수식어에 적용하여 줌인 혹은 줌아웃 효과를 줄 수 있다.

### MagnificationGesture
`MagnificationGesture`는 생성자를 통해 **minimumScaleDelta**의 값을 지정하여 해당 값 이상의 비율로 확대 혹은 축소를 해야 제스처가 반응한다.

```swift
@GestureState private var scale: CGFloat = 1
@State private var latestScale: CGFloat = 11

var magnification: some Gesture {
    MagnificationGesture()
        .updating($scale) { (value, state, _) in
            state = value // 현재 확대, 축소 비율 저장
        }
        .onEnded { scale in
            latestScale += scale // 제스처 종료 시 최종 비율을 저장
        }
}

return Circle().scaleEffect(latestScale * scale).gesture(magnification)
```

## Rotation
두 손가락을 터치한 뒤 회전시킨 정도에 따라 그 회전 각도의 값을 반환하는 제스처이다. 이 값은 `Angle`값을 전달하므로 이것을 그대로 **rotationEffect** 수식어에 적용하여 회전 효과를 줄 수 있다.

### RotationGesture
`RotationGesture`는 생성자를 통해 **minimumAngleDelta**의 값을 지정하여 이 각도 이상을 회전을 해야 제스처가 반응한다.

```swift
@GestureState private var angle: Angle = .zero

var rotation: some Gesture {
    GestureState()
        .updating($angle) { (value, state, _) in
            state = value
        }
}

return Circle().rotationEffect(angle).gesture(rotation)
```

# Gesture Callback
위 예제에서 많이 나오는 `updating`, `onChanged`, `onEnded` 3가지 형태의 콜백을 알아보자.

## updating
제스처를 인식하면 **즉시** 호출된다. 또한 제스처가 관리하는 값이 **변화**할 때마다 호출되며, 제스처가 종료될 때나 취소될 때는 호출이 되지 않는다.

일시적인 상태의 변화를 다루기 위한 것이기에, 아래의 `onChange`와 헷갈릴 수 있다.

### GestureState
여기서 `GestureState`는 제스처가 동작 중일 때만 활용될 임시 값을 저장하며, 읽기 전용이다. 제스처가 종료된 이후에는 다시 초기값으로 되돌아간다.

## onChange
제스처가 가진 값이 새로운 것으로 변경되었을 때 호출되는 콜백이며, `updating` 이후에 호출이 되며, 상태를 영구적으로 저장하는 데 사용이 된다.

```swift
@State private var translation: CGSize = .zero

var drag: some Gesture {
    DragGesture()
        .onChanged({
            translation = $0.translation
        })
}
```

## onEnded
제스처 인식이 종료되었을 때 호출되는 콜백이며, 마지막 순간에 가진 값을 전달해 준다.

# Gesture 수식어
## simultaneousGesture
만약 아래처럼 2가지 이상의 제스처를 동시에 사용해야 할 때 `simultaneousGesture` 수식어를 사용한다.

```swift
var longPress: some Gesture {
    LongPressGesture()
        .onChanged { _ in
            print("long press began")
        }
        .onEnded { _ in
            print("long press ended")
        }
}

var tap: some Gesture {
    TapGesture()
        .onEnded { 
            print("tapped")
        }
}

return Circle().gesture(longPress).simultaneousGesture(tap)
```