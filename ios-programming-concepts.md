## Synopsis

Capturing design concepts in iOS programming

### Protocol

Protocol defines the interface / method definitions that are required or optional
that a class that conforms to the protocol must implement if the functions are
required.

#### Questions

1. When should you create a protocol instead of passing the method as part of the parameter to a function?
  * When you have multiple implementations possible for method for different class types, then it make sense to create a protocol.
2. Use of protocol to limit the parameter type input as opposed to any object.  In Swift, we have generics, but this is not compatible with Objective-C. Defining a super class is also a possibility.  What are the advantages/disadvantages for either approach?  


### Blocks

Block syntax is as follows: ``` {(parameters) -> (return type) in expression statements} ```

```Swift

{(parameters) -> (return type) in expression statements}

```

### Serializing or Converting to JSON

```Objective-C

Credentials *credential = [[Credentials alloc] init];
NSDictionary *parameters = [credential toJSONDictionary];

@implementation Credentials

- (NSDictionary *)toJSONDictionary {

    NSMutableDictionary *jsonKeys = [[super toJSONDictionary] mutableCopy];

    [jsonKeys setValue:self.updatedEmail forKey:@"new_email"];
    [jsonKeys setValue:self.updatedPassword forKey:@"new_password"];

    return [jsonKeys copy];
}

@end

```
