---
layout: post
title: Learning Notes - NSURLSession
---

## NSURLSession
Prior to iOS 7 and OS X 10.9, when a developer wanted to retrieve contents from a URL, he/she used the NSURLConnection API. Starting with iOS 7 and OS X 10.9, NSURLSession became the **preferred** API. The NSURLSession API can be thought of as an improvement to the older NSURLConnection API.

- NSURLSession: This is the main session object. It was written as a replacement for the older NSURLConnection API.
- NSURLSessionConfiguration: This is used to configure the behavior of the NSURLSession object.
- NSURLSessionTask: This is a base class to handle the data being retrieved from the URL. Apple provides three concrete subclasses of the NSURLSessionTask class.
- NSURL: This is an object that represents the URL to connect to.
- NSMutableURLRequest: This class contains information about the request that we are making and is **used by the NSURLSessionTask service to make the request**.
- NSHTTPURLResponse: This class contains the response to our request.

In addition, the NSURLSession API provides four protocols that define delegate methods your app can implement to provide more fine-grained control over session and task behavior.

- NSURLSessionDelegate—Defines delegate methods to handle session-level events
- NSURLSessionTaskDelegate—Defines delegate methods to handle task-level events common to all task types
- NSURLSessionDataDelegate—Defines delegate methods to handle task-level events specific to data and upload tasks
- NSURLSessionDownloadDelegate—Defines delegate methods to handle task-level events specific to download tasks
- NSURLSessionStreamDelegate—Defines delegate methods to handle task-level events specific to stream tasks

