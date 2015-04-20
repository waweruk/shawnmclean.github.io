---
layout: post
title: Getting Windows 8 Developer Preview to work in Virtualbox
date: 2011-09-14 17:55

comments: true
categories: [virtualbox, Windows, windows 8]
---
This is how I installed Windows 8 Developer Preview with developer package (<span style="color: #ff0000;">64-bit</span>) on Virtual Box which is on my <span style="color: #ff0000;">64-bit</span> Windows 7 machine:
<ol>
	<li>Download some ISO mount tool.</li>
	<li>Download the windows 8 ISO. <a href="http://msdn.microsoft.com/en-us/windows/apps/br229516">http://msdn.microsoft.com/en-us/windows/apps/br229516</a></li>
	<li>Download <a href="http://www.virtualbox.org/wiki/Downloads">Virtualbox</a>.</li>
	<li>Setup a new machine in Virtualbox, I gave mine 1.5Gb RAM and 20GB space. Windows 8 requires 1Gb RAM and 13Gb storage, so make sure you're over that.</li>
	<li>Set your OS as Windows 7 64-bit.</li>
	<li>There are some settings that is required for this to work. In the settings, go to System &gt; Motherboard and enable IO APIC extended settings, then move to the Processor tab enable PAE/NX, then move to the Acceleration tab and enable the 2 hardware acceleration (Nested Paging and VT-x/AMD-V). (<span style="color: #ff0000;">Most important part, why I even wrote this post</span>).</li>
	<li>Start the machine, select the mounted drive and everything is common sense from there to install the OS.</li>
</ol>
<div>Here is the machine setup:</div>
<div><a href="http://www.shawnmclean.com/wp-content/uploads/2011/09/settings.jpg"><img title="settings" src="http://www.shawnmclean.com/wp-content/uploads/2011/09/settings.jpg" alt="" width="447" height="435" /></a></div>
