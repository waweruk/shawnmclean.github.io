---
layout: post
title: Convert Image to Canvas and back HTML5
date: 2011-08-23 00:59

comments: true
categories: [HTML, Javascript, jQuery]
---
A person emailed me asking for help on this. So here is a simple example on writing an image to a canvas, drawing a black rectangle beside it and then converting it back to an image using the <em>toDataURL() </em>method. I place the conversion code in the image.onload method instead of using events for the succinctness of the code.

<script src="https://gist.github.com/1764159.js?file=img2canvas.htm"></script>

Should look like:

<a href="{{ site.baseurl}}/images/2011/08/canvtoimg.png"><img class="aligncenter size-full wp-image-263" title="canvtoimg" src="{{ site.baseurl}}/images/2011/08/canvtoimg.png" alt="" width="584" height="245" /></a>

On the left is canvas, on the right is image.

I also rewrote it in <a href="http://jsfiddle.net/mcUHb/">jsfiddle </a>using google logo.

You can also find other examples on the MDN, for example <a href="https://developer.mozilla.org/en/Code_snippets/Canvas">here </a>is one about saving a canvas image to file.
