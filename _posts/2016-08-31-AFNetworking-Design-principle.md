---
layout: post
title:  Learning Notes - AFNetworking Principles
---
## AFURLResponseSerializer
The `AFURLResponseSerialization` protocol is adopted by an object that **decodes data into a more useful object representation**, according to details in the server response. Response serializers may additionally perform *validation* on the incoming response and data.

 For example, a JSON response serializer may check for an acceptable status code (`2XX` range) and content type (`application/json`), decoding a valid JSON response into an object. 

### HTTP 状态码被分成了五大类
状态码为客户端提供了一种理解事务处理结果的便捷方式。

- 100～199——信息性状态码
- 200～299——成功状态码
- 300～399——重定向状态码
- 400～499——**客户端错误**状态码
- 500～599——服务器错误状态码

## AFHTTPResponseSerializer
`AFHTTPResponseSerializer` conforms to the `AFURLRequestSerialization` & `AFURLResponseSerialization` protocols, offering a concrete base implementation of query string / URL form-encoded parameter serialization and default request headers, as well as response status code and content type validation.

 Any request or response serializer dealing with HTTP is encouraged to subclass `AFHTTPResponseSerializer` in order to ensure consistent default behavior.

## AFJSONResponseSerializer
`AFJSONResponseSerializer` is a subclass of `AFHTTPResponseSerializer` that **validates** and **decodes** JSON responses.

 By default, `AFJSONResponseSerializer` accepts the following MIME types, which includes the official standard, `application/json`, as well as other *commonly-used* types:

 - `application/json`
 - `text/json`
 - `text/javascript`

## AFImageResponseSerializer
`AFImageResponseSerializer` is a subclass of `AFHTTPResponseSerializer` that validates and decodes image responses.

 By default, `AFImageResponseSerializer` accepts the following MIME types, which correspond to the image formats supported by UIImage or NSImage:

 - `image/tiff`
 - `image/jpeg`
 - `image/gif`
 - `image/png`
 - `image/ico`
 - `image/x-icon`
 - `image/bmp`
 - `image/x-bmp`
 - `image/x-xbitmap`
 - `image/x-win-bitmap`

## AFCompoundSerializer
`AFCompoundSerializer` is a subclass of `AFHTTPResponseSerializer` that delegates the response serialization to the first `AFHTTPResponseSerializer` object that returns an object for `responseObjectForResponse:data:error:`, falling back on the default behavior of `AFHTTPResponseSerializer`. This is useful for *supporting multiple potential types and structures of server responses* with a single serializer.


## AFURLRequestSerializier 对 Multipart 数据的处理
首先使用类方法产生 Multipart Request

```
//使用AFURLRequestSerialization生成一个MultiPart Request
- (NSMutableURLRequest *)multipartFormRequestWithMethod:(NSString *)method
                                              URLString:(NSString *)URLString
                                             parameters:(NSDictionary *)parameters
                              constructingBodyWithBlock:(void (^)(id <AFMultipartFormData> formData))block
                                                  error:(NSError *__autoreleasing *)error
{
    NSParameterAssert(method);
    NSParameterAssert(![method isEqualToString:@"GET"] && ![method isEqualToString:@"HEAD"]);

    NSMutableURLRequest *mutableRequest = [self requestWithMethod:method URLString:URLString parameters:nil error:error];

    __block AFStreamingMultipartFormData *formData = [[AFStreamingMultipartFormData alloc] initWithURLRequest:mutableRequest stringEncoding:NSUTF8StringEncoding];

    if (parameters) {
        for (AFQueryStringPair *pair in AFQueryStringPairsFromDictionary(parameters)) {
            NSData *data = nil;
            if ([pair.value isKindOfClass:[NSData class]]) {
                data = pair.value;
            } else if ([pair.value isEqual:[NSNull null]]) {
                data = [NSData data];
            } else {
                data = [[pair.value description] dataUsingEncoding:self.stringEncoding];
            }

            if (data) {
                [formData appendPartWithFormData:data name:[pair.field description]];
            }
        }
    }

    if (block) {
        block(formData);
    }

    return [formData requestByFinalizingMultipartFormData];
}
```

- 然后使用 AFStreamingMultipartFormData 封装request data，最终产生可用的request

```
@interface AFStreamingMultipartFormData : NSObject <AFMultipartFormData>
- (instancetype)initWithURLRequest:(NSMutableURLRequest *)urlRequest
                    stringEncoding:(NSStringEncoding)encoding;

- (NSMutableURLRequest *)requestByFinalizingMultipartFormData;  //层层封装后最终在此处产生request
@end
```

