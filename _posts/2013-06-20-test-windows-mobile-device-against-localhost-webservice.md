---
layout: post
title: Test Windows Mobile Device Against Localhost Webservice
date: 2013-06-20 05:26
comments: true
categories: [Mobile Development, Windows CE]
---
<strong>Technical Level: </strong>Intermediate

<strong>Problem:</strong> To test the emulation of Windows Mobile against a web service hosted in localhost has been a problem for many developers. I have yet to find an official method of using the device emulator in visual studio to connect to a localhost web service.

The problem occurs because if you add a web service to the mobile device project in visual studio, it sets the reference URL to localhost. This will tell the application to connect to the localhost of the mobile device, whether real or emulated. Using the computer name may not work for many people because visual studio's test environment will not allow calls from outside to call to the development's localhost.

<strong>Required Tools: </strong>These are tools that I have worked with to get this to work.
<ul>
	<li>Windows Vista</li>
	<li>Visual Studio 2008</li>
	<li>Windows Mobile SDK</li>
	<li>Windows Mobile Center</li>
	<li>Fiddler 2.</li>
</ul>
<strong>Solution:</strong> Assuming you have all of the above tools installed. We will add a proxy to the mobile device connection settings. We will get around to setting up fiddler to work with our mobile device.

<em>Visual Studio</em>

A specific port will need to be set on the Visual Studio Development Server to work correctly with fiddler. In the solution explorer pane, right click the web project that will be hosting your web service and click properties.

[caption id="" align="alignnone" width="308" caption="Visual Studio Web project"]<img class="size-full wp-image-16" title="Visual Studio Web project" src="http://blog.shawndev.com/wp-content/uploads/2009/06/vsweb.png" alt="Visual Studio Web project" width="308" height="149" />[/caption]

In the Web Tab, under Servers, select the Specific port radio button, and select a port. I am using port 81 for my development purposes.

<img class="size-full wp-image-17 alignnone" title="Visual Studio Web Property" src="{{ site.baseurl}}/images/2009/06/vsprop.png" alt="Visual Studio Web Property" />

In the solution explorer where you have your mobile device project, right click the web service you added, go to properties and in the Web Reference URL block, place a dot before the colon of the port number. This dot allows fiddler to intercept the messages sent.

<img class="size-full wp-image-21 alignnone" title="Mobile Web Service Properties" src="{{ site.baseurl}}/images/2009/06/mobwsprop.png" alt="Mobile Web Service Properties" />

Secondly, we are going to change the URL in proxy located in the Reference.cs generated by visual studio. Back in the solution explorer, right click the web service, click view in object browser. I have my client side projects in another solution that I am working on so the image may differ from previous solution image.

<img class="size-full wp-image-22 alignnone" title="Web Service Solution Object" src="{{ site.baseurl}}/images/2009/06/wssol.png" alt="Web Service Solution Object" />

Next we have the object browser showing up. Find a generated object, right click it and select Go To Defintion. This takes you to the Reference.cs generated for that specific web service.

[caption id="attachment_23" align="alignnone" width="293" caption="Visual Studio Mobile Project Object Browser"]<img class="size-full wp-image-23 " title="Visual Studio Mobile Project Object Browser" src="{{ site.baseurl}}/images/2009/06/mobobjbrowser.png" alt="Visual Studio Mobile Project Object Browser" width="293" height="369" />[/caption]

At the very top of the Reference.cs file where the Service class is created, there is the URL of the web service assigned to a Url property. Place the dot after the localhost, before the colon for the port and save the file. DO NOT EDIT ANY OTHER PART OF THIS FILE. If you update the service reference, you will need to come back here and change the URL manually again.

[codesyntax lang="csharp"]

namespace RAINMobile.RMEWService {

public partial class RMEService : System.Web.Services.Protocols.SoapHttpClientProtocol {

/// &lt;remarks/&gt;
public RMEService() {
this.Url = "http://localhost.:81/RME/RMEService.svc";
}
[/codesyntax]

<em>Mobile Device Emulator</em>

If you have not cradled or connect the device to the desktop, run the tool Device Emulator Manager which is found in visual studio, go to the device that you want to test on, right click it and select cradle. We now need to change the proxy of the emulated device that will have a connection to our desktop operating system via the  tool. Change the proxy to 169.254.2.2:8888 in the device.

[caption id="attachment_24" align="alignnone" width="244" caption="Mobile Device Internet Proxy"]<img class="size-full wp-image-24 " title="Mobile Device Internet Proxy" src="{{ site.baseurl}}/images/2009/06/mobproxy.png" alt="Mobile Device Internet Proxy" width="244" height="322" />[/caption]

<em>Fiddler</em>

We are going to add a custom script to re-route calls to the fiddler proxy to the machine's localhost. In the toolbar, go to Rules then Custom Rules. The script will open, search for this function: "static function OnBeforeRequest(oSession: Session)". In that function, add this piece of code:

[codesyntax lang="javascript"]
if (oSession.host=="169.254.2.2:81")
{
oSession.host="127.0.0.1:81";
}
[/codesyntax]
<h3><strong>UPDATE:</strong></h3>
Apparently in Fiddler 2.2, something has changed. Now I have to programmatically remove the dot. Some random IP can be used in the windows mobile proxy settings, but the port has to be whatever port fiddler is listening on (default: 8888). This is the new source is:

[codesyntax lang="javascript"]
if (oSession.host=="localhost.:81") //Intercepts the host
{
oSession.host="127.0.0.1:81"; //remove the dot and redirect to the IP for localhost
}
[/codesyntax]

The 81 signifies the specific port set for the visual studio development server. If you change the port in visual studio, you will need to change it in fiddler script also. Ports have to be consistent in the mobile application and the web service.

<strong>Conclusion</strong>

Until an official solution to this problem is released by microsoft or the mobile device development team, this is one of the methods used in connecting the emulated windows mobile application to a webservice hosted in localhost.
