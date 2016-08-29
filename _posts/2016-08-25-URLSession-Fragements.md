---
layout: post
title:  Learning Notes - URLSession Overview & Other
---
## URLSession fragements
### How to user NSURLSession?
- For very simple, occasional use, this object might be the singleton shared- Session object.- More generally, you’ll create your own NSURLSession by calling an initializer: 

```
■ init(configuration:)■ init(configuration:delegate:delegateQueue:)
```
You’ll hand these initializers an NSURLSessionConfiguration object describing the desired environment.

## How to user a NSURLSession Object to perform a request task?
To use the NSURLSession object to perform a request across the network, you ask it for a new NSURLSessionTask object. This is the object that actually performs (and represents) one upload or download process. NSURLSessionTask is an abstract superclass, ***embodying(象征，具体化)*** various properties, such as:
- A **taskDescription** and **taskIdentifier**; the **former** is up to you, while the **latter** is a unique identifier within the NSURLSession- The ***originalRequest*** and **currentRequest** (the request can change because there might be a **redirect**)- An *initial* **response** from the server- Various ***countOfBytes***... properties allowing you to **track progress**- A **state**, which might be (NSURLSessionTaskState):
	
	```
	■ .Running 
	■ .Suspended 
	■ .Canceling 
	■ .Completed
	```
- An **error** if the task failed

In addition, you can tell a task to resume, suspend, or cancel. A task is ***born suspended（生即暂停）***; it does not start until it is told to resume for the first time.

You can also set an NSURLSessionTask’s **priority** to *a Float between 0 and 1*; this is just a *hint*（提示） to the system, and may be used to rank the relative importance of your tasks. For convenience, **three constant values** are also provided:

```
• NSURLSessionTaskPriorityLow (0.25)• NSURLSessionTaskPriorityDefault (0.5) 
• NSURLSessionTaskPriorityHigh (0.75)
```

Once you’ve obtained a new session task from the NSURLSession, the session retains it; you can keep a reference to the task if you wish, but you don’t have to. The session will provide you with a list of its tasks in progress; call getAllTasksWithCompletion- Handler:. The completion handler is handed an array of tasks. The session releases a task after it is cancelled or completed; thus, if an NSURLSession has no running or suspended tasks, the task array is empty.
There are two ways to ask your NSURLSession for a new NSURLSessionTask:
- **Call a convenience method**
	The convenience methods all take a completionHandler: parameter. This **completion handler** is called when the task process ends.
	- **Call a delegate-based method**
	You give the NSURLSession a delegate when you create it, and the delegate is called back at **various stages** of a task’s progress.

## Formal HTTP Request flowNow let’s go to the other extreme and be very formal:
- We’ll create and retain our own NSURLSession.- We’ll configure the NSURLSession with an NSURLSessionConfiguration object.- We’ll give the session a delegate.- Instead of a mere URL, we’ll start with an NSURLRequest.The NSURLSession initializers permit us to supply an NSURLSessionConfiguration object dictating various options to be applied to the session. Possible options include:
- Whether to permit cell data use, or to require Wi-Fi- The maximum number of simultaneous connections to the remote server- Timeout values:
	- `timeoutIntervalForRequest`		
		The maximum time you’re willing to wait between pieces of data. 
	- `timeoutIntervalForResource`
			The maximum time for the entire download to arrive.- Cookie, caching, and credential policies
- We’re going to call init(configuration:delegate:delegateQueue:), so we’ll also specify a delegate, as well as stating the **queue** on which the delegate methods are to be called. For each type of task, there’s a delegate protocol, which is itself often a composite of multiple protocols.
For example, for a data task, we would want a data delegate — an object conforming to the `NSURLSessionDataDelegate` protocol, which itself conforms to the `NSURLSession`‐ TaskDelegate protocol, which in turn conforms to the `NSURLSessionDelegate` protocol, resulting in about a dozen delegate methods we could implement, though only a few are crucial:
`URLSession:dataTask:didReceiveData:`
Some data has arrived, as an NSData object. The data will arrive piecemeal, so this method may be called many times during the download process, supplying new data each time. Our job is to accumulate all those chunks of data; this involves maintaining state between calls.
`URLSession:task:didCompleteWithError:`
If there is an error, we’ll find out about it here. If there’s no error, this is our signal that the download is over; we can now do something with the accumulated data.
Similarly, for a download task, we need a download delegate, conforming to `NSURSessionDownloadDelegate`, which conforms to `NSURLSessionTaskDelegate`, which conforms to `NSURLSessionDelegate`. Here are some useful delegate methods:
`URLSession:downloadTask:didResumeAtOffset:expectedTotalBytes:`
This method is of interest only in the case of a resumable download that has in fact been paused and resumed.
`URLSession:downloadTask:didWriteData:totalBytesWritten:totalBytesExpectedToWrite:`
Called periodically, to keep us apprised of the download’s progress.
`URLSession:downloadTask:didFinishDownloadingToURL:`
Called at the end of the process; we must grab the downloaded file immediately, as it will be destroyed. This is the only required delegate method.
It’s also a good idea to implement `URLSession:task:didCompleteWithError:`; if there was a communication problem, the error: parameter will tell you about it.Here, then, is my recasting of the same image file download as in the previous example. I’m going to keep a reference both to the `NSURLSession` (self.session) and to the current download task (self.task). In this particular example, I’ll perform just one task at a time, so I can use the task instance variable as a flag to indicate that the task is in progress. Since one NSURLSession can perform multiple tasks, there will typically be just one NSURLSession; so I’ll make a lazy initializer that creates and configures it:
```    
	  var task : NSURLSessionTask!    lazy var session : NSURLSession = {        let config = NSURLSessionConfiguration.ephemeralSessionConfiguration()        config.allowsCellularAccess = false        let session = NSURLSession(configuration: config,            delegate: self, delegateQueue: NSOperationQueue.mainQueue())        return session}()
```
I’ve specified, for purposes of the example, that no caching is to take place and that data downloading via cell is forbidden; you could configure the NSURLSession much more heavily and meaningfully, of course. I have specified self as the delegate, and I have requested delegate callbacks on the main thread.
Here we go. I blank out the image view, to make the progress of the task more obvious for test purposes, and I create, retain, and start the download task:

