---
title: Alert 창 띄우기
description: UIKit로 Alert 창 띄우기.
categories:
- UIKit
tags:
- UIKit
- iOS
- Alert
- TextField
---

# Alert 창 띄우기
## UIAlertController 생성
**preferredStyle 패러미터**로 경고창을 `ActionSheet` 혹은 `Alert` 방식으로 띄울 수 있다.

```swift
let alert = UIAlertController(title: "Alert title", message: "", preferredStyle: .alert)
```

## UIAlertAction 생성 및 추가
**style 패러미터**는 `cancel`, `default` 혹은 `destructive` 총 3가지가 있다.

```swift
let action = UIAlertAction(title: "Action title", style: .default) { action in
    // code that happens once the user clicks
    print("Success")
}
alert.addAction(action)
```

## TextField 추가(선택사항)
```swift
alert.addTextField { alertTextField in
    alertTextField.plcaholder = "Create new item"
}
```

## Alert 띄우기
창을 띄우기 실행하고 싶은 코드가 없기 때문에 **completion 패러미터**를 `nil`로 주었다

```swift
self.present(alert, animated: true, completion: nil)
```

# 총 코드
```swift
@IBAction func buttonPressed(_ sender: UIButton) {
    var textField = UITextField()

    let alert = UIAlertController(title: "Alert title", message: "", preferredStyle: .alert)

    let action = UIAlertAction(title: "Action title", style: .default) { action in
        // code that happens once the user clicks
        print(textField.text)
    }
    alert.addAction(action)

    alert.addTextField { alertTextField in
        alertTextField.plcaholder = "Create new item"
        textField = alertTextField
    }

    self.present(alert, animated: true, completion: nil)
}
```