---
layout: post
title: Strings vs Symbols in Ruby
---

<p>
  Symbols can be really confusing when you first use Ruby - what are the differences between Strings and Symbols?
</p>

<h3>Symbols are Faster</h3>

<p>
  Why are symbols faster?
</p>

<p>
  Consider the following:
</p>

<pre><code class="ruby">
string_1 = "Hello"
string_2 = "Hello"
</code></pre>

<p>
  Even if it has the same value, we are creating a new string. In other words, in the above example, we have manufactured two strings with the value of <code>"Hello"</code>.
</p>

<pre><code class="ruby">
string_1 = "Hello"
string_2 = "Hello"

string_1.object_id
# => 70209310447280
string_2.object_id
# => 70209310428440
</code></pre>

<p>
  However, with symbols this isn't the case.
</p>

<pre><code class="ruby">
string_1 = :hello
string_2 = :hello

string_1.object_id
# => 1149468
string_2.object_id
# => 1149468
</code></pre>

<p>
  In this case, both <code>:hello</code>s are exactly the same, they are not created twice.
</p>

<p>
  Since there can only be one instance of any symbol, comparison is much faster with symbols compared to strings. With strings, there can be multiple instances of strings with the same value, making the comparison relatively slower.
</p>

<p>
  Moreover, comparison of symbols are a O(1) comparison, so they are super efficient.
</p>

<h3>When to use symbols</h3>

<p> 
  Symbols are <strong>immutable</strong>. This means that once you create a symbol, you can't change it.
</p>

<p>
  With strings, you have handy methods like <code>upcase!</code> or <code>reverse!</code>, but symbols do not have that capability.
</p>

<p>
  <strong>This is why symbols are great for using when you are representing something that shouldn't change.</strong>
</p>




















