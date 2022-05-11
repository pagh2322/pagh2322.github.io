---
title: SwiftUI encountered an issue when pushing aNavigationLink. Please file a bug.
description: ForEach으로 감싼 NavigationLink에 isActivie패러미터 값 전달 시 생기는 에러.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- Error
- NavigationLink
- isActivie
---
# 문제점
`ForEach`으로 감싼 `NavigationLink`에 `isActivie`패러미터 값을 전달하면서 발견한 에러이다.

```swift
struct ContentView: View {
    @State var isTappedRowItem = false
    var body: some View {
        NavigationView {
            ScrollView(.vertical, showsIndicators: false) {
                ForEach(0..<100) { index in
                    NavigationLink(destination: Text("NavigationLink를 통해 들어온 \(index)번째 페이지"), isActive: self.$isTappedRowItem) {
                        Button(action: {
                            self.isTappedRowItem = true
                        }) {
                            Text("\(index) : 블로그에 올릴 예제 코드입니다. 가나다라마바사")
                        }
                    }
                    .padding(.bottom, 10)
                }
            }
            .navigationBarHidden(true)
            .padding(.vertical, 1)
        }
    }
}
```

만약 여러개의 Button이 있고 터치 시 원하는 View로 이동하고 싶을 경우 위와 같은 코드를 쓰게 될텐데, 해당 View로 전달된 값이 원하는 값으로 전달이 되지 않는다.

![](/images/swiftui/swiftuiEncounteredWhenpushingaNavigationLink/problem.gif)

# 해결 방법
`ForEach`으로 감싸진 `NavigationLink`를 사용하면서 `isActive`를 전달하게 될 경우엔 위와 같은 현상이 일어난다.

## isActive 제거
만약 `isActivie`를 사용하지 않을 경우엔 아래와 같이 작성하면 에러가 발생하지 않는다.

```swift
struct ContentView: View {
    var body: some View {
        NavigationView {
            ScrollView(.vertical, showsIndicators: false) {
                ForEach(0..<100) { index in
                    NavigationLink(destination: Text("NavigationLink를 통해 들어온 \(index)번째 페이지")) {
                        Text("\(index) : 블로그에 올릴 예제 코드입니다. 가나다라마바사")
                    }
                    .padding(.bottom, 10)
                }
            }
            .navigationBarHidden(true)
            .padding(.vertical, 1)
        }
    }
}
```

## isActive 사용
만약 `isActivie`를 사용해야 할 경우엔 **한 개**의 `isActive`패러미터가 있는 `NavigationLink`를 선언해야한다.

```swift
struct ContentView: View {
    @State var isTappedRowItem = false
    @State var rowItemIndex = 0
    var body: some View {
        NavigationView {
            ScrollView(.vertical, showsIndicators: false) {
                ForEach(0..<100) { index in
                    Button(action: {
                        self.isTappedRowItem = true
                        self.rowItemIndex = index
                    }) {
                        Text("\(index) : 블로그에 올릴 예제 코드입니다. 가나다라마바사")
                    }
                    .padding(.bottom, 10)
                }
                .background(
                    NavigationLink(destination: Text("NavigationLink를 통해 들어온 \(self.rowItemIndex)번째 페이지"), isActive: self.$isTappedRowItem) { })
            }
            .navigationBarHidden(true)
            .padding(.vertical, 1)
        }
    }
}
```

# 결과
![](/images/swiftui/swiftuiEncounteredWhenpushingaNavigationLink/solve.gif)

# 참고 링크
[SwiftUI NavigationLink Bug (SwiftUI encountered an issue when pushing aNavigationLink. Please file a bug.)](https://stackoverflow.com/questions/66017531/swiftui-navigationlink-bug-swiftui-encountered-an-issue-when-pushing-anavigatio)