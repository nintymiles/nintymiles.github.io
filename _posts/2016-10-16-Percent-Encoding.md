---
layout: post
title:  Learning Notes -  About Percent Encoding
---
## About Percent Encoding
RFC 3986 states that the following characters are "reserved" characters.
 - General Delimiters: ":", "#", "[", "]", "@", "?", "/"
 - Sub-Delimiters: "!", "$", "&", "'", "(", ")", "*", "+", ",", ";", "="

 In RFC 3986 - Section 3.4, it states that the "?" and "/" characters should not be escaped to allow
 query strings to include a URL. Therefore, all "reserved" characters with the exception of "?" and "/"
 should be percent-escaped in the query string.  
 
 > RF3986协议规定了保留字符，针对百分号编码，所有的保留字符除了“？”和“／”之外都应该被编码

