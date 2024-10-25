# Status code documentation

## Informational Responses

### 100 (continue)
Indicates that request has been received and not yet rejected by the server.

Client should continue with request or discard the 100 response if request is already finished

Example
```HTTP
PUT /videos HTTP/1.1
Host: uploads.example.com
Content-Type: video/h264
Content-Length: 123456789
Expect: 100-continue
```
### 101 (switching protocols)
Indicates thee protocol that the server has switched to

Example
```HTTP
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
```
### 102 (Processing)
Indicates a request has been received and the server is working on it

Example

```HTTP
102 Processing
```
### 103 (Early Hints)
Sent by the server while it is still prepairing a response, with hints about the sites and resources that the server expects the final response will link to

```HTTP
103 Early Hint
Link: <https://cdn.example.com>; rel=preconnect, <https://cdn.example.com>; rel=preconnect; crossorigin
```

## Successful responses

### 200 (OK)
Indicates that a request has succeeded
 
 #### Get
 A resource was retrieved by the server and included in the response body
 
 Example
 ```HTTP
 HTTP/1.1 200 OK
Accept-Ranges: bytes
Age: 294510
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Fri, 21 Jun 2024 14:18:33 GMT
Etag: "3147526947"
Expires: Fri, 28 Jun 2024 14:18:33 GMT
Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
Server: ECAcc (nyd/D10E)
X-Cache: HIT
Content-Length: 1256

<!doctype html>
<!-- HTML content follows here -->
 ```
 #### post
  An action succeeded; the response has a message body describing the result
  ```HTTP
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "User subscription pending. A confirmation email has been sent.",
  "subscription": {
    "name": "Brian Smith",
    "email": "brian.smith@example.com",
    "id": 123,
    "feed": "default"
  }
}
  ```
  ### 201 (Created)
  Indicates the request has led to the creation of the resource.
  
  CREATE USER

  ```HTTP
  POST /users HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "firstName": "Brian",
  "lastName": "Smith",
  "email": "brian.smith@example.com"
}
  ```
  USER CREATED

  ```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: http://example.com/users/123

{
  "message": "New user created",
  "user": {
    "id": 123,
    "firstName": "Brian",
    "lastName": "Smith",
    "email": "brian.smith@example.com"
  }
}
  ```
### 202 (Accepted)
Indicates that a request has been accepted for processing, but processing has not been completed or may not have started

```JS
POST /tasks HTTP/1.1
Host: example.com
Content-Type: application/json

{
  "task": "emailDogOwners",
  "template": "pickup"
}
```

```JS
HTTP/1.1 202 Accepted
Date: Wed, 26 Jun 2024 12:00:00 GMT
Server: Apache/2.4.1 (Unix)
Content-Type: application/json

{
  "message": "Request accepted. Starting to process task.",
  "taskId": "123",
  "monitorUrl": "http://example.com/tasks/123/status"
}
```
### 203 (Non-Authoritative Information)
Indicates request has been successful, but transforming proxy has modified the headers enclosed content from the origin server's 200 (OK) response.

In this example, a user sends a GET request for content with ID 123 to example.com.
```js
GET /comments/123 HTTP/1.1
Host: example.com
```

```js
HTTP/1.1 203 Non-Authoritative Information
Date: Wed, 26 Jun 2024 12:00:00 GMT
Server: Apache/2.4.1 (Unix)
Content-Type: application/json
Content-Length: 123

{
  "comment": "Check out my bio!",
  "attachment_url": "https://example.com/attachment-unavailable-faq"
}
```
### 204 (No Content)
 Indicates a request has succeeded, but the client doesn't need to navigate away from its current page.

 ```js
 DELETE /image/123 HTTP/1.1
Host: example.com
Authorization: Bearer 1234abcd
 ```
 ```js
 HTTP/1.1 204 No Content
Date: Wed, 26 Jun 2024 12:00:00 GMT
Server: Apache/2.4.1 (Unix)
Content-Length: 0
```
### 205 (Reset Content)
Indicates that the request has been successfully processed and the client should reset the document view.

Resetting a form after receiving a 205 Reset Content

```js
POST /submit HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

comment=Hello!
```

