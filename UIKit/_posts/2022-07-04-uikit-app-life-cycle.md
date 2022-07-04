---
title: App Life Cycle
description: UIKit에서 앱의 라이프 사이클.
categories:
- UIKit
tags:
- UIKit
- iOS
- App
- Life Cycle
---

# 앞서서
이 글은 애플의 [Managing Your App's Life Cycle](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)를 공부를 위한 번역글입니다.

# 개요
> 앱이 **전경**(foreground) 혹은 **후경**(background)에 따른 시스템 응답과, 시스템 관련 중요한 이벤트 처리

앱의 현재 상태에 따라 할 수 있는 것과 할 수 없는 것이 결정된다. 예를 들어, 현재 **전경**인 앱은 유저가 집중을 하고 있기에 해당 앱은 시스템 리소스보다 우선권이 있다. 반대로 **후경**인 앱은 무조건 **가능한 최소한**의 작업만 하거나 더 좋게는 아무것도 안 한다. 왜냐하면 화면에 안 보이기 때문이다.

앱의 상태가 변화함에 따라, 우리는 해당 앱의 활동을 대응하여 조절해야 한다.

앱의 상태가 변화하면, **UIKit**에서는 `알맞는 delegate object`의 함수를 호출한다.

* iOS13 이후 버전: `UISceneDelegate`객체를 사용하여 **scene-기반 앱**에서 라이프 사이클 이벤트에 대응한다.
* iOS 12 이전 버전: `UIApplicationDelegate`객체를 사용하여 라이프 사이클 이벤트에 대응한다.

> iOS13 이후 버전에서 iOS는 항상 scene delegates를 사용한다. iOS 12 이전 버전에선 app delegate를 사용한다.

# scene-기반 라이프 사이클 이벤트 대응
만약 해당 앱이 scene을 지원하면, UIKit는 독립된 라이프 사이클 이벤트를 각각(scene, app) 지원한다.

## scene
scene은 디바이스에서 실행되는(보여주는) 앱의 UI의 어떠한 객체를 말한다. 유저는 각 앱에서 여러개의 scene을 생성할 수 있으며 따로따로 보여주거나 숨길 수 있다.

각 scene은 스스로의 라이프 사이클이 있고, 서로 다른 실행 상태에 있을 수 있다. 예를 들어, 어떠한 scene이 전경에 있는 상황에서 다른 scene들은 후경에 있거나 정지되어 있다.

아래는 scene의 상태 전환을 보여준다.

![scene state](/images/uikit/app-life-cycle/scene-state.png)

사용자나 시스템이 해당 앱에서 새로운 scene을 요구하면, UIKit는 거기에 대응하여 생성한 뒤 어디에도 속하지 않은(unattached) 상태로 둔다.

**유저가 요청한** scene이라면 재빠르게 전경에 위치시켜 화면에 보이게 한다. 하지만 **시스템이 요청한** scene이라면 후경에 위치시켜 이벤트를 실행하게 한다(예: 위치 이벤트).

유저가 앱의 UI를 닫게 되면(dismiss), UIKit는 연관된 scene을 후경에 위치시켜 최종적으론 정지 상태(suspended)로 전환시킨다.

UIKit은 언제든지 후경 또는 정지된 scene의 연결을 끊어서 해당 리소스를 회수하여 해당 scene을 무소속 상태(unattached)로 되돌릴 수 있다.

### scene 전환을 통한 작업 수행
* UIKit가 scene을 앱에 연결할 때 scene의 초기 UI를 구성하고 scene에 필요한 데이터를 로드한다.
* 전경 활성(foreground-active) 상태로 전환할 때 UI를 구성하고 사용자와 상호 작용할 준비를 한다.
* 전경 활성 상태에서 떠날 시, 데이터를 저장하고 앱의 실행을 멈춘다.
* 후경 상태에 들어가면서 중요한 작업을 마치고, 최대한 많은 메모리를 해제시킨다. 또한 앱의 스냅샷을 준비한다.
* scene 연결이 끊길 때 scene과 연결된 모든 공유 리소스를 정리한다.
* scene 관련 이벤트 외에도 `UIApplicationDelegate`객체를 사용하여 앱이 시작하는 상황에 응답해야 한다.

# app-기반 라이프 사이클 이벤트 대응
iOS 12 이전 버전, 그리고 app에서는 scene을 지원하지 않는다. UIKit은 모든 라이프 사이클 이벤트를 `UIApplicationDelegate`객체에게 전달한다. app delegate는 앱의 모든 창(windows)들을 관리한다. 결국, app 상태 전환은 앱의 전체적인 UI에 영향을 끼친다.

아래는 app delegate와 관련된 상태 전환을 보여준다.

![app state](/images/uikit/app-life-cycle/app-state.png)

실행 후 시스템은 UI가 화면에 나타날 것인지에 따라 앱을 활성 혹은 백그라운드 상태로 둔다. 만약 화면에 나오게 실행을 하면 시스템은 자동으로 app을 활성 상태로 전환시킨다. 그 후, 앱이 종료될 때까지 상태는 활성과 백그라운드 간 계속해서 변화한다.

### app 전환을 통한 작업 수행
* 실행 시 앱의 데이터 구조와 UI를 초기화한다.
* 활성화 시 UI 구성을 완료하고 사용자와 상호 작용할 준비를 한다.
* 비활성화 시 데이터를 저장하고 앱의 실행을 멈춘다.
* 후경 상태에 들어가면서 중요한 작업을 마치고, 최대한 많은 메모리를 해제시킨다. 또한 앱의 스냅샷을 준비한다.
* 종료 시, 모든 작업을 즉시 멈추고 모든 공유 리소스를 해제한다.

# 다른 중요한 이벤트 대응
앱은 아래와 같은 이벤트를 처리할 수 있게 준비가 되어 있어야 한다. `UIApplicationDelegate`객체를 사용하면 대부분의 이벤트들을 처리할 수 있다.

1. 메모리 경고 - 앱의 메모리 사용량이 매우 큰 경우
2. 보호된 데이터가 접근 가능/불가능 - 유저가 디바이스를 잠그거나 해제하는 경우
3. 넘겨주는(Handoff) 작업 - `NSUserActivity`객체가 처리되어야 하는 경우
4. 시간 변화 - 여러 시간의 변화 있을 경우(예: 통신사에서 시간 업데이트를 보냄)
5. URL 열기 - 앱이 리소스를 열어야 하는 경우

# 참고 링크
[애플 공식 문서](https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle)