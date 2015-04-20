---
layout: post
title: Entity framework delete/update without round trip to the database
date: 2011-04-16 03:33

comments: true
categories: [.Net Framework, Entity-framework]
---
One of Entity Framework's main advantage is the ability to load and track it's graph. By default, code-first loads and tracks entities, whether they are in a single-tier or n-tier app. This comes at a cost of performance. Tracking entities can be just a little intensive on memory if we are loading thousands of records from the database. If we do not need the benefit of tracking, we may work with detached entity objects.
<h2>Updating</h2>
We'll look at updating by ID of a detached entity.

[codesyntax lang="csharp"]
<pre>        public User UpdateUser(User user, IEnumerable&lt;Expression&lt;Func&lt;User, object&gt;&gt;&gt; properties)
        {
            if (string.IsNullOrEmpty(user.UserId))
            {
                throw new InvalidOperationException("user does not exist");
            }
            else
            {
                db.Users.Attach(user);
                foreach (var selector in properties)
                {
                    string propertyName = Helpers.PropertyToString(selector.Body);
                    db.Entry(user).Property(propertyName).IsModified = true;
                }
                db.SaveChanges();
            }
            return user;
        }</pre>
[/codesyntax]

This is my normal update structure for an entity located in my repository. We pass the user entity (which is detached, can be generated from scratch), we check to make sure we have the ID. We then attach it to the graph. <em>properties </em>is a list of expressions parameters that we can pass to the method to tell EF what property to update. <em>Note: This method of using expressions is known to be slightly slow because of parsing the expression tree.</em> This way, we can update just the properties we want to change. We loop through the expression, convert the body of a selector in the expression to the property name, then set it to being modified. This is 1 database call.

Calling this would look like:

[codesyntax lang="csharp"]
<pre>userRepo.UpdateUser(user, new Expression&lt;Func&lt;User, object&gt;&gt;[]
                                                {
                                                    m =&gt; m.Name
                                                });</pre>
[/codesyntax]

This is just updating the <em>Name</em> property for the user.
<h2>Deleting</h2>
This is the best one, deleting a user without a round trip. Simple and straight forward, create the object with the id, attach it, delete it.

[codesyntax lang="csharp"]
<pre>var user = new User() { UserId = userId };
db.Users.Attach(user);
db.Users.Remove(user);
db.SaveChanges();</pre>
[/codesyntax]

Note: Attach is different from adding a new record to the database. We are attaching it to the EF graph.
<h2>Reading</h2>
If we are going the memory efficient way, we will have to load our entities without tracking.

[codesyntax lang="csharp"]
<pre>IQueryable&lt;User&gt; query = db.Users.AsNoTracking();</pre>
[/codesyntax]

I return <em>IQueryable&lt;T&gt; </em>from my repository, so this is what I basically return. Although my Get method has some additional features, such as accepting an expression that will contain navigational properties to load up.

So in concluding, this is a trade-off between performance and code design. This is exactly why the framework team decided to include these methods, some system design requires us to take things into our own hands and work closer to metal (not too close [SQL]).

If anyone sees anything wrong in my post, please comment and tell. I'm still learning.
