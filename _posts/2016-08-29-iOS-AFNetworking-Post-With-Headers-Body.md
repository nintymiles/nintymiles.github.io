---
layout: post
title:  Learning Notes - AFNetworking how to POST with headers and HTTP Body
---
##AFNetworking 3.0 Migration: how to POST with headers and HTTP Body
First, you need to create the `NSMutableURLRequest` from `AFJSONRequestSerializer` first where you can set the method type to POST.

On this request, you get to `setHTTPBody` after you have set your `HTTPHeaderFields`. Make sure to set the body after you have set the Header fields for content-type, or else the api will give a 400 error.

Then on the manager create a dataTaskWithRequest using the above NSMutableURLRequest. Don't forget to resume the dataTask at the very end or else nothing will get sent yet. Here's my solution code, hopefully someone gets to use this successfully:

```
NSDictionary *body = @{@"snippet": @{@"topLevelComment":@{@"snippet":@{@"textOriginal":self.commentToPost.text}},@"videoId":self.videoIdPostingOn}};
NSError *error;
NSData *jsonData = [NSJSONSerialization dataWithJSONObject:body options:0 error:&error];
NSString *jsonString = [[NSString alloc] initWithData:jsonData encoding:NSUTF8StringEncoding];

AFURLSessionManager *manager = [[AFURLSessionManager alloc] initWithSessionConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]];

NSMutableURLRequest *req = [[AFJSONRequestSerializer serializer] requestWithMethod:@"POST" URLString:[NSString stringWithFormat:@"https://www.googleapis.com/youtube/v3/commentThreads?part=snippet&access_token=%@",
[[LoginSingleton sharedInstance] getaccesstoken]] parameters:nil error:nil];

req.timeoutInterval= [[[NSUserDefaults standardUserDefaults] valueForKey:@"timeoutInterval"] longValue];
[req setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
[req setValue:@"application/json" forHTTPHeaderField:@"Accept"];
[req setHTTPBody:[jsonString dataUsingEncoding:NSUTF8StringEncoding]];


[[manager dataTaskWithRequest:req completionHandler:^(NSURLResponse * _Nonnull response, id  _Nullable responseObject, NSError * _Nullable error) {

    if (!error) {
        NSLog(@"Reply JSON: %@", responseObject);

        if ([responseObject isKindOfClass:[NSDictionary class]]) {
              //blah blah
        }
    } else {
        NSLog(@"Error: %@, %@, %@", error, response, responseObject);
    }
}] resume]; 
```

