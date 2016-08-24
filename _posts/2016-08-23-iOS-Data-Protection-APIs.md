---
layout: post
title:  Learning Notes - iOS Data Protection APIs
---

## iOS Data Protection APIs
There are some FileProtection options you can set when writing an NSData to disk:

- NSDataWritingFileProtectionComplete
- NSDataWritingFileProtectionNone)

as well as an extended attribute, NSFileProtectionKey, you can set on pre-existing files on disk via NSFileManager:

- NSFileProtectionComplete
- NSFileProtectionNone

The app delegate is also informed of when your application is (not) going to be able to access protected data:

- -applicationProtectedDataDidBecomeAvailable:
- -applicationProtectedDataWillBecomeUnavailable:

All the gory details of encrypting and securing the data are handled by the hardware and OS on your behalf. It's fire-and-forget protection that kicks in whenever the device locks. 

## About iOS Data Protection (official description)
Data protection is available for devices that offer hardware encryption, including iPhone 3GS and later, all iPad models, and iPod touch (3rd generation and later). Data protection enhances the built-in hardware encryption by protecting the hardware encryption keys with your passcode. This provides an additional layer of protection for your email messages attachments, and third-party applications.


