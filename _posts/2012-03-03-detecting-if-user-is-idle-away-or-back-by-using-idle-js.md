---
layout: post
title: Detecting if user is idle, away or back by using Idle.js
date: 2012-03-03 21:24
author: shawnmclean
comments: true
categories: [Javascript, javascript]
---
<a href="https://github.com/shawnmclean/Idle.js">Idle.js </a>is a javascript API that can detect if a user is idle, away or back in a webpage. This library is independent of other libraries such as jQuery. It supports callbacks for when the user is idle and back and methods for setting the timeout. The main purpose of creating this library is to use it in a chat application to relay the user status back to the server.
<h2>API</h2>
There are 2 callbacks supported, <strong>onAway</strong> and <strong>onAwayBack</strong>, both of which takes no parameters. <strong>onAway</strong> is trigger whenever the there is no activity inside the webpage by the user for the length of the timeout specified. <strong>onAwayBack</strong> is triggered whenever there is an activity inside the webpage by the user.

One method exposed to set the timeout <strong>setAwayTimeout(int)</strong>, which takes an integer of when to trigger the <strong>onAway</strong> event.

The <strong>constructor</strong> also accepts a configuration object with all of these methods.

Here is an example of usage:
<script type="text/javascript" src="https://gist.github.com/1968087.js?file=activity.js"></script>
<h2>Demo</h2>
There is a demo of this on on github <a href="http://shawnmclean.github.com/Idle.js/">here</a>.

The project is hosted <a href="https://github.com/shawnmclean/Idle.js">here</a>.
