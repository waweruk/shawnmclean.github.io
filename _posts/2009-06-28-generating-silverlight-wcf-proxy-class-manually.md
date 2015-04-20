---
layout: post
title: Generating Silverlight WCF Proxy Class Manually
date: 2009-06-28 21:03

comments: true
categories: [client, Silverlight, Silverlight, wcf, Webservice, Windows Communication Foundation]
---
<strong>Technical Level: </strong>Intermediate

<strong>Problem: </strong>For some developers, when using visual studio's built in service reference proxy generator tool gives an error when generating the client code. This is when I try to add a service reference in the solution explorer of visual studio. Usually it would say "Not support relative URI" when it reaches the section to generate the proxy class. This problem occurred in silverlight 2.0 beta, 2.0, 3.0 beta development projects for 2 different machines.

<strong>Required Tools: </strong>
<ul>
	<li>Silverlight Tools and SDK (Works with Silverlight 2.0 +)</li>
	<li>Visual Studio</li>
</ul>
<strong>Solution: </strong>My solution only works if visual studio partially creates the webservice in the solution explorer. Meaning if you browse to the physical location of the project and view the Webservice folder in the Service References folder created by visual studio, there is a file called "Reference.cs" there.

If visual studio does not create anything at all for you, you can create this file and paste in the generated code(method shown below). You can add this file anywhere in your project.
<ol>
	<li>First of all, start the WCF service. Mine is hosted at "http://localhost:81/CME/CMEService.svc".</li>
	<li>Go to this directory "C:Program FilesMicrosoft SDKsSilverlightv3.0Tools"  and make sure "SlSvcUtil.exe" exists. If that directory does not exist, just browse the silverlight directory for the SlSvcUtil.exe program. This tool generates the client/proxy code from the wsdl of your WCF Service.</li>
	<li>At the same directory as the tool, make sure that "System.Windows.dll" exists and also make a copy to the root of the C: drive. If you do not have this dll, search for it in the .NET folder or do a search of Program Files folder.</li>
	<li>Open command prompt ( Press Wnd + R then type 'cmd' ).</li>
	<li>For ease of use, change your directory to the folder of your tool, eg "cd C:Program FilesMicrosoft SDKsSilverlightv3.0Tools"</li>
	<li>Now to generate the service reference code. Type in command prompt: slsvcutil <strong>{Location of my wcf service}</strong> l /edb /namespace:"*,<strong>{Namespace in client project}</strong>" /ct:System.Collections.Generic.List`1 /r:"c:System.Windows.dll". For eg. -  <strong><em>slsvcutil http://localhost:81/CME/CMEService.svc?wsdl /edb /namespace:"*,RAINLight.CMEWService" /ct:System.Collections.Generic.List`1 /r:"c:System.Windows.dll"</em></strong>. You can change your default list type to ObservableCollection or any Generic type you want to be created as data is received from the service.</li>
	<li>A C# file with the generated code will be created in the folder of the tool with the name of the WCF Service. eg. "CMEService.cs". Copy the contents of this file into the "Reference.cs" file in your project or the file you manually created to store this proxy class.</li>
</ol>
<strong>Conclusion: </strong>I have no idea why visual studio's tool wont work and neither do I have any idea how to fix it. This problem shows up on 2 seperate machines with each having their own installation files of visual studio and silverlight tools coming straight from Microsoft's website.

<strong> </strong>
