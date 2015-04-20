---
layout: post
title: Using jQueryUI autocomplete with jTemplates
date: 2011-02-10 04:52

comments: true
categories: [AJAX, HTML &amp; CSS, html5, Javascript, jQuery]
---
<strong>Technical Level: </strong>Beginner

<strong>Problem: </strong>jQueryUI autocomplete does not allow us to naturally use templates with JSON data. We use jTemplates and a somewhat of a hack to set the templates.

<strong>Required Tools:</strong>
<ul>
	<li>jQueryUI autocomplete plugin - <a href="http://jqueryui.com/demos/autocomplete/">http://jqueryui.com/demos/autocomplete/</a></li>
	<li>jQuery jTemplates plugin - <a href="http://jtemplates.tpython.com/">http://jtemplates.tpython.com/</a></li>
</ul>
<strong>Solution: </strong>First step is we are going to set up our javascript to handle multiple autocomplete plugins on one page and being able to tell the difference between them. All my jQuery code is located in a separate file, so we’ll have a function to initialize the autocomplete plugin on input textboxes by a class name:

[codesyntax lang="javascript" lines="normal"]
<pre>
<pre>
<pre>//Autocomplete templating using jQuery extensions
$.fn.extend({
    templateOverride: function (input) {
        return this.each(function () {
            $(this).data("autocomplete")._renderItem = function (ul, item) {
           //looks for the name of the template that was set on the input element.
           var tmp = $("&lt;div&gt;").setTemplate($("#" + $(input).data('template')).html());
           tmp.processTemplate(item);
           $("&lt;li&gt;&lt;/li&gt;").data("item.autocomplete", item)
                                          .append($(tmp).html())
                                          .appendTo(ul);
           };
        });

} });</pre>
</pre>
<pre>$(function () {
    //Autocomplete
    $('.acJson').each(function () {
        autocompleteJson(this);
    });</pre>
<pre>});</pre>
<pre>//Init autocomplete on the passed input text object.
//Uses html5 data-* attributes:
//data-display should be the name of the json object to show in the input
//data-url is the url to retrieve the json from.
//data-id is the Id of the json object that will be assigned to:
//data-hidden is the name of another element to store the information in data-id
function autocompleteJson(input) {
    $(input).autocomplete({
        source: $(input).data('url'),
        focus: function (event, ui) {
            $(this).val(ui.item[$(this).data('display')]);
            return false;
        },
        select: function (event, ui) {
            $(this).val(ui.item[$(this).data('display')]);
            $('#' + $(this).data('hidden')).val(ui.item[$(this).data('id')]);
            return false;
        }
    }).templateOverride(input);
}</pre>
</pre>
[/codesyntax]

First you may notice that I created a jQuery extension method. You may notice some unorthodox code being used to set the template on the drop-down item. This is the 'hackish' way of overriding the templates for autocomplete.

The autocompleteJson function is meant to setup autocomplete to accept JSON and using jTemplates to display it. There are a few things to explain here. First one is that I'm using <em>HTML5's data-*</em> attribute to store information (all of which are explained in the comments). The purpose of storing data here is for a clean seperation of javascript/jquery and your markup. In my project, no jQuery code can be loaded before the end of the page (using the <a href="http://html5boilerplate.com/" target="_blank">HTML5 boilerplate</a> setup).

The focus property needs to know the name of the JSON object to display in the textbox when one of the drop-down item is focused.

The select property sets both the display object and chooses a JSON object to be inserted into an element of your choice. A reason for this is using it as a key/value pair. Value in the autocomplete and the key/id in a hidden field.

We'll now set up our html, which is naturally in a separate file:
[codesyntax lang="javascript" lines="normal"]
<pre>&lt;input class="acJson" type="text" name="Country" id="Country" data-url="/Geo/CountryContains" data-template="CountryTmp" data-id="CountryId" data-hidden="CountryId" data-display="CountryName" autocomplete="off"&gt;
&lt;input type="hidden" id="CountryId" /&gt;
&lt;script type="text/html" id="CountryTmp"&gt;
        &lt;a&gt;
        {$T.CountryName}
        &lt;/a&gt;
&lt;/script&gt;</pre>
[/codesyntax]

We have 3 elements. The first is the main autocomplete field. This contains multiple <em>html5 data-*</em> attributes which were documented in the javascript code above.
The second is the element you would like to hold a second data, such as a key/ID. The ID of this element needs to be the same as the <em>data-hidden</em> attribute on the main element.
The third is the actual template.
In the next post, I'll wrap all of this in an ASP.NET MVC html helper.
