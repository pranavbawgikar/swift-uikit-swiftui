# Swift
## Variables
Variables are always initialized before use, integers are checked for overflow.
## Constants
Any value that is declared using let, will become a constant.

The value of a constant may not be defined at compile time, but must be assigned only once.
## Type Inference
The type of a variable or a constant may not be defined explicitly, the compiler infers it from the assigned value.
## Type Conversion
### Implicit Type Conversion
Means the compiler performs the type conversion automatically. Swift, by design, never does this.
### Explicit Type Conversion
Example
```
let widthLabel = label + String(width)
```
Here, the `String(width)` is called manually to convert the `Int`. Also, an example of String concatenation.

`String()` is a type initializer. A new `String` instance is created by calling `String`’s initializer with an `Int` argument. The same pattern works for `Int("42")`, `Double("3.14")`, etc.

## Operators
### +
Swift’s `+` operator knows only `String + String` and `Int + Int` operation.
### ??
`??` is the nil coalescing operator.

Example
```
let informalGreeting = "Hi \(nickname ?? fullName)"
```
It means: "use nickname if it has a value, otherwise fall back to fullName." Since nickname is nil, fullName is used.
### ..<
It is a half open, range operator and it excludes the last value.
### ...
It is a closed range operator and includes both ends.

## Control Flow
### If
Example of 'If as an expression'
```
let scoreDecoration = if teamScore > 10 { "🎉" } else { "" }
```
Assigning the result of an If conditional directly to the variable.
### Switch
Example
```
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins...")
case "cucumber", "watercress":        // two values, one case
    print("Good tea sandwich.")
case let x where x.hasSuffix("pepper"):  // pattern match
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
```
When Swift hits case let `x` where `x.hasSuffix("pepper")`. This does two things simultaneously: it binds the value "red pepper" to a new constant `x`, AND checks whether `x` ends with "pepper". It does, so this case wins, and `x` is available inside the block.

Think of `case let x where` condition as: "match anything, call it x, but only if this extra condition is also true."
### ForEach
Example
```
import SwiftUI
struct ContentView: View {
    var body: some View {
        VStack {
            ForEach(0..<10) { index in
                HStack {
                    Circle().fill(Color.blue)
                    Text("Test \(index)")
                }
            }
        }
    }
}
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```
## Optionals
Optional contains a value or nil, marked with `?`.
## Arrays
Arrays automatically grow as elements are added, append() adds to the end.

An empty array/dictionary needs an explicit type to work with.

Swift cannot infer the type from [] alone when declaring an empty array.
```
var emptyArray = [] // ❌
var emptyArray: [String] = []   // annotation on the variable
var emptyArray = [String]()     // or call the initializer
```
[String] vs Array<String> are 100% identical, [String] is just syntactic sugarcoated version that the compiler rewrites to Array<String>.

## Dictionaries
Dictionaries are unordered.

Example 1 `var occupations = ["Malcolm": "Captain", "Kaylee": "Mechanic"]`

Example 2 `let emptyDictionary: [String: Float] = [:]`
> This declares an empty dictionary where every key is a `String` and every value is a `Float`.

> The `[:]` is dictionary literal syntax for "empty dictionary."

> Type annotation is required because `[:]` alone gives Swift nothing to infer from.

> Same principle as Arrays.

> If `String: Float` were wrapped with `( )`, it would make it a tuple.

When you loop over a dictionary, each iteration gives you a tuple — a pair of (key, value). 

Example
```
let interestingNumbers = [
    "Prime":     [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square":    [1, 4, 9, 16, 25],
]
var largest = 0
for (_, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest { largest = number }
    }
}
```
> Here the key would be "Prime", "Fibonacci", or "Square", and the value would be the array.

> `_` tells Swift "I acknowledge the key exists but I am discarding it." numbers captures the array.

> The inner loop then walks every `number` in that array, tracking the maximum.

> Result: 25, because that is the largest number across all three arrays.

## Structs
Structs are value types stored on the stack.

Structs are faster than Class.

Struct instances are technically called values, not objects (objects are instances of classes).

`init` in Swift is exactly the equivalent of a constructor in Java, Kotlin, Dart, or C++. It runs when an instance is created and sets up the initial state. Structs have an implicit `init`.

Example 1
```
struct Person {
    let name: String
    let age: Int

    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
}
let p = Person(name: "Riya", age: 28)
```
Example 2
```
struct Point {
    var x: Int
    var y: Int
}
var pointA = Point(x: 1, y: 2)
var pointB = pointA      // COPY — not the same thing
pointB.x = 99
print(pointA.x)  // still 1 — pointA was not affected
```
Two variables can hold equal struct values but they are entirely independent copies. If `Point` were a class, `pointA.x` would also be `99` because both variables would reference the same heap object.

