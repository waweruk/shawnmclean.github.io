---
layout: post
title: Create/Generate an absolute url in asp.net webapi
date: 2012-04-04 02:55
author: shawnmclean
comments: true
categories: [.Net Framework, asp.net-webapi]
---
To get the absolute Url to an action or route in your WebAPI controller that points to another action, play around with the following code:

<script src="https://gist.github.com/2297415.js?file=absolute.cs"></script>

If the route is set to null, the first mapped route in the global.asax file will be used. You can play around with the parameters and anonymous object to get the correct routing you need.

For absolute url in mvc, see <a href="http://www.shawnmclean.com/blog/2011/07/creategenerate-an-absolute-url-in-asp-net-mvc-controller/">http://www.shawnmclean.com/blog/2011/07/creategenerate-an-absolute-url-in-asp-net-mvc-controller/</a>
