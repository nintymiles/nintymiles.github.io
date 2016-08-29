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

## NSURLSessionTask
The NSURLSessionTask class is the **base** class for tasks in a URL session. **Tasks are always part of a session**; you create a task by calling one of the task creation methods on an NSURLSession object. The method you call determines the type of task.

URL sessions provide three types of tasks: data tasks, upload tasks, and download tasks. These tasks are instances of the `NSURLSessionDataTask`, `NSURLSessionUploadTask`, `NSURLSessionDownloadTask`, `NSURLSessionStreamTask` **subclasses** of `NSURLSessionTask`, respectively.

- Data tasks request a resource, returning the server’s response as one or more NSData objects in memory. They are supported in default, ephemeral, and shared sessions, but are not supported in background sessions.

- Upload tasks are like data tasks, except that they make it easier to provide a request body so you can upload data before retrieving the server’s response. Additionally, upload tasks are supported in background sessions.

- Download tasks download a resource directly to a file on disk. Download tasks are supported in any type of session.

- Stream tasks **establish a TCP/IP connection** from a host name and port or a net service object.  验证链接和服务端口

After you create a task, you start it by calling its ***resume*** method. The session then maintains a strong reference to the task until the request **finishes** or **fails**; you do not need to maintain a reference to the task unless it is useful to do so for your app’s internal bookkeeping purposes.


