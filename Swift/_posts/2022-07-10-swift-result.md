---
title: Result
description: Result타입으로 에러를 처리하자.
categories:
- Swift
tags:
- Swift
- Result
- Error
---

# Result
앱에서 API 호출과 같은 에러를 반환할 수 있는 작업을 실행하면 관련 에러를 처리해주는 로직이 필요하다.

이러한 반환되는 상황에서 반환 타입은 **Result**이며 success와 failure 2가지 케이스가 있다.

# 사용법
process함수의 반환 타입은 **Result<Int, NumberError>**이며, 에러(NumberError)가 아닌 경우엔 Int를 반환한다.

```swift
enum NumberError: Error {
    case negativeNumber
    case evenNumber
}

func process(number: Int) Result<Int, NumberError> {
    guard number >= 0 else {
        throw .failure(.negativeNumber)
    }

    guard !number.isMultiple(of: 2) else {
        throw .failure(.evenNumber)
    }

    return .success(number * 2)
}

let result = Result { try process(number: 1)}
switch result {
case .success(let data):
    print(data)
case .failure(let error):
    print(error.localizedDescription)
}
```

위 처럼 에러를 처리하는 시점이 함수를 호출하는 시점이 아닌 **작업 결과를 사용하는 시점**인 상황으로 바뀐 경우를 `Delayed Error Handling`이라고 부른다.

# 참고 링크
[[Mastering Swift] Result Type #1](https://www.youtube.com/watch?v=dpInY4cyxvA&list=PLziSvys01OeklWvgtiF9tZVPA-awfOzUq&index=5)