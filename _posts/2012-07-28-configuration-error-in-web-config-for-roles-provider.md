---
layout: post
title: Configuration Error in web.config for roles provider
date: 2012-07-28 01:46

comments: true
categories: [ASP.NET-MVC]
---
Today I ran into a unique problem where I got this error:
<blockquote>
<pre>Configuration Error
Description: An error occurred during the processing of a configuration file required to service this request. Please review the specific error details below and modify your configuration file appropriately.</pre>
<pre>Parser Error Message: Exception has been thrown by the target of an invocation.</pre>
<pre>Source Error:</pre>
<pre>Line 46: &lt;providers&gt;
Line 47: &lt;clear /&gt;
Line 48: &lt;add name="CustomRoleProvider" type="Grikly.WebUI.Providers.CustomRoleProvider" /&gt;
Line 49: &lt;/providers&gt;
Line 50: &lt;/roleManager&gt;</pre>
</blockquote>
There can be many solution for this ambiguous exception but in my specific case, I was missing a binding of a dependency on a service the roles provider uses. If you use dependency injection, <strong>ensure all your dependencies are bound</strong>.

I use dependency injection to inject an authentication service into my <em>CustomRoleProvider </em>by property injection<em>. </em>This authentication service also has dependencies on other services and repositories which uses constructor injection from <a href="http://www.ninject.org/">Ninject</a>. The problem is that I missed a binding for one of the dependencies of the authentication service. Now I am using <em>WebActivator</em>, which according to <a href="http://haacked.com/archive/2010/05/16/three-hidden-extensibility-gems-in-asp-net-4.aspx">Haacked</a>, allows you to run code way before Application_Start is called in which ninject bindings are normally executed. Because the RolesProvider is being initialized in the early stages of the pre-app start life-cycle through the web.config, the exception of having unbound dependencies will be thrown though the web.config as a parse error.
<h2>Conclusion</h2>
So this scenario requires that; your provider utilizes property injection via WebActivator[<em>PreApplicationStartMethod</em>] and the injected object also requires its own dependencies to be injected by constructor injection. When one or more of these constructor injected dependency is not bound, this error will be thrown.
