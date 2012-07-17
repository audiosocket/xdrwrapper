xdrwrapper
==========

Internet Explorer's `XDomainRequest` infamously only supports `GET` and `POST` HTTP verbs. 

`xdrwrapper` can be used to wrap a full range of HTTP requests into a single `POST` json requests
that is then translated by your API server.

How to use?
-----------

`XDRWrapper` should be setup using JQuery's `$.ajaxTransport` method as follows:

```
if window.XDomainRequest?
  $.ajaxTransport "html json", (settings, original, xhr) ->
    XDRWrapper settings, original, xhr if isAPIRequest(settings, original, xhr)
```

How it works?
-------------

All requests issues using `XDRWrapper` are wrapped into a single `POST` JSON request. For instance the following request:

```
PUT /api/foo HTTP/1.1
<headers>
    
<data>
```
    
is translated into:

```
POST /api/foo?xdr HTTP/1.1
Content-Type: application/json;charset=UTF-8
Content-Length: (...)
    
["PUT", <headers>, <data>]
```
    
Your API server should then detect xdr wrapping using the `?xdr` parameter and respond with a wrapped response.
For instance, the following response:

```
HTTP/1.1 418 I'm a teapot
<headers>
    
<data>
```
    
should be wrapped into:

```
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Content-Length: (..)
    
[418, <headrs>, <data>]
```
    
`XDRWrapper` will then unwrap this response and return the intended API response.
    
    
    
