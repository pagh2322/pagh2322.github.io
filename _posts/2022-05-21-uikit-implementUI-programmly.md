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

# 어떻게
UI요소를 만들고 원하는 frame을 설정한 뒤, 해당 부모 뷰에다 넣어준다는 코드를 작성하면 끝난다

```swift
class SomeViewController: UIViewController {
    var value: String
    
    override func viewDidLoad() {
        super.viewDidLoad()

        view.background = .white

        let label = UILabel()
        label.text = self.value
        label.frame = CGRect(x: 0, y:0, width: 100, height: 50)
        view.addSubview(label)
    }
}
```

# 다른 View에서 modal로 띄우기
만약 위에 있는 `SomeViewController`를 다른 View (버튼이 하나 있다고 가정)에서 modal로 띄우게 하려면 아래와 같이 해야한다.

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