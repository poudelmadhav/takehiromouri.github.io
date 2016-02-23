---
layout: post
title: Useful techniques for arrays and strings in ruby
---

<p>
  Ruby comes with several useful techniques that you can use to manipulate various data types. In this post, I'll go over some particularly useful techniques that can be used with arrays and strings in Ruby.
</p>

<h3>Using <code>%w{ }</code> to declare an array of strings</h3>

<p>
  You can declare an array of strings like this:
</p>

{% highlight ruby %}
array = ['I', 'am', 'a', 'little', 'pony']
# ["I", "am", "a", "little", "pony"]
{% endhighlight %}

<p>
  There's another way you can declare strings that don't contain strings, which you may find a to be pretty convenient:
</p>

{% highlight ruby %}
array = %w{I am a little pony}
# ["I", "am", "a", "little", "pony"]
{% endhighlight %}

<p>
  It takes less time to type but results in the same thing.
</p>

<h3>Using <code>find_index</code></h3>

<p>
  To find the index of an object in array, the <code>find_index</code> method comes in handy:
</p>

{% highlight ruby %}
words = %w{I am a little pony}
words.find_index{|word| word == "pony"}
# returns 4
{% endhighlight %}

<h3>The Inject Method</h3>

<p>
  When you want to get the sum of all numbers in an array, the <code>inject</code> method allows you to do so in an elegant way:
</p>

{% highlight ruby %}
numbers = [1, 2, 3, 4, 5]
numbers.inject{|sum, num| sum += num}
# returns 15
{% endhighlight %}

<p>
  <code>inject</code> is like an <code>each</code> loop, in that it iterates through each element of the array and performs something with each element. <code>inject</code> takes in 2 arguments. The first argument (<code>sum</code>) is the result of the expression performed in the block. In this case, it is the result of <code>sum += num</code>. The second argument (<code>num</code>) is the element in the array in which the iteration is currently on. For example, the first time around, <code>num</code> will equal <code>1</code>, the second time around, <code>num</code> will equal <code>2</code>.
</p>

<p>
  <code>sum</code> by default is initialized at <code>0</code>, but you can specify another value like such:
</p>

{% highlight ruby %}
numbers = [1, 2, 3, 4, 5]
numbers.inject(100){|sum, num| sum += num}
# returns 115
{% endhighlight %}

<p>
  in which case it returns <code>115</code>.
</p>


<h3>No more escapes with <code>%q{ }</code></h3>

<p>
  Isn't it annoying when you have to do something like this?
</p>

{% highlight ruby %}
string_full_of_escapes = 'I\'m in love with Ruby. "Ruby isn\'t just a language, it\'s a lifestyle". '
{% endhighlight %}

<p>
  Instead, you could simple do this:
</p>

{% highlight ruby %}
no_escape_string = %q{I'm in love with Ruby. "Ruby isn't just a language, it's a lifestyle."}
{% endhighlight %}

<p>
  The <code>q</code> stands for quotes, and it invokes single quotes for you. Beware that <code>#{}</code> won't work with single quotes, and only works with double quotes. In that case, you want to use <code>Q%{ }</code> instead:
</p>


{% highlight ruby %}
name = "Takehiro"
%Q{My name is #{name}}
# "My name is Takehiro"
{% endhighlight %}

<hr>

<p>
  Happy coding!
</p>




