---
layout: post
title: Access User identity object on the silverlight wcf ria service
date: 2011-06-29 02:26
comments: true
categories: [Silverlight]
---
Simple:

[codesyntax lang="csharp"]
<pre>ServiceContext.User.Identity.Name;</pre>
[/codesyntax]

Make sure your class inherits from some form of a Domain Service. For eg. <em>LinqToSqlDomainService</em>
