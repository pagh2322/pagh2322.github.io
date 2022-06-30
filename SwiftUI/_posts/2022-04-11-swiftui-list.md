---
title: List
description: 단일 열에 정렬 된 데이터들을 보여주는 Container.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- List
---

# List
> A container that presents rows of data arranged in a single column, optionally providing the ability to select one or more members.

`List`에 원하는 View를 전달하면 각 행에 담아 보여진다.

# 두 가지 방법
## 정적
```swift
var body: some View {
    List {
        Text("A List Item")
        Text("A Second List Item")
        Text("A Third List Item")
    }
}
```

## 동적
동적으로 생성 시, `Range<Int>`혹은 `RandomAccessCollecion`프로토콜을 준수하는 데이터를 전달한다.

이 경우엔, 데이터의 각 요소들을 구분하고 식별할 수 있어야 하며, 직접 `id`식별자를 지정하거나 아래 예제처럼 데이터 타입 자체에 `Identifiable`프로토콜을 채택한다.

```swift
struct Ocean: Identifiable {
    let name: String
    let id = UUID()
}

private var oceans = [
    Ocean(name: "Pacific"),
    Ocean(name: "Atlantic"),
    Ocean(name: "Indian"),
    Ocean(name: "Southern"),
    Ocean(name: "Arctic")
]

var body: some View {
    List(oceans) {
        Text($0.name)
    }
}
```

# Section
`Section`에는 `header`와 `footer`를 생략 혹은 추가할 수 있다.

```swift
let data = ["drink", "snack"]
Section(header: Text("header"), footer: Text("footer")) {
    ForEach(data, id: \.self) {     
        Text($0)
    }
}
```

# ListStyle
`List`의 밖에 스타일을 설정할 수 있다.

```swift
List {
    // View
}
.listStyle(/* 여러 스타일 값 전달*/)
```

# onDelete
만약 `List`에 있는 데이터를 삭제하고 싶으면 [swipeActions](2022-04-11-swiftui-swipeActions.md)를 쓰거나 `onDelete`를 사용하여 간단하고 직관적이게 구현할 수 있다.

```swift
let dataList = [/* */]
List {
    ForEach(dataList, id: \.self) { data in
        Text(data)
    }
    .onDelete(perform: self.deleteRow)
}
private func deleteRow(at indexSet: IndexSet) {
    self.dataList.remove(atOffsets: indexSet)
}
```

![](/images/swiftui/list/ondelete.png)


## ListStyle 종류
![](/images/swiftui/list/swiftui-list1.png)
![](/images/swiftui/list/swiftui-list2.png)

# 참고 링크
[SwiftUI: List(ListStyle / onDelete / onMove)](https://seons-dev.tistory.com/139)

[SwiftUI Delete Rows from List Tutorial](https://www.ioscreator.com/tutorials/swiftui-delete-rows-list-tutorial)