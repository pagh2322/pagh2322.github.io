---
title: ForEach
description: 주어진 데이터를 통해 View를 계산해주는 Container.
categories:
- SwiftUI
tags:
- SwiftUI
- iOS
- ForEach
- Identifiable
---

# ForEach
> A structure that computes views on demand from an underlying collection of identified data.

`Collection의` 데이터를 기반으로 `View`들을 계산하는 Structure이다. `ForEach`자체가 일종의 `View Container`처럼 작용하고 있고, `View`를 계산해서 보여준다.

## 예제
0부터 9까지 있는 *Int Range*를 만든 뒤, 각 값에 대응하는 `Text`를 보여준다.

```swift
ForEach(0..<10) {
    Text("Num: \($0)")
}
```

![](/images/swiftui/foreach/simpleExample.png)

여기서 *Int Range*는 아까 말했던 `Collection`에 속하는데, 예제처럼 `0..<10`와 같은 *Open Range*는 가능하지만, `0...10`와 같은 *Closed Range*는 불가능하다.

# RAC 데이터
`ForEach`에서 데이터는 무조건 *Random Access Collection*이어야 한다. 코딩을 하면서 자주 데이터는 *Array*형식으로 전달하는데, 이 경우엔 *RAC*를 만족한다.

하지만 아래와 같은 코드를 실행하게 되면 에러를 반환한다.

```swift
struct ContentView: View {
    let array: [Int] = [0, 1, 2, 3, 4]

    var body: some View {
        ForEach(self.array) {
            Text("\($0)")
        }
    }
}
```

**에러 메시지**
> Referencing initializer 'init(_:content:)' on 'ForEach' requires that 'Int' comform to 'Identifiable'

# Identifiable
> A class of types whose instances hold the value of an entity with stable identity.

이 프로토콜은 각 데이터가 각자를 식별할 수 있느 무언가를 가지고 있게 한다. 이 프로토콜을 지키기 위해 필수로 가지고 있어야 하는 프로퍼티는 `id`가 있다.

## 예제

```swift
struct Example: Identifiable {
    let data: Int
    let id: String
}
```

# RAC 데이터로 다시 돌아와서

아까와 같은 예제 코드는 *Array*에서의 데이터들을 구분할 수 있는 기준이 없다.

만약 해당 예제를 실행하고 싶으면, *Int*에 `id`를 *extension*을 통해 넣어줄 수 있지만 번거롭다.

또 다른 방법으로는 `ForEach`에 직접 `id`를 부야할 수 있다.

```swift
struct ContentView: View {
    let array: [Int] = [0, 1, 2, 3, 4]

    var body: some View {
        ForEach(self.array, id: \.self) {
            Text("\($0)")
        }
    }
}
```

이 경우 해당 데이터의 값 자체를 `id`로 간주하여 에러가 없이 실행이 된다. 위와 같은 경우 0의 id는 0, 1의 id는 1이다.

# Hashable
애플 공식문서에서 `ForEach`의 정의를 보면, `id`는 `Hashable`을 지켜야 한다. 아까와 같은 `id: \.self`는 `Hashable`을 지켜야 한다는 뜻이다.

> A type that can be hashed into a Hasher to produce an integer hash value.

이 프로토콜은 `func hash(into:)`라는 메서드를 가지고 있어야 하며, 기본적으로 **Int**, **String**, **Float**, **Boolean** 타입의 경우에는 이 조건을 만족하고 있다. 즉, `\.self`를 통해 넘겨준 **Int**는 `Hashable`을 만족하기 때문에 문제가 없던 것이다.

# 또 다른 문제점
데이터를 `Identifiable`과 `Hashable`을 지키게 하는 목적은 제대로 구분하기 위해서 하는 것이지만, 만약 이 경우에도 값이 같거나 하는 이유로 구분이 불가능할 수 있다. 즉 Hash값을 만들더라도 같은 결과가 나올 수 있다.

## 해결책 1: ForEach에서 id값 조정
```swift
struct Person: Hashable {
    let name: String
    let sex: Int

    init(_ name: String, sex: Int) {
        self.name = name
        self.sex = sex
    }
}
struct ContentView: View {
    let array = [Person("Tom", sex: 0), Person("Anna", sex: 1)]

    var body: some View {
        ForEach(self.array, id: \.self.name) { person in
            Text(person.name)
        }
    }
}
```

이 경우, 각 데이터의 `name`을 기준으로 구분한다.

## 해결책 2: indices를 이용한 인덱스 접근
```swift
struct ContentView: View {
    let array = [Person("Tom", sex: 0), Person("Anna", sex: 1)]

    var body: some View {
        ForEach(self.array.indices, id: \.self) { index in
            Text(self.array[index].name)
        }
    }
}
```

# 참고 링크
[Swift: SwiftUI의 ForEach 알아보기(정의, 사용 팁)](https://medium.com/hcleedev/swift-swiftui의-foreach-알아보기-정의-사용-팁-8790117e6fd9)