---
title: ModalView
description: 임시로 화면을 보여주는 ModalView.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- ModalView
---

# Modal
> **Modality** is a design technique that presents content in a temporary mode that requires an explicit action to exit. Presenting content modally can:
> 1. Help people focus on a self-contained task or set of closely related options
> 2. Ensure that people receive critical information and, if necessary, act on it

![](/images/swiftui/modalview/mail.png)

위와 같은 메일 앱 예시처럼 컨텐츠를 종료하기 위해 **명시적인 작업**이 필요한 임시모드로 나타내는 것으로, 중요한 정보를 보여주거나 집중할 수 있게 해주는 역할을 한다. 

# 예제
### ContentView
```swift
struct ContentView: View {
    @State private var showModal = false // 값이 true라면 Modal를 띄운다.
    
    var body: some View {
        VStack {
            Button(action: {
                self.showModal = true
            }) {
                Text("Button")
            }
            .sheet(isPresented: self.$showModal) {
                ModalView() // Modal로 보여줄 View
            }
        }
    }
}
```
### ModalView
```swift
struct ModalView: View {
    @Environment(\.presentationMode) var presentationMode: Binding<PresentationMode> // 현재 화면의 상태

    var body: some View {
        Group {
            Text("Modal view")

            Button(action: {
                self.presentationMode.wrappedValue.dismiss() // ModalView를 닫는다
            }) {
            Text("닫기")
            }
        }
    }
}
```

# ModalView닫기
위에 있는 예제처럼 `@Environment`를 통해 닫는 방식이 있다. 본인 역시 검색을 하면서 해당 방식을 알게 되었는데, 보면서 한가지 생각이 들었다.

> **ContentView**에 있는 `showModal`를 `Binding`을 통해 **ModalView**로 넘기면, `presentationMode`를 쓰지 않고 `showModal`를 통해 닫을 수 있지 않을까?

결과적으로 된다! (어찌보면 당연한 거..)

### ContentView
```swift
struct ContentView: View {
    @State private var showModal = false
    
    var body: some View {
        VStack {
            Button(action: {
                self.showModal = true
            }) {
                Text("Button")
            }
            .sheet(isPresented: self.$showModal) {
                ModalView(showModal: self.$showModal)
            }
        }
    }
}
```

### ModalView
```swift
struct ModalView: View {
    @Binding var showModal: Bool

    var body: some View {
        Group {
            Text("Modal view")

            Button(action: {
                self.showModal = false
            }) {
            Text("닫기")
            }
        }
    }
}
```

# ModalView에 Navigation Bar추가
ModalView를 `NavigationView`안에 화면을 구상하고, `toolbar`modifier를 통해 취소 혹은 완료와 같은 버튼을 추가할 수 있다.
```swift
struct ModalView: View {
    
    var body: some View {
        NavigationView {
            VStack {
                Text("Modal view")
            }
            .navigationTitle("Title")
            .navigationBarTitleDisplayMode(.inline)
            .toolbar {
                ToolbarItem(placement: .cancellationAction) {
                    Button(action: {
                        // 취소 버튼
                    }) {
                        Text("취소")
                    }
                }
                ToolbarItem(placement: .confirmationAction) {
                    Button(action: {
                        // 완료 버튼
                    }) {
                        Text("완료")
                    }
                }
            }
        }
    }
}
```

![](/images/swiftui/modalview/navigationbar.png)

# 참고 링크
[SwiftUI SwiftUI에서 Modal View 띄우기(모달뷰)](https://lucidmaj7.tistory.com/13)

[HIG - Modality](https://developer.apple.com/design/human-interface-guidelines/ios/app-architecture/modality/)