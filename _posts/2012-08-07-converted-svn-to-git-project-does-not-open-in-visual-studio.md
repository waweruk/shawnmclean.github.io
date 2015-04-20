---
layout: post
title: Converted SVN to Git project does not open in visual studio
date: 2012-08-07 03:33

comments: true
categories: [Visual Studio]
---
I had a SVN hosted visual studio project that I just converted to Git. I am using the <a href="http://ankhsvn.open.collab.net/">AnkhSVN</a> plugin for SVN and <a href="http://visualstudiogallery.msdn.microsoft.com/63a7e40d-4d71-4fbb-a23b-d262124b8f4c/">Git Source Control provider</a> plugin for git. After the conversion, I tried managing the project with `Git Source Control Provider` but visual studio threw this error: <em>The active solution or project is currently controlled by a different source control plugin that the one you have selected. If you change the source control plug-in, the active solution will be closed.</em>

To fix this, apparently AnkhSVN added a pre-selection command to the solution file that tells visual studio to use AnkhSVN as the source control plugin. To fix this, open your solution file and look for the following and remove it:
<pre> 
GlobalSection(SubversionScc) = preSolution
    Manager = AnkhSVN - Subversion Support for Visual Studio
    Svn-Managed = True
EndGlobalSection</pre>
&nbsp;
