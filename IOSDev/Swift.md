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
**7. Unlike static properties, class properties can be overridden by subclasses (however, they can't be stored, only computed).**
```swift
class TableViewCell: UITableViewCell {
    class var preferredHeight: CGFloat { return 60 }
}

class TallTableViewCell: TableViewCell {
    override class var preferredHeight: CGFloat { return 100 }
}
```
**8. Using the zip function in Swift you can easily combine two sequences. Super useful when using two sequences to do some work, since zip takes care of all the bounds-checking.**
```swift
func render(titles: [String]) {
    for (label, text) in zip(titleLabels, titles) {
        print(text)
        label.text = text
    }
}
```

**9. The awesome thing about option sets in Swift is that they can automatically either be passed as a single member or as a set. Even cooler is that you can easily define your own option sets as well, perfect for options and other non-exclusive values.**
```swift
// Option sets are awesome, because you can easily pass them
// both using dot syntax and array literal syntax, like when
// using the UIView animation API:
UIView.animate(withDuration: 0.3,
               delay: 0,
               options: .allowUserInteraction,
               animations: animations)

UIView.animate(withDuration: 0.3,
               delay: 0,
               options: [.allowUserInteraction, .layoutSubviews],
               animations: animations)

// The cool thing is that you can easily define your own option
// sets as well, by defining a struct that has an Int rawValue,
// that will be used as a bit mask.
extension Cache {
    struct Options: OptionSet {
        static let saveToDisk = Options(rawValue: 1)
        static let clearOnMemoryWarning = Options(rawValue: 1 << 1)
        static let clearDaily = Options(rawValue: 1 << 2)

        let rawValue: Int
    }
}

// We can now use Cache.Options just like UIViewAnimationOptions:
Cache(options: .saveToDisk)
Cache(options: [.saveToDisk, .clearDaily])
```
**10. Combine first class functions in Swift with the fact that Dictionary elements are (Key, Value) tuples and you can build yourself some pretty awesome functional chains when iterating over a Dictionary.**
```swift
func makeActor(at coordinate: Coordinate, for building: Building) -> Actor {
    let actor = Actor()
    actor.position = coordinate.point
    actor.animation = building.animation
    return actor
}

func render(_ buildings: [Coordinate : Building]) {
    buildings.map(makeActor).forEach(add)
}
```
**11. One really nice benefit of dropping suffixes from method names (and just using verbs, when possible) is that it becomes super easy to support both single and multiple arguments, and it works really well semantically.**
```swift
extension UIView {
    func add(_ subviews: UIView...) {
        subviews.forEach(addSubview)
    }
}

view.add(button)
view.add(label)

// By dropping the "Subview" suffix from the method name, both
// single and multiple arguments work really well semantically.
view.add(button, label)
```
**12.Using the AnyObject (or class) constraint on protocols is not only useful when defining delegates (or other weak references), but also when you always want instances to be mutable without copying.**
```swift
// By constraining a protocol with 'AnyObject' it can only be adopted
// by classes, which means all instances will always be mutable, and
// that it's the original instance (not a copy) that will be mutated.
protocol DataContainer: AnyObject {
    var data: Data? { get set }
}

class UserSettingsManager {
    private var settings: Settings
    private let dataContainer: DataContainer

    // Since DataContainer is a protocol, we an easily mock it in
    // tests if we use dependency injection
    init(settings: Settings, dataContainer: DataContainer) {
        self.settings = settings
        self.dataContainer = dataContainer
    }

    func saveSettings() throws {
        let data = try settings.serialize()

        // We can now assign properties on an instance of our protocol
        // because the compiler knows it's always going to be a class
        dataContainer.data = data
    }
}
```
**13.  A really interesting side-effect of a UIView's bounds being its rect within its own coordinate system is that transforms don't affect it at all. That's why it's usually a better fit than frame when doing layout calculations of subviews.**
```swift
let view = UIView()
view.frame.size = CGSize(width: 100, height: 100)
view.transform = CGAffineTransform(scaleX: 2, y: 2)

print(view.frame) // (-50.0, -50.0, 200.0, 200.0)
print(view.bounds) // (0.0, 0.0, 100.0, 100.0)
```
**14. Avoiding Massive View Controllers is all about finding the right levels of abstraction and splitting things up. My personal rule of thumb is that as soon as I have 3 methods or properties that have the same prefix, I break them out into their own type.**
```swift
// BEFORE

class LoginViewController: UIViewController {
    private lazy var signUpLabel = UILabel()
    private lazy var signUpImageView = UIImageView()
    private lazy var signUpButton = UIButton()
}

// AFTER

class LoginViewController: UIViewController {
    private lazy var signUpView = SignUpView()
}

class SignUpView: UIView {
    private lazy var label = UILabel()
    private lazy var imageView = UIImageView()
    private lazy var button = UIButton()
}
```
**15. I love the fact that optionals are enums in Swift - it makes it so easy to extend them with convenience APIs for certain types. Especially useful when doing things like data validation on optional values.**
```swift
func validateTextFields() -> Bool {
    guard !usernameTextField.text.isNilOrEmpty else {
        return false
    }

    ...

    return true
}

// Since all optionals are actual enum values in Swift, we can easily
// extend them for certain types, to add our own convenience APIs

extension Optional where Wrapped == String {
    var isNilOrEmpty: Bool {
        switch self {
        case let string?:
            return string.isEmpty
        case nil:
            return true
        }
    }
}

// Since strings are now Collections in Swift 4, you can even
// add this property to all optional collections:

extension Optional where Wrapped: Collection {
    var isNilOrEmpty: Bool {
        switch self {
        case let collection?:
            return collection.isEmpty
        case nil:
            return true
        }
    }
}
```
**16. You can turn any Swift Error into an NSError, which is super useful when pattern matching with a code 👍. Also, switching on optionals is pretty cool!**
```swift
let task = urlSession.dataTask(with: url) { data, _, error in
    switch error {
    case .some(let error as NSError) where error.code == NSURLErrorNotConnectedToInternet:
        presenter.showOfflineView()
    case .some(let error):
        presenter.showGenericErrorView()
    case .none:
        presenter.renderContent(from: data)
    }
}

task.resume()
```
**17. You can switch on a set using array literals as cases in Swift! Can be really useful to avoid many if/else if statements.**
```swift
class RoadTile: Tile {
    var connectedDirections = Set<Direction>()

    func render() {
        switch connectedDirections {
        case [.up, .down]:
            image = UIImage(named: "road-vertical")
        case [.left, .right]:
            image = UIImage(named: "road-horizontal")
        default:
            image = UIImage(named: "road")
        }
    }
}
```
**18. Playing around with an expressive way to check if a value matches any of a list of candidates in Swift:**
```swift
 // Instead of multiple conditions like this:

if string == "One" || string == "Two" || string == "Three" {

}

// You can now do:

if string == any(of: "One", "Two", "Three") {

}
```
