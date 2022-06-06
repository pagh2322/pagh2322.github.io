---
title: Animation
description: SwiftUI에서 애니메이션를 알아보자.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- Animation
---

# Animtation Timing
애니메이션 타입에는 `.default` 이외에도 애니메이션 속도를 조절할 수 있는 여러 옵션이 있다.

## default
기본 값으로, `easeInOut`이 사용되며 애니메이션 지속 시간이 **0.35초**로 고정된다.

```swift
.animation(.default)

withAnimation(.default) {
    // code
}
```

## linear
처음부터 끝까지 일정한 속도로 애니메이션이 진행된다. 반복되는 애니메이션에 많이 사용된다.

```swift
.animation(.linear)
.animation(.linear(duration: 0.35))
```

## easeIn
처음에는 느리게 시작했다가 점점 빠르게 진행되는 애니메이션입니다. 화면 내에서 움직이는 뷰보다는 화면 밖으로 사라지는 뷰 등에 사용한다.

```swift
.animation(.easeIn)
```

## easeOut
처음에는 빠르게 시작하고 끝에서는 천천히 진행되며 감속효과를 준다.

```swift
.animation(.easeOut)
```

## easeInOut
시작과 끝에서 느리게 동작하고 중간 지점에서 빠르게 진행된다.

```swift
.animation(.easeInOut)
```

## spring
목적 지점에서 진동 효과를 주어서 좀 더 동적인 느낌을 줄 수 있다.

```swift
.animation(.spring())
.animation(.spring(response: 0.55, dampingFraction: 0.825, blendDuration: 0))
```

### response
스프링의 강성 및 애니메이션 지속 시간에 대한 근사치를 나타낸다. 하지만 아래의 `dampingFraction`에 영향을 받기에 실제 지속 시간과는 차이가 있다.

### dampingFraction
애니메이션의 진동 수준을 결정짓는 값이다.

1이면 진동하지 않으면서 최단 시간 내에 목적 지점에서 그대로 멈춘다.
1보다 작으면 진동이 생기며,
1보다 크면 목적지에 도착하는 시간이 길어지고 진동하지 않는다.

또한, 0에 가까울수록 진동이 커지고, 만약 0을 넣으면 진동이 작아지지 않고 계속해서 유지되어 애니메이션이 영구적으로 지속된다.

### blendDuration
애니메이션이 조합될 때 response 값의 변화를 보간하는 데 사용된다.

## interactiveSpring
위의 spring 과 거의 동일하며, 대화형 방식의 애니메이션에 좀 더 적합하다.

```swift
.animation(.interactiveSpring(response: 0.15, dampingFraction: 0.86, blendDuration: 0.25))
```

## interpolatingSpring
물리 모델링에 기반한 값을 더욱 세밀하게 다룰 수 있도록 만들어진 옵션이다.

```swift
.animation(.interpolatingSpring(mass: 1, stiffness: 100, damping: 10, initialVelocity: 0))
```

### mass
질량을 의미하며, 작을수록 빠르게 움직이며 힘의 크기나 반동이 약하고, 클수로 느리지만 큰 힘이 가해지고 반동이 커진다.
기본값은 1이다.

### stiffness
스프링의 강성을 의미한다.

### damping
마찰력과 같은 역할을 한다. 다른 값들과 비교해 일정 이상 수치가 커지면 진동하지 않고 바로 목적지에 멈춘다. `spring`의 `dampingFraction`과 마찬가지로 0이 되면 마찰력이 없는 것처럼 진동이 멈추지 않고 무한 반복되지만, 0보다 무조건 커야 한다.

### initialVelocity
애니메이션이 시작할 때 초기에 가해지는 속도를 의미한다.

# Animation Control
애니메이션들을 원하는 방식으로 제어하고 사용하게끔 도와주는 인스터턴스 메소드가 있다.

## delay
매개 변수로 값을 받아 해당하는 시간(초)만큼 애니메이션을 지연시킨 후 수행한다.

```swift
func delay(_ delay: Double) -> Animation
```

## speed
애니메이션을 지정한 배율만큼 곱한 속도로 진행하게 만든다.

```swift
func speed(_ speed: Double) -> Animation
```

## repeatCount, repeatForever
`repeatCount`는 애니메이션을 일정 횟수만큼만 반복할 때 사용하며, `repeatForever`는 이 반복을 무한정 실행하도록 한다. `autoreverses`가 **true**값을 전단하면 애니메이션이 수행되기 전과 후의 모습을 오가는 모습을 볼 수 있다.

```swift
func repeatCount(_ repeatCount: Int, autoreverses: Bool = true) -> Animation
func repeatForver(autoreverses: Bool = true) -> Animation
```