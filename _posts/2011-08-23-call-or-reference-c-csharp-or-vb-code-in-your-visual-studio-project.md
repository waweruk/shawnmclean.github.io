---
layout: post
title: Call or Reference c# (csharp) or vb code in your visual studio project
date: 2011-08-23 02:26

comments: true
categories: [.net, .Net Framework, visual studio]
---
I see many questions like this popup on stackoverflow, or even beginner .net programmers asking this question. Many of them worded differently, but asking the same thing. "How do I reference other projects in visual studio". You can find a worded version on <a href="http://msdn.microsoft.com/en-us/library/f3st0d45.aspx">MSDN </a>or you can view the pictorial version below:

<span class="Apple-style-span" style="font-size: 20px; font-weight: bold;">Referencing the project</span>

You can reference the project with all the source code, etc. You can make real-time changes here and the visual studio intellisense will reflect those changes, even before compiling the other project.

You can create a whole new solution with just a class library project or you can add a class library to an existing project(silverlight will contain their own type of class library as they use a subset of the .net framework):

<img class="aligncenter size-full wp-image-269" style="border-style: initial; border-color: initial;" title="classlib" src="http://www.shawnmclean.com/wp-content/uploads/2011/08/classlib.png" alt="" width="913" height="181" />

&nbsp;

You need to reference this project:

<a href="http://www.shawnmclean.com/wp-content/uploads/2011/08/rclickRef21.png"><img class="aligncenter size-full wp-image-274" title="rclickRef2" src="http://www.shawnmclean.com/wp-content/uploads/2011/08/rclickRef21.png" alt="" width="481" height="410" /></a>

&nbsp;

If its already in your solution, just view the <em>Projects</em> tab and you'll see a list of projects you can reference:

<a href="http://www.shawnmclean.com/wp-content/uploads/2011/08/rclickRef.png"><img class="aligncenter size-full wp-image-272" title="rclickRef" src="http://www.shawnmclean.com/wp-content/uploads/2011/08/rclickRef.png" alt="" width="796" height="162" /></a>

It should then be listed in your reference folder:

<a href="http://www.shawnmclean.com/wp-content/uploads/2011/08/inref1.png"><img class="aligncenter size-full wp-image-275" title="inref" src="http://www.shawnmclean.com/wp-content/uploads/2011/08/inref1.png" alt="" width="276" height="178" /></a><a href="http://www.shawnmclean.com/wp-content/uploads/2011/08/classlib.png">
</a>
<h2>Referencing an assembly</h2>
You can also reference the compiled dll or any other assembly (.exe) you wish. (Which is what referencing a project actually does, it just points the reference the other other project's build output directory). Just do as you would with referencing a project, but just browse for a the file. When referencing a built assembly, you loose debugging features (unless you want to debug the IL), but no C# or higher level language debugging is available.

<a href="http://www.shawnmclean.com/wp-content/uploads/2011/08/lazymethod.png"><img class="aligncenter size-full wp-image-271" title="lazymethod" src="http://www.shawnmclean.com/wp-content/uploads/2011/08/lazymethod.png" alt="" width="649" height="488" /></a><a href="http://www.shawnmclean.com/wp-content/uploads/2011/08/classlib.png">
</a>Now you can reference a class, just remember to add the namespace. The referenced project's code should be <strong>public</strong> so other assemblies can access it.
