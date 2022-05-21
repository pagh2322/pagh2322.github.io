
# 데이터 생성
```swift
struct Response: Codable {
    var results: [Result]
}

struct Result: Codable {
    var trackId: Int
    var trackName: String
    var collectionName: String
}
```

# URL 생성
```swift
guard let url = URL(string: "https://itunes.apple.com/search?term=taylor+swift&entity=song") else {
    print("Invalid URL")
    return
}
```

# 데이터 Fetch(Get)
```swift
do {
    let (data, _) = try await URLSession.shared.data(from: url) // Data 객체를 반환한다

    if let decodedResponse = try? JSONDecoder().decode(Response.self, from: data) {
        results = decodedResponse.results
    }
    // more code to come
} catch {
    print("Invalid data")
}
```

# 데이터 Request(Post)
```swift
guard let encoded = try? JSONEncoder().encode(order) else {
    print("Failed to encode order")
    return
}

let url = URL(string: "https://reqres.in/api/cupcakes")!
var request = URLRequest(url: url)
request.setValue("application/json", forHTTPHeaderField: "Content-Type")
request.httpMethod = "POST"

do {
    let (data, _) = try await URLSession.shared.upload(for: request, from: encoded)
    // handle the result    
} catch {
    print("Checkout failed.")
}
```