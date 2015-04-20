---
layout: post
title: Getting WCF Meta-data from Windows Azure
date: 2009-06-24 00:49

comments: true
categories: [azure, client, meta-data, proxy, wcf, web role, Windows Azure, Windows Communication Foundation]
---
<strong>Technical Level: </strong>Beginner

<strong>Problem: </strong>With using any of the windows azure tools and SDKs before the May 2009 CTP, developers may have experienced problems downloading the meta-data and generating proxies for their WCF services hosted on the local server. I have not tested generating a WCF client from the live server so this solution targets the development platform. Now, I have completely no idea why this does not work so I will not try to explain anything.

<strong>Required Tools: </strong>These are the tools used in this project:
<ul>
	<li>Visual Studio 2008.</li>
	<li>Windows Azure May 2009 CTP.</li>
</ul>
<strong>Solution:</strong> A simple solution, remove your web application / web role project that hosts the WCF services from the windows azure project. Set the web application as the startup project in the solution explorer, run the web application by itself and download your meta-data.

<strong>Precautions:</strong>
<ul>
	<li>Make sure your local IIS is set to serve up .svc files.</li>
	<li>Make sure your WCF endpoints and client are compatible. eg. Silverlight will not work well with normal http binding.</li>
</ul>
<strong>Conclusion: </strong>Apparently, the development fabric does not work well for serving up wsdl and disco information.

<strong> </strong>