```    let s = "http://www.someserver.com/somefolder/someimage.jpg"    let url = NSURL(string:s)!    let req = NSMutableURLRequest(URL:url)    let task = self.session.downloadTaskWithRequest(req)    self.task = task    self.iv.image = nil    task.resume()
```
    In this particular example, there is very little merit in using an NSURLRequest instead of an NSURL to form our task. Still, an NSURLRequest can come in handy (as I’ll demonstrate later in this chapter), and an upload task requires one.
Do not use the `NSURLRequest` to configure properties of the request that are configurable through the NSURLSession. Those properties are left over from the era before NSURLSession existed. For example, there is no point setting the NSURLRequest’s timeoutInterval, as it is the NSURLSession’s timeout properties that are significant.
Here are some delegate methods for responding to the download:

```    func URLSession(session: NSURLSession,        downloadTask: NSURLSessionDownloadTask,        didWriteData bytesWritten: Int64,        totalBytesWritten writ: Int64,        totalBytesExpectedToWrite exp: Int64) {            print("downloaded \(100*writ/exp)%")    }    func URLSession(session: NSURLSession,        task: NSURLSessionTask,        didCompleteWithError error: NSError?) {	 }	 print("completed: error: \(error)")
 
	func URLSession(session: NSURLSession,    downloadTask: NSURLSessionDownloadTask,    didFinishDownloadingToURL location: NSURL) {	}	}	self.task = nil	let response = downloadTask.response as! NSHTTPURLResponse	let stat = response.statusCode	print("status \(stat)")	if stat != 200 {		return }	let d = NSData(contentsOfURL:location)!	let im = UIImage(data:d)	dispatch_async(dispatch_get_main_queue()) {    	self.iv.image = im	}

```
That was a download task; now I’ll describe a data task. A data task is a little more elaborate than a download task, but not much; the chief difference is that it’s up to you to accumulate the data as it arrives in chunks. For this purpose, you’ll want to keep an NSMutableData object on hand; I’ll use a property:

```var data = NSMutableData()When I create the data task, I am careful to prepare self.data (by giving it a zerolength):    let s = "http://www.someserver.com/somefolder/someimage.jpg"    let url = NSURL(string:s)!    let req = NSMutableURLRequest(URL:url)    let task = self.session.dataTaskWithRequest(req) // *    self.task = task    self.iv.image = nil    self.data.length = 0 // *    task.resume()
```
As the chunks of data arrive, I keep appending them to self.data. When all the data has arrived, it is ready for use:

```    func URLSession(session: NSURLSession,        dataTask: NSURLSessionDataTask,        didReceiveData data: NSData) {            self.data.appendData(data)    }    func URLSession(session: NSURLSession,        task: NSURLSessionTask,        didCompleteWithError error: NSError?) {self.task = nilif error == nil {    self.iv.image = UIImage(data:self.data)}
``` 
Some delegate methods provide a completionHandler: parameter. These are delegate methods that require a response from you. For example, in the case of a data task, `URLSession:dataTask:didReceiveResponse:completionHandler: ` arrives when we first connect to the server. Here, we could check the status code of the initial `NSHTTPURLResponse`. We must also return a response saying whether or not to proceed (or whether to convert the data task to a download task, which could certainly come in handy). But because of the multithreaded, asynchronous nature of networking, we do this, not by returning a value directly, but by calling the completionHandler: that we’re handed as a parameter and passing our response into it. Several of the delegate methods are constructed in this way.
There is one final and extremely important consideration when using NSURLSession with a delegate — memory management. The NSURLSession retains its delegate. This is understandable, as it would be 
disastrous if the delegate could simply vanish out from the middle of an asynchronous time-consuming process; but it is also unusual, and requires special measures on our part. In the examples I’ve been describing in this sec‐ tion, we have a retain cycle! That’s because we have an NSURLSession instance variable self.session, but that NSURLSession is retaining self as its delegate.
As with an NSTimer, the solution is to invalidate the NSURLSession at some appropriate moment. There are two ways to do this:

`finishTasksAndInvalidate`

Allows any existing tasks to run to completion. Afterward, the NSURLSession releases the delegate and cannot be used for anything further.
`invalidateAndCancel`
Interrupts any existing tasks immediately. The NSURLSession releases the delegate and cannot be used for anything further.
Let’s suppose that self is a view controller. Then `viewWillDisappear:` could be a good place to invalidate the NSURLSession. (We cannot use deinit, because deinit won’t be called until after we have invalidated the NSURLSession; that’s what it means to have a retain cycle.) So, for example:

```    override func viewWillDisappear(animated: Bool) {        super.viewWillDisappear(animated)        self.session.finishTasksAndInvalidate()}
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

