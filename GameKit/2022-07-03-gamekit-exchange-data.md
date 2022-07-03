---
title: Multiplay 기능 구현 - Part 2
description: GameKit을 통해 멀티 플레이 기능을 구현하자.
categories:
- GameKit
tags:
- GameKit
- iOS
- UIKit
- Multiplay
---

# 이어서
저번엔 게임을 생성하여 플레이어를 초대한 뒤에 게임을 실행하는 단계까지 완성을 했다.

이번 글에선 게임에 참가한 유저들 간에 데이터를 송신하는 법에 대해 알아보자.

# GKMatchDelegate
GameKit의 `matchmakerViewController(_:didFind:)`메소드를 호출하여 플레이어는 게임 시작 및 데이터 전달을 할 수 있다.

게임 뷰(화면)의 컨트롤러는 `GKMatchDelegate`를 채택해야 하며 관련된 함수들을 구현해야 한다.

## match(_:player:didChange:) - 연결 상태 관찰
게임을 진행하면서 플레이어의 연결 상태를 관찰하는 메소드이다. 게임을 플레이하는 도중에 다른 플레이어가 게임을 나가면 해당 메소드가 실행이 된다.

```swift
func match(
    _ match: GKMatch,
    player: GKPlayer,
    didChange state: GKPlayerConnectionState
) {    
    print("Available player slots: \(String(match.expectedPlayerCount))")
    
    switch state {
        case .connected:
            // 다른 플레이어와 연결이 됨
        case .disconnected:
            // 다른 플레이어와 연결이 끊김
        default:
            // unknown - 무작위 상태로, 플레이어는 데이터를 받지 못함
    }
}
```

> **expectedPlayerCount**는 연결이 끊긴 플레이어의 수이며, 만약 게임을 나갔다 다시 초대를 받고 들어오면 **match**메소드가 실행이 되며서 **expectedPlayerCount**는 0이 된다.

### match(_:shouldReinviteDisconnectedPlayer:) - 다시 초대
만약 다른 플레이어가 게임을 하다 나가면 다시 초대를 보냄으로써 다시 게임을 진행할 수 있다. 주의해야 할 점은 다시 초대받은 플레이어의 게임 상태는 **처음으로 초기화**가 되기에 해당 상황에 맞는 처리를 해주어야 한다.

```swift
func match(
    _ match: GKMatch,
    shouldReinviteDisconnectedPlayer player: GKPlayer
) -> Bool {
    true
}
```

## send(_:to:dataMode:) 혹은 sendData(toAllPlayers:with:) - 데이터 전달
GameKit에서 플레이어간 데이터를 전달하기 위해 해당 데이터를 `Data`객체화를 시켜 전달한다.

만약 데이터가 **작고** 데이터 전달에 있어서 지연으로 인해 해당 값이 무효화(?)되면 **mode**의 값은 `GKMatch.SendDataMode.unreliable`으로 한다. (예를 들어 해당 플레이어의 포지션 혹은 속도와 같은 실시간으로 값이 자주 변화하는 경우에 사용하면 좋다.) 그 외엔 `GKMatch.SendDataMode.reliable`로 값을 설정한다.

아래 코드에서 **gkMatch**는 해당 뷰 컨트롤러의 프로퍼티이며, 해당 뷰 컨트롤러가 띄우기 전 게임 생성 창에서 준 **`GKMatch`**타입이다.

```swift
// 모든 플레이어에게 값을 전달
func sendMessage(content: String) {
    do {
        // ...
        let data: Data? = content.data(using: .utf8)
        try self.gkMatch?.sendData(toAllPlayers: data!, with: GKMatch.SendDataMode.unreliable)
    } catch {
        return
    }
}

// 특정한 플레이어들에게 값을 전달, 따로 구현은 안 함
func send(
    _ data: Data,
    to players: [GKPlayer],
    dataMode mode: GKMatch.SendDataMode
) throws
```

## match(_:didReceive:fromRemotePlayer:) - 데이터 받기
다른 플레이어가 보낸 데이터를 언팩해서 값을 추출할 수 있으며, 해당 데이터를 보낸 플레이어는 **player**패러미터로 알 수 있다.

```swift
func match(
    _ match: GKMatch, 
    didReceive data: Data, 
    fromRemotePlayer player: GKPlayer
) {
    let content = String(decoding: data, as: UTF8.self)
    // ...
}
```

# 참고 링크
[애플 공식 문서](https://developer.apple.com/documentation/gamekit/exchanging_data_between_players_in_real-time_games)