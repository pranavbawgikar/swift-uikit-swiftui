# Property Wrappers
## @State
## @Binding
`@Binding` connects a variable in a child view to a `@State` variable in the parent view.

`@Binding` is not exclusively tied to `@State`. It creates a two-way connection to any source of truth, it could come from a `@State`, a `@StateObject`'s published property, or even another `@Binding` passed further down. 
The key idea is that the child does not own the data, it just holds a reference to wherever the data actually lives.
