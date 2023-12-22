# Swift

---

### References:
1. https://medium.com/@PavloShadov/best-resources-for-advanced-ios-developer-swift-ade30374593d
2. [Swift Tips and Tricks](https://www.hackingwithswift.com/articles/106/10-quick-swift-tips)
3. [vincent Tips](https://www.swiftwithvincent.com/tips)

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
>   
>  **Wrong**
>   ```swift
>    var someOptionalString: String?
>    if let stringValue = someOptionalString, stringValue.isEmpty { ... }
>   ```
>   **Right**
>   ```swift
>    var someOptionalString: String?
>    if stringValue?.isEmpty == false { ... }
>   ```

### Best Practices
> - Use value types over reference types
> - Use higher-order functions - Swift supports higher-order functions like map, filter, and reduce to make your code more expressive and functional
> - Use Generics for Flexible Code