```js
HTTP/1.1 205 Reset Content
Content-Type: text/html; charset=utf-8
Content-Length: 0
Date: Wed, 26 Jun 2024 12:00:00 GMT
```
### 206 (Partial Content)
Sent in response to a range request.The response body contains the requested ranges of data as specified in the Range header of the request.

Receiving a 206 response for a single requested range
```js
GET /z4d4kWk.gif HTTP/1.1
Host: images.example.com
Range: bytes=21010-
```

```js
HTTP/1.1 206 Partial Content
Date: Wed, 15 Nov 2015 06:25:24 GMT
Last-Modified: Wed, 15 Nov 2015 04:58:08 GMT
Content-Range: bytes 21010-47021/47022
Content-Length: 26012
Content-Type: image/gif
ETag: "abc123"
Accept-Ranges: bytes

# 26012 bytes of partial image data…
```
Receiving a 206 response for multiple requested ranges

```js
GET /price-list.pdf HTTP/1.1
Host: example.com
Range: bytes=234-639,4590-7999
```
```js
HTTP/1.1 206 Partial Content
Date: Wed, 15 Nov 2015 06:25:24 GMT
Last-Modified: Wed, 15 Nov 2015 04:58:08 GMT
Content-Length: 1741
Content-Type: multipart/byteranges; boundary=String_separator
ETag: "abc123"
Accept-Ranges: bytes

--String_separator
Content-Type: application/pdf
Content-Range: bytes 234-639/8000

# content of first range (406 bytes)
--String_separator
Content-Type: application/pdf
Content-Range: bytes 4590-7999/8000

# content of second range (3410 bytes)
--String_separator--
```
### 207 Multi-Status
status code indicates a mixture of responses. This response is used exclusively in the context of Web Distributed Authoring and Versioning (WebDAV).

Receiving a 207 response in a WebDAV context
```js
<?xml version="1.0" encoding="utf-8" ?>
<D:multistatus xmlns:D="DAV:">
  <D:response>
    <D:href>http://www.example.com/Coll/</D:href>
    <D:propstat>
      <D:prop>
        <D:displayname>Loop Demo</D:displayname>
        <D:resource-id>
          <D:href>urn:uuid:f81d4fae-7dec-11d0-a765-00a0c91e6bf8</D:href>
        </D:resource-id>
      </D:prop>
      <D:status>HTTP/1.1 200 OK</D:status>
    </D:propstat>
  </D:response>
  <D:response>
    <D:href>http://www.example.com/Coll/Bar</D:href>
    <D:propstat>
      <D:prop>
        <D:displayname>Loop Demo</D:displayname>
        <D:resource-id>
          <D:href>urn:uuid:f81d4fae-7dec-11d0-a765-00a0c91e6bf8</D:href>
        </D:resource-id>
      </D:prop>
      <D:status>HTTP/1.1 208 Already Reported</D:status>
    </D:propstat>
  </D:response>
</D:multistatus>

```
### 208 (Already Reported)
The HTTP 208 Already Reported successful response status code is used in a 207 Multi-Status response to save space and avoid conflicts. This response is used exclusively in the context of Web Distributed Authoring and Versioning (WebDAV).

Receiving a 208 in a 207 Multi-Status response
```js
Content-Type: application/xml; charset="utf-8"
Content-Length: 1241

<?xml version="1.0" encoding="utf-8" ?>
<D:multistatus xmlns:D="DAV:">
  <D:response>
    <D:href>http://www.example.com/Coll/</D:href>
    <D:propstat>
      <D:prop>
        <D:displayname>Loop Demo</D:displayname>
        <D:resource-id>
          <D:href>urn:uuid:f81d4fae-7dec-11d0-a765-00a0c91e6bf8</D:href>
        </D:resource-id>
      </D:prop>
      <D:status>HTTP/1.1 200 OK</D:status>
    </D:propstat>
  </D:response>
  <D:response>
    <D:href>http://www.example.com/Coll/Foo</D:href>
    <D:propstat>
      <D:prop>
        <D:displayname>Bird Inventory</D:displayname>
        <D:resource-id>
          <D:href>urn:uuid:f81d4fae-7dec-11d0-a765-00a0c91e6bf9</D:href>
        </D:resource-id>
      </D:prop>
      <D:status>HTTP/1.1 200 OK</D:status>
    </D:propstat>
  </D:response>
  <D:response>
    <D:href>http://www.example.com/Coll/Bar</D:href>
    <D:propstat>
      <D:prop>
        <D:displayname>Loop Demo</D:displayname>
        <D:resource-id>
          <D:href>urn:uuid:f81d4fae-7dec-11d0-a765-00a0c91e6bf8</D:href>
        </D:resource-id>
      </D:prop>
      <D:status>HTTP/1.1 208 Already Reported</D:status>
    </D:propstat>
  </D:response>
</D:multistatus>

```
### 226 (IM Used)
The HTTP 226 IM Used successful response status code indicates that the server is returning a delta in response to a GET request. It is used in the context of HTTP delta encodings.