- AFMultiBodyStream (和AFHTTPBodyPart一样都继承自NSInputStream)

```
- (NSInteger)read:(uint8_t *)buffer
        maxLength:(NSUInteger)length
{
    if ([self streamStatus] == NSStreamStatusClosed) {
        return 0;
    }

    NSInteger totalNumberOfBytesRead = 0;

    while ((NSUInteger)totalNumberOfBytesRead < MIN(length, self.numberOfBytesInPacket)) {
        if (!self.currentHTTPBodyPart || ![self.currentHTTPBodyPart hasBytesAvailable]) {
            if (!(self.currentHTTPBodyPart = [self.HTTPBodyPartEnumerator nextObject])) {
                break;
            }
        } else {
            NSUInteger maxLength = MIN(length, self.numberOfBytesInPacket) - (NSUInteger)totalNumberOfBytesRead;
            NSInteger numberOfBytesRead = [self.currentHTTPBodyPart read:&buffer[totalNumberOfBytesRead] maxLength:maxLength];  //此处使用AFHTTPBodyPart将数据读入缓冲区
            if (numberOfBytesRead == -1) {
                self.streamError = self.currentHTTPBodyPart.inputStream.streamError;
                break;
            } else {  //除非读不到，否则累加读取到的总字节数
                totalNumberOfBytesRead += numberOfBytesRead;

                if (self.delay > 0.0f) {
                    [NSThread sleepForTimeInterval:self.delay];
                }
            }
        }
    }

    return totalNumberOfBytesRead;
}

```

- AFHTTPBodyPart() 扩展

```
//HTTPBodyPart读取文件和数据－流－的地方
- (NSInputStream *)inputStream {
    if (!_inputStream) {
        if ([self.body isKindOfClass:[NSData class]]) {
            _inputStream = [NSInputStream inputStreamWithData:self.body];
        } else if ([self.body isKindOfClass:[NSURL class]]) {
            _inputStream = [NSInputStream inputStreamWithURL:self.body];
        } else if ([self.body isKindOfClass:[NSInputStream class]]) {
            _inputStream = self.body;
        } else {
            _inputStream = [NSInputStream inputStreamWithData:[NSData data]];
        }
    }

    return _inputStream;
}
```

```
- (NSInteger)read:(uint8_t *)buffer
        maxLength:(NSUInteger)length
{
    NSInteger totalNumberOfBytesRead = 0;

    if (_phase == AFEncapsulationBoundaryPhase) {
        NSData *encapsulationBoundaryData = [([self hasInitialBoundary] ? AFMultipartFormInitialBoundary(self.boundary) : AFMultipartFormEncapsulationBoundary(self.boundary)) dataUsingEncoding:self.stringEncoding];
        totalNumberOfBytesRead += [self readData:encapsulationBoundaryData intoBuffer:&buffer[totalNumberOfBytesRead] maxLength:(length - (NSUInteger)totalNumberOfBytesRead)];
    }

    if (_phase == AFHeaderPhase) {
        NSData *headersData = [[self stringForHeaders] dataUsingEncoding:self.stringEncoding];
        totalNumberOfBytesRead += [self readData:headersData intoBuffer:&buffer[totalNumberOfBytesRead] maxLength:(length - (NSUInteger)totalNumberOfBytesRead)];
    }

    if (_phase == AFBodyPhase) {
        NSInteger numberOfBytesRead = 0;
        
        //从NSURL或者NSData等生成的inputStream 中读取具体数据
        numberOfBytesRead = [self.inputStream read:&buffer[totalNumberOfBytesRead] maxLength:(length - (NSUInteger)totalNumberOfBytesRead)];
        if (numberOfBytesRead == -1) {
            return -1;
        } else {
            totalNumberOfBytesRead += numberOfBytesRead;

            if ([self.inputStream streamStatus] >= NSStreamStatusAtEnd) {
                [self transitionToNextPhase];
            }
        }
    }

    if (_phase == AFFinalBoundaryPhase) {
        NSData *closingBoundaryData = ([self hasFinalBoundary] ? [AFMultipartFormFinalBoundary(self.boundary) dataUsingEncoding:self.stringEncoding] : [NSData data]);
        totalNumberOfBytesRead += [self readData:closingBoundaryData intoBuffer:&buffer[totalNumberOfBytesRead] maxLength:(length - (NSUInteger)totalNumberOfBytesRead)];
    }

    return totalNumberOfBytesRead;
}
```




