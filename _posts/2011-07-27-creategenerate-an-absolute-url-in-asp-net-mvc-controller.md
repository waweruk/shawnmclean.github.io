---
layout: post
title: Create/generate an absolute URL in ASP.NET MVC Controller
date: 2011-07-27 01:53

comments: true
categories: [.net, .Net Framework, as, asp.net-mvc, asp.net-mvc-3]
---
Apparently, I forget stuff. So this code creates an absolute url that is an action/controller in my mvc application. Just add Request.Url.Scheme to it, which forces it to generate an absolute URL.

[codesyntax lang="php"]
<pre>string absUrl = Url.Action("Index", "Products", null, Request.Url.Scheme);</pre>
[/codesyntax]