IM stands for instance manipulation, which refers to the algorithm generating a delta. In delta encoding, a client sends a GET request with two headers: A-IM:, which indicates a preference for a differencing algorithm, and If-None-Match, which specifies the version of a resource it has. The server responds with deltas relative to a given base document, rather than the document in full. This response uses the 226 status code, an IM: header that describes the differencing algorithm used, and may include a Delta-Base: header with the ETag matching the base document associated to the delta.

Receiving a 208 with the vcdiff delta algorithm
```js
GET /resource.txt HTTP/1.1
Host: example.com
A-IM: vcdiff, diffe
If-None-Match: "abcd123"
```
```js
HTTP/1.1 226 IM Used
ETag: "5678a23"
IM: vcdiff
Content-Type: text/plain
Content-Length: 123
Delta-Base: abcd123

...
```
## Redirection messages
#### 300 (Multiple Choices)
The request has multiple potential responses, and the user or user agent should select one. No standard exists for automatic selection.

#### 301 (Moved Permanently)
The requested resource's URL has changed permanently, with the new URL provided in the response.

#### 302 Found
The requested resource's URI has changed temporarily; future requests should continue using the original URI.

#### 303 See Other
Indicates the cached version of the resource is still valid, typically used for caching purposes.

### 304 Not Modified
This status required access via a proxy but has been deprecated for security reasons.

#### 306 (Unused)
Reserved status code, no longer in active use.

#### 305 Use Proxy (Deprecated)
Similar to 302, but the client must use the same HTTP method as the original request (e.g., POST remains POST).

#### 308 Permanent Redirect

similar to 301, but retains the HTTP method used in the original request.

### Client error responses

#### 400 Bad Request
The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid request message framing, or deceptive request routing).

#### 401 Unauthorized
Although the HTTP standard specifies "unauthorized", semantically this response means "unauthenticated". That is, the client must authenticate itself to get the requested response.
#### 402 Payment Required
The initial purpose of this code was for digital payment systems, however this status code is rarely used and no standard convention exists.
#### 403 Forbidden
The client does not have access rights to the content; that is, it is unauthorized, so the server is refusing to give the requested resource. Unlike 401 Unauthorized, the client's identity is known to the server.

#### 404 Not Found
The server cannot find the requested resource. In the browser, this means the URL is not recognized. In an API, this can also mean that the endpoint is valid but the resource itself does not exist. Servers may also send this response instead of 403 Forbidden to hide the existence of a resource from an unauthorized client. This response code is probably the most well known due to its frequent occurrence on the web.

#### 405 Method Not Allowed
The request method is known by the server but is not supported by the target resource. For example, an API may not allow DELETE on a resource, or the TRACE method entirely.

#### 406 Not Acceptable
This response is sent when the web server, after performing server-driven content negotiation, doesn't find any content that conforms to the criteria given by the user agent.

