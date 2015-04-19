---
layout: post
status: publish
published: true
comments: true
title: Blue-Green Deployments with Octopus Deploy and Azure Websites from TFS
categories: [ALM]
tags: [DevOps, Deployment, Visual Studio]
---

This article aim to explain how we can use Octopus Deploy to Automate Deployments to Azure Websites with a single deployment agent (tentacle) using the Blue-Green deployment pattern. This infrastructure is being used in my startup [Grikly](http://grik.ly) running purely on Azure using the BizSpark subscription, a simple and nice setup for a low cost software development company. The same concepts can be used in Enterprise grade systems.


**Blue-Green** deployments allow us to deploy web applications that require warm up time without any down times. This require running 2 instances of production grade environments such as a staging and production. We deploy to staging, have it warm up and swap it with production.


**Octopus Deploy** is a cool deployment tool that suites well in .NET projects.


**TFS** is an ALM suite which contains Source Control and Build Server. We're currently utilizing[Visual Studio Online](http://www.visualstudio.com/en-us/products/what-is-visual-studio-online-vs.aspx)


Our workflow is that we will check in code to TFSGit, when we push it to VSO, the CI build will trigger. This builds the solution, versions and package up the components and ships them off to Octopus Deploy Server. We can then create releases, deploying them to Test and promoting them to Production.


## Azure Configuration
 On the Azure Websites, create a new deployment slot called "staging". This feature can be found on the Dashboard of the website under quick glance;

![ ](http://shawnmclean.com/wp-content/uploads/2014/09/Azure-create-slot.png)

[
When you have created the deployment slot, your website listing should look like the following:

](http://shawnmclean.com/wp-content/uploads/2014/09/Azure-create-slot.png)
[![Azure Website Listing](http://shawnmclean.com/wp-content/uploads/2014/09/AzureList.png)</a>](http://shawnmclean.com/wp-content/uploads/2014/09/AzureList.png)

[
You may also have multiple deployment slots, as seen here, we have a slot for our testing environment.

](http://shawnmclean.com/wp-content/uploads/2014/09/AzureList.png)

### [Azure VM</h3>  
 We have installed Octopus Deploy on an Azure VM instance, and a tentacle on that same instance. Since we're deploying applications to Azure Websites and not other VMs, there is no need for multiple deployment agents. Here are a few tips to remember when configuring the VM in Azure:](http://shawnmclean.com/wp-content/uploads/2014/09/AzureList.png)

2.  [Ensure Endpoint ports are exposed so we can remote and browse to the Deployment Server:](http://shawnmclean.com/wp-content/uploads/2014/09/AzureList.png)[![Azure VM Endpoints](http://shawnmclean.com/wp-content/uploads/2014/09/Azure-VM-Endpoints.png)</a></li> ](http://shawnmclean.com/wp-content/uploads/2014/09/Azure-VM-Endpoints.png)
3.  [Enable firewall inbound port 80.</li>  
     </ol>
    ## </h2>

    ](http://shawnmclean.com/wp-content/uploads/2014/09/Azure-VM-Endpoints.png)
    ## [TFS Configuration</h2>  
     Now that we have the Octopus Deployment Server and Agent running on the Virtual Machine, lets ](http://shawnmclean.com/wp-content/uploads/2014/09/Azure-VM-Endpoints.png)[configure TFS</a> and the ](http://docs.octopusdeploy.com/display/OD/Team+Foundation+Server)[visual studio project</a>. Ensure to read those documentation and watch the videos provided. We do the following:](http://docs.octopusdeploy.com/display/OD/Using+OctoPack)
    [First install ](http://docs.octopusdeploy.com/display/OD/Using+OctoPack)[OctoPack</a> NuGet package on the visual studio projects that will be deployed.](https://www.nuget.org/packages/OctoPack/)

    [Then create or configure Continuous Integration build definition:](https://www.nuget.org/packages/OctoPack/)[![TFS Build Settings](http://shawnmclean.com/wp-content/uploads/2014/09/TFS-Build-Settings.png)</a>](http://shawnmclean.com/wp-content/uploads/2014/09/TFS-Build-Settings.png)

    [
    Remember to set Output location to **AsConfigured</strong>.**

    **
    There are a few commands we pass to the MSBuild arguments so we can package and deploy the OctoPack components.

    _/p:RunOctoPack=true /p:OctoPackPublishPackageToHttp=http://octoserv.cloudapp.net/nuget/packages /p:OctoPackPublishApiKey={mykey}</em>_

    _
    The Octopus Deploy Server comes with its own NuGet server, so we point our **OctoPackPublishPackageToHttp</strong> to that server, since we configured our Octo Server to run on port 80, it will look sometime similar to the above.**

    **
    Also in the screen shot above, we are running an UpdateVersion.ps1 script that versions our assembly before building.

    Now push or check in your source and watch the build pushes the packages to the Octopus Deploy NuGet Server, check this by browsing to the Octopus Deploy web app and go to _Library </em>where there should be something like the following:_

    **_**](http://shawnmclean.com/wp-content/uploads/2014/09/TFS-Build-Settings.png)_**_
    [![Octopus Deploy NuGet Packages](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Deploy-NuGet-Packages.png)</a>](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Deploy-NuGet-Packages.png)

    [
    This now means everything is working great from TFS and Build, we can now move on to configuring deployments.

    ](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Deploy-NuGet-Packages.png)
    ## [Octopus Deploy Server</h2>  
     Since we are only deploying Azure Websites, we only need 1 tentacle, our environment setup looks like the following screen shot:](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Deploy-NuGet-Packages.png)
    [![Octopus Environment](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Environment.png)</a>](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Environment.png)

    [
    We configured our local tentacle as a **deployer </b>role. The grikly-qa server you see there is for running automated UI Acceptance tests, which will be covered in another post.**

    **
    We have a Website application that will be using Web Deploy for deploying to Azure. First the Step Template is required. Browse to _Library -> Step Templates</em> and follow the instructions there to copy the Web Deploy Step template from the community template list._

    _
    We can then create an application process similar to this screen shot:

    _**](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Environment.png)**_
    [![Octopus Process](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Process.png)</a>We're doing a _rolling deployment</em> in the **deployer </strong>role. Which means that we are running all child steps in sequence as Octopus by default tries to execute steps in parallel. Set the environments you want the steps to be executed in, such as **Test </strong>and **Production</strong>. The step named _Swap Production with Staging</em> is only executed in **Production</strong> because that is the only environment the swap can take place.**_******_](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Process.png)

    [_****_**
    Configuring the _Grikly WebAPI</em> step is nicely documented on the Octopus Deploy help site._

    _
    The _Grikly WebAPI - Deploy</em> step can be tokenized so we have different values being injected or replaced per deployment to a specific environment by using the _Variables</em> feature:__

    _**_****_](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Process.png)___
    [![Octopus Deploy Variables](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Deploy-Variables-1024x231.png)</a>](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Deploy-Variables.png)

    [
    The Web Deploy Step can be configured as:

    ](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-Deploy-Variables.png)
    [![Octopus WebDeploy Config](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-WebDeploy-Config.png)</a>](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-WebDeploy-Config.png)

    [
    Note, we tokenized the Publish Url and Website name with the variable name. This will be replaced based on the criteria specified in the scope of the variable.

    Create and Execute a Release here manually to ensure this works and is deploying to the site successfully.

    ](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-WebDeploy-Config.png)
    ## [Blue-Green Azure Swapping</h2>  
     Now its time to implement swapping of staging and production.](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-WebDeploy-Config.png)
    [Install and configure ](http://shawnmclean.com/wp-content/uploads/2014/09/Octopus-WebDeploy-Config.png)[Azure Powershell</a> on the Octopus Deploy server or the one with the tentacle that will be used as the **deployer</strong>. My setup is using the Certificate method for authentication, so I downloaded and placed my certificate in a known location. The server may need to be restarted.**](http://azure.microsoft.com/en-us/documentation/articles/install-configure-powershell/)

    [**
    Execute this script on the VM where Azure Powershell was installed and manually verify that the websites were swapped

    https://gist.github.com/shawnmclean/40ec81407d3367c02a7e

    The core here is the method **Switch-AzureWebsiteSlot</strong> which looks for the website named _griklyapi </em>and a slot _staging</i> and swapping it with the _production</em> slot. _production</i> is the default slot name given to the main website. The use of -Slot1 and -Slot2 parameters are here because we have multiple slots (testing and staging).____**

    **____
    If this executes successfully, copy it into Step details for _Swap Production with Staging</em> process step._

    _
    Execute another release and deploy and watch the build gets deployed to staging and then automatically swapped to production. No downtime!

    _____****](http://azure.microsoft.com/en-us/documentation/articles/install-configure-powershell/)

    ____**

    _**_

******
