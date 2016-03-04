---
layout: post
title: 5 Tips for Beginners to Write Better Ruby Code Instantly
---
<p>
  Now that you've learned the basic syntax and gained a basic understanding of ruby works, you want to learn how to write better and more concise code. Here are 5 quick tips picked up from a fantastic book called <a href="https://read.amazon.com/kp/embed?asin=B004MMEJ36&preview=newtab&linkCode=kpe&ref_=cm_sw_r_kb_dp_JqsYwb0AAT5XE" target="_blank">Eloquent Ruby</a> that you can start using right now to make your code more readable.
</p>

<h2>Don't write comments for the wrong reasons</h2>

<p>
  Adding comments, especially in a more complex app can be useful. You have to make sure that you provide comments so that other people can read and understand your code. You also have to make sure that you can understand the code yourself after a while.
</p>

<p>
  On the other hand, you don't want to get in the mindset of, <i>"Oh, this code is pretty messy but I'm going to add comments so it'll be fine."</i> Adding comments to make badly written code somewhat comprehensible is an extremely bad reason for adding comments. <strong>In general, your ruby code should be clear enough so that it explains itself.</strong> Hence the quote from the book, <strong><i>"Good code is like a good joke: it needs no explanation."</i></strong> <span style="font-size:12px;">(pg. 8 - Eloquent Ruby)</span>
</p>

<h2>Ternary Operators</h2>
<p>
  Ever write code like this?
</p>

{% highlight ruby %}
if current_user.subscribed?
  render_already_subscribed_message
else
  current_user.subscribe!
end
{% endhighlight %}

<p>
  This makes perfect sense, except we can make it much shorter with ternary operators. Ternary operators work like such:
</p>

{% highlight ruby %}
if_this_is_a_true_value ? then_the_result_is_this : else_it_is_this
{% endhighlight %}

<p>
   So in this example, we can re-write the code like this:
</p>

{% highlight ruby %}
current_user.subscribed? ? render_already_subscribed_message : current_user.subscribe!
{% endhighlight %}

<h2>Method Name Conventions: Using <code>?</code> and <code>!</code></h2>
<p>
  When you are using ruby, you may have noticed that a lot of the built in methods use <code>?</code> and <code>!</code>. For example <code>.nil?</code> or <code>.flatten!</code>. <code>?</code> and <code>!</code> aren't special symbols in method names, but rather just naming conventions in the ruby community.
</p>

<p>
  <code>?</code>s are used when the method returns a <code>true</code> or <code>false</code> boolean. So for example instead of this:
</p>

{% highlight ruby %}
def is_legal
  self.age >= 21
end

current_user.is_legal
{% endhighlight %}

<p>
  You want to write add a <code>?</code> to the method like this:
</p>

{% highlight ruby %}
def is_legal?
  self.age >= 21
end

current_user.is_legal?
{% endhighlight %}

<p>
  You can see here that <code>current_user.is_legal?</code> makes a little more sense than <code>current_user.is_legal</code>, since we are essentially asking a yes or no question.
</p>

<p>
  Now let's take a look at the <code>!</code> operator in methods. The <code>!</code> operator when used in a method infers that the method is potentially dangerous and could change the state of the variable that the method is called upon. Here's an example:
</p>

{% highlight ruby %}
a = [1, 2, 3, 4]

print a.reverse

print a
# returns [1, 2, 3, 4]
{% endhighlight %}


<p>
  What we see here is that we expect <code>.reverse</code> to actually modify the array <code>a</code>. However, we see that it doesn't modify the actual array, but it just returns the reversed array. Now what if we change the <code>reverse</code> method to <code>reverse!</code>?
</p>

{% highlight ruby %}
a = [1, 2, 3, 4]

print a.reverse!

print a
# returns [4, 3, 2, 1]
{% endhighlight %}

<p>
  Now we see that <code>reverse!</code> has actually modified the actual array. It's important to know the differences between methods with and without the bang operator.
</p>

<h2>Using <code>unless</code> and <code>until</code></h2>
<p>
  When you want to write logic when something is <i>not</i> true, you might be tempted to write code like this:
</p>

{% highlight ruby %}
if !current_user.is_legal?
  puts "I can't buy alcohol"
end
{% endhighlight %}

<p>or alternatively</p>

{% highlight ruby %}
if not current_user.is_legal?
  puts "I can't buy alcohol"
end
{% endhighlight %}

<p>
  Instead of combining <code>if</code> and <code>!</code>, it is often times more intuitive to use <code>unless</code>.
</p>

{% highlight ruby %}
unless current_user.is_legal?
  puts "I can't buy alcohol"
end
{% endhighlight %}

<p>
  This sounds closer to English and is far more readable. Instead of "If current user is not legal, do this", it takes a little less mental energy if I were to say "Unless current user is legal, do this".
</p>

<p>
  Same thing with <code>while</code> loops. Instead of writing this kind of code:
</p>

{% highlight ruby %}
while !fruit.is_ripe?
  puts "This fruit isn't ripe yet."
end
{% endhighlight %}

<p>
  You could make the code much easier to mentally process by rewriting it like this:
</p>

{% highlight ruby %}
until fruit.is_ripe?
  puts "This fruit isn't ripe yet."
end
{% endhighlight %}

<p>
  It takes less mental energy to read and understand code if we use <code>unless</code> and <code>until</code> in these kinds of situations.
</p>

<h2>One Line Statements</h2>

<p>Let's say we have code written like this:</p>

{% highlight ruby %}
if current_user.is_legal?
  puts "I can buy alcohol"
end
{% endhighlight %}

<p>Instead of using multiple lines, we can rewrite this code in one line like this:</p>

{% highlight ruby %}
puts "I can buy alcohol" if current_user.is_legal?
{% endhighlight %}

<p>
  This applies to things like <code>unless</code> as well:
</p>

{% highlight ruby %}
puts "I can't buy alcohol" unless current_user.is_legal?
{% endhighlight %}

<p>
  When writing these one-liners, make sure that the line isn't too long. The point here is to make the code more readable, so you don't want to defeat the purpose by cramming a bunch of code into one line just because you can.
</p>

<hr>

<p>
  Just implementing these simple techniques will make you a better ruby programmer. Ruby offers many elegant solutions like these and learning them are really worth the time.
</p>

<p>Happy coding :)</p>
