---
layout: post
title: Add folders to fsharp (F#) project in visual studio 2010/2008
date: 2011-03-14 04:15

comments: true
categories: [.NET, f#]
---
I recently created a new f# project in my solution and ran into a problem I faced a few months ago. I had no option on creating folders in a F# project. Well, there are 2 solutions, manual coding the config for folders or use a tool.

Here is the easier way:
<ul>
	<li>Download the Fsharp project extender <a href="http://fsprojectextender.codeplex.com/">here</a>. Install it.</li>
	<li>Right click your F# project file and click "enable f# project extender".</li>
	<li><a href="http://www.shawnmclean.com/blog/wp-content/uploads/2011/03/enableProjExt.png"><img class="aligncenter size-full wp-image-210" title="Enable F# Project Extender" src="http://www.shawnmclean.com/blog/wp-content/uploads/2011/03/enableProjExt.png" alt="" width="370" height="163" /></a></li>
	<li>Now you can right click and add a folder as you normally would in other projects.</li>
</ul>
<strong>Note: </strong>Folder order still has effect here. Folders with files in it will be compiled before other files after it.
