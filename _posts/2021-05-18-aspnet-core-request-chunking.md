---
layout: post
title: ASP.NET Core request chunking
date: 2021-05-18 21:41:19
author: Peter Stevenson
summary: How to prevent ASP.NET Core sending requests chunked
categories: programming
thumbnail:
tags:
 - API
 - .NET Core
 - .NET
 - ASP.NET
 - .NET Standard
 - C#
---

# How to prevent ASP.NET Core sending requests chunked

Basically ASP.NET Core HttpClient sends requests chunked by default. This was not the case for .NET Framework. This can cause some issues for compatibility. 

I ran into a problem where a .NET Core library _or a .NET Standard library_ was calling a .NET Framework Web API using HttpClient and the .NET Framework Web API failed to deserialize the request body. 

Previously I never had a issue with .NET Framework HttpClient.

## Request analysis

### .NET Framework

#### Request

```
POST http://redacted.redacted.com/redacted/redacted/redacted HTTP/1.1
Authorization: Bearer redacted
Content-Type: application/json; charset=utf-8
Host: redacted.redacted.com
Content-Length: 509
Expect: 100-continue
Connection: Keep-Alive

{"redacted":"redacted","redacted":"redacted","redacted":redacted,"redacted":"redacted"}
```

#### Response

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 681
Content-Type: application/json; charset=utf-8
Expires: -1
Server: Microsoft-IIS/redacted
X-AspNet-Version: redacted
X-Powered-By: ASP.NET
Date: Tue, 18 May 2021 11:29:14 GMT

{
  "redacted": redacted,
  "redacted": "redacted",
...
```

### .NET Core

#### Request

```
POST http://redacted.redacted.com/redacted/redacted/redacted HTTP/1.1
Authorization: Bearer redacted
Transfer-Encoding: chunked
Content-Type: application/json; charset=utf-8
Host: redacted.redacted.com

1FD
{"redacted":"redacted","redacted":"redacted","redacted":redacted,"redacted":"redacted"}
0
```

* Notice the `Transfer-Encoding: chunked` along with `1FD` before and `0` after the JSON body.
* Notce the lack of `Content-Length` header.

#### Response

```
HTTP/1.1 400 Bad Request
Cache-Control: no-cache
Pragma: no-cache
Content-Type: application/json; charset=utf-8
Expires: -1
Server: Microsoft-IIS/redacted
X-AspNet-Version: redacted
X-Powered-By: ASP.NET
Date: Tue, 18 May 2021 11:29:59 GMT
Content-Length: 78

{
  "message": "redacted"
}
```

### Insomnia

#### Request

```
POST http://redacted.redacted.com/redacted/redacted/redacted HTTP/1.1
Host: redacted.redacted.com
User-Agent: insomnia/2021.3.0
Connection: Keep-Alive
Authorization: Bearer redacted
Content-Type: application/json
Accept: */*
Content-Length: 564

{
	"redacted": "redacted",
	"redacted": "redacted",
...
```

#### Response

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 688
Content-Type: application/json; charset=utf-8
Expires: -1
Server: Microsoft-IIS/redacted
X-AspNet-Version: redacted
X-Powered-By: ASP.NET
Date: Tue, 18 May 2021 20:29:31 GMT

{
  "redacted": redacted,
  "redacted": "redacted",
...
```

## Research links

* [Why httpclient always Transfer-Encoding: chunked?](https://developercommunity.visualstudio.com/t/why-httpclient-always-transfer-encoding-chunked/878468)
* [Web api 2.1 does not work with a chunked request](https://forums.asp.net/t/1978292.aspx?Web+api+2+1+does+not+work+with+a+chunked+request)
	* \> Microsoft.AspNet.WebApi
	* \> FROM version="5.2.3" TO version="5.2.7"
* [runtime - HttpClient issue when calling older ASP.Net webAPIs from net core](https://github.com/dotnet/runtime/issues/28655)
	* [#issuecomment-462185964](https://github.com/dotnet/runtime/issues/28655#issuecomment-462185964)
	* [#issuecomment-462270800](https://github.com/dotnet/runtime/issues/28655#issuecomment-462270800)
		* \> chrizy commented on 11 Feb 2019
		* \> Thanks for the feedback, I suspect the chunked is the issue. The problem as I see it is by default no net core application can talk to older asp net 5.2.3 as asp.net doesn't support or understand the data sent, and does not give an clear indication as to the problem. It seems like an odd default position to be in.
		* \> Also changing each http request (talking about over 70 apis here) in my net core application to disable chunked is not easy, Unless there is some global setting which can control this? or is there a way to get asp.net to support it?
* [AspNetWebStack - HttpClient issue when calling older ASP.Net webAPIs from net core](https://github.com/aspnet/AspNetWebStack/issues/232)
* [runetime - HttpClient ignores TransferEncodingChunked = false if content doesn't specify content length](https://github.com/dotnet/runtime/issues/30283)
	* [#issuecomment-513041637](https://github.com/dotnet/runtime/issues/30283#issuecomment-513041637)
		* \> geoffkizer commented on 19 Jul 2019
		* \> BTW, for doing the actual buffering, we should be able to just use HttpContent.LoadIntoBufferAsync, I believe.

## Code fix

Change the following code.

```csharp
HttpResponseMessage httpResponseMessage = await httpClient.PostAsJsonAsync(apiUrl, obj);
```

to this.

```csharp
string str = JsonConvert.SerializeObject(obj);
HttpContent httpContent = new StringContent(str, System.Text.Encoding.UTF8, "application/json");
await httpContent.LoadIntoBufferAsync();
HttpResponseMessage httpResponseMessage = await httpClient.PostAsync(apiUrl, httpContent);
```