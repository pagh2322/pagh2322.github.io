---
title: ViewController Life Cycle
description: UIKit에서 ViewController의 라이프 사이클.
categories:
- UIKit
tags:
- UIKit
- iOS
- ViewController
- Life Cycle
---

# 생명 주기
![vc-lifcyc](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F2613D13C58C64DE32C838B)

이 처럼, ViewController는 위와 같은 순서로 함수가 실행이 된다. 개인적으로 애플이 작성한 함수 이름들을 좋아하는 점은 will, did와 같은 단어로 언제 발생이 될 거라 직관적으로 생각이 들게끔 한다.

# viewDidLoad
뷰가 생성이 될 때 **딱 한 번** 실행이 된다. 새로운 View를 만들면서 **Cocoa Touch Class**로 파일을 생성하게 되면 자동으로 코드가 작성되는 메소드이다.

> Called after the controller's view is loaded into memory. This method is called after the view controller has loaded its view hierarchy into memory. This method is called regardless of whether the view hierarchy was loaded from a nib file or created programmatically in the loadView() method. You usually override this method to perform additional initialization on views that were loaded from nib files.

즉, 컨트롤러에 뷰가 메모리에 로드가 된 후에 시스템에 의해 자동으로 호출이 된다고 한다. 주로 리소스를 초기화, 초기 화면 구성 등에서 사용된다. 한 번만 실행해야 하는 초기화 코드를 여기에다 작성하면 된다.

# viewWillAppear
뷰가 보이기 **직전**에 실행이 된다. 어떠한 화면에서 잠시 다른 화면에 갔다가 **뒤로 가기**를 실행하면 이 함수가 **다시** 호출이 된다.

네비게이션 바 숨김 등, 특정한 UI를 숨기거나 보여주는 코드를 작성한다.

# viewDidAppear
뷰가 이미 **보여져** 있는 상태일때 실행이 된다. 위에 있는 `viewWillAppear`와 비슷하다.

타이머 실행 등, 해당 뷰가 보여지자 마자 실행이 될 코드를 작성한다.

# viewWillDisappear
뷰가 곧 사라질 때 실행이 된다.(모달뷰에서 아래로 **스크롤을 시작하는 순간** 바로 실행이 됨)

# viewDidDisappear
뷰가 사라지면 실행이 된다.(정확하게는 유저가 해당 뷰를 못 보는 상황일 때, 네비게이션 이동)

# 헷갈릴 수 있는 것
![vc-lif](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile28.uf.tistory.com%2Fimage%2F2115413A58C6726513849E)

2번째 화면이 보여지기 직전에 viewWillAppear함수를 실행한 뒤, 1번째 화면의 viewDidDisappear함수가 실행하는 것을 잊지 말자.

# loadView
애플에서 직접 호출하는 것을 권장하지 않는 이 함수는 무엇일까.(개인적으로 자주 사용했던 함수라 무서웠다..)

> Creates the view that the controller manages. If you want to perform any additional initialization of your views, do so in the viewDidLoad() method.

해당 컨트롤러가 관리하는 뷰를 만든다. loadView에서 뷰를 만든 뒤 메모리에 올리고 나서 `viewDidLoad`함수가 호출이 된다.

# 문득 든 생각
> You can override this method in order to create your views manually. If you choose to do so, assign the root view of your view hierarchy to the view property. The views you create should be unique instances and should not be shared with any other view controller object. Your custom implementation of this method should not call super.

내가 하는 방식은 주로 아래 코드 처럼 해당 컨트롤러의 뷰를 하나의 UIView를 상속받은 커스텀 뷰를 프로퍼티로 만들어 할당하는 방식인데, 이 경우가 애플에서 설명하는 경우인 듯한 느낌을 받았었다.

```swift
class ViewController: UIViewController {
    let customView = CustomView()

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    override func loadView() {
        super.loadView()
        self.view = customView
    }
}
```

# 참고 링크
[Zedd0202- iOS) View Controller의 생명주기(Life-Cycle)](https://zeddios.tistory.com/43)