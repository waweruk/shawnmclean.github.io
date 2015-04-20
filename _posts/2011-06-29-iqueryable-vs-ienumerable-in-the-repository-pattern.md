---
layout: post
title: IQueryable vs. IEnumerable in the repository pattern
date: 2011-06-29 03:15
author: shawnmclean
comments: true
categories: [.Net Framework, Entity-framework]
---
First of all, I'll tell the difference between the two and why I choose to return <strong>IQueryable&lt;T&gt; </strong>over IEnumerable&lt;T&gt; in my repository.

<strong>IQueryable&lt;T&gt;</strong> allows you to operate and build queries that can be sent to a remote provider. For eg. Entity framework and Linq2SQL operates with IQueryable&lt;T&gt; because they will translate this to SQL and send it to the connected database.

<strong>IEnumerable&lt;T&gt; </strong>allows you to execute queries in memory (the same process where the data is located).
<h2>Why would you choose one over the other?</h2>
<strong>IQueryable&lt;T&gt; </strong>if you have epic developers whom you trust that they dont write crappy queries that with bunk your database. Returning this allows your developers free roaming on all your database tables that are related to the returned instance, wheres, take, sort and all the many read operations you can think of. Here, you can utilize the power of <em>lazy loading</em> below your repository. This is suited for tight teams whom you are willing to sit with and help fix performance intensive queries.

BUT, I do not allow this past my service layers, because below that, are the designers...and we developers all know what its like to work with designers. My service layer will possible return a List&lt;T&gt; or an IEnumerable&lt;T&gt;.

(<em>One scenario where I allowed IQueryable&lt;T&gt; from my service layer was because we had to use a jQuery Grid built for asp.net mvc, which required IQueryable&lt;T&gt;)</em>

<strong>IEnumerable&lt;T&gt; </strong>if you have epic developers whom you think will write queries that will bring your database to its knees. Designing a repository that returns this is the same as your database admin writing stored procedures for you. It limits your developers below the repository significantly. All your data operations (sort, where,  etc) are done by the repository layer (which some hardcore patterns people insist is the correct way....screw them, I say, do what works). Whatever you return after your database calls, will be in memory. This means you loose the power of lazy loading entities from the database.
<h2>Time to show some codes</h2>
I'll write the same code twice, using both methods, and explain what happens.

[codesyntax lang="csharp"]
<pre>//Repository
public IQueryable&lt;User&gt; GetUsers()
{
	return context.Users;
}

//Service
public void SomeEpicOperation()
{
	//Building up the query
	var usersWith10OrMoreOrders = repo.GetUsers().Where(u =&gt; u.Orders.Count &gt; 10);

	//Adding on to the previous query.
	var only5Users = usersWith10OrMoreOrders.Take(5);

	//Evaluating the query. (Here is where the database is actually called)
	var theRealUsers = only5Users.ToList();
}</pre>
[/codesyntax]

The above shows lazy loading, along with doing almost anything with your queries.

The code below shows the same thing using IEnumerable&lt;T&gt;:

[codesyntax lang="csharp"]
<pre>//Repository
public IEnumerable&lt;User&gt; GetUsers()
{
        //Since IQueryable is built on IEnumerable, we need no cast.
        //So this returns ALL users and their related orders from the database into memory after the forced evaluation to IEnumerable.
        //This means the database call takes place right here.
	return context.Users.Include("Orders");
}

//Service
public void SomeEpicOperation()
{
	//All the same operations can be made here but these operations are done in memory after being forced to evaluate

	//This is still an in memory query
	var usersWith10OrMoreOrders = repo.GetUsers().Where(u =&gt; u.Orders.Count &gt; 10);
	//Still in memory
	var only5Users = usersWith10OrMoreOrders.Take(5);
	//Operation being executed in memory.
	var theRealUsers = only5Users.ToList();
}</pre>
[/codesyntax]
<h2>Conclusion</h2>
IEnumerable to mimic stored procedures or restrict your database. Lost lazy loading ability on the external provider.

IQueryable to give developers more flexibility. Lazy loading is there.

In terms of performance, both can be terrible if not done properly.
