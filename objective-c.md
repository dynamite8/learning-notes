## Synopsis

Some notes on Objective-C concepts or syntax that I frequently have to look up.  I am making it easier to find them in one place.

## Apple API

[API Reference](https://developer.apple.com/reference/)

### Content Sizing Priorities

* Content Hugging Priority - "Stay close to me"
* Content Compression Resistance Priority - "I need more space"

**Intrinsic Content Size** - Pretty self-explanatory, but views with variable content are aware of how big their content is and describe their content's size through this property. Some obvious examples of views that have intrinsic content sizes are UIImageViews, UILabels, * UIButtons.

**Content Hugging Priority** - The higher this priority is, the more a view resists growing larger than its intrinsic content size.

**Content Compression Resistance Priority** - The higher this priority is, the more a view resists shrinking smaller than its intrinsic content size.

<img src="https://static1.squarespace.com/static/5592eb03e4b051859f0b377f/t/55f86137e4b075b26c33cc17/1442341176554/?format=300w">

Reference: [KRAVENDEV: Auto Layout Magic: Content Sizing Priorities](https://krakendev.io/blog/autolayout-magic-like-harry-potter-but-real)

### Repeat or schedule a delay to update the content

```Objective-C

[NSTimer scheduledTimerWithTimeInterval:1.0f
                                 target:self
                               selector:@selector(updateTime)
                               userInfo:nil
                                repeats:YES];

- (void)updateTime {...}

```

### Foundation URL Loading System (Network Calls)

* **NSURLConnection (2003)** - NSURLRequest, NSURLResponse, NSURLProtocol, NSURLCache, NSHTTPCookieStorage, NSURLCredentialStorage, and its namesake, NSURLConnection
* **NSURLSession (2013)** - replaces NSURLConnection with NSURLSession, NSURLSessionConfiguration, and three subclasses of NSURLSessionTask: NSURLSessionDataTask, NSURLSessionUploadTask, and NSURLSessionDownloadTask
  * Pro: Ability to configure per-session cache, protocol, cookie, and credential policies, rather than sharing them across the app - parts of app/frameworks operate independently, thus improves performance
  * All tasks are cancelable, and can be paused and resumed

#### Examples

```Objective-C

NSURL *URL = [NSURL URLWithString:@"http://example.com"];
NSURLRequest *request = [NSURLRequest requestWithURL:URL];

[NSURLConnection sendAsynchronousRequest:request
                                   queue:[NSOperationQueue mainQueue]
                       completionHandler:^(NSURLResponse *response, NSData *data, NSError *error) {
    // ...
}];

```

Note: NSURLSession allows you to do additional configuration before kicking off request

**Requesting data**

```Objective-C

NSURL *URL = [NSURL URLWithString:@"http://example.com"];
NSURLRequest *request = [NSURLRequest requestWithURL:URL];

NSURLSession *session = [NSURLSession sharedSession];
NSURLSessionDataTask *task = session dataTaskWithRequest:request completionHandler:
    ^(NSData *data, NSURLResponse *response, NSError *error) {
        // ...
    }];

[task resume];

```

**Upload task**

```Objective-C

NSURL *URL = [NSURL URLWithString:@"http://example.com/upload"];
 NSURLRequest *request = [NSURLRequest requestWithURL:URL];
 NSData *data = ...;

 NSURLSession *session = [NSURLSession sharedSession];
 NSURLSessionUploadTask *uploadTask = [session uploadTaskWithRequest:request
                                                            fromData:data
                                                   completionHandler:
     ^(NSData *data, NSURLResponse *response, NSError *error) {
         // ...
     }];

 [uploadTask resume];

```

**Download file**

```Objective-C

NSURL *URL = [NSURL URLWithString:@"http://example.com/file.zip"];
 NSURLRequest *request = [NSURLRequest requestWithURL:URL];

 NSURLSession *session = [NSURLSession sharedSession];
 NSURLSessionDownloadTask *downloadTask = [session downloadTaskWithRequest:request
                                                         completionHandler:
    ^(NSURL *location, NSURLResponse *response, NSError *error) {
        NSString *documentsPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject];
        NSURL *documentsDirectoryURL = [NSURL fileURLWithPath:documentsPath];
        NSURL *documentURL = [documentsDirectoryURL URLByAppendingPathComponent:[response
suggestedFilename]];
        [[NSFileManager defaultManager] moveItemAtURL:location
                                                toURL:documentURL
                                                error:nil];
 }];

 [downloadTask resume];

```

References:

* [NSUrlConnection to NSUrlSession Background](https://www.objc.io/issues/5-ios7/from-nsurlconnection-to-nsurlsession/)
* [NSURLSession Tutorial](https://www.raywenderlich.com/110458/nsurlsession-tutorial-getting-started)

## NSOperation

Specify the priority of the operation in queue

```Objective-C

NSOperation operation = ...
operation.queuePriority = NSOperationQueuePriorityLow;

```

### Initializing NSDictionary

```Objective-C

NSDictionary *originalValues = @{ @"city" : place.city,
                                  @"latitude" : place.latitude,
                                  // etc.
                                };
```

## Author

Original Author

Jenny Chang Ho (jchangho@grubhub.com)
