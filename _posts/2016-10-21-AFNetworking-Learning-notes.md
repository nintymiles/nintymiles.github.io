## AFURLResponseSerialization
The `AFURLResponseSerialization` protocol is adopted by an object that decodes data into a more useful object representation, according to details in the server response. Response serializers may additionally perform validation on the incoming response and data. 根据服务器响应的细节，解码数据为更可用的表达方式。也会对响应和数据额外执行验证。
> 对服务器响应进行进一步封装

For example, a JSON response serializer may check for an acceptable status code (`2XX` range) and content type (`application/json`), decoding a valid JSON response into an object.  比如：检查JSON响应的2xx（成功响应）返回码以及专有的content type
> JSON response serializer主要将响应封装为 JSON 对象

- 协议方法

```swifty
//将特定的响应数据解码为响应对象， 注意命名方法。
- (nullable id)responseObjectForResponse:(nullable NSURLResponse *)response
                           data:(nullable NSData *)data
                          error:(NSError * _Nullable __autoreleasing *)error NS_SWIFT_NOTHROW
```


