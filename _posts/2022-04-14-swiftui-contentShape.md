---
title: .contentShape
description: 탭의 유효 범위를 넓어주는 modifier.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- modifier
---

# 문제점

![](/images/swiftui/contentShape/problem.png)

### 코드
```swift
struct ContentView: View {
    @State private var showModal = false
    
    var body: some View {
        List {
            ForEach(0..<10) { index in
                HStack {
                    Text("Item\(index)")
                    
                    Spacer()
                    
                    Text("Go")
                }
                .onTapGesture {
                    self.showModal = true
                }
            }
        }.sheet(isPresented: self.$showModal) {
            ModalView(showModal: self.$showModal)
        }
    }
}
```

좌우 끝에 있는 `Text`를만 눌러야 `ModalView`가 나온다.

# contentShape
만약 `VStack`혹은 `HStack`에 `tapGesture`를 추가하면 해당 Container에 들어간 유효한 `View`의 범위들만 터치가 가능하다. `View` 사이에 존재하는 `Spacer()`와 같은 빈 범위는 탭이 불가능하다.

이 경우, `contentShape`를 통해 해결할 수 있다.

# 예제
```swift
HStack {
    Text("Item\(index)")
    
    Spacer()
    
    Text("Go")
}
.contentShape(Rectangle())
.onTapGesture {
    self.showModal = true
}
```

### 결과
![](/images/swiftui/contentShape/solve.gif)

### 다른 방법?
우측에 있는 `Text`를 `Button`으로 변경하면 `contentShape`필요없이 터치가 가능해진다.

```swift
HStack {
    Text("Item\(index)")
    
    Spacer()
    
    Button(action: {
        self.showModal = true
    }) {
        Text("Go")
    }
}
```

#### 결과
![](/images/swiftui/contentShape/another.gif)

아마 `Text`나 `Image`가 아니라서 되는 듯한데, 정확한 이유는 추후에 찾아봐야겠다.

# 참고 링크
[SwiftUI can't tap in Spacer of HStack](https://stackoverflow.com/questions/57191013/swiftui-cant-tap-in-spacer-of-hstack)

[How to control the tappable area of a view using contentShape()](https://www.hackingwithswift.com/quick-start/swiftui/how-to-control-the-tappable-area-of-a-view-using-contentshape)