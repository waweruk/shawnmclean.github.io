---
layout: post
title: Blue-Green Deployments with Octopus Deploy and Azure Websites from TFS
date: 2014-09-18 06:02
comments: true
categories: [ALM, DevOps]
tags: [visual studio, windows azure]
---
This article aim to explain how we can use Octopus Deploy to Automate Deployments to Azure Websites with a single deployment agent (tentacle) using the Blue-Green deployment pattern. This infrastructure is being used in my startup <a href="http://grik.ly">Grikly</a> running purely on Azure using the BizSpark subscription, a simple and nice setup for a low cost software development company. The same concepts can be used in Enterprise grade systems.

<strong>Blue-Green</strong> deployments allow us to deploy web applications that require warm up time without any down times. This require running 2 instances of production grade environments such as a staging and production. We deploy to staging, have it warm up and swap it with production.

<strong>Octopus Deploy</strong> is a cool deployment tool that suites well in .NET projects.

<strong>TFS</strong> is an ALM suite which contains Source Control and Build Server. We're currently utilizing <a href="http://www.visualstudio.com/en-us/products/what-is-visual-studio-online-vs.aspx">Visual Studio Online</a>.

Our workflow is that we will check in code to TFSGit, when we push it to VSO, the CI build will trigger. This builds the solution, versions and package up the components and ships them off to Octopus Deploy Server. We can then create releases, deploying them to Test and promoting them to Production.
<h2>Azure Configuration</h2>
On the Azure Websites, create a new deployment slot called "staging". This feature can be found on the Dashboard of the website under quick glance;

<a href="{{ site.baseurl}}/images/2014/09/Azure-create-slot.png"><img class="alignnone size-full wp-image-1041" src="{{ site.baseurl}}/images/2014/09/Azure-create-slot.png" alt="Azure-create-slot" width="248" height="331" /></a>

When you have created the deployment slot, your website listing should look like the following:

<a href="{{ site.baseurl}}/images/2014/09/AzureList.png"><img class="alignnone size-full wp-image-1051" src="{{ site.baseurl}}/images/2014/09/AzureList.png" alt="Azure Website Listing" width="915" height="144" /></a>

You may also have multiple deployment slots, as seen here, we have a slot for our testing environment.
<h3>Azure VM</h3>
We have installed Octopus Deploy on an Azure VM instance, and a tentacle on that same instance. Since we're deploying applications to Azure Websites and not other VMs, there is no need for multiple deployment agents. Here are a few tips to remember when configuring the VM in Azure:
<ol>
	<li>Ensure Endpoint ports are exposed so we can remote and browse to the Deployment Server:<a href="{{ site.baseurl}}/images/2014/09/Azure-VM-Endpoints.png"><img class="alignnone size-full wp-image-1061" src="{{ site.baseurl}}/images/2014/09/Azure-VM-Endpoints.png" alt="Azure VM Endpoints" width="928" height="187" /></a></li>
	<li>Enable firewall inbound port 80.</li>
</ol>
<h2></h2>
<h2>TFS Configuration</h2>
Now that we have the Octopus Deployment Server and Agent running on the Virtual Machine, lets <a href="http://docs.octopusdeploy.com/display/OD/Team+Foundation+Server">configure TFS</a> and the <a href="http://docs.octopusdeploy.com/display/OD/Using+OctoPack">visual studio project</a>. Ensure to read those documentation and watch the videos provided. We do the following:

First install <a href="https://www.nuget.org/packages/OctoPack/">OctoPack</a> NuGet package on the visual studio projects that will be deployed.

Then create or configure Continuous Integration build definition:<a href="{{ site.baseurl}}/images/2014/09/TFS-Build-Settings.png"><img class="alignnone size-full wp-image-1071" src="{{ site.baseurl}}/images/2014/09/TFS-Build-Settings.png" alt="TFS Build Settings" width="951" height="486" /></a>

Remember to set Output location to <strong>AsConfigured</strong>.

There are a few commands we pass to the MSBuild arguments so we can package and deploy the OctoPack components.

<em>/p:RunOctoPack=true /p:OctoPackPublishPackageToHttp=http://octoserv.cloudapp.net/nuget/packages  /p:OctoPackPublishApiKey={mykey}</em>

The Octopus Deploy Server comes with its own NuGet server, so we point our <strong>OctoPackPublishPackageToHttp</strong> to that server, since we configured our Octo Server to run on port 80, it will look sometime similar to the above.

