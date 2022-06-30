---
title: TextField에 입력된 문자가 *로 표시하기
description: 비밀번호와 같은 용도에 쓰일 방법.
categories:
- UIKit
tags:
- UIKit
- iOS
- TextField
- UITextField
---

# 문제점
SwiftUI에서 Alert창에 TextField를 넣어서 비밀번호 입력 창을 띄우는 과정에서 UIKit코드를 사용하였다.

사용자가 TextField에 입력한 문자는 기본적으로 보여지지만, 만약 비밀번호와 같은 경우에는 *로 표시하면서 하는 것이 일반적이다.

스토리보드의 Inspector를 통해 구현을 할 수 있지만, 이 글에서는 코드로 구현을 하였다.

# 코드로 구현
UITextField가 제공하는 여러 속성 값들 중에서 `isSecureTextEntry`가 있다.

> Identifies whether the text object should disable text copying and in some cases hide the text being entered

해당 속성 값이 `true`이라면 아래와 같이 설정된다.

1. 입력된 값은 복사가 불가능
2. **문자**가 입력이 들어오게 되면, 이전 **문자**는 숨김 상태(*)로 변환

```swift
let tempTextField = UITextField()
tempTextField.isSecureTextEntry = true
```

# 참고 링크
[비밀번호(*로 표시되는) textfield 설정 방법](https://m.blog.naver.com/chltmddus23/221768284384)