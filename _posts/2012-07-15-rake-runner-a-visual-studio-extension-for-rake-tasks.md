---
layout: post
title: Rake Runner a visual studio extension for rake tasks
date: 2012-07-15 18:12
author: shawnmclean
comments: true
categories: [.Net Framework, extensions, Visual Studio, visual studio]
---
<h2>Overview</h2>
<a href="http://visualstudiogallery.msdn.microsoft.com/551978a5-68e3-452b-9067-22ef237ce929">Rake Runner</a>, a visual studio 2012 extension that lists rake tasks in the right click menu context of the solution explorer. These tasks can be executed and the output is shown in the build output. This tool calls `<em>rake -P</em>` for listing tasks in the selected directory and `<em>rake {taskname}</em>` for executing tasks which requires rake to be in the PATH Environment or manually set in the options. This project is open source and hosted on <a href="https://github.com/shawnmclean/Rake-Runner">github</a>.
<h2>Details</h2>
Rake tasks context menu shows up for<em> Solutions, Projects, Solution Folders </em>and<em> Folders</em>. On first load of a context menu, tasks will be retrieved synchronously and hangs the IDE until retrieved. (Will be fixed in ver. 0.0.0.4). These tasks are cached per directory until `Refresh` is clicked.

Rake tasks are executed via an asynchronous process and all output are redirected to the build console output including errors.

Context Menu:

<a href="http://www.shawnmclean.com/wp-content/uploads/2012/07/rake_context.png"><img class="alignnone wp-image-461 size-full" title="rake_context" src="http://shawnmclean.com/wp-content/uploads/2012/07/rake_context.png" alt="" width="563" height="105" /></a>

Output pane:

<a href="http://www.shawnmclean.com/wp-content/uploads/2012/07/build_output.png"><img class="alignnone wp-image-460 size-full" title="build_output" src="http://shawnmclean.com/wp-content/uploads/2012/07/build_output.png" alt="" width="939" height="283" /></a>

Options Screen:

<a href="http://www.shawnmclean.com/wp-content/uploads/2012/07/options.png"><img class="alignnone wp-image-465 size-full" title="options" src="http://shawnmclean.com/wp-content/uploads/2012/07/options.png" alt="" width="640" height="372" /></a>
<h2>Requirements</h2>
<ol>
	<li>Rake and Ruby.</li>
	<li>Visual Studio 2012 RC.</li>
</ol>
<h2>Change log</h2>
<ul>
	<li><strong><strong><strong>0.0.0.3 - July 21, 2012</strong></strong></strong>
<ul>
	<li>Ensures rake is installed by using path set in options.</li>
	<li>Falls back to using the path from the system's Environment PATH.</li>
</ul>
</li>
	<li><strong>0.0.0.2 - July 15, 2012</strong>
<ul>
	<li>Tasks are listed synchronously.</li>
	<li>Tasks are executed asynchronously.</li>
</ul>
</li>
</ul>
