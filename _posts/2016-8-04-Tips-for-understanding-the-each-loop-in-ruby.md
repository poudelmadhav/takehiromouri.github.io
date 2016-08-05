---
layout: post
title: Tips for understanding the each loop in ruby
---

<p>
  I've had some students ask me about the <code>.each</code> loop in ruby :D Hopefully I can clarify some things.
</p>

<h3>Different Ways to Write .each Loop</h3>

{% highlight ruby %}
numbers = [1, 2, 3, 4, 5]

# one way to loop
numbers.each { |item| puts item }

# another way to loop
numbers.each do |item|
  puts item
end
{% endhighlight %}

<p>
  Above, we see two ways to write the <code>each</code> loop. Which one is better?
</p>

<p>
  The answer is, it depends. If the code is really short, then it makes sense to write it as a one liner. But if it's longer, it's better to write it as a <code>do</code> <code>end</code> block.
</p>

{% highlight ruby %}
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

numbers.each { |number| puts number }
# this is better as one liner

numbers.each do |number|
  puts number
  puts "Hello World"
  puts "Another thing"
  puts "Do another thing"
end
# this is better with each do, since doing this in 
# one line would be very messy :)
{% endhighlight %}

<p>
  In ruby, there are often multiple ways of writing code. Ruby developers tend to like shorter and more concise code :) For example, the <code>if</code> statement can even be written as a one-liner:
</p>

{% highlight ruby %}
if some_condition
  puts "TECHRISE students are awesome"
end

# the above is OK, but we can rewrite it like this

puts "TECHRISE students are awesome" if some_condition

# this is considered to be cleaner and many ruby programmers
# prefer this style
{% endhighlight %}

<p>
  Pretty cool right? :)
</p>


<h3>What is this || thing?</h3>

<p>
  When you are using <code>each</code> loops, you'll encounter these <code>||</code> things. You're probably wondering what these are :)
</p>

<p>
  Let's take a look at the following:
</p>

{% highlight ruby %}
numbers = [1, 2, 3, 4, 5]
numbers.each do |number|
  puts number
end

# this will print out 
# 1
# 2
# 3
# 4
# 5
{% endhighlight %}

<p>
  Notice how we have <code>|number|</code>. These placeholders only work within the loop and not outside:
</p>

{% highlight ruby %}
numbers = [1, 2, 3, 4, 5]
numbers.each do |number|
  puts number
end

puts number

# returns an error => "NameError: undefined local variable or method `number' for main:Object"
{% endhighlight %}

<p>
  You can name these placeholders whatever you want:
</p>

{% highlight ruby %}
numbers = [1, 2, 3, 4, 5]
numbers.each do |momo|
  puts momo
end

# still prints out all of the numbers

numbers = [1, 2, 3, 4, 5]
numbers.each do |dog|
  puts dog
end

# still prints out all of the numbers
{% endhighlight %}

<p>
  However, we should name the placeholder something that makes sense.
</p>

<ul>
  <li>Be careful of pluralization</li>
  <ul>
    <li>If it's singular, name it a singular placeholder (<code>number</code>)</li>
    <li>If it's pluralization, name it a plural placeholder (<code>numbers</code>)</li>
  </ul>
  <li>The name should represent what the item is</li>
</ul>

<small>Originally posted in TECHRISE Community at www.techrise.me</small>
