---
layout: post
title: Getting started with rake on .NET and visual studio
date: 2012-08-26 18:16

comments: true
categories: [.NET, ALM]
---
<a href="http://rake.rubyforge.org/">Rake</a> is normally used as a build tool scripting language (if used for anything else, please do comment). I've been using it for almost 2 years in place of NAnt and MSBuild due to its programmability. Executing a concise script to do anything I want for my build process is a great bonus rather than writing complex XML configurations. At work, I hook this script into the team city build server (which supports running rake tasks). My tasks ranges from versioning, unit-tests, deploy to staging and manual click to deploy to production, without having server specific features. Anything the build server can do, I can do it from my machine.

To get started with rake in .NET, here are the steps that easily gets you up and running:
<ol>
	<li>Download <a href="http://www.ruby-lang.org/en/downloads/">ruby</a> and install it. Ruby comes bundled with rake. Ensure they are installed in your Environment PATH. If somehow rake isn't bundled with ruby, its a gem anyway so install it with:
<pre>gem install rake</pre>
</li>
	<li>Install <a href="http://albacorebuild.net/">Albacore</a> gem, a set of rake task made specifically for making building .NET projects easier. Tasks include running NUnit and MSTests, zip, nuget deployments, etc. Getting started found <a href="https://github.com/Albacore/albacore/wiki/Getting-Started">here</a>. To install the gem:
<pre>gem install albacore</pre>
</li>
	<li>Download the visual studio plugin <a href="http://visualstudiogallery.msdn.microsoft.com/551978a5-68e3-452b-9067-22ef237ce929">Rake Runner</a> for visual studio 2012+. This allows you to run available rake tasks from the solution explorer.</li>
	<li>Create a visual studio project, place your <em>Rakefile </em>in the root folder of your project. This file should show up under <em>Solution Items </em>folder. This is where you write your scripts. Paste this code:
<pre>require 'albacore'</pre>
<pre>desc "Build"
msbuild :build do |msb|
msb.properties :configuration =&gt; :Release
msb.targets :Clean, :Build
msb.solution = "YourSolution.sln"
end</pre>
Replace the name of the solution with yours. Right click your solution, go to rake(Rake Runner extension) and select <em>Build </em>(refresh if not shown). The project should be built and the output shown in the <em>output </em>window pane.</li>
	<li>Go read this small <a href="http://jasonseifer.com/2010/04/06/rake-tutorial">tutorial</a> which should help you get off the ground to writing some complex build scripts.</li>
</ol>
<div>Build automation is a basic software engineering practise, rake is a great introduction for beginners to learn and very powerful and flexible for seasoned engineers. The support for rake in the .NET world is growing as more engineers see the power of rake.</div>
