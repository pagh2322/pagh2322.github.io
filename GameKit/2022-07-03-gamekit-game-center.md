---
title: Game Center 연동 및 로컬 플레이어 계정 체크
description: Game Center을 연동한 뒤 로컬 플레이어의 계정을 체크한다.
categories:
- GameKit
tags:
- GameKit
- iOS
- UIKit
- Game Center
---

# GameKit
많은 iOS 게임에서 `GameKit`를 통해 여러 기능(예: 랭킹, 멀티 등)을 구현할 수 있다.

필자 역시 **다이노 퍼즐링**이라는 게임을 만드면서 연동하여 사용해보았고, 이에 정리를 해보려한다. 개인적으로 애플 공식 문서가 정리를 잘 하여 주로 참고를 했다.

> 랭킹은 `Leaderboard`라는 것을 통해 구현을 한다. 이에 대한 내용 역시 따로 정리를 해보겠다.

# Game Center 연동
이 글에 다룰 내용이며, GameKit에서 제공하는 기능들을 사용하기 위한 준비 단계라고 생각하면 될 듯 하다.

아래 사진처럼 해당 프로젝트로 들어가 **더하기 버튼**(Signing & Capabilities 밑)을 눌러 **`Game Center`**를 추가해준다.

![](/images/gamekit/project.png)

# 로컬 플레이어 계정 체크
기본적으로 게임 앱이 실행이 되면 로컬 플레이어의 정보를 불러온다. **authenticateUser**라는 메소드를 생성하여 첫 화면의 컨트롤러에서 실행이 되게 한다.

```swift
func authenticateUser() {
    // authenticateHandler will give a view controller we need to present it
    GKLocalPlayer.local.authenticateHandler = { viewController, error in
        if let viewController = viewController {
            // Present the view controller so the player can sign in.
            self.present(viewController, animated: true)
            return
        }
        if error != nil {
            // Player could not be authenticated.
            // Disable Game Center in the game.
            return
        }
        
        // Player was successfully authenticated.
        // Check if there are any player restrictions before starting the game.
        
        if GKLocalPlayer.local.isUnderage {
            // Hide explicit game content.
        }
        
        if GKLocalPlayer.local.isMultiplayerGamingRestricted {
            // Disable multiplayer game features.
        }
        
        if GKLocalPlayer.local.isPersonalizedCommunicationRestricted {
            // Disable in game communication UI.
        }
        
        // Perform any other configurations as needed (for example, access point).
    }
}
```

첫 번째 if 구문에서는 사용자가 **로그인을 안 했을시** 로그인 화면을 띄우게 한다.

마지막 if 구문이 실행이 되면 사용자는 **멀티 플레이에서 필요한 기능**(음성 채팅, 유저간 데이터 송신)들을 실행할 수 없다.

# 참고 링크
[애플 공식 문서](https://developer.apple.com/documentation/gamekit)