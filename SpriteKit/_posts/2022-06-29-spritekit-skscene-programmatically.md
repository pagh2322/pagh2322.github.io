---
title: SKScene 코드로 구현
description: SpriteKit에서 SKScene을 코드로 구현하자.
categories:
- SpriteKit
tags:
- SpriteKit
- iOS
---

# SKView
> A view subclass that renders a SpriteKit scene.
설명처럼 `scene`을 불려오려면 `SKView`으로 감싸서 해야한다.

# 예제
UIKit을 사용한다는 가정 하에 코드를 작성해보겠다. **frame**방법이 아닌 **auto layout**으로 뷰를 그릴 것이다.

## GameView
게임 화면에 대한 뷰이다. 상단 및 하단에 있는 커스텀 툴바와 함께 게임이 진행되는 뷰가 있다.

```swift
class GameView: UIView {
    let topToolBar: TopToolBar = {
        // ...
    }()
    let bottomToolBar: BottomToolBar = {
        // ...
    }()
    let gameView: SKView = {
        return SKView(frame: .zero)
    }()

    override init(frame: CGRect) {
        super.init(frame: frame)

        self.makeSubviews()
        self.makeConstraints()
    }
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }

    func makeSubviews() {
        self.addSubview(self.topToolBar)
        self.addSubview(self.bottomToolBar)
        self.addSubview(self.gameView)
    }

    func makeConstraints() {
        // constraints about top and bottom tool bar..
        self.gameView.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            self.gameView.topAnchor.constraint(equalTo: self.topToolBar.bottomAnchor),
            self.gameView.leadingAnchor.constraint(equalTo: self.leadingAnchor),
            self.gameView.trailingAnchor.constraint(equalTo: self.trailingAnchor),
            self.gameView.bottomAnchor.constraint(equalTo: self.bottomToolBar.topAnchor)
        ])
    }
}
```

## GameViewController
`GameView`에 대한 컨트롤러이다. 위에서 작성한 **GameView**를 해당 컨트롤러의 뷰로 설정하며, 보여줄 **GameScene**을 프로퍼티로 저장한다.

```swift
class GameViewController: UIViewController {
    var scene: GameScene?
    let gameView: GameView = {
        return GameView(frame: .zero)
    }()

    override func viewDidLoad() {
        super.viewDidLoad()

        self.setGameScene()
        self.setGameView()
    }
    override func loadView() {
        super.loadView()
        self.view = self.gameView
    }

    func setGameScene() {
        self.scene = GameScene(size: )
        self.scene?.scaleMode = .aspectFit
    }
    func setGameView() {
        self.gameView.gameView.ignoresSiblingOrder = true // gamescene에 있는 부모-자식 및 형제 노드의 렌더링 관련 프로퍼티이다. false가 기본 값이다
        self.gameView.gameView.presentScene(self.scene)
    }
}
```