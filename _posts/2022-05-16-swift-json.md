```swift
let data = Data(input.utf8)
let decoder = JSONDecoder()
if let user = try? decoder.decode(User.self, from: data) {
    print(user.address.street)
}
```

```swift
guard let encoded = try? JSONEncoder().encode(order) else {
    print("Failed to encode order")
    return
}
```
"An app to drink Dolce Gusto's delicious coffee"


It shows a list of coffee sold in Dolce Gusto, and a timer according to each coffee's level.

You can add coffee you like to your favorites and search for the coffee you want.