## Synopsis

Notes on Swift concepts

### Define functionality and add shared functionality

Use protocol to add functionality

```swift

protocol Shape {
    func draw(context: CGContext)
    var boundingBox: CGRect { get }
    mutating func rotate(by angle: CGFloat)
}

```

Create extension of protocol to provide shared functionality

```swift

extension Shape {
    func rotated(by angle: CGFloat) -> Shape {
        // ...
    }
}

```

### Date and Time

* **NSDate** **<-->** **NSDateFormatter** with NSCalendar, NSTimeZone **<-->** **Human readable string**
* **NSDate** **<-->** **NSCalendar** with NSTimeZone **<-->** **Date Components**

Tips

* Use timeInterval for things in pure seconds like timeouts, timer
```let fireDate = Date(timeIntervalSinceNow: 10)```
* Use NSCalendar for comparison, and creating or modifying dates such as next week or tomorrow
* Use NSDateFormatter to create string of dates in human readable format
* NSTimeZone
    * Always use full ID and not abbreviation like EST which could mean different things in different countries.  To get a list of all available names in Swift: ```TimeZone.abbreviationDictionary```
    * Support for geocode available
* Don't use DateFormatter directly
* Creating a date formatter is not a cheap operation. If you are likely to use a formatter frequently, it is typically more efficient to cache a single instance than to create and dispose of multiple instances. One approach is to use a static variable

Reference
* [How to be an iOS Time Lord Talk by Ameir Al-Zoubi](https://cocoaheads.tv/how-to-be-an-ios-time-lord-by-ameir-al-zoubi/)
