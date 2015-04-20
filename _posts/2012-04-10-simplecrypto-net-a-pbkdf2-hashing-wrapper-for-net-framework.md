---
layout: post
title: SimpleCrypto.Net a PBKDF2 Hashing wrapper for .Net Framework
date: 2012-04-10 06:46
author: shawnmclean
comments: true
categories: [.net, .Net Framework, c#, hashing, security]
---
Recently, Jeff Atwood blogged about a <a href="http://www.codinghorror.com/blog/2012/04/speed-hashing.html">hashing speed problem</a>, so I decided to rewrite my library since many people will be searching for easier ways of implementing PBKDF2 hashing. So I moved and re-factored my PBKDF2 algorithm stated in this <a href="http://www.shawnmclean.com/blog/2011/10/storing-passwords-using-pbkdf2-algorithm/">post</a> into this project which is much more object oriented friendly and focused only as a cryptography library. <a href="https://github.com/shawnmclean/SimpleCrypto.net">SimpleCrypto.Net</a>, a simple wrapper for .NET that abstracts away complex cryptography algorithms from the user (easy password hashing for noobs). 

There are 3 basic ways of using this wrapper:
<h3>1. Compute Hash for a string by generating a salt</h3>
Default Hash Iteration: 5000

Default Salt Size: 16
<script src="https://gist.github.com/2348795.js?file=basic.cs"></script>
&nbsp;
<h3>2. Compute Hash for a string by generating a salt with parameters</h3>
<script src="https://gist.github.com/2348809.js?file=basicwithsaltsettings.cs"></script>
&nbsp;
<h3>3. Compute Hash for a string with known salt</h3>

<script src="https://gist.github.com/2348820.js?file=basicwithsalt.cs"></script>
&nbsp;
<h3>Salt Generation</h3>
The salt is stored in the format of: "{#hash_iterations}.{generated_salt}". Salt size is used as the number of bits the generated_salt should be. An example would be: "50.dakh3oihh123knadn". If the salt is not in this format, a <strong>FormatException</strong> will be thrown. Remember to always generate an unique salt for each user and regenerate it whenever they change password.


The library is also hosted on NuGet: <em> PM&gt; Install-Package SimpleCrypto</em>

I may be adding more hashing algorithms in the future, or if you would like to add some, fork the project and do a pull request. Please include unit test.
