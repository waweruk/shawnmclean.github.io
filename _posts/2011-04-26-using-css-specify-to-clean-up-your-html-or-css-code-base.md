---
layout: post
title: Using CSS specify to clean up your HTML or CSS code-base
date: 2011-04-26 04:07

comments: true
categories: [CSS, HTML &amp; CSS]
---
I would normally structure my site as follows:

<em>html &gt; body &gt; div#header //Or just header if using html5</em>

<em>html &gt; body &gt; div#main //Or section#main if using html5</em>

<em>html &gt; body &gt; div#footer //Or footer if using html5</em>

Using an ID for the main container will give alot of benefits. But if you structure your css the way I do (compact), then you're going to end up with problems with CSS specify. <em>Specify </em>is considered advanced css on most website tutorials (I have no idea why, since almost everyone will run into this problem as their css code-base grows).

Basically, what <em>specify</em> is, it gives a rank on the css rules when there is a conflict. A weight is attached to how you structure the selectors. Have a read <a title="CSS specify" href="http://htmldog.com/guides/cssadvanced/specificity/">here</a>.

Using the structure above, this is how I setup my CSS:

[codesyntax lang="css"]
<pre>#main a
{
	color: #000;
}

/*now lets say I want to single out some other headers in some other div (located inside #main)*/
.someReallyCoolDivs a
{
	color: #fff;
}</pre>
<pre>/*OR even if you have this, it doesn't work*/</pre>
<pre>
<pre>a.theReallyCoolAnchor
{
	color: #fff;
}</pre>
</pre>
[/codesyntax]

Because <em>#main a</em> has a higher weight than the other 2, the anchor will always be of <em>color: #000;. </em>I kept running into this problem constantly for the past few years. So to go around this, I always ended up using <em>!important. </em>I hated using <em>!important </em>so I just found out about <em>specify.</em>

I found 2 solutions to getting rid of !important and also reducing css size by a small amount.
<ol>
	<li>Use the ID of the main, or any other ID lower in your html hierarchy. eg.[codesyntax lang="css"]
<pre>#main .someReallyCoolDivs a
{
	color: #fff;
}</pre>
[/codesyntax]</li>
	<li>Attach a class to #main.[codesyntax lang="css"]
<pre>.main .someReallyCoolDivs a
{
	color: #fff;
}</pre>
[/codesyntax]</li>
</ol>
I have no idea to conclude this post, so...kthxbai?
