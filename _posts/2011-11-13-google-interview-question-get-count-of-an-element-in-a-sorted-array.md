---
layout: post
title: Google Interview Question - Get Count of an element in a sorted array
date: 2011-11-13 23:27
comments: true
categories: [.Net Framework, Algorithm]
---
So a few weeks ago I did an online technical interview with Google (I was contacted by one of their recruiter). The main question centred around "<strong>Find the occurrence of an element in a sorted array". </strong>Now the interviewer wanted the most optimal solution, which is O(log n). This means there shall be no linear counting of any sort, whether after a binary search or before it.

I created a <em>CountSorted() </em>extension method in my C# utility library that uses just this method. Its alittle more complex because I implemented it with <em>IComparable&lt;T&gt;. </em>I'm sure I will find use for this type of count sometime in the future as the built in one for .NET is O(N). All that is currently needed is for me to implement the extension on <em>IEnumerable&lt;T&gt; </em>instead of <em>IList&lt;T&gt;</em> and accept predicates as parameters.
<h2>Codez</h2>
The main code file could be found <a href="https://github.com/shawnmclean/ShawnMclean-.Net-Utility-Library/blob/master/src/ShawnMclean.Utility/Collections/Count.cs">here</a>.

The most optimal solution I could come up with is using a binary search to find an index of the value we are counting, then find the left edge and right edge by using modified binary searches, then minus them to get the count.

So I basically had 3 methods, the main method:

<script type="text/javascript" src="https://gist.github.com/1362902.js?file=CountSorted.cs"></script>Find left edge method:<script type="text/javascript" src="https://gist.github.com/1362902.js?file=findLeftEdge.cs"></script>

Find right edge method:

<script src="https://gist.github.com/1362902.js?file=findRightEdge.cs"></script>

This code could be done better, so if you can optimize it, please do a pull request on github. A few test cases are <a href="https://github.com/shawnmclean/ShawnMclean-.Net-Utility-Library/blob/master/tests/ShawnMclean.Utility.Tests/Collections/SearchTest.cs">here </a>for this algorithm.
