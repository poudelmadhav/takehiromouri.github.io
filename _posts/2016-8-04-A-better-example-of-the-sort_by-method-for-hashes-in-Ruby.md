---
layout: post
title: A better example of the sort_by method for hashes in Ruby
---

<p>
  Codecademy is great for learning the fundamentals, but at the same time sometimes they use really confusing examples.
</p>

<p>
  For example, in one of the lessons, you have to write code that looks like this:
</p>

{% highlight ruby %}
puts "Enter Text:"
text = gets.chomp

words = text.split(" ")

frequencies = Hash.new(0)

words.each do |x|
    frequencies[x] += 1
end

frequencies =  frequencies.sort_by { |k, v| v}
frequencies.reverse!
{% endhighlight %}

<p>
  What is this <code>frequencies.sort_by { |k, v| v}</code> code?!
</p>

<p>
  It's actually really easy to understand if you look at another example of how the <code>sort_by</code> method is used in hashes.
</p>

<h3>First, what are hashes?</h3>

<p>
  Hashes consist of keys and values. For example, we could have a <code>people</code> hash that contains information about a person's name and their age.
</p>

{% highlight ruby %}
people = {
  :fred => 23,
  :joan => 18,
  :pete => 54
}
{% endhighlight %}

<p>
  Inside the <code>people</code> hash, we have 3 sets of keys and values. <code>:fred</code>, <code>:joan</code>, <code>:pete</code> are all keys, and <code>23</code>, <code>18</code>, and <code>54</code> are all values.
</p>

<p>
  Hashes are really nice because you can get the value of a key really quickly. For example, if we wanted to find out the age of <code>:fred</code>, all we have to do is this:
</p>

{% highlight ruby %}
people[:fred]
# => 23
{% endhighlight %}

<p>
  It's hard to do the same thing with arrays, so hashes are very useful.
</p>  

<p>
  However, what if we want to sort the hash? What if we want to sort the hash by the name or the age?
</p>

<p>
  This is where the <code>.sort_by</code> method is handy :) Take a look at the code below:
</p>

{% highlight ruby %}
people = {
  :fred => 23,
  :joan => 18,
  :pete => 54
}

people.sort_by { |name, age| age }
  # => [[:joan, 18], [:fred, 23], [:pete, 54]]

people.sort_by { |name, age| name }
  # => [[:fred, 23], [:joan, 18], [:pete, 54]]
{% endhighlight %}

<p>
  <code>people.sort_by { |name, age| age }</code> -> here we are sorting the <code>people</code> hash by each person's <code>age</code>.
</p>

<p>
  <code>people.sort_by { |name, age| name }</code> -> here we are sorting the <code>people</code> hash by each person's <code>name</code>.
</p>

<p> 
  By looking at this example, hopefully all of the sudden the mystery of <code>frequencies.sort_by { |k, v| v}</code> is solved :)
</p>  

<small>Originally posted in TECHRISE Community at www.techrise.me</small>
