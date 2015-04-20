---
layout: post
title: Integration test ASP.NET WebAPI with OWIN and Authentication
date: 2014-08-31 19:29
author: shawnmclean
comments: true
categories: [ASP.NET WebApi, asp.net-webapi-2, Testing]
---
In this article, I'm going to share the steps I take in order to run integration tests on ASP.NET WebAPI using OWIN/Katana that uses authentication from ASP.NET Identities.

One of the great things about ASP.NET WebAPI 2.0 is that we can self-host the whole application without the need for IIS. This allows us to easily write integration tests, we can have our test runner spin up the web application in memory and throw in memory requests at it.
<h2>Prepare WebAPI for testing</h2>
First we need to prepare the WebAPI project for in-memory testing.
<h3>Help Page</h3>
If you are generating the Help Page from an XML Documentation, instead of uncommenting the <em>SetDocumentationProvider</em> code in HelpPageConfig.cs, change it to this:

https://gist.github.com/shawnmclean/8a269bcfd7a4265abb7b

This gist shows us setting the provider if path is found. I did not find this file anywhere when using the OWIN TestServer so we could not use <em>DeploymentItem, </em>hence we just not set the provider if not found.
<h3>Data Protector and UserTokenProvider</h3>
When using ASP.NET Identities, we have to set the <em>UserTokenProvider </em>on the <i>UserManager </i>so we can generate confirmation, password reset tokens and other features that require encrypting user information. When using the OWIN TestServer, it does not seem to automatically set the Data Protection Provider on the App Builder. We are going to bypass all of that and set the <em>UserTokenProvider  as EmailTokenProvider </em>on the user manager:

https://gist.github.com/shawnmclean/dd6b87736aec72e24932
<h2>Configuring Integration Test project</h2>
In your integration test project, reference the <a title="OWIN Testing Nuget package" href="https://www.nuget.org/packages/Microsoft.Owin.Testing/" target="_blank">OWIN.Testing</a> nuget package:
<blockquote>PM&gt; Install-Package Microsoft.Owin.Testing</blockquote>
First lets create a base class that will be inherited by all other test classes:

https://gist.github.com/shawnmclean/58217a22ade86acd3866

<strong>AssemblyInitialize  </strong>is used for configuring the test server for the lifetime of the test runner.

<strong>TestInitialize </strong>will be cleaning up and re-seeding the database before every test is executed. We use the same Entity Framework context used by our WebAPI for this operation.

We are referencing assemblies in the <strong><em>referenceLibs</em> </strong>method due to some test runners like resharper and the built in one for visual studio does not copy references that are not directly referenced in the test project.

We can then write a test like this:

https://gist.github.com/shawnmclean/9ce025a39f2412a83a71

Very minimal amount of code reqired to create integration tests when using OWIN/Katana. A similar setup can be done for MVC.

&nbsp;

&nbsp;
