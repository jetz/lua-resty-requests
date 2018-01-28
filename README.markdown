Name
====

lua-resty-requests - Yet Another HTTP Library for OpenResty.

Table of Contents
=================

* [Name](#name)
* [Synopsis](#synopsis)
* [Status](#status)
* [Methods](#methods)
	* [request](#request)
	* [state](#state)
	* [get](#get)
	* [head](#head)
	* [post](#post)
	* [put](#put)
	* [post](#post)
	* [delete](#delete)
	* [options](#options)
	* [patch](#patch)
* [Response Object](#response-object)
* [Connection Management](#connection-management)
* [Session](#session)
* [Author](#author)
* [Copyright and License](#copyright-and-license)

Status
======

This Lua module is currently considered experimental.

Synopsis
========

```lua
local requests = require "resty.requests"

-- example url
local url = "http://example.com/index.html"

local r, err = requests.get(url)
if not r then
    ngx.log(ngx.ERR, err)
    return
end

-- read all body
local body = r:body()
ngx.print(body)

-- or you can iterate the response body
-- while true do
--     local chunk, err = r:iter_content(4096)
--     if not chunk then
--         ngx.log(ngx.ERR, err)
--         return
--     end
--
--     if chunk == "" then
--         break
--     end
--
--     ngx.print(chunk)
-- end
```

Methods
=======

### request

**syntax**: *local r, err = requests.request(method, url, opts?)*

This is the pivotal method in `lua-resty-requests`, it will return a [response object](response-object) `r`. In the case of fail, `nil` and a Lua string which describles the corresponding error will be given.

The first param `method`, is the HTTP method that you want to use(same as
HTTP's semantic), which takes a Lua string and the value can be:

* `GET`
* `HEAD`
* `POST`
* `PUT`
* `DELETE`
* `OPTIONS`
* `PATCH`

The second param `url`, just takes the literal meaning(i.e. Uniform Resource Location),
for instance, `http://foo.com/blah?a=b`, you can omit the scheme prefix and as the default scheme,
`http` will be selected.

The third param, an optional Lua table, which contains a number of  options:

* `headers` holds the custom request headers.

* `allow_redirects` specifies whether redirecting to the target url(specified by `Location` header) or not when the status code is `301` or `302`(`303`, `307` and `308` haven't been supported yet).

* `redirect_max_times` specifies the redirect limits, default is 10.

* `body`, the request body, can be:
	* a Lua string, or
	* a Lua function, takes no param and return a piece of data(string) or an empty Lua string to represent EOF

* `error_filter`,
 holds a Lua function which takes two params, `state` and `err`. 
 the param `err` describes the error and `state` is always one of these values(represents the current stage):

 ```lua
 requests.CONNECT
 requests.HANDSHAKE
 requests.SEND_HEADER
 requests.SEND_BODY
 requests.RECV_HEADER
 requests.RECV_BODY
 requests.CLOSE
 ```


* `timeouts`, which is an array-like table, `timeouts[1]`, `timeouts[2]` and `timeouts[3]` represents `connect timeout`, `send timeout` and `read timeout` respectively.

* `http10` specify whether the `HTTP/1.0` should be used, the default verion is `HTTP/1.1`.

* `ssl` holds a Lua table, with three fields:
	* `verify`, controls whether to perform SSL verification
	* `server_name`, is used to specify the server name for the new TLS extension Server Name Indication (SNI)
	* `reused_session`, takes a former SSL session userdata returned by a previous sslhandshake call for exactly the same target

* `proxies` specify proxy servers, the form is like

```lua
{
    http = { host = "127.0.0.1", port = 80 },
    https = { host = "192.168.1.3", port = 443 },
}
```

There also some "short path" options:

* `auth`, to do the Basic HTTP Authorization, takes a Lua table contains `user` and `pass`, e.g. when `auth` is:

```lua
{
	name = "alex",
	pass = "123456"
}
```

Request header `Authorzation` will be added, and the value is `Basic bmlsOm5pbA==`.

* `json`, takes a Lua table, it will be serialized by `cjson`, the serialized data will be sent as the request body, and it takes the priority when both `json` and `body` is specified.

* `cookie`, takes a Lua table, the key-value pairs will be organized according to the `Cookie` header's rule, e.g. `cookies` is:

```lua
{
	["PHPSESSID"] = "298zf09hf012fh2",
	["csrftoken"] = "u32t4o3tb3gg43"
}
```

The `Cookie` header will be `PHPSESSID=298zf09hf012fh2; csrftoken=u32t4o3tb3gg43 `.

### state

### get

### head

### post

### put

### post

### delete

### options

### patch

Response Object
===============

Session
=======

Not implemented yet.


Author
======

Alex Zhang(张超) zchao1995@gmail.com, UPYUN Inc.

Copyright and License
=====================

The bundle itself is licensed under the 2-clause BSD license.

Copyright (c) 2017-2018, Alex Zhang.

This module is licensed under the terms of the BSD license.

Redistribution and use in source and binary forms, with or without modification, are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.
THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.