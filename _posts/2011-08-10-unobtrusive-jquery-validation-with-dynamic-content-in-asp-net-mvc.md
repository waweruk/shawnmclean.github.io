---
layout: post
title: Unobtrusive jquery validation with dynamic content in ASP.NET MVC
date: 2011-08-10 04:16

comments: true
categories: [asp.net-mvc, asp.net-mvc-3, HTML &amp; CSS, html5, Javascript, jQuery]
---
I wanted to have a dynamic form that had hiding/showing input elements, and still contained validation that was extended from my data annotated models. I followed xhalent's blog post <a href="http://xhalent.wordpress.com/2011/01/24/applying-unobtrusive-validation-to-dynamic-content/">here</a>, but ended up wasting alot of time. Apparently his method supports changing validation rules at run-time.

My method was simple. I added the input (which is supposed to be hidden from the start) inside the form (using Asp.net mvc syntax). This allows the unobtrusive data attributes to be rendered in html. Now after the page has finished loaded, I ran a script that took out the contents and stored it in a javascript variable. When I needed to show this input field, I'll just add it back. No need to call any javascript code, as the validation will automatically be triggered anyways.

This code shows merging a login and registration form in 1. There is more to it than whats posted here, but the idea here is to get dynamic input validation to work.

<script src="https://gist.github.com/1136105.js?file=dynamicvalidation.html"></script>
<h2>Conclusion</h2>
No need to write much javascript. Just removing and adding it inside the form with the data-val* attributes is good enough for validation. At the time of this writing I am using jquery-1.6.2.
