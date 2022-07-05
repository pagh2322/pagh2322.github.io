---
title: 근처에 있는 Physiscs body 탐색
description: 일정 범위에 Physiscs body가 있는 지 탐색하자.
categories:
- SpriteKit
tags:
- SpriteKit
- iOS
- Physiscs body
- Physics world
---

# 개요
어떤 상황에선 한 씬(scene)에서 **physiscs body**를 찾아야 할 경우가 생긴다. 예를 들어

* 씬의 특정한 범위 내에 **physiscs body**가 있는 지 탐색
* **physiscs body**가 어떠한 선(닿으면 게임 종료)을 넘었는 지 탐색
* 두 **physiscs body** 사이에 벽과 같은 다른 **physiscs body**가 있는지 탐색

이런 경우엔 충동(collisions)과 접촉(contacts) 시스템을 사용하여 구현할 수 있다. 가령 첫 번째 경우엔 보이지 않은 **physiscs body**를 생성하여 충동 마스크(collision mask)를 구성하여 다른 물체와 충돌이 나지 않게 하고, 접촉 마스크(contact mask)를 구성하여 관심있는 **physiscs body**와 접촉 시 인식되게 할 수 있다.

하지만 이런 방법은 두 번째 경우와 같은 상황에선 구현하기가 쉽지 않다. 이 경우엔 씬의 **physics world**을 사용해야 한다. 이 개념을 통해 **특정한 점**이나 **직사각형**을 교차하는 **광선**을 따라 모든 **physiscs body**를 탐색할 수 있다.

# 씬에서 광선 생성
아래 코드에선 **시선 탐지 시스템**을 구현하였다. 위 예시의 두 번째 상황이라고 볼 수 있으며, 씬의 원점(origin)에서 특정 방향으로 나아가는 광선을 생성하여 근처에 있는 **physiscs body**를 탐색한다.

만약 존재 한다면, 해당 **physiscs body**의 카테고리 마스크(category mask)를 검사하여 공격을 해도 되는 지 확인 후 공격을 실행한다.

```swift
// Detect nearest physics bodies
func isTargetVisibleAtAngle(angle: CGFloat, distance: CGFloat) -> Bool {
    let rayStart = CGPoint.zero
    let rayEnd = CGPoint(x: distance * cos(angle),
                         y: distance * sin(angle))
    
    let body = scene.physicsWorld.body(alongRayStart: rayStart, end: rayEnd)
    
    return body?.categoryBitMask == targetCategory
}

func attackTargetIfVisible() {
    if isTargetVisibleAtAngle(angle: cannon.zRotation, distance: 512) {
        shootCannon()
    }
}
```

# 참고 링크
[애플 공식 문서](https://developer.apple.com/documentation/spritekit/skphysicsworld/searching_the_world_for_physics_bodies)