---
layout: post
title:  Learning Notes - Three basic iOS app security tenets
---
##Three basic iOS app security tenets
For developers, three are the basic tenets of iOS app security, according to Guido:

1. Using **HTTPS** exclusively. This is accomplished by always specifying https in the URL. In such cases, NSURLConnection and NSURLSession use App Transport Security (ATS), which *ensures you use properly TLS 1.2, certificate validation*, and so on. Guido additionally suggests using TrustKit, a framework that makes it easy to deploy SSL public key pinning in any iOS or OS X App and to monitor pinning validation failures.
2. Using **encryption**. DPAPI and the Keychain should be used to encrypt ***all*** files, *passwords*, and *tokens*. Guido spends a few moments to describe the options that are available with DPAPI:
	- to **encrypt** files: NSFileProtectionComplete, NSFileProtectionCompleteUnlessOpen, NSFileProtectionCompleteUntilFirstAuth;
	- to **write NSData** to disk: NSDataWritingProtectionComplete, NSDataWritingProtectionCompleteUnlessOpen, NSDataWritingProtectionCompleteUntilFirstAuth.
3. **Cleaning up**. This is a fundamental step to avoid leaving sensitive data behind. While it is true that nearly all files are encrypted when stored on disk, Guido warns against a number of pitfalls. In particular, **iTunes backups are stored unencrypted**, so it becomes possible to steal sensitive data by attacking a desktop OS instead of iOS. Guido mentions you can prevent syncing to iCloud or iTunes by using the **NSURLIsExcludedFromBackupKey** key. Additionally, apps usually leave behind a certain amount of potentially sensitive, unencrypted data in the ***UIPasteboard***, or in the URL or keyboard cache, the *app backgrounding snapshot* that iOS automatically takes, *cookies*, *NSLog entries*, and so on. All of these risks can be avoided by following the proper policy, e.g., overriding applicationDidEnterBackground and setting hidden to YES; use secureTextEntry and disabling autocorrection to *circumvent* the keyboard cache, etc. 

