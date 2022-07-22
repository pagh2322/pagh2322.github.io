---
title: UIImage Pinch Zoom
description: 
categories:
- UIKit
tags:
- UIKit
- Swift
- UIImage
- Pinch
- Zoom
---

# 이미지 확대/축소
현재 많은 앱에서 사용되는 기능인 이미지 확대/축소를 구현하게 되었다. 자주 구현해야 할 기능이라고 생각이 들어서 정리를 해보았다.

# 구현하기
핵심은 먼저 `UIScrollView`를 생성한 뒤, `UIImageView`를 그 안에 집어넣는 방식으로 구현을 한다.

```swift
let imageView: UIImageView = {
    let imageView = UIImageView(image: UIImage(named: "test"))
    imageView.contentMode = .scaleAspectFit
    imageView.translatesAutoresizingMaskIntoConstraints = false
    return imageView
}()
    
let imageViewContainer: UIScrollView = {
    let scrollView = UIScrollView()
    scrollView.translatesAutoresizingMaskIntoConstraints = false
    return scrollView
}()
```

먼저, 위 처럼 View 프로퍼티를 생성한 뒤 전체 view에 addSubview를 해준다.

```swift
func makeSubviews() {
    imageViewContainer.addSubview(imageView)
    view.addSubview(imageViewContainer)
}
```

아까 말했던 것처럼 UIScrollView**안에** UIImageView가 들어간다.

```swift
let imageViewContainerConstraints = [imageViewContainer.topAnchor.constraint(equalTo: self.topAnchor),
                                     imageViewContainer.bottomAnchor.constraint(equalTo: self.bottomAnchor),
                                     imageViewContainer.leadingAnchor.constraint(equalTo: self.leadingAnchor),
                                     imageViewContainer.trailingAnchor.constraint(equalTo: self.trailingAnchor)]
        
let imageViewConstraints = [imageView.leadingAnchor.constraint(equalTo: imageViewContainer.contentLayoutGuide.leadingAnchor),
                            imageView.trailingAnchor.constraint(equalTo: imageViewContainer.contentLayoutGuide.trailingAnchor),
                            imageView.topAnchor.constraint(equalTo: imageViewContainer.contentLayoutGuide.topAnchor),
                            imageView.bottomAnchor.constraint(equalTo: imageViewContainer.contentLayoutGuide.bottomAnchor),
                            imageView.widthAnchor.constraint(equalTo: imageViewContainer.frameLayoutGuide.widthAnchor),
                            imageView.heightAnchor.constraint(equalTo: imageViewContainer.frameLayoutGuide.heightAnchor)]

[imageViewContainerConstraints,
 imageViewConstraints].forEach { NSLayoutConstraint.activate($0) }
```
UIScrollView는 부모 뷰와 맞물리게 제약을 걸어주며, UIImageView는 UIScrollView의 **contentLayoutGuide**에 맞게, 넓이와 높이는 **frameLayoutGuide**에 맞게 걸어준다. 여기서 넓이와 높이는 부모 뷰의 크기에 맞게 설정을 해주어도 되지만, 설정을 아예 안 하면 우리가 원하는 방식대로 동작이 되지 않는다..

```swift
extension ViewController: UIScrollViewDelegate {
    private func setUpScroll() {
        imageViewContainer.minimumZoomScale = 1
        imageViewContainer.maximumZoomScale = 2
        imageViewContainer.delegate = self
    }
    
    func viewForZooming(in scrollView: UIScrollView) -> UIView? {
        return imageView
    }
}
```

여기서 `viewForZooming(in:)`메소드는 아래와 같은 역할은 한다고 나와있다.

> Asks the delegate for the view to scale when zooming is about to occur in the scroll view.

즉 UIScrollView가 줌이 되면 **확대 혹은 축소가 되는 View를 반환**하는 메소드이다. 우리는 UIImageView가 해당되기에 **imageView**를 반환한다.

# 정리
구현의 핵심은 UIImageView를 UIScrollView에 **addSubview**해주는 것과, `viewForZooming(in:)`메소드를 구현하는 것에 있다고 보여진다.