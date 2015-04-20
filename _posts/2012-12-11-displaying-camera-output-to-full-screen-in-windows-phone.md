---
layout: post
title: Displaying camera output to full screen in windows phone
date: 2012-12-11 15:31
author: shawnmclean
comments: true
categories: [Uncategorized]
---
Sometimes we might need to display the camera output as full screen on our windows phone but due to the difference between aspect ratio of the camera output and the phone screen, this poses as a slight challenge.

The first problem is that the mapped camera output is rotated vertically. The second problem is the aspect ratio.
<h2>Code</h2>
The following code shows how to solve this:
Xaml
<script type="text/javascript" src="https://gist.github.com/4251768.js?file=CameraView.xaml"></script>CSharp<script type="text/javascript" src="https://gist.github.com/4259012.js?file=CameraView.cs"></script>

The code-behind is a watered down version of setting up a camera. (There are checks to see if camera is supported, etc.)
<h2> Explanation</h2>
<ol>
	<li> Ensure the Canvas you are mapping the camera's video stream to is Verticalled Stretched.</li>
	<li>Set the CompositeTransform's CenterX and CenterY properties to .5 so when we set the rotation in code, it is done from the center.</li>
	<li>In the codebehind, the first thing we do when the camera is initialized is to move processing to the UI thread, hence the <em>this.Dispatcher.BeginInvoke </em>delegate.  <em>this </em>keyword ensures that the code is executed on the thread the <strong>CameraView </strong>PhoneApplication Page is running under. Without this, if we attempt to modify anything on the UI, we will get a cross thread exception.</li>
	<li>We then set the rotation of the canvas to the orientation of the camera.</li>
	<li>The final operation is to set the aspect ratio by using the first available resolution supported by the camera to calculate the width of the canvas.</li>
</ol>