Also in the screen shot above, we are running an UpdateVersion.ps1 script that versions our assembly before building.

Now push or check in your source and watch the build pushes the packages to the Octopus Deploy NuGet Server, check this by browsing to the Octopus Deploy web app and go to <em>Library </em>where there should be something like the following:

<a href="{{ site.baseurl}}/images/2014/09/Octopus-Deploy-NuGet-Packages.png"><img class="alignnone size-full wp-image-1081" src="{{ site.baseurl}}/images/2014/09/Octopus-Deploy-NuGet-Packages.png" alt="Octopus Deploy NuGet Packages" width="934" height="453" /></a>

This now means everything is working great from TFS and Build, we can now move on to configuring deployments.
<h2>Octopus Deploy Server</h2>
Since we are only deploying Azure Websites, we only need 1 tentacle, our environment setup looks like the following screen shot:

<a href="{{ site.baseurl}}/images/2014/09/Octopus-Environment.png"><img class="alignnone wp-image-1091 " src="{{ site.baseurl}}/images/2014/09/Octopus-Environment.png" alt="Octopus Environment" width="611" height="485" /></a>

&nbsp;

We configured our local tentacle as a <b>deployer </b>role. The grikly-qa server you see there is for running automated UI Acceptance tests, which will be covered in another post.

We have a Website application that will be using Web Deploy for deploying to Azure. First the Step Template is required. Browse to <em>Library -&gt; Step Templates</em> and follow the instructions there to copy the Web Deploy Step template from the community template list.

We can then create an application process similar to this screen shot:

<a href="{{ site.baseurl}}/images/2014/09/Octopus-Process.png"><img class="alignnone wp-image-1101 size-full" src="{{ site.baseurl}}/images/2014/09/Octopus-Process.png" alt="Octopus Process" width="836" height="527" /></a>We're doing a <em>rolling deployment</em> in the <strong>deployer </strong>role. Which means that we are running all child steps in sequence as Octopus by default tries to execute steps in parallel. Set the environments you want the steps to be executed in, such as <strong>Test </strong>and <strong>Production</strong>. The step named <em>Swap Production with Staging</em> is only executed in <strong>Production</strong> because that is the only environment the swap can take place.

Configuring the <em>Grikly WebAPI</em> step is nicely documented on the Octopus Deploy help site.

The <em>Grikly WebAPI - Deploy</em> step can be tokenized so we have different values being injected or replaced per deployment to a specific environment by using the <em>Variables</em> feature:

<a href="{{ site.baseurl}}/images/2014/09/Octopus-Deploy-Variables.png"><img class="alignnone wp-image-1111 size-large" src="{{ site.baseurl}}/images/2014/09/Octopus-Deploy-Variables-1024x231.png" alt="Octopus Deploy Variables" width="700" height="157" /></a>

The Web Deploy Step can be configured as:

<a href="{{ site.baseurl}}/images/2014/09/Octopus-WebDeploy-Config.png"><img class="alignnone wp-image-1121 size-full" src="{{ site.baseurl}}/images/2014/09/Octopus-WebDeploy-Config.png" alt="Octopus WebDeploy Config" width="783" height="501" /></a>

Note, we tokenized the Publish Url and Website name with the variable name. This will be replaced based on the criteria specified in the scope of the variable.

Create and Execute a Release here manually to ensure this works and is deploying to the site successfully.
<h2>Blue-Green Azure Swapping</h2>
Now its time to implement swapping of staging and production.

Install and configure <a href="http://azure.microsoft.com/en-us/documentation/articles/install-configure-powershell/">Azure Powershell</a> on the Octopus Deploy server or the one with the tentacle that will be used as the <strong>deployer</strong>. My setup is using the Certificate method for authentication, so I downloaded and placed my certificate in a known location. The server may need to be restarted.

Execute this script on the VM where Azure Powershell was installed and manually verify that the websites were swapped

https://gist.github.com/shawnmclean/40ec81407d3367c02a7e

The core here is the method <strong>Switch-AzureWebsiteSlot</strong> which looks for the website named <em>griklyapi </em>and a slot <i>staging</i> and swapping it with the <em>production</em> slot. <i>production</i> is the default slot name given to the main website. The use of -Slot1 and -Slot2 parameters are here because we have multiple slots (testing and staging).

If this executes successfully, copy it into Step details for <em>Swap Production with Staging</em> process step.

Execute another release and deploy and watch the build gets deployed to staging and then automatically swapped to production. No downtime!

&nbsp;
