---
layout: post
title:  Learning Notes - URLSession Overview & Other
---
## URLSession fragements
### How to user NSURLSession?
- For very simple, occasional use, this object might be the singleton shared- Session object.

```
■ init(configuration:)
```


## How to user a NSURLSession Object to perform a request task?
To use the NSURLSession object to perform a request across the network, you ask it for a new NSURLSessionTask object. This is the object that actually performs (and represents) one upload or download process. NSURLSessionTask is an abstract superclass, ***embodying(象征，具体化)*** various properties, such as:

	
	```
	■ .Running 
	■ .Suspended 
	■ .Canceling 
	■ .Completed
	```


In addition, you can tell a task to resume, suspend, or cancel. A task is ***born suspended（生即暂停）***; it does not start until it is told to resume for the first time.

You can also set an NSURLSessionTask’s **priority** to *a Float between 0 and 1*; this is just a *hint*（提示） to the system, and may be used to rank the relative importance of your tasks. For convenience, **three constant values** are also provided:

```
• NSURLSessionTaskPriorityLow (0.25)
• NSURLSessionTaskPriorityHigh (0.75)
```

Once you’ve obtained a new session task from the NSURLSession, the session retains it; you can keep a reference to the task if you wish, but you don’t have to. The session will provide you with a list of its tasks in progress; call getAllTasksWithCompletion- Handler:. The completion handler is handed an array of tasks. The session releases a task after it is cancelled or completed; thus, if an NSURLSession has no running or suspended tasks, the task array is empty.



	


## Formal HTTP Request flow


	- `timeoutIntervalForRequest`
		The maximum time you’re willing to wait between pieces of data. 
	- `timeoutIntervalForResource`
	
- 













```    
	  var task : NSURLSessionTask!
```



```
```
    



```

	func URLSession(session: NSURLSession,

```


```var data = NSMutableData()
```


```
```
Some delegate methods provide a completionHandler: parameter. These are delegate methods that require a response from you. For example, in the case of a data task, `URLSession:dataTask:didReceiveResponse:completionHandler: ` arrives when we first connect to the server. Here, we could check the status code of the initial `NSHTTPURLResponse`. We must also return a response saying whether or not to proceed (or whether to convert the data task to a download task, which could certainly come in handy). But because of the multithreaded, asynchronous nature of networking, we do this, not by returning a value directly, but by calling the completionHandler: that we’re handed as a parameter and passing our response into it. Several of the delegate methods are constructed in this way.

disastrous if the delegate could simply vanish out from the middle of an asynchronous time-consuming process; but it is also unusual, and requires special measures on our part. In the examples I’ve been describing in this sec‐ tion, we have a retain cycle! That’s because we have an NSURLSession instance variable self.session, but that NSURLSession is retaining self as its delegate.


`finishTasksAndInvalidate`






```
```

###Definition

```
- (void)URLSession:(NSURLSession *)session task:(NSURLSessionTask *)task didCompleteWithError:(NSError *)error
```

###Description	
Tells the delegate that the task **finished** *transferring data*.

Server errors are not reported through the error parameter. **The only errors** your delegate receives through the error parameter **are client-side errors**, such as being unable to resolve the hostname or connect to the host.

## Other 
It is possible in C to pass the wrong type to a function, but it is almost always not a good idea. Even if technically they might appear to be defined the same thing, there is probably a good reason Apple has them as separate things. Most likely it is to make more code readable and understandable, or Apple plans on making changes later.

After all that, a jump to definition reveals the real differences:

```
typedef struct CGPath *CGMutablePathRef;
typedef const struct CGPath *CGPathRef;
```
CGPathRef is a const typedef (google it if you're not quite sure what this does). However C allows you to break const definitions which is why you are still able to pass your CGPathRef to a function expecting a CGMutablePathRef and modify it fine. It also explains why it throws a discards qualifiers warning. 


```
NSGraphicsContext* newCtx = [NSGraphicsContext graphicsContextWithGraphicsPort:bitmapContext flipped:true];
[NSGraphicsContext saveGraphicsState];
[NSGraphicsContext setCurrentContext:newCtx];
NSAttributedString *string = /* make a string with all of the desired attributes */;
[string drawInRect:locationToDraw];
[NSGraphicsContext restoreGraphicsState];



int charCount = [string length];
        CGGlyph glyphs[charCount];
        CGRect rects[charCount];

        CTFontGetGlyphsForCharacters(theCTFont, (const unichar*)[string cStringUsingEncoding:NSUnicodeStringEncoding], glyphs, charCount);
        CTFontGetBoundingRectsForGlyphs(theCTFont, kCTFontDefaultOrientation, glyphs, rects, charCount);

        int totalwidth = 0, maxheight = 0;
        for (int i=0; i < charCount; i++)
        {
            totalwidth += rects[i].size.width;
            maxheight = maxheight < rects[i].size.height ? rects[i].size.height : maxheight;
        }

        dim = CGSizeMake(totalwidth, maxheight);
```
