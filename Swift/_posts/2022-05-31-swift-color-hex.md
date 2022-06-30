---
title: Hex로 Color값 설정
description: Hex값으로 색상을 부여하자.
categories:
- Swift
tags:
- Swift
- Color
- HEX
---

디자이너가 건네준 색상들은 보통 헥스값을 가지고 있다.

해당 색상들을 일일히 RGB형태로 할 수 있지만, `Color`에 extension을 추가하여 바로 사용하게 하면 더욱 편리할 듯 하다.

# 코드
```swift
extension Color {
    init(hex: String) {
        let scanner = Scanner(string: hex)
        _ = scanner.scanString("#") // "#" 문자 제거

        var rgb: UInt64 = 0
        scanner.scanHexInt64(&rgb) // 문자열을 Int64타입으로 변환해 rgb에 저장

        let r = Double((rgb >> 16) & 0xff) / 255.0
        let g = Double((rgb >> 8) & 0xff) / 255.0
        let b = Double((rgb >> 0) & 0xff) / 255.0
        self.init(red: r, green: g, blue: b)
    }
}
```

# 사용하기
```swift
static let myColor = Color(hex: "#6e6e6e")
```