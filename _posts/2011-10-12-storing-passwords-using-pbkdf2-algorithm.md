---
layout: post
title: Storing Passwords using PBKDF2 Algorithm
date: 2011-10-12 00:29

comments: true
categories: [.net, .Net Framework, c#, encryption, hashing]
---
I recently stopped using SHA, etc, etc for hashing passwords due to my misguided attempt of having higher security. The problem with these hashing algorithms are that they are <em>fast, </em>even when coupled with a salt, brute force algorithms could be written to crack these combinations faster. Coupled with parallel brute force on CUDA or ATI graphics cards, my passwords wont last much long with cracking algorithms running on top of these systems.

What's the solution? Slower hashing. Algorithms like BCrypt, SCrypt and PBKDF2 . These algorithms are scalable with more hardware, which means that crackers will need to add more hardware to get better performance. No matter how fast the hardware is, it still executes within that same time-span depending on how you tweaked your algorithm. Combine that with a salt and I'd say by the time they cracked even 1 password in your database, you'd have probably figured out that someone has stolen it and have ample time to shut down your system and alert your users.

A stable password storing scheme would be:
<ol>
	<li>Generate Salt.</li>
	<li>Combine salt and password to generate hash.</li>
	<li>Save hash and salt in the database. (it doesn't matter if the salt is in plain sight)</li>
</ol>
For every time you're saving a password, generate a completely new salt.

<strong>Update:</strong> Wrapper updated and moved to <a href="http://www.shawnmclean.com/blog/2012/04/simplecrypto-net-a-pbkdf2-hashing-wrapper-for-net-framework/">http://www.shawnmclean.com/blog/2012/04/simplecrypto-net-a-pbkdf2-hashing-wrapper-for-net-framework/</a>

I created a wrapper around the PBKDF2 algorithm for use in my ASP.NET MVC applications. You can analyse it, tell me what's wrong, how to optimize or use it if you want. There are a few unit tests there to verify the hashing algorithm and salt generation. You can view the code on <a href="https://github.com/Mixmasterxp/ShawnMclean-.Net-Utility-Library/blob/master/src/ShawnMclean.Utility/Encryption/Hashing.cs#L32">GitHub</a>.

This is how simple it is to use:

[codesyntax lang="csharp"]
<pre>string salt = Hashing.GenerateSalt(HASH_ITERATIONS, SALT_SIZE);
string hashedPw = Hashing.ComputeHash(textToHash, salt, HashAlgorithmType.PBKDF2);</pre>
[/codesyntax]

The GenerateSalt method generates a salt with the intention of use with PBKFD2 algorithm, hence the acceptance of HASH_ITERATIONS. This can be any integer if you are not using PBKFD2, if you are, make sure it is over 1, recommended is like 50+. SALT_SIZE recommended is 16+, which is in bytes. This method returns a string in the format of <em>'int.string' | '</em><em>{iteration}.{salt}</em>'.

This should help people secure their passwords alittle more...unlike those morons who developed the Play Station Network system and stored our passwords in plain text. I hope that you are at least hashing your passwords with a salt.
