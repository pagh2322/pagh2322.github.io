---
title: Alert 창 띄우기
description: UIKit로 Alert 창 띄우기.
categories:
- UIKit
tags:
- UIKit
- iOS
- Alert
---

# UIAlertController 생성
```swift
let alert = UIAlertController(title: "Alert title", message: "", preferredStyle: .alert)
```

# UIAlertAction 생성
```swift
let action = UIAlertAction(title: "Action title", style: .default) { action in
    // code that happens once the user clicks
    print("Success")
}
```

# TextField 추가(선택사항)
```swift
alert.addTextField { alertTextField in
    alertTextField.plcaholder = "Create new item"
}
```

# Action 추가 및 띄우기
```swift
alert.addAction(action)

present(alert, animated: true, completion: nil)
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

    alert.addTextField { alertTextField in
        alertTextField.plcaholder = "Create new item"
        textField = alertTextField
    }

    alert.addAction(action)

    present(alert, animated: true, completion: nil)
}
```