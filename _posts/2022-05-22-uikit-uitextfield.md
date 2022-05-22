---
title: UITextField
description: 사용자의 입력 받기.
categories:
- UIKit
tags:
- UIKit
- iOS
- UITextField
- Input
- Delegate
---

# UITextField
저번에 SwiftUI로 Alert창에 TextField를 넣었을 때 잠시 사용했던 적이 있다.

UIKit에선 `UITextField`를 통해 사용자의 입력 값을 받아 올 수 있다.

# 사용법
`endEditing`를 통해 버튼을 눌러 키보드를 내릴 수 있다.

```swift
class SomeVC: UIViewController {
    @IBOutlet weak var searchTF: UITextField!

    @IBAction func searchPressed(_ sender: UIButton) {
        searchTextField.endEditing(true)
        print(self.searchTF.text!)
    }
}
```

# 키보드 확인 버튼
만약 키보드에 우측 하단에 있는 확인 버튼을 눌러 어떠한 동작을 하고 싶을 경우엔 **`Delegate패턴`**을 통해 아래와 같이 진행한다.

이 경우 텍스트 필드에서 어떠한 이벤트가 발생이 되면 컨트롤러에게 알림을 전달하게 된다. 그리고 `textFieldShouldReturn`이라는 함수를 통해 확인 버튼이 눌렀는지 알 수 있다.

```swift
class SomeVC: UIViewController, UITextFieldDelegate {
    @IBOutlet weak var searchTF: UITextField!

    override func viewDidLoad() {
        super.viewDidLoad()

        self.searchTF.delegate = self
    }

    @IBAction func searchPressed(_ sender: UIButton) {
        self.searchTF.endEditing(true)
        print(self.searchTF.text!)
    }

    func textFieldShouldReturn(_ textField: UITextField) -> Bool {
        self.searchTF.endEditing(true)
        print(self.searchTF.text!)
        return true // 확인 프로세스가 진행할 수 있게끔 true를 반환한다
    }
}
```

# 입력 종료
만약 사용자가 확인 버튼 등을 눌러 입력을 종료하여 어떠한 처리를 하고 싶을 경우 `textFieldDidEndEditing`을 통해 아래와 같이 진행한다.

```swift
class SomeVC: UIViewController, UITextFieldDelegate {
    func textFieldDidEndEditing(_ textField: UITextField) {
        self.searchTF.text = ""
    }
}
```

만약 사용자가 확인 버튼 등을 눌렀을 시, 그 전에 어떠한 처리를 하여 확인 버튼 등을 **계속해서 진행할지 판단**하기 위해 `textFieldShouldEndEditing`을 사용한다.

```swift
class SomeVC: UIViewController, UITextFieldDelegate {
    func textFieldShouldEndEditing(_ textField: UITextField) -> Bool {
        if textField.text != "" { // 입력된 값이 있기에 확인을 계속 진행
            return true
        } else { // 입력된 값이 없기에 계속 진행을 안 함
            textField.placeholder = "Please type city's name"
            return false
        }
    }
}
```

# 주의해야 할 점
위에 적어놓은 예시들은 한 화면에서 UITextField가 하나만 있다고 가정해서 작성을 하였다.

UITextField가 여러개가 있고, 그것들의 delegate를 해당 컨트롤러로 설정을 하면 위에 적어놓은 함수들이 모든 UITextField의 textField패러미터에 따라 적용이 된다.