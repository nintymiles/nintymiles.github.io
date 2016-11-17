---
layout: post
title: Swift Learning Notes - 如何使用Swift将String写入文件（Mac Dev）
---
## 如何使用`String.writeTo(...)`写内容到文件
示例代码：

```
    func loadPageHtml(url:URL,fileName:String) {
        let htmlString = try! String.init(contentsOf: url)
        print(htmlString)

        let filename = getDocumentsDirectory().appendingPathComponent("test/\(fileName).html")

        do {
            //若要写入文件，则url一定为file url (file://users/xxx/xxx/xxx.html)
            try htmlString.write(to: filename, atomically: true, encoding: String.Encoding.utf8)
        } catch {
            fatalError("failed to write file – bad permissions, bad filename, missing permissions, or more likely it can't be converted to the encoding")
        }


    }
	 
	 //获取文件URL
    func getDocumentsDirectory() -> URL {
        let paths = FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)
        let documentsDirectory = paths[0]
        return documentsDirectory
    }
```


### 不使用文件URL导致的错误
若使用下面代码，生成的urlstring为／user/xxx/xxx.html，则会抛出错误。

```
        //此种写法错误在于没有组合为文件URL
        //        let dirs = NSSearchPathForDirectoriesInDomains(FileManager.SearchPathDirectory.documentDirectory, FileManager.SearchPathDomainMask.userDomainMask, true)
//        let path = dirs[0].appending("/test/file.html")
//        try! htmlString.write(to: URL(string:path)!, atomically: true, encoding: String.Encoding.utf8)
```

错误Log

