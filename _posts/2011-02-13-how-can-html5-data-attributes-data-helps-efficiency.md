---
layout: post
title: How can HTML5 data attributes (data-*) help efficiency?
date: 2011-02-13 19:34
author: shawnmclean
comments: true
categories: [HTML &amp; CSS, html5, Javascript, jQuery]
---
As web-apps become more like desktop apps, we demand faster and more efficient client-side optimizations. These optimizations rely on us developers. As the features are released and supported, we have to be able to mix and match the techniques we use so our apps can reach their full potential. One such feature is the HTML5-data-attributes.

When we write web applications, we normally want to associate data with specific tags/elements so we can have a nice interactive client-side application running in html and javascript. HTML5 data-attributes comes in handy here.

[codesyntax lang="html4strict" lines="normal"]

&lt;span data-appname="SomeAppName"&gt; This element has data attached to it.&lt;/span&gt;

[/codesyntax]

This makes it possible to move javascript completely out of all our html pages. This makes it easy to run a minifier or compiler on all javascript code in our web app. An example would be some Ajax call is needed on an element. If all our javascript resides outside, then we need to have it access 2 types of data. The ID of the triggering element and the url. Take for example, an autosuggest textbox:

[codesyntax lang="html4strict" lines="normal"]

&lt;input id="AutoSuggest" data-url="MyController/AutoCompleteAction"/&gt;

[/codesyntax]

Using jQuery to access it:

[codesyntax lang="javascript" lines="normal"]
<pre>$(function () {</pre>
<pre>      $('#AutoSuggest').keyup(function() {</pre>
<pre>           $.get($(this).data('url'), function (data) {
<pre>
<pre>            //Whatever you want to do with the data here.
           });</pre>
<pre>      });</pre>
<pre>});</pre>
<pre>//A non jQuery way to get data attributes</pre>
<pre>var url = document.getElementById('AutoSuggest').dataset.url;</pre>
//IE6 way of using it (this is a hack, not actually using html5)</pre>
<pre>var url = <span style="font-family: monospace;">document.getElementById("AutoSuggest").getAttribute("data-url");</span>
<pre><span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; font-size: 13px; line-height: 19px; white-space: normal;">[/codesyntax]</span></pre>
</pre>
</pre>
So you can see that we can have less code instead of writing custom ajax calls by just refactoring our jQuery code. No more injecting a URL or other data into our javascript. This can be clearly seen for developers who are familiar with the MVC pattern in which we can use the framework's url helpers to set the url directly on the element itself. Other uses of this is in gaming, games built in html and javascript.
