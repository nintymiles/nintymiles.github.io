---
layout: post
title:  Learning Notes - ObjC Rules of Memory Management
---
##Base64 and Percent Encoding**Cocoa has long needed easy access to Base64 encoding and decoding**. *Base64 is the standard for many web protocols and is useful for many cases in which you need to store arbitrary data in a string*.In iOS 7, new NSData methods such as `initWithBase64EncodedString:options:` and `base64EncodedStringWithOptions:` are available for converting between Base64 and NSData.
**Percent encoding is also important for web protocols, particularly *URLs***. You can now decode percent-encoded strings using `[NSString stringByRemovingPercentEncoding]`. Although it has always been possible to percent-encode strings with `stringByAddingPercentEscapesUsingEncoding:`, iOS 7 adds `stringByAddingPercentEncodingWithAllowedCharacters:`, which allows you to control which characters are percent-encoded. 

##Rules of Memory ManagementNow that we know the details of these lifetime qualifiers, it is important to review some basic rules of memory management.As per Apple’s official documentation, there are four basic rules of memory manage‐ ment:• You own any object you create, using, for example, new, alloc, copy, or mutable Copy.• You can take ownership of any object using retain in MRC or a __strong refer‐ ence in ARC.• You must relinquish ownership of an owned object when you no longer need it using release in MRC. It’s not necessary to do anything special in ARC. The      Rules of Memory Management | 51
ownership will be relinquished after the last reference to the owned object (i.e., the last line in a method).• You must not relinquish ownership of any object that you do not own.To help avoid memory leaks or app crashes, you should keep these rules handy whenwriting Objective-C code.

