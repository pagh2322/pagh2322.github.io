---
title: 코드만으로 UI구현
description: UIKit에서 Storyboard없이 UI를 구현.
categories:
- UIKit
tags:
- UIKit
- iOS
- Storyboard
---

# Storyboard
현재 UIKit를 공부하고 있는 과정에서 UI는 Storyboard에서 구현을 많이 하는 듯 하다. 마치 안드로이드에서 XML파일에 구현하듯이.

하지만 전에 얼핏 듣기론 Storyboard만으로 UI를 구성하다보면 어느새 버그가 많아진다고 들었고, Storyboard없이 직접 코드만으로 UI를 그리는 방식이 있다고 들었다.

마지막으로 하나씩 Storyboard에서 드래그 드랍으로 UI요소들을 생성하고 연결하는 과정이 귀찮기도 했다.

# 추가하기
UI요소를 만들고 원하는 frame을 설정한 뒤, 해당 부모 뷰에다 넣어준다는 코드를 작성하면 끝난다

## UILabel
```swift
class SomeViewController: UIViewController {
    var value: String
    
    override func viewDidLoad() {
        super.viewDidLoad()

        view.background = .white

        let width = view.frame.size.width
        let height = view.frame.size.height

        let label = UILabel()
        label.text = self.value
        label.frame = CGRect(x: width * 0.5 - width * 0.8 / 2, y: height * 0.5 - 50 / 2, width: width * 0.8, height: 50)
        view.addSubview(label)
    }
}
```

## UIButton(터치 시 함수까지 구현)
예제에서 마지막 `addTarget`의 첫 번째 **target 파라미터**는 `action 파라미터`에서 실행 할 메소드를 가지고 있는 객체를 전달한다. 해당 코드에서는 ButtonViewController의 `buttonPressed` 함수를 실행할 것이기에 `self`를 전달한다.

```swift
class ButtonViewController: UIViewController {

    var label = UILabel()

    override func viewDidLoad() {
        super.viewDidLoad()

        let width = view.frame.size.width
        let height = view.frame.size.height

        label.text = "test"
        label.frame = CGRect(x: width * 0.5 - width * 0.8 / 2, y: height * 0.5 - 50 / 2, width: width * 0.8, height: 50)
        view.addSubview(label)

        let button = UIButton()
        button.setTitle("Some Button", for: UIControl.State.normal)
        button.setTitleColor(UIColor.blue, for: UIControl.State.normal)
        button.frame = CGRect(x: width * 0.5 - width * 0.8 / 2, y: height * 0.8 - 50 / 2, width: width * 0.8, height: 50)
        view.addSubview(button)

        button.addTarget(self, action: #selector(ButtonViewController.buttonPressed), for: UIControl.Event.touchUpInside)
    }

    @objc func buttonPressed() {
        label.text = "button pressed"
    }
}
```

# 다른 View에서 modal로 띄우기
만약 위에 있는 **UILabel 파트**의 `SomeViewController`를 다른 View (버튼이 하나 있다고 가정)에서 modal로 띄우게 하려면 아래와 같이 해야한다.

만약 띄우고 난 뒤에 어떠한 처리를 하고 싶으면 completion패러미터에 클로저를 넣어준다.

```swift
class OriginViewController: UIViewController {
    @IBAction func buttonPressed(_ sender: UIButton) {
        let someVC = SomeViewController()
        someVC.value = "Hi"

        self.present(someVC, animated: true, completion: nil)
    }
}
```