```
2016-11-17 16:03:25.226582 BlogTracker[19161:1293149] CFURLCopyResourcePropertyForKey failed because it was passed an URL which has no scheme
fatal error: 'try!' expression unexpectedly raised an error: Error Domain=NSCocoaErrorDomain Code=518 "The file couldn’t be saved because the specified URL type isn’t supported." UserInfo={NSURL=/Users/xxx/Documents/test/file.html}: file /Library/Caches/com.apple.xbs/Sources/swiftlang/swiftlang-800.0.58.6/src/swift/stdlib/public/core/ErrorType.swift, line 178
2016-11-17 16:03:25.228952 BlogTracker[19161:1293149] fatal error: 'try!' expression unexpectedly raised an error: Error Domain=NSCocoaErrorDomain Code=518 "The file couldn’t be saved because the specified URL type isn’t supported." UserInfo={NSURL=/Users/xxx/Documents/test/file.html}: file /Library/Caches/com.apple.xbs/Sources/swiftlang/swiftlang-800.0.58.6/src/swift/stdlib/public/core/ErrorType.swift, line 178
Current stack trace:
0    libswiftCore.dylib                 0x0000000100365cc0 swift_reportError + 132
1    libswiftCore.dylib                 0x0000000100382f50 _swift_stdlib_reportFatalErrorInFile + 112
2    libswiftCore.dylib                 0x0000000100331370 partial apply for (_assertionFailed(StaticString, String, StaticString, UInt, flags : UInt32) -> Never).(closure #1).(closure #1).(closure #1) + 99
3    libswiftCore.dylib                 0x00000001001790a0 specialized specialized StaticString.withUTF8Buffer<A> ((UnsafeBufferPointer<UInt8>) -> A) -> A + 355
4    libswiftCore.dylib                 0x00000001003312b0 partial apply for (_assertionFailed(StaticString, StaticString, StaticString, UInt, flags : UInt32) -> Never).(closure #1).(closure #1) + 144
5    libswiftCore.dylib                 0x00000001001795b0 specialized specialized String._withUnsafeBufferPointerToUTF8<A> ((UnsafeBufferPointer<UInt8>) throws -> A) throws -> A + 613
6    libswiftCore.dylib                 0x00000001002d5af0 partial apply for (_assertionFailed(StaticString, String, StaticString, UInt, flags : UInt32) -> Never).(closure #1) + 185
7    libswiftCore.dylib                 0x00000001001790a0 specialized specialized StaticString.withUTF8Buffer<A> ((UnsafeBufferPointer<UInt8>) -> A) -> A + 355
8    libswiftCore.dylib                 0x0000000100178e80 _assertionFailed(StaticString, String, StaticString, UInt, flags : UInt32) -> Never + 144
9    libswiftCore.dylib                 0x000000010019c540 swift_unexpectedError_merged + 569
10   BlogTracker                        0x0000000100004cb0 ViewController.loadPageHtml(url : URL, fileName : String) -> () + 1474
11   BlogTracker                        0x0000000100003280 ViewController.webView(WebView!, didFinishLoadFor : WebFrame!) -> () + 4839
12   BlogTracker                        0x0000000100004c50 @objc ViewController.webView(WebView!, didFinishLoadFor : WebFrame!) -> () + 79
13   WebKitLegacy                       0x00007fffb3c02530 CallFrameLoadDelegate(void (*)(), WebView*, objc_selector*, objc_object*) + 59
14   WebKitLegacy                       0x00007fffb3b95270 WebFrameLoaderClient::dispatchDidFinishLoad() + 81
15   WebCore                            0x00007fffb299b1d0 WebCore::FrameLoader::checkLoadCompleteForThisFrame() + 2120
16   WebCore                            0x00007fffb299af60 WebCore::FrameLoader::checkLoadComplete() + 443
17   WebCore                            0x00007fffb29f7450 WebCore::SubresourceLoader::didFinishLoading(double) + 1353
18   CFNetwork                          0x00007fffaad70d73 __65-[NSURLConnectionInternal _withConnectionAndDelegate:onlyActive:]_block_invoke + 72
19   CFNetwork                          0x00007fffaad70c07 -[NSURLConnectionInternal _withConnectionAndDelegate:onlyActive:] + 198
20   CFNetwork                          0x00007fffaad70bc5 -[NSURLConnectionInternal _withActiveConnectionAndDelegate:] + 48
21   CFNetwork                          0x00007fffaad7517c invocation function for block in URLConnectionClient_Classic::_delegate_didFinishLoading(void () block_pointer) + 104
22   CFNetwork                          0x00007fffaaf98a27 invocation function for block in URLConnectionClient_Classic::_withDelegateAsync(char const*, void (_CFURLConnection*, CFURLConnectionClientCurrent_VMax const*) block_pointer) + 100
23   libdispatch.dylib                  0x00000001009fbfc4 _dispatch_client_callout + 8
24   libdispatch.dylib                  0x0000000100a118db _dispatch_block_invoke_direct + 553
25   CFNetwork                          0x00007fffaad70aa8 RunloopBlockContext::_invoke_block(void const*, void*) + 24
26   CoreFoundation                     0x00007fffabb3e1d0 CFArrayApplyFunction + 68
27   CFNetwork                          0x00007fffaad70930 RunloopBlockContext::perform() + 137
28   CFNetwork                          0x00007fffaad70736 MultiplexerSource::perform() + 282
29   CFNetwork                          0x00007fffaad7062a MultiplexerSource::_perform(void*) + 72
30   CoreFoundation                     0x00007fffabb9b4a0 __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__ + 17
31   CoreFoundation                     0x00007fffabb7c3f0 __CFRunLoopDoSources0 + 423
32   CoreFoundation                     0x00007fffabb7b770 __CFRunLoopRun + 934
33   CoreFoundation                     0x00007fffabb7b370 CFRunLoopRunSpecific + 420
34   HIToolbox                          0x00007fffab118ecc RunCurrentEventLoopInMode + 240
35   HIToolbox                          0x00007fffab118c41 ReceiveNextEventCommon + 432
36   HIToolbox                          0x00007fffab118bdf _BlockUntilNextEventMatchingListInModeWithFilter + 71
37   AppKit                             0x00007fffa9802734 _DPSNextEvent + 1093
38   AppKit                             0x00007fffa9f17b5e -[NSApplication(NSEvent) _nextEventMatchingEventMask:untilDate:inMode:dequeue:] + 1637
39   AppKit                             0x00007fffa97f719f -[NSApplication run] + 926
40   AppKit                             0x00007fffa97c1cd8 NSApplicationMain + 1237
41   BlogTracker                        0x0000000100006200 main + 84
42   libdyld.dylib                      0x00007fffc0c46254 start + 1
```



