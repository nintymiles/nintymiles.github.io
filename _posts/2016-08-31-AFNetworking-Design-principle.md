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
`AFCompoundSerializer` is a subclass of `AFHTTPResponseSerializer` that delegates the response serialization to the first `AFHTTPResponseSerializer` object that returns an object for `responseObjectForResponse:data:error:`, falling back on the default behavior of `AFHTTPResponseSerializer`. This is useful for supporting multiple potential types and structures of server responses with a single serializer.

