## Synopsis

Some notes on Swift and Objective-C interoperability.

### Named Obj-C Property Getters and Setters in Swift

Use the @objc(<#name#>) attribute to provide Objective-C names for properties and methods when necessary. For example, you can mark a property called enabled to have a getter named isEnabled in Objective-C like this:

SWIFT

```swift
var enabled: Bool {
      @objc(isEnabled) get {
      /* ... */
      }
  }
```

Named Objective-C Getter Property in Swift

```swift
var _enabled:Bool = false
var enabled:Bool {
    @objc(isEnabled) get {
        return self._enabled
    }
    set(newValue){
        _enabled = newValue
    }
}
```

**readonly Objective-C Property** in Swift

Either

```swift
var _exists:Bool = false
private(set) var exists:Bool {
    get{
        return self._exists
    }
    set(newValue){
        self._exists = newValue
    }
}
```

or

```swift
var _exists:Bool = false
var exists:Bool {
    get{
        return self._exists
    }
}
```
and access self._exists directly since there is no setter.

### Enum

Enums in Objective-C are never nil because they always default to 0 as they are not reference types.

This is not valid

```swift
public var deviceFamily: DeviceFamily?
```
To make Swift enum compatible with Objective-C

```swift

public enum DeviceFamily : Int {

        case Web

        case Tablet

        case Mobile

        case Other // added this to handle nil or other cases
    }

public var deviceFamily: DeviceFamily = .Other
```

### Compatibility

In case all of the previous directives were old hat for you, there’s a strong likelihood that you didn’t know about this one:

@compatibility_alias: Allows existing classes to be aliased by a different name.

For example PSTCollectionView uses @compatibility_alias to significantly improve the experience of using the backwards-compatible, drop-in replacement for UICollectionView

```Objective-C

@compatibility_alias UICollectionViewController PSTCollectionViewController;
@compatibility_alias UICollectionView PSTCollectionView;
@compatibility_alias UICollectionReusableView PSTCollectionReusableView;
@compatibility_alias UICollectionViewCell PSTCollectionViewCell;
@compatibility_alias UICollectionViewLayout PSTCollectionViewLayout;
@compatibility_alias UICollectionViewFlowLayout PSTCollectionViewFlowLayout;
@compatibility_alias UICollectionViewLayoutAttributes     PSTCollectionViewLayoutAttributes;
@protocol UICollectionViewDataSource <PSTCollectionViewDataSource> @end
@protocol UICollectionViewDelegate <PSTCollectionViewDelegate> @end

```
Reference: [NSHipster Compatibility](http://nshipster.com/at-compiler-directives/)

### Namespacing

Reference: [NSHipster Namespacing](http://nshipster.com/namespacing/)

## Questions

* Do we prefix classes in Swift? **Yes** to avoid conflicts in Objective-C potentially.  Refer to compatibility section to resolve conflicts.
* Do we add space after class name ? ```swift class MyClass : NSObject```

### Extending protocols with default implementation

In Swift, you can implement shared functionality for all classes that conforms to the protocol. However, the methods are not accessible from Objective-C.

```swift

@objc public protocol DataProtocol {

    /// Returns dictionary representation of the data
    var dictionary: Dictionary<String, Any> { get }Â

    /// Add optional variables to the payload.
    ///
    /// - Parameter optionalVars: optional Dictionary<String, Any> of variables to append to event payload.
    func appendOptionalVars(_ optionalVars: Dictionary<String, Any>?)
}

/// Shared functionality for all classes that conforms to DataProtocol
extension DataProtocol {

  public func appendOptionalVars(_ optionalVars: Dictionary<String, Any>?) {

      if let keys = optionalVars?.keys {
          for key in keys {
              if let value = optionalVars?[key] {
                  addParameter(key: key, value: value)
              }
          }
      }
  }
}

```

### Optional Int?

Objective-C does not recognize optional Int.  When it's not an optional int in Swift, it's bridged as NSInteger.  As a workaround, I created NSNumber variable and updated the setter to set a private Int?

```Swift

private var _number: Int?
public var number: NSNumber? {
    get {
        return _number as NSNumber?
    }
    set(newNumber) {
        _number = newNumber?.intValue
    }
}
```

Reference:
* [Can swift method defined on extensions on protocols accessed in Objective-C? NO](http://stackoverflow.com/questions/32762841/can-swift-method-defined-on-extensions-on-protocols-accessed-in-objective-c)
* [Ray Wenderlich on Protocol Oriented Programming](https://www.raywenderlich.com/148448/introducing-protocol-oriented-programming)

## Author

Original Author

Jenny Chang Ho (jchangho@grubhub.com)