#### 407 Proxy Authentication Required
This is similar to 401 Unauthorized but authentication is needed to be done by a proxy.
#### 408 Request Timeout
This response is sent on an idle connection by some servers, even without any previous request by the client. It means that the server would like to shut down this unused connection. This response is used much more since some browsers use HTTP pre-connection mechanisms to speed up browsing. Some servers may shut down a connection without sending this message.
#### 409 Conflict
This response is sent when a request conflicts with the current state of the server. In WebDAV remote web authoring, 409 responses are errors sent to the client so that a user might be able to resolve a conflict and resubmit the request.
#### 410 Gone
This response is sent when the requested content has been permanently deleted from server, with no forwarding address. Clients are expected to remove their caches and links to the resource. The HTTP specification intends this status code to be used for "limited-time, promotional services". APIs should not feel compelled to indicate resources that have been deleted with this status code.

#### 411 Length Required
Server rejected the request because the Content-Length header field is not defined and the server requires it.
#### 412 Precondition Failed
In conditional requests, the client has indicated preconditions in its headers which the server does not meet.
#### 413 Content Too Large
The request body is larger than limits defined by server. The server might close the connection or return an Retry-After header field.
#### 414 URI Too Long
The URI requested by the client is longer than the server is willing to interpret.

#### 415 Unsupported Media Type
The media format of the requested data is not supported by the server, so the server is rejecting the request.

#### 416 Range Not Satisfiable
The ranges specified by the Range header field in the request cannot be fulfilled. It's possible that the range is outside the size of the target resource's data.

#### 417 Expectation Failed
This response code means the expectation indicated by the Expect request header field cannot be met by the server.

#### 418 I'm a teapot
The server refuses the attempt to brew coffee with a teapot.

#### 421 Misdirected Request
The request was directed at a server that is not able to produce a response. This can be sent by a server that is not configured to produce responses for the combination of scheme and authority that are included in the request URI.

#### 422 Unprocessable Content (WebDAV)
The request was well-formed but was unable to be followed due to semantic errors.

#### 423 Locked (WebDAV)
The resource that is being accessed is locked.

#### 424 Failed Dependency (WebDAV)
The request failed due to failure of a previous request.

#### 425 Too Early Experimental
Indicates that the server is unwilling to risk processing a request that might be replayed.

#### 426 Upgrade Required
The server refuses to perform the request using the current protocol but might be willing to do so after the client upgrades to a different protocol. The server sends an Upgrade header in a 426 response to indicate the required protocol(s).

#### 428 Precondition Required
The origin server requires the request to be conditional. This response is intended to prevent the 'lost update' problem, where a client GETs a resource's state, modifies it and PUTs it back to the server, when meanwhile a third party has modified the state on the server, leading to a conflict.

#### 429 Too Many Requests
The user has sent too many requests in a given amount of time (rate limiting).

#### 431 Request Header Fields Too Large
The server is unwilling to process the request because its header fields are too large. The request may be resubmitted after reducing the size of the request header fields.

#### 451 Unavailable For Legal Reasons
The user agent requested a resource that cannot legally be provided, such as a web page censored by a government.

### Server Responses
#### 500 Internal Server Error
The server has encountered a situation it does not know how to handle. This error is generic, indicating that the server cannot find a more appropriate 5XX status code to respond with.

#### 501 Not Implemented
The request method is not supported by the server and cannot be handled. The only methods that servers are required to support (and therefore that must not return this code) are GET and HEAD.

#### 502 Bad Gateway
This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response.

#### 503 Service Unavailable
The server is not ready to handle the request. Common causes are a server that is down for maintenance or that is overloaded. This response should be used for temporary conditions and the Retry-After HTTP header should, if possible, contain the estimated time before the recovery of the service.

#### 504 Gateway Timeout
This error response is given when the server is acting as a gateway and cannot get a response in time.

#### 505 HTTP Version Not Supported
The HTTP version used in the request is not supported by the server.

#### 506 Variant Also Negotiates
The server has an internal configuration error: during content negotiation, the chosen variant is configured to engage in content negotiation itself, resulting in circular references when creating responses.

#### 507 Insufficient Storage (WebDAV)
The method could not be performed on the resource because the server is unable to store the representation needed to successfully complete the request.

#### 508 Loop Detected (WebDAV)
The server detected an infinite loop while processing the request.

#### 510 Not Extended
The client request declares an HTTP Extension (RFC 2774) that should be used to process the request, but the extension is not supported.

#### 511 Network Authentication Required
Indicates that the client needs to authenticate to gain network access.




