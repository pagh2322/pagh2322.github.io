---
title: API-JSON 사용
description: Swift로 API를 통신하여 JSON을 불러오자.
categories:
- Swift
tags:
- Swift
- SwiftUI
- iOS
- UIKit
- API
- JSON
---

# API
기본적으로 앱을 개발하면서 API를 자주 사용하게 된다. 이번엔 Swift에선 어떻게 통신을 하는지 알아보자. 예시로 날씨를 가져오는 코드를 작성해보자.

# 사용법 - 가져오기
각 단계별로 정리를 해보았다.
## URL 및 URLSession 생성
```swift
func performRequest(urlString: String) {
    guard let url = URL(string: urlString) else { // 주소가 잘못된 주소일 수 있기에 옵셔널 타입이다
        print("Invalid URL")
        return
    }
    let session = URLSession(configuration: .default)
}
```

## 데이터 가져오기
```swift
func doRequest(url: URL, session: URLSession) {
    let task = session.dataTask(with: url, completionHandler: self.handle)
    task.resume()
}

func handle(data: Data?, response: URLResponse?, error: Error?) {
    if error != nil {
        print(error!)
        return
    }

    if let safeData = data {
        self.parseJSON(weather: safeData)
    }
}
```

## 데이터 구조 생성
```swift
struct WeatherData: Decodable {
    let name: String
    let main: Main
    let weather: [Weather]
}

struct Main: Decodable {
    let temp: Double
}

struct Weather: Decodable {
    let description: String
}
```

## JSON Parsing
```swift
func parseJSON(weatherData: Data) {
    let decoder = JSONDecoder()
    do {
        let decodedData = try decoder.decode(WeatherData.self, from: weatherData)
        print(decodedDate.main.temp)
        print(decodedDate.weather[0].description)
    } catch {
        print(error)
    }
}
```

# 총 코드
```swift
struct WeatherManager {
    let weatherURL = "https://api.openweathermap.org/data/..."

    func fetchWeather(cityName: String) {
        let urlString = "\(self.weatherURL)&q=\(cityName)"
        performRequest(urlString: urlString)
    }

    func performRequest(urlString: String) {
        guard let url = URL(string: urlString) else {
            print("Invalid URL")
            return
        }
        let session = URLSession(configuration: .default)
    }

    func doRequest(url: URL, session: URLSession) {
        let task = session.dataTask(with: url, completionHandler: self.handle)
        task.resume()
    }

    func handle(data: Data?, response: URLResponse?, error: Error?) {
        if error != nil {
            print(error!)
            return
        }

        if let safeData = data {
            self.parseJSON(weather: safeData)
        }
    }
}

struct WeatherData: Decodable {
    let name: String
    let main: Main
    let weather: [Weather]
}

struct Main: Decodable {
    let temp: Double
}

struct Weather: Decodable {
    let description: String
}
```