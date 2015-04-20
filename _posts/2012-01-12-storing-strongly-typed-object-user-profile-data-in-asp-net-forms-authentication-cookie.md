---
layout: post
title: Storing object as profile data in ASP.NET forms authentication cookie
date: 2012-01-12 20:16

comments: true
categories: [asp.net-mvc, Uncategorized]
---
In ASP.NET, you are normally given just a string to store additional user data using the <em>FormsAuthenticationTicket</em>, this ticket also takes a UserId, but most times we need to store some form of structured data in cookies, such as an object.

The solution to this is JSON, serialize the object to a JSON string, save it as the UserData, then de-serialize it back to the object when needed.
<h2>Setting up the object and IIdentity</h2>
To make retrieving the data easily, I would create a custom identity that has the properties of the object you are storing. For eg.

<script type="text/javascript" src="https://gist.github.com/1602704.js?file=CustomIdentity.cs"></script>

You should also have a cast, that will convert the User Identity to your custom Identity. So I made an Extension to do this:

<script type="text/javascript" src="https://gist.github.com/1602750.js?file=IdentityExtensions.cs"></script>

<h2>Setting the Data</h2>
You should have a FormsAuthenticationService, and it would look like this:

<script type="text/javascript" src="https://gist.github.com/1602683.js?file=FormsAuthenticationService.cs"></script>  

<h2>Using the Data</h2>

Now its simple to retrieve this data from the cookie:

<script type="text/javascript" src="https://gist.github.com/1602763.js?file=layout.cshtml"></script>

You can use this inside your controller or your views, such as the above was used in the layout to welcome the user by their FirstName and LastName.
