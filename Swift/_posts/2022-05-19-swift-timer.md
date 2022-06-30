---
title: Timer
description: 타이머에 대해 알아보자.
categories:
- Swift
tags:
- iOS
- Swift
- Timer
---

# Timer
혼자서 타이머 관련 앱을 간단하게 만들어 보면서 `Timer`객체를 사용을 해보았다. 당시엔 잘 모르고 썼던 거 같아 다시 한 번 정리를 해보았다.

`Thread.sleep(forTimerInterval:)`함수를 통해 구현을 하기엔 너무 부적합하다. 쓰레드를 재우는 방법이 독이 될 수 있기 때문.

# 화면 설정
화면에 있는 버튼을 누르면 위에 있는 라벨에 시간이 갱신이 되도록 하였다.

```swift
class TimerVC: UIViewController {
    @IBOutlet weak var timerLabel: UILabel!

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    @IBAction func buttonPressed(_ sender: UIButton) {
    }
}
```

# Timer 생성 및 schedule
`timeInterval`: 원하는 시간(초) 간격(**Int**)
`target`: selctor에 전달할 메소드를 가지고 있는 객체
`selector`: 매 간격마다 실행될 메소드
`userInfo`: 유저의 정보
`repeats`: 반복 실핼할 것인지(**Bool**)

```swift
var timer = Timer()
var counter = 0

override func viewDidLoad() {
    super.viewDidLoad()

    counter = 10
    timerLabel.text = "Time: \(counter)"
}

@IBAction func buttonPressed(_ sender: UIButton) {
    timer = Timer.scheduledTimer(timeInterval: , target: self selector: #selector(timerMethod), userInfo: nil, repeats: true)
}
    
@objc func timerMethod() {}
    timerLabel.text = "Time: \(counter)"
    counter -= 1

    if counter == 0 {
        timer.invalidate()
        timeLabel.text = "Over"
    }
}
```