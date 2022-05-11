---
title: No exact matches in call to instance method 'appendInterpolation'
description: 문자열 보간법 에러.
categories:
- Swift
tags:
- Swift
- iOS
- Error
---

# 문제점
![](/images/swift/appendInterpolation/problem.png)

위에서 `getTime`함수는 아래와 같이 *Optional*를 반환한다.

```swift
func getTime(level: Int) -> Int? {
	// codes
}
```

# 검색
구글에 검색한 결과 아래처럼 나왔다.(변수의 이름은 코드와 알맞게 변경을 하였다.)

> It's because your parameter’s type is Optional Int and Int interpolation can't handle Optionals. You need to use nil coalescing to provide a default value in case **`coffeeCapsule.coffeeLevel`** is **`nil`**

즉, *Int*의 문자열 보간법은 `Optional`에 대해서 처리할 수 없으니, 값이 `nil` 인 상황에 대한 처리가 필요하다.

# 해결법
```swift
Text("\(getTime(level: coffeeCaspule.coffeeLevel) ?? 0)")
    .font(.title)
    .padding(.bottom, 100.0)
```

# 참고 링크
[Swift Error - No exact matches in call to instance method 'appendInterpolation'](https://www.hackingwithswift.com/forums/swiftui/swift-error-no-exact-matches-in-call-to-instance-method-appendinterpolation/10472)