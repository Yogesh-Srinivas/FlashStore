# Swift

---

### References:
1. https://medium.com/@PavloShadov/best-resources-for-advanced-ios-developer-swift-ade30374593d
2. [Swift Tips and Tricks](https://www.hackingwithswift.com/articles/106/10-quick-swift-tips)
3. [vincent Tips](https://www.swiftwithvincent.com/tips)
4. [sundell Tips](https://github.com/JohnSundell/SwiftTips)

# “do as the locals do”

### Tips
> - Use `first(where: )` instead of `filter().first`
> - Use `contains(where: )` instead of `filter().isEmpty`
> - Use `enumerated()`
> - Use `for` & `where`
> - Use `forEach` instead of `for in`
> - Use computed property instead of method
> - Protocol extensions can provide default property values - Ref 2
> - New in Swift 4.2 is an allSatisfy() method: give it a condition closure to run, and if all elements return true when passed into that closure then the allSatisfy() call will return true.
> - Use destructuring to manipulate tuples
> - Adding and subtracting with wrapping using overflow operators
> - Public getter, private setter: Swift has a great middle ground: **public private(set)**. This lets us mark a property as being open for reading but closed for writing, which in the case of our bank means anyone can read the bank’s address but only the bank itself can change it.
> - Class properties in Swift can be created using two keywords: **static and class**. They both make the property shared across all instances of a class, but static implies final so that it cannot be overridden in subclasses.
> - **numericCast()** is specifically designed for safely converting numeric types, ensuring that the conversion doesn't result in loss of precision or overflow. It performs compile-time checks and guarantees that the conversion is safe, providing a way to handle the conversions in a more controlled and explicit manner.
> - switch used as an expression
> - Use Variadic operators
> - Use opaque Arguments ("Some" for Generics)
> - USe dump instead print, provides more info
> - Use addAction clouse for UIButton instead addTarget
> - You don't need to unwrap an optional value if you are going to compare it to an exact value.
>   ```swift
>   //wrong
>    var someOptionalString: String?
>    if let stringValue = someOptionalString, stringValue.isEmpty { ... }
>   //right
>    var someOptionalString: String?
>    if stringValue?.isEmpty == false { ... }
>   ```

### Best Practices
> - Use value types over reference types
> - Use higher-order functions - Swift supports higher-order functions like map, filter, and reduce to make your code more expressive and functional
> - Use Generics for Flexible Code
> - Adding support for Apple Pencil double-taps (its easy)

## Tips
**1. Swift's pattern matching capabilities are so powerful! Two enum cases with associated values can even be matched and handled by the same switch case - which is super useful when handling state changes with similar data.**
```swift
enum DownloadState {
    case inProgress(progress: Double)
    case paused(progress: Double)
    case cancelled
    case finished(Data)
}

func downloadStateDidChange(to state: DownloadState) {
    switch state {
    case .inProgress(let progress), .paused(let progress):
        updateProgressView(with: progress)
    case .cancelled:
        showCancelledMessage()
    case .finished(let data):
        process(data)
    }
}
```

**2. Here I'm using tuples to create a lightweight hierarchy for my data, giving me a nice structure without having to introduce any additional types.**
```swift
struct CodeSegment {
    var tokens: (
        previous: String?,
        current: String
    )

    var delimiters: (
        previous: Character?
        next: Character?
    )
}

handle(segment.tokens.current)
```
**3. UserDefaults is a lot more powerful than what it first might seem like. Not only can it store more complex values (like dates & dictionaries) and parse command line arguments - it also enables easy sharing of settings & lightweight data between apps in the same App Group.**
```swift
let sharedDefaults = UserDefaults(suiteName: "my-app-group")!
let useDarkMode = sharedDefaults.bool(forKey: "dark-mode")

// This value is put into the shared suite.
sharedDefaults.set(true, forKey: "dark-mode")

// If you want to treat the shared settings as read-only (and add
// local overrides on top of them), you can simply add the shared
// suite to the standard UserDefaults.
let combinedDefaults = UserDefaults.standard
combinedDefaults.addSuite(named: "my-app-group")

// This value is a local override, not added to the shared suite.
combinedDefaults.set(true, forKey: "app-specific-override")
```
**4. One of the key features it brings is conditional conformances, which lets you have a type only conform to a protocol under certain constraints.**
```swift
protocol UnboxTransformable {
    associatedtype RawValue

    static func transform(_ value: RawValue) throws -> Self?
}

extension Array: UnboxTransformable where Element: UnboxTransformable {
    typealias RawValue = [Element.RawValue]

    static func transform(_ value: RawValue) throws -> [Element]? {
        return try value.compactMap(Element.transform)
    }
}
```
**5. Swift's & operator is awesome! Not only can you use it to compose protocols, you can compose other types too! Very useful if you want to hide concrete types & implementation details.**
```swift
protocol LoadableFromURL {
    func load(from url: URL)
}

class ContentViewController: UIViewController, LoadableFromURL {
    func load(from url: URL) {
        ...
    }
}

class ViewControllerFactory {
    func makeContentViewController() -> UIViewController & LoadableFromURL {
        return ContentViewController()
    }
}
```
**6. Combining lazily evaluated sequences with builder pattern-like properties can lead to some pretty sweet APIs for configurable sequences in Swift.
Also useful for queries & other things you "build up" and then execute.**
```swift
// Extension adding builder pattern-like properties that return
// a new sequence value with the given configuration applied
extension FileSequence {
    var recursive: FileSequence {
        var sequence = self
        sequence.isRecursive = true
        return sequence
    }

    var includingHidden: FileSequence {
        var sequence = self
        sequence.includeHidden = true
        return sequence
    }
}

// BEFORE

let files = folder.makeFileSequence(recursive: true, includeHidden: true)

// AFTER

let files = folder.files.recursive.includingHidden
```


