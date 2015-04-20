---
layout: post
title: Access command prompt | terminal in visual studio
date: 2012-06-25 01:28

comments: true
categories: [Visual Studio, visual studio]
---
Sometimes I need to execute a command line script on my visual studio solutions. Here is how to set up an External Tool to start command prompt that localizes the directory to your project:

Tools &gt; External Tools &gt; Add

Command Field: cmd.exe

Initial Directory: $(SolutionDir)

Done.

Screens:

<a href="{{ site.baseurl}}/images/2012/06/menuTerminal.png"><img class="size-full wp-image-436 alignnone" title="menuTerminal" src="{{ site.baseurl}}/images/2012/06/menuTerminal.png" alt="" width="357" height="440" /></a>

External Tools:

<a href="{{ site.baseurl}}/images/2012/06/terminal.png"><img class="size-full wp-image-438 alignnone" title="terminal" src="{{ site.baseurl}}/images/2012/06/terminal.png" alt="" width="456" height="428" /></a>

The Console/Terminal opened from the tools menu while having a project opened.

<a href="{{ site.baseurl}}/images/2012/06/terminalconsole.png"><img class="size-full wp-image-437 alignnone" title="terminalconsole" src="{{ site.baseurl}}/images/2012/06/terminalconsole.png" alt="" width="542" height="109" /></a>
