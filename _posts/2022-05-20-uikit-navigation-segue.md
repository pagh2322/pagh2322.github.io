---
title: 화면 간 이동
description: UIKit에서 화면 이동하기.
categories:
- UIKit
tags:
- UIKit
- iOS
- Segue
---

# 화면 이동
모바일 앱에서는 화면 이동이 매우 자주 일어난다. UIKit에선 어떻게 해야 화면 간 이동할 수 있을까?

# Segue
Storyboard에서 각 화면에 대응하는 `UIViewController`가 있어야 한다. 그리고 어떠한 컨트롤러에서 이동할 다른 컨트롤러로 `Segue`를 연결해줌으로써 첫번째 준비가 끝난다.(이때 Identifier를 설정하여 코드에서 식별할 수 있게 해준다.)

```swift
class FirstVC: UIViewController {
    @IBAction func buttonPressed(_ sender: UIButton) {
        self.performSegue(withIdentifier: "firstToSecond", sender: nil) // 여기서 sender는 메시지를 전달하는 역할을 하는 객체이다
    }
}

class SecondVC: UIViewController {
    var value: Int

    override func viewDidLoad() {
        super.viewDidLoad()
    }
}
```

# prepare
> Prepares the generator to trigger feedback.

만약 다른 화면으로 이동하면서 값을 같이 넘기고 싶을때, `prepare`함수를 통해 넘겨줄 수 있다. 이때 이동할 수 있는 화면이 여러개가 있을 수 있기에 식별자를 통해 진행한다.

```swift
class FirstVC: UIViewController {
    @IBAction func buttonPressed(_ sender: UIButton) {
        self.performSegue(withIdentifier: "firstToSecond", sender: nil)
    }

    override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
        if segue.identifier == "firstToSecond" {
            let destinationVC = segue.destination as! SecondVC // segue.destination는 UIViewController타입이기 때문에 형 변환이 필요하다
            destinationVC.value = 1
        }
    }
}
```

# 뒤로 가기
만약 뒤로 가기를 한 뒤에 어떠한 처리를 하고 싶으면 completion에 클로저를 넣어준다.

```swift
class SecondVC: UIViewController {
    var value: Int

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func buttonPressed(_ sender: UIButton) {
        self.dismiss(animated: true, completion: nil)
    }
}
```