---
title: Multiplay 기능 구현 - Part 1
description: GameKit을 통해 멀티 플레이 기능을 구현하자.
categories:
- GameKit
tags:
- GameKit
- iOS
- UIKit
- Multiplay
---

# Multiplay
현재 필자가 만드려는 게임에서 멀티 플레이 기능이 필수적으로 들어가야 했다. 그렇기에 구현을 하면서 정리를 해보겠다.

# 화면 인테페이스
다른 사용자들을 초대를 하면서 대기하는 방의 화면은 기본적으로 아래와 같이 보여진다. 만약 마음에 안 드면 직접 구현을 할 수 있다.

기본적으로 제공되는 화면에서 사용자들은 몇 명이 준비중이고, 몇 명이 초대를 받았는지에 대한 정보들을 볼 수 있다. 만약 게임 플레이가 가능한 **최소한의 플레이어**가 들어오면 게임 시작 버튼이 **활성화**가 되어 게임을 시작할 수 있다.

[대기 화면](https://docs-assets.developer.apple.com/published/1454cd0dac/rendered2x-1621619422.png)

# 프로토콜 채택
먼저 `GKLocalPlayerListener`와 `GKMatchmakerViewControllerDelegate`를 채택한다.

# GKMatchmakerViewControllerDelegate
게임 생성 창과 관련한 함수들을 구현한다.

## match request 생성
먼저 `GKMatchRequest`객체를 생성하여 관련 설정 값(최소, 최대 플레이어 수 등)을 설정한다.

```swift
let request = GKMatchRequest()
request.minPlayers = 2
request.maxPlayers = 12
```

**`maxPlayersAllowedForMatch(of:)`**를 통해 최대 가능한 플레이어의 수를 구할 수 있다.

> 최대 가능한 플레이어 수는 16명이다.

## 방 화면 띄우기
match request를 생성한 뒤에 해당 요청이 적용된 방의 화면을 띄운다. 이 화면에서 사용자는 다른 사용자를 초대하거나, 게임을 시작할 수 있다.

**GKMatchmakerViewController**의 delegate(**GKMatchmakerViewControllerDelegate**)를 위임한 뒤 게임 설정(ar게임이면 같은 방에 위치한 플레이어만 초대 등)의 값을 할당하고 해당 뷰를 띄운다.

```swift
let vc = GKMatchmakerViewController(matchRequest: request)
vc?.matchmakerDelegate = self
vc?.matchmakingMode = .default
self.present(vc!, animated: true)
```

> matchmakingMode의 값은 `nearbyOnly`, `automatchOnly`, `inviteOnly` 등이 있다.

### 최소 인원수로 플레이
iOS 15.0 버전 이후에선 최소의 인원수로도 플레이가 가능하게 하려면 아래처럼 `canStartWithMinimumPlayers`의 값을 true로 한다.

```swift
vc?.canStartWithMinimumPlayers = true
```

# GKLocalPlayerListener
사용자 초대와 관련한 함수를 구현한다.

## 준비
먼저 `GKInviteEventListener`프로토콜의 콜백을 받기 위해 아래의 구문을 실행한다. `self`는 해당 컨트롤러를 뜻한다.

```swift
GKLocalPlayer.local.register(self)
```

## 초대 수락하기
Game Center는 플레이어들에게 비동기로 초대를 보낸다. 만약 해당 플레이어가 초대를 수락하면 `player(_:didAccept:)`메소드를 호출한다.

호출을 하면 게임 생성 창이 **초대 상태로** 뜨면서 초대된 플레이어는 현재 초대 상태를 보면서 게임 시작을 기다린다. 게임 생성 창은 루트 컨트롤러를 통해 띄우게 하여 만약 플레이어가 방에서 나가면 앱의 첫 화면을 보게 한다.

```swift
func player(
    _ player: GKPlayer, 
    didAccept invite: GKInvite
) {
    // Present the view controller in the invitation state.
    let viewController = GKMatchmakerViewController(invite: invite)
    viewController?.matchmakerDelegate = self
    let rootViewController = UIApplication.shared.windows.first!.rootViewController
    rootViewController?.present(viewController!, animated: true, completion: nil)
}
```

# 게임 시작
모든 플레이어가 초대를 수락하면 GameKit은 게임에 참가한 플레이어들의 `matchmakerViewController(_:didFind:)`메소드를 호출한다.

해당 메소드안에서 게임 생성 창을 내리고 게임 화면을 띄워 게임을 시작한다. 부모 컨트롤러에선 게임 뷰(화면)의 컨트롤러를 프로퍼티로 가지고 있기에 **match의 delegate**를 해당 게임 뷰 컨트롤러로 한다. 이를 통해 게임 뷰 컨트롤러에선 플레이어 리스트를 알 수 있으며 플레이어간의 데이터를 전송할 수 있다.

```swift
func matchmakerViewController(
        _ viewController: GKMatchmakerViewController,
        didFind match: GKMatch
) {
    viewController.dismiss(animated: true)
    self.gameViewController = GameViewController(gkMatch: match)
    match.delegate = self.gameViewController

    self.gameViewController?.modalTransitionStyle = .crossDissolve
    self.gameViewController?.modalPresentationStyle = .fullScreen
    self.present(gameViewController!, animated: true)
}
```

# 기타 함수들
## matchmakerViewControllerWasCancelled(_:)
만약 플레이어가 어떠한 이유로 인해 게임 생성 창을 닫게 되면 실행이 되는 메소드이다.

```swift
func matchmakerViewControllerWasCancelled(
    _ viewController: GKMatchmakerViewController
) {
    // Dismiss the view controller.
    viewController.dismiss(animated:true)
}
```

## matchmakerViewController(_:didFailWithError:)
발생 가능한 에러들을 처리하는 메소드이다.

```swift
func matchmakerViewController(
    _ viewController: GKMatchmakerViewController,
    didFailWithError error: Error
) {
        
}
```

발생 가능한 에러들은 [관련 링크](https://developer.apple.com/documentation/gamekit/gkerror)를 참고하면 된다.

# 랜덤 매칭
많은 게임에서 플레이어는 방을 생성해서 초대하고 플레이를 한다기 보단 랜덤으로 만난 플레이어와 게임을 한다. GameKit에서도 랜덤 매칭을 할 수 있으며, 이 경우엔 게임 생성 창을 띄우지 않는다.

`match request`를 GKMatchmaker의 싱글톤 객체에게 `findMatch(for:withCompletionHandler:)` 메소드로 전달한다. 만약 상대방을 찾게 되면 **completion handler 클로저**를 실행하여 게임을 시작할 수 있다.

```swift
let request = GKMatchRequest()
request.minPlayers = 2
request.maxPlayers = 12

GKMatchmaker.shared().findMatch(
    for: request, 
    withCompletionHandler: { (match: GKMatch?, error: Error?) -> Void in
        if match != nil {
            // Set the match delegate.
            match?.delegate = self.gameViewController

            // Start the game with the players in the match.
            self.gameViewController = GameViewController(gkMatch: match)
            self.gameViewController?.modalTransitionStyle = .crossDissolve
            self.gameViewController?.modalPresentationStyle = .fullScreen
            self.present(self.gameViewController!, animated: true)
        }
        if error != nil {
            // Handle the error that occurred finding a match.
        }
})
```

# 마지막으로
컨트롤러가 GKLocalPlayerListener 프로토콜를 채택하면 플레이어들 간의 데이터 송신 및 초대 요청 등에 대해 처리할 수 있다.

# 참고 링크
[애플 공식 문서](https://developer.apple.com/documentation/gamekit/finding_multiple_players_for_a_game)