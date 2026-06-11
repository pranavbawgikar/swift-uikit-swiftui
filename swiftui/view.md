# View
View is a protocol and every UI element conforms to it. It is not a type of element, it is a contract that every single UI element must fulfil.

Example
```
import SwiftUI
struct ContentView: View {
    var body: some View {
        HStack {
            Rectangle()
                .fill(Color.red)
                .frame(width: 100, height: 100)
            Rectangle()
                .fill(Color.red)
                .frame(width: 100, height: 100)
            Rectangle()
                .fill(Color.red)
                .frame(width: 100, height: 100)
        }
    }
}
struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```
`some View` = opaque return type, compiler figures out the concrete type.

The `body` is a computed property that returns a single thing = single return value rule of computed properties. A computed property can only return one value. Basically, the `body` must return exactly one `View`.

Swift will refuse to build because a computed property cannot return two things.

## @ViewBuilder
`@ViewBuilder` allows listing multiple views inside a container closure without explicit returns.

`@ViewBuilder` is what makes this work inside VStack { }, HStack { }, and similar containers. The closure passed to those containers is marked `@ViewBuilder`, which is why you can write multiple views inside without `return`. 

SwiftUI uses a feature called result builders under the hood to combine them into a single composite view.
