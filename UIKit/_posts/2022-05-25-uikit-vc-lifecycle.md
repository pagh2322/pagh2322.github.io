---
title: ViewController LifeCycle
description: UIKit에서 ViewController의 LifeCycle.
categories:
- UIKit
tags:
- UIKit
- iOS
- ViewController
- LifeCycle
---

# 생명 주기
순서대로 ViewController가 생성이 되면 실행이 되는 함수들을 나열을 해보았다.

# viewDidLoad
뷰가 생성이 될 때 **딱 한 번** 실행이 된다.

# viewWillAppear
뷰가 보이기 **직전**에 실행이 된다. 네비게이션 바 숨김 등, 특정한 UI를 숨기거나 보여주는 코드를 작성한다.

# viewDidAppear
뷰가 이미 **보여져** 있는 상태일때 실행이 된다. 타이머 실행 등, 해당 뷰가 보여지자 마자 실행이 될 코드를 작성한다.

# viewWillDisappear
뷰가 곧 사라질 때 실행이 된다.(모달뷰에서 아래로 **스크롤을 시작하는 순간** 바로 실행이 됨)

# viewDidDisappear
뷰가 사라지면 실행이 된다.(정확하게는 유저가 해당 뷰를 못 보는 상황일 때, 네비게이션 이동)