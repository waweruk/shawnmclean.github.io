---
layout: post
title: ASP.NET Webapi same origin policy workaround using Custom Http Headers
date: 2012-04-29 19:04
author: shawnmclean
comments: true
categories: [.Net Framework, asp.net-webapi]
---
Recently, I'm working on an API that should be accessed by both javascript and desktop clients. During debugging of the javascript client, I ran into the <a href="http://en.wikipedia.org/wiki/Same_origin_policy">Same Origin Policy</a> problem which prevented my javascript app to make XHR to the API. Both the client and the API are hosted on different ports which triggers this policy. <em>localhost:35000</em> and <em>localhost:35001</em>

One such workaround is to have the API utilize <a href="http://www.w3.org/TR/cors/">Cross-Origin Resource Sharing</a>. This is basically sending back the header `<code>Access-Control-Allow-Origin: *</code>` the asterisk(allow all domains) can be replaced with specific domain names. I utilized a custom http header attribute that can be used on the Controller or the Action;

<script src="https://gist.github.com/2552652.js?file=HttpHeaderAttribute.cs"></script>

Using it:

<script src="https://gist.github.com/2552696.js?file=UserController.cs"></script>

The headers should now look like:

<code>Access-Control-Allow-Origin:*
Cache-Control:no-cache
Connection:Close
Content-Type:application/json; charset=utf-8
Date:Sun, 29 Apr 2012 18:39:05 GMT
Expires:-1
Pragma:no-cache
Server:ASP.NET Development Server/11.0.0.0
Transfer-Encoding:chunked
X-AspNet-Version:4.0.30319
</code>

NOTE: This will work for <strong>GET </strong>requests. Any other method will fail, such as POST or PUT. The browser will send a pre-flight request which contains the method OPTIONS, which would not be mapped to the action and the ASP.NET routing engine will just return an error. If you need CORS support for all methods, use a DelegatingHandler, explained here: <a href="http://code.msdn.microsoft.com/windowsdesktop/Implementing-CORS-support-a677ab5d">http://code.msdn.microsoft.com/windowsdesktop/Implementing-CORS-support-a677ab5d</a>
