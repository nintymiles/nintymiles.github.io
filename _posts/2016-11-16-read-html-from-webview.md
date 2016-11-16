---
layout: post
title: How to read HTML content from a UIWebView or by using String
---
## How to read HTML content from a UIWebView
Copy from stackoverflow

The second question is actually easier to answer. Look at the stringWithContentsOfURL:encoding:error: method of NSString - it lets you pass in a URL as an instance of NSURL (which can easily be instantiated from NSString) and returns a string with the complete contents of the page at that URL. For example:

```
NSString *googleString = @"http://www.google.com";
NSURL *googleURL = [NSURL URLWithString:googleString];
NSError *error;
NSString *googlePage = [NSString stringWithContentsOfURL:googleURL 
                                                encoding:NSASCIIStringEncoding
                                                   error:&error];
```

After running this code, googlePage will contain the HTML for www.google.com, and error will contain any errors encountered in the fetch. (You should check the contents of error after the fetch.) 

>Html页面中包含js等后台请求脚本，直接获取html数据时，脚本无法解析。

Going the other way (from a UIWebView) is a bit trickier, but is basically the same concept. You'll have to pull the request from the view, then do the fetch as before:

```
NSURL *requestURL = [[yourWebView request] URL];
NSError *error;
NSString *page = [NSString stringWithContentsOfURL:requestURL 
                                          encoding:NSASCIIStringEncoding
                                             error:&error];
```

EDIT: Both these methods take a performance hit, however, since they do the request twice. You can get around this by grabbing the content from a currently-loaded UIWebView using its`stringByEvaluatingJavascriptFromString:` method, as such:

```
NSString *html = [yourWebView stringByEvaluatingJavaScriptFromString: 
                                         @"document.body.innerHTML"];
```

This will grab the current HTML contents of the view using the Document Object Model, parse the Javascript, then give it to you as an NSString* of HTML.

Another way is to do your request programmatically first, then load the UIWebView from what you requested. Let's say you take the second example above, where you have NSString *page as the result of a call to stringWithContentsOfURL:encoding:error:. You can then push that string into the web view using loadHTMLString:baseURL:, assuming you also held on to the NSURL you requested:

```
[yourWebView loadHTMLString:page baseURL:requestURL];
```

I'm not sure, however, if this will run Javascript found in the page you load (the method name, loadHTMLString, is somewhat ambiguous, and the docs don't say much about it). 



