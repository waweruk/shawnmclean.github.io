---
layout: post
title: Retrieve Coordinates from Google Maps on Click
date: 2010-01-07 20:07

comments: true
categories: [Javascript, Maps, Uncategorized]
---
<strong>Technical Level: </strong>Beginner

<strong>Problem: </strong>Retrieve the coordinates from a click event on html/javascript google maps.

<strong>Required Tools: </strong>
<ul>
	<li>Google Map <a href="http://code.google.com/apis/maps/signup.html">API Key</a>.</li>
</ul>
<strong>Solution:</strong>Very simple solution, a Google map click event passes the coordinate as a parameter. I Use JQuery in my implementation of this map because I intend to make AJAX calls. If you are not familiar with JQuery, then the following codes can be substituted in the as follows:

[codesyntax lang="javascript" lines="normal"]
$("#SomeIDOfATextBox").val("Some value");
//Can be replaced by:
document.getElementById('SomeIDOfATextBox').value = "Some value";
[/codesyntax]

Below is the javascript used to setup the map and grab the coordinates:

[codesyntax lang="javascript" lines="normal"]
var map;
function initialize() {

if (GBrowserIsCompatible()) //a check to make sure the browser is compatible for GEmaps.
{
map = new GMap2(document.getElementById("map_canvas")); //assign the div to map var
map.setCenter(new GLatLng(18, -77.4), 13); //centering the location
map.setUIToDefault(); //setting the default location to the center.

//The GE event for clicking the map.
GEvent.addListener(map, 'click', function(overlay, point) {
map.clearOverlays(); //clearing the previous marker
var lati = point.lat(); //Function to extract latitude
var longi = point.lng(); //Function to extract longitude
var marker = new GMarker(new GLatLng(lati, longi), { draggable: false }); //Setting the marker to clicked location
map.addOverlay(marker); //adding the marker to the map
//JQuery function to assign the lat and long to the values of textboxes.
$('#LatTxt').val(lati);
$('#LonTxt').val(longi);
});
}

//This JQuery Function operates almost the same as the onload function, except more earlier
$(function() {
initialize();//call to initialize the map.
}
[/codesyntax]

[codesyntax lang="html4strict" lines="normal"]
<pre>&lt;div id="map_canvas" style="width: 600px; height: 400px;"&gt;
&lt;/div&gt;
&lt;label&gt;
Latitude
&lt;input id="LatTxt" disabled="disabled" type="text" /&gt;
&lt;/label&gt;
&lt;label&gt;
Longitude
&lt;input id="LonTxt" disabled="disabled" type="text" /&gt;
&lt;/label&gt;</pre>
[/codesyntax]

Below is a screenshot of my extended version (I'll show how to get Geo-coded locations in a next post):

<a href="http://www.shawndev.com/blog/wp-content/uploads/2010/01/gmappoint1.png"><img class="size-full wp-image-77 alignnone" title="Google Map Cordinate Retrieval" src="http://www.shawndev.com/blog/wp-content/uploads/2010/01/gmappoint1.png" alt="" /></a>

PS. Google Geo-Code service sucks for Jamaica. I'm now seeking alternative data sets.
