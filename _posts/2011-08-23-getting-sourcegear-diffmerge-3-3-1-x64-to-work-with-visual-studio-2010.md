---
layout: post
title: Getting SourceGear DiffMerge 3.3.1 x64 to work with visual studio 2010
date: 2011-08-23 03:35

comments: true
categories: [.NET, visual studio]
---
<h2>Problem</h2>
So SourceGear released a new version of <a href="http://www.sourcegear.com/diffmerge/">DiffMerge</a>, and moved around directories which caused my visual studio to throw an error when using AnkhSVN to resolve a conflict.

I'm using Windows 7 64 bit and Visual studio 2010 Ultimate with AnkhSVN as my primary source control.
<h2>Solution</h2>
In visual studio, go to Tools &gt; Options &gt; Source Control &gt; Subversion User Tools. For both External Diff Tool and External Merge Tool, select DiffMerge (it will say not found). Then click the dotted button to open up the settings. Change the directory from <em>"<span style="color: #ff6600;">$(ProgramFiles)SourceGearDiffMergeDiffMerge.exe" </span></em> to <span style="color: #ff6600;"><em>"$(ProgramW6432)SourceGearCommonDiffMergesgdm.exe". </em><span style="color: #000000;">So basically, you should have this: <span style="color: #ff6600;"> <em>"$(ProgramW6432)SourceGearCommonDiffMergesgdm.exe" "$(Base)" "$(Mine)" /t1="$(BaseName)" /t2="$(MineName)" $(ReadOnly?"/ro2")<span style="color: #000000;">. </span></em><span style="color: #000000;">Do the same for External Merge tool.</span></span></span></span>

DiffMerge is moved to the folder : <em>C:Program FilesSourceGearCommonDiffMergesgdm.exe </em>and also renamed to sgdm.exe. $(ProgramW6432) checks in both 64bit and 32 bit folders whilst $(ProgramFiles) checks only the 32bit folder, since visual studio is also 32 bit.
