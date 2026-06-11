# ScrollView
Example 1
```
import SwiftUI
struct ContentView: View {
    var body: some View {
        ScrollView {
            VStack {
                Rectangle().frame(height: 300)
                Rectangle().frame(height: 300)
                Rectangle().frame(height: 300)
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
Example 2
```
import SwiftUI
struct ContentView: View {
    var body: some View {
        ScrollView {
            VStack {
                ForEach(0..<50) { index in
                    Rectangle().frame(height: 300)
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
Example 3
```
import SwiftUI
struct ContentView: View {
    var body: some View {
        ScrollView {
            VStack {
                ForEach(0..<50) { index in
                    ScrollView(.horizontal, showsIndicators: false, content: {
                        HStack {
                            ForEach(0..<20) { index in
                                RoundedRectangle(cornerRadius: 25.0)
                                    .fill(.white)
                                    .frame(width: 200, height: 150)
                                    .shadow(radius: 10)
                                    .padding()
                            }
                        }
                    })
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
