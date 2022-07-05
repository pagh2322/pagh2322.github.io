---
title: App Launch
description: UIKit에서 앱의 실행 시 일어나는 일들.
categories:
- UIKit
tags:
- UIKit
- iOS
- App
- Launch
- Life Cycle
---

# 개요
앱을 실행하면 일련의 과정들이 순서대로 일어나며, 이 과정은 시스템에서 대부분 자동으로 처리를 한다. 이 순서 과정에서 UIKit은 `app delegate`의 메서드들을 호출하여 사용자 상호 작용을 위해 앱을 준비하고 앱 요구 사항에 맞는 작업을 수행할 수 있다.

아래는 앱 실행 시 발생하는 과정을 하나씩 구분하였다.

[app launch seq](https://docs-assets.developer.apple.com/published/dd1b7996c3/rendered2x-1635759039.png)

# 과정 순서
1. 앱이 실행이 된다.
2. 시스템은 `main()`함수를 실행한다.
3. `main()`함수는 `UIApplicationMain(_:_:_:_:)`을 호출하여 `UIApplication`인스턴스와 `app delegate`인스턴스를 생성한다.
4. UIKit은 앱의 Info.plist 파일 또는 Xcode 프로젝트 에디터의 타겟(target)의 **Custom iOS Target Properties 탭**에서 지정한 기본 스토리보드를 로드한다. 만약 기본 스토리보드를 사용하지 않는 앱은 이 단계를 건너뛴다.(코드로 UI를 구현할 시)
5. UIKit은 `app delegate`애서 `application(_:willFinishLaunchingWithOptions:)`메서드를 호출한다.
6. UIKit은 **상태 복원**을 수행하여 `app delegate`와 앱의 뷰 컨트롤러에서 추가 메서드가 실행될 수 있다.
7. UIKit은 `app delegate`애서 `application(_:didFinishLaunchingWithOptions:)`메서드를 호출한다.

위 과정이 끝나면 시스템에선 `app delegate`혹은 `scene delegate`을 사용하여 앱의 UI를 보여주고 라이프 사이클을 관리한다.

# 참고 링크
[애플 공식 문서](https://developer.apple.com/documentation/uikit/app_and_environment/responding_to_the_launch_of_your_app/about_the_app_launch_sequence)