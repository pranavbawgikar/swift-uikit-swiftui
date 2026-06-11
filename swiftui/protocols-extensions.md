# Protocols
A protocol is just a list of rules that an object (types: structs, classes, and enums) has to follow. Basically, it is a list of commands.

Structs and enums can conform to protocols too, which is different from languages like Java where interfaces only apply to classes.

A protocol only declares what must exist, not how it works. Example:
```
protocol ProtocolSelectionDelegate {
    func didSelectProduct(name: String, imageName: String)
}
```
The body is written by whoever conforms to the protocol.

# Extensions
An extension enables adding new functionality to an existing type.

A protocol's extension can write a default body on the protocol. 

Protocol Declaration = No Body. Protocol Extension = Body Allowed.

## Protocols + Extensions