Example 3
```
import UIKit

struct Developer {
    var name: String
    var jobTitle: String
    var experienceInYears: Int
}

var sean = Developer(name: "Sean", jobTitle: "Associate System Engineer", experienceInYears: 3)
var cody = sean // A copy of `sean` is created and gets assigned to `cody`
cody.name = "Lucas"
print(sean.name)
```

### mutating
Methods in Structs are not allowed to modify Struct’s own properties because modifying a property in a `struct` means changing the entire value.

When a method is marked with `mutating`, it means that the method is allowed to change the Struct’s properties.

A `mutating` method cannot be called on a `let` constant, because a constant Struct cannot be replaced. If you need to call mutating methods, use `var`.

Example
```
struct Counter {
    var count: Int = 0
    mutating func increment() {
        count += 1
    }
    func display() {
        print(count)
    }
}
var c = Counter()
c.increment()
c.display()
```
### Struct + `nil` Coalescing
Example
```
struct UserProfile {
    let firstName: String
    let lastName: String
    let nickname: String?      // optional — user may not have set one
}

let user = UserProfile(firstName: "Arjun", lastName: "Mehta", nickname: nil)

// nil coalescing: use nickname if present, fall back to firstName
let displayName = user.nickname ?? user.firstName
print(displayName)   // prints "Arjun"

let user2 = UserProfile(firstName: "Priya", lastName: "Nair", nickname: "PK")
let displayName2 = user2.nickname ?? user2.firstName
print(displayName2)  // prints "PK"
```

## Class
`deinit` is the opposite of `init` → destroying/deinitializing an object.

`deinit` is called automatically by ARC just before a class instance is freed from memory.

Only classes have `deinit` because classes are reference-counted on the heap. Structs are just popped off the stack when they go out of scope, no deinitialization required.

`let` can be used for class instances because the object itself isn't changing, the data inside is. `let` on a class instance locks the reference (cannot be pointed at a different object), but the object's internal properties can still be mutated. With a `let` struct, nothing at all can change, neither the reference nor the data.

Example
```
import UIKit

class Developer {
    var name: String
    var jobTitle: String
    var experienceInYears: Int
    
    init(name: String, jobTitle: String, experienceInYears: Int) {
        self.name = name
        self.jobTitle = jobTitle
        self.experienceInYears = experienceInYears
    }
}

var sean = Developer(name: "Sean", jobTitle: "Associate System Engineer", experienceInYears: 3)
var cody = sean // cody is pointing to the same space where sean is pointing to
cody.name = "Lucas"
print(sean.name) // Lucas
// Both variables were pointing to the same object in the memory so when the name of `cody` was changed, the name for `sean` changed too.
```

## Enums
Enums are the same as Structs, except all the cases are known at compile time.

Enums can be mutated with a mutating keyword, just like Structs.

Example 1
```
import UIKit

enum SocialPlatform {
    case instagram
    case facebook
    case twitter
    case linkedin
}

func shareImage(on platform: SocialPlatform) {
    switch platform {
    case .instagram:
        print("gram")
    case .facebook:
        print("fb")
    case .linkedin:
        print("link")
    case .twitter:
        print("twt")
    }
}

shareImage(on: .twitter)
```

Example 2
```
import UIKit

enum SocialPlatform: String {
    case instagram = "Gram"
    case facebook = "Fb"
    case twitter = "Twt"
    case linkedin = "Link"
}

func getEnumRawValue(on platform: SocialPlatform) {
    print(platform.rawValue)
}

getEnumRawValue(on: .instagram)
```
Example 3
```
import UIKit

enum SocialPlatform: String, CaseIterable {
    case instagram = "Gram" // Raw Value
    case facebook = "Fb"
    case twitter = "Twt"
    case linkedin = "Link"
}

print(SocialPlatform.allCases.count)

for platform in SocialPlatform.allCases {
    print(platform.rawValue)
}
```
Example 4
```
import UIKit

enum SocialMediaPlatform {
    case youtube(subscribers: Int)
    case instagram
    case facebook
    case twitter(followers: Int)
}

func getSponsorshipEligibity(for platform: SocialMediaPlatform) {
    switch platform {
    case .twitter(let followers) where followers > 10_000:
        print("Eligibile for sponsored tweets")
    case .youtube(let subscribers) where subscribers > 25_000:
        print("Eligible for sponsored video")
    case .instagram, .facebook, .twitter, .youtube:
        print("Not eligible for sponsorship")
    }
}

getSponsorshipEligibity(for: .twitter(followers: 15000))
getSponsorshipEligibity(for: .youtube(subscribers: 100))
getSponsorshipEligibity(for: .twitter(followers: 50))
```
