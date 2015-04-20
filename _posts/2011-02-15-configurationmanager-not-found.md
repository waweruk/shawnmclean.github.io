---
layout: post
title: ConfigurationManager not found.
date: 2011-02-15 15:20

comments: true
categories: [.net, .Net Framework]
---
I know this is an old problem but every few months I get caught by it. The problem is that <em>ConfigurationManager </em>that is used to get settings from the app.config/web.config file is not available.

The solution is simple. Add reference to the dll: <em>System.Configuration</em>.

[codesyntax lang="csharp"]

using System.Configuration;
<pre>string settings = ConfigurationManager.AppSettings["SomeSettings"];</pre>
[/codesyntax]
