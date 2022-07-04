---
title: SpriteKit에서의 Node
description: SpriteKit에서의 Node란 무엇일까.
categories:
- SpriteKit
tags:
- SpriteKit
- iOS
---

# SpriteKit에서의 Node
SpriteKit를 사용하면서 화면에 있는 각 원소들은 **하나의 노드(Node)**라고 볼 수 있다.

노드는 보여지는 **원소**(예 라벨, 이미지 등)이거나 **다른 노드들의 컨테이너**이다.

우리는 계층적 형태로 노드를 서로 **나란히** 그리고 위에 **추가하여** SpriteKit로 구현된 화면을 설정할 수 있다. 이러한 구조를 총칭하여 **노드 트리** 또는 **노드 계층**이라고 한다. 

> 노드들은 기존 상위 뷰와 하위 뷰가 작동하는 방식과 유사하게 계층적으로 노드 트리로 구성된다.

## 시각적 및 비시각적 노드
`SKNode`, `SKReferenceNode` 그리고 `SKCameraNode` 이 3가지 노드는 기본적인 비시각적 노드이다. 시각적 노드는 `SKSpriteNode`, `SKShapeNode` 등이 있다.

### SKNode
SKNode는 다른 노드들의 컨테이너 역할을 할 수 있다. 가령 여러개의 노드들이 SKNode의 자식이 되어 scene에서 같이 움직이게 할 수 있다. 왜냐하면 자식 노드들은 부모의 속성을 상속받아 부모의 위치값이 변화하면 자식들에게도 전파가 된다.

SKNode는 비시각적 노드이기에 스스로의 화면을 그릴 수 없다.

# 기본 속성
## SKScene - 화면 노드
위에 나와 있지만 노드들은 계층적으로 노드 트리로 구성이 된다. 노드 트리는 `SKScene`이라는 **화면(씬) 노드**가 루트 노드가 되며 다른 노드들은 후손 노드가 된다.

화면 노드에선 노드의 작업을 처리하고, 물리 엔진을 시뮬레이션하며, 노드 트리의 내용을 렌더링하는 애니메이션 루프를 실행한다.

자식 노드가 노드 트리에 추가되면, 해당 노드의 `zPosition`프로퍼티 값을 설정하면서 부모의 좌표 값에서 위치를 잡을 수 있다. 노드의 좌표 값은 `xScale`, `yScale` 그리고 `zRotation`의 값을 바꿔 변경할 수 있다.

## 기타
모든 노드는 사용자 상호 작용에 직접 응답할 수 있는 **응답 객체**이다.

트리에 위치한 노드는 **action**을 통해 노드의 속성을 애니메이션화하고, 노드를 추가 또는 제거하며, 소리를 재생하거나, 다른 사용자 지정 작업을 수행하는 데 사용된다.

노드는 물리적 범위와 속성을 가지고 있어 시뮬레이션을 돌리면서 관련 값에 대응되는 변화를 볼 수 있다.(예를 들어 다른 노드와 부딪히면 튕기는 현상)

마지막으로 노드는 다른 노드 혹은 화면의 어떤 위치간의 관계에 제약을 걸 수 있다. 화면이 바뀌면 해당 제약이 자동으로 적용이 된다.(예를 들어 팔 관절과 같은 기능)

# 참고 링크
[애플 공식 문서](https://developer.apple.com/documentation/spritekit/nodes_for_scene_building/using_base_nodes_to_lay_out_spritekit_content)