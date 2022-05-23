---
title: ... must be used from main thread only
description: 메인 쓰레드에서 UI를 갱신할 시 나오는 에러.
categories:
- Swift
tags:
- Swift
- UIKit
- iOS
- SwiftUI
---

# 에러 설명
> Updating UI from a Completion Handler
> Long-running tasks such as networking are often executed in the background, and provide a completion handler to signal completion. Attempting to read or update the UI from a completion handler may cause problems.

사용자가 어떠한 이벤트를 발생하여 서버와의 통신을 통해 UI를 업데이트하는 과정에서, 해당 통신에 시간 간격이 길다면 사용자 입장에선 앱이 죽었다고 느낌을 받게 된다.

그렇기에 네트워크와 같은 처리를 진행할때는 일방적으로 메인 쓰레드에서 바로 UI를 업데이트 하는 코드를 작성하게 된다면 제목과 같은 에러를 반환한다.

# 해결
UI를 업데이트 하는 코드를 `DispatchQueue.main.async`에 감싸므로 해결을 한다.

```swift
DispatchQueue.main.async {
    // UI 업데이트 코드
}
```