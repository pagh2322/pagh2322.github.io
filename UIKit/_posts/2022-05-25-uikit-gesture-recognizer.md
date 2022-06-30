---
title: 제스처 인식
description: UIKit에서 제스처를 인식해보자.
categories:
- UIKit
tags:
- UIKit
- iOS
- Gesture
---

# 뷰 설정
화면에 인물의 이미지와 이름이 있는 뷰가 있다.

```swift
class PersonViewController: UIViewController {
    @IBOutlet weak var imageView: UIImageView!
    @IBOutlet weak var nameLabel: UILabel!

    override func viewDidLoad() {
        super.viewDidLoad()
    }
}
```

# 제스처 인식
# isUserInteractionEnabled
사용자가 해당 UI 요소를 인터렉트할 수 있는지 값을 할달할 수 있다. `true`로 줌으로써 사용자의 발생하는 제스처에 반응할 수 있게 해준다.

```swift
self.imageView.isUserInteractionEnabled = true
```

# GestureRecognizer
탭(하나 혹은 멀티 탭)을 인식하는 Recognizer를 생성한다. **target 패러미터**는 **action 패러미터**에 전달된 메소드를 가지고 있는 객체를 넣는다.

```swift
override func viewDidLoad() {
    // ...

    let gestureRecognizer = UITapGestureRecognizer(target: self., action: #selctor(changeImage))

    self.imageView.addGestureRecognizer(gestureRecognizer)
}

@objc func changeImage() {
    self.nameLabel = "Changed"
}
```