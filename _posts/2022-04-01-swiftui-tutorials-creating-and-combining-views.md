---
title: SwiftUI Tutorials - Creating and Combining Views
description: 애플 SwiftUI 공식 튜토리얼를 정리하였습니다.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- Apple Tutorials
---

# SwiftUI Tutorials
## 시작하기 전에
이 게시글은 애플 SwiftUI 공식 튜토리얼를 정리하였습니다. 본인이 이해가 안 가거나 기억하고 싶은 부분들을 위주로 정리하였기에, 자세한 내용을 알고 싶으시다면 아래 링크로 이동해주세요!

## 링크
[애플 SwiftUI 공식 튜토리얼](https://developer.apple.com/tutorials/swiftui)

## 다른 방법
Xcode를 키신 후, `command` + `shift` + `0`을 누르시면 애플 공식 도움창을 통해 보실 수 있다.

# Creating and Combining Views
## Create a New Project and Explore the Canvas
Xcode에서 프로젝트를 실행하고 나면, `@main`이 적혀있는 파일이 열린다. 이 속성은 앱의 진입 포인트를 식별하는 역할을 한다.

Xcode에서 코드를 수정하면 오른쪽 Canvas창에 있는 화면이 갱신되는데, Flutter와 유사한 느낌을 준다.

## Customize the Text View
Inspector와 같은 GUI방식을 통해 수정도 할 수 있다.

### modifier
View를 커스텀할 경우, `modifier`라고 부르는 메소드를 불러서 할 수 있다. 해당 View의 여러 프로퍼티들을 바꿀 수 있으며, 각 `modifier`는 **새로운 View**를 반환한다.

## Combine Views Using Stacks
### 중첩된 View
Flutter에서 모든 요소를 Widget으로 보듯이, SwiftUI에선 **View**라고 보는 듯 하다. 그렇기 때문에 View안에 **또 다른 View**가 존재할 수 있다. 즉 중첩된 View.

코드상에선 해당 View(Text, Image 등)을 `command` + `click`하면 간단하게 여러 View에 중첩할 수 있게 해준다.

`command` + `shift` + `l` 또는 우측 상단에 있는 더하기 버튼(+)을 통해 쉽게 View들을 찾아 삽입할 수 있지만, 아직 버그가 많아 사용을 자제해야 할 듯.

### alignment
예를 들어, VStack에선 기본적으로 View들을 center기준으로 배치를 한다. `alignment`패러미터로 원하는 값을 통해 배치를 바꿀 수 있다.

```swift
VStack(alignment: .leading) {
    Text("Turtle Rock")
        .font(.title)

    Text("Joshua Tree National Park")
        .font(.subheadline)
}
```

### Spacer()
쉽게 생각하면, 투명하지만 분명히 존재하는 View로 인해 빈 공간을 채워 넣어 다른 View들과 같이 배치를 한다. 아래 예시는 2개의 `Text`가 서로 양 끝에 있는 코드이다.

```swift
HStack {
    Text("Joshua Tree National Park")

    Spacer()
                
    Text("California")
}
```

## Create a Custom Image View
### 이미지를 넣는 방법
개인적으로 놀란 부분이다. 이미지를 `Asset`에 넣은 뒤, 간단하게 `Image("사진 파일 이름")`이라는 방식으로 구현한다. Flutter나 안드로이드보다 훨씬 간단하다.

### Custom View
앞서 말했다 싶이 Flutter와 매우 유사하게, 모든 요소가 View이다 보니 자주 쓰일듯 한 View들은 **따로 파일**을 생성해서 관리를 해준다. 개발하는 앱에서 자주 쓰이는 버튼이나 다른 View들은 하나의 파일에서 미리 만들어 관리하면 유지보수 및 개발 속도 측면에서도 도움이 될 거 같다.

### overlay
`overlay`를 통해 해당 View**위**에 View를 배치할 수 있다.

> `background`와 `overlay`의 차이점
> `background` : View**밑**에 배치
> `overlay` : View**위**에 배치

### shadow
CardView와 같은 View에서 쓰면 좋을 듯 하다.

## Use SwiftUI Views From Other Frameworks
### @State 속성
Flutter에서의 Stateful와 유사하다. 해당 속성을 가진 프로퍼티의 값이 변경이 되면, 속한 View가 전체적으로 업데이트, 즉 새로 로드한다.

> 만약 해당 View가 매우 복잡하게 구성되어 있으면 자주 로드하면 성능에 부담이 줄 텐데, 가령 Flutter에서 GetX와 같은 상태 관리 프레임워크가 있을까? EnviornmentObject와 같은 개념이 있는 듯 한데, State와 크게 다르지 않아 보인다.

## Compose the Detail View
### frame
`frame`을 통해 View의 사이즈를 수정할 수 있다. 아래 예제처럼 height의 값만 전달하면, width의 값은 해당 View의 콘텐츠의 크기만큼 설정된다.

```swift
Rectangle()
    .frame(height: 300)
```

### offset
아래 예제처럼 배치된 위치에서 더 세세하게 변경하고 싶은 경우 쓰면 좋을 듯하다.

```swift
Text("text")
    .offset(x: 50)
```

### ignoresSafeArea
만약 스크린의 상단 부분까지 View가 배치되길 원한다면 아래 예제처럼 적어준다.

```swift
Rectangle()
    .ignoresSafeArea(edges: .top)
    .frame(height: 300)
```

### Divider()
서로 다른 View사이에 구분선이 필요할 경우 쓰인다.