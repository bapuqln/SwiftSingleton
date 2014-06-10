SwiftSingleton
==============

An exploration of the Singleton pattern in Swift. 

All approaches below support lazy initialization and thread safety. Or so it seems. 

Issues and pull requests welcomed.

### Approach A: Global constant

```swift
let _SingletonASharedInstance = SingletonA()

class SingletonA  {

    class var sharedInstance : SingletonA {
        return _SingletonASharedInstance
    }
    
}
```

It should be noted that global constants (and variables) are lazily loaded in Swift.

### Approach B: Nested struct

```swift
class SingletonB {
    
    class var sharedInstance : SingletonB {
        struct Static {
            static let instance : SingletonB = SingletonB()
        }
        return Static.instance
    }
    
}
```

### Approach C: dispatch_once

```swift
class SingletonC {
    
    class var sharedInstance : SingletonC {
        struct Static {
            static var onceToken : dispatch_once_t = 0
            static var instance : SingletonC? = nil
        }
        dispatch_once(&Static.onceToken) {
            Static.instance = SingletonC()
        }
        return Static.instance!
    }
}
```
