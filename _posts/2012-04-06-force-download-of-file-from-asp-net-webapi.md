---
layout: post
title: Force download of file from ASP.NET WebAPI
date: 2012-04-06 21:27

comments: true
categories: [.NET, ASP.NET-WebAPI]
---
How to generate and download content as files from an ASP.NET WebApi action. This example is focused on text content. When returning a file, we actually utilize `<a href="http://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage(v=vs.110).aspx">HttpResponseMessage</a>` for low level modification of http headers.

First we return HttpStatusCode.OK, then set the content to a StringContent, content could be set to many other things such as streams, etc. We then set the Content-Type to 'application/octet-stream', which means the downloaded file is associated with a program that can open/read the file. We then create a Content-Disposition type of 'attachment', which tells the browser to utilize a save-file dialog box to save the file. We use <a href="http://msdn.microsoft.com/en-us/library/system.net.http.headers.contentdispositionheadervalue(v=vs.110).aspx">ContentDispositionHeaderValue</a> class for that. ContentDisposition is null so we need to initialize and configure it.

Sample Code:

<script src="https://gist.github.com/2322994.js?file=getFile.cs"></script>

This gives the response header:
<ol>
	<li>Status Code: 200</li>
	<li>Date: Fri, 06 Apr 2012 21:28:30 GMT</li>
	<li>X-AspNet-Version: 4.0.30319</li>
	<li>Transfer-Encoding: chunked</li>
	<li>Content-Disposition: attachment; filename=mytext.txt</li>
	<li>Connection: Close</li>
	<li>Pragma: no-cache</li>
	<li>Server: ASP.NET Development Server/11.0.0.0</li>
	<li>Content-Type: application/octet-stream</li>
	<li>Cache-Control: no-cache</li>
	<li>Expires: -1</li>
</ol>
And Body: Hello

When the browser gets this response, you will be prompted to download the file: mytext.txt.
